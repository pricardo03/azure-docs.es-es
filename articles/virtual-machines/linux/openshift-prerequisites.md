---
title: Requisitos previos para OpenShift en Azure | Microsoft Docs
description: Requisitos previos para implementar OpenShift en Azure.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldwongms
manager: mdotson
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/19/2019
ms.author: haroldw
ms.openlocfilehash: d8a9b82e51c837af6343ddf851545d02299aa527
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61473892"
---
# <a name="common-prerequisites-for-deploying-openshift-in-azure"></a>Requisitos previos comunes para la implementación de OpenShift en Azure

En este artículo se describen los requisitos previos comunes para implementar OpenShift Container Platform u OKD en Azure.

La instalación de OpenShift usa cuadernos de estrategias de Ansible. Ansible usa Secure Shell (SSH) para conectarse a todos los hosts del clúster y completar los pasos de instalación.

Cuando ansible realiza la conexión SSH a los hosts remotos, que no puede introducir una contraseña. Por este motivo, la clave privada no puede tener una contraseña (frase de contraseña) asociada o se producirá un error en la implementación.

Dado que todas las máquinas virtuales (VM) se implementan a través de plantillas de Azure Resource Manager, se usa la misma clave pública para acceder a todas ellas. La clave privada correspondiente debe estar en la máquina virtual que ejecuta todos los cuadernos. Para realizar esta acción de forma segura, Azure key vault se usa para pasar la clave privada a la máquina virtual.

Si es necesario el almacenamiento persistente de contenedores, también se necesitan volúmenes persistentes. OpenShift es compatible con discos duros virtuales (VHD de) Azure para los volúmenes persistentes, pero Azure debe configurarse primero como proveedor de nube.

En este modelo, OpenShift:

- Crea un objeto de disco duro virtual en una cuenta de almacenamiento de Azure o un disco administrado.
- Monta el VHD en una máquina virtual y da formato al volumen.
- Monta el volumen en el pod.

Para que esta configuración funcione, OpenShift necesita permisos para realizar estas tareas en Azure. Una entidad de servicio se usa para este propósito. La entidad de servicio es una cuenta de seguridad de Azure Active Directory con permisos para los recursos.

La entidad de servicio debe tener acceso a las cuentas de almacenamiento y a las máquinas virtuales que componen el clúster. Si todos los recursos de clúster de OpenShift se implementan en un único grupo de recursos, pueden concederse permisos a la entidad de servicio para ese grupo de recursos.

En esta guía se describe cómo crear los artefactos asociados con los requisitos previos.

> [!div class="checklist"]
> * Crear un almacén de claves para administrar las claves SSH para el clúster de OpenShift.
> * Cree una entidad de servicio para que la use el proveedor en la nube de Azure.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure 
Inicie sesión en la suscripción de Azure con el comando [az login](/cli/azure/reference-index) y siga las instrucciones de la pantalla, o bien haga clic en **Pruébelo** para usar Cloud Shell.

```azurecli 
az login
```
## <a name="create-a-resource-group"></a>Crear un grupo de recursos

Cree un grupo de recursos con el comando [az group create](/cli/azure/group). Un grupo de recursos de Azure es un contenedor lógico en el que se implementan y se administran los recursos de Azure. Debe usar un grupo de recursos dedicado para hospedar el almacén de claves. Este grupo es independiente del grupo de recursos en el que se implementan los recursos del clúster de OpenShift.

En el ejemplo siguiente se crea un grupo de recursos denominado *keyvaultrg* en la ubicación *eastus*:

```azurecli 
az group create --name keyvaultrg --location eastus
```

## <a name="create-a-key-vault"></a>Creación de un Almacén de claves
Cree un almacén de claves para almacenar las claves SSH para el clúster con el comando [az keyvault create](/cli/azure/keyvault). El nombre del almacén de claves debe ser único globalmente.

En el ejemplo siguiente se crea un almacén de claves denominado *keyvault* en el grupo de recursos *keyvaultrg*:

```azurecli 
az keyvault create --resource-group keyvaultrg --name keyvault \
       --enabled-for-template-deployment true \
       --location eastus
```

## <a name="create-an-ssh-key"></a>Creación de una clave SSH 
Se necesita una clave SSH para proteger el acceso al clúster de OpenShift. Cree un par de claves SSH con el comando `ssh-keygen` (en Linux o macOS):
 
 ```bash
ssh-keygen -f ~/.ssh/openshift_rsa -t rsa -N ''
```

> [!NOTE]
> El par de claves SSH no puede tener una contraseña ni frase de contraseña.

Para obtener más información sobre las claves SSH en Windows, consulte el artículo sobre la [creación de claves SSH en Windows](/azure/virtual-machines/linux/ssh-from-windows). Asegúrese de exportar la clave privada en formato OpenSSH.

## <a name="store-the-ssh-private-key-in-azure-key-vault"></a>Almacenamiento de la clave privada SSH en Azure Key Vault
La implementación de OpenShift utiliza la clave SSH que creó para proteger el acceso al maestro de OpenShift. Para permitir que la implementación recupere de forma segura la clave SSH, almacénela en Key Vault con el comando siguiente:

