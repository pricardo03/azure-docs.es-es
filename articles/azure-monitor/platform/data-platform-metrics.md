---
title: Las métricas en Azure Monitor | Microsoft Docs
description: Describe las métricas en Azure Monitor, que son ligeros capaz de admitir escenarios en tiempo real casi los datos de supervisión.
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
ms.openlocfilehash: 1027398a1a7f790adedf6c7eebed44a8db501b8a
ms.sourcegitcommit: 9f4eb5a3758f8a1a6a58c33c2806fa2986f702cb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2019
ms.locfileid: "58905042"
---
# <a name="metrics-in-azure-monitor"></a>Métricas en Azure Monitor

> [!NOTE]
> La plataforma de datos de Azure Monitor se basa en dos tipos de datos fundamentales: Métricas y registros. En este artículo se describe las métricas. Consulte [inicia sesión en Azure Monitor](data-platform-logs.md) para obtener una descripción detallada de los registros y a [platforn de datos de Azure Monitor](data-platform.md) para ver una comparación de los dos.


Las métricas en Azure Monitor son ligeras y capaz de admitir escenarios en tiempo real que sean especialmente útiles para casi rápida y alertas de detección de problemas. En este artículo se describe cómo se estructuran las métricas, lo que puede hacer con ellos e identifica diferentes orígenes de datos que almacenan datos en las métricas.

## <a name="what-are-metrics"></a>¿Qué son las métricas?
Las métricas son valores numéricos que describen algunos aspectos de un sistema en un momento dado. Las métricas se recopilan a intervalos regulares y son útiles para las alertas ya pueden ser muestreadas con frecuencia y se puede desencadenar una alerta rápidamente con lógica relativamente sencilla.

## <a name="what-can-you-do-with-azure-monitor-metrics"></a>¿Qué puede hacer con las métricas en Microsoft Azure?
En la tabla siguiente se enumera las distintas formas que puede usar datos de métricas en Azure Monitor.

