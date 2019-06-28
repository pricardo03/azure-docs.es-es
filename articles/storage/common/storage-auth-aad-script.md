---
title: Ejecución de la CLI de Azure o comandos de PowerShell con credenciales de Azure AD para acceder a datos de blob o de cola | Microsoft Docs
description: La CLI de Azure y PowerShell admiten el inicio de sesión con credenciales de Azure AD para ejecutar comandos en los datos de blob y de cola de Azure Storage. Se proporciona un token de acceso para la sesión y se usa para autorizar operaciones de llamada. Los permisos dependen del rol RBAC asignado a la entidad de seguridad de Azure AD.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 04/19/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 41ca1c5f413e5e15691f336d203edb918f21dc1a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65147289"
---
# <a name="run-azure-cli-or-powershell-commands-with-azure-ad-credentials-to-access-blob-or-queue-data"></a>Ejecución de la CLI de Azure o comandos de PowerShell con credenciales de Azure AD para acceder a datos de blob o de cola

Azure Storage proporciona extensiones para la CLI de Azure y PowerShell que le permiten iniciar sesión y ejecutar comandos de scripting con credenciales de Azure Active Directory (Azure AD). Al iniciar sesión en la CLI de Azure o en PowerShell con credenciales de Azure AD, se devuelve un token de acceso OAuth 2.0. La CLI de Azure o PowerShell usan automáticamente ese token para autorizar las operaciones de datos posteriores en Blob Storage o Queue Storage. Para las operaciones admitidas, ya no necesita pasar una clave de cuenta o token SAS con el comando.

Puede asignar permisos en los datos de blob y de cola a una entidad de seguridad de Azure AD a través del control de acceso basado en rol (RBAC). Para más información sobre los roles RBAC en Azure Storage, consulte [Administración de los derechos de acceso a los datos de Azure Storage con RBAC](storage-auth-aad-rbac.md).

## <a name="supported-operations"></a>Operaciones compatibles

Se admiten las extensiones para las operaciones en contenedores y colas. Las operaciones a las que podrá llamar dependerán de los permisos que se concedan a la entidad de seguridad de Azure AD con la que inicie sesión en la CLI de Azure o en PowerShell. Los permisos para los contenedores o colas de Azure Storage se asignan mediante el control de acceso basado en rol (RBAC). Por ejemplo, si se le asigna el rol de **lector de datos de blob**, puede ejecutar comandos de scripting que lean datos de un contenedor o cola. Si se le asigna el rol de **colaborador de datos de blob**, podrá ejecutar comandos de scripting que lean, escriban o eliminen un contenedor o cola, o los datos que estos contienen. 

Para más información sobre los permisos requeridos para cada operación de Azure Storage, consulte [Permissions for calling REST operations](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-rest-operations) (Permisos para llamar a operaciones de REST).  

## <a name="call-cli-commands-using-azure-ad-credentials"></a>Llamada a comandos de la CLI mediante credenciales de Azure AD

La CLI de Azure es compatible con el parámetro `--auth-mode` para las operaciones de datos de blob y cola:

- Establezca el parámetro `--auth-mode` en `login` para iniciar sesión mediante una entidad de seguridad de Azure AD.
- Establezca el parámetro `--auth-mode` en el valor heredado `key` para intentar consultar una clave de cuenta si no se proporcionan parámetros de autenticación para la cuenta. 

En el ejemplo siguiente se muestra cómo crear un contenedor en una nueva cuenta de almacenamiento desde la CLI de Azure mediante las credenciales de Azure AD. No olvide reemplazar los valores del marcador de posición entre corchetes angulares por sus propios valores: 

1. Asegúrese de que tiene instalada la CLI de Azure versión 2.0.46 o posterior. Ejecute `az --version` para comprobar la versión instalada.

1. Ejecute `az login` y autentíquese en la ventana del explorador: 

    ```azurecli
    az login
    ```
    
