---
title: Plataforma de datos de Azure Monitor | Microsoft Docs
description: Los datos de supervisión recopilados por Azure Monitor se dividen en métricas que son ligeras y capaces de admitir escenarios casi en tiempo real y registros que se usan para realizar análisis avanzados.
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.service: monitoring
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/26/2019
ms.author: bwren
ms.openlocfilehash: 8883c1e7f2874e1e2e61b8eca122f2ec294c7849
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59790933"
---
# <a name="azure-monitor-data-platform"></a>Plataforma de datos de Azure Monitor

Habilitación de observación en entornos informáticos complejos de hoy en día que ejecutan aplicaciones distribuidas que se basan en la nube y servicios locales, requiere la recopilación de datos operativos de todas las capas y todos los componentes del sistema distribuido. Debe ser capaz de realizar información detallada sobre estos datos y consolidarla en un único panel de vidrio con distintas perspectivas para admitir la ingente cantidad de partes interesadas de su organización.

[Azure Monitor](../overview.md) recopila y agrega los datos desde diversos orígenes en una plataforma de datos comunes que puede usarse para análisis, visualización y alertas. Proporciona una experiencia coherente encima de los datos de varios orígenes, que le ofrece información detallada de todos los recursos supervisados y con los datos de otros servicios que almacenan sus datos en Azure Monitor.


![Introducción a Azure Monitor](media/data-platform/overview.png)

## <a name="observability-data-in-azure-monitor"></a>Datos de observación en Azure Monitor
Las métricas, registros y seguimientos distribuidos se conocen comúnmente como los tres pilares de observación. Estos son los diferentes tipos de datos que una herramienta de supervisión debe recopilar y analizar para proporcionar suficiente observación de un sistema supervisado. Observación puede lograrse mediante la correlación de datos de varios pilares y agregación de datos en todo el conjunto de recursos que se está supervisando. Dado que Azure Monitor almacena datos de varios orígenes juntos, los datos se pueden correlacionar y analizan mediante un conjunto común de herramientas. También pone en correlación datos a través de varios inquilinos, además de hospedar datos para otros servicios y suscripciones de Azure.

Recursos de Azure generan una cantidad significativa de datos de supervisión. Azure Monitor consolida estos datos además de supervisar los datos de otros orígenes a cualquiera una plataforma de métricas o registros. Cada uno está optimizado para escenarios de supervisión determinados, y cada uno es compatible con diferentes características en Azure Monitor. Características como análisis de datos, visualizaciones o las alertas requieren a comprender las diferencias para que puedan implementar su escenario requiere de manera más eficaz y rentable. Insights en Azure Monitor como [Application Insights](../app/app-insights-overview.md) o [Azure Monitor para las máquinas virtuales](../insights/vminsights-overview.md) tienen herramientas de análisis que le permiten centrarse en el escenario de supervisión determinado sin tener que entender el diferencias entre los dos tipos de datos. 


### <a name="metrics"></a>Métricas
Las [métricas](data-platform-metrics.md) son valores numéricos que describen algún aspecto de un sistema en un momento dado. Que se recopilan a intervalos regulares y se identifican con una marca de tiempo, un nombre, un valor y una o más etiquetas de definición. Las métricas se pueden agregar con una variedad de algoritmos, en comparación con otras métricas y analizar tendencias con el tiempo. 

Las métricas en Azure Monitor se almacenan en una base de datos de serie temporal que está optimizado para analizar los datos con marca de tiempo. Esto hace que las métricas especialmente adecuados para las alertas y la rápida detección de problemas. Pueden saber cuál es el rendimiento del sistema, pero normalmente deben combinarse con los registros para identificar la causa raíz de problemas.

Las métricas están disponibles para hacer análisis interactivos en el portal de Azure con [Explorador de métricas](../app/metrics-explorer.md). Se puede agregar a un [panel Azure](../learn/tutorial-app-dashboards.md) para la visualización en combinación con otros datos y se utiliza para casi en tiempo real [alertas](alerts-metric.md).

Obtenga más información sobre métricas de Monitor de Azure, incluidos sus orígenes de datos en [métricas en Azure Monitor](data-platform-metrics.md).

### <a name="logs"></a>Registros
[Registros](data-platform-logs.md) son eventos que se produjeron dentro del sistema. Puede contener diferentes tipos de datos y como se puede estructurar o formato de texto con una marca de tiempo libre. Se pueden crear esporádicamente como eventos en el entorno de generarán las entradas del registro y un sistema con mucha carga normalmente hay que generar más volumen de registro.

Registros en Azure Monitor se almacenan en un área de trabajo de Log Analytics se basa en [Explorador de Azure Data](/azure/data-explorer/) que proporciona un motor de análisis eficaces y [lenguaje de consulta completo](/azure/kusto/query/). Los registros normalmente proporcionan información suficiente para proporcionar un contexto completo sobre el problema que se ha identificado y sirven para identificar el caso de raíz de problemas.

