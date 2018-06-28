---
title: Cifrado de discos en una VM de Linux en Azure | Microsoft Docs
description: Cifrado de discos virtuales en una VM de Linux para mejorar la seguridad con la CLI de Azure 2.0
services: virtual-machines-linux
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 2a23b6fa-6941-4998-9804-8efe93b647b3
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/31/2018
ms.author: iainfou
ms.openlocfilehash: 343408366c2970d10a952634ac671721caed74d4
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/25/2018
ms.locfileid: "36936877"
---
# <a name="how-to-encrypt-a-linux-virtual-machine-in-azure"></a>Cifrado de una máquina virtual Linux en Azure
Para mejorar la seguridad y el cumplimiento de la máquina virtual (VM), se pueden cifrar los discos virtuales y la propia máquina vrtual. Las máquinas virtuales se cifran mediante claves criptográficas que están protegidas en Azure Key Vault. Estas claves criptográficas se pueden controlar y se puede auditar su uso. En este artículo se detalla cómo cifrar los discos virtuales en una VM de Linux con la CLI de Azure 2.0. 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Si decide instalar y usar la CLI localmente, para este artículo es preciso que ejecute la versión 2.0.30 o posterior de la CLI de Azure. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure 2.0]( /cli/azure/install-azure-cli).

