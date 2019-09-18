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
ms.openlocfilehash: 65c1011e6e005c190d1ae5d51fdd009f66a20956
ms.sourcegitcommit: 083aa7cc8fc958fc75365462aed542f1b5409623
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/11/2019
ms.locfileid: "70919741"
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

* Estos límites de conexión son independientes. Por ejemplo, en una SKU de VpnGw1 puede tener 128 conexiones SSTP, además de 250 conexiones IKEv2.

* Puede encontrar más información sobre los precios en la página de [precios](https://azure.microsoft.com/pricing/details/vpn-gateway).

* La información del SLA (contrato de nivel de servicio) puede encontrarse en la página [SLA](https://azure.microsoft.com/support/legal/sla/vpn-gateway/).

* Se admiten VpnGw1, VpnGw2, VpnGw3, VpnGw1AZ, VpnGw2AZ y VpnGw3AZ para las puertas de enlace VPN únicamente con un modelo de implementación de Resource Manager.

* La SKU Basic se considera una SKU heredada. La SKU Basic tiene ciertas limitaciones de características. No se puede cambiar el tamaño de una puerta de enlace que utiliza una SKU Basic a una de las SKU de puerta de enlace nuevas, sino que debe cambiar a una SKU nueva, lo que implica eliminar y volver a crear la puerta de enlace VPN. Compruebe que se admite la característica que necesita antes de usar la SKU Basic.

* Las pruebas comparativas de rendimiento agregado se basan en las mediciones de varios túneles agregados a través de una sola puerta de enlace. El banco de pruebas de rendimiento agregado para una puerta de enlace de VPN es la combinación de S2S + P2S. **Si tiene una gran cantidad de conexiones P2S, puede afectar negativamente a una conexión S2S debido a las limitaciones del rendimiento.** Las pruebas comparativas de rendimiento agregado no es un rendimiento garantizado debido a las condiciones del tráfico de Internet y a los comportamientos de las aplicaciones.

* Para ayudar a nuestros clientes a comprender el rendimiento relativo de las SKU de VpnGw, usamos herramientas iPerf y CTSTraffic disponibles públicamente para medir los rendimientos. En la tabla siguiente se enumeran los resultados de las pruebas de rendimiento mediante algoritmos diferentes. Como puede ver, el mejor rendimiento se obtiene cuando usamos el algoritmo GCMAES256 para el cifrado y la integridad IPsec. Obtuvimos un rendimiento medio al usar AES256 para el cifrado IPsec y SHA256 para la integridad. Cuando usamos DES3 para el cifrado IPsec y SHA256 para la integridad, obtuvimos el rendimiento más bajo.

|**SKU**   | **Algoritmos<br>usados** | **Rendimiento<br>observado** | **Paquetes por segundo<br>observados** |
|---       | ---                 | ---            | ---                    |
|**VpnGw1**| GCMAES256<br>AES256 y SHA256<br>DES3 y SHA256| 650 MBps<br>500 Mbps<br>120 Mbps   | 58 000<br>50.000<br>50.000|
|**VpnGw2**| GCMAES256<br>AES256 y SHA256<br>DES3 y SHA256| 1 Gbps<br>500 Mbps<br>120 Mbps | 90 000<br>80 000<br>55 000|
|**VpnGw3**| GCMAES256<br>AES256 y SHA256<br>DES3 y SHA256| 1,25 Gbps<br>550 Mbps<br>120 Mbps | 105 000<br>90 000<br>60 000|
|**VpnGw1AZ**| GCMAES256<br>AES256 y SHA256<br>DES3 y SHA256| 650 MBps<br>500 Mbps<br>120 Mbps   | 58 000<br>50.000<br>50.000|
|**VpnGw2AZ**| GCMAES256<br>AES256 y SHA256<br>DES3 y SHA256| 1 Gbps<br>500 Mbps<br>120 Mbps | 90 000<br>80 000<br>55 000|
|**VpnGw3AZ**| GCMAES256<br>AES256 y SHA256<br>DES3 y SHA256| 1,25 Gbps<br>550 Mbps<br>120 Mbps | 105 000<br>90 000<br>60 000|