> [!NOTE]
> Es importante distinguir entre registros de Azure Monitor y orígenes de datos de registro en Azure. Por ejemplo, se escriben los eventos de nivel de suscripción de Azure en un [registro de actividad](activity-logs-overview.md) que puede ver en el menú de Azure Monitor. La mayoría de los recursos escriben información operativa en un [registro de diagnóstico](diagnostic-logs-overview.md) que pueda reenviar a ubicaciones diferentes. Registros de Azure Monitor es una plataforma de datos de registro que recopila los registros de actividad y los registros de diagnóstico, junto con otros datos de supervisión para proporcionar un análisis profundo a través de todo el conjunto de recursos.


 Puede trabajar con [registrar consultas](../log-query/log-query-overview.md) interactivamente con [Log Analytics](../log-query/portals.md) en Azure portal o agregar los resultados a un [panel Azure](../learn/tutorial-app-dashboards.md) para la visualización en combinación con otros datos. También puede crear [alertas de registro](alerts-log.md) que desencadenará una alerta basada en los resultados de una consulta de programación.

Obtenga más información sobre los registros de Azure Monitor incluyendo sus orígenes de datos en [inicia sesión en Azure Monitor](data-platform-logs.md).

### <a name="distributed-traces"></a>Seguimientos distribuidos
Los seguimientos son la serie de eventos relacionados que siguen una solicitud de usuario a través de un sistema distribuido. Que pueden utilizarse para determinar el comportamiento del código de aplicación y el rendimiento de transacciones diferentes. Aunque a menudo se crearán los registros por componentes individuales de un sistema distribuido, una traza mide la operación y el rendimiento de la aplicación en todo el conjunto de componentes.

Seguimiento distribuido en Azure Monitor se habilita con la [SDK de Application Insights](../app/distributed-tracing.md), y se almacenan los datos de seguimiento con otros datos de registro de aplicación recopilados por Application Insights. Esto pone a disposición las mismas herramientas de análisis como otros datos de registro incluidos alertas, paneles y consultas de registros.

Obtenga más información sobre seguimiento en distribuido [What ' s seguimiento distribuido?](../app/distributed-tracing.md).


## <a name="compare-azure-monitor-metrics-and-logs"></a>Comparar los registros y métricas de Azure Monitor

En la tabla siguiente compara las métricas y registros en Azure Monitor.

| Atributo  | Métricas | Registros |
|:---|:---|:---|
| Ventajas | Ligero y ser capaz de escenarios casi en tiempo real, como las alertas. Ideal para la detección rápida de problemas. | Analizar con enriquecido lenguaje de consulta. Ideal para un análisis profundo e identificar la causa raíz. |
| Datos | Sólo los valores numéricos | Datos numéricos o de texto |
| sección Estructura | Conjunto estándar de propiedades, incluido el tiempo de muestra, recursos que se están supervisando, un valor numérico. Algunas métricas incluyen varias dimensiones para una definición ulterior. | Conjunto único de propiedades según el tipo de registro. |
| Colección | Se recopilan a intervalos regulares. | Puede ser recopilados esporádicamente como desencadenan de eventos que se cree un registro. |
| Ver en Azure portal | Explorador de métricas | Log Analytics |
| Incluyen orígenes de datos | Se recopilan métricas de la plataforma de recursos de Azure.<br>Aplicaciones supervisadas por Application Insights.<br>Personalizado definido por la aplicación o API. | Aplicación y los registros de diagnóstico.<br>Soluciones de supervisión.<br>Los agentes y extensiones de máquina virtual.<br>Solicitudes de aplicación y excepciones.<br>Azure Security Center.<br>API del recopilador de datos. |

## <a name="collect-monitoring-data"></a>Recopilar datos de supervisión
Diferentes [orígenes de datos de Azure Monitor](data-sources.md) escribirá en un área de trabajo de Log Analytics (registros), la base de datos de Azure Monitor metrics (métricas) o ambos. Algunos orígenes escribirá directamente a estos almacenes de datos, mientras que otros pueden escribir en otra ubicación como almacenamiento de Azure y que requieren una configuración para rellenar los registros o métricas. 

Consulte [métricas en Azure Monitor](data-platform-metrics.md) y [inicia sesión en Azure Monitor](data-platform-logs.md) para obtener una lista de orígenes de datos diferentes que rellenar cada tipo.


## <a name="stream-data-to-external-systems"></a>Hacer streaming de datos a sistemas externos
Además de usar las herramientas de Azure para analizar datos de supervisión, es posible que necesite reenviarlos a una herramienta externa, como un producto de administración de eventos e información de seguridad (SIEM). Este reenvío normalmente se realiza directamente desde los recursos supervisados a través de [Azure Event Hubs](/azure/event-hubs/). Algunos orígenes pueden configurarse para enviar datos directamente a un centro de eventos, aunque puede usar otro proceso como una aplicación lógica para recuperar los datos necesarios. Consulte [datos de supervisión a un centro de eventos para su uso por una herramienta externa de Azure de Stream](stream-monitoring-data-event-hubs.md) para obtener más información.



## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre [métricas en Azure Monitor](data-platform-metrics.md).
- Obtenga más información sobre [inicia sesión en Azure Monitor](data-platform-logs.md).
- Obtenga información sobre la [supervisión de datos disponible](data-sources.md) para diferentes recursos en Azure.
