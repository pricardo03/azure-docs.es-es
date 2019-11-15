---
author: dlepow
ms.service: container-instances
ms.topic: include
ms.date: 02/13/2019
ms.author: danlep
ms.openlocfilehash: 3608eb6182f7806c0d4df8b96aa69faac68928f1
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73591402"
---
| Resource | Límite predeterminado |
| --- | :--- |
| Grupos de contenedor por región por [suscripción](../articles/billing-buy-sign-up-azure-subscription.md) | 100<sup>1</sup> |
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
