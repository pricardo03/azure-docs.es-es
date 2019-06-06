---
title: Uso de Azure Storage como un back-end de Terraform
description: Introducción al almacenamiento del estado de Terraform en Azure Storage.
services: terraform
author: tomarchermsft
ms.service: azure
ms.topic: article
ms.date: 09/13/2018
ms.author: tarcher
ms.openlocfilehash: a88ad25e335026d5172c7997f62629d5ada46f6e
ms.sourcegitcommit: 18a0d58358ec860c87961a45d10403079113164d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/05/2019
ms.locfileid: "66693297"
---
# <a name="store-terraform-state-in-azure-storage"></a>Estado de Terraform en Azure Storage

El estado de Terraform se usa para conciliar los recursos implementados con configuraciones de Terraform. Al usar el estado, Terraform sabe qué recursos de Azure agregar, actualizar o eliminar. De forma predeterminada, el estado de Terraform se almacena localmente cuando se ejecuta *Terraform apply*. Esta configuración no es ideal por diversas razones:

- El estado local no funciona bien en un equipo ni en un entorno de colaboración
- El estado de Terraform puede incluir información confidencial
- Almacenar el estado localmente aumenta la posibilidad de eliminación accidental

Terraform incluye el concepto de un back-end de estado, que se trata de almacenamiento remoto para el estado de Terraform. Cuando se usa un back-end de estado, el archivo de estado se almacena en un almacén de datos, como Azure Storage. En este documento se describe cómo configurar y usar Azure Storage como un back-end de estado de Terraform.

## <a name="configure-storage-account"></a>Configurar la cuenta de almacenamiento

Antes de usar Azure Storage como back-end, se debe crear una cuenta de almacenamiento. La cuenta de almacenamiento puede crearse con Azure Portal, PowerShell, la CLI de Azure o Terraform. Use el ejemplo siguiente para configurar la cuenta de almacenamiento con la CLI de Azure.

```azurecli-interactive
#!/bin/bash

RESOURCE_GROUP_NAME=tstate
STORAGE_ACCOUNT_NAME=tstate$RANDOM
CONTAINER_NAME=tstate

# Create resource group
az group create --name $RESOURCE_GROUP_NAME --location eastus

# Create storage account
az storage account create --resource-group $RESOURCE_GROUP_NAME --name $STORAGE_ACCOUNT_NAME --sku Standard_LRS --encryption-services blob

# Get storage account key
ACCOUNT_KEY=$(az storage account keys list --resource-group $RESOURCE_GROUP_NAME --account-name $STORAGE_ACCOUNT_NAME --query [0].value -o tsv)

# Create blob container
az storage container create --name $CONTAINER_NAME --account-name $STORAGE_ACCOUNT_NAME --account-key $ACCOUNT_KEY

echo "storage_account_name: $STORAGE_ACCOUNT_NAME"
echo "container_name: $CONTAINER_NAME"
echo "access_key: $ACCOUNT_KEY"
```

Anote el nombre de la cuenta de almacenamiento, el nombre del contenedor y la clave de acceso de almacenamiento. Estos valores son necesarios al configurar el estado remoto.

## <a name="configure-state-backend"></a>Configuración del back-end de estado

El back-end de estado de Terraform se configura al ejecutar *Terraform init*. Para configurar el back-end de estado, se requieren los siguientes datos.

- storage_account_name: nombre de la cuenta de Azure Storage.
- container_name: nombre del contenedor de blobs.
- key: nombre del archivo de almacenamiento de estados que se va a crear.
- access_key: clave de acceso de almacenamiento.

Cada uno de estos valores puede especificarse en el archivo de configuración de Terraform o en la línea de comandos, pero se recomienda usar una variable de entorno para `access_key`. El uso de una variable de entorno evita que la clave se escriba en el disco.

Cree una variable de entorno denominada `ARM_ACCESS_KEY` con el valor de la clave de acceso de Azure Storage.

```console
export ARM_ACCESS_KEY=<storage access key>
```

Para proteger aún más la clave de acceso de la cuenta de Azure Storage, almacénela en Azure Key Vault. A continuación, se puede establecer la variable de entorno con un comando similar al siguiente. Para obtener más información acerca de Azure Key Vault, consulte la [documentación de Azure Key Vault][azure-key-vault].

```console
export ARM_ACCESS_KEY=$(az keyvault secret show --name terraform-backend-key --vault-name myKeyVault --query value -o tsv)
```

Para configurar Terraform para usar el back-end, incluya una configuración de *back-end* con un tipo de *azurerm* dentro de la configuración de Terraform. Agregue los valores *storage_account_name*, *container_name* y *key* al bloque de configuración.

El ejemplo siguiente configura un back-end de Terraform y crea un grupo de recursos de Azure. Reemplace los valores por los de su entorno.

```json
terraform {
  backend "azurerm" {
    storage_account_name  = "tstate09762"
    container_name        = "tstate"
    key                   = "terraform.tfstate"
  }
}

resource "azurerm_resource_group" "state-demo-secure" {
  name     = "state-demo"
  location = "eastus"
}
```

Ahora, inicialice la configuración con *Terraform init* y, después, ejecute la configuración con *Terraform apply*. Una vez finalizado, encontrará el archivo de estado en Azure Storage Blob.

## <a name="state-locking"></a>Estado bloqueado

Cuando se usa una instancia de Azure Storage Blob para el almacenamiento de estados, el blob se bloquea automáticamente antes de cualquier operación que escriba el estado. Esta configuración evita varias operaciones de estado simultáneas que pueden causar daños. Para obtener más información sobre Terraform, consulte la sección acerca del [estado bloqueado][terraform-state-lock] en la documentación de Terraform.

El bloqueo puede verse al examinar el blob mediante el portal de Azure o de otras herramientas de administración de Azure.

![Blob de Azure con bloqueo](media/terraform-backend/lock.png)

## <a name="encryption-at-rest"></a>Cifrado en reposo

De forma predeterminada, los datos almacenados en un blob de Azure se cifran antes de enviarse a la infraestructura de almacenamiento. Cuando Terraform necesita un estado, se recupera del back-end y se almacena en la memoria de su sistema de desarrollo. En esta configuración, el estado se protege en Azure Storage y no se escribe en el disco local.

Para obtener más información acerca del cifrado de Azure Storage, consulte [Azure Storage Service Encryption para datos en reposo][azure-storage-encryption].

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre la configuración del back-end de Terraform en la [documentación del back-end de Terraform][terraform-backend].

<!-- LINKS - internal -->
[azure-key-vault]: ../key-vault/quick-create-cli.md
[azure-storage-encryption]: ../storage/common/storage-service-encryption.md

<!-- LINKS - external -->
[terraform-azurerm]: https://www.terraform.io/docs/backends/types/azurerm.html
[terraform-backend]: https://www.terraform.io/docs/backends/
[terraform-state-lock]: https://www.terraform.io/docs/state/locking.html
