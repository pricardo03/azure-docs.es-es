---
author: dlepow
ms.service: container-instances
ms.topic: include
ms.date: 11/09/2018
ms.author: danlep
ms.openlocfilehash: 44bdaec78e1fad574f29a5945b07041b588aaff8
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2018
ms.locfileid: "51572706"
---
| Recurso | Límite predeterminado |
| --- | :--- |
| Grupos de contenedor por [suscripción](../articles/billing-buy-sign-up-azure-subscription.md) | 100<sup>1</sup> |
| Número de contenedores por grupo de contenedor | 60 |
| Número de volúmenes por grupo de contenedor | 20 |
| Puertos por IP | 5 |
| Creaciones de contenedores cada hora |300<sup>1</sup> |
| Creaciones de contenedores cada 5 minutos | 100<sup>1</sup> |
| Eliminaciones de contenedores cada hora | 300<sup>1</sup> |
| Eliminaciones de contenedores cada 5 minutos | 100<sup>1</sup> |
| Varios contenedores por grupo de contenedor | Solamente para Linux<sup>2</sup> |
| Volúmenes de Azure Files | Solamente para Linux<sup>2</sup> |
| Volúmenes de GitRepo | Solamente para Linux<sup>2</sup> |
| Volúmenes secretos | Solamente para Linux<sup>2</sup> |

<sup>1</sup> Crear una [solicitud de soporte técnico de Azure][azure-support] para solicitar un aumento del límite.<br />
<sup>2</sup> La compatibilidad de Windows para esta característica está planeada.

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
