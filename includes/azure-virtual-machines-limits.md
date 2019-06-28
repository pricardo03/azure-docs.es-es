---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 11/09/2018
ms.author: cynthn
ms.openlocfilehash: 083d59a2d8720a08abc7ea8998c1fbe048db3f28
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/18/2019
ms.locfileid: "67186673"
---
| Recurso | Límite predeterminado | Límite máximo |
| --- | --- | --- |
| [Máquinas virtuales](../articles/virtual-machines/virtual-machines-linux-about.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) por servicio en la nube<sup>1</sup> |50 |50 |
| Extremos de entrada por servicio en la nube<sup>2</sup> |150 |150 |

<sup>1</sup>Las máquinas virtuales creadas mediante el modelo de implementación clásico, en lugar de Azure Resource Manager, se almacenan automáticamente en un servicio en la nube. Puede agregar más máquinas virtuales a ese servicio en la nube para usarlas para el equilibrio de carga y la disponibilidad. 

<sup>2</sup>Los extremos de entrada permiten establecer comunicación con una máquina virtual desde fuera del servicio en la nube de la máquina virtual. Las máquinas virtuales en el mismo servicio en la nube o red virtual pueden comunicarse automáticamente entre sí. Para más información, consulte [Configuración de puntos de conexión en una máquina virtual](../articles/virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json). 
