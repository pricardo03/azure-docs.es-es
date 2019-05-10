---
title: Configurar alertas de métricas de Azure VPN Gateway
description: Pasos para configurar alertas en métricas de puerta de enlace de VPN
services: vpn-gateway
author: anzaman
ms.service: vpn-gateway
ms.topic: conceptional
ms.date: 04/22/2019
ms.author: alzam
ms.openlocfilehash: e54dadbda0582095e8152ea30376d369177bfd86
ms.sourcegitcommit: 8fc5f676285020379304e3869f01de0653e39466
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/09/2019
ms.locfileid: "65509902"
---
# <a name="set-up-alerts-on-vpn-gateway-metrics"></a>Configurar alertas de métricas de puerta de enlace de VPN

Este artículo le ayudará a configurar alertas de métricas de puerta de enlace de VPN de Azure. Azure Monitor proporciona la capacidad de configurar alertas para los recursos de Azure. Puede configurar alertas para las puertas de enlace de red virtual del tipo "VPN".


|**Métrica**   | **Unidad** | **Granularidad** | **Descripción** | 
|---       | ---        | ---       | ---            | ---       |
|**AverageBandwidth**| Bytes/s  | 5 minutos| Promedio de uso de ancho de banda combinado de todas las conexiones de sitio a sitio en la puerta de enlace.     |
|**P2SBandwidth**| Bytes/s  | 1 minuto  | Promedio de uso de ancho de banda combinado de todas las conexiones de punto a sitio en la puerta de enlace.    |
|**P2SConnectionCount**| Count  | 1 minuto  | Recuento de conexiones de punto a sitio en la puerta de enlace.   |
|**TunnelAverageBandwidth** | Bytes/s    | 5 minutos  | Promedio de utilización del ancho de banda de los túneles creados en la puerta de enlace. |
|**TunnelEgressBytes** | Bytes | 5 minutos | Tráfico saliente en los túneles creados en la puerta de enlace.   |
|**TunnelEgressPackets** | Count | 5 minutos | Recuento de paquetes salientes de los túneles creados en la puerta de enlace.   |
|**TunnelEgressPacketDropTSMismatch** | Count | 5 minutos | Recuento de paquetes salientes se coloca en túneles causados por falta de coincidencia del selector de tráfico. |
|**TunnelIngressBytes** | Bytes | 5 minutos | Tráfico entrante en los túneles creados en la puerta de enlace.   |
|**TunnelIngressPackets** | Count | 5 minutos | Recuento de paquetes entrantes en los túneles creados en la puerta de enlace.   |
|**TunnelIngressPacketDropTSMismatch** | Count | 5 minutos | Recuento de paquetes entrantes descartados en túneles causados por falta de coincidencia del selector de tráfico. |


## <a name="setup"></a>Configurar alertas de Azure Monitor basadas en métricas mediante el portal de Azure

Los pasos del ejemplo siguiente creará una alerta en una puerta de enlace para:

- **Métrica:** TunnelAverageBandwidth
- **Condición:** Ancho de banda > 10 bytes por segundo
- **Window:** 5 minutos
- **Acción de alerta:** Email



1. Vaya al recurso de puerta de enlace de red virtual y seleccione **alertas** desde el **supervisión** ficha. A continuación, cree una nueva regla de alerta o editar una regla de alerta existente.

   ![Selecciones para crear una regla de alerta](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric/metric-alert1.png "Create")

2. Seleccione la puerta de enlace VPN como el recurso.

   ![El botón de selección y la puerta de enlace VPN en la lista de recursos](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric/metric-alert2.png "seleccione")

3. Seleccione una métrica para configurar la alerta.

   ![Selecciona la métrica en la lista de métricas](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric/metric-alert3.png "seleccione")
4. Configurar la lógica de señal. Hay tres componentes:

     a. **Dimensiones** Si la métrica tiene dimensiones, puede seleccionar valores de dimensión específicos para que la alerta evalúa solo los datos de esa dimensión. Estos son opcionales.

    b. **Condición**: Se trata de la operación para evaluar el valor de métrica.

    c. **Tiempo**: Especificar la granularidad de datos de métrica y el período de tiempo para evaluar la alerta.

   ![Detalles de configuración de lógica de señal](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric/metric-alert4.png "seleccione")

5. Para ver las reglas configuradas, seleccione **administrar reglas de alerta**.

   ![Botón para administrar las reglas de alerta](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric/metric-alert8.png "seleccione")

## <a name="next-steps"></a>Pasos siguientes

Para configurar alertas en los registros de diagnóstico de túnel, consulte [configurar alertas en los registros de diagnóstico de puerta de enlace VPN](vpn-gateway-howto-setup-alerts-virtual-network-gateway-log.md).
