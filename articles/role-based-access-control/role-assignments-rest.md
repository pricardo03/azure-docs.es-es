---
title: 'Administración del acceso a recursos de Azure mediante RBAC y la API de REST: Azure | Microsoft Docs'
description: Aprenda a administrar el acceso a recursos de Azure de usuarios, grupos y aplicaciones mediante el control de acceso basado en rol (RBAC) y la API de REST. Esto incluye cómo enumerar el acceso, conceder acceso y quitar el acceso.
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
ms.openlocfilehash: c2ef9b0070cc9ac190b773f023ffc18d1b251a41
ms.sourcegitcommit: fcb674cc4e43ac5e4583e0098d06af7b398bd9a9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/18/2019
ms.locfileid: "56338417"
---
# <a name="manage-access-to-azure-resources-using-rbac-and-the-rest-api"></a>Administración del acceso a recursos de Azure mediante RBAC y la API de REST

El [control de acceso basado en rol (RBAC)](overview.md) es la forma en la que se administra el acceso a los recursos de Azure. En este artículo se describe cómo administrar el acceso de usuarios, grupos y aplicaciones mediante RBAC y la API REST.

## <a name="list-access"></a>Lista de acceso

En RBAC, para enumerar el acceso se enumeran las asignaciones de roles. Para mostrar la lista de asignación de roles, use una de las API de REST de [Asignación de roles - Lista](/rest/api/authorization/roleassignments/list). Para mejorar los resultados, especifique un ámbito y un filtro opcional. Para llamar a la API, debe tener acceso a la operación `Microsoft.Authorization/roleAssignments/read` en el ámbito especificado. Se concede acceso a esta operación a varios [roles integrados para recursos de Azure](built-in-roles.md).

1. Empiece con la solicitud siguiente:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$filter={filter}
    ```

1. En el identificador URI, reemplace *{scope}* por el ámbito cuya lista de asignaciones de roles quiere obtener.

    | Ámbito | Type |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Subscription |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Grupos de recursos |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Recurso |

1. Reemplace *{filter}* por la condición que quiere aplicar para filtrar la lista de asignación de roles.

    | Filtrar | DESCRIPCIÓN |
    | --- | --- |
    | `$filter=atScope()` | Lista de las asignaciones de roles únicamente para el ámbito especificado, sin incluir las asignaciones de roles en ámbitos secundarios. |
    | `$filter=principalId%20eq%20'{objectId}'` | Lista de las asignaciones de roles para un usuario, un grupo o una entidad de servicio determinados. |
    | `$filter=assignedTo('{objectId}')` | Lista de las asignaciones de roles para un usuario determinado, incluidas las que se heredaron de otros grupos. |

## <a name="grant-access"></a>Conceder acceso

En RBAC, para conceder acceso es preciso crear una asignación de roles. Para crear una asignación de rol, use la API de REST [Asignaciones de roles - Crear](/rest/api/authorization/roleassignments/create) y especifique la entidad de seguridad, la definición de roles y el ámbito. Para llamar a esta API, debe tener acceso a la operación `Microsoft.Authorization/roleAssignments/write`. Entre los roles integrados, solo se concede acceso a esta operación a [Propietario](built-in-roles.md#owner) y [Administrador de acceso de usuario](built-in-roles.md#user-access-administrator).

1. Use la API de REST [Definiciones de roles - Lista](/rest/api/authorization/roledefinitions/list) o consulte [Roles integrados](built-in-roles.md) para obtener el identificador de la definición de roles que quiere asignar.

1. Use una herramienta de GUID para generar un identificador único que se use para el identificador de asignación de roles. El identificador tiene el formato: `00000000-0000-0000-0000-000000000000`

1. Empiece con la solicitud y el cuerpo siguientes:

    ```http
    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{roleAssignmentName}?api-version=2015-07-01
    ```

    ```json
    {
      "properties": {
        "roleDefinitionId": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionId}",
        "principalId": "{principalId}"
      }
    }
    ```
    
1. En el identificador URI, reemplace *{scope}* por el ámbito de la asignación de roles.

    | Ámbito | Type |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Subscription |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Grupos de recursos |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Recurso |

1. Reemplace *{roleAssignmentName}* por el identificador GUID de la asignación de roles.

1. En el cuerpo de la solicitud, reemplace *{subscriptionId}* por el identificador de la suscripción.

1. Reemplace *{roleDefinitionId}* por el identificador de la definición de roles.

1. Reemplace *{principalId}* por el identificador de objeto del usuario, grupo o entidad de servicio al que se asignará el rol.

## <a name="remove-access"></a>Quitar acceso

En RBAC, para quitar el acceso hay que quitar una asignación de roles. Para quitar una asignación de roles, use la API de REST [Asignaciones de roles - Eliminar](/rest/api/authorization/roleassignments/delete). Para llamar a esta API, debe tener acceso a la operación `Microsoft.Authorization/roleAssignments/delete`. Entre los roles integrados, solo se concede acceso a esta operación a [Propietario](built-in-roles.md#owner) y [Administrador de acceso de usuario](built-in-roles.md#user-access-administrator).

1. Obtenga el identificador de asignación de roles (GUID). Este identificador se devuelve cuando se crea por primera vez la asignación de roles o puede obtenerlo enumerando las asignaciones de roles.

1. Empiece con la solicitud siguiente:

    ```http
    DELETE https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{roleAssignmentName}?api-version=2015-07-01
    ```

1. En el identificador URI, reemplace *{scope}* por el ámbito para eliminar la asignación de roles.

    | Ámbito | Type |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Subscription |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Grupos de recursos |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Recurso |

1. Reemplace *{roleAssignmentName}* por el identificador GUID de la asignación de roles.

## <a name="next-steps"></a>Pasos siguientes

- [Implementación de recursos con las plantillas de Resource Manager y la API de REST de Resource Manager](../azure-resource-manager/resource-group-template-deploy-rest.md)
- [Azure REST API Reference](/rest/api/azure/) (Referencia de API de REST en Azure)
- [Creación de roles personalizados para los recursos de Azure con la API de REST](custom-roles-rest.md)