---
title: Supervisión, métricas y alertas de Azure ExpressRoute | Microsoft Docs
description: En esta página se proporciona información acerca de la supervisión de ExpressRoute.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 08/22/2019
ms.author: cherylmc
ms.custom: seodec18
ms.openlocfilehash: dbe03ef29bd28d465fa671abc915d63d4b038cb2
ms.sourcegitcommit: 116bc6a75e501b7bba85e750b336f2af4ad29f5a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/20/2019
ms.locfileid: "71154778"
---
# <a name="expressroute-monitoring-metrics-and-alerts"></a>Supervisión, métricas y alertas de ExpressRoute

Este artículo le servirá para comprender la supervisión, las métricas y las alertas de ExpressRoute con Azure Monitor. Azure Monitor es un centro único para todas las métricas, alertas y registros de diagnóstico en todo Azure.
 
>[!NOTE]
>No se recomienda el uso de **métricas clásicas**.
>

## <a name="expressroute-metrics"></a>Métricas de ExpressRoute

Para ver **Métricas**, vaya a la página *Azure Monitor* y haga clic en *Métricas*. Para ver las métricas de **ExpressRoute**, filtre por el tipo de recurso *Circuitos de ExpressRoute*. Para ver las métricas de **Global Reach**, filtre por el tipo de recurso *Circuitos de ExpressRoute* y seleccione un recurso de circuito de ExpressRoute que tenga Global Reach habilitado. Para ver las métricas de ExpressRoute Direct, filtre por el tipo de recurso Puertos de ExpressRoute. 

Una vez seleccionada una métrica, se aplicará la agregación predeterminada. Opcionalmente, puede aplicar la división, que mostrará la métrica con diferentes dimensiones.

### <a name="available-metrics"></a>Métricas disponibles
|**Métrica**|**Categoría**|**Dimensiones**|**Características**|
| --- | --- | --- | --- |
|Disponibilidad de ARP|Disponibilidad|<ui><li>Par (enrutador de ExpressRoute principal y secundario)</ui></li><ui><li> Tipo de emparejamiento (privado, público o Microsoft)</ui></li>|ExpressRoute|
|Disponibilidad de BGP|Disponibilidad|<ui><li> Par (enrutador de ExpressRoute principal y secundario)</ui></li><ui><li> Tipo de emparejamiento</ui></li>|ExpressRoute|
|BitsInPerSecond|Tráfico|<ui><li> Tipo de emparejamiento (ExpressRoute)</ui></li><ui><li>Vínculo (ExpressRoute Direct)</ui></li>| <li> ExpressRoute</li><li>ExpressRoute Direct|
|BitsOutPerSecond|Tráfico| <ui><li>Tipo de emparejamiento (ExpressRoute)</ui></li><ui><li> Vínculo (ExpressRoute Direct) | <ui><li>ExpressRoute<ui><li>ExpressRoute Direct</ui></li> |
|GlobalReachBitsInPerSecond|Tráfico|<ui><li>Skey de circuito emparejado (clave de servicio)</ui></li>|Global Reach|
|GlobalReachBitsOutPerSecond|Tráfico|<ui><li>Skey de circuito emparejado (clave de servicio)</ui></li>|Global Reach|
|AdminState|Conectividad física|Vínculo|ExpressRoute Direct|
|LineProtocol|Conectividad física|Vínculo|ExpressRoute Direct|
|RxLightLevel|Conectividad física|<ui><li>Vínculo</ui></li><ui><li>Carril</ui></li>|ExpressRoute Direct|
|TxLightLevel|Conectividad física|<ui><li>Vínculo</ui></li><ui><li>Carril</ui></li>|ExpressRoute Direct|
>[!NOTE]
>El uso de *GlobalGlobalReachBitsInPerSecond* y *GlobalGlobalReachBitsOutPerSecond* solo será visible si se ha establecido al menos una conexión Global Reach.
>

## <a name="circuits-metrics"></a>Métricas de circuito