|  |  |
|:---|:---|
| Análisis | Use [Explorador de métricas](metrics-charts.md) para analizar las métricas recopiladas en un gráfico y comparar las métricas de diferentes recursos. |
| Visualizar | Anclar un gráfico desde el Explorador de métricas para un [panel Azure](../learn/tutorial-app-dashboards.md).<br>Crear un [libro](../app/usage-workbooks.md) para combinar con varios conjuntos de datos en un informe interactivo. Exportar los resultados de una consulta a [Grafana](grafana-plugin.md) aprovechar sus paneles y combinar con otros orígenes de datos. |
| Alerta | Configurar un [regla de alerta de métrica](alerts-metric.md) que envía una notificación o realiza [una acción automatizada](action-groups.md) cuando el valor de métrica cruza un umbral. |
| Automatizar |  Use [escalado automático](autoscale-overview.md) para aumentar o disminuir los recursos en función del valor de una métrica cruza un umbral. |
| Exportación | [Redirigir las métricas para los registros](diagnostic-logs-stream-log-store.md) para analizar datos en las métricas en Microsoft Azure junto con los datos en registros de Azure Monitor y para almacenar los valores de métrica durante más tiempo que 93 días.<br>Stream de métricas para un [centro de eventos](stream-monitoring-data-event-hubs.md) enrutarlos a sistemas externos. |
| Recuperar | Tener acceso a los valores de métrica desde una línea de comandos mediante [cmdlets de PowerShell](https://docs.microsoft.com/powershell/module/az.applicationinsights)<br>Obtener acceso a los valores de métrica de aplicación personalizada con [API de REST](rest-api-walkthrough.md).<br>Tener acceso a los valores de métrica desde una línea de comandos mediante [CLI](/azure/monitor/metrics). |
| Archivar | [Archivar](..//learn/tutorial-archive-data.md) el historial de rendimiento o estado del recurso para fines de cumplimiento, auditoría o creación de informes sin conexión. |


## <a name="how-is-data-in-azure-monitor-metrics-structured"></a>¿Cómo son datos de métricas de Azure Monitor estructurado?
Datos recopilados por las métricas en Microsoft Azure se almacenan en una base de datos de serie temporal que está optimizado para analizar los datos con marca de tiempo. Cada conjunto de valores de métrica es una serie temporal con las siguientes propiedades:

* El tiempo que se recopiló el valor
* El valor de recurso está asociado
* Un espacio de nombres que actúa como una categoría para la métrica
* Un nombre de métrica
* El propio valor
* Algunas métricas pueden tener varias dimensiones, como se describe en [métricas multidimensionales](#multi-dimensional-metrics). Las métricas personalizadas pueden tener hasta 10 dimensiones.

Las métricas en Azure se almacenan durante 93 días. También puede [enviar métricas de la plataforma para recursos de Azure Monitor a un área de trabajo de Log Analytics](diagnostic-logs-stream-log-store.md) para detectar tendencias a largo plazo.

## <a name="multi-dimensional-metrics"></a>Métricas multidimensionales
Uno de los desafíos a los datos de métricas es que a menudo tiene limitada información para proporcionar contexto para los valores recopilados. Azure Monitor aborda este reto con métricas multidimensionales. Las dimensiones de una métrica son pares nombre-valor que transportan datos adicionales para describir el valor de la métrica. Por ejemplo, una métrica _espacio en disco disponible_ podría tener una dimensión denominada _unidad_ con valores _C:_, _D:_, lo que le permitiría ver el espacio en disco disponible en todas las unidades o individualmente para cada unidad.

En el ejemplo siguiente se muestran dos conjuntos de datos para una métrica hipotética denominada _Rendimiento de la red_. El primer conjunto de datos no tiene ninguna dimensión. El segundo conjunto de datos muestra los valores con dos dimensiones, _Dirección IP_ y _Dirección_:

### <a name="network-throughput"></a>Rendimiento de la red

| Timestamp     | Valor de métrica |
| ------------- |:-------------|
| 9/8/2017 8:14 | 1331,8 Kbps |
| 9/8/2017 8:15 | 1141,4 Kbps |
| 9/8/2017 8:16 | 1110,2 Kbps |

Esta métrica no dimensional solo puede responder una pregunta básica, como “¿cuál era el rendimiento de la red en un momento dado?”.

### <a name="network-throughput--two-dimensions-ip-and-direction"></a>Rendimiento de la red + dos dimensiones (“IP” y “Dirección”)

| Timestamp     | Dimensión “IP”   | Dimensión “Dirección” | Valor de métrica|
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

## <a name="interacting-with-azure-monitor-metrics"></a>Interactuar con las métricas en Microsoft Azure
Use [Explorador de métricas](metrics-charts.md) para analizar los datos en la base de datos de métrica y los valores de varias métricas del gráfico con el tiempo de forma interactiva. Puede anclar los gráficos a un panel para verlos con otras visualizaciones. También puede recuperar las métricas con la [API REST de supervisión de Azure](rest-api-walkthrough.md).

![Explorador de métricas](media/data-platform/metrics-explorer.png)

## <a name="sources-of-azure-monitor-metrics"></a>Orígenes de métricas de Azure Monitor
Las métricas que Azure Monitor recopila proceden de tres orígenes fundamentales. Una vez que estas métricas se recopilan en la base de datos de métrica de Azure Monitor, puede evaluarse de forma conjunta, independientemente de su origen.

Las **métricas de plataforma** se crean mediante los recursos de Azure y brindan visibilidad sobre su estado y rendimiento. Cada tipo de recurso crea un [conjunto distinto de métricas](metrics-supported.md) sin ninguna configuración necesaria. A menos que se especifique lo contrario en la definición de la métrica, se recopilan métricas de la plataforma de recursos de Azure con frecuencia un minuto. 

**Métricas de SO invitado** se recopilan desde el sistema operativo invitado de una máquina virtual. Habilitar las métricas de SO invitado para máquinas virtuales de Windows con [extensión de diagnóstico de Windows (WAD)](../platform/diagnostics-extension-overview.md) y las máquinas virtuales de Linux con [InfluxData Telegraf agente](https://www.influxdata.com/time-series-platform/telegraf/).

Las **métricas de aplicación** se crean mediante Application Insights para sus aplicaciones supervisadas y le ayudan a detectar problemas de rendimiento y a realizar un seguimiento de las tendencias de uso de la aplicación. Incluye valores como _Tiempo de respuesta del servidor_ y _Excepciones del explorador_.

**Métricas personalizadas** son las métricas que defina además de las métricas estándares que están disponibles automáticamente. También puede [define métricas personalizadas en la aplicación](../app/api-custom-events-metrics.md) que se supervisa mediante Application Insights o crear métricas personalizadas para un servicio de Azure con el [métricas personalizadas API](metrics-store-custom-rest-api.md).


## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre la [plataforma de datos de Azure Monitor](data-platform.md).
- Obtenga información sobre [registrar datos en Azure Monitor](data-platform-logs.md).
- Obtenga información sobre la [supervisión de datos disponible](data-sources.md) para diferentes recursos en Azure.