1. Especifique la suscripción deseada. Para crear un grupo de recursos, use [az group create](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest#az-group-create). Cree una cuenta de almacenamiento en ese grupo de recursos mediante [az storage account create](https://docs.microsoft.com/cli/azure/storage/account?view=azure-cli-latest#az-storage-account-create): 

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
    
1. Antes de crear el contenedor, asígnese a sí mismo el rol [Colaborador de datos de Storage Blob](../../role-based-access-control/built-in-roles.md#storage-blob-data-contributor). Aunque sea el propietario de la cuenta, necesita permisos explícitos para realizar operaciones de datos en la cuenta de almacenamiento. Para obtener más información sobre la asignación de roles RBAC, consulte [Conceder acceso a datos blob y cola de Azure con RBAC en Azure Portal](storage-auth-aad-rbac.md).

    > [!IMPORTANT]
    > La propagación de las asignaciones de roles RBAC pueden tardar unos minutos.
    
1. Llame al comando [az storage container create](https://docs.microsoft.com/cli/azure/storage/container?view=azure-cli-latest#az-storage-container-create) con el parámetro `--auth-mode` establecido en `login` para crear el contenedor con sus credenciales de Azure AD:

    ```azurecli
    az storage container create \ 
        --account-name <storage-account> \ 
        --name sample-container \
        --auth-mode login
    ```

La variable de entorno asociada con el parámetro `--auth-mode` es `AZURE_STORAGE_AUTH_MODE`. Puede especificar el valor adecuado en la variable de entorno para evitar incluirlo en cada llamada a una operación de datos de Azure Storage.

## <a name="call-powershell-commands-using-azure-ad-credentials"></a>Llamada a comandos de PowerShell mediante credenciales de Azure AD

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Si quiere usar Azure PowerShell para iniciar sesión y ejecutar operaciones posteriores en Azure Storage mediante credenciales de Azure AD, cree un contexto de almacenamiento para hacer referencia a la cuenta de almacenamiento e incluya el parámetro `-UseConnectedAccount`.

En el ejemplo siguiente se muestra cómo crear un contenedor en una nueva cuenta de almacenamiento desde Azure PowerShell mediante las credenciales de Azure AD. No olvide reemplazar los valores del marcador de posición entre corchetes angulares por sus propios valores:

1. Inicie sesión en la suscripción de Azure con el comando `Connect-AzAccount` y siga las instrucciones que aparecen en pantalla para especificar las credenciales de Azure AD: 

    ```powershell
    Connect-AzAccount
    ```
    
1. Cree un grupo de recursos de Azure mediante una llamada a [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup). 

    ```powershell
    $resourceGroup = "sample-resource-group-ps"
    $location = "eastus"
    New-AzResourceGroup -Name $resourceGroup -Location $location
    ```

1. Cree una cuenta de almacenamiento mediante una llamada a [New-AzStorageAccount](/powershell/module/az.storage/new-azstorageaccount).

    ```powershell
    $storageAccount = New-AzStorageAccount -ResourceGroupName $resourceGroup `
      -Name "<storage-account>" `
      -SkuName Standard_LRS `
      -Location $location `
    ```

1. Obtenga el contexto de la cuenta de almacenamiento que especifica la nueva cuenta de almacenamiento mediante una llamada a [New-AzStorageContext](/powershell/module/az.storage/new-azstoragecontext). Cuando actúe en una cuenta de almacenamiento, puede hacer referencia al contexto en lugar de proporcionar varias veces las credenciales. Incluya el parámetro `-UseConnectedAccount` para llamar a las operaciones de datos posteriores con sus credenciales de Azure AD:

    ```powershell
    $ctx = New-AzStorageContext -StorageAccountName "<storage-account>" -UseConnectedAccount
    ```

1. Antes de crear el contenedor, asígnese a sí mismo el rol [Colaborador de datos de Storage Blob](../../role-based-access-control/built-in-roles.md#storage-blob-data-contributor). Aunque sea el propietario de la cuenta, necesita permisos explícitos para realizar operaciones de datos en la cuenta de almacenamiento. Para obtener más información sobre la asignación de roles RBAC, consulte [Conceder acceso a datos blob y cola de Azure con RBAC en Azure Portal](storage-auth-aad-rbac.md).

    > [!IMPORTANT]
    > La propagación de las asignaciones de roles RBAC pueden tardar unos minutos.

1. Cree un contenedor mediante una llamada a [New-AzStorageContainer](/powershell/module/az.storage/new-azstoragecontainer). Dado que esta llamada usa el contexto creado en los pasos anteriores, el contenedor se crea con sus credenciales de Azure AD. 

    ```powershell
    $containerName = "sample-container"
    New-AzStorageContainer -Name $containerName -Context $ctx
    ```

## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre los roles RBAC en Azure Storage, consulte [Administración de los derechos de acceso a los datos de Azure Storage con RBAC](storage-auth-aad-rbac.md).
- Para más información sobre el uso de identidades administradas para los recursos de Azure con Azure Storage, consulte [Autenticar el acceso a los blobs y colas con Azure Active Directory y las identidades administradas para los recursos de Azure](storage-auth-aad-msi.md).
- Para información sobre la autorización de acceso a los contenedores y las colas desde las aplicaciones de almacenamiento, consulte el artículo sobre el [uso de Azure AD con aplicaciones de almacenamiento](storage-auth-aad-app.md).
