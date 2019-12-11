---
title: Lista de asignaciones de roles con RBAC de Azure y la API REST
description: Aprenda a determinar a qué recursos tienen acceso los usuarios, grupos, entidades de servicio e identidades administradas mediante el control de acceso basado en rol (RBAC) y la API REST.
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
ms.date: 11/25/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: e20edcb5e2406c216711a2e0f696ef06e19fe21e
ms.sourcegitcommit: c69c8c5c783db26c19e885f10b94d77ad625d8b4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2019
ms.locfileid: "74709884"
---
# <a name="list-role-assignments-using-azure-rbac-and-the-rest-api"></a>Lista de asignaciones de roles con RBAC de Azure y la API REST

[!INCLUDE [Azure RBAC definition list access](../../includes/role-based-access-control-definition-list.md)] En este artículo se describe cómo enumerar las asignaciones de roles mediante la API REST.

## <a name="list-role-assignments"></a>Lista de asignaciones de roles

En RBAC, para enumerar el acceso se enumeran las asignaciones de roles. Para mostrar la lista de asignación de roles, use una de las API de REST de [Asignación de roles - Lista](/rest/api/authorization/roleassignments/list). Para mejorar los resultados, especifique un ámbito y un filtro opcional.

1. Empiece con la solicitud siguiente:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$filter={filter}
    ```

1. En el identificador URI, reemplace *{scope}* por el ámbito cuya lista de asignaciones de roles quiere obtener.

    | Ámbito | type |
    | --- | --- |
    | `providers/Microsoft.Management/managementGroups/{groupId1}` | Grupo de administración |
    | `subscriptions/{subscriptionId1}` | Subscription |
    | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1` | Resource group |
    | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Resource |

    En el ejemplo anterior, microsoft.web es un proveedor de recursos que hace referencia a una instancia de App Service. De forma similar, puede usar cualquier otro proveedor de recursos y especificar el ámbito. Para más información, consulte [Tipos y proveedores de recursos de Azure](../azure-resource-manager/resource-manager-supported-services.md) y [Operaciones del proveedor de recursos de Azure Resource Manager](resource-provider-operations.md) compatibles.  
     
1. Reemplace *{filter}* por la condición que quiere aplicar para filtrar la lista de asignación de roles.

    | Filter | DESCRIPCIÓN |
    | --- | --- |
    | `$filter=atScope()` | Lista de las asignaciones de roles únicamente para el ámbito especificado, sin incluir las asignaciones de roles en ámbitos secundarios. |
    | `$filter=principalId%20eq%20'{objectId}'` | Lista de las asignaciones de roles para un usuario, un grupo o una entidad de servicio determinados. |
    | `$filter=assignedTo('{objectId}')` | Lista de las asignaciones de roles para un usuario o una entidad de servicio determinados. Si el usuario es miembro de un grupo que tiene una asignación de roles, también se muestra esa asignación de roles. Este filtro es transitivo para los grupos, lo que significa que, si el usuario es miembro de un grupo y ese grupo es miembro de otro grupo que tiene una asignación de roles, también se muestra esa asignación de roles. Este filtro solo acepta un identificador de objeto para un usuario o una entidad de servicio. No se puede pasar un identificador de objeto para un grupo. |

## <a name="next-steps"></a>Pasos siguientes

- [Incorporación o eliminación de asignaciones de roles con RBAC de Azure y la API REST](role-assignments-rest.md)
- [Azure REST API Reference](/rest/api/azure/) (Referencia de API de REST en Azure)
