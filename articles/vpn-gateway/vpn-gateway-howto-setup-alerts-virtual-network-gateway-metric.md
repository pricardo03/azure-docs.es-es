---
title: Cómo configurar alertas de métricas de Azure VPN Gateway
description: Pasos para configurar alertas en métricas de puerta de enlace de VPN
services: vpn-gateway
author: anzaman
ms.service: vpn-gateway
ms.topic: conceptional
ms.date: 04/22/2019
ms.author: alzam
ms.openlocfilehash: 890b096acba601ec20efaac21155da84e77a1f31
ms.sourcegitcommit: 37343b814fe3c95f8c10defac7b876759d6752c3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/24/2019
ms.locfileid: "63769472"
---
# <a name="setting-up-alerts-on-vpn-gateway-metrics"></a>Configuración de alertas en métricas de puerta de enlace de VPN

Este artículo le ayudará a configurar alertas para métricas de puerta de enlace de VPN. Azure monitor proporciona la capacidad de configurar alertas para los recursos de Azure. Las alertas pueden configurarse para puertas de enlace de red virtual de tipo "VPN".


|**Métrica**   | **Unidad** | **Granularidad** | **Descripción** | 
|---       | ---        | ---       | ---            | ---       |
|**AverageBandwidth**| Bytes/s  | 5 minutos| Promedio de uso de ancho de banda combinado de todas las conexiones de sitio a sitio en la puerta de enlace.     |
|**P2SBandwidth**| Bytes/s  | 1 minuto  | Promedio de uso de ancho de banda combinado de todas las conexiones de punto a sitio en la puerta de enlace.    |
|**P2SConnectionCount**| Número  | 1 minuto  | Recuento de P2S las conexiones de puerta de enlace.   |
|**TunnelAverageBandwidth** | Bytes/s    | 5 minutos  | Promedio de utilización del ancho de banda de los túneles creados en la puerta de enlace. |
|**TunnelEgressBytes** | Bytes | 5 minutos | Tráfico saliente en los túneles creados en la puerta de enlace.   |
|**TunnelEgressPackets** | Número | 5 minutos | Recuento de paquetes salientes de los túneles creados en la puerta de enlace.   |
|**TunnelEgressPacketDropTSMismatch** | Número | 5 minutos | Recuento de paquetes salientes se coloca en túneles causados por falta de coincidencia de TS. |
|**TunnelIngressBytes** | Bytes | 5 minutos | Tráfico entrante en los túneles creados en la puerta de enlace.   |
|**TunnelIngressPackets** | Número | 5 minutos | Recuento de paquetes entrantes en los túneles creados en la puerta de enlace.   |
|**TunnelIngressPacketDropTSMismatch** | Número | 5 minutos | Recuento de paquetes entrantes descartados en túneles causados por falta de coincidencia de TS. |


## <a name="setup"></a>Configurar alertas de Azure Monitor basadas en métricas mediante el portal

Los pasos de ejemplo siguientes creará una alerta en una puerta de enlace para: <br>

**Métrica:** Ancho de banda de túnel promedio <br>
**Condición:** ancho de banda > 10 Bytes por segundo <br>
**Window:** 5 minutos <br>
**Acción de alerta:** Email <br>



1. Navegue hasta el recurso de puerta de enlace de red virtual y seleccione "Alertas" en la ficha Supervisión, a continuación, cree una nueva regla de alerta o editar una regla de alerta existente.

![Point-to-site](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric/metric-alert1.png "Create")

2. Seleccione la puerta de enlace VPN como el recurso.

![point-to-site](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric/metric-alert2.png "Select")

3. Seleccione una métrica para configurar la alerta ![point-to-site](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric/metric-alert3.png "seleccione")
4. Configurar la lógica de señal. Hay tres componentes para la lógica de señal:

     a. Dimensiones: Si la métrica tiene dimensiones, se pueden seleccionar valores de dimensión específicos para que la alerta solo se evalúa como datos de esa dimensión. Estos son opcionales.<br>
    b. Condición: La operación para evaluar el valor de métrica.<br>
    c. Hora: Especificar la granularidad de datos de métrica y el período de tiempo para evaluar la alerta.<br>

![point-to-site](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric/metric-alert4.png "Select")

5. Para ver las reglas configuradas, haga clic en "Manage alert rules" ![point-to-site](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric/metric-alert8.png "seleccione")

## <a name="next-steps"></a>Pasos siguientes

Para configurar alertas en los registros de diagnóstico de túnel, consulte [cómo configurar alertas en los registros de diagnóstico de puerta de enlace VPN](vpn-gateway-howto-setup-alerts-virtual-network-gateway-log.md).
