---
title: Creación o actualización de roles personalizados para los recursos de Azure con la API REST
description: Obtenga información sobre cómo mostrar, crear, actualizar o eliminar roles personalizados con el control de acceso basado en rol (RBAC) para recursos de Azure con la API REST.
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
ms.date: 04/18/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 145bc45e1b7faeddc23cf5f0662337e15ab51c29
ms.sourcegitcommit: b95983c3735233d2163ef2a81d19a67376bfaf15
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/11/2020
ms.locfileid: "77137354"
---
# <a name="create-or-update-custom-roles-for-azure-resources-using-the-rest-api"></a>Creación o actualización de roles personalizados para los recursos de Azure con la API REST

Si los [roles integrados para los recursos de Azure](built-in-roles.md) no cumplen las necesidades específicas de su organización, puede crear sus propios roles personalizados. En este artículo se describe cómo mostrar, crear, actualizar o eliminar roles personalizados con la API REST.

## <a name="list-custom-roles"></a>Lista de roles personalizados

Para enumerar todos los roles personalizados de un directorio, use la API REST [Definiciones de roles: lista](/rest/api/authorization/roledefinitions/list).

1. Empiece con la solicitud siguiente:

    ```http
    GET https://management.azure.com/providers/Microsoft.Authorization/roleDefinitions?api-version=2015-07-01&$filter={filter}
    ```

1. Reemplace *{filter}* por el tipo de rol.

    | Filter | Descripción |
    | --- | --- |
    | `$filter=type%20eq%20'CustomRole'` | Filtro basado en el tipo CustomRole |

## <a name="list-custom-roles-at-a-scope"></a>Enumeración de roles personalizados en un ámbito

Para enumerar los roles personalizados en un ámbito, use la API REST [Definiciones de roles: lista](/rest/api/authorization/roledefinitions/list).

1. Empiece con la solicitud siguiente:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions?api-version=2015-07-01&$filter={filter}
    ```

1. En el identificador URI, reemplace *{scope}* por el ámbito cuya lista de roles quiere obtener.

    | Ámbito | Tipo |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Subscription |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Resource group |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Resource |

1. Reemplace *{filter}* por el tipo de rol.

    | Filter | Descripción |
    | --- | --- |
    | `$filter=type%20eq%20'CustomRole'` | Filtro basado en el tipo CustomRole |

## <a name="list-a-custom-role-definition-by-name"></a>Enumeración de una definición de rol personalizado por nombre

Para obtener información sobre el rol personalizado por su nombre para mostrar, use la API REST [Definiciones de roles: lista: obtener](/rest/api/authorization/roledefinitions/get).

1. Empiece con la solicitud siguiente:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions?api-version=2015-07-01&$filter={filter}
    ```

1. En el identificador URI, reemplace *{scope}* por el ámbito cuya lista de roles quiere obtener.

    | Ámbito | Tipo |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Subscription |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Resource group |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Resource |

1. Reemplace *{filter}* con el nombre para mostrar para el rol.

    | Filter | Descripción |
    | --- | --- |
    | `$filter=roleName%20eq%20'{roleDisplayName}'` | Use la forma con codificación URL del nombre para mostrar exacto del rol. Por ejemplo: `$filter=roleName%20eq%20'Virtual%20Machine%20Contributor'` |

## <a name="list-a-custom-role-definition-by-id"></a>Enumeración de una definición de rol personalizado por identificador

Para obtener información sobre un rol personalizado por su identificador único, use la API REST [Definiciones de roles: obtener](/rest/api/authorization/roledefinitions/get).

1. Use la API de REST [Role Definitions - List](/rest/api/authorization/roledefinitions/list) para obtener el identificador GUID para el rol.

1. Empiece con la solicitud siguiente:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionId}?api-version=2015-07-01
    ```

1. En el identificador URI, reemplace *{scope}* por el ámbito cuya lista de roles quiere obtener.

    | Ámbito | Tipo |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Subscription |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Resource group |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Resource |

1. Reemplace *{roleDefinitionId}* por el identificador GUID de la definición de roles.

## <a name="create-a-custom-role"></a>Crear un rol personalizado

Para crear un rol personalizado, use la API de REST [Role Definitions - Create Or Update](/rest/api/authorization/roledefinitions/createorupdate). Para llamar a esta API, debe haber iniciado sesión con un usuario que tenga asignado un rol que, a su vez, tenga el permiso `Microsoft.Authorization/roleDefinitions/write` en todos los `assignableScopes`. Entre los roles integrados, solamente [Propietario](built-in-roles.md#owner) y [Administrador de acceso de usuario](built-in-roles.md#user-access-administrator) incluyen este permiso.

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

    | Ámbito | Tipo |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Subscription |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Resource group |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Resource |

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
              "Microsoft.ResourceHealth/availabilityStatuses/read",
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

Para actualizar un rol personalizado, use la API de REST [Role Definitions - Create Or Update](/rest/api/authorization/roledefinitions/createorupdate). Para llamar a esta API, debe haber iniciado sesión con un usuario que tenga asignado un rol que, a su vez, tenga el permiso `Microsoft.Authorization/roleDefinitions/write` en todos los `assignableScopes`. Entre los roles integrados, solamente [Propietario](built-in-roles.md#owner) y [Administrador de acceso de usuario](built-in-roles.md#user-access-administrator) incluyen este permiso.

1. Para obtener información sobre el rol personalizado, use las API de REST [Role Definitions - List](/rest/api/authorization/roledefinitions/list) o [Role Definitions - Get](/rest/api/authorization/roledefinitions/get). Para más información, consulte la sección [Lista de roles personalizados](#list-custom-roles) anterior.

1. Empiece con la solicitud siguiente:

    ```http
    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionId}?api-version=2015-07-01
    ```

1. Dentro del URI, reemplace *{scope}* por el primer elemento `assignableScopes` del rol personalizado.

    | Ámbito | Tipo |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Subscription |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Resource group |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Resource |

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
              "Microsoft.ResourceHealth/availabilityStatuses/read",
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

Para eliminar un rol personalizado, use la API de REST [Role Definitions - Delete](/rest/api/authorization/roledefinitions/delete). Para llamar a esta API, debe haber iniciado sesión con un usuario que tenga asignado un rol que, a su vez, tenga el permiso `Microsoft.Authorization/roleDefinitions/delete` en todos los `assignableScopes`. Entre los roles integrados, solamente [Propietario](built-in-roles.md#owner) y [Administrador de acceso de usuario](built-in-roles.md#user-access-administrator) incluyen este permiso.

1. Para obtener el identificador GUID del rol personalizado, use las API de REST [Role Definitions - List](/rest/api/authorization/roledefinitions/list) o [Role Definitions - Get](/rest/api/authorization/roledefinitions/get). Para más información, consulte la sección [Lista de roles personalizados](#list-custom-roles) anterior.

1. Empiece con la solicitud siguiente:

    ```http
    DELETE https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionId}?api-version=2015-07-01
    ```

1. En el identificador URI, reemplace *{scope}* por el ámbito que desea eliminar para el rol personalizado.

    | Ámbito | Tipo |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Subscription |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Resource group |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Resource |

1. Reemplace *{roleDefinitionId}* por el identificador GUID del rol personalizado.

## <a name="next-steps"></a>Pasos siguientes

- [Roles personalizados en los recursos de Azure](custom-roles.md)
- [Administración del acceso a recursos de Azure mediante RBAC y la API de REST](role-assignments-rest.md)
- [Azure REST API Reference](/rest/api/azure/) (Referencia de API de REST en Azure)
