---
author: rothja
ms.service: billing
ms.topic: include
ms.date: 11/09/2018
ms.author: jroth
ms.openlocfilehash: 2b6ba1dcddd435c42ad864b8e87175d0e98c9b3a
ms.sourcegitcommit: 8d88a025090e5087b9d0ab390b1207977ef4ff7c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2018
ms.locfileid: "52279938"
---
| Recurso | Límite predeterminado | Límite máximo |
| --- | --- | --- |
| [Roles web y de trabajo por implementación](../articles/cloud-services/cloud-services-choose-me.md)<sup>1</sup> |25 |25 |
| [Extremos de entrada de instancia](https://msdn.microsoft.com/library/gg557552.aspx#InstanceInputEndpoint) por implementación |25 |25 |
| [Extremos de entrada](https://msdn.microsoft.com/library/gg557552.aspx#InputEndpoint) por implementación |25 |25 |
| [Extremos internos](https://msdn.microsoft.com/library/gg557552.aspx#InternalEndpoint) por implementación |25 |25 |

<sup>1</sup>Cada servicio en la nube con roles web y de trabajo puede tener dos implementaciones, una para producción y otra para ensayo. Tenga en cuenta también que este límite hace referencia al número de roles (configuración) y no al número de instancias por rol (escalado).