```azurecli
az keyvault secret set --vault-name keyvault --name keysecret --file ~/.ssh/openshift_rsa
```

## <a name="create-a-service-principal"></a>Creación de una entidad de servicio 
OpenShift se comunica con Azure mediante un nombre de usuario y una contraseña, o bien a través de una entidad de servicio. Las entidades de servicio de Azure son identidades de seguridad que pueden usarse con aplicaciones, servicios y herramientas de automatización como OpenShift. El usuario controla los permisos y los define con respecto a cuáles son las operaciones que la entidad de servicio puede realizar en Azure. Es mejor definir el ámbito de los permisos de la entidad de servicio al grupos de recursos específicos en lugar de toda la suscripción.

Cree una entidad de servicio con [az ad sp create-for-rbac](/cli/azure/ad/sp) y genere las credenciales necesarias para OpenShift.

En el ejemplo siguiente se crea una entidad de servicio y se le asignan permisos de colaborador para un grupo de recursos llamado openshiftrg.

En primer lugar, cree el grupo de recursos llamado openshiftrg:

```azurecli
az group create -l eastus -n openshiftrg
```

Cree una entidad de servicio:

```azurecli
scope=`az group show --name openshiftrg --query id`
az ad sp create-for-rbac --name openshiftsp \
      --role Contributor --password {Strong Password} \
      --scopes $scope
```
Si usa Windows, ejecute ```az group show --name openshiftrg --query id``` y use la salida en lugar de $scope.

Tome nota de la propiedad appId que devuelve el comando:
```json
{
  "appId": "11111111-abcd-1234-efgh-111111111111",
  "displayName": "openshiftsp",
  "name": "http://openshiftsp",
  "password": {Strong Password},
  "tenant": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX"
}
```
 > [!WARNING] 
 > Asegúrese de crear una contraseña segura. Siga la guía de las [restricciones y reglas de contraseña de Azure AD](/azure/active-directory/active-directory-passwords-policy).

Para más información sobre las entidades de servicio, consulte [Creación de una entidad de servicio de Azure con la CLI de Azure](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest).

## <a name="prerequisites-applicable-only-to-resource-manager-template"></a>Requisitos previos aplicables únicamente a la plantilla de Resource Manager

Los secretos se deben crearse para la clave privada SSH (**sshPrivateKey**), el secreto del cliente de Azure AD (**aadClientSecret**), contraseña de administrador de OpenShift (**openshiftPassword** ) y Red Hat Subscription Manager contraseña o clave de activación (**rhsmPasswordOrActivationKey**).  Además, si se usan los certificados SSL personalizados, a continuación, seis secretos adicionales deberá crearse - **routingcafile**, **routingcertfile**, **routingkeyfile**, **mastercafile**, **mastercertfile**, y **masterkeyfile**.  Estos parámetros se explican con más detalle.

La plantilla hace referencia a nombres de secretos específicos por lo que le **debe** use los nombres en negrita mencionados anteriormente (distingue mayúsculas de minúsculas).

### <a name="custom-certificates"></a>Certificados personalizados

De forma predeterminada, la plantilla implementará un clúster de OpenShift mediante certificados autofirmados para la consola web de OpenShift y el dominio de enrutamiento. Si desea usar los certificados SSL personalizados, establecer 'routingCertType' a 'custom' y 'masterCertType' a 'custom'.  Necesitará los archivos de la entidad de certificación, certificado y clave en formato PEM para los certificados.  Es posible utilizar certificados personalizados para una pero no en la otra.

Se necesitará para almacenar estos archivos en los secretos de Key Vault.  Use el mismo almacén de claves que el utilizado para la clave privada.  En lugar de requerir 6 entradas adicionales para los nombres de secretos, la plantilla está codificada para utilizar nombres de secretos específicos para cada uno de los archivos de certificado SSL.  Store los datos del certificado con la información de la tabla siguiente.

| Nombre del secreto      | Archivo de certificado   |
|------------------|--------------------|
| mastercafile     | archivo maestro de CA     |
| mastercertfile   | archivo de certificado maestro   |
| masterkeyfile    | archivo de clave maestra    |
| routingcafile    | archivo de la entidad de certificación de enrutamiento    |
| routingcertfile  | archivo de certificado de enrutamiento  |
| routingkeyfile   | archivo de clave de enrutamiento   |

Cree los secretos mediante la CLI de Azure. A continuación es un ejemplo.

```bash
az keyvault secret set --vault-name KeyVaultName -n mastercafile --file ~/certificates/masterca.pem
```

## <a name="next-steps"></a>Pasos siguientes

En este artículo se trataron los temas siguientes:
> [!div class="checklist"]
> * Crear un almacén de claves para administrar las claves SSH para el clúster de OpenShift.
> * Crear una entidad de servicio para que la use el Proveedor de soluciones en la nube de Azure.

A continuación, implemente un clúster de OpenShift:

- [Deploy OpenShift Container Platform](./openshift-container-platform.md) (Implementar OpenShift Container Platform)
- [Implementar la oferta de Marketplace autoadministrados de plataforma de contenedor de OpenShift](./openshift-marketplace-self-managed.md)