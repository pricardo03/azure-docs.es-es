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
ms.openlocfilehash: 92ec03e20fb6e681a0afd14048449ad004ebca0c
ms.sourcegitcommit: 007ee4ac1c64810632754d9db2277663a138f9c4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/23/2019
ms.locfileid: "69991486"
---
# <a name="expressroute-monitoring-metrics-and-alerts"></a>Supervisión, métricas y alertas de ExpressRoute

Este artículo le servirá para comprender la supervisión, las métricas y las alertas de ExpressRoute con Azure Monitor. Azure Monitor es un centro único para todas las métricas, alertas y registros de diagnóstico en todo Azure.
 
>[!NOTE]
>No se recomienda el uso de **métricas clásicas**.
>

## <a name="circuit-metrics"></a>Métricas de circuito

Para ir a **Métricas**, haga clic en la página de ExpressRoute del circuito que desea supervisar. En **Supervisión**, puede ver las **Métricas**. Seleccione una de las métricas que se muestran a continuación. Se aplicará la agregación predeterminada. Opcionalmente puede aplicar la división, que mostrará las métricas con diferentes dimensiones.

### <a name="metrics-available"></a>Métricas disponibles: 
* **Disponibilidad** 
    * Disponibilidad de ARP
      * Dimensiones disponibles:
        * Par (enrutador de ExpressRoute principal y secundario)
        * Tipo de emparejamiento (privado, público o Microsoft)
    * Disponibilidad de BGP
      * Dimensiones disponibles:
        * Par (enrutador de ExpressRoute principal y secundario)
        * Tipo de emparejamiento (privado, público o Microsoft)
* **Tráfico**
    * BitsInPerSecond
      * Dimensiones disponibles:
        * Tipo de emparejamiento (privado, público o Microsoft)
    * BitsOutPerSecond
      * Dimensiones disponibles:
        * Tipo de emparejamiento (privado, público o Microsoft)
    * GlobalReachBitsInPerSecond
      * Dimensiones disponibles:
        * Skey de circuito emparejado (clave de servicio)
    * GlobalReachBitsOutPerSecond
      * Dimensiones disponibles:
        * Skey de circuito emparejado (clave de servicio)

>[!NOTE]
>El uso de *GlobalGlobalReachBitsInPerSecond* y *GlobalGlobalReachBitsOutPerSecond* solo será visible si se ha establecido al menos una conexión Global Reach.
>

## <a name="bits-in-and-out---metrics-across-all-peerings"></a>Bits dentro y fuera: métricas de todos los emparejamientos

Puede ver las métricas de todos los emparejamientos de un circuito ExpressRoute determinado.

![métricas de circuito](./media/expressroute-monitoring-metrics-alerts/ermetricspeering.jpg)

## <a name="bits-in-and-out---metrics-per-peering"></a>Bits dentro y fuera: métricas por emparejamiento

Puede ver las métricas de emparejamiento público, privado y de Microsoft en bits por segundo.

![métricas por emparejamiento](./media/expressroute-monitoring-metrics-alerts/erpeeringmetrics.jpg) 

## <a name="bgp-availability---split-by-peer"></a>Disponibilidad de BGP: división por par  

Puede ver la disponibilidad casi en tiempo real de BGP entre emparejamientos y pares (enrutadores de ExpressRoute principales y secundarios). En este panel se muestra la sesión de BGP principal arriba para el emparejamiento privado y la segunda sesión BGP abajo para el emparejamiento privado. 

![Disponibilidad de BGP por par](./media/expressroute-monitoring-metrics-alerts/erBgpAvailabilityMetrics.jpg) 

## <a name="arp-availability---split-by-peering"></a>Disponibilidad de ARP: división por emparejamiento  

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
