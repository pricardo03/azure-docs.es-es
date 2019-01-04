---
author: rothja
ms.service: billing
ms.topic: include
ms.date: 11/09/2018
ms.author: jroth
ms.openlocfilehash: eb1fe7f83ed83efe078be0aadf26cc7db6f498e2
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2018
ms.locfileid: "52886234"
---
| Recurso | Límite predeterminado | Límite máximo |
| --- | --- | --- |
| [Roles web y de trabajo por implementación](../articles/cloud-services/cloud-services-choose-me.md)<sup>1</sup> |25 |25 |
| [Extremos de entrada de instancia](https://msdn.microsoft.com/library/gg557552.aspx#InstanceInputEndpoint) por implementación |25 |25 |
| [Extremos de entrada](https://msdn.microsoft.com/library/gg557552.aspx#InputEndpoint) por implementación |25 |25 |
| [Extremos internos](https://msdn.microsoft.com/library/gg557552.aspx#InternalEndpoint) por implementación |25 |25 |
| [Certificados de servicio hospedados](../articles/cloud-services/cloud-services-certs-create.md#what-are-service-certificates) por implementación |199 |199 |

<sup>1</sup>Cada servicio en la nube con roles web y de trabajo puede tener dos implementaciones, una para producción y otra para ensayo. Tenga en cuenta también que este límite hace referencia al número de roles (configuración) y no al número de instancias por rol (escalado).

