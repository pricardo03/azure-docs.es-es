---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines-windows
author: cynthn
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 05/17/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: d9c8a0e6a3bd6d79a11ee0d0dab0500a209e5571
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2018
ms.locfileid: "34684989"
---
En Azure, se abre un puerto o se crea un punto de conexión a una máquina virtual creando un filtro de red en una subred o una interfaz de red de máquina virtual. Estos filtros, que controlan el tráfico entrante y saliente, se colocan en un grupo de seguridad de red y se asocian al recurso que va a recibir dicho tráfico.

Vamos a usar un ejemplo común de tráfico web en el puerto 80. Una vez que tenga una máquina virtual configurada para atender solicitudes web en el puerto TCP estándar 80 (recuerde iniciar los servicios apropiados y abrir también las reglas del firewall del sistema operativo en la máquina virtual), tendrá que realizar estas acciones:

1. Crear un grupo de seguridad de red.
2. Crear una regla de entrada que permita el tráfico con la siguiente configuración:
   * El valor del intervalo de puertos de destino debe ser 80.
   * El intervalo de puerto de origen tiene que ser "*" (para permitir cualquier puerto de origen).
   * Un valor de prioridad inferior a 65 500 (para que tenga mayor prioridad que la regla de entrada predeterminada de denegación de comodín).
3. Asociar el grupo de seguridad de red con la subred o la interfaz de red de máquina virtual.

Puede crear configuraciones de red complejas para proteger su entorno mediante reglas y grupos de seguridad de red. Nuestro ejemplo solo utiliza una o dos reglas que permiten el tráfico HTTP o la administración remota. Para más información, consulte la sección [Más información acerca de los grupos de seguridad de red](#more-information-on-network-security-groups) o el artículo [¿Qué es un grupo de seguridad de red?](../articles/virtual-network/security-overview.md)

