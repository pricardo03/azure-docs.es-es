---
title: Previsión y detección de anomalías de serie temporal en Azure Data Explorer
description: Obtenga información sobre cómo analizar datos de series temporales para la previsión y detección de anomalías mediante Azure Data Explorer.
author: orspod
ms.author: orspodek
ms.reviewer: jasonh
ms.service: data-explorer
ms.topic: conceptual
ms.date: 04/24/2019
ms.openlocfilehash: f40350129a12c7865051bcae80b74b6f9c069179
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65233525"
---
# <a name="anomaly-detection-and-forecasting-in-azure-data-explorer"></a>Previsión y detección de anomalías en Azure Data Explorer

Azure Data Explorer realiza la recopilación continua de datos de telemetría desde servicios en la nube o dispositivos de IoT. Estos datos se analizan para obtener diversa información, como la supervisión del estado de los servicios, los procesos físicos de producción, las tendencias de utilización y la previsión de cargas. El análisis se realiza en series temporales de las métricas seleccionadas para buscar un patrón de desviación de la métrica relacionado con su patrón típico de línea de base normal. Azure Data Explorer contiene funcionalidad nativa para crear, manipular y analizar múltiples series temporales. Puede crear y analizar miles de series temporales en cuestión de segundos, lo que permite obtener flujos de trabajo y soluciones de supervisión casi en tiempo real.

En este artículo se detallan las capacidades de Azure Data Explorer de previsión y detección de anomalías de series temporales. Las funciones de serie temporal aplicables se basan en un modelo de descomposición conocido sólido, en el que cada serie temporal original se descompone en los componentes estacional, residual y de tendencia. Las anomalías se detectan mediante los valores atípicos en el componente residual, mientras que la previsión se realiza mediante la extrapolación de los componentes estacional y de tendencia. La implementación de Azure Data Explorer mejora significativamente el modelo de descomposición básico por detección de estacionalidad automática, análisis de valores atípicos sólido e implementación vectorizada para procesar miles de series temporales en cuestión de segundos.

## <a name="prerequisites"></a>Requisitos previos

Lea [Análisis de series temporales en Azure Data Explorer](/azure/data-explorer/time-series-analysis) para obtener una introducción a las capacidades de serie temporal.

## <a name="time-series-decomposition-model"></a>Modelo de descomposición de series temporales

La implementación nativa de Azure Data Explorer para la predicción y detección de anomalías de serie temporal utiliza un modelo de descomposición conocido. Este modelo se aplica a series temporales de métricas que se espera que manifiesten un comportamiento periódico y de tendencia, como el tráfico del servicio, latidos del componente y medidas periódicas de IoT para predecir valores de métrica futuros y detectar los valores anómalos. La suposición de este proceso de regresión es que, con la excepción del comportamiento estacional y de tendencia anteriormente conocido, la serie temporal se distribuye de forma aleatoria. Por consiguiente, puede predecir los valores de métrica futuros a partir de los componentes estacional y de tendencia, denominados colectivamente como línea de base, y omitir la parte residual. También puede detectar valores anómalos según el análisis de valores atípicos utilizando solo la parte residual.
Para crear un modelo de descomposición, use la función [`series_decompose()`](/azure/kusto/query/series-decomposefunction). La función `series_decompose()` toma un conjunto de series temporales y descompone automáticamente cada serie temporal en sus componentes estacional, de tendencia, residual y de línea de base. 

Por ejemplo, puede descomponer el tráfico de un servicio web interno utilizando la consulta siguiente:

```kusto
let min_t = datetime(2017-01-05);
let max_t = datetime(2017-02-03 22:00);
let dt = 2h;
demo_make_series2
| make-series num=avg(num) on TimeStamp from min_t to max_t step dt by sid 
| where sid == 'TS1'   //  select a single time series for a cleaner visualization
| extend (baseline, seasonal, trend, residual) = series_decompose(num, -1, 'linefit')  //  decomposition of a set of time series to seasonal, trend, residual, and baseline (seasonal+trend)
| render timechart with(title='Web app. traffic of a month, decomposition', ysplit=panels)
```

![Descomposición de series temporales](media/anomaly-detection/series-decompose-timechart.png)

* La serie temporal original se denomina **num** (en rojo). 
* El proceso empieza por la detección automática de la estacionalidad mediante la función [`series_periods_detect()`](/azure/kusto/query/series-periods-detectfunction) y extrae el patrón **estacional** (en púrpura).
* El patrón estacional se resta de la serie temporal original y se ejecuta una regresión lineal con la función [`series_fit_line()`](/azure/kusto/query/series-fit-linefunction) para encontrar el componente de **tendencia** (en azul claro).
* La función resta la tendencia y lo que queda es el componente **residual** (en verde).
* Por último, la función agrega los componentes estacional y de tendencia para generar la **línea de base** (en azul).

