---
title: Recopilación de datos de supervisión en Azure | Microsoft Docs
description: Información general de los datos de supervisión que se recopilan de la aplicación y los servicios en Azure, así como las herramientas que se usan para analizarlos.
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.service: monitoring
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/06/2018
ms.author: bwren
ms.openlocfilehash: d3ebd512f8244de74c009ac8a2936ed8e817dad9
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/12/2018
ms.locfileid: "38992580"
---
# <a name="collecting-monitoring-data-in-azure"></a>Recopilación de datos de supervisión en Azure
En este artículo se proporciona información general de los datos de supervisión que se recopilan de la aplicación y los servicios en Azure, así como de las herramientas que se usan para analizarlos. 

## <a name="types-of-monitoring-data"></a>Tipos de datos de supervisión
Todos los datos de supervisión pertenecen a uno de los dos tipos fundamentales, métricas o registros. Cada tipo tiene características diferentes y es idóneo para escenarios concretos, como se describe a continuación.

### <a name="metrics"></a>Métricas
Las métricas son valores numéricos que describen algunos aspectos de un sistema en un momento dado en el tiempo. Incluyen distintos datos, entre ellos el propio valor, el momento en que se recopiló, el tipo de medida que representa y el recurso concreto con el que está asociado. Las métricas se recopilan a intervalos regulares independientemente de si cambia el valor o no. Por ejemplo, puede recopilar la utilización del procesador de una máquina virtual cada minuto o el número de usuarios que han iniciado sesión en su aplicación cada 10 minutos.

Las métricas son ligeras y capaces de admitir escenarios en tiempo real casi. Son especialmente útiles para las alertas, dado que las métricas se pueden muestrear con frecuencia y se puede activar una alerta rápidamente con una lógica relativamente sencilla. Por ejemplo, puede activar una alerta cuando una métrica supere un valor de umbral o cuando la diferencia entre el valor de dos métricas alcance un valor determinado.

Normalmente, las métricas individuales ofrecen poca información por sí solas. Proporcionan un único valor sin ningún contexto que no sea la comparación con un simple umbral. Sin embargo, son valiosas cuando se combinan con otras métricas para identificar patrones y tendencias o cuando se combinan con registros que proporcionan contexto relacionado con valores concretos. Por ejemplo, un número determinado de usuarios de la aplicación en un momento dado pueden ofrecerle poca información sobre el estado de la aplicación. Pero un descenso repentino de usuarios indicado por varios valores de la misma métrica puede indicar un problema. Demasiadas excepciones emitidas por la aplicación e indicadas por una métrica independiente pueden identificar un problema de la aplicación que provoque el descenso. Los eventos creados por la aplicación que identifican un error en determinados componentes de la aplicación pueden ayudar a identificar la causa raíz.

Las alertas basadas en registros no tienen la misma capacidad de respuesta que las alertas basadas en métricas, pero pueden incluir una lógica más compleja. Puede crear una alerta basada en los resultados de cualquier consulta que realice un análisis complejo de datos de varios orígenes.

### <a name="logs"></a>Registros
Los registros contienen distintos tipos de datos organizados en registros con diferentes conjuntos de propiedades para cada tipo. Los registros pueden contener valores numéricos, como métricas, pero normalmente contienen datos de texto con descripciones detalladas. Se diferencian aún más de las métricas en que difieren en su estructura y a menudo no se recopilan a intervalos regulares.

Un tipo común de entrada de registro es un evento. Los eventos se recopilan de forma esporádica, cuando una aplicación o servicio los crea, y suelen incluir suficiente información para proporcionar un contexto completo por sí solos.  Por ejemplo, un evento puede indicar que se creó o modificó un recurso determinado, se inició un nuevo host en respuesta a un aumento de tráfico o se detectó un error en una aplicación.

Los registros son especialmente útiles para combinar datos de una variedad de orígenes con el fin de realizar un análisis complejo y extraer las tendencias en el tiempo. Dado que el formato de los datos puede variar, las aplicaciones pueden crear registros personalizados con la estructura que necesiten. Incluso se pueden replicar métricas en registros para combinarlas con otros datos de supervisión para tendencias y otros análisis de datos.


## <a name="monitoring-tools-in-azure"></a>Herramientas de supervisión en Azure
Los datos de supervisión en Azure se recopilan y analizan con varios orígenes que se describen en las secciones siguientes.

### <a name="azure-metrics"></a>Métricas de Azure
Las métricas de recursos y aplicaciones de Azure se recopilan en las métricas de Azure. Los datos de las métricas se integran en las páginas de Azure Portal para recursos específicos de Azure, como las máquinas virtuales, que incluyen grafos de dichas métricas, como la utilización de CPU y de red para la máquina seleccionada. También se pueden analizar con el [Explorador de métricas](../monitoring-and-diagnostics/monitoring-metric-charts.md) que puede representar gráficamente los valores de varias métricas a lo largo del tiempo.  Puede ver los gráficos de forma interactiva o anclarlos a un panel para verlos con otras visualizaciones. También puede recuperar las métricas con la [API de REST de supervisión de Azure](../monitoring-and-diagnostics/monitoring-rest-api-walkthrough.md).

Puede obtener detalles sobre los datos de métricas que se recopilan mediante los diferentes tipos de recursos de Azure en [Sources of monitoring data in Azure](monitoring-data-sources.md) (Orígenes de datos de supervisión en Azure). 

![Explorador de métricas](media/monitoring-data-collection/metrics-explorer.png)


### <a name="azure-activity-log"></a>Azure Activity Log 
El [registro de actividad de Azure](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) almacena registros sobre la configuración y el estado de los servicios de Azure. Puede usar el explorador del registro de actividad para ver estos registros en Azure Portal, pero normalmente se [copian en Log Analytics](../log-analytics/log-analytics-activity.md) para analizarlos con otros datos de registro.

