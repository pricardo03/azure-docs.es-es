---
title: Ejecute los comandos de PowerShell o la CLI de Azure con credenciales de Azure AD para tener acceso a datos blob o cola | Microsoft Docs
description: PowerShell y CLI de Azure admiten iniciar sesión con credenciales de Azure AD para ejecutar comandos en los datos de blobs y colas de Azure Storage. Se proporciona un token de acceso para la sesión y se usa para autorizar operaciones de llamada. Los permisos dependen el rol RBAC asignado a la entidad de seguridad de Azure AD.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 04/19/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 41ca1c5f413e5e15691f336d203edb918f21dc1a
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65147289"
---
# <a name="run-azure-cli-or-powershell-commands-with-azure-ad-credentials-to-access-blob-or-queue-data"></a>Ejecute los comandos de PowerShell o la CLI de Azure con credenciales de Azure AD para tener acceso a datos blob o cola

Almacenamiento de Azure proporciona extensiones para la CLI de Azure y PowerShell que le permiten iniciar sesión y ejecutar secuencias de comandos con credenciales de Azure Active Directory (Azure AD). Al iniciar sesión PowerShell o la CLI de Azure con credenciales de Azure AD, se devuelve un token de acceso de OAuth 2.0. Ese token se usa automáticamente mediante la CLI o PowerShell para autorizar las operaciones de datos subsiguientes en el almacenamiento de Blob o cola. Para las operaciones admitidas, ya no necesita pasar una clave de cuenta o token SAS con el comando.

Puede asignar permisos a los datos de blob y cola a una entidad de seguridad de Azure AD a través del control de acceso basado en roles (RBAC). Para obtener más información acerca de los roles RBAC en Azure Storage, consulte [administrar derechos de acceso a datos de Azure Storage con RBAC](storage-auth-aad-rbac.md).

## <a name="supported-operations"></a>Operaciones compatibles

Se admiten las extensiones para las operaciones en contenedores y las colas. Las operaciones que puede llamar a depende de los permisos concedidos a la entidad de seguridad de Azure AD con el que iniciar sesión en PowerShell o la CLI de Azure. Los permisos para los contenedores o colas de Azure Storage se asignan mediante el control de acceso basado en rol (RBAC). Por ejemplo, si se le asigna el **lector de datos de Blob** rol y, después, se pueden ejecutar secuencias de comandos que leen datos de un contenedor o una cola. Si se le asigna el **colaborador de datos de Blob** rol y, después, se pueden ejecutar secuencias de comandos que leer, escribirán o eliminación un contenedor o la cola o los datos que contienen. 

Para más información sobre los permisos requeridos para cada operación de Azure Storage, consulte [Permissions for calling REST operations](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-rest-operations) (Permisos para llamar a operaciones de REST).  

## <a name="call-cli-commands-using-azure-ad-credentials"></a>Llamar a comandos CLI mediante credenciales de Azure AD

CLI de Azure es compatible con la `--auth-mode` parámetro para las operaciones de datos de blob y cola:

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
    
1. Antes de crear el contenedor, asigne el [colaborador de datos de almacenamiento Blob](../../role-based-access-control/built-in-roles.md#storage-blob-data-contributor) rol a sí mismo. Aunque sea el propietario de la cuenta, necesita los permisos explícitos para realizar operaciones de datos en la cuenta de almacenamiento. Para obtener más información sobre la asignación de roles de RBAC, consulte [conceder acceso a datos blob y cola de Azure con RBAC en Azure portal](storage-auth-aad-rbac.md).

    > [!IMPORTANT]
    > Las asignaciones de roles RBAC pueden tardar unos minutos en propagarse.
    
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

1. Antes de crear el contenedor, asigne el [colaborador de datos de almacenamiento Blob](../../role-based-access-control/built-in-roles.md#storage-blob-data-contributor) rol a sí mismo. Aunque sea el propietario de la cuenta, necesita los permisos explícitos para realizar operaciones de datos en la cuenta de almacenamiento. Para obtener más información sobre la asignación de roles de RBAC, consulte [conceder acceso a datos blob y cola de Azure con RBAC en Azure portal](storage-auth-aad-rbac.md).

    > [!IMPORTANT]
    > Las asignaciones de roles RBAC pueden tardar unos minutos en propagarse.

1. Crear un contenedor mediante una llamada a [New AzStorageContainer](/powershell/module/az.storage/new-azstoragecontainer). Dado que esta llamada usa el contexto creado en los pasos anteriores, se crea el contenedor con sus credenciales de Azure AD. 

    ```powershell
    $containerName = "sample-container"
    New-AzStorageContainer -Name $containerName -Context $ctx
    ```

## <a name="next-steps"></a>Pasos siguientes

- Para obtener más información acerca de los roles RBAC para el almacenamiento de Azure, consulte [administrar derechos de acceso a los datos de almacenamiento con RBAC](storage-auth-aad-rbac.md).
- Para obtener información sobre el uso de identidades administradas por los recursos de Azure con Azure Storage, consulte [autenticar el acceso a los blobs y colas con Azure Active Directory y las identidades administradas para Azure Resources](storage-auth-aad-msi.md).
- Para información sobre la autorización de acceso a los contenedores y las colas desde las aplicaciones de almacenamiento, consulte el artículo sobre el [uso de Azure AD con aplicaciones de almacenamiento](storage-auth-aad-app.md).
