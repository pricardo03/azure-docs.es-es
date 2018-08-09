---
title: Orígenes de datos de supervisión en Azure | Microsoft Docs
description: Describe los datos disponibles para supervisar el estado y rendimiento de los recursos de Azure y las aplicaciones que se ejecutan en ellos.
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.service: monitoring
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/05/2018
ms.author: bwren
ms.openlocfilehash: 48cbfac78b41b47419799584837e094d45757628
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/08/2018
ms.locfileid: "39627464"
---
# <a name="sources-of-monitoring-data-in-azure"></a>Orígenes de datos de supervisión en Azure
En este artículo se describen los datos disponibles para supervisar el estado y rendimiento de los recursos de Azure y las aplicaciones que se ejecutan en ellos.  Recopilar y analizar estos datos con las herramientas descritas en [Collecting monitoring data in Azure](monitoring-data-collection.md) (Recopilar datos de supervisión en Azure)

La supervisión de datos en Azure procede de distintos orígenes que se pueden organizar en niveles, donde el nivel superior representa la aplicación y el inferior, la plataforma de Azure. Esto se muestra en el diagrama siguiente, con cada nivel descrito en detalle en las secciones siguientes.

![Niveles de datos de supervisión](media/monitoring-data-sources/monitoring-tiers.png)


## <a name="azure-platform"></a>Plataforma Azure
La telemetría relacionada con el estado y el funcionamiento de Azure incluye datos sobre el funcionamiento y la administración del inquilino o la suscripción de Azure. Incluye datos de Service Health almacenados en el registro de actividad de Azure y registros de auditoría de Azure Active Directory.

![Colección de Azure](media/monitoring-data-sources/azure-collection.png)

### <a name="azure-service-health"></a>Azure Service Health
[Azure Service Health](../monitoring-and-diagnostics/monitoring-service-notifications.md) proporciona información sobre el estado de los servicios de Azure de la suscripción de los que dependen la aplicación y los recursos. Puede crear alertas para que se le notifiquen los problemas críticos actuales y previstos que pueden afectar a la aplicación. Los registros de Service Health se almacenan en el [registro de actividad de Azure](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md), de modo que puede verlos en el explorador de registro de actividad y copiarlos en Log Analytics.

### <a name="azure-activity-log"></a>Azure Activity Log
El [registro de actividad de Azure](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) incluye registros de Service Health, además de registros sobre los cambios de configuración aplicados a los recursos de Azure. El registro de actividad está disponible para todos los recursos de Azure y representa su vista _externa_. Los tipos específicos de registros del registro de actividad se describen en el [esquema de eventos de registro de actividad de Azure](../monitoring-and-diagnostics/monitoring-activity-log-schema.md).

Puede ver el registro de actividad de un recurso determinado en su página de Azure Portal o ver registros de varios recursos en el [explorador de registro de actividad](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md). Resulta especialmente útil copiar las entradas del registro en Log Analytics para combinarlas con otros datos de supervisión. También puede enviarlas a otras ubicaciones mediante [Event Hubs](../monitoring-and-diagnostics/monitoring-stream-activity-logs-event-hubs.md).


### <a name="azure-active-directory-audit-logs"></a>Registros de auditoría de Azure Active Directory
Los [informes de Azure Active Directory](../active-directory/reports-monitoring/overview-reports.md), contienen el historial de actividad de inicio de sesión y la traza de auditoría de los cambios realizados en un inquilino determinado. Actualmente, no se pueden combinar datos de auditoría de Azure Active Directory con otros datos de supervisión, ya que solo está accesible a través de Azure Active Directory y la [API de informes de Azure Active Directory](../active-directory/reports-monitoring/concept-reporting-api.md).


## <a name="azure-services"></a>Servicios de Azure
Los registros de diagnóstico de nivel de recursos y las métricas proporcionan información sobre el funcionamiento _interno_ de recursos de Azure. Están disponibles para la mayoría de servicios de Azure, y las soluciones de administración proporcionan información adicional sobre determinados servicios.

![Colección de recursos de Azure](media/monitoring-data-sources/azure-resource-collection.png)


### <a name="metrics"></a>Métricas
La mayoría de servicios de Azure generan métricas que reflejan su rendimiento y funcionamiento. Las [métricas específicas varían en función del tipo de recurso](../monitoring-and-diagnostics/monitoring-supported-metrics.md).  Son accesibles desde el Explorador de métricas y se pueden copiar en Log Analytics para realizar análisis de tendencias y de otro tipo.


### <a name="resource-diagnostic-logs"></a>Registros de diagnóstico de recursos
El registro de actividad proporciona información sobre las operaciones realizadas en recursos de Azure, mientras que el nivel de recursos [Registros de diagnóstico](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) proporciona conclusiones sobre el funcionamiento del propio recurso.   Los requisitos de configuración y el contenido de estos registros [varía según el tipo de recurso](../monitoring-and-diagnostics/monitoring-diagnostic-logs-schema.md).

