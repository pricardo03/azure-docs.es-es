---
author: dlepow
ms.service: container-instances
ms.topic: include
ms.date: 02/13/2019
ms.author: danlep
ms.openlocfilehash: 055fbc652d0d72925cccfae40efff8b8eef2a8c2
ms.sourcegitcommit: 3eb0cc8091c8e4ae4d537051c3265b92427537fe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2020
ms.locfileid: "75904444"
---
| Resource | Límite predeterminado |
| --- | :--- |
| Grupos de contenedores de SKU estándar por región por [suscripción](../articles/billing-buy-sign-up-azure-subscription.md) | 100<sup>1</sup> |
| Grupos de contenedores de SKU dedicado por región por [suscripción](../articles/billing-buy-sign-up-azure-subscription.md) | 0<sup>1</sup> |
| Número de contenedores por grupo de contenedor | 60 |
| Número de volúmenes por grupo de contenedor | 20 |
| Puertos por IP | 5 |
| Tamaño del registro de instancia de contenedor: instancia en ejecución | 4 MB |
| Tamaño del registro de instancia de contenedor: instancia detenida | 16 KB o 1000 líneas |
| Creaciones de contenedores cada hora |300<sup>1</sup> |
| Creaciones de contenedores cada 5 minutos | 100<sup>1</sup> |
| Eliminaciones de contenedores cada hora | 300<sup>1</sup> |
| Eliminaciones de contenedores cada 5 minutos | 100<sup>1</sup> |


<sup>1</sup>Para solicitar un aumento del límite, cree una [solicitud de soporte técnico de Azure][azure-support].<br />

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
