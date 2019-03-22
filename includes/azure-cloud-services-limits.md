---
author: rothja
ms.service: billing
ms.topic: include
ms.date: 11/09/2018
ms.author: jroth
ms.openlocfilehash: acf5a7e8d7feebaac3d82629c5b561a22112de0f
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/07/2019
ms.locfileid: "57553988"
---
| Recurso | Límite predeterminado | Límite máximo |
| --- | --- | --- |
| [Roles Web o de trabajo por implementación](../articles/cloud-services/cloud-services-choose-me.md)<sup>1</sup> |25 |25 |
| [Los puntos de conexión de entrada de instancia](https://msdn.microsoft.com/library/gg557552.aspx#InstanceInputEndpoint) por implementación |25 |25 |
| [Extremos de entrada](https://msdn.microsoft.com/library/gg557552.aspx#InputEndpoint) por implementación |25 |25 |
| [Los extremos internos](https://msdn.microsoft.com/library/gg557552.aspx#InternalEndpoint) por implementación |25 |25 |
| [Certificados de servicio hospedados](../articles/cloud-services/cloud-services-certs-create.md#what-are-service-certificates) por implementación |199 |199 |

<sup>1</sup>cada servicio en la nube de Azure con roles web o de trabajo puede tener dos implementaciones, uno para producción y otro para el almacenamiento provisional. Este límite se refiere al número de roles diferentes, es decir, la configuración. Este límite no hace referencia al número de instancias por rol, es decir, el escalado.

