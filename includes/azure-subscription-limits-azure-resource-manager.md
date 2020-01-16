---
title: archivo de inclusión
description: archivo de inclusión
services: billing
author: rothja
ms.service: cost-management-billing
ms.topic: include
ms.date: 07/22/2019
ms.author: jroth
ms.custom: include file
ms.openlocfilehash: 2c5312c2e0a7121237c4d11197d90a2a5638557c
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/15/2020
ms.locfileid: "76021244"
---
| Resource | Límite predeterminado | Límite máximo |
| --- | --- | --- |
| Máquinas virtuales por [suscripción](../articles/billing-buy-sign-up-azure-subscription.md) |25 000<sup>1</sup> por región. |25 000 por región |
| Total de núcleos de máquina virtual por [suscripción](../articles/billing-buy-sign-up-azure-subscription.md) |20<sup>1</sup> por región. | Póngase en contacto con el servicio de soporte técnico. |
| Total de núcleos de máquina virtual de Azure Spot por [suscripción](../articles/billing-buy-sign-up-azure-subscription.md) |20<sup>1</sup> por región. | Póngase en contacto con el servicio de soporte técnico. |
| Máquina virtual por serie, como Dv2 y F, núcleos por [suscripción](../articles/billing-buy-sign-up-azure-subscription.md) |20<sup>1</sup> por región. | Póngase en contacto con el servicio de soporte técnico. |
| [Coadministradores](../articles/cost-management-billing/manage/add-change-subscription-administrator.md) por suscripción |Sin límite. |Sin límite. |
| [Cuentas de almacenamiento](../articles/storage/common/storage-account-create.md) por región por suscripción |250 |250 |
| [Grupos de recursos](../articles/azure-resource-manager/management/overview.md) por suscripción |980 |980 |
| [Conjuntos de disponibilidad](../articles/virtual-machines/windows/manage-availability.md#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy) por suscripción |2000 por región |2000 por región |
| Tamaño de solicitud de API de Azure Resource Manager |4 194 304 bytes. |4 194 304 bytes. |
| Etiquetas por suscripción<sup>2</sup> |Sin límite. |Sin límite. |
| Cálculos de etiquetas únicas por suscripción<sup>2</sup> | 10 000 | 10 000 |
| [Servicios en la nube](../articles/cloud-services/cloud-services-choose-me.md) por suscripción |N/A<sup>3</sup> |N/A<sup>3</sup> |
| [Grupos de afinidad](../articles/virtual-network/virtual-networks-migrate-to-regional-vnet.md) por suscripción |N/A<sup>3</sup> |N/A<sup>3</sup> |
| [Implementaciones de nivel de suscripción](../articles/azure-resource-manager/templates/deploy-to-subscription.md) por ubicación | 800<sup>4</sup> | 800 |

<sup>1</sup>Los límites predeterminados varían según el tipo de categoría de la oferta, por ejemplo, evaluación gratuita, pago por uso y por serie, como Dv2, F y G. Por ejemplo, el valor predeterminado de la suscripción Contrato Enterprise es 350.

<sup>2</sup>Puede aplicar un número ilimitado de etiquetas por suscripción. El número de etiquetas por recurso o grupo de recursos se limita a 50. Resource Manager devuelve una [lista de valores y nombres de etiqueta únicos](/rest/api/resources/tags) para la suscripción solo cuando el número de etiquetas es 10 000 o menos. Sin embargo, todavía puede encontrar un recurso por etiqueta cuando el número sea mayor que 10 000.  

<sup>3</sup>Estas características ya no son necesarias con los grupos de recursos de Azure y Resource Manager.

<sup>4</sup>Si se alcanza el límite de 800 implementaciones, elimine las implementaciones que ya no necesite del historial. Para eliminar implementaciones de nivel de suscripción, use [Remove-AzDeployment](/powershell/module/az.resources/Remove-AzDeployment) o [az deployment delete](/cli/azure/deployment?view=azure-cli-latest#az-deployment-delete).

> [!NOTE]
> Los núcleos de máquina virtual tienen un límite total regional. También tienen un límite para la serie regional por tamaño, como Dv2 y F. Estos límites se aplican por separado. Por ejemplo, considere una suscripción con un límite total de núcleos de máquinas virtuales de Este de EE. UU. de 30, un límite de núcleos de serie A de 30 y un límite de núcleos de serie D de 30. Esta suscripción puede implementar 30 máquinas virtuales A1 o 30 máquinas virtuales D1, o una combinación de ambas, con tal de que no supere un total de 30 núcleos. Ejemplo de una combinación es 10 máquinas virtuales A1 y 20 máquinas virtuales D1.  
> <!-- -->
> 
> 

