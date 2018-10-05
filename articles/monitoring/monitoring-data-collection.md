---
title: Datos de supervisión recopilados por Azure Monitor | Microsoft Docs
description: Los datos de supervisión recopilados por Azure Monitor se dividen en métricas que son ligeras y capaces de admitir escenarios casi en tiempo real y registros que se almacenan en Log Analytics para realizar análisis avanzados.
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.service: monitoring
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/27/2018
ms.author: bwren
ms.openlocfilehash: 19a611ca88310f06503bea2b8606699fe3e1c709
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2018
ms.locfileid: "47406047"
---
# <a name="monitoring-data-collected-by-azure-monitor"></a>Datos de supervisión recopilados por Azure Monitor
[Azure Monitor](../azure-monitor/overview.md) es un servicio que le ayuda a supervisar sus aplicaciones y los recursos de los que dependen. Lo fundamental de esta función es el almacenamiento de datos de telemetría y otros datos de los recursos supervisados. En este artículo se proporciona una descripción completa de cómo Azure Monitor almacena y usa estos datos.

Todos los datos recopilados por Azure Monitor pueden clasificarse como uno de los dos tipos fundamentales: [métricas](#metrics) y [registros](#logs). Las métricas son valores numéricos que describen algunos aspectos de un sistema en un momento dado en el tiempo. Las métricas son ligeras y capaces de admitir escenarios de tiempo casi real. Los registros contienen distintos tipos de datos organizados en registros con diferentes conjuntos de propiedades para cada tipo. Los datos de telemetría, como los eventos y los seguimientos, se almacenan como registros junto con los datos de rendimiento para poder analizarlos de forma combinada.

![Introducción a Azure Monitor](../azure-monitor/media/overview/overview.png)

## <a name="metrics"></a>Métricas
Las métricas son valores numéricos que describen algunos aspectos de un sistema en un momento dado. Las métricas son ligeras y capaces de admitir escenarios de tiempo casi real. Las métricas se recopilan a intervalos regulares independientemente de si cambia el valor o no. Son útiles para las alertas, dado que se pueden muestrear con frecuencia y se puede activar una alerta con rapidez con una lógica relativamente sencilla. 

Por ejemplo, puede recopilar la utilización del procesador de una máquina virtual cada minuto o el número de usuarios que han iniciado sesión en su aplicación cada 10 minutos. Puede activar una alerta cuando uno de los valores recopilados o incluso la diferencia entre dos valores supere un umbral definido.

Algunos atributos específicos de las métricas en Azure son:

* Los recopilados con una frecuencia de un minuto, a menos que se especifique lo contrario en la definición de la métrica.
* Los identificados de manera exclusiva mediante un nombre de métrica y un espacio de nombres que actúa como una categoría.
* Se almacenan durante 93 días. Puede copiar las métricas en Log Analytics para obtener tendencias a largo plazo.

Cada valor de métrica tiene las siguientes propiedades:
* Hora en que se recopiló el valor.
* Tipo de medida que el valor representa.
* El recurso con el que el valor está asociado.
* El propio valor.
* Algunas métricas pueden tener varias dimensiones, como se describe en la sección siguiente. Las métricas personalizadas pueden tener hasta 10 dimensiones.

### <a name="multi-dimensional-metrics"></a>Métricas multidimensionales
Las dimensiones de una métrica son pares nombre-valor que transportan datos adicionales para describir el valor de la métrica. Por ejemplo, una métrica _Espacio disponible en disco_ puede tener una dimensión denominada _Unidad_ con los valores _C:_ o _D:_, que permitiría ver cualquier espacio disponible en disco de todas las unidades o de cada unidad individual. 

En el ejemplo siguiente se muestran dos conjuntos de datos para una métrica hipotética denominada _Rendimiento de la red_. El primer conjunto de datos no tiene ninguna dimensión. El segundo conjunto de datos muestra los valores con dos dimensiones, _Dirección I_P y _Dirección_:

### <a name="network-throughput"></a>Rendimiento de la red

 |Timestamp        | Valor de métrica | 
   | ------------- |:-------------| 
   | 9/8/2017 8:14 | 1331,8 Kbps | 
   | 9/8/2017 8:15 | 1141,4 Kbps |
   | 9/8/2017 8:16 | 1110,2 Kbps |

Esta métrica no dimensional solo puede responder una pregunta básica, como “¿cuál era el rendimiento de la red en un momento dado?”.

### <a name="network-throughput--two-dimensions-ip-and-direction"></a>Rendimiento de la red + dos dimensiones (“IP” y “Dirección”)

| Timestamp          | Dimensión “IP” | Dimensión “Dirección” | Valor de métrica| 
   | ------------- |:-----------------|:------------------- |:-----------|  
   | 9/8/2017 8:14 | IP="192.168.5.2" | Dirección="Envío"    | 646,5 Kbps |
   | 9/8/2017 8:14 | IP="192.168.5.2" | Dirección="Recepción" | 420,1 Kbps |
   | 9/8/2017 8:14 | IP="10.24.2.15"  | Dirección="Envío"    | 150,0 Kbps | 
   | 9/8/2017 8:14 | IP="10.24.2.15"  | Dirección="Recepción" | 115,2 Kbps |
   | 9/8/2017 8:15 | IP="192.168.5.2" | Dirección="Envío"    | 515,2 Kbps |
   | 9/8/2017 8:15 | IP="192.168.5.2" | Dirección="Recepción" | 371,1 Kbps |
   | 9/8/2017 8:15 | IP="10.24.2.15"  | Dirección="Envío"    | 155,0 Kbps |
   | 9/8/2017 8:15 | IP="10.24.2.15"  | Dirección="Recepción" | 100,1 Kbps |

Esta métrica puede responder a preguntas como “¿cuál era el rendimiento de la red para cada dirección IP?” y “¿cuántos datos se enviaron en comparación los datos que se recibieron?”. Las métricas multidimensionales llevan valores analíticos y de diagnóstico adicionales en comparación con las métricas no dimensionales.

### <a name="value-of-metrics"></a>Valor de las métricas
Normalmente, las métricas individuales ofrecen poca información por sí solas. Proporcionan un único valor sin ningún contexto que no sea la comparación con un simple umbral. Sin embargo, son valiosas cuando se combinan con otras métricas para identificar patrones y tendencias, o cuando se combinan con registros que proporcionan contexto relacionado con valores concretos. 

Por ejemplo, un número determinado de usuarios de la aplicación en un momento dado pueden ofrecerle poca información sobre el estado de la aplicación. Pero un descenso repentino de usuarios indicado por varios valores de la misma métrica podría indicar un problema. Demasiadas excepciones iniciadas por la aplicación e indicadas por una métrica independiente pueden identificar un problema de la aplicación que provoque el descenso. Los eventos que la aplicación crea para identificar errores en sus componentes pueden ayudarle a identificar la causa raíz.

### <a name="sources-of-metric-data"></a>Orígenes de datos de métricas
Las métricas que Azure Monitor recopila proceden de tres orígenes fundamentales. Todas estas métricas están disponibles en el almacén de métricas, donde se pueden evaluar juntas independientemente de su origen.

Las **métricas de plataforma** se crean mediante los recursos de Azure y brindan visibilidad sobre su estado y rendimiento. Cada tipo de recurso crea un [conjunto distinto de métricas](../monitoring-and-diagnostics/monitoring-supported-metrics.md) sin ninguna configuración necesaria. 

Las **métricas de aplicación** se crean mediante Application Insights para sus aplicaciones supervisadas y le ayudan a detectar problemas de rendimiento y a realizar un seguimiento de las tendencias de uso de la aplicación. Incluye valores como _Tiempo de respuesta del servidor_ y _Excepciones del explorador_.

Las **métricas personalizadas** son métricas que se definen además de las métricas estándar que están disponibles automáticamente. Las métricas personalizadas se deben crear en un único recurso en la misma región que el recurso. Puede crear métricas personalizadas con los métodos siguientes:
    - [Definir métricas personalizadas en la aplicación](../application-insights/app-insights-api-custom-events-metrics.md) que se supervisa mediante Application Insights. Son métricas adicionales al conjunto de métricas de aplicación estándar.
    - Publicar las métricas personalizadas desde Windows Virtual Machines mediante la [extensión de diagnóstico de Windows (WAD)](../monitoring-and-diagnostics/azure-diagnostics.md).
    - Publicar las métricas personalizadas desde las máquinas virtuales Linux mediante el [agente Telegraf de InfluxData](https://www.influxdata.com/time-series-platform/telegraf/).
    - Escribir métricas personalizadas desde un servicio de Azure mediante la API de métricas personalizadas.
    
![Información general de las métricas](media/monitoring-data-collection/metrics-overview.png)

### <a name="what-can-you-do-with-metrics"></a>¿Para qué sirven las métricas?
Entre las tareas que puede realizar con las métricas se incluyen:

- Usar el [Explorador de métricas](../monitoring-and-diagnostics/monitoring-metric-charts.md) para analizar las métricas recopiladas y representarlas en un gráfico. Realizar el seguimiento del rendimiento de un recurso (como una máquina virtual, un sitio web o una aplicación lógica) mediante el anclaje de gráficos a un [panel de Azure](../azure-portal/azure-portal-dashboards.md).
- Configurar una [regla de alerta de métrica](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md) que envíe una notificación o realice una [acción automatizada](../monitoring-and-diagnostics/monitoring-action-groups.md) cuando la métrica cruce un umbral.
- Usar [Escalado automático](../monitoring-and-diagnostics/monitoring-overview-autoscale.md) para aumentar o disminuir los recursos según una métrica que cruce un umbral.
- Enrutar las métricas a Log Analytics para analizar los datos de métricas junto con los datos de registro y para almacenar los valores de métricas durante más de 93 días. 
- Hacer streaming de métricas a un [centro de eventos](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md) para enrutarlas a [Azure Stream Analytics](../stream-analytics/stream-analytics-introduction.md) o a sistemas externos.
- [Archivar](../monitoring-and-diagnostics/monitor-tutorial-archive-monitoring-data.md) el historial de rendimiento o estado del recurso para fines de cumplimiento, auditoría o creación de informes sin conexión.
- Acceder a los valores de métricas desde una línea de comandos o una aplicación personalizada mediante [cmdlets de PowerShell](https://docs.microsoft.com/powershell/module/azurerm.insights/?view=azurermps-6.7.0) o la [API de REST](../monitoring-and-diagnostics/monitoring-rest-api-walkthrough.md).



### <a name="viewing-metrics"></a>Visualización de métricas
Las métricas en Azure se recopilan en el almacén de métricas de Azure Monitor. Se trata de una base de datos de serie temporal optimizada para la recuperación rápida y almacena los valores de métricas durante 93 días. Copie las métricas en Log Analytics para obtener tendencias y análisis a largo plazo.

Los datos métricos se usan de distintas formas, como se ha descrito anteriormente. Use el [Explorador de métricas](../monitoring-and-diagnostics/monitoring-metric-charts.md) para analizar los datos en el almacén de métricas directamente y representar los valores de varias métricas con el tiempo. Puede ver los gráficos de forma interactiva o anclarlos a un panel para verlos con otras visualizaciones. También puede recuperar las métricas con la [API REST de supervisión de Azure](../monitoring-and-diagnostics/monitoring-rest-api-walkthrough.md).

![Explorador de métricas](media/monitoring-data-collection/metrics-explorer.png)





## <a name="logs"></a>Registros
Los registros contienen distintos tipos de datos organizados en registros con diferentes conjuntos de propiedades para cada tipo. Los registros pueden contener valores numéricos, como métricas, pero normalmente contienen datos de texto con descripciones detalladas. Se diferencian aún más de las métricas en que difieren en su estructura y a menudo no se recopilan a intervalos regulares.

Un tipo común de entrada de registro es un evento. Los eventos se recopilan de forma esporádica. Una aplicación o servicio los crea, y suelen incluir suficiente información para proporcionar un contexto completo por sí solos. Por ejemplo, un evento puede indicar que se creó o modificó un recurso determinado, se inició un nuevo host en respuesta a un aumento de tráfico o se detectó un error en una aplicación.

Los registros son especialmente útiles para combinar datos de diversos orígenes con el fin de realizar un análisis complejo y extraer las tendencias en el tiempo. Dado que el formato de los datos puede variar, las aplicaciones pueden crear registros personalizados con la estructura que necesiten. Las métricas también se replican en registros para combinarlas con otros datos de supervisión para obtener tendencias y otros análisis de datos.



### <a name="log-analytics"></a>Log Analytics
Los registros que recopila Azure Monitor se almacenan en Log Analytics, que recopila datos de telemetría y de otros tipos de distintos orígenes. Proporciona un lenguaje de consulta completo y un motor de análisis que ofrece información sobre el funcionamiento de las aplicaciones y los recursos. Otros servicios de Azure, como [Azure Security Center](../security-center/security-center-intro.md), almacenan sus datos en Log Analytics para proporcionar una plataforma de datos común en la administración de Azure.

> [!IMPORTANT]
> Los datos de Application Insights se almacenan en Log Analytics como otros datos de registro, salvo que se almacenan en una partición independiente. Es compatible con la misma funcionalidad que otros datos de Log Analytics, pero debe usar la [consola de Application Insights](/application-insights/app-insights-analytics.md) o la [API de Application Insights](https://dev.applicationinsights.io/) para acceder a estos datos. Puede usar un [consulta entre recursos](../log-analytics/log-analytics-cross-workspace-search.md) para analizar datos de la aplicación junto con otros datos de registro.


### <a name="sources-of-log-data"></a>Orígenes de datos de registro
Log Analytics puede recopilar datos de diversos orígenes, tanto en Azure como desde recursos locales. Los orígenes de datos escritos en Log Analytics incluyen los siguientes:

- [Registros de actividad](../log-analytics/log-analytics-activity.md) de recursos de Azure, que incluyen información sobre su configuración y mantenimiento, y [registros de diagnóstico](../monitoring-and-diagnostics/monitor-stream-diagnostic-logs-log-analytics.md), que proporcionan información sobre su funcionamiento.
- Agentes de las máquinas virtuales [Windows](../log-analytics/log-analytics-windows-agent.md) y [Linux](../log-analytics/log-analytics-linux-agents.md), que envían datos de telemetría desde el sistema operativo invitado y las aplicaciones a Log Analytics de acuerdo con los [orígenes de datos](../log-analytics/log-analytics-data-sources.md) que se configuran.
- Datos de aplicaciones recopilados por [Application Insights](https://docs.microsoft.com/azure/application-insights/).
- Datos que proporcionan información sobre una aplicación o un servicio determinado desde [soluciones de supervisión](../monitoring/monitoring-solutions.md) o características como las de información del contenedor, información de máquinas virtuales o información del grupo de recursos.
- Datos de seguridad recopilados por [Azure Security Center](https://docs.microsoft.com/azure/security-center/).
- [Métricas](#metrics) de recursos de Azure. Esto permite almacenar las métricas durante más de 93 días y analizarlas junto con otros datos de registro.
- Telemetría escrita en [Azure Storage](../log-analytics/log-analytics-azure-storage-iis-table.md).
- Datos personalizados de cualquier cliente de API de REST mediante el cliente [HTTP Data Collector API](../log-analytics/log-analytics-data-collector-api.md) o de un flujo de trabajo de [Azure Logic Apps](https://docs.microsoft.com/azure/logic-apps/).

![Componentes de Log Analytics](media/monitoring-data-collection/logs-overview.png)




### <a name="what-can-you-do-with-logs"></a>¿Para qué sirven los registros?
Entre las tareas que puede realizar con los registros se incluyen:

- Usar la [página de Log Analytics](../log-analytics/query-language/get-started-analytics-portal.md) en Azure Portal para escribir consultas de análisis de datos de registro.  Anclar los resultados representados como tablas o gráficos a un [panel de Azure](../azure-portal/azure-portal-dashboards.md).
- Configurar un [regla de alerta de registro](../monitoring-and-diagnostics/alert-log.md) que envíe una notificación o realice [una acción automatizada](../monitoring-and-diagnostics/monitoring-action-groups.md) cuando los resultados de la consulta coincidan con un resultado concreto.
- Crear un flujo de trabajo basado en datos de Log Analytics mediante [Logic Apps]().
- Exportar los resultados de una consulta a [Power BI](../log-analytics/log-analytics-powerbi.md) para usar diferentes visualizaciones y compartirlos con usuarios fuera de Azure.
- Acceder a los valores de métricas desde una línea de comandos o una aplicación personalizada mediante [cmdlets de PowerShell](https://docs.microsoft.com/powershell/module/azurerm.operationalinsights/?view=azurermps-6.8.1) o la [API de REST](https://dev.loganalytics.io/).

### <a name="viewing-log-data"></a>Visualización de datos de registro
Todos los datos de Log Analytics se recuperan mediante una [consulta de registro](../log-analytics/log-analytics-queries.md), que especifica un conjunto de datos determinado. Las consultas se escriben con el [lenguaje de consulta de Log Analytics](../log-analytics/query-language/get-started-queries.md), que es un lenguaje de consulta enriquecido para recuperar, consolidar y analizar rápidamente los datos recopilados. Use la [página de Log Analytics](../log-analytics/log-analytics-log-search-portals.md) de Azure Portal para analizar los datos en el almacén de métricas directamente y representar los valores de varias métricas con el tiempo. Puede ver los gráficos de forma interactiva o anclarlos a un panel para verlos con otras visualizaciones. También puede recuperar las métricas con la [API REST de supervisión de Azure](../monitoring-and-diagnostics/monitoring-rest-api-walkthrough.md).

![Registros](media/monitoring-data-collection/logs.png)

## <a name="convert-monitoring-data"></a>Conversión de datos de supervisión

### <a name="metrics-to-logs"></a>Métricas a registros
Puede copiar las métricas en Log Analytics para realizar un análisis complejo con otros tipos de datos mediante su lenguaje de consulta enriquecido. También puede conservar datos de registros durante períodos de tiempo más largos que las métricas, lo que permite obtener las tendencias en el tiempo. Cuando se almacenan las métricas o cualquier otro dato de rendimiento en Log Analytics, los datos actúan como registros. Use las métricas para la compatibilidad del análisis en tiempo casi real y las alertas, y use los registros para las tendencias y el análisis con otros datos.

Puede obtener instrucciones para recopilar métricas de recursos de Azure en [Recopilación de registros y métricas de Azure para servicios de Log Analytics](../log-analytics/log-analytics-azure-storage.md). Obtenga instrucciones para recopilar métricas de los recursos de PaaS de Azure en [Configuración de la recopilación de métricas de recursos de PaaS de Azure con Log Analytics](../log-analytics/log-analytics-collect-azurepass-posh.md).

### <a name="logs-to-metrics"></a>Registros a métricas
Tal y como se ha descrito anteriormente, las métricas tienen una capacidad de respuesta mayor que los registros, lo que permite crear alertas con una latencia y un costo menores. Log Analytics recopila una cantidad significativa de datos numéricos que son adecuados para las métricas, pero que no se almacenan en el almacén de métricas de Azure.  Un ejemplo común son los datos de rendimiento recopilados de los agentes y las soluciones de administración. Algunos de estos valores se pueden copiar en el almacén de métricas, donde están disponibles para las alertas y para realizar análisis con el Explorador de métricas.

La explicación de esta característica está disponible en [Creación de alertas de métricas para registros en Azure Monitor](../monitoring-and-diagnostics/monitoring-metric-alerts-logs.md). La lista de compatibilidad de los valores está disponible en [Métricas compatibles con Azure Monitor](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftoperationalinsightsworkspaces).

## <a name="stream-data-to-external-systems"></a>Hacer streaming de datos a sistemas externos
Además de usar las herramientas de Azure para analizar datos de supervisión, es posible que necesite reenviarlos a una herramienta externa, como un producto de administración de eventos e información de seguridad (SIEM). Este reenvío normalmente se realiza directamente desde los recursos supervisados a través de [Azure Event Hubs](https://docs.microsoft.com/azure/event-hubs/). 

Puede obtener instrucciones para los diferentes tipos de datos de supervisión en [Flujo de datos de supervisión de Azure a un centro de eventos para que lo consuma una herramienta externa](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md).

## <a name="next-steps"></a>Pasos siguientes

- Obtenga información sobre la [supervisión de datos disponible](monitoring-data-sources.md) para diferentes recursos en Azure.