## <a name="overview-of-disk-encryption"></a>Introducción al cifrado de discos
Los discos virtuales en máquinas virtuales Linux se cifran en reposo mediante [dm-crypt](https://wikipedia.org/wiki/Dm-crypt). El cifrado de los discos virtuales en Azure no conlleva ningún cargo. Las claves criptográficas se almacenan en Azure Key Vault con protección de software, o puede importar o generar las claves en módulos de seguridad de hardware (HSM) certificados conforme a las normas FIPS 140-2 de nivel 2. Estas claves criptográficas se pueden controlar y se puede auditar su uso. Las claves criptográficas se usan para cifrar y descifrar los discos virtuales conectados a la máquina virtual. Como las máquinas virtuales se encienden y se apagan, una entidad de servicio de Azure Active Directory proporciona un mecanismos seguro para la emisión de estas claves criptográficas.

El proceso para cifrar una máquina virtual es el siguiente:

1. Cree una clave criptográfica en Azure Key Vault.
2. Configure la clave criptográfica para poder utilizarla para el cifrado de discos.
3. Para leer la clave criptográfica de Azure Key Vault, cree una entidad de servicio de Azure Active Directory con los permisos adecuados.
4. Emita el comando para cifrar los discos virtuales y especifique la entidad de servicio de Azure Active Directory y la clave criptográfica adecuada que se deberá usar.
5. La entidad de servicio de Azure Active Directory solicita la clave criptográfica necesaria a Azure Key Vault.
6. Los discos virtuales se cifran con la clave criptográfica proporcionada.

## <a name="encryption-process"></a>Proceso de cifrado
El cifrado de discos utiliza los siguientes componentes adicionales:

* **Azure Key Vault**: se usa para proteger las claves criptográficas y los secretos usados para el proceso de cifrado y descifrado de discos.
  * Si ya existe un almacén de Azure Key Vault, puede utilizarlo. No es necesario dedicar un almacén de Key Vault para el cifrado de discos.
  * Para separar los límites administrativos y la visibilidad de las claves, puede crear un almacén de Key Vault dedicado.
* **Azure Active Directory**: controla el intercambio seguro de las claves cifradas necesarias y la autenticación para las acciones solicitadas.
  * Normalmente, puede usar un almacén existente de Azure Active Directory para hospedar la aplicación.
  * La entidad de servicio proporciona un mecanismo seguro para solicitar y recibir las claves criptográficas correspondientes. No está desarrollando una aplicación real que se integrará con Azure Active Directory.

## <a name="requirements-and-limitations"></a>Requisitos y limitaciones
Requisitos y escenarios admitidos para el cifrado de discos:

* Las siguientes SKU de servidor Linux: Ubuntu, CentOS, SUSE y SUSE Linux Enterprise Server (SLES), y Red Hat Enterprise Linux.
* Todos los recursos (por ejemplo: Key Vault, cuenta de Storage, máquina virtual, etc.) deben pertenecer a la misma región y suscripción de Azure.
* Máquinas virtuales estándar de las series A, D, DS, G, GS, etc.
* Actualización de las claves criptográficas en una máquina virtual Linux ya cifrada.

El cifrado del disco no se admite actualmente en los siguientes escenarios:

* Máquina s virtuales de nivel básico
* Máquinas virtuales creadas con el modelo de implementación clásica.
* Deshabilitado del cifrado de disco de sistema operativo en máquinas virtuales Linux.
* Uso de imágenes personalizadas de Linux.

Para más información sobre los escenarios admitidos y las limitaciones, consulte [Azure Disk Encryption para máquinas virtuales IaaS](../../security/azure-security-disk-encryption.md).


## <a name="create-an-azure-key-vault-and-keys"></a>Creación de Azure Key Vault y claves
En los ejemplos siguientes, reemplace los nombres de parámetros de ejemplo por los suyos propios. Los nombres de parámetro de ejemplo incluyen *myResourceGroup*, *myKey* y *myVM*.

El primer paso es crear un almacén de Azure Key Vault para almacenar las claves criptográficas. Azure Key Vault puede almacenar claves, secretos o contraseñas que permiten su implementación segura en las aplicaciones y los servicios. Para el cifrado de discos virtuales, se usa Key Vault para almacenar una clave criptográfica que se utiliza para cifrar o descifrar los discos virtuales.

Habilite el proveedor de Azure Key Vault en su suscripción de Azure con [az provider register](/cli/azure/provider#az-provider-register) y cree un grupo de recursos con [az group create](/cli/azure/group#az-group-create). En el ejemplo siguiente, se crea un grupo de recursos denominado *myResourceGroup* en la ubicación `eastus`:

```azurecli-interactive
az provider register -n Microsoft.KeyVault
az group create --name myResourceGroup --location eastus
```

El almacén de Azure Key Vault que contiene las claves criptográficas y los recursos de proceso asociados, como el almacenamiento y la propia máquina virtual, debe residir en la misma región. Cree una instancia de Azure Key Vault con [az keyvault create](/cli/azure/keyvault#az-keyvault-create) y habilite Key Vault para usarlo con el cifrado de discos. Especifique un nombre de Key Vault único para *keyvault_name* de la siguiente manera:

```azurecli-interactive
keyvault_name=myuniquekeyvaultname
az keyvault create \
    --name $keyvault_name \
    --resource-group myResourceGroup \
    --location eastus \
    --enabled-for-disk-encryption True
```

Puede almacenar las claves criptográficas mediante software o protección del modelo de seguridad de hardware (HSM). Para usar HSM se necesita un almacén premium de Key Vault. La creación de un almacén premium de Key Vault conlleva un coste, frente al almacén estándar de Key Vault, que almacena las claves protegidas por software. Para crear un almacén de Key Vault premium, en el paso anterior, agregue `--sku Premium` al comando. En el ejemplo siguiente se usan claves protegidas mediante software ya que se ha creado una instancia de Key Vault estándar.

En ambos modelos de protección, la plataforma Windows Azure debe tener acceso para solicitar las claves criptográficas cuando la máquina virtual arranca para descifrar los discos virtuales. Cree una clave criptográfica en la instancia de Key Vault con [az keyvault key create](/cli/azure/keyvault/key#az-keyvault-key-create). En el ejemplo siguiente se crea una clave llamada *myKey*:

```azurecli-interactive
az keyvault key create --vault-name $keyvault_name --name myKey --protection software
```


## <a name="create-an-azure-active-directory-service-principal"></a>Creación de una entidad de servicio de Azure Active Directory
Cuando los discos virtuales se cifran o descifran, se especifica una cuenta para controlar la autenticación y el intercambio de claves criptográficas desde Key Vault. Esta cuenta, una entidad de servicio de Azure Active Directory, permite que la plataforma de Azure solicite las claves criptográficas correspondientes en nombre de la máquina virtual. Su suscripción dispone de una instancia de Azure Active Directory predeterminada, aunque muchas organizaciones tienen directorios de Azure Active Directory dedicados.

Cree una entidad de servicio mediante Azure Active Directory con [az ad sp create-for-rbac](/cli/azure/ad/sp#az-ad-sp-create-for-rbac). En el ejemplo siguiente se leen los valores de la contraseña y la entidad de servicio para usarlos en comandos posteriores:

```azurecli-interactive
read sp_id sp_password <<< $(az ad sp create-for-rbac --query [appId,password] -o tsv)
```

La contraseña solo se muestra cuando crea la entidad de servicio. Si lo desea, vea y anote la contraseña (`echo $sp_password`). Puede mostrar las entidades de servicio con el comando [az ad sp list](/cli/azure/ad/sp#az-ad-sp-list) y consultar información adicional sobre una entidad de servicio específica con [az ad sp show](/cli/azure/ad/sp#az-ad-sp-show).

Para cifrar o descifrar los discos virtuales correctamente, debe establecer los permisos de la clave criptográfica que se almacena en Key Vault para que permitan que la entidad de servicio de Azure Active Directory lea las claves. Establezca permisos en Key Vault con [az keyvault set-policy](/cli/azure/keyvault#az-keyvault-set-policy). En el ejemplo siguiente, el id. de la entidad de servicio se proporciona a partir del comando anterior:

```azurecli-interactive
az keyvault set-policy --name $keyvault_name --spn $sp_id \
  --key-permissions wrapKey \
  --secret-permissions set
```


## <a name="create-a-virtual-machine"></a>de una máquina virtual
Cree una máquina virtual con [az vm create](/cli/azure/vm#az-vm-create) y conecte un disco de datos de 5 GB. Solo ciertas imágenes de Marketplace admiten el cifrado de discos. En el ejemplo siguiente se crea una máquina virtual llamada *myVM* mediante una imagen de *Ubuntu 16.04 LTS*:

```azurecli-interactive
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --data-disk-sizes-gb 5
```

Use SSH para tener acceso a la máquina virtual con el valor de *publicIpAddress* que se muestra en la salida del comando anterior. Cree una partición y un sistema de archivos y, luego, monte el disco de datos. Para más información, consulte la sección sobre cómo [conectarse a una máquina virtual de Linux para montar el disco nuevo](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#connect-to-the-linux-vm-to-mount-the-new-disk). Cierre la sesión de SSH.


## <a name="encrypt-the-virtual-machine"></a>Cifrado de la máquina virtual
Para cifrar los discos virtuales, reúna todos los componentes anteriores:

1. Especifique la entidad de servicio de Azure Active Directory y la contraseña.
2. Especifique el almacén de Key Vault donde se almacenarán los metadatos de los discos cifrados.
3. Especifique las claves criptográficas que se utilizarán para el cifrado y descifrado.
4. Especifique si desea cifrar el disco del sistema operativo, los discos de datos o todos.

Cifre la máquina virtual con [az vm encryption enable](/cli/azure/vm/encryption#az-vm-encryption-enable). En el ejemplo siguiente se usan las variables *$sp_id* y *$sp_password* del comando [az ad sp create-for-rbac](/cli/azure/ad/sp#az-ad-sp-create-for-rbac) anterior:

```azurecli-interactive
az vm encryption enable \
    --resource-group myResourceGroup \
    --name myVM \
    --aad-client-id $sp_id \
    --aad-client-secret $sp_password \
    --disk-encryption-keyvault $keyvault_name \
    --key-encryption-key myKey \
    --volume-type all
```

El proceso de cifrado de disco demora unos minutos en completarse. Supervise el estado del proceso con [az vm encryption show](/cli/azure/vm/encryption#az-vm-encryption-show):

```azurecli-interactive
az vm encryption show --resource-group myResourceGroup --name myVM
```

La salida es similar a la del siguiente ejemplo truncado:

```json
[
  "dataDisk": "EncryptionInProgress",
  "osDisk": "EncryptionInProgress"
]
```

Espere hasta que el estado del disco del SO indique **VMRestartPending** y, luego, reinicie la máquina virtual con [az vm restart](/cli/azure/vm#az-vm-restart):

```azurecli-interactive
az vm restart --resource-group myResourceGroup --name myVM
```

El proceso de cifrado de disco finaliza durante el proceso de arranque, por lo que debe esperar unos minutos antes de comprobar nuevamente el estado de cifrado con [az vm encryption show](/cli/azure/vm/encryption#az-vm-encryption-show):

```azurecli-interactive
az vm encryption show --resource-group myResourceGroup --name myVM
```

El estado ahora debería indicar que tanto el disco del SO como el disco de datos están **cifrados**.


## <a name="add-additional-data-disks"></a>Adición de discos de datos adicionales
Una vez cifrados los discos de datos, más adelante podrá agregar más discos virtuales a la máquina virtual, y también cifrarlos. Por ejemplo, vamos a agregar un segundo disco virtual a la máquina virtual de la siguiente manera:

```azurecli-interactive
az vm disk attach \
    --resource-group myResourceGroup \
    --vm-name myVM \
    --disk myDataDisk \
    --new \
    --size-gb 5
```

Vuelva a ejecutar el comando para cifrar los discos virtuales de la manera siguiente:

```azurecli-interactive
az vm encryption enable \
    --resource-group myResourceGroup \
    --name myVM \
    --aad-client-id $sp_id \
    --aad-client-secret $sp_password \
    --disk-encryption-keyvault $keyvault_name \
    --key-encryption-key myKey \
    --volume-type all
```


## <a name="next-steps"></a>Pasos siguientes
* Para más información acerca de cómo administrar Azure Key Vault, incluido cómo eliminar claves criptográficas y almacenes, consulte [Administración de Key Vault mediante CLI](../../key-vault/key-vault-manage-with-cli2.md).
* Para obtener más información acerca del cifrado de discos, por ejemplo, cómo preparar una máquina virtual personalizada cifrada para cargar en Azure, consulte [Azure Disk Encryption](../../security/azure-security-disk-encryption.md).