## <a name="time-series-anomaly-detection"></a>Detección de anomalías en una serie temporal

La función [`series_decompose_anomalies()`](/azure/kusto/query/series-decompose-anomaliesfunction) busca puntos anómalos en un conjunto de series temporales. Esta función llama a `series_decompose()` para generar el modelo de descomposición y, a continuación, ejecuta [`series_outliers()`](/azure/kusto/query/series-outliersfunction) en el componente residual. `series_outliers()` calcula las puntuaciones de anomalía para cada punto del componente residual mediante la prueba de la barrera de Tukey. Las puntuaciones de anomalía por encima de 1,5 o por debajo de -1,5 indican un aumento o una disminución leves de las anomalías, respectivamente. Las puntuaciones de anomalía por encima de 3,0 o por debajo de -3,0 indican una anomalía segura. 

La siguiente consulta le permite detectar anomalías en el tráfico de un servicio web interno:

```kusto
let min_t = datetime(2017-01-05);
let max_t = datetime(2017-02-03 22:00);
let dt = 2h;
demo_make_series2
| make-series num=avg(num) on TimeStamp from min_t to max_t step dt by sid 
| where sid == 'TS1'   //  select a single time series for a cleaner visualization
| extend (anomalies, score, baseline) = series_decompose_anomalies(num, 1.5, -1, 'linefit')
| render anomalychart with(anomalycolumns=anomalies, title='Web app. traffic of a month, anomalies') //use "| render anomalychart with anomalycolumns=anomalies" to render the anomalies as bold points on the series charts.
```

![Detección de anomalías en una serie temporal](media/anomaly-detection/series-anomaly-detection.png)

* La serie temporal original (en rojo). 
* El componente de línea de base (estacional + tendencia) (en azul).
* Los puntos anómalos (en púrpura) encima de la serie temporal original. Los puntos anómalos se desvían significativamente de los valores esperados de la línea de base.

## <a name="time-series-forecasting"></a>Previsión de series temporales

La función [`series_decompose_forecast()`](/azure/kusto/query/series-decompose-forecastfunction) predice valores futuros de un conjunto de series temporales. Esta función llama a `series_decompose()` para generar el modelo de descomposición y, a continuación, para cada serie temporal, extrapola el componente de línea de base al futuro.

La consulta siguiente le permite predecir el tráfico del servicio web de la próxima semana:

```kusto
let min_t = datetime(2017-01-05);
let max_t = datetime(2017-02-03 22:00);
let dt = 2h;
let horizon=7d;
demo_make_series2
| make-series num=avg(num) on TimeStamp from min_t to max_t+horizon step dt by sid 
| where sid == 'TS1'   //  select a single time series for a cleaner visualization
| extend forecast = series_decompose_forecast(num, toint(horizon/dt))
| render timechart with(title='Web app. traffic of a month, forecasting the next week by Time Series Decmposition')
```

![Previsión de series temporales](media/anomaly-detection/series-forecasting.png)

* Métrica original (en rojo). Faltan los valores futuros, que se establecen en 0 de manera predeterminada.
* Extrapole el componente de línea de base (en azul) para predecir los valores de la próxima semana.

## <a name="scalability"></a>Escalabilidad

La sintaxis del lenguaje de consulta de Azure Data Explorer permite que una única llamada procese varias series temporales. Su implementación optimizada única permite un rendimiento rápido, lo que es vital para una previsión y detección de anomalías eficaz si se supervisan miles de contadores en escenarios casi en tiempo real.

La consulta siguiente muestra el procesamiento simultáneo de tres series temporales:

```kusto
let min_t = datetime(2017-01-05);
let max_t = datetime(2017-02-03 22:00);
let dt = 2h;
let horizon=7d;
demo_make_series2
| make-series num=avg(num) on TimeStamp from min_t to max_t+horizon step dt by sid
| extend offset=case(sid=='TS3', 4000000, sid=='TS2', 2000000, 0)   //  add artificial offset for easy visualization of multiple time series
| extend num=series_add(num, offset)
| extend forecast = series_decompose_forecast(num, toint(horizon/dt))
| render timechart with(title='Web app. traffic of a month, forecasting the next week for 3 time series')
```

![Escalabilidad de las series temporales](media/anomaly-detection/series-scalability.png)

## <a name="summary"></a>Resumen

En este documento se detallan las funciones nativas de Azure Data Explorer para la previsión y detección de anomalías en series temporales. Cada serie temporal original se descompone en los componentes estacional, de tendencia y residual para detectar anomalías y/o realizar previsiones. Estas funcionalidades se pueden usar para escenarios de supervisión casi en tiempo real, como la detección de errores, el mantenimiento predictivo y la previsión de cargas y peticiones.

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre las [capacidades de aprendizaje automático](/azure/data-explorer/machine-learning-clustering) en Azure Data Explorer.
