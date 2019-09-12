---
author: rothja
ms.service: billing
ms.topic: include
ms.date: 11/09/2018
ms.author: jroth
ms.openlocfilehash: 0b24688b502a40e722d2fcc4436ff1824862f489
ms.sourcegitcommit: 23389df08a9f4cab1f3bb0f474c0e5ba31923f12
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "67186689"
---
| Recurso | Límite predeterminado | Límite máximo |
| --- | --- | --- |
| [Roles web o de trabajo por implementación](../articles/cloud-services/cloud-services-choose-me.md)<sup>1</sup> |25 |25 |
| [Puntos de conexión de entrada de instancia](/previous-versions/azure/reference/gg557552(v=azure.100)#instanceinputendpoint) por implementación |25 |25 |
| [Puntos de conexión de entrada](/previous-versions/azure/reference/gg557552(v=azure.100)#inputendpoint) por implementación |25 |25 |
| [Puntos de conexión internos](/previous-versions/azure/reference/gg557552(v=azure.100)#internalendpoint) por implementación |25 |25 |
| [Certificados de servicio hospedados](../articles/cloud-services/cloud-services-certs-create.md#what-are-service-certificates) por implementación |199 |199 |

<sup>1</sup>Cada servicio en la nube de Azure con roles web o de trabajo puede tener dos implementaciones, una para producción y otra para ensayo. Este límite se refiere al número de roles diferentes, es decir, la configuración. No hace referencia al número de instancias por rol, es decir, el escalado.

