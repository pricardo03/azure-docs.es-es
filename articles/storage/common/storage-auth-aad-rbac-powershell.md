---
title: Usar Azure PowerShell para administrar los derechos de acceso de Azure AD a los datos de blob y cola con RBAC - almacenamiento de Azure
description: Uso de Azure PowerShell para asignar el acceso a los contenedores y las colas de control de acceso basado en roles (RBAC). Almacenamiento de Azure admite los roles RBAC integrados y personalizados para la autenticación a través de Azure AD.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 03/21/2019
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: f14c6625a36356a6882e1596db13c1749a9a292a
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2019
ms.locfileid: "58450036"
---
# <a name="grant-access-to-azure-blob-and-queue-data-with-rbac-using-powershell"></a>Conceder acceso a datos de blob y cola de Azure con RBAC con PowerShell

Azure Active Directory (Azure AD) autoriza derechos de acceso a los recursos protegidos mediante el [control de acceso basado en rol (RBAC)](../../role-based-access-control/overview.md). Azure Storage define un conjunto de roles de RBAC integrados que abarcan conjuntos comunes de permisos utilizados para acceder a los contenedores o las colas. 

Cuando un rol de RBAC se asigna a una entidad de seguridad de Azure AD, Azure concede acceso a esos recursos para esa entidad de seguridad. El acceso se puede limitar al nivel de la suscripción, el grupo de recursos, la cuenta de almacenamiento o un contenedor individual o una cola. Una entidad de seguridad de Azure AD puede ser un usuario, un grupo, una entidad de servicio de aplicación, o un [la identidad de los recursos de Azure administrada](../../active-directory/managed-identities-azure-resources/overview.md).

