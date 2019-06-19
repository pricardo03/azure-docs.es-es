---
author: kumudD
ms.service: load-balancer
ms.topic: include
ms.date: 11/09/2018
ms.author: kumud
ms.openlocfilehash: 459c99d1b45af9c98cc1a6d0d7dd7a9a04c824ec
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/18/2019
ms.locfileid: "67186737"
---
Azure Load Balancer es un equilibrador de carga de nivel 4 (TCP y UDP) que distribuye proporcionando una alta disponibilidad el tráfico entrante entre las instancias de servicio correctas de los servicios en la nube o las máquinas virtuales de un conjunto de carga equilibrada. Azure Load Balancer también pueden presentar prestar servicios en varios puertos, varias direcciones IP o ambos.

Puede configurar un equilibrador de carga para los siguientes objetivos:

* Equilibrar la carga del tráfico entrante de Internet a las máquinas virtuales (VM). En este escenario hacemos referencia a un equilibrador de carga como un [equilibrador de carga con conexión a Internet](../articles/load-balancer/load-balancer-internet-overview.md).
* El tráfico de equilibrio de carga entre máquinas virtuales de una red virtual (VNet), las máquinas virtuales de los servicios en la nube o entre equipos locales y máquinas virtuales en una red virtual entre entornos. En este escenario hacemos referencia a un equilibrador de carga como un [equilibrador de carga interno (ILB)](../articles/load-balancer/load-balancer-internal-overview.md).
* Enrutar el tráfico externo a una instancia específica de máquina virtual.
