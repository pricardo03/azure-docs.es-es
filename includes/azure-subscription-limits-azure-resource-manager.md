---
title: archivo de inclusión
description: archivo de inclusión
services: azure-resource-manager
author: tfitzmac
ms.service: cost-management-billing
ms.topic: include
ms.date: 02/10/2020
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: b7db49d09d2292c08ce33ce86b1b7f427ef75fbc
ms.sourcegitcommit: f15f548aaead27b76f64d73224e8f6a1a0fc2262
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/26/2020
ms.locfileid: "77618483"
---
| Resource | Límite predeterminado | Límite máximo |
| --- | --- | --- |
| Suscripciones por inquilino de Azure Active Directory | Sin límite. | Sin límite. |
| [Coadministradores](../articles/cost-management-billing/manage/add-change-subscription-administrator.md) por suscripción |Sin límite. |Sin límite. |
| [Grupos de recursos](../articles/azure-resource-manager/management/overview.md) por suscripción |980 |980 |
| Tamaño de solicitud de API de Azure Resource Manager |4 194 304 bytes. |4 194 304 bytes. |
| Etiquetas por suscripción<sup>1</sup> |Sin límite. |Sin límite. |
| Cálculos de etiquetas únicas por suscripción<sup>1</sup> | 10 000 | 10 000 |
| [Implementaciones de nivel de suscripción](../articles/azure-resource-manager/templates/deploy-to-subscription.md) por ubicación | 800<sup>2</sup> | 800 |

<sup>1</sup>Puede aplicar un número ilimitado de etiquetas por suscripción. El número de etiquetas por recurso o grupo de recursos se limita a 50. Resource Manager devuelve una [lista de valores y nombres de etiqueta únicos](/rest/api/resources/tags) para la suscripción solo cuando el número de etiquetas es 10 000 o menos. Sin embargo, todavía puede encontrar un recurso por etiqueta cuando el número sea mayor que 10 000.  

<sup>2</sup>Si se alcanza el límite de 800 implementaciones, elimine las implementaciones que ya no necesite del historial. Para eliminar implementaciones de nivel de suscripción, use [Remove-AzDeployment](/powershell/module/az.resources/Remove-AzDeployment) o [az deployment delete](/cli/azure/deployment?view=azure-cli-latest#az-deployment-delete).
