---
title: Elevación de los privilegios de acceso para administrar todas las suscripciones y los grupos de administración de Azure | Microsoft Docs
description: Describe cómo elevar los privilegios de acceso de un administrador global para administrar todas las suscripciones y los grupos de administración en Azure Active Directory mediante Azure Portal o la API REST.
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
ms.date: 01/15/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 7552018c32078295c164023f909a604c6522c32f
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2019
ms.locfileid: "54437477"
---
# <a name="elevate-access-to-manage-all-azure-subscriptions-and-management-groups"></a>Elevación de los privilegios de acceso para administrar todas las suscripciones y los grupos de administración de Azure

En tanto que administrador global de Azure Active Directory (Azure AD), es posible que no tenga acceso a todas las suscripciones y los grupos de administración de su directorio. En este artículo se describen los medios para elevar sus derechos de acceso a todas las suscripciones y grupos de administración.

[!INCLUDE [gdpr-dsr-and-stp-note](../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="why-would-you-need-to-elevate-your-access"></a>¿Por qué necesita elevar los derechos de acceso?

Si es un administrador global, es posible que haya momentos en los que quiera hacer lo siguiente:

- Recuperar el acceso a una suscripción o grupo de administración de Azure cuando un usuario ha perdido el acceso
- Conceder acceso a otro usuario o usted mismo a una suscripción o grupo de administración de Azure
- Ver todas las suscripciones o grupos de administración de Azure de una organización
- Permitir que una aplicación de automatización (por ejemplo, una aplicación de facturación o auditoría) tenga acceso a todas las suscripciones o grupos de administración de Azure

## <a name="how-does-elevate-access-work"></a>¿Cómo funciona la elevación de los privilegios de acceso?

Azure AD y los recursos de Azure están protegidos de forma independiente entre sí. Es decir, las asignaciones de roles de Azure AD no otorgan acceso a recursos de Azure, y las asignaciones de roles de Azure no conceden acceso a Azure AD. Sin embargo, si es [administrador global](../active-directory/users-groups-roles/directory-assign-admin-roles.md#company-administrator) de Azure AD, puede asignarse a sí mismo los privilegios de acceso para todas las suscripciones a Azure y los grupos de administración de su directorio. Use esta funcionalidad si no tiene acceso a recursos de suscripción a Azure, como máquinas virtuales o cuentas de almacenamiento, y quiere usar su privilegio de administrador global para obtener acceso a esos recursos.

Al elevar los privilegios de acceso, se le asignará el rol [Administrador de acceso de usuario](built-in-roles.md#user-access-administrator) en Azure en el ámbito raíz (`/`). Esto le permite ver todos los recursos y asignar acceso en cualquier suscripción o grupo de administración en el directorio. Se pueden quitar las asignaciones de roles de administrador de acceso de usuario mediante PowerShell.

Debe quitar este acceso con privilegios elevados una vez que haya hecho los cambios necesarios en el ámbito raíz.

![Elevación de los privilegios de acceso](./media/elevate-access-global-admin/elevate-access.png)

## <a name="azure-portal"></a>Azure Portal

Siga estos pasos para elevar los privilegios de acceso de un administrador global mediante Azure Portal.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com) o en el [Centro de administración de Azure Active Directory](https://aad.portal.azure.com) como administrador global.

1. En la lista de navegación, haga clic en **Azure Active Directory** y, luego, haga clic en **Propiedades**.

   ![Propiedades de Azure AD, captura de pantalla](./media/elevate-access-global-admin/aad-properties.png)

1. En **Administración del acceso para los recursos de Azure**, establezca el botón de alternancia en **Sí**.

   ![Captura de pantalla de administración de acceso a recursos de Azure](./media/elevate-access-global-admin/aad-properties-global-admin-setting.png)

   Al establecer el botón de alternancia en **Sí**, se le asigna el rol de administrador de accesos de usuario en Azure RBAC en el ámbito raíz (/). Esto le concede permiso para asignar roles en todas las suscripciones de Azure y los grupos de administración asociados a este directorio de Azure AD. Este botón de alternancia solo está disponible para los usuarios que tienen asignado el rol de administrador global de Azure AD.

   Al establecer el botón de alternancia en **No**, se quita el rol de administrador de accesos de usuario en Azure RBAC de su cuenta de usuario. Ya no puede asignar roles en todas las suscripciones de Azure y los grupos de administración asociados a este directorio de Azure AD. Puede ver y administrar solo las suscripciones de Azure y los grupos de administración a los que se le ha concedido acceso.

1. Haga clic en **Guardar** para guardar la configuración.

   Esta configuración no es una propiedad global y se aplica solo al usuario que tiene la sesión iniciada. No puede elevar los privilegios de acceso para todos los miembros del rol de administrador global.

1. Cierre la sesión e inicie sesión de nuevo para actualizar el acceso.

    Ahora debería tener acceso a todas las suscripciones y los grupos de administración de Azure de este directorio. Observará que se le ha asignado el rol de administrador de accesos de usuario en el ámbito raíz.

   ![Asignaciones de roles de la suscripción con el ámbito raíz: captura de pantalla](./media/elevate-access-global-admin/iam-root.png)

1. Haga los cambios que tenga que hacer con privilegios de acceso elevados.

    Para obtener información acerca de cómo asignar roles, consulte [Administración del acceso mediante RBAC y Azure Portal](role-assignments-portal.md). Si usa Azure AD Privileged Identity Management (PIM), consulte [Detección de recursos de Azure que se administran en PIM](../active-directory/privileged-identity-management/pim-resource-roles-discover-resources.md) o [Asignación de roles de recursos de Azure en PIM](../active-directory/privileged-identity-management/pim-resource-roles-assign-roles.md).

1. Cuando haya terminado, establezca el botón de alternancia **Administración del acceso para los recursos de Azure** de nuevo en **No**. Puesto que se trata de una configuración que se realiza a nivel de usuario, debe haber iniciado sesión con el mismo usuario que el utilizado para elevar los privilegios de acceso.

## <a name="azure-powershell"></a>Azure PowerShell

### <a name="list-role-assignment-at-the-root-scope-"></a>Mostrar la asignación de roles en el ámbito raíz (/)

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
CanDelegate        : False
```

### <a name="remove-a-role-assignment-at-the-root-scope-"></a>Eliminación de la asignación de roles en el ámbito raíz (/)

Para quitar la asignación de roles de administrador de accesos de usuario de un usuario en el ámbito raíz (`/`), siga estos pasos.

1. Inicie sesión como un usuario que puede quitar los privilegios de acceso elevado. Puede ser el mismo usuario que se usó para elevar los privilegios de acceso u otro administrador global con privilegios de acceso elevados en el ámbito raíz.


1. Use el comando [Remove-AzureRmRoleAssignment](/powershell/module/azurerm.resources/remove-azurermroleassignment) para quitar la asignación del rol de administrador de accesos de usuario.

    ```azurepowershell
    Remove-AzureRmRoleAssignment -SignInName <username@example.com> `
      -RoleDefinitionName "User Access Administrator" -Scope "/"
    ```

## <a name="rest-api"></a>API DE REST

### <a name="elevate-access-for-a-global-administrator"></a>Elevación de los privilegios de acceso de un administrador global

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

### <a name="list-role-assignments-at-the-root-scope-"></a>Enumeración de las asignaciones de roles en el ámbito raíz (/)

Puede enumerar todas las asignaciones de roles para un usuario en el ámbito raíz (`/`).

- Llamar a [GET roleAssignments](/rest/api/authorization/roleassignments/listforscope) donde `{objectIdOfUser}` es el id. de objeto del usuario cuyas asignaciones de roles quiere recuperar.

   ```http
   GET https://management.azure.com/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$filter=principalId+eq+'{objectIdOfUser}'
   ```

### <a name="list-deny-assignments-at-the-root-scope-"></a>Enumeración de las asignaciones de denegación en el ámbito raíz (/)

Puede enumerar todas las asignaciones de denegación de un usuario en el ámbito raíz (`/`).

- Llame a GET denyAssignments, donde `{objectIdOfUser}` es el id. de objeto del usuario cuyas asignaciones de denegación desea recuperar.

   ```http
   GET https://management.azure.com/providers/Microsoft.Authorization/denyAssignments?api-version=2018-07-01-preview&$filter=gdprExportPrincipalId+eq+'{objectIdOfUser}'
   ```

### <a name="remove-elevated-access"></a>Eliminación de privilegios de acceso elevados

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

2. También tiene que mostrar la asignación de roles del administrador de directorios en el ámbito del directorio. Muestre todas las asignaciones en el ámbito del directorio para `principalId` del administrador de directorios que hizo la llamada de elevación de privilegios de acceso. Esto mostrará todas las asignaciones en el directorio para objectid.

    ```http
    GET https://management.azure.com/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$filter=principalId+eq+'{objectid}'
    ```
    
    >[!NOTE] 
    >Un administrador de directorios no debe tener muchas asignaciones; si la consulta anterior devuelve demasiadas asignaciones, puede consultar también todas las asignaciones en el nivel de ámbito de directorio y, después, filtrar los resultados: `GET https://management.azure.com/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$filter=atScope()`
        
    2. Las llamadas anteriores devuelven una lista de asignaciones de roles. Busque la asignación de roles en la que el ámbito sea `"/"`, `roleDefinitionId` termine con el id. del nombre de rol que se encuentra en el paso 1, y `principalId` coincida con el valor de ObjectId del administrador de directorios. 
    
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

- [Descripción de los distintos roles](rbac-and-directory-admin-roles.md)
- [Control de acceso basado en roles con REST](role-assignments-rest.md)