No se pueden ver directamente los registros de diagnóstico en Azure Portal, pero puede [enviarlos al almacenamiento de Azure para archivarlos](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md) y exportarlos al [centro de eventos](../event-hubs/event-hubs-what-is-event-hubs.md) para redirigirlos a otros servicios, o [a Log Analytics](../monitoring-and-diagnostics/monitor-stream-diagnostic-logs-log-analytics.md) para analizarlos. Algunos recursos pueden escribir directamente en Log Analytics, mientras que otros escriben en una cuenta de almacenamiento antes de [importarse a Log Analytics](../log-analytics/log-analytics-azure-storage-iis-table.md#use-the-azure-portal-to-collect-logs-from-azure-storage).

### <a name="management-solutions"></a>Soluciones de administración
 Las [soluciones de administración](../monitoring/monitoring-solutions.md) recopilan datos para proporcionar conclusiones adicionales sobre el funcionamiento de un servicio determinado. Recopilan datos en Log Analytics, donde se pueden analizar mediante un [lenguaje de consulta](../log-analytics/log-analytics-log-search.md) o vistas que se suelen incluir en la solución.

## <a name="guest-operating-system"></a>Sistema operativo invitado
Además de la telemetría que generan todos los servicios de Azure, los recursos de proceso tienen un sistema operativo invitado para supervisar. Con la instalación de uno o más agentes, puede recopilar datos de telemetría del invitado en las mismas herramientas de supervisión que los propios servicios de Azure.

![Colección de recursos de proceso de Azure](media/monitoring-data-sources/compute-resource-collection.png)

### <a name="diagnostic-extension"></a>Extensión de diagnóstico
Con la [extensión de Azure Diagnostics](../monitoring-and-diagnostics/azure-diagnostics.md), puede recopilar registros y datos de rendimiento del sistema operativo cliente de los recursos de proceso de Azure. Las métricas y los registros que se recopilan de los clientes se almacenan en una cuenta de almacenamiento cuya [importación puede configurar desde Log Analytics](../log-analytics/log-analytics-azure-storage-iis-table.md#use-the-azure-portal-to-collect-logs-from-azure-storage).  El Explorador de métricas comprende cómo se leen datos de la cuenta de almacenamiento e incluye métricas de cliente junto con otras métricas recopiladas.


### <a name="log-analytics-agent"></a>Agente de Log Analytics
Puede instalar al agente de Log Analytics en cualquier máquina virtual o equipo físico Windows o Linux. La máquina virtual se puede ejecutar en Azure, en otra nube o en el entorno local.  El agente se conecta a Log Analytics directamente o mediante un [grupo de administración de System Center Operations Manager conectado](../log-analytics/log-analytics-om-agents.md) y le permite recopilar datos de [orígenes de datos](../log-analytics/log-analytics-data-sources.md) que configura o de [soluciones de administración](../monitoring/monitoring-solutions.md) que proporcionan conclusiones adicionales sobre las aplicaciones que se ejecutan en la máquina virtual.

### <a name="service-map"></a>Mapa de servicio
[Service Map](../operations-management-suite/operations-management-suite-service-map.md) requiere una instancia de Dependency Agent en máquinas virtuales Windows y Linux. Esto funciona con el agente de Log Analytics para recopilar datos acerca de los procesos que se ejecutan en la máquina virtual y las dependencias de los procesos externos. Almacena estos datos en Log Analytics e incluye una consola que muestra visualmente los datos que recopila, además de otros datos almacenados en Log Analytics.

## <a name="applications"></a>APLICACIONES
Además de la telemetría que la aplicación puede escribir en el sistema operativo invitado, la supervisión detallada de la aplicación se realiza con [Application Insights](https://docs.microsoft.com/azure/application-insights/). Application Insights pueden recopilar datos de aplicaciones que se ejecutan en una variedad de plataformas. La aplicación se puede ejecutar en Azure, en otra nube o en el entorno local.

![Recopilación de datos de aplicación](media/monitoring-data-sources/application-collection.png)


#### <a name="application-data"></a>Datos de aplicación
Cuando se habilita Application Insights para una aplicación mediante la instalación de un paquete de instrumentación, recopila métricas y registros relacionados con el rendimiento y el funcionamiento de la aplicación. Esto incluye información detallada acerca de las vistas de página, las solicitudes de la aplicación y las excepciones. Application Insights almacena los datos que recopila en las métricas de Azure y en Log Analytics. Incluye completas herramientas para analizar estos datos, pero también puede analizarlos con datos de otros orígenes mediante herramientas como el Explorador de métricas y las búsquedas de registros.

También puede usar Application Insights para [crear una métrica personalizada](../application-insights/app-insights-api-custom-events-metrics.md).  Esto le permite definir su propia lógica para calcular un valor numérico y, a continuación, almacenarlo con otras métricas a las que se puede acceder desde el Explorador de métricas y que se pueden usar para el [Escalado automático](../monitoring-and-diagnostics/monitoring-autoscale-scale-by-custom-metric.md) y las alertas de métricas.

#### <a name="dependencies"></a>Dependencias
Para supervisar las distintas operaciones lógicas de una aplicación, debe [recopilar datos de telemetría de varios componentes](../application-insights/app-insights-transaction-diagnostics.md). Application Insights admite la [correlación de telemetría distribuida](../application-insights/application-insights-correlation.md) que identifica las dependencias entre componentes y le permite analizarlas juntos.

#### <a name="availability-tests"></a>Pruebas de disponibilidad
Las [pruebas de disponibilidad](../application-insights/app-insights-monitor-web-app-availability.md) de Application Insights le permiten probar la disponibilidad y capacidad de respuesta de la aplicación desde diferentes ubicaciones de la red Internet pública. Puede realizar una prueba sencilla de ping para comprobar que la aplicación está activa o usar Visual Studio para crear una prueba web que simule un escenario de usuario.  Las pruebas de disponibilidad no requieren ninguna instrumentación en la aplicación.

## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre los [tipos de datos de supervisión y las herramientas de Azure](monitoring-data-collection.md) que se usan para recopilarlos y analizarlos.
