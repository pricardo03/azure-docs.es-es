---
title: archivo de inclusión
description: archivo de inclusión
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 08/02/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 4cbbb64489acf23c1248e35269e1441dd2a6878e
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/03/2018
ms.locfileid: "39513793"
---
|**SKU**   | **Túneles<br>S2S/entre redes virtuales** | **Conexiones<br>P2S** | **Pruebas comparativas de rendimiento<br>agregado** |
|---       | ---                             | ---                    | ---                         |
|**VpnGw1**| Máx. 30*                         | Máx. 128\*\*             | 650 MBps                    |
|**VpnGw2**| Máx. 30*                         | Máx. 128\*\*             | 1 Gbps                      |
|**VpnGw3**| Máx. 30*                         | Máx. 128\*\*             | 1,25 Gbps                   |
|**Básico** | Máx. 10                         | Máx. 128               | 100 Mbps                    | 

* (*) Use una [red WAN virtual](../articles/virtual-wan/virtual-wan-about.md) si necesita más de 30 túneles VPN S2S.

* (**) Póngase en contacto con soporte técnico si se necesitan conexiones adicionales. Esto solo se aplica a IKEv2, no se puede aumentar el número de conexiones para SSTP.

* Las pruebas comparativas de rendimiento agregado se basan en las mediciones de varios túneles agregados a través de una sola puerta de enlace. No es un rendimiento garantizado debido a las condiciones del tráfico de Internet y a los comportamientos de las aplicaciones.

* Puede encontrar más información sobre los precios en la página de [precios](https://azure.microsoft.com/pricing/details/vpn-gateway).

* La información del SLA (contrato de nivel de servicio) puede encontrarse en la página [SLA](https://azure.microsoft.com/support/legal/sla/vpn-gateway/).

* Se admiten VpnGw1, VpnGw2 y VpnGw3 para las puertas de enlace VPN únicamente con modelo de implementación de Resource Manager.