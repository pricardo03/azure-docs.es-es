---
title: Firewall de aplicaciones web de Azure de supervisión y registro
description: Obtenga información sobre firewall de aplicaciones web (WAF) con la puerta delantera supervisión y registro
services: frontdoor
author: KumudD
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/08/2019
ms.author: tyao;kumud
ms.openlocfilehash: 5368ed8d1e60a646366065e2cf617fb2f3735b53
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61459766"
---
# <a name="azure-web-application-firewall-monitoring-and-logging"></a>Firewall de aplicaciones web de Azure de supervisión y registro 

Supervisión de firewall (WAF) de aplicación web de Azure y el registro se proporcionan a través del registro y la integración con Azure Monitor y Azure Monitor registros.

> [!IMPORTANT]
> El WAF de supervisión y la característica de registro para el acceso de principal de Azure está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="azure-monitor"></a>Azure Monitor

WAF con registro de la puerta delantera se integra con [Azure Monitor](../azure-monitor/overview.md). Azure Monitor permite realizar un seguimiento de información de diagnóstico incluidos los registros y alertas de WAF. Puede configurar la supervisión de WAF en el recurso de puerta de entrada en el portal en el **diagnósticos** pestaña o a través de Azure Monitor de servicio directamente.

Desde Azure portal, vaya al tipo de recurso de puerta de entrada. Desde **supervisión**/**métricas** pestaña a la izquierda, puede agregar **WebApplicationFirewallRequestCount** para realizar un seguimiento del número de solicitudes que coinciden con las reglas de WAF. Filtros personalizados se pueden crear en función de los tipos de acción y los nombres de las reglas.

![WAFMetrics](./media//waf-front-door-monitor/waf-frontdoor-metrics.png)

## <a name="logs-and-diagnostics"></a>Registros y diagnósticos

WAF con puerta delantera proporciona informes detallados acerca de las amenazas detectadas. El registro se integra con los registros de Diagnósticos de Azure y las alertas se registran en formato json. Estos registros pueden integrarse con los [registros de Azure Monitor](../azure-monitor/insights/azure-networking-analytics.md).

![WAFDiag](./media/waf-front-door-monitor/waf-frontdoor-diagnostics.png)

FrontdoorAccessLog registra todas las solicitudes que se reenvían al back-end del cliente. FrontdoorWebApplicationFirewallLog registra cualquier solicitud que coincide con una regla de WAF.

La siguiente consulta de ejemplo obtiene los registros de WAF en solicitudes bloqueadas:

``` WAFlogQuery
AzureDiagnostics
| where ResourceType == "FRONTDOORS" and Category == "FrontdoorWebApplicationFirewallLog"
| where action_s == "Block"

```

La siguiente consulta de ejemplo obtiene las entradas de AccessLogs:

``` AccessLogQuery
AzureDiagnostics
| where ResourceType == "FRONTDOORS" and Category == "FrontdoorAccessLog"


```

## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre [puerta delantera](front-door-overview.md).

