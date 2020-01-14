---
title: Ejecución de comandos de la CLI de Azure con credenciales de Azure AD para acceder a datos de blob o de cola
titleSuffix: Azure Storage
description: La CLI de Azure admite el inicio de sesión con credenciales de Azure AD para ejecutar comandos en los datos de blob y de cola de Azure Storage. Se proporciona un token de acceso para la sesión y se usa para autorizar operaciones de llamada. Los permisos dependen del rol RBAC asignado a la entidad de seguridad de Azure AD.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 12/30/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: bfbb466b7679e841c9c0154dbe28de14574d4282
ms.sourcegitcommit: ec2eacbe5d3ac7878515092290722c41143f151d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/31/2019
ms.locfileid: "75553265"
---
# <a name="run-azure-cli-commands-with-azure-ad-credentials-to-access-blob-or-queue-data"></a>Ejecución de comandos de la CLI de Azure con credenciales de Azure AD para acceder a datos de blob o de cola

Azure Storage proporciona extensiones para la CLI de Azure que le permiten iniciar sesión y ejecutar comandos de scripting con credenciales de Azure Active Directory (Azure AD). Al iniciar sesión en la CLI de Azure con credenciales de Azure AD, se devuelve un token de acceso OAuth 2.0. La CLI de Azure usa automáticamente ese token para autorizar las operaciones de datos posteriores en Blob Storage o Queue Storage. Para las operaciones admitidas, ya no necesita pasar una clave de cuenta o token SAS con el comando.

Puede asignar permisos en los datos de blob y de cola a una entidad de seguridad de Azure AD a través del control de acceso basado en rol (RBAC). Para más información sobre los roles RBAC en Azure Storage, consulte [Administración de los derechos de acceso a los datos de Azure Storage con RBAC](storage-auth-aad-rbac.md).

## <a name="supported-operations"></a>Operaciones compatibles

Las extensiones de Azure Storage se admiten para las operaciones en datos de blob o de cola. Las operaciones a las que podrá llamar dependerán de los permisos que se concedan a la entidad de seguridad de Azure AD con la que inicie sesión en la CLI de Azure. Los permisos para los contenedores o colas de Azure Storage se asignan mediante RBAC. Por ejemplo, si se le asigna el rol de **lector de datos de blob**, puede ejecutar comandos de scripting que lean datos de un contenedor o cola. Si se le asigna el rol de **colaborador de datos de blob**, podrá ejecutar comandos de scripting que lean, escriban o eliminen un contenedor o cola, o los datos que estos contienen.

Para más información sobre los permisos requeridos para cada operación de Azure Storage en un contenedor o una cola, consulte el artículo de [Llamada a las operaciones de almacenamiento con tokens de OAuth](/rest/api/storageservices/authorize-with-azure-active-directory#call-storage-operations-with-oauth-tokens).  

## <a name="call-azure-cli-commands-using-azure-ad-credentials"></a>Llamada a comandos de la CLI de Azure mediante credenciales de Azure AD

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

## <a name="next-steps"></a>Pasos siguientes

- [Uso de la CLI de Azure para asignar un rol de RBAC para el acceso a datos de blobs y colas](storage-auth-aad-rbac-cli.md)
- [Autorización del acceso a datos de blobs y colas con identidades administradas para los recursos de Azure](storage-auth-aad-msi.md)
