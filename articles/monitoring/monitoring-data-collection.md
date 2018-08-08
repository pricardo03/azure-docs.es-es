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
ms.openlocfilehash: 35580d71aa2592fa94f42cfdbad3c192acc303c5
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/31/2018
ms.locfileid: "39363992"
---
# <a name="collect-monitoring-data-in-azure"></a>Recopilación de datos de supervisión en Azure
En este artículo se proporciona información general de los datos de supervisión que se recopilan de la aplicación y los servicios en Azure. También describe las herramientas que puede usar para analizar los datos. 

## <a name="types-of-monitoring-data"></a>Tipos de datos de supervisión
Todos los datos de supervisión pertenecen a uno de los dos tipos fundamentales, métricas o registros. Cada tipo tiene características diferentes y es idóneo para escenarios concretos.

### <a name="metrics"></a>Métricas
Las métricas son valores numéricos que describen algunos aspectos de un sistema en un momento dado. Entre ellos se incluyen los siguientes:

* Datos distintos, incluido el propio valor.
* Hora en que se recopiló el valor.
* Tipo de medida que el valor representa.
* El recurso con el que el valor está asociado. 

Las métricas se recopilan a intervalos regulares independientemente de si cambia el valor o no. Por ejemplo, puede recopilar la utilización del procesador de una máquina virtual cada minuto o el número de usuarios que han iniciado sesión en su aplicación cada 10 minutos.

Las métricas son ligeras y capaces de admitir escenarios en tiempo real casi. Son útiles para las alertas, dado que las métricas se pueden muestrear con frecuencia y se puede activar una alerta con rapidez con una lógica relativamente sencilla. Por ejemplo, podría desencadenar una alerta cuando una métrica supere un valor de umbral. O bien, puede activar una alerta cuando la diferencia entre dos métricas alcanza un valor determinado.

Normalmente, las métricas individuales ofrecen poca información por sí solas. Proporcionan un único valor sin ningún contexto que no sea la comparación con un simple umbral. Sin embargo, son valiosas cuando se combinan con otras métricas para identificar patrones y tendencias, o cuando se combinan con registros que proporcionan contexto relacionado con valores concretos. 

Por ejemplo, un número determinado de usuarios de la aplicación en un momento dado pueden ofrecerle poca información sobre el estado de la aplicación. Pero un descenso repentino de usuarios indicado por varios valores de la misma métrica podría indicar un problema. Demasiadas excepciones iniciadas por la aplicación e indicadas por una métrica independiente pueden identificar un problema de la aplicación que provoque el descenso. Los eventos que la aplicación crea para identificar errores en sus componentes pueden ayudarle a identificar la causa raíz.

Las alertas basadas en registros no tienen la misma capacidad de respuesta que las alertas basadas en métricas, pero pueden incluir una lógica más compleja. Puede crear una alerta basada en los resultados de cualquier consulta que realice un análisis complejo de datos de varios orígenes.

### <a name="logs"></a>Registros
Los registros contienen distintos tipos de datos organizados en registros con diferentes conjuntos de propiedades para cada tipo. Los registros pueden contener valores numéricos, como métricas, pero normalmente contienen datos de texto con descripciones detalladas. Se diferencian aún más de las métricas en que difieren en su estructura y a menudo no se recopilan a intervalos regulares.

Un tipo común de entrada de registro es un evento. Los eventos se recopilan de forma esporádica. Una aplicación o servicio los crea, y suelen incluir suficiente información para proporcionar un contexto completo por sí solos. Por ejemplo, un evento puede indicar que se creó o modificó un recurso determinado, se inició un nuevo host en respuesta a un aumento de tráfico o se detectó un error en una aplicación.

Los registros son especialmente útiles para combinar datos de diversos orígenes con el fin de realizar un análisis complejo y extraer las tendencias en el tiempo. Dado que el formato de los datos puede variar, las aplicaciones pueden crear registros personalizados con la estructura que necesiten. Incluso se pueden replicar métricas en registros para combinarlas con otros datos de supervisión para tendencias y otros análisis de datos.


## <a name="monitoring-tools-in-azure"></a>Herramientas de supervisión en Azure
Los datos de supervisión en Azure se recopilan y analizan mediante los siguientes orígenes.

### <a name="azure-monitor"></a>Azure Monitor
Las métricas de recursos y aplicaciones de Azure se recopilan en Azure Monitor. Los datos de las métricas se integran en las páginas en Azure Portal para los recursos de Azure. Para las máquinas virtuales, aparecen grafos de dichas métricas como el uso de CPU y red para la máquina seleccionada. 

También se pueden analizar datos con el [Explorador de métricas](../monitoring-and-diagnostics/monitoring-metric-charts.md), que representan gráficamente los valores de varias métricas a lo largo del tiempo. Puede ver los gráficos de forma interactiva o anclarlos a un panel para verlos con otras visualizaciones. También puede recuperar las métricas con la [API REST de supervisión de Azure](../monitoring-and-diagnostics/monitoring-rest-api-walkthrough.md).

Puede obtener más información sobre los datos de métricas que los diferentes tipos de recursos de Azure recopilan en [Sources of monitoring data in Azure](monitoring-data-sources.md) (Orígenes de datos de supervisión en Azure). 

![Explorador de métricas](media/monitoring-data-collection/metrics-explorer.png)


### <a name="activity-log"></a>Registro de actividad 
El [registro de actividad de Azure](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) almacena registros sobre la configuración y el estado de los servicios de Azure. Puede usar el explorador del registro de actividad para ver estos registros en Azure Portal, pero normalmente se [copian en Log Analytics](../log-analytics/log-analytics-activity.md) para analizarlos con otros datos de registro.

