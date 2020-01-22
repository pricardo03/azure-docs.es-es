---
title: archivo de inclusión
description: archivo de inclusión
services: networking
author: anavinahar
ms.service: networking
ms.topic: include
ms.date: 01/14/2020
ms.author: anavin
ms.custom: include file
ms.openlocfilehash: 17558b44c91425ce1a06625f8fd5c1806a762ba2
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/15/2020
ms.locfileid: "76021060"
---
<a name="azure-resource-manager-virtual-networking-limits"></a>Límites de redes: Azure Resource Manager Los siguientes límites solo son válidos en los recursos de redes administrados mediante **Azure Resource Manager** por región y por suscripción. Aprenda a [ver el uso de recursos actual comparado con los límites de su suscripción](../articles/networking/check-usage-against-limits.md).

> [!NOTE]
> Recientemente hemos aumentado todos los límites predeterminados a sus límites máximos. Si no hay ninguna columna de límite máximo, el recurso no tiene límites ajustables. Si el soporte técnico ha aumentado estos límites en el pasado y no ve los límites actualizados en las tablas siguientes, [abra una solicitud de soporte técnico al cliente en línea sin cargo](../articles/azure-resource-manager/templates/error-resource-quota.md)

| Resource | Límite predeterminado/máximo | 
| --- | --- |
| Redes virtuales |1,000 |
| Subredes por red virtual |3000 |
| Emparejamientos de redes virtuales por red virtual |500 |
| [Puertas de enlace de red virtual (puertas de enlace VPN) por red virtual](../articles/vpn-gateway/vpn-gateway-about-vpngateways.md#gwsku) |1 |
| [Puertas de enlace de red virtual (puertas de enlace ExpressRoute) por red virtual](../articles/expressroute/expressroute-about-virtual-network-gateways.md#gwsku) |1 |
| Servidores DNS por red virtual |20 |
| Direcciones IP privadas por red virtual |65 536 |
| Direcciones IP privadas por interfaz de red |256 |
| Direcciones IP privadas por máquina virtual |256 |
| Direcciones IP públicas por interfaz de red |256 |
| Direcciones IP públicas por máquina virtual |256 |
| [Conexiones TCP concurrentes o flujo UDP por NIC de una máquina virtual o instancia de rol](../articles/virtual-network/virtual-machine-network-throughput.md#flow-limits-and-recommendations) |500.000 |
| Tarjetas adaptadoras de red |65 536 |
| Grupos de seguridad de red |5\.000 |
| Reglas de NSG por NSG |1,000 |
| Direcciones IP y rangos especificados para el origen o destino en un grupo de seguridad |4\.000 |
| Grupos de seguridad de aplicaciones |3000 |
| Grupos de seguridad de aplicaciones por configuración de IP, por NIC |20 |
| Configuraciones de IP por grupo de seguridad de aplicaciones |4\.000 |
| Grupos de seguridad de aplicaciones que se pueden especificar en todas las reglas de seguridad de un grupo de seguridad de red |100 |
| Tablas de rutas definidas por el usuario |200 |
| Rutas definidas por el usuario por tabla de ruta |400 |
| Certificados raíz de punto a sitio por Azure VPN Gateway |20 |
| TAP de red virtual |100 |
| Configuraciones de TAP de la interfaz de red por TAP de red virtual |100 |

#### <a name="publicip-address"></a>Límites de dirección IP pública
| Resource | Límite predeterminado | Límite máximo |
| --- | --- | --- |
| Direcciones IP públicas - dinámicas | 1000 para Basic. |Póngase en contacto con el servicio de soporte técnico. |
| Direcciones IP públicas - estáticas | 1000 para Basic. |Póngase en contacto con el servicio de soporte técnico. |
| Direcciones IP públicas - estáticas | 1000 para Standard.|Póngase en contacto con el servicio de soporte técnico. |
| Longitud del prefijo de IP pública | /28 | Póngase en contacto con el servicio de soporte técnico. |

#### <a name="load-balancer"></a>Límites del equilibrador de carga
Los límites siguientes solo se aplican a los recursos de redes administrados a través de Azure Resource Manager por región y por suscripción. Aprenda a [ver el uso de recursos actual comparado con los límites de su suscripción](../articles/networking/check-usage-against-limits.md).

**Standard Load Balancer**

| Resource                                | Límite predeterminado/máximo         |
|-----------------------------------------|-------------------------------|
| Equilibradores de carga                          | 1,000                         |
| Reglas por recurso                      | 1500                         |
| Reglas por NIC (en todas las direcciones IP de una NIC) | 300                           |
| Configuraciones de direcciones IP de front-end              | 600                           |
| Tamaño de grupo de back-end                       | 1000 configuraciones de IP, una sola red virtual |
| Puertos de alta disponibilidad                 | 1 por front-end interno       |
| Reglas de salida por Load Balancer         | 20                            |


**Load Balancer básico**

| Resource                                | Límite predeterminado/máximo        |
|-----------------------------------------|------------------------------|
| Equilibradores de carga                          | 1,000                        |
| Reglas por recurso                      | 250                          |
| Reglas por NIC (en todas las direcciones IP de una NIC) | 300                          |
| Configuraciones de direcciones IP de front-end              | 200                          |
| Tamaño de grupo de back-end                       | 300 configuraciones de IP, un solo conjunto de disponibilidad |
| Conjuntos de disponibilidad por Load Balancer     | 150                          |

#### <a name="virtual-networking-limits-classic"></a>Los límites siguientes se aplican solo a los recursos de redes administrados a través del modelo de implementación **clásico** por suscripción. Aprenda a [ver el uso de recursos actual comparado con los límites de su suscripción](../articles/networking/check-usage-against-limits.md).

| Resource | Límite predeterminado | Límite máximo |
| --- | --- | --- |
| Redes virtuales |100 |100 |
| Sitios de red local |20 |50 |
| Servidores DNS por red virtual |20 |20 |
| Direcciones IP privadas por red virtual |4 096 |4 096 |
| Conexiones TCP concurrentes o flujo UDP por NIC de una máquina virtual o instancia de rol |500 000, hasta 1 000 000 de dos o varias NIC. |500 000, hasta 1 000 000 de dos o varias NIC. |
| Grupos de seguridad de red (NSG) |200 |200 |
| Reglas de NSG por NSG |1,000 |1,000 |
| Tablas de rutas definidas por el usuario |200 |200 |
| Rutas definidas por el usuario por tabla de ruta |400 |400 |
| Direcciones IP públicas (dinámicas) |500 |500 |
| Direcciones IP públicas reservadas |500 |500 |
| VIP pública por implementación |5 |Ponerse en contacto con soporte técnico |
| VIP privada (equilibrio de carga interno) por implementación |1 |1 |
| Listas de control de acceso (ACL) de punto de conexión |50 |50 |
