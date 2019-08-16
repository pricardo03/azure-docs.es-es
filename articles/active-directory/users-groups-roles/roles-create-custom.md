---
title: 'Creación de una definición de roles personalizados en el control de acceso basado en rol de Azure AD: Azure Active Directory | Microsoft Docs'
description: Cree roles personalizados de Azure AD con el ámbito de recurso en recursos de Azure Active Directory.
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
ms.openlocfilehash: c1166839608c709db9aa052d6d0db5221fa15354
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2019
ms.locfileid: "68880744"
---
# <a name="create-a-custom-role-and-assign-at-resource-scope-in-azure-active-directory"></a>Creación de un rol personalizado y asignación en un ámbito de recurso en Azure Active Directory

En este artículo se describe cómo crear roles personalizados en Azure Active Directory (Azure AD). Los roles personalizados se pueden crear en la pestaña [Roles y administradores](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RolesAndAdministrators) en la página de información general sobre Azure AD. El rol se puede asignar en el ámbito de nivel de directorio o en un ámbito de recurso del registro de aplicación únicamente.

Para más información, consulte la [información general sobre roles personalizados](roles-custom-overview.md) para conocer los aspectos básicos de los roles personalizados.

## <a name="using-the-azure-ad-portal"></a>Uso del portal de Azure AD

### <a name="create-a-new-custom-role-to-grant-access-to-manage-app-registrations"></a>Creación de un rol personalizado para conceder acceso para administrar los registros de aplicaciones