Puede usar el explorador del registro de actividad para ver el registro de actividad filtrado para que coincida con determinados criterios. La mayoría de los recursos también tiene una opción de **registro de actividad** en su menú, en Azure Portal. Muestra el explorador del registro de actividad filtrado para ese recurso. También puede recuperar registros de actividad con la [API REST de supervisión de Azure](../monitoring-and-diagnostics/monitoring-rest-api-walkthrough.md).

![Eventos del registro de actividad](media/monitoring-data-collection/activity-log-explorer.png)


### <a name="log-analytics"></a>Log Analytics
Log Analytics proporciona una plataforma de datos común para la administración en Azure. Es el servicio principal que se usa para el almacenamiento y el análisis de registros en Azure. Recopila los datos desde diversos orígenes, incluidos los agentes en las máquinas virtuales, las soluciones de administración y los recursos de Azure. Puede copiar los datos de otros orígenes, incluidas las métricas y el registro de actividad, para crear un repositorio central completo de datos de supervisión.

Log Analytics dispone de un lenguaje de consulta enriquecido para analizar los datos que recopila. Puede usar [portales de búsqueda de registros](../log-analytics/log-analytics-log-search-portals.md) para escribir y probar consultas, así como analizar sus resultados interactivamente. También puede [crear vistas](../log-analytics/log-analytics-view-designer.md) para visualizar los resultados de búsquedas de registros o pegar los resultados de una consulta directamente en un panel de Azure.  

Las soluciones de administración incluyen búsquedas de registros y vistas en Log Analytics para analizar los datos que recopilan. Otros servicios, como Azure Application Insights, almacenan datos en Log Analytics y proporcionan herramientas adicionales para el análisis.  

![Registros](media/monitoring-data-collection/logs.png)

### <a name="application-insights"></a>Application Insights
Application Insights recopila telemetría para las aplicaciones web instaladas en una variedad de plataformas. Almacena sus datos en Azure Monitor y Log Analytics. Y proporciona un amplio conjunto de herramientas para analizar y visualizar sus datos. Estas funcionalidades le permiten aprovechar un conjunto común de servicios, como las alertas, las búsquedas de registros y los paneles que se usan para otras funciones de supervisión.


![Application Insights](media/monitoring-data-collection/app-insights.png)

### <a name="service-map"></a>Mapa de servicio
Service Map proporciona una representación visual de las máquinas virtuales con sus procesos y dependencias. Almacena la mayoría de estos datos en Log Analytics para poder analizarlos con otros datos de administración. La consola de Service Map también recupera datos de Log Analytics para presentarlos en el contexto de la máquina virtual que se analiza.

![Mapa de servicio](media/monitoring-data-collection/service-map.png)


## <a name="transferring-monitoring-data"></a>Transferir datos de supervisión

### <a name="metrics-to-logs"></a>Métricas a registros
Puede replicar las métricas en Log Analytics para realizar un análisis complejo con otros tipos de datos mediante su lenguaje de consulta enriquecido. También puede conservar datos de registros durante períodos de tiempo más largos que las métricas, lo que permite obtener las tendencias en el tiempo. Cuando se almacenan las métricas o cualquier otro dato de rendimiento en Log Analytics, los datos actúan como registros. Use las métricas para la compatibilidad del análisis en tiempo casi real y las alertas, y use los registros para las tendencias y el análisis con otros datos.

Puede obtener instrucciones para recopilar métricas de recursos de Azure en [Recopilación de registros y métricas de Azure para servicios de Log Analytics](../log-analytics/log-analytics-azure-storage.md). Obtenga instrucciones para recopilar métricas de los recursos de PaaS de Azure en [Configuración de la recopilación de métricas de recursos de PaaS de Azure con Log Analytics](../log-analytics/log-analytics-collect-azurepass-posh.md).

### <a name="logs-to-metrics"></a>Registros a métricas
Tal y como se describió anteriormente, las métricas tienen una capacidad de respuesta mayor que los registros, lo que permite crear alertas con una latencia y un costo menores. Log Analytics recopila una cantidad significativa de datos numéricos que son adecuados para las métricas, pero que no se almacenan en Azure Monitor. 

Un ejemplo común son los datos de rendimiento recopilados de los agentes y las soluciones de administración. Algunos de estos valores se pueden copiar en Azure Monitor, donde están disponibles para las alertas y para realizar análisis con el Explorador de métricas.

La explicación de esta característica está disponible en [Faster Metric Alerts for Logs now in limited public preview](https://azure.microsoft.com/blog/faster-metric-alerts-for-logs-now-in-limited-public-preview/) (Alertas de métricas más rápidas para los registros en versión preliminar y público limitado). La lista de compatibilidades de valores está disponible en [Métricas compatibles con alertas más recientes](../monitoring-and-diagnostics/monitoring-near-real-time-metric-alerts.md).

### <a name="event-hubs"></a>Event Hubs
Además de usar las herramientas de Azure para analizar datos de supervisión, se pueden reenviar a una herramienta externa, como un producto de administración de eventos e información de seguridad (SIEM). Este reenvío normalmente se realiza mediante [Azure Event Hubs](https://docs.microsoft.com/azure/event-hubs/). 

Puede obtener instrucciones para los diferentes tipos de datos de supervisión en [Flujo de datos de supervisión de Azure a un centro de eventos para que lo consuma una herramienta externa](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md).

## <a name="next-steps"></a>Pasos siguientes

- Obtenga información sobre la [supervisión de datos disponible](monitoring-data-sources.md) para diferentes recursos en Azure. 