---
title: Cifrado de discos en una VM de Linux en Azure | Microsoft Docs
description: Cifrado de discos virtuales en una VM de Linux para mejorar la seguridad con la CLI de Azure
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 2a23b6fa-6941-4998-9804-8efe93b647b3
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 10/30/2018
ms.author: cynthn
ms.openlocfilehash: 15bd3cf2ab6ea5285662610c2c0a850bb180e2f8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60731624"
---
# <a name="how-to-encrypt-a-linux-virtual-machine-in-azure"></a>Cifrado de una máquina virtual Linux en Azure

Para mejorar la seguridad y el cumplimiento de la máquina virtual (VM), se pueden cifrar los discos virtuales y la propia máquina vrtual. Las máquinas virtuales se cifran mediante claves criptográficas que están protegidas en Azure Key Vault. Estas claves criptográficas se pueden controlar y se puede auditar su uso. En este artículo se detalla cómo cifrar los discos virtuales en una VM Linux con la CLI de Azure. 

## <a name="launch-azure-cloud-shell"></a>Inicio de Azure Cloud Shell

Azure Cloud Shell es un shell interactivo gratuito que puede usar para ejecutar los pasos de este artículo. Tiene las herramientas comunes de Azure preinstaladas y configuradas para usarlas en la cuenta. 

