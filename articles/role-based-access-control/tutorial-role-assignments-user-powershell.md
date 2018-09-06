---
title: 'Tutorial: Concesión de acceso a un usuario mediante RBAC y Azure PowerShell | Microsoft Docs'
description: Utilice el control de acceso basado en rol (RBAC) para conceder a un usuario acceso para ver todos los elementos de una suscripción y administrar todo el contenido de un grupo de recursos mediante Azure PowerShell.
services: active-directory
documentationCenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: role-based-access-control
ms.devlang: ''
ms.topic: tutorial
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 06/11/2018
ms.author: rolyon
ms.openlocfilehash: cac585b36c3b5969a18c941215b623443850cd4c
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/30/2018
ms.locfileid: "43301735"
---
# <a name="tutorial-grant-access-for-a-user-using-rbac-and-azure-powershell"></a>Tutorial: Concesión de acceso a un usuario mediante RBAC y Azure PowerShell

[Control de acceso basado en rol (RBAC)](overview.md) es la forma en que se administra el acceso a los recursos en Azure. En este tutorial se concede a un usuario acceso para ver todos los elementos de una suscripción y administrar todo el contenido de un grupo de recursos mediante Azure PowerShell.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Conceder acceso a un usuario a distintos ámbitos
> * Lista de acceso
> * Quitar acceso

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial, necesitará:

- Permisos para crear usuarios en Azure Active Directory (o tener un usuario existente)
- [Azure Cloud Shell](/azure/cloud-shell/quickstart-powershell)

## <a name="role-assignments"></a>Asignaciones de roles

En RBAC, para conceder acceso es preciso crear una asignación de roles. Una asignación de roles consta de tres elementos: entidad de seguridad, definición de rol y ámbito. Estas son las dos asignaciones de roles que llevará a cabo en este tutorial:

