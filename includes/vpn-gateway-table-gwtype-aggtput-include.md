---
title: archivo de inclusión
description: archivo de inclusión
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 11/12/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: b698dd03473dd3cb708c47c6554869eebba48bf9
ms.sourcegitcommit: 0fc99ab4fbc6922064fc27d64161be6072896b21
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2018
ms.locfileid: "51597654"
---
|**SKU**   | **Túneles<br>S2S/entre redes virtuales** | Conexiones **P2S<br> SSTP** | Conexiones **P2S<br> IKEv2** | **Pruebas comparativas de rendimiento<br>agregado** | **BGP** |
|---       | ---        | ---       | ---            | ---       | --- |
|**Básico** | Máx. 10    | Máx. 128  | No compatible  | 100 Mbps  | No compatible|
|**VpnGw1**| Máx. 30*   | Máx. 128  | Máx. 250       | 650 MBps  | Compatible |
|**VpnGw2**| Máx. 30*   | Máx. 128  | Máx. 500       | 1 Gbps    | Compatible |
|**VpnGw3**| Máx. 30*   | Máx. 128  | Máx. 1000      | 1,25 Gbps | Compatible |


(*) Use una [red WAN virtual](../articles/virtual-wan/virtual-wan-about.md) si necesita más de 30 túneles VPN S2S.

* El banco de pruebas de rendimiento agregado para una puerta de enlace de VPN es la combinación de S2S + P2S. **Si tiene una gran cantidad de conexiones P2S, puede afectar negativamente a una conexión S2S debido a las limitaciones del rendimiento.** Las pruebas comparativas de rendimiento agregado se basan en las mediciones de varios túneles agregados a través de una sola puerta de enlace. No es un rendimiento garantizado debido a las condiciones del tráfico de Internet y a los comportamientos de las aplicaciones.

* Estos límites de conexión son independientes. Por ejemplo, en una SKU de VpnGw1 puede tener 128 conexiones SSTP, además de 250 conexiones IKEv2.

* Puede encontrar más información sobre los precios en la página de [precios](https://azure.microsoft.com/pricing/details/vpn-gateway).

* La información del SLA (contrato de nivel de servicio) puede encontrarse en la página [SLA](https://azure.microsoft.com/support/legal/sla/vpn-gateway/).

* Se admiten VpnGw1, VpnGw2 y VpnGw3 para las puertas de enlace VPN únicamente con modelo de implementación de Resource Manager.
