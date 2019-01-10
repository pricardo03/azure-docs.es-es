---
title: Ejecute comandos de la CLI de Azure o de PowerShell bajo una identidad de Azure AD para acceder a Azure Storage (versión preliminar) | Microsoft Docs
description: La CLI de Azure y PowerShell admiten el inicio de sesión con una identidad de Azure AD para ejecutar comandos en contenedores y colas de Azure Storage y en sus datos. Se proporciona un token de acceso para la sesión y se usa para autorizar operaciones de llamada. Los permisos dependen del rol asignado a la identidad de Azure AD.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 10/15/2018
ms.author: tamram
ms.component: common
ms.openlocfilehash: b5a129c2a92c18b979a3b0c2eeea7fa19791551c
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/19/2018
ms.locfileid: "53633772"
---
# <a name="use-an-azure-ad-identity-to-access-azure-storage-with-cli-or-powershell-preview"></a>Uso de una identidad de Azure AD para acceder a Azure Storage con la CLI o PowerShell (versión preliminar).

Azure Storage proporciona extensiones de versión preliminar para la CLI de Azure y PowerShell que le permiten iniciar sesión y ejecutar comandos de scripting en una identidad de Azure Active Directory (Azure AD). La identidad de Azure AD puede ser un usuario, un grupo o una entidad de servicio de aplicación, o puede ser una [identidad administrada para los recursos de Azure](../../active-directory/managed-identities-azure-resources/overview.md). Puede asignar permisos para acceder a recursos de almacenamiento en la identidad de Azure AD a través del control de acceso basado en rol (RBAC). Para más información sobre los roles de RBAC en Azure Storage, consulte [Administración de los derechos de acceso a los datos de Azure Storage con RBAC (versión preliminar)](storage-auth-aad-rbac.md).

Cuando inicia sesión en la CLI de Azure o en PowerShell con una identidad de Azure AD, se devuelve un token de acceso para acceder a Azure Storage con esa identidad. Después, la CLI de Azure o PowerShell usan automáticamente ese token para autorizar operaciones en Azure Storage. Para las operaciones admitidas, ya no necesita pasar una clave de cuenta o token SAS con el comando.

[!INCLUDE [storage-auth-aad-note-include](../../../includes/storage-auth-aad-note-include.md)]

## <a name="supported-operations"></a>Operaciones compatibles

Se admiten las extensiones de versión preliminar para las operaciones en contenedores y colas. Las operaciones a las que podrá llamar dependerán de los permisos que se concedan a la identidad de Azure AD con la que inicie sesión en la CLI de Azure o en PowerShell. Los permisos para los contenedores o colas de Azure Storage se asignan mediante el control de acceso basado en rol (RBAC). Por ejemplo, si se asigna un rol de lector de datos a la identidad, puede ejecutar comandos de scripting que lean datos de un contenedor o cola. Si se asigna un rol de colaborador de datos a la identidad, podrá ejecutar comandos de scripting que lean, escriban o eliminen un contenedor o cola, o los datos que estos contienen. 

Para más información sobre los permisos requeridos para cada operación de Azure Storage, consulte [Permissions for calling REST operations](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-rest-operations) (Permisos para llamar a operaciones de REST).  

## <a name="call-cli-commands-with-an-azure-ad-identity"></a>Llamada a los comandos de la CLI con una identidad de Azure AD

Para instalar la extensión de la versión preliminar para la CLI de Azure:

1. Asegúrese de que tiene instalada la CLI de Azure versión 2.0.32 o posterior. Ejecute `az --version` para comprobar la versión instalada.
2. Ejecute el siguiente comando para instalar la extensión de versión preliminar: 

    ```azurecli
    az extension add -n storage-preview
    ```

La extensión de versión preliminar agrega un nuevo parámetro `--auth-mode` a los comandos admitidos:

- Establezca el parámetro `--auth-mode` en `login` para iniciar sesión mediante una identidad de Azure AD.
- Establezca el parámetro `--auth-mode` en el valor heredado `key` para intentar consultar una clave de cuenta si no se proporcionan parámetros de autenticación para la cuenta. 

Por ejemplo, para descargar un blob en la CLI de Azure mediante una identidad de Azure AD, primero ejecute `az login`, después, llame al comando con `--auth-mode` establecido en `login`:

```azurecli
az login
az storage blob download --account-name storagesamples --container sample-container --name myblob.txt --file myfile.txt --auth-mode login 
```

La variable de entorno asociada con el parámetro `--auth-mode` es `AZURE_STORAGE_AUTH_MODE`.

## <a name="call-powershell-commands-with-an-azure-ad-identity"></a>Llamada a los comandos de PowerShell con una identidad de Azure AD

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Para usar Azure PowerShell para iniciar sesión con una identidad de Azure AD:

1. Desinstale las instalaciones anteriores de Azure PowerShell:

    - Quite cualquier instalación anterior de Azure PowerShell desde Windows usando el ajuste **Aplicaciones y características** en **Configuración**.
    - Quite todos los módulos de **Azure*** desde `%Program Files%\WindowsPowerShell\Modules`.

1. Asegúrese de tener instalada la versión más reciente de PowerShellGet. Abra una ventana de Windows PowerShell y ejecute el siguiente comando para instalar la versión más reciente:
 
    ```powershell
    Install-Module PowerShellGet –Repository PSGallery –Force
    ```
1. Cierre y vuelva a abrir la ventana de PowerShell después de instalar PowerShellGet. 

1. Instale la versión más reciente de Azure PowerShell:

    ```powershell
    Install-Module Az –Repository PSGallery –AllowClobber
    ```

1. Instale uno de los módulos de versión preliminar de Azure Storage que admita Azure AD:
   
   ```powershell
   Install-Module Az.Storage -Repository PSGallery -AllowPrerelease -AllowClobber -Force
   ```
1. Cierre y vuelva a abrir la ventana de PowerShell.
1. Llame al cmdlet [New-AzStorageContext](https://docs.microsoft.com/powershell/module/azure.storage/new-AzStoragecontext) para crear un contexto e incluya el parámetro `-UseConnectedAccount`. 
1. Para llamar a un cmdlet con una identidad de Azure AD, pase el contexto recién creado al cmdlet.

En el ejemplo siguiente se muestra cómo enumerar los blobs de un contenedor desde Azure PowerShell mediante una identidad de Azure AD. No olvide reemplazar los nombres de cuenta y de contenedor de marcador de posición por los suyos propios: 

```powershell
$ctx = New-AzStorageContext -StorageAccountName storagesamples -UseConnectedAccount 
Get-AzStorageBlob -Container sample-container -Context $ctx 
```

## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre los roles RBAC en Azure Storage, consulte [Administración de los derechos de acceso a los datos de Azure Storage con RBAC (versión preliminar)](storage-auth-aad-rbac.md).
- Para más información acerca del uso de identidades administradas para Azure Resources con Azure Storage, consulte [Autenticación con Azure AD mediante Azure Managed Service Identity (versión preliminar)](storage-auth-aad-msi.md).
- Para información sobre la autorización de acceso a los contenedores y las colas desde las aplicaciones de almacenamiento, consulte el artículo sobre el [uso de Azure AD con aplicaciones de almacenamiento](storage-auth-aad-app.md).
- Para información adicional sobre la integración de Azure AD para los blobs y las colas de Azure, consulte la entrada de blog del equipo de Azure Storage acerca del [anuncio de la versión preliminar de Autenticación de Azure AD para Azure Storage](https://azure.microsoft.com/blog/announcing-the-preview-of-aad-authentication-for-storage/).
