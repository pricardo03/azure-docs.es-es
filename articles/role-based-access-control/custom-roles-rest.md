---
title: Creación de roles personalizados con la API de REST en Azure | Microsoft Docs
description: Obtenga información sobre cómo crear roles para el control de acceso basado en roles (RBAC) con la API de REST. Esto incluye cómo enumerar, crear, actualizar y eliminar roles personalizados.
services: active-directory
documentationcenter: na
author: rolyon
manager: mtillman
editor: ''
ms.assetid: 1f90228a-7aac-4ea7-ad82-b57d222ab128
ms.service: role-based-access-control
ms.workload: multiple
ms.tgt_pltfrm: rest-api
ms.devlang: na
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 8a1bbe8217e2d4a9846f56124e248e19cbe70b19
ms.sourcegitcommit: e0834ad0bad38f4fb007053a472bde918d69f6cb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37436069"
---
# <a name="create-custom-roles-using-the-rest-api"></a>Creación de roles personalizados con la API de REST

Si los [roles integrados](built-in-roles.md) no cumplen las necesidades específicas de su organización, puede crear sus propios roles personalizados. En este artículo se describe cómo crear y administrar roles personalizados con la API de REST.

## <a name="list-roles"></a>Lista de roles

Para enumerar todos los roles u obtener información sobre un rol en particular con el nombre para mostrar, use la API de REST [Role Definitions - List](/rest/api/authorization/roledefinitions/list). Para llamar a esta API, debe tener acceso a la operación `Microsoft.Authorization/roleDefinitions/read` en el ámbito. Se concede acceso a esta operación a varios [roles integrados](built-in-roles.md).

1. Empiece con la solicitud siguiente:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions?api-version=2015-07-01&$filter={filter}
    ```

1. En el identificador URI, reemplace *{scope}* por el ámbito cuya lista de roles quiere obtener.

    | Scope | Escriba |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Subscription |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Grupos de recursos |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Recurso |

1. Reemplace *{filter}* por la condición que quiere aplicar para filtrar la lista de roles.

    | Filtrar | DESCRIPCIÓN |
    | --- | --- |
    | `$filter=atScopeAndBelow()` | Enumera los roles disponibles para asignar en el ámbito especificado y cualquiera de sus ámbitos secundarios. |
    | `$filter=roleName%20eq%20'{roleDisplayName}'` | Use la forma con codificación URL del nombre para mostrar exacto del rol. Por ejemplo: `$filter=roleName%20eq%20'Virtual%20Machine%20Contributor'` |

### <a name="get-information-about-a-role"></a>Obtención de información acerca de un rol

Para obtener información sobre un rol usando el identificador de definición del rol, use la API de REST [Role Definitions - Get](/rest/api/authorization/roledefinitions/get). Para llamar a esta API, debe tener acceso a la operación `Microsoft.Authorization/roleDefinitions/read` en el ámbito. Se concede acceso a esta operación a varios [roles integrados](built-in-roles.md).

Para obtener información sobre un solo rol utilizando su nombre para mostrar, consulte la sección [Lista de roles](custom-roles-rest.md#list-roles) anterior.

1. Use la API de REST [Role Definitions - List](/rest/api/authorization/roledefinitions/list) para obtener el identificador GUID para el rol. Para roles integrados, también puede obtener el identificador de [Roles integrados](built-in-roles.md).

1. Empiece con la solicitud siguiente:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionId}?api-version=2015-07-01
    ```

1. En el identificador URI, reemplace *{scope}* por el ámbito cuya lista de roles quiere obtener.

    | Scope | Escriba |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Subscription |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Grupos de recursos |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Recurso |

1. Reemplace *{roleDefinitionId}* por el identificador GUID de la definición de roles.

## <a name="create-a-custom-role"></a>Crear un rol personalizado