### <a name="bits-in-and-out---metrics-across-all-peerings"></a>Bits dentro y fuera: métricas de todos los emparejamientos

Puede ver las métricas de todos los emparejamientos de un circuito ExpressRoute determinado.

![métricas de circuito](./media/expressroute-monitoring-metrics-alerts/ermetricspeering.jpg)

### <a name="bits-in-and-out---metrics-per-peering"></a>Bits dentro y fuera: métricas por emparejamiento

Puede ver las métricas de emparejamiento público, privado y de Microsoft en bits por segundo.

![métricas por emparejamiento](./media/expressroute-monitoring-metrics-alerts/erpeeringmetrics.jpg) 

### <a name="bgp-availability---split-by-peer"></a>Disponibilidad de BGP: división por par  

Puede ver la disponibilidad casi en tiempo real de BGP entre emparejamientos y pares (enrutadores de ExpressRoute principales y secundarios). En este panel se muestra la sesión de BGP principal arriba para el emparejamiento privado y la segunda sesión BGP abajo para el emparejamiento privado. 

![Disponibilidad de BGP por par](./media/expressroute-monitoring-metrics-alerts/erBgpAvailabilityMetrics.jpg) 

### <a name="arp-availability---split-by-peering"></a>Disponibilidad de ARP: división por emparejamiento  

Puede ver la disponibilidad casi en tiempo real de [ARP](https://docs.microsoft.com/azure/expressroute/expressroute-troubleshooting-arp-resource-manager) entre emparejamientos y pares (enrutadores de ExpressRoute principales y secundarios). En este panel se muestra la sesión de ARP de emparejamiento privado arriba entre ambos pares, y completa abajo para el emparejamientos de Microsoft entre emparejamientos. La agregación predeterminada (promedio) se usó en ambos pares.  

![Disponibilidad de ARP por par](./media/expressroute-monitoring-metrics-alerts/erArpAvailabilityMetrics.jpg) 

## <a name="expressroute-gateway-connections-in-bitsseconds"></a>Conexiones de puerta de enlace de ExpressRoute en bits por segundo

![conexiones de puerta de enlace](./media/expressroute-monitoring-metrics-alerts/erconnections.jpg ) 

## <a name="alerts-for-expressroute-gateway-connections"></a>Alertas para las conexiones de puerta de enlace de ExpressRoute

1. Para configurar las alertas, vaya a **Azure Monitor** y, a continuación, haga clic en **Alertas**.

   ![alerts](./media/expressroute-monitoring-metrics-alerts/eralertshowto.jpg)

2. Haga clic en **+Seleccionar destino** y seleccione el recurso de conexión de puerta de enlace de ExpressRoute.

   ![Destino]( ./media/expressroute-monitoring-metrics-alerts/alerthowto2.jpg)
3. Defina los detalles de la alerta.

   ![grupo de acciones](./media/expressroute-monitoring-metrics-alerts/alerthowto3.jpg)

4. Defina y agregue el grupo de acciones.

   ![adición de grupo de acciones](./media/expressroute-monitoring-metrics-alerts/actiongroup.png)

## <a name="alerts-based-on-each-peering"></a>Alertas basadas en cada emparejamiento

 ![what](./media/expressroute-monitoring-metrics-alerts/basedpeering.jpg)

## <a name="configure-alerts-for-activity-logs-on-circuits"></a>Configuración de alertas para los registros de actividad en circuitos

En **Criterios de alerta**, puede seleccionar **Registro de actividad** para el tipo de señal y seleccionar la señal.

  ![otro](./media/expressroute-monitoring-metrics-alerts/alertshowto6activitylog.jpg)
  
## <a name="next-steps"></a>Pasos siguientes

Configure su conexión ExpressRoute.
  
  * [Creación y modificación de un circuito](expressroute-howto-circuit-arm.md)
  * [Creación y modificación de la configuración de emparejamiento](expressroute-howto-routing-arm.md)
  * [Vinculación de redes virtuales a circuitos ExpressRoute](expressroute-howto-linkvnet-arm.md)
