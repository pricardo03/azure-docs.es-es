---
title: Administración del acceso a los recursos de Azure mediante RBAC y la CLI de Azure | Microsoft Docs
description: Aprenda a administrar el acceso a recursos de Azure de usuarios, grupos y aplicaciones mediante el control de acceso basado en rol (RBAC) y la CLI de Azure. Esto incluye cómo enumerar el acceso, conceder acceso y quitar el acceso.
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
ms.date: 04/17/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 1cc3d3eca4063a8120851a9d3de1a85292eacb11
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/22/2019
ms.locfileid: "60011068"
---
# <a name="manage-access-to-azure-resources-using-rbac-and-azure-cli"></a>Administración del acceso a los recursos de Azure mediante RBAC y la CLI de Azure

El [control de acceso basado en rol (RBAC)](overview.md) es la forma en la que se administra el acceso a los recursos de Azure. En este artículo se describe cómo administrar el acceso de usuarios, grupos y aplicaciones mediante el uso de RBAC y la CLI de Azure.

## <a name="prerequisites"></a>Requisitos previos

Para gestionar el acceso, necesita una de las siguientes opciones:

* [Bash en Azure Cloud Shell](/azure/cloud-shell/overview)
* [CLI de Azure](/cli/azure)

## <a name="list-roles"></a>Lista de roles

