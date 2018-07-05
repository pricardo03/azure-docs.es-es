---
title: Creación de roles personalizados con la CLI de Azure | Microsoft Docs
description: Obtenga información sobre cómo crear roles para el control de acceso basado en roles (RBAC) con la CLI de Azure. Esto incluye cómo enumerar, crear, actualizar y eliminar roles personalizados.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 3483ee01-8177-49e7-b337-4d5cb14f5e32
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/20/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 3b5d18a3e0bf846137dfdf68b8e5dd9e2db58792
ms.sourcegitcommit: e0834ad0bad38f4fb007053a472bde918d69f6cb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37437263"
---
# <a name="create-custom-roles-using-azure-cli"></a>Creación de roles personalizados con la CLI de Azure

Si los [roles integrados](built-in-roles.md) no cumplen las necesidades específicas de su organización, puede crear sus propios roles personalizados. En este artículo se describe cómo crear y administrar roles personalizados con la CLI de Azure.

## <a name="prerequisites"></a>requisitos previos

Para crear roles personalizados, necesita:

- Permisos para crear roles personalizados, como [Propietario](built-in-roles.md#owner) o [Administrador de acceso de usuarios](built-in-roles.md#user-access-administrator)
- La [CLI de Azure](/cli/azure/install-azure-cli) instalada localmente

## <a name="list-custom-roles"></a>Lista de roles personalizados

Para enumerar los roles personalizados que están disponibles para la asignación, use [az role definition list](/cli/azure/role/definition#az-role-definition-list). En los siguientes ejemplos se enumeran todos los roles personalizados de la suscripción actual.

```azurecli
az role definition list --custom-role-only true --output json | jq '.[] | {"roleName":.roleName, "roleType":.roleType}'
```

```azurecli
az role definition list --output json | jq '.[] | if .roleType == "CustomRole" then {"roleName":.roleName, "roleType":.roleType} else empty end'
```

```Output
{
  "roleName": "My Management Contributor",
  "type": "CustomRole"
}
{
  "roleName": "My Service Reader Role",
  "type": "CustomRole"
}
{
  "roleName": "Virtual Machine Operator",
  "type": "CustomRole"
}

...
```

## <a name="create-a-custom-role"></a>Crear un rol personalizado

Para crear un rol personalizado, use [az role definition create](/cli/azure/role/definition#az-role-definition-create). La definición de roles puede ser una descripción JSON o una ruta de acceso a un archivo que contenga esta descripción.

```azurecli
az role definition create --role-definition <role_definition>
```

En el ejemplo siguiente, se crea un rol personalizado llamado *Operador de máquina virtual*. El rol personalizado asigna acceso a todas las operaciones de lectura de los proveedores de recursos *Microsoft.Compute*, *Microsoft.Storage* y *Microsoft.Network*, y asigna acceso para iniciar, reiniciar y supervisar las máquinas virtuales. El rol personalizado se puede usar en dos suscripciones. En este ejemplo, se usa un archivo JSON como entrada.

vmoperator.json

```json
{
  "Name": "Virtual Machine Operator",
  "IsCustom": true,
  "Description": "Can monitor and restart virtual machines.",
  "Actions": [
    "Microsoft.Storage/*/read",
    "Microsoft.Network/*/read",
    "Microsoft.Compute/*/read",
    "Microsoft.Compute/virtualMachines/start/action",
    "Microsoft.Compute/virtualMachines/restart/action",
    "Microsoft.Authorization/*/read",
    "Microsoft.Resources/subscriptions/resourceGroups/read",
    "Microsoft.Insights/alertRules/*",
    "Microsoft.Support/*"
  ],
  "NotActions": [

  ],
  "AssignableScopes": [
    "/subscriptions/11111111-1111-1111-1111-111111111111",
    "/subscriptions/33333333-3333-3333-3333-333333333333"
  ]
}
```

```azurecli
az role definition create --role-definition ~/roles/vmoperator.json
```

## <a name="update-a-custom-role"></a>Actualización de un rol personalizado

Para actualizar un rol personalizado, primero use [az role definition list](/cli/azure/role/definition#az-role-definition-list) para recuperar la definición de roles. Después, haga los cambios que desee en la definición de rol. Por último, use [az role definition update](/cli/azure/role/definition#az-role-definition-update) para guardar la definición de roles actualizada.

```azurecli
az role definition update --role-definition <role_definition>
```

En el ejemplo siguiente, se agrega la operación *Microsoft.Insights/diagnosticSettings/* a *Acciones* del rol personalizado de *Operador de máquina virtual*.

vmoperator.json

```json
{
  "Name": "Virtual Machine Operator",
  "IsCustom": true,
  "Description": "Can monitor and restart virtual machines.",
  "Actions": [
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
  "NotActions": [

  ],
  "AssignableScopes": [
    "/subscriptions/11111111-1111-1111-1111-111111111111",
    "/subscriptions/33333333-3333-3333-3333-333333333333"
  ]
}
```

```azurecli
az role definition update --role-definition ~/roles/vmoperator.json
```

## <a name="delete-a-custom-role"></a>Eliminación de un rol personalizado

Para eliminar un rol personalizado, use [az role definition delete](/cli/azure/role/definition#az-role-definition-delete). Para especificar el rol que desea eliminar, use el nombre del rol o el identificador. Para determinar el identificador del rol, use [az role definition list](/cli/azure/role/definition#az-role-definition-list).

```azurecli
az role definition delete --name <role_name or role_id>
```

En el ejemplo siguiente se elimina el rol personalizado *Operador de máquina virtual*.

```azurecli
az role definition delete --name "Virtual Machine Operator"
```

## <a name="next-steps"></a>Pasos siguientes

- [Tutorial: Creación de un rol personalizado con la CLI de Azure](tutorial-custom-role-cli.md)
- [Roles personalizados en Azure](custom-roles.md)
- [Operaciones del proveedor de recursos de Azure Resource Manager](resource-provider-operations.md)