| Entidad de seguridad | Definición de roles | Ámbito |
| --- | --- | --- |
| Usuario<br>(Usuario de tutorial de RBAC) | [Lector](built-in-roles.md#reader) | Subscription |
| Usuario<br>(Usuario de tutorial de RBAC)| [Colaborador](built-in-roles.md#contributor) | Grupos de recursos<br>(rbac-tutorial-resource-group) |

   ![Asignaciones de roles de un usuario](./media/tutorial-role-assignments-user-powershell/rbac-role-assignments-user.png)

## <a name="create-a-user"></a>Creación de un usuario

Para asignar un rol, se necesita un usuario, un grupo o una entidad de servicio. Si aún no tiene un usuario, puede crearlo.

1. En Azure Cloud Shell, cree una contraseña que cumpla con los requisitos de complejidad de contraseña.

    ```azurepowershell
    $PasswordProfile = New-Object -TypeName Microsoft.Open.AzureAD.Model.PasswordProfile
    $PasswordProfile.Password = "Password"
    ```

1. Cree un nuevo usuario para el dominio mediante el comando [New-AzureADUser](/powershell/module/azuread/new-azureaduser).

    ```azurepowershell
    New-AzureADUser -DisplayName "RBAC Tutorial User" -PasswordProfile $PasswordProfile `
      -UserPrincipalName "rbacuser@example.com" -AccountEnabled $true -MailNickName "rbacuser"
    ```
    
    ```Example
    ObjectId                             DisplayName        UserPrincipalName    UserType
    --------                             -----------        -----------------    --------
    11111111-1111-1111-1111-111111111111 RBAC Tutorial User rbacuser@example.com Member
    ```

## <a name="create-a-resource-group"></a>Crear un grupo de recursos

Use un grupo de recursos para mostrar cómo asignar un rol a un ámbito de un grupo de recursos.

1. Obtenga una lista de ubicaciones de la región mediante el comando [Get AzureRmLocation](/powershell/module/azurerm.resources/get-azurermlocation).

   ```azurepowershell
   Get-AzureRmLocation | select Location
   ```

1. Seleccione una ubicación próxima y asígnela a una variable.

   ```azurepowershell
   $location = "westus"
   ```

1. Cree un nuevo grupo de recursos mediante el comando [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).

   ```azurepowershell
   New-AzureRmResourceGroup -Name "rbac-tutorial-resource-group" -Location $location
   ```

   ```Example
   ResourceGroupName : rbac-tutorial-resource-group
   Location          : westus
   ProvisioningState : Succeeded
   Tags              :
   ResourceId        : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/rbac-tutorial-resource-group
   ```

## <a name="grant-access"></a>Conceder acceso

Para conceder acceso al usuario, utilice el comando [New-AzureRmRoleAssignment](/powershell/module/azurerm.resources/new-azurermroleassignment) para asignar un rol. Debe especificar la entidad de seguridad, la definición del rol y el ámbito.

1. Obtenga el identificador de la suscripción mediante el comando [Get AzureRmSubscription](/powershell/module/azurerm.profile/get-azurermsubscription).

    ```azurepowershell
    Get-AzureRmSubscription
    ```

    ```Example
    Name     : Pay-As-You-Go
    Id       : 00000000-0000-0000-0000-000000000000
    TenantId : 22222222-2222-2222-2222-222222222222
    State    : Enabled
    ```

1. Guarde el ámbito de la suscripción en una variable.

    ```azurepowershell
    $subScope = "/subscriptions/00000000-0000-0000-0000-000000000000"
    ```

1. Asigne el rol de [lector](built-in-roles.md#reader) al usuario en el ámbito de la suscripción.

    ```azurepowershell
    New-AzureRmRoleAssignment -SignInName rbacuser@example.com `
      -RoleDefinitionName "Reader" `
      -Scope $subScope
    ```

    ```Example
    RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Authorization/roleAssignments/44444444-4444-4444-4444-444444444444
    Scope              : /subscriptions/00000000-0000-0000-0000-000000000000
    DisplayName        : RBAC Tutorial User
    SignInName         : rbacuser@example.com
    RoleDefinitionName : Reader
    RoleDefinitionId   : acdd72a7-3385-48ef-bd42-f606fba81ae7
    ObjectId           : 11111111-1111-1111-1111-111111111111
    ObjectType         : User
    CanDelegate        : False
    ```

1. Asigne el rol de [colaborador](built-in-roles.md#contributor) al usuario en el ámbito del grupo de recursos.

    ```azurepowershell
    New-AzureRmRoleAssignment -SignInName rbacuser@example.com `
      -RoleDefinitionName "Contributor" `
      -ResourceGroupName "rbac-tutorial-resource-group"
    ```

    ```Example
    RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/rbac-tutorial-resource-group/providers/Microsoft.Authorization/roleAssignments/33333333-3333-3333-3333-333333333333
    Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/rbac-tutorial-resource-group
    DisplayName        : RBAC Tutorial User
    SignInName         : rbacuser@example.com
    RoleDefinitionName : Contributor
    RoleDefinitionId   : b24988ac-6180-42a0-ab88-20f7382dd24c
    ObjectId           : 11111111-1111-1111-1111-111111111111
    ObjectType         : User
    CanDelegate        : False
    ```

## <a name="list-access"></a>Lista de acceso

1. Para comprobar el acceso de la suscripción, utilice el comando [Get-AzureRmRoleAssignment](/powershell/module/azurerm.resources/get-azurermroleassignment) para enumerar las asignaciones de roles.

    ```azurepowershell
    Get-AzureRmRoleAssignment -SignInName rbacuser@example.com -Scope $subScope
    ```

    ```Example
    RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Authorization/roleAssignments/22222222-2222-2222-2222-222222222222
    Scope              : /subscriptions/00000000-0000-0000-0000-000000000000
    DisplayName        : RBAC Tutorial User
    SignInName         : rbacuser@example.com
    RoleDefinitionName : Reader
    RoleDefinitionId   : acdd72a7-3385-48ef-bd42-f606fba81ae7
    ObjectId           : 11111111-1111-1111-1111-111111111111
    ObjectType         : User
    CanDelegate        : False
    ```

    En la salida, puede ver que se ha asignado que el rol de lector se ha asignado al usuario del tutorial de RBAC en el ámbito de la suscripción.

1. Para comprobar el acceso del grupo de recursos, utilice el comando [Get-AzureRmRoleAssignment](/powershell/module/azurerm.resources/get-azurermroleassignment) para enumerar las asignaciones de roles.

    ```azurepowershell
    Get-AzureRmRoleAssignment -SignInName rbacuser@example.com -ResourceGroupName "rbac-tutorial-resource-group"
    ```

    ```Example
    RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/rbac-tutorial-resource-group/providers/Microsoft.Authorization/roleAssignments/33333333-3333-3333-3333-333333333333
    Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/rbac-tutorial-resource-group
    DisplayName        : RBAC Tutorial User
    SignInName         : rbacuser@example.com
    RoleDefinitionName : Contributor
    RoleDefinitionId   : b24988ac-6180-42a0-ab88-20f7382dd24c
    ObjectId           : 11111111-1111-1111-1111-111111111111
    ObjectType         : User
    CanDelegate        : False
    
    RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Authorization/roleAssignments/22222222-2222-2222-2222-222222222222
    Scope              : /subscriptions/00000000-0000-0000-0000-000000000000
    DisplayName        : RBAC Tutorial User
    SignInName         : rbacuser@example.com
    RoleDefinitionName : Reader
    RoleDefinitionId   : acdd72a7-3385-48ef-bd42-f606fba81ae7
    ObjectId           : 11111111-1111-1111-1111-111111111111
    ObjectType         : User
    CanDelegate        : False
    ```

    En la salida, puede ver que ambos roles, colaborador y lector, se han asignado al usuario del tutorial de RBAC. El rol de colaborador está en el ámbito de rbac-tutorial-resource-group, mientras que el rol de lector se hereda en el ámbito de la suscripción.

## <a name="optional-list-access-using-the-azure-portal"></a>(Opcional) Acceso a la lista mediante Azure Portal

1. Para ver el aspecto de las asignaciones de roles en Azure Portal, vea la hoja **Control de acceso (IAM)** de la suscripción.

    ![Asignaciones de roles para un usuario a un ámbito de la suscripción](./media/tutorial-role-assignments-user-powershell/role-assignments-subscription-user.png)

1. Vea la hoja **Control de acceso (IAM)** de un grupo de recursos.

    ![Asignaciones de roles para un usuario en el ámbito del grupo de recursos](./media/tutorial-role-assignments-user-powershell/role-assignments-resource-group-user.png)

## <a name="remove-access"></a>Quitar acceso

Para quitar el acceso de usuarios, grupos y aplicaciones, use [Remove-AzureRmRoleAssignment](/powershell/module/azurerm.resources/remove-azurermroleassignment) para quitar la asignación de roles.

1. Utilice el siguiente comando para quitar la asignación del rol de colaborador al usuario del ámbito del grupo de recursos.

    ```azurepowershell
    Remove-AzureRmRoleAssignment -SignInName rbacuser@example.com `
      -RoleDefinitionName "Contributor" `
      -ResourceGroupName "rbac-tutorial-resource-group"
    ```

1. Utilice el siguiente comando para quitar la asignación del rol de colaborador al usuario del ámbito de la suscripción.

    ```azurepowershell
    Remove-AzureRmRoleAssignment -SignInName rbacuser@example.com `
      -RoleDefinitionName "Reader" `
      -Scope $subScope
    ```

## <a name="clean-up-resources"></a>Limpieza de recursos

Para limpiar los recursos creados por este tutorial puede eliminar tanto el usuario como el grupo de recursos.

1. Eliminar el grupo de recursos mediante el comando [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup).

    ```azurepowershell
    Remove-AzureRmResourceGroup -Name "rbac-tutorial-resource-group"
    ```

    ```Example
    Confirm
    Are you sure you want to remove resource group 'rbac-tutorial-resource-group'
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
    ```
    
1. Cuando se le pida confirmación, escriba **Y**. Tardará unos segundos en eliminarse.

1. Elimine el usuario mediante el comando [Remove-AzureADUser](/powershell/module/azuread/remove-azureaduser).

    ```azurepowershell
    Remove-AzureADUser -ObjectId "rbacuser@example.com"
    ```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Administración de acceso mediante RBAC y PowerShell](role-assignments-powershell.md)
