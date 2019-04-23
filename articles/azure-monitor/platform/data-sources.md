---
title: Orígenes de datos en Azure Monitor | Microsoft Docs
description: Describe los datos disponibles para supervisar el estado y rendimiento de los recursos de Azure y las aplicaciones que se ejecutan en ellos.
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.service: azure-monitor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/13/2018
ms.author: bwren
ms.openlocfilehash: 6d03c219025c8cd39214bd8ab6807125709f9742
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59790883"
---
# <a name="sources-of-data-in-azure-monitor"></a>Orígenes de datos en Azure Monitor
En este artículo se describen los orígenes de datos que recopila Azure Monitor para supervisar el mantenimiento y el rendimiento de los recursos y las aplicaciones que se ejecutan en ellos. Estos recursos pueden estar en Azure, en otra nube o en el entorno local.  Consulte [Datos recopilados por Azure Monitor](data-platform.md) para obtener más información sobre cómo se almacenan estos datos y cómo puede verlos.

La supervisión de datos en Azure procede de distintos orígenes que pueden organizarse en niveles, donde el nivel superior representa la aplicación y los sistemas operativos, y los niveles inferiores son componentes de la plataforma de Azure. Esto se muestra en el diagrama siguiente, con cada nivel descrito en detalle en las secciones siguientes.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

![Niveles de datos de supervisión](media/data-sources/monitoring-tiers.png)

## <a name="azure-tenant"></a>Inquilino de Azure
Los datos de telemetría relacionados con el inquilino de Azure se recopilan desde los servicios de todos los inquilinos, como Azure Active Directory.

![Recopilación del inquilino de Azure](media/data-sources/tenant-collection.png)

### <a name="azure-active-directory-audit-logs"></a>Registros de auditoría de Azure Active Directory
Los [informes de Azure Active Directory](../../active-directory/reports-monitoring/overview-reports.md), contienen el historial de actividad de inicio de sesión y la traza de auditoría de los cambios realizados en un inquilino determinado. Estos registros de auditoría se pueden escribir en Azure Monitor para analizarlos junto con otros datos de registro.


## <a name="azure-platform"></a>Plataforma Azure
La telemetría relacionada con el estado y el funcionamiento de Azure incluye datos sobre el funcionamiento y la administración de la suscripción a Azure. Incluye datos de Service Health almacenados en el registro de actividad de Azure y registros de auditoría de Azure Active Directory.

![Recopilación de la suscripción a Azure](media/data-sources/azure-collection.png)

### <a name="azure-service-health"></a>Azure Service Health
[Azure Service Health](service-notifications.md) proporciona información sobre el estado de los servicios de Azure de la suscripción de los que dependen la aplicación y los recursos. Puede crear alertas para que se le notifiquen los problemas críticos actuales y previstos que pueden afectar a la aplicación. Los registros de Service Health se almacenan en el [registro de actividad de Azure](activity-logs-overview.md), de modo que pueda verlos en el explorador de registro de actividad y copiarlos en registros de Azure Monitor.

### <a name="azure-activity-log"></a>Azure Activity Log
El [registro de actividad de Azure](activity-logs-overview.md) incluye registros de Service Health, además de registros sobre los cambios de configuración aplicados a los recursos de Azure. El registro de actividad está disponible para todos los recursos de Azure y representa su vista _externa_. Los tipos específicos de registros del registro de actividad se describen en el [esquema de eventos de registro de actividad de Azure](activity-log-schema.md).

Puede ver el registro de actividad de un recurso determinado en su página de Azure Portal o ver registros de varios recursos en el [explorador de registro de actividad](activity-logs-overview.md). Resulta especialmente útil copiar las entradas del registro en Azure Monitor para combinarlas con otros datos de supervisión. También puede enviarlas a otras ubicaciones mediante [Event Hubs](activity-logs-stream-event-hubs.md).



## <a name="azure-services"></a>Servicios de Azure
Los registros de diagnóstico de nivel de recursos y las métricas proporcionan información sobre el funcionamiento _interno_ de recursos de Azure. Están disponibles para la mayoría de servicios de Azure, y las soluciones de administración proporcionan información adicional sobre determinados servicios.

![Colección de recursos de Azure](media/data-sources/azure-resource-collection.png)


### <a name="metrics"></a>Métricas
La mayoría de los servicios de Azure generarán [métricas de plataforma](data-platform-metrics.md) que reflejan su rendimiento y funcionamiento. Las [métricas específicas varían en función del tipo de recurso](metrics-supported.md).  Son accesibles desde análisis de métricas y se pueden copiar en registros para realizar análisis de tendencias y de otro tipo con Log Analytics.


### <a name="resource-diagnostic-logs"></a>Registros de diagnóstico de recursos
El registro de actividad proporciona información sobre las operaciones realizadas en recursos de Azure, mientras que el nivel de recursos [Registros de diagnóstico](diagnostic-logs-overview.md) proporciona conclusiones sobre el funcionamiento del propio recurso.   Los requisitos de configuración y el contenido de estos registros [varía según el tipo de recurso](diagnostic-logs-schema.md).