Para abrir Cloud Shell, seleccione **Pruébelo** en la esquina superior derecha de un bloque de código. También puede ir a [https://shell.azure.com/bash](https://shell.azure.com/bash) para iniciar Cloud Shell en una pestaña independiente del explorador. Seleccione **Copiar** para copiar los bloques de código, péguelos en Cloud Shell y, luego, presione Entrar para ejecutarlos.

Si decide instalar y usar la CLI localmente, para este artículo es preciso que ejecute la versión 2.0.30 o posterior de la CLI de Azure. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, vea [Instalación de la CLI de Azure]( /cli/azure/install-azure-cli).

## <a name="overview-of-disk-encryption"></a>Introducción al cifrado de discos
Los discos virtuales en máquinas virtuales Linux se cifran en reposo mediante [dm-crypt](https://wikipedia.org/wiki/Dm-crypt). El cifrado de los discos virtuales en Azure no conlleva ningún cargo. Las claves criptográficas se almacenan en Azure Key Vault con protección de software, o puede importar o generar las claves en módulos de seguridad de hardware (HSM) certificados conforme a las normas FIPS 140-2 de nivel 2. Estas claves criptográficas se pueden controlar y se puede auditar su uso. Las claves criptográficas se usan para cifrar y descifrar los discos virtuales conectados a la máquina virtual. 

El proceso para cifrar una máquina virtual es el siguiente:

1. Cree una clave criptográfica en Azure Key Vault.
1. Configure la clave criptográfica para poder utilizarla para el cifrado de discos.
1. Habilite el cifrado de disco para los discos virtuales.
1. Las claves criptográficas necesarias se solicitan desde Azure Key Vault.
1. Los discos virtuales se cifran con la clave criptográfica proporcionada.


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
resourcegroup="myResourceGroup"
az group create --name $resourcegroup --location eastus
```

El almacén de Azure Key Vault que contiene las claves criptográficas y los recursos de proceso asociados, como el almacenamiento y la propia máquina virtual, debe residir en la misma región. Cree una instancia de Azure Key Vault con [az keyvault create](/cli/azure/keyvault#az-keyvault-create) y habilite Key Vault para usarlo con el cifrado de discos. Especifique un nombre de Key Vault único para *keyvault_name* de la siguiente manera:

```azurecli-interactive
keyvault_name=myvaultname$RANDOM
az keyvault create \
    --name $keyvault_name \
    --resource-group $resourcegroup \
    --location eastus \
    --enabled-for-disk-encryption True
```

Puede almacenar las claves criptográficas mediante software o protección del modelo de seguridad de hardware (HSM). Para usar HSM se necesita un almacén premium de Key Vault. La creación de un almacén premium de Key Vault conlleva un coste, frente al almacén estándar de Key Vault, que almacena las claves protegidas por software. Para crear un almacén de Key Vault premium, en el paso anterior, agregue `--sku Premium` al comando. En el ejemplo siguiente se usan claves protegidas mediante software ya que se ha creado una instancia de Key Vault estándar.

En ambos modelos de protección, la plataforma Windows Azure debe tener acceso para solicitar las claves criptográficas cuando la máquina virtual arranca para descifrar los discos virtuales. Cree una clave criptográfica en la instancia de Key Vault con [az keyvault key create](/cli/azure/keyvault/key#az-keyvault-key-create). En el ejemplo siguiente se crea una clave llamada *myKey*:

```azurecli-interactive
az keyvault key create \
    --vault-name $keyvault_name \
    --name myKey \
    --protection software
```


## <a name="create-a-virtual-machine"></a>de una máquina virtual
Cree una máquina virtual con [az vm create](/cli/azure/vm#az-vm-create) y conecte un disco de datos de 5 GB. Solo ciertas imágenes de Marketplace admiten el cifrado de discos. En el ejemplo siguiente se crea una máquina virtual llamada *myVM* mediante una imagen de *Ubuntu 16.04 LTS*:

```azurecli-interactive
az vm create \
    --resource-group $resourcegroup \
    --name myVM \
    --image Canonical:UbuntuServer:16.04-LTS:latest \
    --admin-username azureuser \
    --generate-ssh-keys \
    --data-disk-sizes-gb 5
```

Use SSH para tener acceso a la máquina virtual con el valor de *publicIpAddress* que se muestra en la salida del comando anterior. Cree una partición y un sistema de archivos y, luego, monte el disco de datos. Para más información, consulte la sección sobre cómo [conectarse a una máquina virtual de Linux para montar el disco nuevo](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#connect-to-the-linux-vm-to-mount-the-new-disk). Cierre la sesión de SSH.


## <a name="encrypt-the-virtual-machine"></a>Cifrado de la máquina virtual


Cifre la máquina virtual con [az vm encryption enable](/cli/azure/vm/encryption#az-vm-encryption-enable):

```azurecli-interactive
az vm encryption enable \
    --resource-group $resourcegroup \
    --name myVM \
    --disk-encryption-keyvault $keyvault_name \
    --key-encryption-key myKey \
    --volume-type all
```

El proceso de cifrado de disco demora unos minutos en completarse. Supervise el estado del proceso con [az vm encryption show](/cli/azure/vm/encryption#az-vm-encryption-show):

```azurecli-interactive
az vm encryption show --resource-group $resourcegroup --name myVM --query 'status'
```

Al finalizar, el resultado será similar al siguiente ejemplo:

```json
[
  {
    "code": "ProvisioningState/succeeded",
    "displayStatus": "Provisioning succeeded",
    "level": "Info",
    "message": "Encryption succeeded for all volumes",
    "time": null
  }
]
```


## <a name="add-additional-data-disks"></a>Adición de discos de datos adicionales
Una vez cifrados los discos de datos, podrá agregar más discos virtuales a la máquina virtual y cifrarlos. 

Después de que el disco de datos se ha agregado a la máquina virtual, vuelva a ejecutar el comando para cifrar los discos virtuales de la manera siguiente:

```azurecli-interactive
az vm encryption enable \
    --resource-group $resourcegroup \
    --name myVM \
    --disk-encryption-keyvault $keyvault_name \
    --key-encryption-key myKey \
    --volume-type data
```


## <a name="next-steps"></a>Pasos siguientes
* Para más información acerca de cómo administrar Azure Key Vault, incluido cómo eliminar claves criptográficas y almacenes, consulte [Administración de Key Vault mediante CLI](../../key-vault/key-vault-manage-with-cli2.md).
* Para obtener más información acerca del cifrado de discos, por ejemplo, cómo preparar una máquina virtual personalizada cifrada para cargar en Azure, consulte [Azure Disk Encryption](../../security/azure-security-disk-encryption.md).