1. Inicie sesión en el [centro de administración de Azure AD](https://aad.portal.azure.com) con permisos de administrador de roles con privilegios o de administrador global en la organización de Azure AD.
1. Seleccione  **Azure Active Directory** > **Roles y administradores** > **Nuevo rol personalizado**.

   ![Creación o edición de roles en la página Roles y administradores](./media/roles-create-custom/new-custom-role.png)

1. En la pestaña **Aspectos básicos**, proporcione un nombre y una descripción para el rol y, luego, haga clic en **Siguiente**.

   ![Proporcione un nombre y una descripción para un rol personalizado en la pestaña Aspectos básicos](./media/roles-create-custom/basics-tab.png)

1. En la pestaña **Permisos**, seleccione los permisos necesarios para administrar las propiedades básicas y las propiedades de credenciales de los registros de aplicaciones. Para una descripción detallada de cada permiso, consulte [Permisos y subtipos del registro de aplicaciones en Azure Active Directory](./roles-custom-available-permissions.md).
   1. En primer lugar, escriba "credenciales" en la barra de búsqueda y seleccione el permiso `microsoft.directory/applications/credentials/update`.

      ![Selección de los permisos de un rol personalizado en la pestaña Permisos](./media/roles-create-custom/permissions-tab.png)

   1. A continuación, escriba "básico" en la barra de búsqueda, seleccione el permiso `microsoft.directory/applications/basic/update` y, luego, haga clic en **Siguiente**.
1. En la pestaña **Revisar y crear**, revise los detalles y seleccione **Crear**.

El rol personalizado se mostrará en la lista de los roles disponibles para asignar.

## <a name="assign-a-role-scoped-to-a-resource"></a>Asignación de un rol con ámbito a un recurso

Al igual que los roles integrados, los roles personalizados se pueden asignar en un ámbito de toda la organización para conceder acceso sobre todos los registros de aplicaciones. Sin embargo, los roles personalizados también se pueden asignar en el ámbito de recurso. Esto le permite entregar al usuario asignado el permiso para actualizar las credenciales y las propiedades básicas de una aplicación única sin la necesidad de crear un segundo rol personalizado.

1. Si todavía no lo ha hecho, inicie sesión en el  [Centro de administración de Azure AD](https://aad.portal.azure.com) con los permisos Desarrollador de aplicaciones en la organización de Azure AD.
1. Seleccione **App registrations** (Registros de aplicaciones).
1. Seleccione el registro de aplicaciones al que se concede acceso para administrar. Es posible que tenga que seleccionar **Todas las aplicaciones** para ver la lista completa de los registros de aplicaciones de la organización de Azure AD.

    ![Selección del registro de aplicaciones como un ámbito de recurso para una asignación de roles](./media/roles-create-custom/appreg-all-apps.png)

1. En el registro de aplicaciones, seleccione **Roles y administradores**. Si todavía no crea uno, puede encontrar instrucciones sobre cómo hacerlo en el [procedimiento anterior](#create-a-new-custom-role-to-grant-access-to-manage-app-registrations).

1. Seleccione el rol para abrir la página **Asignaciones**.
1. Seleccione **Agregar asignación** para agregar un usuario. Al usuario no se le concederás permisos sobre ningún registro de aplicaciones distinto del seleccionado.

## <a name="create-a-custom-role-using-azure-ad-powershell"></a>Creación de un rol personalizado mediante Azure AD PowerShell

### <a name="prepare-powershell"></a>Preparación para PowerShell

En primer lugar, debe [descargar el módulo de Azure AD PowerShell en versión preliminar](https://www.powershellgallery.com/packages/AzureADPreview).

Para instalar el módulo de Azure AD PowerShell, use los siguientes comandos:

``` PowerShell
install-module azureadpreview
import-module azureadpreview
```

Para comprobar que el módulo esté listo para usar, ejecute el siguiente comando:

``` PowerShell
get-module azureadpreview
  ModuleType Version      Name                         ExportedCommands
  ---------- ---------    ----                         ----------------
  Binary     2.0.2.31     azuread                      {Add-AzureADAdministrati...}
```

### <a name="create-the-custom-role"></a>Creación del rol personalizado

Cree un nuevo rol mediante el siguiente script de PowerShell:

``` PowerShell
# Basic role information
$displayName = "Application Support Administrator"
$description = "Can manage basic aspects of application registrations."
$templateId = (New-Guid).Guid
 
# Set of permissions to grant
$allowedResourceAction =
@(
    "microsoft.directory/applications/basic/update",
    "microsoft.directory/applications/credentials/update"
)
$rolePermissions = @{'allowedResourceActions'= $allowedResourceAction}
 
# Create new custom admin role
$customAdmin = New-AzureADMSRoleDefinition -RolePermissions $rolePermissions -DisplayName $displayName -Description $description -TemplateId $templateId -IsEnabled $true
```

### <a name="assign-the-custom-role-using-azure-ad-powershell"></a>Asignación del rol personalizado mediante Azure AD PowerShell

Asigne el rol mediante el siguiente script de PowerShell:

``` PowerShell
# Get the user and role definition you want to link
$user = Get-AzureADUser -Filter "userPrincipalName eq 'cburl@f128.info'"
$roleDefinition = Get-AzureADMSRoleDefinition -Filter "displayName eq 'Application Support Administrator'"

# Get app registration and construct resource scope for assignment.
$appRegistration = Get-AzureADApplication -Filter "displayName eq 'f/128 Filter Photos'"
$resourceScope = '/' + $appRegistration.objectId

# Create a scoped role assignment
$roleAssignment = New-AzureADMSRoleAssignment -ResourceScope $resourceScope -RoleDefinitionId $roleDefinition.Id -PrincipalId $user.objectId
```

## <a name="create-a-custom-role-using-microsoft-graph-api"></a>Creación de un rol personalizado mediante Microsoft Graph API

1. Cree la definición de roles.

    Solicitud HTTP para crear una definición de roles personalizados.

    POST

    ``` HTTP
    https://graph.microsoft.com/beta/roleManagement/directory/roleDefinitions
    ```

    Body

    ``` HTTP
   {
       "description": "Can manage basic aspects of application registrations.",
       "displayName": "Application Support Administrator",
       "isEnabled": true,
       "templateId": "<GUID>",
       "rolePermissions": [
           {
               "allowedResourceActions": [
                   "microsoft.directory/applications/basic/update",
                   "microsoft.directory/applications/credentials/update"
               ]
           }
       ]
   }
    ```

1. Cree la asignación de roles.

    Solicitud HTTP para crear una definición de roles personalizados.

    POST

    ``` HTTP
    https://graph.microsoft.com/beta/roleManagement/directory/roleAssignments
    ```

    Body

    ``` HTTP
   {
       "principalId":"<GUID OF USER>",
       "roleDefinitionId":"<GUID OF ROLE DEFINITION>",
       "resourceScope":"/<GUID OF APPLICATION REGISTRATION>"
   }
    ```

## <a name="next-steps"></a>Pasos siguientes

- No dude en compartir con nosotros en el [foro de roles administrativos de Azure AD](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=166032).
- Para obtener más información sobre los roles y la asignación de roles de administrador, consulte el artículo [Assign administrator roles](directory-assign-admin-roles.md) (Asignación de roles de administrador).
- Para conocer los permisos predeterminados de usuario, vea una [comparación de los permisos predeterminados de usuario miembro e invitado](../fundamentals/users-default-permissions.md).
