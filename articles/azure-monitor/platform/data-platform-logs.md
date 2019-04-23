---
title: Inicia sesión en Azure Monitor | Microsoft Docs
description: Describe los registros en Azure Monitor que se usan para realizar análisis avanzados de datos de supervisión.
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.service: monitoring
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 0203/26/2019
ms.author: bwren
ms.openlocfilehash: ec037b16840afe669ac3934beaa832f850cdcfb0
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/22/2019
ms.locfileid: "60003690"
---
# <a name="logs-in-azure-monitor"></a>Registros en Azure Monitor

> [!NOTE]
> Todos los datos recopilados por Azure Monitor se adapta a uno de dos tipos fundamentales, métricas y registros. Este artículo describe los registros. Consulte [métricas en Azure Monitor](data-platform-metrics.md) para obtener una descripción detallada de las métricas y a [datos de supervisión recopilados por Azure Monitor](data-platform.md) para ver una comparación de los dos.

Los registros en Azure Monitor son especialmente útiles para llevar a cabo un análisis complejo de datos desde una variedad de orígenes. Este artículo describe cómo los registros se estructuran en Azure Monitor, lo que puede hacer con los datos e identifica los distintos orígenes de datos que almacenan datos en los registros.

> [!NOTE]
> Es importante distinguir entre registros de Azure Monitor y orígenes de datos de registro en Azure. Por ejemplo, se escriben los eventos de nivel de suscripción de Azure en un [registro de actividad](activity-logs-overview.md) que puede ver en el menú de Azure Monitor. La mayoría de los recursos escriben información operativa en un [registro de diagnóstico](diagnostic-logs-overview.md) que pueda reenviar a ubicaciones diferentes. Registros de Azure Monitor es una plataforma de datos de registro que recopila los registros de actividad y los registros de diagnóstico, junto con otros datos de supervisión para proporcionar un análisis profundo a través de todo el conjunto de recursos.

## <a name="what-are-azure-monitor-logs"></a>¿Cuáles son los registros de Azure Monitor?

Los registros en Azure Monitor contienen distintos tipos de datos que se organizan en registros con diferentes conjuntos de propiedades para cada tipo. Los registros pueden contener valores numéricos, como las métricas en Microsoft Azure pero normalmente contienen datos de texto con descripciones detalladas. Aún más difieren de los datos de métricas que varían en su estructura y a menudo no se recopilan a intervalos regulares. Se almacenan los datos de telemetría, como eventos y seguimientos de registros de Azure Monitor, además de los datos de rendimiento para que pueda todos combinarse para el análisis.

Un tipo común de entrada del registro es un evento, que se recopila de forma esporádica. Los eventos se crean mediante una aplicación o servicio y suelen incluyen información suficiente para proporcionar un contexto completo por sí solos. Por ejemplo, un evento puede indicar que se creó o modificó un recurso determinado, se inició un nuevo host en respuesta a un aumento de tráfico o se detectó un error en una aplicación.

 Dado que el formato de los datos puede variar, las aplicaciones pueden crear registros personalizados con la estructura que necesiten. Incluso se pueden almacenar datos de métricas de registros para combinarlos con otros datos de supervisión para detectar tendencias y otros análisis de datos.


## <a name="what-can-you-do-with-azure-monitor-logs"></a>¿Qué puede hacer con registros de Azure Monitor?
En la tabla siguiente se enumera las distintas formas que puede usar los registros en Azure Monitor.


