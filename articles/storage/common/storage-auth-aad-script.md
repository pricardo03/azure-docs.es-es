---
title: Ejecute los comandos de PowerShell o la CLI de Azure bajo una identidad de Azure AD para acceder al almacenamiento de Azure | Microsoft Docs
description: La CLI de Azure y PowerShell admiten el inicio de sesión con una identidad de Azure AD para ejecutar comandos en contenedores y colas de Azure Storage y en sus datos. Se proporciona un token de acceso para la sesión y se usa para autorizar operaciones de llamada. Los permisos dependen del rol asignado a la identidad de Azure AD.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 03/06/2019
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: f8fd3cdcf73749d787fc6f1c2222946961091f80
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "57849846"
---
# <a name="use-an-azure-ad-identity-to-access-azure-storage-with-cli-or-powershell"></a>Usar una identidad de Azure AD para acceder a Azure Storage con PowerShell o CLI

Almacenamiento de Azure proporciona extensiones para la CLI de Azure y PowerShell que le permiten iniciar sesión y ejecutar secuencias de comandos bajo una identidad de Azure Active Directory (Azure AD). La identidad de Azure AD puede ser un usuario, un grupo o una entidad de servicio de aplicación, o puede ser una [identidad administrada para los recursos de Azure](../../active-directory/managed-identities-azure-resources/overview.md). Puede asignar permisos para acceder a recursos de almacenamiento en la identidad de Azure AD a través del control de acceso basado en rol (RBAC). Para más información sobre los roles de RBAC en Azure Storage, consulte [Administración de los derechos de acceso a los datos de Azure Storage con RBAC (versión preliminar)](storage-auth-aad-rbac.md).

Cuando inicia sesión en la CLI de Azure o en PowerShell con una identidad de Azure AD, se devuelve un token de acceso para acceder a Azure Storage con esa identidad. Después, la CLI de Azure o PowerShell usan automáticamente ese token para autorizar operaciones en Azure Storage. Para las operaciones admitidas, ya no necesita pasar una clave de cuenta o token SAS con el comando.

## <a name="supported-operations"></a>Operaciones compatibles

Se admiten las extensiones para las operaciones en contenedores y las colas. Las operaciones a las que podrá llamar dependerán de los permisos que se concedan a la identidad de Azure AD con la que inicie sesión en la CLI de Azure o en PowerShell. Los permisos para los contenedores o colas de Azure Storage se asignan mediante el control de acceso basado en rol (RBAC). Por ejemplo, si se asigna un rol de lector de datos a la identidad, puede ejecutar comandos de scripting que lean datos de un contenedor o cola. Si se asigna un rol de colaborador de datos a la identidad, podrá ejecutar comandos de scripting que lean, escriban o eliminen un contenedor o cola, o los datos que estos contienen. 

Para más información sobre los permisos requeridos para cada operación de Azure Storage, consulte [Permissions for calling REST operations](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-rest-operations) (Permisos para llamar a operaciones de REST).  

## <a name="call-cli-commands-using-azure-ad-credentials"></a>Llamar a comandos CLI mediante credenciales de Azure AD

CLI de Azure es compatible con la `--auth-mode` parámetro para las operaciones de datos en Azure Storage:

- Establecer el `--auth-mode` parámetro `login` que inicie sesión con una entidad de seguridad de Azure AD.
- Establezca el parámetro `--auth-mode` en el valor heredado `key` para intentar consultar una clave de cuenta si no se proporcionan parámetros de autenticación para la cuenta. 

El ejemplo siguiente muestra cómo crear un contenedor en una nueva cuenta de almacenamiento desde la CLI de Azure con sus credenciales de Azure AD. No olvide reemplazar los valores de marcador de posición en corchetes angulares con sus propios valores: 

1. Asegúrese de que ha instalado la versión de CLI de Azure 2.0.46 o posterior. Ejecute `az --version` para comprobar la versión instalada.

1. Ejecute `az login` y autenticarse en la ventana del explorador: 

    ```azurecli
    az login
    ```
    