Puede usar el explorador del registro de actividad para ver el registro de actividad filtrado para que coincida con determinados criterios.  La mayoría de los recursos también dispondrán de una opción de registro de actividad en el menú de Azure Portal que muestre el explorador del registro de actividad filtrado para el recurso. También puede recuperar registros de actividad con la [API de REST de supervisión de Azure](../monitoring-and-diagnostics/monitoring-rest-api-walkthrough.md).

![Eventos del registro de actividad](media/monitoring-data-collection/activity-log-explorer.png)


### <a name="log-analytics"></a>Log Analytics
Log Analytics proporciona una plataforma de datos común para la administración en Azure. Es el servicio principal usado para el almacenamiento y análisis de registros en Azure, y recopila datos de diversos orígenes, incluidos los agentes de máquinas virtuales, las soluciones de administración y los diferentes recursos de Azure. Se pueden copiar datos de otros orígenes, incluidas las métricas y el registro de actividades, en Log Analytics con el fin de crear un repositorio central completo de datos de supervisión.

Log Analytics dispone de un lenguaje de consulta enriquecido para analizar los datos que recopila.  Puede usar [portales de búsqueda de registros](../log-analytics/log-analytics-log-search-portals.md) para escribir y probar consultas, así como analizar sus resultados interactivamente. También puede [crear vistas](../log-analytics/log-analytics-view-designer.md) para visualizar los resultados de búsquedas de registros o pegar los resultados de una consulta directamente en un panel de Azure.  Las soluciones de administración incluyen búsquedas de registros y vistas en Log Analytics para analizar los datos recopilados. Otros servicios, como Application Insights, almacenan datos en Log Analytics y proporcionan herramientas adicionales para el análisis.  

![Registros](media/monitoring-data-collection/logs.png)

### <a name="application-insights"></a>Application Insights
Application Insights recopila telemetría para las aplicaciones web instaladas en una variedad de plataformas. Almacena sus datos en métricas de Azure y Log Analytics y proporciona un amplio conjunto de herramientas muy completas, además de las herramientas existentes de análisis de datos, para analizar y visualizar sus datos. Esto permite aprovechar un conjunto común de servicios, como las alertas, las búsquedas de registros y los paneles que se usan para otra supervisión.


![Detalles de la aplicación](media/monitoring-data-collection/app-insights.png)

### <a name="service-map"></a>Mapa de servicio
Service Map proporciona una representación visual de las máquinas virtuales con sus procesos y dependencias. Almacena la mayoría de estos datos en Log Analytics para poder analizarlos con otros datos de administración. La consola de Service Map también recupera datos de Log Analytics para presentarlos en el contexto de la máquina virtual que se analiza.

![Mapa de servicio](media/monitoring-data-collection/service-map.png)


## <a name="transferring-monitoring-data"></a>Transferir datos de supervisión

### <a name="metrics-to-logs"></a>Métricas a registros
También se pueden replicar las métricas en Log Analytics para realizar un análisis complejo con otros tipos de datos mediante su lenguaje de consulta enriquecido. Además puede conservar datos de registros durante períodos de tiempo más largos que las métricas, lo que permite extraer las tendencias en el tiempo. Cuando se almacenan las métricas o cualquier otro dato de rendimiento en Log Analytics, los datos actúan como registros. Use las métricas para la compatibilidad del análisis en tiempo casi real y las alertas, y use los registros para las tendencias y el análisis con otros datos.

Puede obtener instrucciones para recopilar métricas de recursos de Azure en [Recopilación de registros y métricas de Azure para servicios de Log Analytics](../log-analytics/log-analytics-azure-storage.md). Obtenga instrucciones para recopilar métricas de los recursos de PaaS de Azure en [Configuración de la recopilación de métricas de recursos de PaaS de Azure con Log Analytics](../log-analytics/log-analytics-collect-azurepass-posh.md).

### <a name="logs-to-metrics"></a>Registros a métricas
Tal y como se describió anteriormente, las métricas tienen una capacidad de respuesta mayor que los registros, lo que permite crear alertas con una latencia y un costo menores. Log Analytics recopila una cantidad significativa de datos numéricos que son adecuados para las métricas, pero que no se almacenan en las métricas de Azure. Un ejemplo común son los datos de rendimiento recopilados de los agentes y las soluciones de administración. Algunos de estos valores se pueden copiar en las métricas de Azure donde están disponibles para las alertas y para realizar análisis con el explorador de métricas.

La explicación de esta característica está disponible en [Faster Metric Alerts for Logs now in limited public preview](https://azure.microsoft.com/blog/faster-metric-alerts-for-logs-now-in-limited-public-preview/) (Alertas de métricas más rápidas para los registros en versión preliminar y público limitado). La lista de compatibilidades de valores está disponible en [Métricas compatibles con alertas más recientes](../monitoring-and-diagnostics/monitoring-near-real-time-metric-alerts.md).

### <a name="event-hub"></a>Centro de eventos
Además de usar las herramientas de Azure para analizar datos de supervisión, se pueden reenviar a una herramienta externa, como un producto de administración de eventos e información de seguridad (SIEM). Suele realizarse mediante el [centro de eventos de Azure](https://docs.microsoft.com/azure/event-hubs/). 

Puede obtener instrucciones para los diferentes tipos de datos de supervisión en [Flujo de datos de supervisión de Azure a un centro de eventos para que lo consuma una herramienta externa](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md).

## <a name="next-steps"></a>Pasos siguientes

- Obtenga información sobre la [supervisión de datos disponible](monitoring-data-sources.md) para diferentes recursos en Azure. 