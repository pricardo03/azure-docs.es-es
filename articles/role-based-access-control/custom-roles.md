---
title: Roles personalizados en Azure | Microsoft Docs
description: Obtenga información sobre cómo definir roles personalizados con el control de acceso basado en roles (RBAC) para la administración de acceso específico de recursos en Azure.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: e4206ea9-52c3-47ee-af29-f6eef7566fa5
ms.service: role-based-access-control
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/12/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 074c305cb15bc1fb25dfa5cfc52dcce53b661a7e
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/22/2018
ms.locfileid: "36324190"
---
# <a name="custom-roles-in-azure"></a>Roles personalizados en Azure

Si los [roles integrados](built-in-roles.md) no cumplen las necesidades específicas de su organización, puede crear sus propios roles personalizados. Igual que los roles integrados, puede asignar roles personalizados a usuarios, grupos y entidades de servicio en los ámbitos de suscripción, grupo de recursos y recurso. Los roles personalizados se almacenan en un inquilino de Azure Active Directory (Azure AD) y se pueden compartir entre suscripciones. Cada inquilino puede tener hasta 2000 roles personalizados. Se pueden crear roles personalizados con Azure PowerShell, la CLI de Azure o la API REST.

## <a name="custom-role-example"></a>Ejemplo de rol personalizado

A continuación, se muestra un rol personalizado para supervisar y reiniciar máquinas virtuales, tal como se muestra mediante Azure PowerShell:

```json
{
  "Name":  "Virtual Machine Operator",
  "Id":  "88888888-8888-8888-8888-888888888888",
  "IsCustom":  true,
  "Description":  "Can monitor and restart virtual machines.",
  "Actions":  [
                  "Microsoft.Storage/*/read",
                  "Microsoft.Network/*/read",
                  "Microsoft.Compute/*/read",
                  "Microsoft.Compute/virtualMachines/start/action",
                  "Microsoft.Compute/virtualMachines/restart/action",
                  "Microsoft.Authorization/*/read",
                  "Microsoft.Resources/subscriptions/resourceGroups/read",
                  "Microsoft.Insights/alertRules/*",
                  "Microsoft.Insights/diagnosticSettings/*",
                  "Microsoft.Support/*"
  ],
  "NotActions":  [

                 ],
  "DataActions":  [

                  ],
  "NotDataActions":  [

                     ],
  "AssignableScopes":  [
                           "/subscriptions/{subscriptionId1}",
                           "/subscriptions/{subscriptionId2}",
                           "/subscriptions/{subscriptionId3}"
                       ]
}
```

Después de crear un rol personalizado, aparece en Azure Portal con un icono de recurso naranja.

![Icono de rol personalizado](./media/custom-roles/roles-custom-role-icon.png)

## <a name="steps-to-create-a-custom-role"></a>Pasos para crear un rol personalizado