No se pueden ver directamente los registros de diagnóstico en Azure Portal, pero puede [enviarlos al almacenamiento de Azure para archivarlos](archive-diagnostic-logs.md) y exportarlos al [centro de eventos](../../event-hubs/event-hubs-about.md) para redirigirlos a otros servicios, o [a Azure Monitor](diagnostic-logs-stream-log-store.md) para analizarlos. Algunos recursos pueden escribir directamente en Azure Monitor, mientras que otros escriben en una cuenta de almacenamiento antes de [importarse a Log Analytics](azure-storage-iis-table.md#use-the-azure-portal-to-collect-logs-from-azure-storage).

### <a name="monitoring-solutions"></a>Soluciones de supervisión
 Las [soluciones de supervisión](../../azure-monitor/insights/solutions.md) recopilan datos para proporcionar conclusiones adicionales sobre el funcionamiento de un servicio o aplicación en particular. Recopilan datos en registros de Azure Monitor, donde se pueden analizar mediante un [lenguaje de consulta](../../azure-monitor/log-query/log-query-overview.md) o [vistas](view-designer.md) que se suelen incluir en la solución.


## <a name="guest-operating-system"></a>Sistema operativo invitado
Los recursos de proceso en Azure, en otras nubes y en el entorno local tienen un sistema operativo invitado para supervisar. Con la instalación de uno o más agentes, puede recopilar datos de telemetría del invitado en las mismas herramientas de supervisión que los propios servicios de Azure.

![Colección de recursos de proceso de Azure](media/data-sources/compute-resource-collection.png)

### <a name="azure-diagnostic-extension"></a>Extensión de diagnóstico de Azure
Con la extensión de Azure Diagnostics, se proporciona un nivel de supervisión básico al recopilar registros y datos de rendimiento del sistema operativo cliente de los recursos de proceso de Azure.   

### <a name="log-analytics-agent"></a>Agente de Log Analytics
El agente de Log Analytics proporciona supervisión y administración completas de las máquinas virtuales o equipos físicos de Windows y Linux. La máquina virtual se puede ejecutar en Azure, otra nube o de forma local, y el agente se conecta a Azure Monitor directamente o mediante System Center Operations Manager, y permite recopilar datos de [orígenes de datos](agent-data-sources.md) que configure, o bien de [soluciones de supervisión](../../azure-monitor/insights/solutions.md) que proporcionan conclusiones adicionales sobre las aplicaciones que se ejecutan en la máquina virtual.


### <a name="dependency-agent"></a>Dependency Agent
[Service Map](../insights/service-map.md) y [Azure Monitor para VM](../../azure-monitor/insights/vminsights-overview.md) requieren una instancia de Dependency Agent en máquinas virtuales de Windows y Linux. Se integra con el agente de Log Analytics para recopilar datos detectados acerca de los procesos que se ejecutan en la máquina virtual y las dependencias de los procesos externos. Almacena estos datos en Azure Monitor y visualiza los componentes interconectados detectados.  

Para comprender mejor las diferencias entre los agentes y cuáles debe usar según sus requisitos de supervisión, consulte la [información general sobre los agentes de supervisión](agents-overview.md).

## <a name="applications"></a>APLICACIONES
Además de la telemetría que la aplicación puede escribir en el sistema operativo invitado, la supervisión detallada de la aplicación se realiza con [Application Insights](https://docs.microsoft.com/azure/application-insights/). Application Insights pueden recopilar datos de aplicaciones que se ejecutan en una variedad de plataformas. La aplicación se puede ejecutar en Azure, en otra nube o en el entorno local.

![Recopilación de datos de aplicación](media/data-sources/application-collection.png)


### <a name="application-data"></a>Datos de aplicación
Cuando se habilita Application Insights para una aplicación mediante la instalación de un paquete de instrumentación, recopila métricas y registros relacionados con el rendimiento y el funcionamiento de la aplicación. Esto incluye información detallada acerca de las vistas de página, las solicitudes de la aplicación y las excepciones. Application Insights almacena los datos que recopila en Azure Monitor. Incluye herramientas completas para analizar estos datos, pero también se pueden analizar con datos de otros orígenes mediante herramientas como el análisis de métricas y Log Analytics.

También puede usar Application Insights para [crear una métrica personalizada](../../application-insights/app-insights-api-custom-events-metrics.md).  Esto le permite definir su propia lógica para calcular un valor numérico y, después, almacenarlo con otras métricas a las que se puede acceder desde el análisis de métricas y que se pueden usar para el [Escalado automático](autoscale-custom-metric.md) y las alertas de métricas.


### <a name="dependencies"></a>Dependencias
Para supervisar las distintas operaciones lógicas de una aplicación, debe [recopilar datos de telemetría de varios componentes](../../application-insights/app-insights-transaction-diagnostics.md). Application Insights admite la [correlación de telemetría distribuida](../../application-insights/application-insights-correlation.md) que identifica las dependencias entre componentes y le permite analizarlas juntos.

### <a name="availability-tests"></a>Pruebas de disponibilidad
Las [pruebas de disponibilidad](../../application-insights/app-insights-monitor-web-app-availability.md) de Application Insights le permiten probar la disponibilidad y capacidad de respuesta de la aplicación desde diferentes ubicaciones de la red Internet pública. Puede realizar una prueba sencilla de ping para comprobar que la aplicación está activa o usar Visual Studio para crear una prueba web que simule un escenario de usuario.  Las pruebas de disponibilidad no requieren ninguna instrumentación en la aplicación.

## <a name="custom-sources"></a>Orígenes personalizados
Además de los niveles estándar de una aplicación, puede que necesite supervisar otros recursos que tengan datos de telemetría y que no puedan recopilarse con los otros orígenes de datos. Para estos recursos, deberá escribir estos datos mediante una API de Azure Monitor.

![Recopilación de datos personalizados](media/data-sources/custom-collection.png)

### <a name="data-collector-api"></a>API de recopilador de datos
Azure Monitor puede recopilar datos de registro de cualquier cliente de REST mediante [Data Collector API](../../azure-monitor/platform/data-collector-api.md). Esto permite crear escenarios de supervisión personalizados y ampliar la supervisión a los recursos que no exponen datos de telemetría en otros orígenes.

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre la [tipos de datos de supervisión recopilados por Azure Monitor](data-platform.md) y cómo ver y analizar estos datos.
