---
author: rothja
ms.service: billing
ms.topic: include
ms.date: 11/09/2018
ms.author: jroth
ms.openlocfilehash: 0b24688b502a40e722d2fcc4436ff1824862f489
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2019
ms.locfileid: "58919349"
---
| Recurso | Límite predeterminado | Límite máximo |
| --- | --- | --- |
| [Roles Web o de trabajo por implementación](../articles/cloud-services/cloud-services-choose-me.md)<sup>1</sup> |25 |25 |
| [Los puntos de conexión de entrada de instancia](/previous-versions/azure/reference/gg557552(v=azure.100)#instanceinputendpoint) por implementación |25 |25 |
| [Extremos de entrada](/previous-versions/azure/reference/gg557552(v=azure.100)#inputendpoint) por implementación |25 |25 |
| [Los extremos internos](/previous-versions/azure/reference/gg557552(v=azure.100)#internalendpoint) por implementación |25 |25 |
| [Certificados de servicio hospedados](../articles/cloud-services/cloud-services-certs-create.md#what-are-service-certificates) por implementación |199 |199 |

<sup>1</sup>cada servicio en la nube de Azure con roles web o de trabajo puede tener dos implementaciones, uno para producción y otro para el almacenamiento provisional. Este límite se refiere al número de roles diferentes, es decir, la configuración. Este límite no hace referencia al número de instancias por rol, es decir, el escalado.

