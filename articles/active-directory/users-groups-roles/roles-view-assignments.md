---
title: Visualización de permisos de roles de administrador en el centro de administración Azure Active Directory | Microsoft Docs
description: Ahora puede ver y administrar los miembros de un rol de administrador de Azure AD en el centro de administración de AD.
services: active-directory
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 07/31/2019
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: bf45d45481712e30d40bacec9a3c4d80d1ed56b6
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/01/2019
ms.locfileid: "68707433"
---
# <a name="view-custom-role-assignments-in-azure-active-directory"></a>Visualización de asignación de roles personalizados en Azure Active Directory

En este artículo se describe cómo ver los roles personalizados que ha asignado en Azure Active Directory (Azure AD). En Azure Active Directory (Azure AD), los roles se pueden asignar en el nivel de directorio o con un ámbito de una sola aplicación. Las asignaciones de roles en el ámbito de directorio se agregan a la lista de asignaciones de roles de aplicación individuales, pero las asignaciones de roles en el ámbito de aplicación única no se agregan a la lista de asignaciones de nivel de directorio.

## <a name="view-the-assignments-of-a-role-with-directory-scope-using-the-azure-ad-portal"></a>Visualización de las asignaciones de un rol con ámbito de directorio mediante el portal de Azure AD

1. Inicie sesión en el [centro de administración de Azure AD](https://aad.portal.azure.com) con permisos de administrador de roles con privilegios o de administrador global en la organización de Azure AD.
1. Seleccione **Azure Active Directory**, elija  **Roles y administradores** y, luego, seleccione un rol para abrirlo y ver sus propiedades.
1. Seleccione **Asignaciones** para ver las asignaciones del rol.

    ![Visualización de asignaciones de roles y permisos al abrir un rol de la lista](./media/roles-view-assignments/role-assignments.png)

## <a name="view-the-assignments-of-a-role-with-directory-scope-using-azure-ad-powershell"></a>Visualización de las asignaciones de un rol con ámbito de directorio mediante Azure AD PowerShell

Puede automatizar cómo asignar roles de administración de Azure AD a los usuarios mediante Azure PowerShell. Este artículo se usa el módulo [Azure Active Directory PowerShell, versión 2](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0#directory_roles).

### <a name="prepare-powershell"></a>Preparación para PowerShell

En primer lugar, debe [descargar el módulo de Azure AD PowerShell en versión preliminar](https://www.powershellgallery.com/packages/AzureAD/).

Para instalar el módulo de Azure AD PowerShell, use los siguientes comandos:

``` PowerShell
install-module azureadpreview
import-module azureadpreview
```

Para comprobar que el módulo esté listo para usar, ejecute el siguiente comando:

``` PowerShell
get-module azuread
  ModuleType Version      Name                         ExportedCommands
  ---------- ---------    ----                         ----------------
  Binary     2.0.0.115    azuread                      {Add-AzureADAdministrati...}
```

### <a name="view-the-assignments-of-a-role"></a>Ver las asignaciones de un rol

Ejemplo de visualización de las asignaciones de un rol.

``` PowerShell
# Fetch list of all directory roles with object ID
Get-AzureADDirectoryRole

# Fetch a specific directory role by ID
$role = Get-AzureADDirectoryRole -ObjectId "5b3fe201-fa8b-4144-b6f1-875829ff7543"

# Fetch role membership for a role
Get-AzureADDirectoryRoleMember -ObjectId $role.ObjectId | Get-AzureADUser
```

## <a name="view-the-assignments-of-a-role-with-directory-scope-using-microsoft-graph-api"></a>Visualización de las asignaciones de un rol con ámbito de directorio mediante Microsoft Graph API

Solicitud HTTP para obtener una asignación de roles para una definición de rol determinada.

GET

``` HTTP
https://graph.windows.net/<tenantDomain-or-tenantId>/roleAssignments?api-version=1.61-internal&$filter=roleDefinitionId eq ‘<object-id-or-template-id-of-role-definition>’
```

Response

``` HTTP
HTTP/1.1 200 OK
{
    "id":"CtRxNqwabEKgwaOCHr2CGJIiSDKQoTVJrLE9etXyrY0-1"
    "principalId":"ab2e1023-bddc-4038-9ac1-ad4843e7e539",
    "roleDefinitionId":"3671d40a-1aac-426c-a0c1-a3821ebd8218",
    "resourceScopes":["/"]
}
```

## <a name="view-the-assignments-of-a-role-with-single-application-scope-using-the-azure-ad-portal-preview"></a>Visualización de las asignaciones de un rol con ámbito de directorio de aplicación individual mediante el portal de Azure AD (versión preliminar)

1. Inicie sesión en el [centro de administración de Azure AD](https://aad.portal.azure.com) con permisos de administrador de roles con privilegios o de administrador global en la organización de Azure AD.
1. Seleccione Azure Active Directory, elija **Registros de aplicaciones** y, luego, seleccione el registro de aplicación para ver sus propiedades. Es posible que tenga que seleccionar **Todas las aplicaciones** para ver la lista completa de los registros de aplicaciones de la organización de Azure AD.

    ![Creación o edición de registros de aplicaciones desde la página Registros de aplicaciones](./media/roles-create-custom/appreg-all-apps.png)

1. Seleccione **Roles y administradores** y elija un rol para ver sus propiedades.

    ![Visualización de las asignaciones de roles de registro de aplicaciones desde la página Registros de aplicaciones](./media/roles-view-assignments/appreg-assignments.png)

1. Seleccione **Asignaciones** para ver las asignaciones del rol.

    ![Visualización de las asignaciones de roles de registro de aplicaciones desde las propiedades de un registro de aplicaciones](./media/roles-view-assignments/appreg-assignments-2.png)

## <a name="next-steps"></a>Pasos siguientes

* No dude en compartir con nosotros en el [foro de roles administrativos de Azure AD](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=166032).
* Para obtener más información sobre los roles y la asignación de roles de administrador, consulte el artículo [Assign administrator roles](directory-assign-admin-roles.md) (Asignación de roles de administrador).
* Para conocer los permisos predeterminados de usuario, vea una [comparación de los permisos predeterminados de usuario miembro e invitado](../fundamentals/users-default-permissions.md).
