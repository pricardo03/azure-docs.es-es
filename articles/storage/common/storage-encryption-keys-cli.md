---
title: Configurar las claves administradas por el cliente para el cifrado de almacenamiento de Azure desde la CLI de Azure
description: Obtenga información sobre cómo usar la CLI de Azure para configurar las claves administradas por el cliente para el cifrado de almacenamiento de Azure. Claves administradas por el cliente permiten crear, girar, deshabilitar y revocar el acceso a los controles.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 04/16/2019
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: dc9f660ddf7cd003ac113725a9bf7b66189807b6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61593598"
---
# <a name="configure-customer-managed-keys-for-azure-storage-encryption-from-azure-cli"></a>Configurar las claves administradas por el cliente para el cifrado de almacenamiento de Azure desde la CLI de Azure

[!INCLUDE [storage-encryption-configure-keys-include](../../../includes/storage-encryption-configure-keys-include.md)]

En este artículo se muestra cómo configurar un almacén de claves con claves administradas por el cliente mediante la CLI de Azure.

## <a name="assign-an-identity-to-the-storage-account"></a>Asignar una identidad para la cuenta de almacenamiento

Para habilitar claves administradas por el cliente para la cuenta de almacenamiento, primero asignar una identidad administrada asignado por el sistema a la cuenta de almacenamiento. Usará esta identidad administrada para conceder los permisos de cuenta de almacenamiento para acceder al almacén de claves.

Para asignar una identidad administrada mediante la CLI de Azure, llame al [actualización de cuenta de almacenamiento de az](/cli/azure/storage/account#az-storage-account-update). No olvide reemplazar los valores de marcador de posición entre corchetes por los suyos propios.

```azurecli-interactive
az account set --subscription <subscription-id>

az storage account update \
    --name <storage-account> \
    --resource-group <resource_group> \
    --assign-identity
```

Para obtener más información sobre cómo configurar asignado por el sistema de identidades administradas con la CLI de Azure, consulte [configurar identidades para los recursos de Azure en una máquina virtual de Azure mediante la CLI de Azure de administradas](../../active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm.md).

## <a name="create-a-new-key-vault"></a>Crear un nuevo almacén de claves

El almacén de claves que se usará para almacenar las claves administradas por el cliente para el cifrado de almacenamiento de Azure debe tener dos configuraciones de protección de claves habilitadas, **eliminación temporal** y **no purgar**. Para crear un nuevo almacén de claves con PowerShell o CLI de Azure con esta configuración habilitada, ejecute los siguientes comandos. No olvide reemplazar los valores de marcador de posición entre corchetes por los suyos propios. 

Para crear un nuevo almacén de claves mediante la CLI de Azure, llame a [crear az keyvault](/cli/azure/keyvault#az-keyvault-create). No olvide reemplazar los valores de marcador de posición entre corchetes por los suyos propios.

```azurecli-interactive
az keyvault create \
    --name <key-vault> \
    --resource-group <resource_group> \
    --location <region> \
    --enable-soft-delete \
    --enable-purge-protection
```

## <a name="configure-the-key-vault-access-policy"></a>Configurar la directiva de acceso del almacén de claves

A continuación, configure la directiva de acceso para el almacén de claves para que la cuenta de almacenamiento tenga permisos para acceder a él. En este paso, usará la identidad administrada que asignó previamente a la cuenta de almacenamiento.

Para establecer la directiva de acceso para el almacén de claves, llame a [az keyvault set-policy](/cli/azure/keyvault#az-keyvault-set-policy). No olvide reemplazar los valores de marcador de posición entre corchetes por los suyos propios.

```azurecli-interactive
storage_account_principal=$(az storage account show \
    -name <storage-account> \
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

A continuación, cree una clave en el almacén de claves. Para crear una clave, llame a [crear clave de az keyvault](/cli/azure/keyvault/key#az-keyvault-key-create). No olvide reemplazar los valores de marcador de posición entre corchetes por los suyos propios.

```azurecli-interactive
az keyvault key create
    --name <key> \
    --vault-name <key-vault>
```

## <a name="configure-encryption-with-customer-managed-keys"></a>Configurar el cifrado con claves administradas por el cliente

De forma predeterminada, el cifrado de almacenamiento de Azure usa claves administradas por Microsoft. Configurar la cuenta de almacenamiento de Azure para las claves administradas por el cliente y especifique la clave para asociar la cuenta de almacenamiento.

Para actualizar la configuración de cifrado de la cuenta de almacenamiento, llame a [actualización de cuenta de almacenamiento de az](/cli/azure/storage/account#az-storage-account-update). En este ejemplo también se consulta para el URI de almacén de claves y la versión de la clave, ambos de los valores que son necesarios para asociar la clave de la cuenta de almacenamiento. No olvide reemplazar los valores de marcador de posición entre corchetes por los suyos propios.

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

Cuando se crea una nueva versión de una clave, deberá actualizar la cuenta de almacenamiento para usar la nueva versión. En primer lugar, consultar el URI del almacén de claves mediante una llamada a [show de az keyvault](/cli/azure/keyvault#az-keyvault-show)y para la versión de la clave mediante una llamada a [az keyvault clave enumerar versiones](/cli/azure/keyvault/key#az-keyvault-key-list-versions). A continuación, llame a [actualización de cuenta de almacenamiento de az](/cli/azure/storage/account#az-storage-account-update) para actualizar la configuración de cifrado de la cuenta de almacenamiento para usar la nueva versión de la clave, como se muestra en la sección anterior.

## <a name="next-steps"></a>Pasos siguientes

- [Cifrado de Azure Storage para datos en reposo](storage-service-encryption.md) 
- [¿Qué es Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-whatis)?
