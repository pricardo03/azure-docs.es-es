---
title: Supervisión y registro del firewall de aplicaciones web de Azure
description: Aprenda sobre la supervisión y el registro del firewall de aplicaciones web (WAF) con FrontDoor
services: frontdoor
author: KumudD
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/31/2019
ms.author: tyao;kumud
ms.openlocfilehash: e4ba6cca679ce4910ea941d9578939721514b2ec
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66478975"
---
# <a name="azure-web-application-firewall-monitoring-and-logging"></a>Supervisión y registro del firewall de aplicaciones web de Azure 

La supervisión y el registro del firewall de aplicaciones web de Azure (WAF) se proporcionan mediante el registro y la integración con Azure Monitor y con los registros de Azure Monitor.

## <a name="azure-monitor"></a>Azure Monitor

WAF con el registro de FrontDoor se integra con [Azure Monitor](../azure-monitor/overview.md). Azure Monitor permite realizar un seguimiento de la información de diagnóstico incluidos los registros y las alertas de WAF. Puede configurar la supervisión de WAF dentro del recurso de Front Door en el portal, en la pestaña **Diagnóstico** o mediante el servicio Azure Monitor directamente.

Desde Azure Portal, vaya al tipo de recurso Front Door. En la pestaña **Supervisión**/**Métricas** a la izquierda, puede agregar **WebApplicationFirewallRequestCount** para realizar un seguimiento del número de solicitudes que coinciden con las reglas de WAF. Se pueden crear filtros personalizados en función de los tipos de acción y los nombres de las reglas.

![WAFMetrics](./media//waf-front-door-monitor/waf-frontdoor-metrics.png)

## <a name="logs-and-diagnostics"></a>Registros y diagnósticos

WAF con Front Door ofrece informes detallados sobre cada amenaza que detecta. El registro se integra con los registros de Diagnósticos de Azure y las alertas se registran en formato json. Estos registros pueden integrarse con los [registros de Azure Monitor](../azure-monitor/insights/azure-networking-analytics.md).

![WAFDiag](./media/waf-front-door-monitor/waf-frontdoor-diagnostics.png)

FrontdoorAccessLog registra todas las solicitudes que se reenvían a los back-ends del cliente. FrontdoorWebApplicationFirewallLog registra cualquier solicitud que coincida con una regla de WAF.

La siguiente consulta de ejemplo obtiene los registros de WAF en las solicitudes bloqueadas:

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

- Más información sobre [Front Door](front-door-overview.md).

