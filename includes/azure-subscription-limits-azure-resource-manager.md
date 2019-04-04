---
title: archivo de inclusión
description: archivo de inclusión
services: billing
author: rothja
ms.service: billing
ms.topic: include
ms.date: 04/02/2019
ms.author: jroth
ms.custom: include file
ms.openlocfilehash: d490cab4d437c30fdb211ea27397777afc27e72e
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2019
ms.locfileid: "58891095"
---
| Recurso | Límite predeterminado | Límite máximo |
| --- | --- | --- |
| Máquinas virtuales por [suscripción](../articles/billing-buy-sign-up-azure-subscription.md) |25 000<sup>1</sup> por región. |25 000 por región. |
| Total de núcleos de máquina virtual por [suscripción](../articles/billing-buy-sign-up-azure-subscription.md) |20<sup>1</sup> por región. | Póngase en contacto con el servicio de soporte técnico. |
| Máquina virtual por serie, como Dv2 y F, núcleos por [suscripción](../articles/billing-buy-sign-up-azure-subscription.md) |20<sup>1</sup> por región. | Póngase en contacto con el servicio de soporte técnico. |
| [A los coadministradores](../articles/billing-add-change-azure-subscription-administrator.md) por suscripción |Sin límite. |Sin límite. |
| [Cuentas de almacenamiento](../articles/storage/common/storage-quickstart-create-account.md) por región por suscripción |250 |250 |
| [Grupos de recursos](../articles/azure-resource-manager/resource-group-overview.md) por suscripción |980 |980 |
| [Conjuntos de disponibilidad](../articles/virtual-machines/windows/manage-availability.md#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy) por suscripción |2000 por región. |2000 por región. |
| Tamaño de solicitud de API de Resource Manager de Azure |4,194,304 bytes. |4,194,304 bytes. |
| Etiquetas por suscripción<sup>2</sup> |Sin límite. |Sin límite. |
| Cálculos de etiquetas únicas por suscripción<sup>2</sup> | 10 000 | 10 000 |
| [Servicios en la nube](../articles/cloud-services/cloud-services-choose-me.md) por suscripción |N/A<sup>3</sup> |N/A<sup>3</sup> |
| [Grupos de afinidad](../articles/virtual-network/virtual-networks-migrate-to-regional-vnet.md) por suscripción |N/A<sup>3</sup> |N/A<sup>3</sup> |
| [Las implementaciones de nivel de suscripción](../articles/azure-resource-manager/deploy-to-subscription.md) por ubicación | 800 | 800 |

<sup>1</sup>límites predeterminados varían según el tipo de categoría de oferta, por ejemplo, la evaluación gratuita y de pago por uso y por serie, como Dv2, F y G.

<sup>2</sup>puede aplicar un número ilimitado de etiquetas por suscripción. El máximo de etiquetas por recurso o grupo de recursos es 15. Resource Manager devuelve un [lista de valores y nombres de etiqueta únicos](/rest/api/resources/tags) en la suscripción solo cuando el número de etiquetas es 10 000 o menos. Todavía puede encontrar un recurso por etiqueta cuando el número mayor que 10 000.  

<sup>3</sup>estas características ya no son necesarias con el Administrador de recursos y grupos de recursos de Azure.

> [!NOTE]
> Núcleos de máquina virtual tienen un límite total regional. También tienen un límite para regional serie por tamaño, como Dv2 y f el. Estos límites se aplican por separado. Por ejemplo, considere una suscripción con un límite total de núcleos de máquinas virtuales de Este de EE. UU. de 30, un límite de núcleos de serie A de 30 y un límite de núcleos de serie D de 30. Esta suscripción puede implementar 30 máquinas virtuales A1, o 30 máquinas virtuales D1 o una combinación de ambos, para que no supere un total de 30 núcleos. Un ejemplo de una combinación es de 10 máquinas virtuales A1 y 20 máquinas virtuales D1.  
> <!-- -->
> 
> 

