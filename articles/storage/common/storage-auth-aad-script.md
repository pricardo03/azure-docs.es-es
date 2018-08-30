---
title: Ejecute comandos de la CLI de Azure o de PowerShell bajo una identidad de Azure AD para acceder a Azure Storage (versión preliminar) | Microsoft Docs
description: La CLI de Azure y PowerShell admiten el inicio de sesión con una identidad de Azure AD para ejecutar comandos en contenedores y colas de Azure Storage y en sus datos. Se proporciona un token de acceso para la sesión y se usa para autorizar operaciones de llamada. Los permisos dependen del rol asignado a la identidad de Azure AD.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 08/29/2018
ms.author: tamram
ms.component: common
ms.openlocfilehash: abd4a3b21ede2ddbdede2ec133938d412d5d4c8d
ms.sourcegitcommit: 2b2129fa6413230cf35ac18ff386d40d1e8d0677
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/30/2018
ms.locfileid: "43248172"
---
# <a name="use-an-azure-ad-identity-to-access-azure-storage-with-cli-or-powershell-preview"></a>Uso de una identidad de Azure AD para acceder a Azure Storage con la CLI o PowerShell (versión preliminar).

Azure Storage proporciona extensiones de versión preliminar para la CLI de Azure y PowerShell que le permiten iniciar sesión y ejecutar comandos de scripting en una identidad de Azure Active Directory (Azure AD). Una identidad de Azure AD puede ser un usuario, un grupo o una entidad de servicio de aplicación, o puede ser una [identidad de servicio administrada](../../active-directory/managed-service-identity/overview.md). Puede asignar permisos para acceder a recursos de almacenamiento en la identidad de Azure AD a través del control de acceso basado en rol (RBAC). Para más información sobre los roles de RBAC en Azure Storage, consulte [Administración de los derechos de acceso a los datos de Azure Storage con RBAC (versión preliminar)](storage-auth-aad-rbac.md).

Cuando inicia sesión en la CLI de Azure o en PowerShell con una identidad de Azure AD, se devuelve un token de acceso para acceder a Azure Storage con esa identidad. Después, la CLI de Azure o PowerShell usan automáticamente ese token para autorizar operaciones en Azure Storage. Para las operaciones admitidas, ya no necesita pasar una clave de cuenta o token SAS con el comando.

> [!IMPORTANT]
> Esta versión preliminar está pensada para usos distintos del de producción. Los Acuerdos de nivel de Servicio (SLA) de producción no estarán disponibles hasta que la integración de Azure AD para Azure Storage se declare disponible con carácter general. Si la integración de Azure AD no se admite todavía para su escenario, siga usando la autorización con clave compartida o tokens de SAS en las aplicaciones. Para más información sobre la versión preliminar, consulte [Authenticate access to Azure Storage using Azure Active Directory (Preview)](storage-auth-aad.md) (Autenticación del acceso en Azure Storage mediante Azure Active Directory [versión preliminar]).
>
> Durante la versión preliminar, las asignaciones de roles de control de acceso basado en rol pueden tardar hasta cinco minutos en propagarse.
>
> La integración de Azure AD con Azure Storage requiere HTTPS para las operaciones de Azure Storage.

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

Para usar Azure PowerShell para iniciar sesión con una identidad de Azure AD:

1. Asegúrese de tener instalada la versión más reciente de PowerShellGet. Ejecute el siguiente comando para instalar la versión más reciente:
 
    ```powershell
    Install-Module -Name Azure.Storage -AllowPrerelease –AllowClobber -RequiredVersion "4.4.1-preview"
    ```

2. Desinstale las instalaciones anteriores de Azure PowerShell.
3. Instale AzureRM:

    ```powershell
    Install-Module AzureRM –Repository PSGallery –AllowClobber
    ```

4. Instale el módulo de versión preliminar:

    ```powershell
    Install-Module -Name Azure.Storage -AllowPrerelease –AllowClobber 
    ```

5. Llame al cmdlet [New-AzureStorageContext](https://docs.microsoft.com/powershell/module/azure.storage/new-azurestoragecontext) para crear un contexto e incluya el parámetro `-UseConnectedAccount`. 
6. Para llamar a un cmdlet con una identidad de Azure AD, pase el contexto al cmdlet.

En el ejemplo siguiente se muestra cómo enumerar los blobs de un contenedor desde Azure PowerShell mediante una identidad de Azure AD: 

```powershell
$ctx = New-AzureStorageContext -StorageAccountName $storageAccountName -UseConnectedAccount 
Get-AzureStorageBlob -Container $sample-container -Context $ctx 
```

## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre los roles RBAC en Azure Storage, consulte [Administración de los derechos de acceso a los datos de Azure Storage con RBAC (versión preliminar)](storage-auth-aad-rbac.md).
- Para aprender a usar Managed Service Identity con Azure Storage, consulte [Autenticación con Azure AD mediante Azure Managed Service Identity (versión preliminar)](storage-auth-aad-msi.md).
- Para información sobre la autorización de acceso a los contenedores y las colas desde las aplicaciones de almacenamiento, consulte el artículo sobre el [uso de Azure AD con aplicaciones de almacenamiento](storage-auth-aad-app.md).
- Para información adicional sobre la integración de Azure AD para los blobs y las colas de Azure, consulte la entrada de blog del equipo de Azure Storage acerca del [anuncio de la versión preliminar de Autenticación de Azure AD para Azure Storage](https://azure.microsoft.com/blog/announcing-the-preview-of-aad-authentication-for-storage/).