1. Determinar los permisos que necesita

    Cuando crea un rol personalizado, debe conocer las operaciones del proveedor de recursos que están disponibles para definir los permisos. Para ver la lista de las operaciones, puede usar los comandos [Get-AzureRMProviderOperation](/powershell/module/azurerm.resources/get-azurermprovideroperation) o [az provider operation list](/cli/azure/provider/operation#az-provider-operation-list).
    Para especificar los permisos para el rol personalizado, agregue las operaciones a las propiedades `actions` o `notActions` de la [definición del rol](role-definitions.md). Si dispone de operaciones de datos, agréguelas a las propiedades `dataActions` o `notDataActions`.

2. Creación del rol personalizado

    Puede usar Azure PowerShell o la CLI de Azure para crear el rol personalizado. Por lo general, empiece con un rol integrado existente y, a continuación, modifíquelo según sus necesidades. A continuación, use los comandos [New-AzureRmRoleDefinition](/powershell/module/azurerm.resources/new-azurermroledefinition) o [az role definition create](/cli/azure/role/definition#az-role-definition-create) y para crear el rol personalizado. Para crear un rol personalizado, debe tener el permiso `Microsoft.Authorization/roleDefinitions/write` en todos los `assignableScopes`, como [Propietario](built-in-roles.md#owner) o [Administrador de acceso de usuario](built-in-roles.md#user-access-administrator).

3. Probar el rol personalizado

    Una vez que tenga el rol personalizado, tiene que probarlo para comprobar que funciona según lo esperado. Si es necesario realizar ajustes, puede actualizar el rol personalizado.

## <a name="custom-role-properties"></a>Propiedades del rol personalizado

Un rol personalizado tiene las siguientes propiedades.

| Propiedad | Obligatorio | Escriba | DESCRIPCIÓN |
| --- | --- | --- | --- |
| `Name` | Sí | string | Nombre para mostrar del rol personalizado. Debe ser único para el inquilino. Puede incluir letras, números, espacios y caracteres especiales. El número máximo de caracteres es 128. |
| `Id` | Sí | string | El identificador exclusivo del rol personalizado. Para Azure PowerShell y la CLI de Azure, este identificador se genera automáticamente cuando se crea un rol. |
| `IsCustom` | Sí | string | Indica si es un rol personalizado. Establecido en `true` para los roles personalizados. |
| `Description` | Sí | string | Descripción del rol personalizado. Puede incluir letras, números, espacios y caracteres especiales. El número máximo de caracteres es 1024. |
| `Actions` | Sí | String[] | Matriz de cadenas que especifica las operaciones de administración que el rol permite realizar. Para obtener más información, consulte [actions](role-definitions.md#actions). |
| `NotActions` | Sin  | String[] | Matriz de cadenas que especifica las operaciones de administración que se excluyen de las `actions` permitidas. Para obtener más información, consulte [notActions](role-definitions.md#notactions). |
| `DataActions` | Sin  | String[] | Matriz de cadenas que especifica las operaciones de datos que el rol permite realizar en los datos dentro de ese objeto. Para obtener más información, consulte [dataActions (versión preliminar)](role-definitions.md#dataactions-preview). |
| `NotDataActions` | Sin  | String[] | Matriz de cadenas que especifica las operaciones de datos que se excluyen de las `dataActions` permitidas. Para obtener más información, consulte [notDataActions (versión preliminar)](role-definitions.md#notdataactions-preview). |
| `AssignableScopes` | Sí | String[] | Matriz de cadenas que especifica los ámbitos en los que el rol personalizado está disponible para la asignación. No se puede establecer en el ámbito raíz (`"/"`). Para obtener más información, consulte [assignableScopes](role-definitions.md#assignablescopes). |

## <a name="assignablescopes-for-custom-roles"></a>assignableScopes para roles personalizados

Al igual que los roles integrados, la propiedad `assignableScopes` especifica los ámbitos en los que el rol está disponible para la asignación. Sin embargo, no puede usar el ámbito raíz (`"/"`) en sus propios roles personalizados. Si lo intenta, se producirá un error de autorización. La propiedad `assignableScopes` de un rol personalizado también controla quién puede crear, eliminar, modificar o ver dicho rol.

| Task | Operación | DESCRIPCIÓN |
| --- | --- | --- |
| Creación o eliminación de un rol personalizado | `Microsoft.Authorization/ roleDefinition/write` | Los usuarios que tienen acceso a esta operación en todos los ámbitos `assignableScopes` del rol personalizado pueden crear (o eliminar) roles personalizados para usar en esos ámbitos. Por ejemplo, los [propietarios](built-in-roles.md#owner) y [administradores del acceso de los usuarios](built-in-roles.md#user-access-administrator) de las suscripciones, los grupos de recursos y los recursos. |
| Modificación de un rol personalizado | `Microsoft.Authorization/ roleDefinition/write` | Los usuarios que tienen acceso a esta operación en todos los ámbitos `assignableScopes` del rol personalizado pueden modificar roles personalizados en esos ámbitos. Por ejemplo, los [propietarios](built-in-roles.md#owner) y [administradores del acceso de los usuarios](built-in-roles.md#user-access-administrator) de las suscripciones, los grupos de recursos y los recursos. |
| Visualización de un rol personalizado | `Microsoft.Authorization/ roleDefinition/read` | Los usuarios que tienen acceso a esta operación en un ámbito pueden ver los roles personalizados que están disponibles para su asignación en ese ámbito. Todos los roles integrados permiten que los roles personalizados estén disponibles para la asignación. |

## <a name="next-steps"></a>Pasos siguientes
- [Creación de roles personalizados con Azure PowerShell](custom-roles-powershell.md)
- [Creación de roles personalizados con la CLI de Azure](custom-roles-cli.md)
- [Descripción de definiciones de roles](role-definitions.md)
