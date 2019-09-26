---
title: Configurar las claves que administra el cliente para el cifrado de Azure Storage desde la CLI de Azure.
description: Configurar las claves que administra el cliente para el cifrado de Azure Storage desde la CLI de Azure. Las claves que administra el cliente le ofrecen más flexibilidad para crear, girar, deshabilitar y revocar los controles de acceso.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 09/17/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: ba6cc03a93b664e05a667116cbf845e777416c6b
ms.sourcegitcommit: 116bc6a75e501b7bba85e750b336f2af4ad29f5a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/20/2019
ms.locfileid: "71155413"
---
# <a name="configure-customer-managed-keys-for-azure-storage-encryption-from-azure-cli"></a>Configurar las claves que administra el cliente para el cifrado de Azure Storage desde la CLI de Azure.

[!INCLUDE [storage-encryption-configure-keys-include](../../../includes/storage-encryption-configure-keys-include.md)]

En este artículo se muestra cómo configurar un almacén de claves con claves que administra el cliente mediante la CLI de Azure.

> [!IMPORTANT]
> El uso de claves administradas del cliente con el cifrado de Azure Storage requiere que el almacén de claves tenga configuradas dos propiedades obligatorias, **Eliminación temporal** y **No purgar**. Estas propiedades están habilitadas de manera predeterminada cuando crea un nuevo almacén de claves en Azure Portal. Sin embargo, si tiene que habilitar estas propiedades en un almacén de claves existente, deberá usar PowerShell o la CLI de Azure.
> Solo se admiten claves RSA y el tamaño de clave 2048.

## <a name="assign-an-identity-to-the-storage-account"></a>Asignación de una identidad a la cuenta de almacenamiento

Para habilitar claves que administra el cliente para la cuenta de almacenamiento, debe asignar primero una identidad administrada que haya asignado el sistema a la cuenta de almacenamiento. Usará esta identidad administrada para conceder los permisos de la cuenta de almacenamiento para obtener acceso al almacén de claves.

Para asignar una identidad administrada mediante la CLI de Azure, llame al comando [az storage account update](/cli/azure/storage/account#az-storage-account-update). No olvide reemplazar los valores del marcador de posición entre corchetes con sus propios valores.

```azurecli-interactive
az account set --subscription <subscription-id>

az storage account update \
    --name <storage-account> \
    --resource-group <resource_group> \
    --assign-identity
```

Para obtener información sobre cómo configurar identidades administradas que haya asignado el sistema con la CLI de Azure, consulte [Configure managed identities for Azure resources on an Azure VM using Azure CLI](../../active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm.md) (Configuración de identidades administradas de recursos de Azure en una VM de Azure con la CLI de Azure).

## <a name="create-a-new-key-vault"></a>Crear un almacén de claves nuevo

El almacén de claves que se usará para almacenar las claves que administre el cliente para el cifrado de Azure Storage debe tener dos configuraciones de protección de claves habilitadas: la **eliminación temporal** y la opción de **no purgar**. Para crear un nuevo almacén de claves con PowerShell o la CLI de Azure mediante esta configuración habilitada, ejecute los siguientes comandos. No olvide reemplazar los valores del marcador de posición entre corchetes con sus propios valores. 

Para crear un nuevo almacén de claves con la CLI de Azure, llame al comando [az keyvault create](/cli/azure/keyvault#az-keyvault-create). No olvide reemplazar los valores del marcador de posición entre corchetes con sus propios valores.

```azurecli-interactive
az keyvault create \
    --name <key-vault> \
    --resource-group <resource_group> \
    --location <region> \
    --enable-soft-delete \
    --enable-purge-protection
```

## <a name="configure-the-key-vault-access-policy"></a>Configuración de la directiva de acceso del almacén de claves

A continuación, configure la directiva de acceso del almacén de claves, para que la cuenta de almacenamiento tenga permiso para acceder a él. En este paso, usará la identidad administrada que asignó previamente a la cuenta de almacenamiento.

Use [az keyvault set-policy](/cli/azure/keyvault#az-keyvault-set-policy) para establecer la directiva de acceso del almacén de claves. No olvide reemplazar los valores del marcador de posición entre corchetes con sus propios valores.

```azurecli-interactive
storage_account_principal=$(az storage account show \
    --name <storage-account> \
    --resource-group <resource-group> \
    --query identity.principalId \
    --output tsv)
az keyvault set-policy \
    --name <key-vault> \
    --resource-group <resource_group>
    --object-id $storage_account_principal \
    --key-permissions get recover unwrapKey wrapKey
```

## <a name="create-a-new-key"></a>Creación de una nueva clave

A continuación, cree una clave en el almacén de claves. Para crear una clave, llame a [az keyvault key create](/cli/azure/keyvault/key#az-keyvault-key-create). No olvide reemplazar los valores del marcador de posición entre corchetes con sus propios valores.

```azurecli-interactive
az keyvault key create
    --name <key> \
    --vault-name <key-vault>
```

## <a name="configure-encryption-with-customer-managed-keys"></a>Configurar el cifrado con claves que administra el cliente

De forma predeterminada, el cifrado de Azure Storage usa claves que administra Microsoft. Configure la cuenta de Azure Storage para las claves que administra el cliente y especifique la clave para asociar a la cuenta de almacenamiento.

Para actualizar la configuración de cifrado de la cuenta de almacenamiento, llame a [az storage account update](/cli/azure/storage/account#az-storage-account-update). En este ejemplo también se consulta el URI del almacén de claves y la versión de la clave; ambos valores que son necesarios para asociar la clave de la cuenta de almacenamiento. No olvide reemplazar los valores del marcador de posición entre corchetes con sus propios valores.

```azurecli-interactive
key_vault_uri=$(az keyvault show \
    --name <key-vault> \
    --resource-group <resource_group> \
    --query properties.vaultUri \
    --output tsv)
key_version=$(az keyvault key list-versions \
    --name <key> \
    --vault-name <key-vault> \
    --query [].kid \
    --output tsv | cut -d '/' -f 6)
az storage account update 
    --name <storage-account> \
    --resource-group <resource_group> \
    --encryption-key-name <key> \
    --encryption-key-version $key_version \
    --encryption-key-source Microsoft.Keyvault \
    --encryption-key-vault $key_vault_uri
```

## <a name="update-the-key-version"></a>Actualizar la versión de la clave

Cuando se crea una nueva versión de una clave, deberá actualizar la cuenta de almacenamiento para usar la nueva versión. En primer lugar, consulte el URI del almacén de claves mediante una llamada a [az keyvault show](/cli/azure/keyvault#az-keyvault-show) y, para la versión de la clave, llame a [az keyvault key list-versions](/cli/azure/keyvault/key#az-keyvault-key-list-versions). A continuación, llame a [az storage account update](/cli/azure/storage/account#az-storage-account-update) para actualizar la configuración de cifrado de la cuenta de almacenamiento y así poder usar la nueva versión de la clave, tal como se muestra en la sección anterior.

## <a name="next-steps"></a>Pasos siguientes

- [Cifrado de Azure Storage para datos en reposo](storage-service-encryption.md) 
- [¿Qué es Azure Key Vault?](https://docs.microsoft.com/azure/key-vault/key-vault-overview)
