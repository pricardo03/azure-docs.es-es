---
title: Vista previa de Escritorio Virtual Windows host grupo equilibrio de carga métodos - Azure
description: Grupo equilibrio de carga métodos de host para un entorno de Windows Vista previa del escritorio Virtual.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 03/21/2019
ms.author: helohr
ms.openlocfilehash: 8b18224339654c067d8ab9b543fa49a9c7d55ddd
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/25/2019
ms.locfileid: "58400176"
---
# <a name="host-pool-load-balancing-methods"></a>Métodos de equilibrio de carga de grupo host

Vista previa de Escritorio Virtual de Windows admite dos métodos de equilibrio de carga. Cada método determina qué host de sesión va a hospedar una sesión de usuario cuando se conectan a un recurso en un grupo host.

Los siguientes métodos de equilibrio de carga están disponibles en el escritorio Virtual de Windows:

- Equilibrio de carga de amplitud le permite distribuir uniformemente las sesiones de usuario en los hosts de sesión en un grupo host.
- Equilibrio de carga de prioridad de profundidad permite saturar un host de sesión con las sesiones de usuario en un grupo host. Una vez que la primera sesión alcanza su umbral de límite de sesión, el equilibrador de carga dirige las conexiones de usuario nuevo en el siguiente host de sesión en el grupo host hasta que alcanza su límite y así sucesivamente.

Cada grupo host sólo puede configurar un tipo de equilibrio de carga específico para la aplicación. Sin embargo, ambas comparten de métodos de equilibrio de carga los comportamientos siguientes, independientemente de qué grupo de host que se encuentren en:

- Si un usuario ya tiene una sesión en el grupo host y se vuelve a conectar a esa sesión, el equilibrador de carga le redirigirá correctamente para el host de sesión con su sesión existente. Este comportamiento se aplica incluso si la propiedad AllowNewConnections de ese host sesión se establece en False.
- Si un usuario ya no tiene una sesión en el grupo host, a continuación, el equilibrador de carga no considere la posibilidad de hosts de sesión cuya propiedad AllowNewConnections se establece en False durante el equilibrio de carga.

## <a name="breadth-first-load-balancing-method"></a>Método de equilibrio de carga de amplitud

El método de equilibrio de carga de amplitud permite distribuir las conexiones de usuario para optimizar este escenario. Este método es ideal para las organizaciones que desean proporcionar la mejor experiencia para los usuarios se conectan a su entorno de escritorio virtual agrupado.

El método de amplitud consulta primero los hosts de sesión que permiten nuevas conexiones. El método, a continuación, selecciona el host de sesión con el menor número de sesiones. Si hay un empate, el método selecciona el primer host de sesión de la consulta.

## <a name="depth-first-load-balancing-method"></a>Método de equilibrio de carga de prioridad de profundidad

El método de equilibrio de carga de prioridad de profundidad permite saturar un host de sesión a la vez para optimizar este escenario. Este método es ideal para las organizaciones de costos que desean tener más control sobre el número de máquinas virtuales que han asignado para un grupo host.

El método de prioridad de profundidad consulta primero los hosts de sesión que se permiten nuevas conexiones y aún no ha excedido su límite máximo de la sesión. El método, a continuación, selecciona el host de sesión con el número máximo de sesiones. Si hay un empate, el método selecciona el primer host de sesión de la consulta.