Para enumerar todas las definiciones de roles disponibles, utilice [az role definition list](/cli/azure/role/definition#az-role-definition-list):

```azurecli
az role definition list
```

En el ejemplo siguiente se muestra el nombre y la descripción de todas las definiciones de roles disponibles:

```azurecli
az role definition list --output json | jq '.[] | {"roleName":.roleName, "description":.description}'
```

```Output
{
  "roleName": "API Management Service Contributor",
  "description": "Can manage service and the APIs"
}
{
  "roleName": "API Management Service Operator Role",
  "description": "Can manage service but not the APIs"
}
{
  "roleName": "API Management Service Reader Role",
  "description": "Read-only access to service and APIs"
}

...
```

En el ejemplo siguiente se muestran todas las definiciones de roles integradas:

```azurecli
az role definition list --custom-role-only false --output json | jq '.[] | {"roleName":.roleName, "description":.description, "roleType":.roleType}'
```

```Output
{
  "roleName": "API Management Service Contributor",
  "description": "Can manage service and the APIs",
  "roleType": "BuiltInRole"
}
{
  "roleName": "API Management Service Operator Role",
  "description": "Can manage service but not the APIs",
  "roleType": "BuiltInRole"
}
{
  "roleName": "API Management Service Reader Role",
  "description": "Read-only access to service and APIs",
  "roleType": "BuiltInRole"
}

...
```

## <a name="list-a-role-definition"></a>Lista de una definición de rol

Para mostrar una definición de función, utilice [lista de definición de roles az](/cli/azure/role/definition#az-role-definition-list):

```azurecli
az role definition list --name <role_name>
```

En el ejemplo siguiente se muestra la definición de roles de *Colaborador*:

```azurecli
az role definition list --name "Contributor"
```

```Output
[
  {
    "additionalProperties": {},
    "assignableScopes": [
      "/"
    ],
    "description": "Lets you manage everything except access to resources.",
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c",
    "name": "b24988ac-6180-42a0-ab88-20f7382dd24c",
    "permissions": [
      {
        "actions": [
          "*"
        ],
        "additionalProperties": {},
        "dataActions": [],
        "notActions": [
          "Microsoft.Authorization/*/Delete",
          "Microsoft.Authorization/*/Write",
          "Microsoft.Authorization/elevateAccess/Action"
        ],
        "notDataActions": []
      }
    ],
    "roleName": "Contributor",
    "roleType": "BuiltInRole",
    "type": "Microsoft.Authorization/roleDefinitions"
  }
]
```

### <a name="list-actions-of-a-role"></a>Lista de las acciones de un rol

El ejemplo siguiente se muestra solo la *acciones* y *notActions* de la *colaborador* rol:

```azurecli
az role definition list --name "Contributor" --output json | jq '.[] | {"actions":.permissions[0].actions, "notActions":.permissions[0].notActions}'
```

```Output
{
  "actions": [
    "*"
  ],
  "notActions": [
    "Microsoft.Authorization/*/Delete",
    "Microsoft.Authorization/*/Write",
    "Microsoft.Authorization/elevateAccess/Action"
  ]
}
```

En el ejemplo siguiente se muestran solo las acciones de la *colaborador de máquina Virtual* rol:

```azurecli
az role definition list --name "Virtual Machine Contributor" --output json | jq '.[] | .permissions[0].actions'
```

```Output
[
  "Microsoft.Authorization/*/read",
  "Microsoft.Compute/availabilitySets/*",
  "Microsoft.Compute/locations/*",
  "Microsoft.Compute/virtualMachines/*",
  "Microsoft.Compute/virtualMachineScaleSets/*",
  "Microsoft.Insights/alertRules/*",
  "Microsoft.Network/applicationGateways/backendAddressPools/join/action",
  "Microsoft.Network/loadBalancers/backendAddressPools/join/action",

  ...

  "Microsoft.Storage/storageAccounts/listKeys/action",
  "Microsoft.Storage/storageAccounts/read"
]
```

## <a name="list-access"></a>Lista de acceso

En RBAC, para enumerar el acceso se enumeran las asignaciones de roles.

### <a name="list-role-assignments-for-a-user"></a>Lista de las asignaciones de rol de un usuario

Para mostrar las asignaciones de roles de un usuario específico use [az role assignment list](/cli/azure/role/assignment#az-role-assignment-list):

```azurecli
az role assignment list --assignee <assignee>
```

De forma predeterminada, se mostrarán únicamente las asignaciones que se limitan a la suscripción. Para ver las asignaciones limitadas por grupo o recurso, use `--all`.

En el ejemplo siguiente se muestran las asignaciones de roles que se asignan directamente a la *patlong\@contoso.com* usuario:

```azurecli
az role assignment list --all --assignee patlong@contoso.com --output json | jq '.[] | {"principalName":.principalName, "roleDefinitionName":.roleDefinitionName, "scope":.scope}'
```

```Output
{
  "principalName": "patlong@contoso.com",
  "roleDefinitionName": "Backup Operator",
  "scope": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales"
}
{
  "principalName": "patlong@contoso.com",
  "roleDefinitionName": "Virtual Machine Contributor",
  "scope": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales"
}
```

### <a name="list-role-assignments-for-a-resource-group"></a>Lista de las asignaciones de roles de un grupo de recursos

Para mostrar las asignaciones de roles de un grupo de recursos, utilice [az role assignment list](/cli/azure/role/assignment#az-role-assignment-list):

```azurecli
az role assignment list --resource-group <resource_group>
```

En el ejemplo siguiente se muestran las asignaciones de roles para la *pharma-sales* grupo de recursos:

```azurecli
az role assignment list --resource-group pharma-sales --output json | jq '.[] | {"roleDefinitionName":.roleDefinitionName, "scope":.scope}'
```

```Output
{
  "roleDefinitionName": "Backup Operator",
  "scope": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales"
}
{
  "roleDefinitionName": "Virtual Machine Contributor",
  "scope": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales"
}

...
```

## <a name="grant-access"></a>Conceder acceso

En RBAC, para conceder acceso es preciso crear una asignación de roles.

### <a name="create-a-role-assignment-for-a-user"></a>Creación de una asignación de roles para un usuario

Para crear una asignación de roles para un usuario en el ámbito del grupo de recursos, use [az role assignment create](/cli/azure/role/assignment#az-role-assignment-create):

```azurecli
az role assignment create --role <role_name_or_id> --assignee <assignee> --resource-group <resource_group>
```

En el ejemplo siguiente se asigna el *colaborador de máquina Virtual* rol *patlong\@contoso.com* usuario en el *pharma-sales* ámbito del grupo de recursos:

```azurecli
az role assignment create --role "Virtual Machine Contributor" --assignee patlong@contoso.com --resource-group pharma-sales
```

### <a name="create-a-role-assignment-using-the-unique-role-id"></a>Crear una asignación de roles mediante el identificador único de la función

Hay un par de veces cuando es posible que cambiar un nombre de rol, por ejemplo:

- Usa un rol personalizado propio y decide cambiar el nombre.
- Usa un rol de vista previa que tenga **(versión preliminar)** en el nombre. Cuando se suelta el rol, se cambia el nombre de la función.

> [!IMPORTANT]
> Una versión preliminar se proporciona sin un contrato de nivel de servicio y no se recomienda para las cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Incluso si se cambia el nombre de un rol, no cambia el identificador de rol. Si utiliza secuencias de comandos o la automatización para crear las asignaciones de rol, es una práctica recomendada para usar el identificador único de la función en lugar del nombre de rol. Por lo tanto, si se cambia el nombre de un rol, las secuencias de comandos están más probable que funcione.

Para crear una asignación de roles mediante el identificador único de la función en lugar del nombre de rol, use [crear asignación de roles az](/cli/azure/role/assignment#az-role-assignment-create).

```azurecli
az role assignment create --role <role_id> --assignee <assignee> --resource-group <resource_group>
```

En el ejemplo siguiente se asigna el [colaborador de máquina Virtual](built-in-roles.md#virtual-machine-contributor) rol *patlong\@contoso.com* usuario en el *pharma-sales* ámbito del grupo de recursos. Para obtener el identificador único de la función, puede usar [lista de definición de roles az](/cli/azure/role/definition#az-role-definition-list) o vea [roles integrados para los recursos de Azure](built-in-roles.md).

```azurecli
az role assignment create --role 9980e02c-c2be-4d73-94e8-173b1dc7cf3c --assignee patlong@contoso.com --resource-group pharma-sales
```

### <a name="create-a-role-assignment-for-a-group"></a>Creación de una asignación de roles para un grupo

Para crear una asignación de roles para un grupo, use [az role assignment create](/cli/azure/role/assignment#az-role-assignment-create):

```azurecli
az role assignment create --role <role_name_or_id> --assignee-object-id <assignee_object_id> --resource-group <resource_group> --scope </subscriptions/subscription_id>
```

En el ejemplo siguiente se asigna el rol *Lector* al grupo *equipo de Ann Mack* con el identificador 22222222-2222-2222-2222-222222222222 en el ámbito de la suscripción. Para obtener el identificador del grupo, puede usar [az ad group list](/cli/azure/ad/group#az-ad-group-list) o [az ad group show](/cli/azure/ad/group#az-ad-group-show).

```azurecli
az role assignment create --role Reader --assignee-object-id 22222222-2222-2222-2222-222222222222 --scope /subscriptions/00000000-0000-0000-0000-000000000000
```

En el ejemplo siguiente se asigna el rol *Colaborador de la máquina virtual* al grupo *equipo de Ann Mack* con el identificador 22222222-2222-2222-2222-222222222222 en un ámbito de recursos de una red virtual denominada *pharma-sales-project-network*:

```azurecli
az role assignment create --role "Virtual Machine Contributor" --assignee-object-id 22222222-2222-2222-2222-222222222222 --scope /subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/pharma-sales/providers/Microsoft.Network/virtualNetworks/pharma-sales-project-network
```

### <a name="create-a-role-assignment-for-an-application"></a>Creación de una asignación de roles para una aplicación

Para crear un rol para una asignación, use [az role assignment create](/cli/azure/role/assignment#az-role-assignment-create):

```azurecli
az role assignment create --role <role_name_or_id> --assignee-object-id <assignee_object_id> --resource-group <resource_group> --scope </subscriptions/subscription_id>
```

En el ejemplo siguiente se asigna el *colaborador de máquina Virtual* rol a una aplicación con el identificador de objeto 44444444-4444-4444-4444-444444444444 en el *pharma-sales* ámbito del grupo de recursos. Para obtener el identificador de objeto de la aplicación, puede usar [az ad app list](/cli/azure/ad/app#az-ad-app-list) o [az ad app show](/cli/azure/ad/app#az-ad-app-show).

```azurecli
az role assignment create --role "Virtual Machine Contributor" --assignee-object-id 44444444-4444-4444-4444-444444444444 --resource-group pharma-sales
```

## <a name="remove-access"></a>Quitar acceso

En RBAC, para quitar el acceso, es preciso quitar una asignación de roles mediante [az role assignment delete](/cli/azure/role/assignment#az-role-assignment-delete):

```azurecli
az role assignment delete --assignee <assignee> --role <role_name_or_id> --resource-group <resource_group>
```

En el ejemplo siguiente se quita el *colaborador de máquina Virtual* asignación de roles de la *patlong\@contoso.com* usuario en el *pharma-sales* grupo de recursos:

```azurecli
az role assignment delete --assignee patlong@contoso.com --role "Virtual Machine Contributor" --resource-group pharma-sales
```

En el ejemplo siguiente se quita el rol *Lector* al grupo *equipo de Ann Mack* con el identificador 22222222-2222-2222-2222-222222222222 en el ámbito de la suscripción. Para obtener el identificador del grupo, puede usar [az ad group list](/cli/azure/ad/group#az-ad-group-list) o [az ad group show](/cli/azure/ad/group#az-ad-group-show).

```azurecli
az role assignment delete --assignee 22222222-2222-2222-2222-222222222222 --role "Reader" --scope /subscriptions/00000000-0000-0000-0000-000000000000
```

## <a name="next-steps"></a>Pasos siguientes

- [Tutorial: Creación de un rol personalizado para los recursos de Azure con la CLI de Azure](tutorial-custom-role-cli.md)
- [Uso de la CLI de Azure para administrar los recursos y grupos de recursos de Azure](../azure-resource-manager/cli-azure-resource-manager.md)