Para crear un rol personalizado, use la API de REST [Role Definitions - Create Or Update](/rest/api/authorization/roledefinitions/createorupdate). Para llamar a esta API, debe tener acceso a la operación `Microsoft.Authorization/roleDefinitions/write` en todos los `assignableScopes`. Entre los roles integrados, solo se concede acceso a esta operación a [Propietario](built-in-roles.md#owner) y [Administrador de acceso de usuario](built-in-roles.md#user-access-administrator). 

1. Revise la lista de [operaciones de proveedores de recursos](resource-provider-operations.md) que están disponibles para crear los permisos para su rol personalizado.

1. Use una herramienta de GUID para generar un identificador único que se use para el identificador del rol personalizado. El identificador tiene el formato: `00000000-0000-0000-0000-000000000000`

1. Empiece con la solicitud y el cuerpo siguientes:

    ```http
    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionId}?api-version=2015-07-01
    ```

    ```json
    {
      "name": "{roleDefinitionId}",
      "properties": {
        "roleName": "",
        "description": "",
        "type": "CustomRole",
        "permissions": [
          {
            "actions": [
    
            ],
            "notActions": [
    
            ]
          }
        ],
        "assignableScopes": [
          "/subscriptions/{subscriptionId}"
        ]
      }
    }
    ```

1. Dentro del URI, reemplace *{scope}* por el primer elemento `assignableScopes` del rol personalizado.

    | Scope | Escriba |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Subscription |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Grupos de recursos |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Recurso |

1. Reemplace *{roleDefinitionId}* por el identificador GUID del rol personalizado.

1. Dentro del cuerpo de la solicitud, en la propiedad `assignableScopes`, reemplace *{roleDefinitionId}* por el identificador GUID.

1. Reemplace *{subscriptionId}* por el identificador de la suscripción.

1. En la propiedad `actions`, agregue las operaciones que permite realizar el rol.

1. En la propiedad `notActions`, agregue las operaciones excluidas de las `actions` permitidas.

1. En las propiedades `roleName` y `description`, especifique un nombre de rol exclusivo y una descripción. Para obtener información sobre las propiedades, consulte [Roles personalizados](custom-roles.md).

    A continuación se muestra un ejemplo de cuerpo de la solicitud:

    ```json
    {
      "name": "88888888-8888-8888-8888-888888888888",
      "properties": {
        "roleName": "Virtual Machine Operator",
        "description": "Can monitor and restart virtual machines.",
        "type": "CustomRole",
        "permissions": [
          {
            "actions": [
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
            "notActions": []
          }
        ],
        "assignableScopes": [
          "/subscriptions/00000000-0000-0000-0000-000000000000"
        ]
      }
    }
    ```

## <a name="update-a-custom-role"></a>Actualización de un rol personalizado

Para actualizar un rol personalizado, use la API de REST [Role Definitions - Create Or Update](/rest/api/authorization/roledefinitions/createorupdate). Para llamar a esta API, debe tener acceso a la operación `Microsoft.Authorization/roleDefinitions/write` en todos los `assignableScopes`. Entre los roles integrados, solo se concede acceso a esta operación a [Propietario](built-in-roles.md#owner) y [Administrador de acceso de usuario](built-in-roles.md#user-access-administrator). 

1. Para obtener información sobre el rol personalizado, use las API de REST [Role Definitions - List](/rest/api/authorization/roledefinitions/list) o [Role Definitions - Get](/rest/api/authorization/roledefinitions/get). Para más información, consulte la sección [Lista de roles](custom-roles-rest.md#list-roles) anterior.

1. Empiece con la solicitud siguiente:

    ```http
    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionId}?api-version=2015-07-01
    ```

1. Dentro del URI, reemplace *{scope}* por el primer elemento `assignableScopes` del rol personalizado.

    | Scope | Escriba |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Subscription |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Grupos de recursos |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Recurso |

1. Reemplace *{roleDefinitionId}* por el identificador GUID del rol personalizado.

1. Según la información sobre el rol personalizado, cree un cuerpo de la solicitud con el formato siguiente:

    ```json
    {
      "name": "{roleDefinitionId}",
      "properties": {
        "roleName": "",
        "description": "",
        "type": "CustomRole",
        "permissions": [
          {
            "actions": [
    
            ],
            "notActions": [
    
            ]
          }
        ],
        "assignableScopes": [
          "/subscriptions/{subscriptionId}"
        ]
      }
    }
    ```

1. Actualice el cuerpo de la solicitud con los cambios que desee hacer en el rol personalizado.

    A continuación se muestra un ejemplo de cuerpo de la solicitud con la nueva acción de configuración de diagnóstico agregada:

    ```json
    {
      "name": "88888888-8888-8888-8888-888888888888",
      "properties": {
        "roleName": "Virtual Machine Operator",
        "description": "Can monitor and restart virtual machines.",
        "type": "CustomRole",
        "permissions": [
          {
            "actions": [
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
            "notActions": []
          }
        ],
        "assignableScopes": [
          "/subscriptions/00000000-0000-0000-0000-000000000000"
        ]
      }
    }
    ```

## <a name="delete-a-custom-role"></a>Eliminación de un rol personalizado

Para eliminar un rol personalizado, use la API de REST [Role Definitions - Delete](/rest/api/authorization/roledefinitions/delete). Para llamar a esta API, debe tener acceso a la operación `Microsoft.Authorization/roleDefinitions/delete` en todos los `assignableScopes`. Entre los roles integrados, solo se concede acceso a esta operación a [Propietario](built-in-roles.md#owner) y [Administrador de acceso de usuario](built-in-roles.md#user-access-administrator). 

1. Para obtener el identificador GUID del rol personalizado, use las API de REST [Role Definitions - List](/rest/api/authorization/roledefinitions/list) o [Role Definitions - Get](/rest/api/authorization/roledefinitions/get). Para más información, consulte la sección [Lista de roles](custom-roles-rest.md#list-roles) anterior.

1. Empiece con la solicitud siguiente:

    ```http
    DELETE https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionId}?api-version=2015-07-01
    ```

1. En el identificador URI, reemplace *{scope}* por el ámbito que desea eliminar para el rol personalizado.

    | Scope | Escriba |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Subscription |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Grupos de recursos |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Recurso |

1. Reemplace *{roleDefinitionId}* por el identificador GUID del rol personalizado.

## <a name="next-steps"></a>Pasos siguientes

- [Roles personalizados en Azure](custom-roles.md)
- [Administración del acceso mediante RBAC y API REST](role-assignments-rest.md)
- [Azure REST API Reference](/rest/api/azure/) (Referencia de API de REST en Azure)