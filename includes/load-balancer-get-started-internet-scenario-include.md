---
author: WenJason
ms.service: load-balancer
ms.topic: include
origin.date: 11/09/2018
ms.date: 12/31/2018
ms.author: v-jay
ms.openlocfilehash: bf3aee30460e052db23cf9090f13e2af3b22628b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60516977"
---
En este escenario se realizarán las siguientes tareas:

* Crear un equilibrador de carga que reciba tráfico de red en el puerto 80 y envíe tráfico de carga equilibrada a las máquinas virtuales "web1" y "web2".
* Crear reglas NAT para el acceso de escritorio remoto / SSH para máquinas virtuales detrás del equilibrador de carga
* Crear sondeos de estado

![Escenario del equilibrador de carga](./media/load-balancer-get-started-internet-scenario-include/scenario-classic.png)