|  |  |
|:---|:---|
| Análisis | Use [Log Analytics](../log-query/get-started-portal.md) en el portal de Azure para escribir [registrar consultas](../log-query/log-query-overview.md) y analizar datos de registro mediante el potente motor de análisis de explorador de datos de forma interactiva.<br>Use la [consola de Application Insights analytics](../app/analytics.md) en el portal de Azure para escribir consultas de registros y analizar datos de registro de Application Insights de forma interactiva. |
| Visualizar | Anclar los resultados de la consulta se representa como tablas o gráficos a un [panel Azure](../../azure-portal/azure-portal-dashboards.md).<br>Crear un [libro](../app/usage-workbooks.md) para combinar con varios conjuntos de datos en un informe interactivo. <br>Exportar los resultados de una consulta a [Power BI](powerbi.md) para usar diferentes visualizaciones y compartirlos con usuarios fuera de Azure.<br>Exportar los resultados de una consulta a [Grafana](grafana-plugin.md) aprovechar sus paneles y combinar con otros orígenes de datos.|
| Alerta | Configurar un [regla de alerta de registro](alerts-log.md) que envíe una notificación o realice [una acción automatizada](action-groups.md) cuando los resultados de la consulta coincidan con un resultado concreto.<br>Configurar un [regla de alerta de métrica](alerts-metric-logs.md) en ciertos registros de datos de registro extraídos como métricas. |
| Recuperar | Obtener acceso a los resultados de consulta de registro desde una línea de comandos mediante [CLI de Azure](/cli/azure/ext/log-analytics/monitor/log-analytics).<br>Obtener acceso a los resultados de consulta de registro desde una línea de comandos mediante [cmdlets de PowerShell](https://docs.microsoft.com/powershell/module/az.operationalinsights).<br>Obtener acceso a los resultados de la consulta de registro de una aplicación personalizada con [API de REST](https://dev.loganalytics.io/). |
| Exportación | Crear un flujo de trabajo para recuperar datos de registro y cópielo en una ubicación externa mediante [Logic Apps](~/articles/logic-apps/index.yml). |


## <a name="how-is-data-in-azure-monitor-logs-structured"></a>¿Es cómo los datos en Azure Monitor registros estructurado?
Datos recopilados por los registros de Azure Monitor se almacenan en un [área de trabajo de Log Analytics](../platform/manage-access.md). También puede [crear varias áreas de trabajo](manage-access.md#determine-the-number-of-workspaces-you-need) en su suscripción para administrar distintos conjuntos de datos de registro. Cada área de trabajo contiene varias tablas que se almacenan los datos de un origen determinado. Aunque todas las tablas comparten [algunas propiedades comunes](log-standard-properties.md), cada uno tiene un único conjunto de propiedades según el tipo de datos que almacena. Una nueva área de trabajo tendrá un conjunto estándar de tablas, y se agregarán más tablas diferentes soluciones de supervisión y otros servicios que escriben en el área de trabajo.

Datos de registro de Application Insights utilizan el mismo motor de Log Analytics como áreas de trabajo, pero se almacena por separado para cada aplicación supervisada. Cada aplicación tiene un conjunto estándar de las tablas para almacenar los datos, como las vistas de página, excepciones y solicitudes de aplicación.

Consultas de registros le ser usar datos de un área de trabajo de Log Analytics o una aplicación de Application Insights. Puede usar un [consulta entre recursos](../log-query/cross-workspace-query.md) para analizar datos de la aplicación junto con otros datos de registro o para crear consultas que incluyen varias áreas de trabajo o aplicaciones.

![Áreas de trabajo](media/data-platform-logs/workspaces.png)

## <a name="log-queries"></a>Consultas de registros
Se recuperan datos en registros de Azure Monitor mediante un [consulta de registro](../log-query/log-query-overview.md) escrito con el [lenguaje de consulta Kusto](../log-query/get-started-queries.md), lo que permite recuperar rápidamente, consolidar y analizar los datos recopilados. Use [Log Analytics](../log-query/portals.md) para escribir y probar las consultas de registros en el portal de Azure. Permite trabajar con los resultados de forma interactiva o anclarlos a un panel para verlos con otras visualizaciones.

![Log Analytics](media/data-platform-logs/log-analytics.png)

Abra [Log Analytics de Application Insights](../app/analytics.md) para analizar datos de Application Insights.

![Application Insights Analytics](media/data-platform-logs/app-insights-analytics.png)

También puede recuperar datos de registro mediante el uso de la [API de Log Analytics](https://dev.loganalytics.io/documentation/overview) y [API de REST de Application Insights](https://dev.applicationinsights.io/documentation/overview).


## <a name="sources-of-azure-monitor-logs"></a>Orígenes de los registros de Azure Monitor
Azure Monitor puede recopilar datos de registro de diversos orígenes, tanto en Azure como desde recursos locales. Las tablas siguientes muestran los distintos orígenes de datos disponibles en diferentes recursos que escriben datos en registros de Azure Monitor. Cada uno tiene un vínculo para obtener información detallada sobre cualquier configuración necesaria.

### <a name="azure-tenant-and-subscription"></a>Suscripción y el inquilino de azure

| Datos | DESCRIPCIÓN |
|:---|:---|
| Registros de auditoría de Azure Active Directory | Configurar a través de la configuración de diagnóstico para cada directorio. Consulte [registros de integración de Azure AD con registros de Azure Monitor](../../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md).  |
| Registros de actividad | Almacenan por separado de forma predeterminada y puede usarse para cerca de alertas en tiempo real. Instale la solución Activity Log Analytics para escribir en el área de trabajo de Log Analytics. Consulte [recopilar y analizar los registros de actividad de Azure en Log Analytics](collect-activity-logs.md). |

### <a name="azure-resources"></a>Recursos de Azure

| Datos | DESCRIPCIÓN |
|:---|:---|
| Diagnóstico de recursos | Configurar opciones de diagnóstico para escribir en datos de diagnóstico, incluidas las métricas para un área de trabajo de Log Analytics. Consulte [Stream registros de diagnóstico de Azure a Log Analytics](diagnostic-logs-stream-log-store.md). |
| Soluciones de supervisión | Soluciones de supervisión de escriben los datos que se recopilan a su área de trabajo de Log Analytics. Consulte [detalles de la recopilación de datos para soluciones de administración de Azure](../insights/solutions-inventory.md) para obtener una lista de soluciones. Consulte [supervisión de soluciones en Azure Monitor](../insights/solutions.md) para obtener más información sobre la instalación y uso de soluciones. |
| Métricas | Enviar métricas de la plataforma para recursos de Azure Monitor a un área de trabajo de Log Analytics para conservar los datos de registro durante períodos más largos y llevar a cabo un análisis complejo con otros tipos de datos mediante el [lenguaje de consulta Kusto](/azure/kusto/query/). Consulte [Stream registros de diagnóstico de Azure a Log Analytics](diagnostic-logs-stream-log-store.md). |
| Almacenamiento de tablas de Azure | Recopilar datos de Azure storage donde escriben algunos recursos de Azure los datos de supervisión. Consulte [almacenamiento de blobs de Azure de uso para IIS y Azure table storage para eventos con Log Analytics](azure-storage-iis-table.md). |

### <a name="virtual-machines"></a>Virtual Machines

| Datos | DESCRIPCIÓN |
|:---|:---|
|  Orígenes de datos de agentes | Orígenes de datos recopilan de [Windows](agent-windows.md) y [Linux](../learn/quick-collect-linux-computer.md) agentes incluyen eventos, datos de rendimiento y los registros personalizados. Consulte [orígenes de datos del agente de Azure Monitor](data-sources.md) para obtener una lista de orígenes de datos y los detalles de configuración. |
| Soluciones de supervisión | Soluciones de supervisión de escriben los datos que se recopilan desde los agentes a su área de trabajo de Log Analytics. Consulte [detalles de la recopilación de datos para soluciones de administración de Azure](../insights/solutions-inventory.md) para obtener una lista de soluciones. Consulte [supervisión de soluciones en Azure Monitor](../insights/solutions.md) para obtener más información sobre la instalación y uso de soluciones. |
| System Center Operations Manager | Conecte el grupo de administración de Operations Manager a Azure Monitor para recopilar datos de rendimiento y eventos desde agentes locales en los registros. Consulte [conectar Operations Manager a Log Analytics](om-agents.md) para obtener más información sobre esta configuración. |


### <a name="applications"></a>APLICACIONES

| Datos | DESCRIPCIÓN |
|:---|:---|
| Solicitudes y excepciones | Datos detallados sobre las solicitudes de aplicación y las excepciones que se encuentran en el _solicitudes_, _pageViews_, y _excepciones_ tablas. Las llamadas a [componentes externos](../app/asp-net-dependencies.md) están en el _dependencias_ tabla. |
| Uso y rendimiento | El rendimiento de la aplicación está disponible en el _solicitudes_, _browserTimings_ y _performanceCounters_ tablas. Datos de [métricas personalizadas](../app/api-custom-events-metrics.md#trackevent) está en el _customMetrics_ tabla.|
| Datos de seguimiento | Resultado de [seguimiento distribuido](../app/distributed-tracing.md) se almacenan en el _seguimientos_ tabla. |
| Pruebas de disponibilidad | Datos de resumen de [pruebas de disponibilidad](../app/monitor-web-app-availability.md) se almacena en el _availabilityResults_ tabla. Datos detallados de estas pruebas se encuentran en el almacenamiento independiente y tiene acceso desde Application Insights en Azure portal. |

### <a name="insights"></a>Información detallada

| Datos | DESCRIPCIÓN |
|:---|:---|
| Azure Monitor para contenedores | Datos de inventario y de rendimiento recopilan por [Azure Monitor para contenedores](../insights/container-insights-overview.md). Consulte [los detalles de recopilación de datos del contenedor](../insights/container-insights-log-search.md#container-records) para obtener una lista de las tablas. |
| Azure Monitor para máquinas virtuales | Datos de asignación y de rendimiento recopilan por [Azure Monitor para las máquinas virtuales](../insights/vminsights-overview.md). Consulte [cómo consultar los registros de Azure Monitor para las máquinas virtuales](../insights/vminsights-log-search.md) para obtener más información sobre las consultas de estos datos. |

### <a name="custom"></a>Personalizado 

| Datos | DESCRIPCIÓN |
|:---|:---|
| API DE REST | Escribir datos en un área de trabajo de Log Analytics desde cualquier cliente REST. Consulte [enviar datos de registro a Azure Monitor con la API HTTP Data Collector](data-collector-api.md) para obtener más información.
| Aplicación lógica | Escribir datos en un área de trabajo de Log Analytics desde un flujo de trabajo de aplicación lógica con el **recopilador de datos de Azure Log Analytics** acción. |

### <a name="security"></a>Seguridad

| Datos | DESCRIPCIÓN |
|:---|:---|
| Azure Security Center | [Azure Security Center](/azure/security-center/) almacena los datos que recopila en un área de trabajo de Log Analytics donde se puede analizar con otros datos del registro. Consulte [recopilación de datos en Azure Security Center](../../security-center/security-center-enable-data-collection.md) para obtener más información sobre la configuración de área de trabajo. |
| Azure Sentinel | [Azure centinela](/azure/sentinel/) almacena los datos de orígenes de datos en un área de trabajo de Log Analytics. Consulte [conectar orígenes de datos](/azure/sentinel/connect-data-sources).  |


## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre la [plataforma de datos de Azure Monitor](data-platform.md).
- Obtenga información sobre [métricas en Azure Monitor](data-platform-metrics.md).
- Obtenga información sobre la [supervisión de datos disponible](data-sources.md) para diferentes recursos en Azure.
