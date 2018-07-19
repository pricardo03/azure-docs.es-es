---
title: Elevación de los privilegios de acceso de un administrador global en Azure Active Directory | Microsoft Docs
description: Describe cómo elevar los privilegios de acceso de un administrador global en Azure Active Directory mediante Azure Portal o la API de REST.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: bagovind
ms.assetid: b547c5a5-2da2-4372-9938-481cb962d2d6
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/29/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 0abf0a5971435fc3842a93e79d39468cba5c74da
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37445218"
---
# <a name="elevate-access-for-a-global-administrator-in-azure-active-directory"></a>Elevación de los privilegios de acceso de un administrador global en Azure Active Directory

Si es un [administrador global](../active-directory/users-groups-roles/directory-assign-admin-roles.md#company-administrator) en Azure Active Directory (Azure AD), es posible que haya momentos en los que quiera hacer lo siguiente:

- Recuperar el acceso a una suscripción de Azure cuando un usuario ha perdido el acceso
- Conceder acceso a otro usuario o usted mismo a una suscripción a Azure
- Ver todas las suscripciones a Azure de una organización
- Permitir que una aplicación de automatización (por ejemplo, una aplicación de facturación o auditoría) tenga acceso a todas las suscripciones de Azure

De forma predeterminada, los roles del administrador de Azure AD y el control de acceso basado en roles (RBAC) de Azure (RBAC) no abarcan Azure AD y Azure. Sin embargo, si es un administrador Global de Azure AD, puede elevar los privilegios de acceso para administrar las suscripciones a Azure y los grupos de administración. Cuando eleve los privilegios de acceso, se le concederá el rol de [administrador de accesos de usuario](built-in-roles.md#user-access-administrator) (un rol RBAC) en todas las suscripciones para un inquilino determinado. El rol de administrador de accesos de usuario permite conceder a otros usuarios acceso a recursos de Azure en el ámbito raíz (`/`).

Esta elevación debe ser temporal y solo debe realizarse cuando sea necesario.

[!INCLUDE [gdpr-dsr-and-stp-note](../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="elevate-access-for-a-global-administrator-using-the-azure-portal"></a>Elevación de los privilegios de acceso de un administrador global mediante Azure Portal

1. Inicie sesión en el [Azure Portal](https://portal.azure.com) o en el [Centro de administración de Azure Active Directory](https://aad.portal.azure.com).

1. En la lista de navegación, haga clic en **Azure Active Directory** y, luego, haga clic en **Propiedades**.

   ![Propiedades de Azure AD, captura de pantalla](./media/elevate-access-global-admin/aad-properties.png)

1. En **El administrador global puede administrar las suscripciones a Azure y los grupos de administración**, seleccione **Sí**.

   ![El administrador global puede gestionar las suscripciones s Azure y los grupos de administración (captura de pantalla)](./media/elevate-access-global-admin/aad-properties-global-admin-setting.png)

   Si selecciona **Sí**, se agrega la cuenta de administrador global (usuario conectado actualmente) al rol de administrador de accesos de usuario en RBAC de Azure en el ámbito raíz (`/`), que le concede acceso para ver e informar sobre todas las suscripciones a Azure asociadas al inquilino de Azure AD.

   Si selecciona **No**, se quita la cuenta de administrador global (usuario conectado actualmente) del rol de administrador de accesos de usuario en Azure RBAC. No se pueden ver todas las suscripciones a Azure asociadas con el inquilino de Azure AD. Solo se pueden ver y administrar las suscripciones a Azure a las que se le ha concedido acceso.

1. Haga clic en **Guardar** para guardar la configuración.

   Esta configuración no es una propiedad global y se aplica solo al usuario que tiene la sesión iniciada.

1. Realice las tareas que debe realizar al tener privilegios elevados de acceso. Cuando haya terminado, seleccione **No**.

## <a name="list-role-assignment-at-the-root-scope--using-powershell"></a>Mostrar la asignación de roles en el ámbito raíz (/) mediante PowerShell

Para mostrar la asignación de roles de administrador de accesos de usuario de un usuario en el ámbito raíz (`/`), use el comando [Get-AzureRmRoleAssignment](/powershell/module/azurerm.resources/get-azurermroleassignment).

```azurepowershell
Get-AzureRmRoleAssignment | where {$_.RoleDefinitionName -eq "User Access Administrator" `
  -and $_.SignInName -eq "<username@example.com>" -and $_.Scope -eq "/"}
```

```Example
RoleAssignmentId   : /providers/Microsoft.Authorization/roleAssignments/098d572e-c1e5-43ee-84ce-8dc459c7e1f0
Scope              : /
DisplayName        : username
SignInName         : username@example.com
RoleDefinitionName : User Access Administrator
RoleDefinitionId   : 18d7d88d-d35e-4fb5-a5c3-7773c20a72d9
ObjectId           : d65fd0e9-c185-472c-8f26-1dafa01f72cc
ObjectType         : User
```

## <a name="remove-a-role-assignment-at-the-root-scope--using-powershell"></a>Quitar la asignación de un rol en el ámbito raíz (/) mediante PowerShell

Para quitar la asignación de roles de administrador de acceso de usuario de un usuario en el ámbito raíz (`/`), use el comando [Remove-AzureRmRoleAssignment](/powershell/module/azurerm.resources/remove-azurermroleassignment).

```azurepowershell
Remove-AzureRmRoleAssignment -SignInName <username@example.com> `
  -RoleDefinitionName "User Access Administrator" -Scope "/"
```

## <a name="elevate-access-for-a-global-administrator-using-the-rest-api"></a>Elevación de los privilegios de acceso de un administrador global mediante la API de REST

Utilice los siguientes pasos básicos para elevar los privilegios de acceso de un administrador global mediante la API de REST.

1. Mediante REST, llame a `elevateAccess`. Entonces, se le concede el rol de administrador de accesos de usuario en el ámbito raíz (`/`).

   ```http
   POST https://management.azure.com/providers/Microsoft.Authorization/elevateAccess?api-version=2016-07-01
   ```

1. Cree una [asignación de roles](/rest/api/authorization/roleassignments) para asignar cualquier rol en cualquier ámbito. En el ejemplo siguiente, se muestran las propiedades para asignar el rol {roleDefinitionID} en el ámbito (`/`):

   ```json
   { 
     "properties": {
       "roleDefinitionId": "providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionID}",
       "principalId": "{objectID}",
       "scope": "/"
     },
     "id": "providers/Microsoft.Authorization/roleAssignments/64736CA0-56D7-4A94-A551-973C2FE7888B",
     "type": "Microsoft.Authorization/roleAssignments",
     "name": "64736CA0-56D7-4A94-A551-973C2FE7888B"
   }
   ```

1. Mientras sea administrador de acceso de usuario, también podrá quitar asignaciones de roles en el ámbito raíz (`/`).

1. Quite los privilegios de administrador de acceso de usuario hasta que los vuelva a necesitar.

## <a name="list-role-assignments-at-the-root-scope--using-the-rest-api"></a>Mostrar las asignaciones de roles en el ámbito raíz (/) mediante la API REST

Puede enumerar todas las asignaciones de roles para un usuario en el ámbito raíz (`/`).

- Llamar a [GET roleAssignments](/rest/api/authorization/roleassignments/listforscope) donde `{objectIdOfUser}` es el id. de objeto del usuario cuyas asignaciones de roles quiere recuperar.

   ```http
   GET https://management.azure.com/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$filter=principalId+eq+'{objectIdOfUser}'
   ```

## <a name="remove-elevated-access-using-the-rest-api"></a>Quitar el acceso con privilegios elevados mediante la API REST

Cuando llama a `elevateAccess`, crea una asignación de roles para sí mismo, por lo que, para revocar esos privilegios, debe quitar la asignación.

1. Llame al comando [GET-roleDefinitions](/rest/api/authorization/roledefinitions/get), donde `roleName` es el administrador de accesos de usuario, para determinar el id. del nombre del rol de administrador de accesos de usuario.

    ```http
    GET https://management.azure.com/providers/Microsoft.Authorization/roleDefinitions?api-version=2015-07-01&$filter=roleName+eq+'User Access Administrator'
    ```

    ```json
    {
      "value": [
        {
          "properties": {
        "roleName": "User Access Administrator",
        "type": "BuiltInRole",
        "description": "Lets you manage user access to Azure resources.",
        "assignableScopes": [
          "/"
        ],
        "permissions": [
          {
            "actions": [
              "*/read",
              "Microsoft.Authorization/*",
              "Microsoft.Support/*"
            ],
            "notActions": []
          }
        ],
        "createdOn": "0001-01-01T08:00:00.0000000Z",
        "updatedOn": "2016-05-31T23:14:04.6964687Z",
        "createdBy": null,
        "updatedBy": null
          },
          "id": "/providers/Microsoft.Authorization/roleDefinitions/18d7d88d-d35e-4fb5-a5c3-7773c20a72d9",
          "type": "Microsoft.Authorization/roleDefinitions",
          "name": "18d7d88d-d35e-4fb5-a5c3-7773c20a72d9"
        }
      ],
      "nextLink": null
    }
    ```

    Guarde el id. del parámetro `name`; en este caso: `18d7d88d-d35e-4fb5-a5c3-7773c20a72d9`.

2. También necesita mostrar la asignación de roles del administrador de inquilinos en el ámbito del inquilino. Mostrar todas las asignaciones en el ámbito del inquilino para `principalId` del administrador de inquilinos, que realizó la llamada de elevación de privilegios de acceso. Esto mostrará todas las asignaciones en el inquilino para ObjectID.

    ```http
    GET https://management.azure.com/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$filter=principalId+eq+'{objectid}'
    ```
    
    >[!NOTE] 
    >Un administrador de inquilinos no debe tener muchas asignaciones; si la consulta anterior devuelve demasiadas asignaciones, puede consultar también todas las asignaciones en el nivel de ámbito de inquilino y, después, filtrar los resultados: `GET https://management.azure.com/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$filter=atScope()`
        
    2. Las llamadas anteriores devuelven una lista de asignaciones de roles. Busque la asignación de roles en la que el ámbito sea `"/"`, `roleDefinitionId`termine con el id. del nombre de rol que se encuentra en el paso 1, y `principalId` coincida con el valor de ObjectId del administrador de inquilinos. 
    
    Ejemplo de asignación de rol:

        ```json
        {
          "value": [
            {
              "properties": {
                "roleDefinitionId": "/providers/Microsoft.Authorization/roleDefinitions/18d7d88d-d35e-4fb5-a5c3-7773c20a72d9",
                "principalId": "{objectID}",
                "scope": "/",
                "createdOn": "2016-08-17T19:21:16.3422480Z",
                "updatedOn": "2016-08-17T19:21:16.3422480Z",
                "createdBy": "93ce6722-3638-4222-b582-78b75c5c6d65",
                "updatedBy": "93ce6722-3638-4222-b582-78b75c5c6d65"
              },
              "id": "/providers/Microsoft.Authorization/roleAssignments/e7dd75bc-06f6-4e71-9014-ee96a929d099",
              "type": "Microsoft.Authorization/roleAssignments",
              "name": "e7dd75bc-06f6-4e71-9014-ee96a929d099"
            }
          ],
          "nextLink": null
        }
        ```
        
    Una vez más, guarde el id. del parámetro `name`; en este caso: e7dd75bc-06f6-4e71-9014-ee96a929d099.

    3. Por último, utilice el id. de asignación de roles para quitar la asignación agregada por `elevateAccess`:

    ```http
    DELETE https://management.azure.com/providers/Microsoft.Authorization/roleAssignments/e7dd75bc-06f6-4e71-9014-ee96a929d099?api-version=2015-07-01
    ```

## <a name="next-steps"></a>Pasos siguientes

- [Control de acceso basado en roles con REST](role-assignments-rest.md)
- [Administración del acceso a recursos de Azure con Privileged Identity Management](pim-azure-resource.md)
- [Administración el acceso a la administración de Azure con acceso condicional](conditional-access-azure-management.md)