1. Especifique la suscripción deseada. Para crear un grupo de recursos, use [az group create](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest#az-group-create). Crear una cuenta de almacenamiento dentro de ese grupo de recursos con [crear cuenta de almacenamiento de az](https://docs.microsoft.com/cli/azure/storage/account?view=azure-cli-latest#az-storage-account-create): 

    ```azurecli
    az account set --subscription <subscription-id>

    az group create \
        --name sample-resource-group-cli \
        --location eastus

    az storage account create \
        --name <storage-account> \
        --resource-group sample-resource-group-cli \
        --location eastus \
        --sku Standard_LRS \
        --encryption-services blob
    ```
    
1. Antes de crear el contenedor, asigne el [colaborador de datos de Blob de almacenamiento (versión preliminar)](../../role-based-access-control/built-in-roles.md#storage-blob-data-contributor-preview) rol a sí mismo. Aunque sea el propietario de la cuenta, necesita los permisos explícitos para realizar operaciones de datos en la cuenta de almacenamiento. Para obtener más información sobre la asignación de roles de RBAC, consulte [conceder acceso a los contenedores de Azure y colas con RBAC en Azure portal (versión preliminar)](storage-auth-aad-rbac.md).

    > [!IMPORTANT]
    > Durante la versión preliminar de Azure AD compatibilidad para blobs y colas, las asignaciones de roles RBAC pueden tardar hasta 5 minutos en propagarse.
    
1. Llame a la [crear contenedor de almacenamiento de az](https://docs.microsoft.com/cli/azure/storage/container?view=azure-cli-latest#az-storage-container-create) comando con el `--auth-mode` parámetro establecido en `login` para crear el contenedor con sus credenciales de Azure AD:

    ```azurecli
    az storage container create \ 
        --account-name <storage-account> \ 
        --name sample-container \
        --auth-mode login
    ```

La variable de entorno asociada con el parámetro `--auth-mode` es `AZURE_STORAGE_AUTH_MODE`. Puede especificar el valor adecuado en la variable de entorno para evitar la inclusión en cada llamada a una operación de datos de Azure Storage.

## <a name="call-powershell-commands-using-azure-ad-credentials"></a>Llamar a comandos de PowerShell con credenciales de Azure AD

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Para usar Azure PowerShell para iniciar sesión y ejecutar las operaciones siguientes en Azure Storage mediante credenciales de Azure AD, crear un contexto de almacenamiento para hacer referencia a la cuenta de almacenamiento y la inclusión del `-UseConnectedAccount` parámetro.

El ejemplo siguiente muestra cómo crear un contenedor en una nueva cuenta de almacenamiento de Azure PowerShell con sus credenciales de Azure AD. No olvide reemplazar los valores de marcador de posición en corchetes angulares con sus propios valores:

1. Inicie sesión en su suscripción de Azure con el `Connect-AzAccount` comando y siga el en pantalla las direcciones que especifique sus credenciales de Azure AD: 

    ```powershell
    Connect-AzAccount
    ```
    
1. Crear un grupo de recursos de Azure mediante una llamada a [New AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup). 

    ```powershell
    $resourceGroup = "sample-resource-group-ps"
    $location = "eastus"
    New-AzResourceGroup -Name $resourceGroup -Location $location
    ```

1. Crear una cuenta de almacenamiento mediante una llamada a [New AzStorageAccount](/powershell/module/az.storage/new-azstorageaccount).

    ```powershell
    $storageAccount = New-AzStorageAccount -ResourceGroupName $resourceGroup `
      -Name "<storage-account>" `
      -SkuName Standard_LRS `
      -Location $location `
    ```

1. Obtener el contexto de la cuenta de almacenamiento que especifica la nueva cuenta de almacenamiento mediante una llamada a [New AzStorageContext](/powershell/module/az.storage/new-azstoragecontext). Cuando actúa en una cuenta de almacenamiento, puede hacer referencia al contexto en lugar de pasar varias veces las credenciales. Incluir el `-UseConnectedAccount` parámetro al llamar a las operaciones de datos subsiguientes con sus credenciales de Azure AD:

    ```powershell
    $ctx = New-AzStorageContext -StorageAccountName "<storage-account>" -UseConnectedAccount
    ```

1. Antes de crear el contenedor, asigne el [colaborador de datos de Blob de almacenamiento (versión preliminar)](../../role-based-access-control/built-in-roles.md#storage-blob-data-contributor-preview) rol a sí mismo. Aunque sea el propietario de la cuenta, necesita los permisos explícitos para realizar operaciones de datos en la cuenta de almacenamiento. Para obtener más información sobre la asignación de roles de RBAC, consulte [conceder acceso a los contenedores de Azure y colas con RBAC en Azure portal (versión preliminar)](storage-auth-aad-rbac.md).

    > [!IMPORTANT]
    > Durante la versión preliminar de Azure AD compatibilidad para blobs y colas, las asignaciones de roles RBAC pueden tardar hasta 5 minutos en propagarse.

1. Crear un contenedor mediante una llamada a [New AzStorageContainer](/powershell/module/az.storage/new-azstoragecontainer). Dado que esta llamada usa el contexto creado en los pasos anteriores, se crea el contenedor con sus credenciales de Azure AD. 

    ```powershell
    $containerName = "sample-container"
    New-AzStorageContainer -Name $containerName -Context $ctx
    ```

## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre los roles RBAC en Azure Storage, consulte [Administración de los derechos de acceso a los datos de Azure Storage con RBAC (versión preliminar)](storage-auth-aad-rbac.md).
- Para más información acerca del uso de identidades administradas para Azure Resources con Azure Storage, consulte [Autenticación con Azure AD mediante Azure Managed Service Identity (versión preliminar)](storage-auth-aad-msi.md).
- Para información sobre la autorización de acceso a los contenedores y las colas desde las aplicaciones de almacenamiento, consulte el artículo sobre el [uso de Azure AD con aplicaciones de almacenamiento](storage-auth-aad-app.md).