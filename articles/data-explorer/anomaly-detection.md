---
title: Detección de anomalías de serie de tiempo y la previsión en el Explorador de datos de Azure
description: Aprenda a analizar datos de series temporales para la detección de anomalías y previsiones mediante el Explorador de datos de Azure.
author: orspod
ms.author: orspodek
ms.reviewer: jasonh
ms.service: data-explorer
ms.topic: conceptual
ms.date: 04/24/2019
ms.openlocfilehash: f40350129a12c7865051bcae80b74b6f9c069179
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/07/2019
ms.locfileid: "65233525"
---
# <a name="anomaly-detection-and-forecasting-in-azure-data-explorer"></a>Detección de anomalías y previsión en el Explorador de datos de Azure

El Explorador de datos de Azure realiza la recopilación continua de datos de telemetría desde dispositivos de IoT o servicios en la nube. Estos datos se analizan diversos información como la supervisión de estado del servicio, los procesos de producción física, las tendencias de uso y previsión de carga. El análisis se realiza en series temporales de las métricas seleccionadas para buscar un patrón de desviación de la métrica en relación con su patrón típico de línea de base normal. El Explorador de datos de Azure contiene compatibilidad nativa para la creación, manipulación y análisis de varias series temporales. Puede crear y analizar miles de serie de tiempo en segundos, habilitar casi en tiempo real los flujos de trabajo y soluciones de supervisión.

Este artículo detalla el Explorador de Azure Data tiempo anomalías detección y la previsión de capacidades de la serie. Las funciones de la serie de tiempo aplicable se basan en un modelo robusto descomposición conocido, donde cada serie temporal original se descompone en tendencias estacionales, y los componentes residuales. Las anomalías se detectan mediante los valores atípicos en el componente residual, mientras la previsión se realiza mediante la extrapolación de la temporada y componentes de tendencia. La implementación del explorador de datos de Azure mejora significativamente el modelo básico descomposición por detección de estacionalidad automática, el análisis de valores atípicos sólida e implementación vectorizada para procesar miles de serie de tiempo en segundos.

## <a name="prerequisites"></a>Requisitos previos

Lectura [tiempo de análisis de series en el Explorador de datos de Azure](/azure/data-explorer/time-series-analysis) para obtener información general de las capacidades de la serie de tiempo.

## <a name="time-series-decomposition-model"></a>Modelo de descomposición de serie temporal

Azure implementación nativa de explorador de datos para la detección de anomalías y predicción de serie temporal usa un modelo de descomposición conocido. Este modelo se aplica a la serie temporal de las métricas que se espera que el manifiesto periódicas y el comportamiento de tendencia, como tráfico del servicio, los latidos de componente y mediciones periódicas de IoT para predecir valores futuros de métrica y detectar las anómalas. La suposición de este proceso de regresión que es que no sea conocido previamente estacionales y el comportamiento de tendencia, el tiempo de serie se distribuye aleatoriamente. A continuación, puede prever valores futuros de métrica de la temporada y componentes de tendencia, denominados colectivamente línea base y omitir la parte residual. También puede detectar valores anómalos según el análisis de valores atípicos utilizando sólo la parte residual.
Para crear un modelo de descomposición, use la función [ `series_decompose()` ](/azure/kusto/query/series-decomposefunction). El `series_decompose()` función toma un conjunto de serie temporal y automáticamente se descompone cada serie temporal para su estacionales, tendencia, residual y componentes de la línea base. 

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

![Descomposición de la serie de tiempo](media/anomaly-detection/series-decompose-timechart.png)

* La serie temporal original se denomina **num** (en rojo). 
* El proceso se inicia mediante la detección automática de la estacionalidad mediante la función [ `series_periods_detect()` ](/azure/kusto/query/series-periods-detectfunction) y extrae el **estacionales** patrón (en púrpura).
* El patrón estacional se resta de la serie de tiempo original y se ejecuta una regresión lineal con la función [ `series_fit_line()` ](/azure/kusto/query/series-fit-linefunction) para encontrar el **tendencia** componente (en color azul claro).
* La función resta la tendencia y el resto es el **residual** componente (en verde).
* Por último, la función agrega la temporada y tendencias de los componentes para generar el **baseline** (en azul).

## <a name="time-series-anomaly-detection"></a>Detección de anomalías de serie de tiempo

La función [ `series_decompose_anomalies()` ](/azure/kusto/query/series-decompose-anomaliesfunction) busca puntos anómalos en un conjunto de serie temporal. Esta función llama a `series_decompose()` para generar el modelo de descomposición y, a continuación, se ejecuta [ `series_outliers()` ](/azure/kusto/query/series-outliersfunction) en el componente residual. `series_outliers()` calcula las puntuaciones de anomalías para cada punto del componente mediante la prueba de Tukey barrera residual. Puntuaciones de anomalías anteriormente 1.5 o por debajo de -1,5 indican un aumento de anomalías leves o rechazar respectivamente. Las puntuaciones de anomalías anteriormente 3.0 o por debajo de -3,0 indican una anomalía de segura. 

La siguiente consulta le permite detectar anomalías en el tráfico web interno del servicio:

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

![Detección de anomalías de serie de tiempo](media/anomaly-detection/series-anomaly-detection.png)

* La serie temporal original (en rojo). 
* La línea base (estacionales + tendencia) componente (en azul).
* Los puntos anómalos (en púrpura) encima de la serie temporal original. Los puntos anómalos significativamente desvían de los valores de línea de base esperada.

## <a name="time-series-forecasting"></a>Previsión de series temporales

La función [ `series_decompose_forecast()` ](/azure/kusto/query/series-decompose-forecastfunction) predice valores futuros de un conjunto de serie temporal. Esta función llama a `series_decompose()` generar el modelo y, a continuación, para cada serie temporal, la descomposición extrapola el componente de línea de base en el futuro.

La consulta siguiente permite predecir el tráfico del servicio web de la semana siguiente:

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

* Métrica original (en rojo). Valores futuros faltan y se establece en 0, de forma predeterminada.
* Extrapole el componente de línea base (en azul) para predecir valores de la próxima semana.

## <a name="scalability"></a>Escalabilidad

Sintaxis del lenguaje de consulta del explorador de datos Azure permite una sola llamada al procesar varias series temporales. Permite que su implementación optimizada único para un rendimiento rápido, lo cual es crítico para la detección de anomalías eficaz y la previsión al supervisar miles de contadores en los escenarios en tiempo real casi.

La consulta siguiente muestra el procesamiento de las tres series de tiempo al mismo tiempo:

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

![Escalabilidad de la serie de tiempo](media/anomaly-detection/series-scalability.png)

## <a name="summary"></a>Resumen

Este documento detalla las funciones de explorador de datos de Azure nativas para la detección de anomalías de serie de tiempo y la previsión. Cada serie temporal original se descompone en componentes de tendencia y residual estacionales, para detectar anomalías y previsión. Estas funcionalidades se pueden usar para cerca de los escenarios de supervisión en tiempo real, como la detección de errores, el mantenimiento predictivo y a petición y previsión de carga.

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre [capacidades de aprendizaje automático](/azure/data-explorer/machine-learning-clustering) en el Explorador de datos de Azure.
