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
ms.date: 11/21/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 795a97f84bebf6c0e7c1692e82df2f7ce11e0bbd
ms.sourcegitcommit: f523c8a8557ade6c4db6be12d7a01e535ff32f32
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/22/2019
ms.locfileid: "74384096"
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

## <a name="list-a-role-definition"></a>Enumeración de una definición de roles

Para enumerar una definición de roles, use [az role definition list](/cli/azure/role/definition#az-role-definition-list):

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

En el ejemplo siguiente, se muestran solo los valores de *actions* y *notActions* del rol *Colaborador*:

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

En el ejemplo siguiente se muestran solo los valores de actions del rol *Colaborador de máquina virtual*:

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

De forma predeterminada, se mostrarán únicamente las asignaciones directas que se limitan a la suscripción. Para ver las asignaciones con ámbito por grupo o recurso, use `--all`, y para ver asignaciones heredadas, `--include-inherited`.

En el ejemplo siguiente, se muestran las asignaciones de roles asignadas directamente al usuario *patlong\@contoso.com*:

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

### <a name="list-role-assignments-at-a-resource-group-scope"></a>Enumeración de asignaciones de roles en un ámbito de grupo de recursos

Para mostrar las asignaciones de roles de un ámbito de grupo de recursos, utilice [az role assignment list](/cli/azure/role/assignment#az-role-assignment-list):

```azurecli
az role assignment list --resource-group <resource_group>
```

En el ejemplo siguiente se muestran las asignaciones de roles del grupo de recursos *pharma-sales*:

```azurecli
az role assignment list --resource-group pharma-sales --output json | jq '.[] | {"principalName":.principalName, "roleDefinitionName":.roleDefinitionName, "scope":.scope}'
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

...
```

### <a name="list-role-assignments-at-a-subscription-scope"></a>Enumeración de asignaciones de roles en un ámbito de suscripción

Para enumerar todas las asignaciones de roles en un ámbito de suscripción, use [az role assignment list](/cli/azure/role/assignment#az-role-assignment-list). El identificador de suscripción lo puede encontrar en la hoja **Suscripciones** de Azure Portal, o bien, puede usar [az account list](/cli/azure/account#az-account-list).

```azurecli
az role assignment list --subscription <subscription_name_or_id>
```

```Example
az role assignment list --subscription 00000000-0000-0000-0000-000000000000 --output json | jq '.[] | {"principalName":.principalName, "roleDefinitionName":.roleDefinitionName, "scope":.scope}'
```

### <a name="list-role-assignments-at-a-management-group-scope"></a>Lista de asignaciones de roles en un ámbito de grupo de administración

Para enumerar todas las asignaciones de roles de un ámbito de grupo de administración, use [az role assignment list](/cli/azure/role/assignment#az-role-assignment-list). El identificador del grupo de administración lo puede encontrar en la hoja **Grupos de administración** de Azure Portal, o bien, puede usar [az account management-group list](/cli/azure/ext/managementgroups/account/management-group#ext-managementgroups-az-account-management-group-list).

```azurecli
az role assignment list --scope /providers/Microsoft.Management/managementGroups/<group_id>
```

```Example
az role assignment list --scope /providers/Microsoft.Management/managementGroups/marketing-group --output json | jq '.[] | {"principalName":.principalName, "roleDefinitionName":.roleDefinitionName, "scope":.scope}'
```

## <a name="get-object-ids"></a>Obtener identificadores de objeto

Para enumerar, agregar o quitar asignaciones de roles, es posible que tenga que especificar el identificador único de un objeto. El identificador tiene el formato: `11111111-1111-1111-1111-111111111111`. Puede obtener el identificador mediante Azure Portal o la CLI de Azure.

### <a name="user"></a>Usuario

Para obtener el identificador del objeto de un usuario de Azure AD, use [az ad user show](/cli/azure/ad/user#az-ad-user-show).

```azurecli
az ad user show --id "{email}" --query objectId --output tsv
```

### <a name="group"></a>Grupo

Para obtener el identificador de objeto de un grupo de Azure AD, puede usar [az ad group show](/cli/azure/ad/group#az-ad-group-show) o [az ad group list](/cli/azure/ad/group#az-ad-group-list).

```azurecli
az ad group show --group "{name}" --query objectId --output tsv
```

### <a name="application"></a>Application

Para obtener el identificador de objeto de una entidad de servicio de Azure AD (identidad usada por una aplicación), puede usar [az ad sp list](/cli/azure/ad/sp#az-ad-sp-list). Para una entidad de servicio, use el id. de objeto y **no** el de aplicación.

```azurecli
az ad sp list --display-name "{name}" --query [].objectId --output tsv
```

## <a name="grant-access"></a>Conceder acceso

En RBAC, para conceder acceso es preciso crear una asignación de roles.

### <a name="create-a-role-assignment-for-a-user-at-a-resource-group-scope"></a>Creación de una asignación de roles para un usuario en el ámbito de un grupo de recursos

Para conceder acceso a un usuario a un ámbito de grupo de recursos, use [az role assignment create](/cli/azure/role/assignment#az-role-assignment-create).

```azurecli
az role assignment create --role <role_name_or_id> --assignee <assignee> --resource-group <resource_group>
```

En el ejemplo siguiente, se asigna el rol *Colaborador de la máquina virtual* al usuario *patlong\@contoso.com* en el ámbito del grupo de recursos *pharma-sales*:

```azurecli
az role assignment create --role "Virtual Machine Contributor" --assignee patlong@contoso.com --resource-group pharma-sales
```

### <a name="create-a-role-assignment-using-the-unique-role-id"></a>Creación de una asignación de roles mediante el identificador único de la función

Hay un par de veces en las que el nombre de un rol puede cambiar, por ejemplo:

- Utiliza su propio rol personalizado y decide cambiar el nombre.
- Utiliza una rol de versión preliminar que tiene **(Versión preliminar)** en el nombre. Cuando se libera el rol, se cambia su nombre.

> [!IMPORTANT]
> Se ofrece una versión preliminar sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Incluso si se cambia el nombre de un rol, su identificador no cambia. Si utiliza scripts o automatización para crear las asignaciones de roles, se recomienda utilizar el identificador de rol único en lugar del nombre del rol. Por lo tanto, si se cambia el nombre de un rol, es más probable que los scripts funcionen.

Para crear una asignación de roles con el identificador de función único en lugar del nombre de rol, utilice el cmdlet [az role assignment create](/cli/azure/role/assignment#az-role-assignment-create).

```azurecli
az role assignment create --role <role_id> --assignee <assignee> --resource-group <resource_group>
```

En el ejemplo siguiente, se asigna el rol [Colaborador de la máquina virtual](built-in-roles.md#virtual-machine-contributor) al usuario *patlong\@contoso.com* en el ámbito del grupo de recursos *pharma-sales*. Para obtener el identificador de rol único, puede usar [az role definition list](/cli/azure/role/definition#az-role-definition-list) o consultar [Roles integrados en los recursos de Azure](built-in-roles.md).

```azurecli
az role assignment create --role 9980e02c-c2be-4d73-94e8-173b1dc7cf3c --assignee patlong@contoso.com --resource-group pharma-sales
```

### <a name="create-a-role-assignment-for-a-group"></a>Creación de una asignación de roles para un grupo

Para conceder acceso a un grupo, use [az role assignment create](/cli/azure/role/assignment#az-role-assignment-create). Para obtener información sobre cómo obtener el identificador de objeto del grupo, consulte [Obtener identificadores de objeto](#get-object-ids).

```azurecli
az role assignment create --role <role_name_or_id> --assignee-object-id <assignee_object_id> --resource-group <resource_group> --scope </subscriptions/subscription_id>
```

En el ejemplo siguiente se asigna el rol *Lector* al grupo *Ann Mack Team* con el identificador 22222222-2222-2222-2222-222222222222 en el ámbito de suscripción.

```azurecli
az role assignment create --role Reader --assignee-object-id 22222222-2222-2222-2222-222222222222 --scope /subscriptions/00000000-0000-0000-0000-000000000000
```

En el ejemplo siguiente se asigna el rol *Colaborador de la máquina virtual* al grupo *Ann Mack Team* con el identificador 22222222-2222-2222-2222-222222222222 en un ámbito de recurso de una red virtual denominada *pharma-sales-project-network*.

```azurecli
az role assignment create --role "Virtual Machine Contributor" --assignee-object-id 22222222-2222-2222-2222-222222222222 --scope /subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/pharma-sales/providers/Microsoft.Network/virtualNetworks/pharma-sales-project-network
```

### <a name="create-a-role-assignment-for-an-application-at-a-resource-group-scope"></a>Creación de una asignación de roles para una aplicación en un ámbito de grupo de recursos

Para conceder acceso a una aplicación, use [az role assignment create](/cli/azure/role/assignment#az-role-assignment-create). Para obtener información sobre cómo obtener el identificador de objeto de la aplicación, consulte [Obtener identificadores de objeto](#get-object-ids).

```azurecli
az role assignment create --role <role_name_or_id> --assignee-object-id <assignee_object_id> --resource-group <resource_group>
```

En el ejemplo siguiente, se asigna el rol *Colaborador de la máquina virtual* a una aplicación con el identificador de objeto 44444444-4444-4444-4444-444444444444 en el ámbito del grupo de recursos *pharma-sales*.

```azurecli
az role assignment create --role "Virtual Machine Contributor" --assignee-object-id 44444444-4444-4444-4444-444444444444 --resource-group pharma-sales
```

### <a name="create-a-role-assignment-for-a-user-at-a-subscription-scope"></a>Creación de una asignación de roles para un usuario en un ámbito de suscripción

Para conceder acceso a un usuario a un ámbito de suscripción, use [az role assignment create](/cli/azure/role/assignment#az-role-assignment-create). El identificador de suscripción lo puede encontrar en la hoja **Suscripciones** de Azure Portal, o bien, puede usar [az account list](/cli/azure/account#az-account-list).

```azurecli
az role assignment create --role <role_name_or_id> --assignee <assignee> --subscription <subscription_name_or_id>
```

En el ejemplo siguiente se asigna el rol *Lector* al usuario *annm\@example.com* en un ámbito de suscripción.

```azurecli
az role assignment create --role "Reader" --assignee annm@example.com --subscription 00000000-0000-0000-0000-000000000000
```

### <a name="create-a-role-assignment-for-a-user-at-a-management-group-scope"></a>Creación de una asignación de roles para un usuario en el ámbito de un grupo de administración

Para conceder acceso a un usuario a un ámbito de grupo de administración, use [az role assignment create](/cli/azure/role/assignment#az-role-assignment-create). El identificador del grupo de administración lo puede encontrar en la hoja **Grupos de administración** de Azure Portal, o bien, puede usar [az account management-group list](/cli/azure/ext/managementgroups/account/management-group#ext-managementgroups-az-account-management-group-list).

```azurecli
az role assignment create --role <role_name_or_id> --assignee <assignee> --scope /providers/Microsoft.Management/managementGroups/<group_id>
```

En el ejemplo siguiente se asigna el rol *Lector de facturación* al usuario *alain\@example.com* en un ámbito de grupo de administración.

```azurecli
az role assignment create --role "Billing Reader" --assignee alain@example.com --scope /providers/Microsoft.Management/managementGroups/marketing-group
```

### <a name="create-a-role-assignment-for-a-new-service-principal"></a>Creación de una asignación de roles para una nueva entidad de servicio

Si crea una entidad de servicio e inmediatamente intenta asignarle un rol, esa asignación de roles puede producir un error en algunos casos. Por ejemplo, si usa un script para crear una identidad administrada y luego intenta asignarle un rol a esa entidad de servicio, la asignación de roles podría producir un error. Es probable que el motivo de este error sea un retraso en la replicación. La entidad de servicio se crea en una región; sin embargo, la asignación de roles puede tener lugar en una región distinta que todavía no haya replicado la entidad de servicio. Para abordar este escenario, debe especificar el tipo de entidad de seguridad al crear la asignación de roles.

Para crear una asignación de roles, use [az role assignment create](/cli/azure/role/assignment#az-role-assignment-create), especifique un valor para `--assignee-object-id` y establezca `--assignee-principal-type` en `ServicePrincipal`.

```azurecli
az role assignment create --role <role_name_or_id> --assignee-object-id <assignee_object_id> --assignee-principal-type <assignee_principal_type> --resource-group <resource_group> --scope </subscriptions/subscription_id>
```

En el ejemplo siguiente, se asigna el rol *Colaborador de la máquina virtual* a la identidad administrada *msi-test* en el ámbito del grupo de recursos *pharma-sales*:

```azurecli
az role assignment create --role "Virtual Machine Contributor" --assignee-object-id 33333333-3333-3333-3333-333333333333 --assignee-principal-type ServicePrincipal --resource-group pharma-sales
```

## <a name="remove-access"></a>Quitar acceso

En RBAC, para quitar el acceso, es preciso quitar una asignación de roles mediante [az role assignment delete](/cli/azure/role/assignment#az-role-assignment-delete):

```azurecli
az role assignment delete --assignee <assignee> --role <role_name_or_id> --resource-group <resource_group>
```

En el ejemplo siguiente, se quita la asignación de roles *Colaborador de la máquina virtual* del usuario *patlong\@contoso.com* en el grupo de recursos *pharma-sales*:

```azurecli
az role assignment delete --assignee patlong@contoso.com --role "Virtual Machine Contributor" --resource-group pharma-sales
```

En el ejemplo siguiente se quita el rol *Lector* al grupo *Ann Mack Team* con el identificador 22222222-2222-2222-2222-222222222222 en un ámbito de suscripción. Para obtener información sobre cómo obtener el identificador de objeto del grupo, consulte [Obtener identificadores de objeto](#get-object-ids).

```azurecli
az role assignment delete --assignee 22222222-2222-2222-2222-222222222222 --role "Reader" --subscription 00000000-0000-0000-0000-000000000000
```

En el ejemplo siguiente se quita el rol *Lector de facturación* al usuario *alain\@example.com* en el ámbito de grupo de administración. Para obtener el identificador del grupo de administración, puede usar [az account management-group list](/cli/azure/ext/managementgroups/account/management-group#ext-managementgroups-az-account-management-group-list).

```azurecli
az role assignment delete --assignee alain@example.com --role "Billing Reader" --scope /providers/Microsoft.Management/managementGroups/marketing-group
```

## <a name="next-steps"></a>Pasos siguientes

- [Tutorial: Creación de un rol personalizado para los recursos de Azure con la CLI de Azure](tutorial-custom-role-cli.md)
- [Uso de la CLI de Azure para administrar los recursos y grupos de recursos de Azure](../azure-resource-manager/cli-azure-resource-manager.md)