En este artículo se describe cómo usar Azure PowerShell para enumerar los roles RBAC integrados y asignarlos a los usuarios. Para obtener más información sobre cómo usar Azure PowerShell, consulte [información general de Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="rbac-roles-for-blobs-and-queues"></a>Roles RBAC para blobs y colas

[!INCLUDE [storage-auth-rbac-roles-include](../../../includes/storage-auth-rbac-roles-include.md)]

## <a name="determine-resource-scope"></a>Determinar el ámbito del recurso 

[!INCLUDE [storage-auth-resource-scope-include](../../../includes/storage-auth-resource-scope-include.md)]

## <a name="list-available-rbac-roles"></a>Lista de roles RBAC disponible

Para obtener una lista de roles RBAC integrados disponibles con Azure PowerShell, use el [Get AzRoleDefinition](/powershell/module/az.resources/get-azroledefinition) comando:

```powershell
Get-AzRoleDefinition | FT Name, Description
```

Verá los roles integrados de datos de Azure Storage en la lista, junto con otros roles integrados de Azure:

```Example
Storage Blob Data Contributor             Allows for read, write and delete access to Azure Storage blob containers and data
Storage Blob Data Owner                   Allows for full access to Azure Storage blob containers and data, including assigning POSIX access control.
Storage Blob Data Reader                  Allows for read access to Azure Storage blob containers and data
Storage Queue Data Contributor            Allows for read, write, and delete access to Azure Storage queues and queue messages
Storage Queue Data Message Processor      Allows for peek, receive, and delete access to Azure Storage queue messages
Storage Queue Data Message Sender         Allows for sending of Azure Storage queue messages
Storage Queue Data Reader                 Allows for read access to Azure Storage queues and queue messages
```

## <a name="assign-an-rbac-role-to-a-user"></a>Asignar un rol de RBAC a un usuario

Para asignar un rol de RBAC a un usuario, use el [New AzRoleAssignment](/powershell/module/az.resources/new-azroleassignment) comando. El formato del comando puede diferir en función del ámbito de la asignación. Los ejemplos siguientes muestran cómo asignar un rol a un usuario en varios ámbitos.

### <a name="container-scope"></a>Ámbito del contenedor

Para asignar un rol con ámbito en un contenedor, especifique una cadena que contiene el ámbito del contenedor para el `--scope` parámetro. El ámbito de un contenedor está en el formulario:

```
/subscriptions/<subscription>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/blobServices/default/containers/<container-name>
```

En el ejemplo siguiente se asigna el **colaborador de datos de Blob de almacenamiento** rol a un ámbito de usuario, en un contenedor denominado *contenedor-ejemplo*. No olvide reemplazar los valores de ejemplo y los valores de marcador de posición entre corchetes por los suyos propios: 

```powershell
New-AzRoleAssignment -SignInName <email> `
    -RoleDefinitionName "Storage Blob Data Contributor" `
    -Scope  "/subscriptions/<subscription>/resourceGroups/sample-resource-group/providers/Microsoft.Storage/storageAccounts/<storage-account>/blobServices/default/containers/sample-container"
```

### <a name="queue-scope"></a>Ámbito de cola

Para asignar un rol con ámbito en una cola, especifique una cadena que contiene el ámbito de la cola para el `--scope` parámetro. El ámbito de una cola tiene el formato:

```
/subscriptions/<subscription>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/queueServices/default/queues/<queue-name>
```

En el ejemplo siguiente se asigna el **colaborador de datos de almacenamiento cola** rol a un usuario, el ámbito a una cola denominada *ejemplo cola*. No olvide reemplazar los valores de ejemplo y los valores de marcador de posición entre corchetes por los suyos propios: 

```powershell
New-AzRoleAssignment -SignInName <email> `
    -RoleDefinitionName "Storage Queue Data Contributor" `
    -Scope  "/subscriptions/<subscription>/resourceGroups/sample-resource-group/providers/Microsoft.Storage/storageAccounts/<storage-account>/queueServices/default/queues/sample-queue"
```

### <a name="storage-account-scope"></a>Ámbito de la cuenta de almacenamiento

Para asignar un rol con ámbito en la cuenta de almacenamiento, especifique el ámbito del recurso de la cuenta de almacenamiento para el `--scope` parámetro. El ámbito de una cuenta de almacenamiento está en el formulario:

```
/subscriptions/<subscription>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>
```

El ejemplo siguiente se muestra cómo el ámbito de la **lector de datos de Blob de almacenamiento** rol a un usuario en el nivel de la cuenta de almacenamiento. No olvide reemplazar los valores de ejemplo con sus propios valores: 

```powershell
New-AzRoleAssignment -SignInName <email> `
    -RoleDefinitionName "Storage Blob Data Reader" `
    -Scope  "/subscriptions/<subscription>/resourceGroups/sample-resource-group/providers/Microsoft.Storage/storageAccounts/<storage-account>"
```

### <a name="resource-group-scope"></a>Ámbito del grupo de recursos

Para asignar un rol con ámbito en el grupo de recursos, especifique el nombre del grupo de recursos o el identificador para el `--resource-group` parámetro. En el ejemplo siguiente se asigna el **lector de datos de almacenamiento cola** rol a un usuario en el nivel del grupo de recursos. No olvide reemplazar los valores de ejemplo y los valores de marcador de posición entre corchetes por los suyos propios: 

```powershell
New-AzRoleAssignment -SignInName <email> `
    -RoleDefinitionName "Storage Queue Data Reader" `
    -ResourceGroupName "sample-resource-group"
```

### <a name="subscription-scope"></a>Ámbito de la suscripción

Para asignar un rol con ámbito en la suscripción, especifique el ámbito para la suscripción para el `--scope` parámetro. El ámbito de una suscripción está en el formulario:

```
/subscriptions/<subscription>
```

El ejemplo siguiente muestra cómo asignar el **lector de datos de almacenamiento Blob** rol a un usuario en el nivel de la cuenta de almacenamiento. No olvide reemplazar los valores de ejemplo con sus propios valores: 

```powershell
New-AzRoleAssignment -SignInName <email> `
    -RoleDefinitionName "Storage Blob Data Reader" `
    -Scope  "/subscriptions/<subscription>"
```

## <a name="next-steps"></a>Pasos siguientes

- [Administración del acceso a los recursos de Azure mediante RBAC y Azure PowerShell](../../role-based-access-control/role-assignments-powershell.md)
- [Conceder acceso a datos de blob y cola de Azure con RBAC, mediante la CLI de Azure](storage-auth-aad-rbac-cli.md)
- [Conceder acceso a datos blob y cola de Azure con RBAC en Azure portal](storage-auth-aad-rbac-portal.md)
