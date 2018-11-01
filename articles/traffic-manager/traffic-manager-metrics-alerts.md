---
title: Métricas y alertas en Azure Traffic Manager | Microsoft Docs
description: En este artículo se describen las métricas disponibles para Traffic Manager en Azure.
services: traffic-manager
documentationcenter: ''
author: KumudD
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/11/2018
ms.author: kumud
ms.openlocfilehash: 2be5718dd95eff837c5a016b5254be0992eb8f9a
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/26/2018
ms.locfileid: "50138385"
---
# <a name="traffic-manager-metrics-and-alerts"></a>Métricas y alertas de Traffic Manager

Traffic Manager le proporciona equilibrio de carga basado en DNS, lo que incluye varios métodos de enrutamiento y opciones de supervisión de puntos de conexión. En este artículo se describen las métricas y las alertas asociadas que están a disposición de los clientes. 

## <a name="metrics-available-in-traffic-manager"></a>Métricas disponibles en Traffic Manager 

Traffic Manager proporciona las siguientes métricas para cada perfil individual, que los clientes pueden consumir para comprender su uso de Traffic Manager y el estado de sus puntos de conexión con ese perfil.  

### <a name="queries-by-endpoint-returned"></a>Consultas devueltas por punto de conexión
Use [esta métrica](../monitoring-and-diagnostics/monitoring-supported-metrics.md) para ver el número de consultas que ha procesado un perfil de Traffic Manager durante un periodo determinado. También puede ver la misma información con una granularidad de nivel de punto de conexión, lo que le ayudará a comprender cuántas veces se devolvió un punto de conexión en las respuestas a consultas desde Traffic Manager.

En el ejemplo siguiente, la figura 1 muestra todas las respuestas a consultas que ha devuelto el perfil de Traffic Manager. 

  
![Métricas de Traffic Manager: vista agregada de todas las consultas](./media/traffic-manager-metrics-alerts/traffic-manager-metrics-queries-aggregate-view.png)

*Figura 1: Vista agregada con todas las consultas*
  
La figura 2 muestra la misma información, sin embargo, se divide por puntos de conexión. Como resultado, puede ver el volumen de las respuestas a consultas en la que se devolvió un punto de conexión en particular.

![Métricas de Traffic Manager: vista dividida del volumen de consultas por punto de conexión](./media/traffic-manager-metrics-alerts/traffic-manager-metrics-query-volume-per-endpoint.png)

*Figura 2: Vista dividida con el volumen de consultas por punto de conexión devuelto*

## <a name="endpoint-status-by-endpoint"></a>Estado de punto de conexión por punto de conexión
Use [esta métrica](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftnetworktrafficmanagerprofiles) para conocer el estado de mantenimiento de los puntos de conexión en el perfil. Toma dos valores:
 - Use **1**, si el punto de conexión está activo.
 - Use **0**, si el punto de conexión está inactivo.

Esta métrica puede mostrarse como valor agregado que representa el estado de todas las métricas (figura 3), o bien se puede dividir (consulte la figura 4) para mostrar el estado de puntos de conexión específicos. En el primer caso, si el nivel de agregación se selecciona como **Avg**, el valor de esta métrica es la media aritmética del estado de todos los puntos de conexión. Por ejemplo, si un perfil tiene dos puntos de conexión y solo uno es correcto, esta métrica tendrá un valor de **0,50**, tal como se muestra en la figura 3. 


![Métricas de Traffic Manager: vista compuesta del estado de los puntos de conexión](./media/traffic-manager-metrics-alerts/traffic-manager-metrics-endpoint-status-composite-view.png)

*Figura 3: Vista compuesta de la métrica de estado de los puntos de conexión, agregación "Avg" seleccionada*


![Métricas de Traffic Manager: vista dividida del estado de los puntos de conexión](./media/traffic-manager-metrics-alerts/traffic-manager-metrics-endpoint-status-split-view.png)

*Figura 4: Vista dividida de las métricas de estado de los puntos de conexión*

Puede consumir estas métricas a través del portal del [servicio Azure Monitor](../monitoring-and-diagnostics/monitoring-supported-metrics.md), la [API de REST](https://docs.microsoft.com/rest/api/monitor/), [CLI de Azure](https://docs.microsoft.com/cli/azure/monitor) y [Azure PowerShell](https://docs.microsoft.com/powershell/module/azurerm.insights), o a través de la sección de métricas de la experiencia del portal de Traffic Manager.

## <a name="alerts-on-traffic-manager-metrics"></a>Alertas relativas a métricas de Traffic Manager
Además de procesar y mostrar las métricas de Traffic Manager, Azure Monitor permite a los clientes configurar y recibir alertas asociadas a estas métricas. Puede elegir qué condiciones deben cumplirse en estas métricas para que se produzca una alerta, con qué frecuencia deben supervisarse esas condiciones y cómo se le deberían enviar las alertas. Para obtener más información, consulte la [documentación sobre alertas de Azure Monitor](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md).

## <a name="next-steps"></a>Pasos siguientes
- Más información sobre el [servicio Azure Monitor](../monitoring-and-diagnostics/monitoring-supported-metrics.md)
- Más información sobre cómo [crear un nuevo gráfico con Azure Monitor](../monitoring-and-diagnostics/monitoring-metric-charts.md#how-do-i-create-a-new-chart)
