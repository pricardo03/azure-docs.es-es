---
title: archivo de inclusión
description: archivo de inclusión
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 05/22/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 8fb9e9ea0e126509697b4874bf1e5e0b6a380e7f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66425759"
---
|**SKU**   | **Túneles<br>S2S/entre redes virtuales** | Conexiones **P2S<br> SSTP** | Conexiones **P2S<br> IKEv2/OpenVPN** | **Pruebas comparativas de rendimiento<br>agregado** | **BGP** | **Con redundancia de zona** |
|---       | ---        | ---       | ---            | ---       | --- | --- |
|**Básico** | Máx. 10    | Máx. 128  | No compatible  | 100 Mbps  | No compatible| Sin |
|**VpnGw1**| Máx. 30*   | Máx. 128  | Máx. 250       | 650 MBps  | Compatible | Sin |
|**VpnGw2**| Máx. 30*   | Máx. 128  | Máx. 500       | 1 Gbps    | Compatible | Sin |
|**VpnGw3**| Máx. 30*   | Máx. 128  | Máx. 1000      | 1,25 Gbps | Compatible | Sin |
|**VpnGw1AZ**| Máx. 30*   | Máx. 128  | Máx. 250       | 650 MBps  | Compatible | Sí |
|**VpnGw2AZ**| Máx. 30*   | Máx. 128  | Máx. 500       | 1 Gbps    | Compatible | Sí |
|**VpnGw3AZ**| Máx. 30*   | Máx. 128  | Máx. 1000      | 1,25 Gbps | Compatible | Sí |


(*) Use una [red WAN virtual](../articles/virtual-wan/virtual-wan-about.md) si necesita más de 30 túneles VPN S2S.

* Las pruebas comparativas de rendimiento agregado se basan en las mediciones de varios túneles agregados a través de una sola puerta de enlace. El banco de pruebas de rendimiento agregado para una puerta de enlace de VPN es la combinación de S2S + P2S. **Si tiene una gran cantidad de conexiones P2S, puede afectar negativamente a una conexión S2S debido a las limitaciones del rendimiento.** Las pruebas comparativas de rendimiento agregado no es un rendimiento garantizado debido a las condiciones del tráfico de Internet y a los comportamientos de las aplicaciones.

* Estos límites de conexión son independientes. Por ejemplo, en una SKU de VpnGw1 puede tener 128 conexiones SSTP, además de 250 conexiones IKEv2.

* Puede encontrar más información sobre los precios en la página de [precios](https://azure.microsoft.com/pricing/details/vpn-gateway).

* La información del SLA (contrato de nivel de servicio) puede encontrarse en la página [SLA](https://azure.microsoft.com/support/legal/sla/vpn-gateway/).

* Se admiten VpnGw1, VpnGw2 y VpnGw3 para las puertas de enlace VPN únicamente con modelo de implementación de Resource Manager.

* La SKU Basic se considera una SKU heredada. La SKU Basic tiene ciertas limitaciones de características. No se puede cambiar el tamaño de una puerta de enlace que utiliza una SKU Basic a una de las SKU de puerta de enlace nuevas, sino que debe cambiar a una SKU nueva, lo que implica eliminar y volver a crear la puerta de enlace VPN. Compruebe que se admite la característica que necesita antes de usar la SKU Basic.
