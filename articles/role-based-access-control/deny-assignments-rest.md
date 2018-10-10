---
title: 'Enumeración de las asignaciones de denegación mediante RBAC y la API REST: Azure | Microsoft Docs'
description: Aprenda a enumerar las asignaciones de denegación para usuarios, grupos y aplicaciones mediante el control de acceso basado en rol (RBAC) y la API REST.
services: active-directory
documentationcenter: na
author: rolyon
manager: mtillman
editor: ''
ms.assetid: ''
ms.service: role-based-access-control
ms.workload: multiple
ms.tgt_pltfrm: rest-api
ms.devlang: na
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 44c1d3b18bb9bdc63247379fe3f277cb6542f2da
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46975594"
---
# <a name="list-deny-assignments-using-rbac-and-the-rest-api"></a>Enumeración de las asignaciones de denegación mediante RBAC y la API REST

Actualmente, las asignaciones de denegación son de **solo lectura** y únicamente pueden establecerse mediante Azure. Aunque no se pueden crear asignaciones de denegación propias, se pueden enumerar, ya que pueden afectar a los permisos vigentes. En este artículo se describe la enumeración de las asignaciones de denegación mediante RBAC y la API REST.

## <a name="list-a-single-deny-assignment"></a>Lista de una sola asignación de denegación

1. Empiece con la solicitud siguiente:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/denyAssignments/{deny-assignment-id}?api-version=2018-07-01-preview
    ```

1. En el identificador URI, reemplace *{scope}* por el ámbito cuya lista de asignaciones de denegación quiere obtener.

    | Ámbito | Escriba |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Subscription |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Grupos de recursos |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Recurso |

1. Reemplace *{deny-assignment-id}* por el identificador de asignación de denegación que desea recuperar.

## <a name="list-multiple-deny-assignments"></a>Lista de varias asignaciones de denegación

1. Comience con una de las siguientes solicitudes:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/denyAssignments?api-version=2018-07-01-preview
    ```

    Con parámetros opcionales:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/denyAssignments?api-version=2018-07-01-preview&$filter={filter}
    ```

1. En el identificador URI, reemplace *{scope}* por el ámbito cuya lista de asignaciones de denegación quiere obtener.

    | Ámbito | Escriba |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Subscription |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Grupos de recursos |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Recurso |

1. Reemplace *{filter}* por la condición que quiere aplicar para filtrar la lista de asignaciones de denegación.

    | Filtrar | DESCRIPCIÓN |
    | --- | --- |
    | (sin filtro) | Lista de todas las asignaciones de denegación del ámbito especificado, y también por encima y por debajo. |
    | `$filter=atScope()` | Lista de las asignaciones de denegación del ámbito especificado y por encima. No incluye las asignaciones de denegación de ámbitos secundarios. |
    | `$filter=denyAssignmentName%20eq%20'{deny-assignment-name}'` | Lista de asignaciones de denegación con el nombre especificado. |

## <a name="list-deny-assignments-at-the-root-scope-"></a>Enumeración de las asignaciones de denegación en el ámbito raíz (/)

1. Eleve el privilegio de acceso como se describe en [Elevación de los privilegios de acceso de un administrador global en Azure Active Directory](elevate-access-global-admin.md).

1. Use la siguiente solicitud:

    ```http
    GET https://management.azure.com/providers/Microsoft.Authorization/denyAssignments?api-version=2018-07-01-preview&$filter={filter}
    ```

1. Reemplace *{filter}* por la condición que quiere aplicar para filtrar la lista de asignaciones de denegación. Se requiere un filtro.

    | Filtrar | DESCRIPCIÓN |
    | --- | --- |
    | `$filter=atScope()` | Lista de asignaciones de denegación solo en el ámbito raíz. No incluye las asignaciones de denegación de ámbitos secundarios. |
    | `$filter=denyAssignmentName%20eq%20'{deny-assignment-name}'` | Lista de asignaciones de denegación con el nombre especificado. |

1. Elimine los privilegios de acceso elevados.

## <a name="next-steps"></a>Pasos siguientes

- [Descripción de las asignaciones de denegación](deny-assignments.md)
- [Elevación de los privilegios de acceso de un administrador global en Azure Active Directory](elevate-access-global-admin.md)
- [Azure REST API Reference](/rest/api/azure/) (Referencia de API de REST en Azure)
