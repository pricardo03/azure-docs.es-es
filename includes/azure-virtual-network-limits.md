---
title: archivo de inclusión
description: archivo de inclusión
services: networking
author: jimdial
ms.service: networking
ms.topic: include
ms.date: 06/20/2018
ms.author: jdial
ms.custom: include file
ms.openlocfilehash: 9ba9bc993832350f6b6ce1c642e2dc852731b6f0
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/13/2018
ms.locfileid: "39029950"
---
<a name="virtual-networking-limits-classic"></a>Los límites siguientes se aplican solo a los recursos de redes administrados a través del modelo de implementación clásico por suscripción. Aprenda a [ver el uso de recursos actual comparado con los límites de su suscripción](../articles/networking/check-usage-against-limits.md).

| Recurso | Límite predeterminado | Límite máximo |
| --- | --- | --- |
| Redes virtuales |50 |100 |
| Sitios de red local |20 |ponerse en contacto con el servicio de soporte técnico |
| Servidores DNS por red virtual |20 |100 |
| Direcciones IP privadas por red virtual |4096 |4096 |
| Conexiones TCP concurrentes o flujo UDP por NIC de una máquina virtual o instancia de rol |500 KB |500 KB |
| Grupos de seguridad de red (NSG) |100 |200 |
| Reglas de NSG por NSG |200 |1000 |
| Tablas de rutas definidas por el usuario |100 |200 |
| Rutas definidas por el usuario por tabla de ruta |100 |400 |
| Direcciones IP públicas (dinámicas) |5 |ponerse en contacto con el servicio de soporte técnico |
| Direcciones IP públicas reservadas |20 |ponerse en contacto con el servicio de soporte técnico |
| VIP pública por implementación |5 |ponerse en contacto con el servicio de soporte técnico |
| VIP privada (ILB) por implementación |1 |1 |
| Listas de control de acceso (ACL) de extremo |50 |50 |

#### <a name="azure-resource-manager-virtual-networking-limits"></a>Límites de redes - Azure Resource Manager
Los límites siguientes solo se aplican a los recursos de redes administrados a través de Azure Resource Manager por región y por suscripción. Aprenda a [ver el uso de recursos actual comparado con los límites de su suscripción](../articles/networking/check-usage-against-limits.md).

| Recurso | Límite predeterminado | Límite máximo |
| --- | --- | --- |
| Redes virtuales |50 |1000 |
| Subredes por red virtual |1000 |10000 |
| Emparejamientos de redes virtuales por red virtual |50** |100 |
| Servidores DNS por red virtual |9 |25 |
| Direcciones IP privadas por red virtual |16384** |16384 |
| Direcciones IP privadas por interfaz de red |256 |256 |
| Conexiones TCP concurrentes o flujo UDP por NIC de una máquina virtual o instancia de rol |500 KB |500 KB |
| Interfaces de red (NIC) |24000** |24000 |
| Grupos de seguridad de red (NSG) |100 |5000 |
| Reglas de NSG por NSG |1000** |1000 |
| Direcciones IP y rangos especificados para el origen o destino en un grupo de seguridad |2000 |4000 |
| Grupos de seguridad de aplicaciones |500 |3000 |
| Grupos de seguridad de aplicaciones por configuración de IP, por NIC |10 |20 |
| Configuraciones de IP por grupo de seguridad de aplicaciones |1000 |4000 |
| Grupos de seguridad de aplicaciones que se pueden especificar en todas las reglas de seguridad de un grupo de seguridad de red |50 |100 |
| Tablas de rutas definidas por el usuario |100 |200 |
| Rutas definidas por el usuario por tabla de ruta |400** |400 |
| Direcciones IP públicas - dinámicas |(Básico) 60 |ponerse en contacto con el servicio de soporte técnico |
| Direcciones IP públicas - estáticas |(Básico) 20 |ponerse en contacto con el servicio de soporte técnico |
| Direcciones IP públicas - estáticas |(Estándar) 20 |ponerse en contacto con el servicio de soporte técnico |
| Certificados raíz de punto a sitio por VPN Gateway |20 |20 |

**Estos límites predeterminados actualizados se aplican a las suscripciones cuyos límites no habían aumentado anteriormente mediante soporte técnico. Si ha hecho que el soporte técnico incrementase estos límites en el pasado y desea actualizarlos a los nuevos valores predeterminados, [abra una solicitud de soporte técnico al cliente en línea sin cargo](../articles/azure-resource-manager/resource-manager-quota-errors.md)

#### <a name="load-balancer"></a>Límites de Load Balancer
Los límites siguientes solo se aplican a los recursos de redes administrados a través de Azure Resource Manager por región y por suscripción. Aprenda a [ver el uso de los recursos actuales comparado con los límites de su suscripción](../articles/networking/check-usage-against-limits.md).

| Recurso | Límite predeterminado | Límite máximo |
| --- | --- | --- |
| Equilibradores de carga | 100 | 1000 |
| Reglas por recurso, básico | 150 | 250 |
| Reglas por recurso, estándar | 1250 | 1.500 |
| Reglas por configuración de dirección IP | 299 |299 |
| Configuraciones de direcciones IP de front-end, básico | 10 | 200 |
| Configuraciones de direcciones IP de front-end, estándar | 10 | 600 |
| Grupo de back-end, básico | 100, conjunto de disponibilidad único | 100, conjunto de disponibilidad único |
| Grupo de back-end, estándar | 1000, red virtual única | 1000, red virtual única |
| Recursos de back-end por Load Balancer, Estándar &ast; | 50 | 150 |
| Puertos de alta disponibilidad, estándar | 1 por front-end interno | 1 por front-end interno |

&ast; hasta 150 recursos, cualquier combinación de máquinas virtuales independientes, conjuntos de disponibilidad y conjuntos de escalado de máquinas virtuales.

Si necesita que los límites sean superiores a lo establecido de manera predeterminada, [póngase en contacto con el soporte técnico](../articles/azure-supportability/resource-manager-core-quotas-request.md ).

