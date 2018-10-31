---
title: Supervisión, métricas y alertas de ExpressRoute | Microsoft Docs
description: En esta página se proporciona información acerca de la supervisión de ExpressRoute.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 09/21/2018
ms.author: cherylmc
ms.openlocfilehash: fcdaf10ad30cee0e7b79304ff21d67560522875c
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2018
ms.locfileid: "49377804"
---
# <a name="expressroute-monitoring-metrics-and-alerts"></a>Supervisión, métricas y alertas de ExpressRoute

 Este artículo le servirá para comprender la supervisión, las métricas y las alertas de ExpressRoute. Azure Monitor es un centro único para todas las métricas, alertas y registros de diagnóstico en todo Azure.

## <a name="circuit-metrics"></a>Métricas de circuito

Para ir a **Métricas**, haga clic en la página de ExpressRoute del circuito que desea supervisar. En **Supervisión** puede ver las **Métricas**. Seleccione BitsInPerSecond o BitsOutPerSecond y la agregación. Opcionalmente puede aplicar la división, que mostrará las métricas por tipo de emparejamiento.

![métricas de circuito](./media/expressroute-monitoring-metrics-alerts/ermetricspeering.jpg)

## <a name="metrics-per-peering"></a>Métricas por emparejamiento

Puede ver las métricas de emparejamiento público, privado y de Microsoft en bits por segundo.

![métricas por emparejamiento](./media/expressroute-monitoring-metrics-alerts/erpeeringmetrics.jpg) 

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
* Configure su conexión ExpressRoute.
  
  * [Creación y modificación de un circuito](expressroute-howto-circuit-arm.md)
  * [Creación y modificación de la configuración de emparejamiento](expressroute-howto-routing-arm.md)
  * [Vinculación de redes virtuales a circuitos ExpressRoute](expressroute-howto-linkvnet-arm.md)
