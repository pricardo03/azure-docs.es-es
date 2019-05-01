---
title: Capacidad en el Explorador de datos de Azure de aprendizaje automático
description: Usar machine learning de agrupación en clústeres para el análisis de causa raíz en el Explorador de datos de Azure.
author: orspod
ms.author: orspodek
ms.reviewer: jasonh
ms.service: data-explorer
ms.topic: conceptual
ms.date: 04/29/2019
ms.openlocfilehash: 2358cb2ea411a0077f34798183da30bd32ae067b
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2019
ms.locfileid: "64925124"
---
# <a name="machine-learning-capability-in-azure-data-explorer"></a>Capacidad en el Explorador de datos de Azure de aprendizaje automático

El Explorador de datos de Azure, una plataforma de análisis de datos de gran tamaño, se usa para supervisar el estado del servicio, QoS o dispositivos que no funciona correctamente para comportamientos anómalos mediante integrado [detección de anomalías y previsión](/azure/data-explorer/anomaly-detection) funciones. Una vez que se detecta un patrón anómalo, se realiza un análisis de causa raíz (RCA) para mitigar o resolver las anomalías.

El proceso de diagnóstico es complejas y largas y realizado por expertos de dominio. El proceso incluye la obtención y combinar datos adicionales de diferentes orígenes del mismo período de tiempo para buscar cambios en la distribución de valores en varias dimensiones, gráficos variables adicionales, y otras técnicas basado en conocimiento del dominio y intuición. Puesto que estos escenarios de diagnóstico son comunes en el Explorador de datos de Azure, están disponibles para facilitar la fase de diagnóstico y acortar el tiempo que dure el RCA complementos de machine learning.

El Explorador de datos de Azure tiene tres complementos de Machine Learning: [ `autocluster` ](/azure/kusto/query/autoclusterplugin), [ `basket` ](/azure/kusto/query/basketplugin), y [ `diffpatterns` ](/azure/kusto/query/diffpatternsplugin). Todos los complementos de implementan algoritmos de agrupación en clústeres. El `autocluster` y `basket` complementos un único conjunto de registros de clúster y el `diffpatterns` complemento de clústeres de las diferencias entre dos conjuntos de registros.

## <a name="clustering-a-single-record-set"></a>Agrupación en clústeres de un conjunto de registros único

Un escenario común incluye un conjunto de datos seleccionado por un criterio específico, como el período de tiempo que exhibe el comportamiento anómalo, lecturas de dispositivos de temperatura alta, los comandos de larga duración y superior a los usuarios de gasto. Nos gustaría una manera sencilla y rápida para encontrar patrones comunes (segmentos) en los datos. Los patrones son un subconjunto del conjunto de datos cuyos registros comparten los mismos valores en varias dimensiones (columnas de categorías). La siguiente consulta genera y muestra una serie temporal de las excepciones de servicio durante una semana en las ubicaciones de diez minutos:

```kusto
let min_t = toscalar(demo_clustering1 | summarize min(PreciseTimeStamp));  
let max_t = toscalar(demo_clustering1 | summarize max(PreciseTimeStamp));  
demo_clustering1
| make-series num=count() on PreciseTimeStamp from min_t to max_t step 10m
| render timechart with(title="Service exceptions over a week, 10 minutes resolution")
```

![Gráfico de tiempo de las excepciones de servicio](media/machine-learning-clustering/service-exceptions-timechart.png)

El número de excepción de servicio se correlaciona con total tráfico del servicio. Puede ver claramente el patrón de diario para días laborables de lunes al viernes, con un aumento en el servicio de excepción cuenta el mediodía y coloca en recuentos durante la noche. Recuentos de plano bajos son visibles durante el fin de semana. Los picos de la excepción se pueden detectar mediante [tiempo de detección de anomalías de serie](/azure/data-explorer/anomaly-detection?#time-series-anomaly-detection) en el Explorador de datos de Azure.

El segundo pico en los datos se produce por la tarde el martes. La siguiente consulta se utiliza para efectuar un diagnóstico exhaustivo este pico. Utilice la consulta para volver a dibujar el gráfico en torno a especial en una resolución mayor (ocho horas en intervalos de un minuto) para comprobar si se trata de un pico sharp y ver sus bordes.

```kusto
let min_t=datetime(2016-08-23 11:00);
demo_clustering1
| make-series num=count() on PreciseTimeStamp from min_t to min_t+8h step 1m
| render timechart with(title="Zoom on the 2nd spike, 1 minute resolution")
```

![Centrarse en el gráfico de tiempo pico](media/machine-learning-clustering/focus-spike-timechart.png)

Se puede ver un pico de dos minutos estrecho entre 15:00:02 15. En la siguiente consulta, contar las excepciones en esta ventana de dos minutos:

```kusto
let min_peak_t=datetime(2016-08-23 15:00);
let max_peak_t=datetime(2016-08-23 15:02);
demo_clustering1
| where PreciseTimeStamp between(min_peak_t..max_peak_t)
| count
```

|Número |
|---------|
|972    |

En la siguiente consulta de ejemplo 20 excepciones fuera 972:

```kusto
let min_peak_t=datetime(2016-08-23 15:00);
let max_peak_t=datetime(2016-08-23 15:02);
demo_clustering1
| where PreciseTimeStamp between(min_peak_t..max_peak_t)
| take 20
```

| PreciseTimeStamp            | Region | ScaleUnit | DeploymentId                     | Punto de seguimiento | ServiceHost                          |
|-----------------------------|--------|-----------|----------------------------------|------------|--------------------------------------|
| 2016-08-23 15:00:08.7302460 | scus   | su5       | 9dbd1b161d5b4779a73cf19a7836ebd6 | 100005     | 00000000-0000-0000-0000-000000000000 |
| 2016-08-23 15:00:09.9496584 | scus   | su5       | 9dbd1b161d5b4779a73cf19a7836ebd6 | 10007006   | 8d257da1-7a1c-44f5-9acd-f9e02ff507fd |
| 2016-08-23 15:00:10.5911748 | scus   | su5       | 9dbd1b161d5b4779a73cf19a7836ebd6 | 100005     | 00000000-0000-0000-0000-000000000000 |
| 2016-08-23 15:00:12.2957912 | scus   | su5       | 9dbd1b161d5b4779a73cf19a7836ebd6 | 10007007   | f855fcef-ebfe-405d-aaf8-9c5e2e43d862 |
| 2016-08-23 15:00:18.5955357 | scus   | su5       | 9dbd1b161d5b4779a73cf19a7836ebd6 | 10007006   | 9d390e07-417d-42eb-bebd-793965189a28 |
| 2016-08-23 15:00:20.7444854 | scus   | su5       | 9dbd1b161d5b4779a73cf19a7836ebd6 | 10007006   | 6e54c1c8-42d3-4e4e-8b79-9bb076ca71f1 |
| 2016-08-23 15:00:23.8694999 | eus2   | su2       | 89e2f62a73bb4efd8f545aeae40d7e51 | 36109      | 19422243-19b9-4d85-9ca6-bc961861d287 |
| 2016-08-23 15:00:26.4271786 | ncus   | su1       | e24ef436e02b4823ac5d5b1465a9401e | 36109      | 3271bae4-1c5b-4f73-98ef-cc117e9be914 |
| 2016-08-23 15:00:27.8958124 | scus   | su3       | 90d3d2fc7ecc430c9621ece335651a01 | 904498     | 8cf38575-fca9-48ca-bd7c-21196f6d6765 |
| 2016-08-23 15:00:32.9884969 | scus   | su3       | 90d3d2fc7ecc430c9621ece335651a01 | 10007007   | d5c7c825-9d46-4ab7-a0c1-8e2ac1d83ddb |
| 2016-08-23 15:00:34.5061623 | scus   | su5       | 9dbd1b161d5b4779a73cf19a7836ebd6 | 1002110    | 55a71811-5ec4-497a-a058-140fb0d611ad |
| 2016-08-23 15:00:37.4490273 | scus   | su3       | 90d3d2fc7ecc430c9621ece335651a01 | 10007006   | f2ee8254-173c-477d-a1de-4902150ea50d |
| 2016-08-23 15:00:41.2431223 | scus   | su3       | 90d3d2fc7ecc430c9621ece335651a01 | 103200     | 8cf38575-fca9-48ca-bd7c-21196f6d6765 |
| 2016-08-23 15:00:47.2983975 | ncus   | su1       | e24ef436e02b4823ac5d5b1465a9401e | 423690590  | 00000000-0000-0000-0000-000000000000 |
| 2016-08-23 15:00:50.5932834 | scus   | su5       | 9dbd1b161d5b4779a73cf19a7836ebd6 | 10007006   | 2a41b552-aa19-4987-8cdd-410a3af016ac |
| 2016-08-23 15:00:50.8259021 | scus   | su5       | 9dbd1b161d5b4779a73cf19a7836ebd6 | 1002110    | 0d56b8e3-470d-4213-91da-97405f8d005e |
| 2016-08-23 15:00:53.2490731 | scus   | su5       | 9dbd1b161d5b4779a73cf19a7836ebd6 | 36109      | 55a71811-5ec4-497a-a058-140fb0d611ad |
| 2016-08-23 15:00:57.0000946 | eus2   | su2       | 89e2f62a73bb4efd8f545aeae40d7e51 | 64038      | cb55739e-4afe-46a3-970f-1b49d8ee7564 |
| 2016-08-23 15:00:58.2222707 | scus   | su5       | 9dbd1b161d5b4779a73cf19a7836ebd6 | 10007007   | 8215dcf6-2de0-42bd-9c90-181c70486c9c |
| 2016-08-23 15:00:59.9382620 | scus   | su3       | 90d3d2fc7ecc430c9621ece335651a01 | 10007006   | 451e3c4c-0808-4566-a64d-84d85cf30978 |

### <a name="use-autocluster-for-single-record-set-clustering"></a>Use `autocluster()` para el conjunto de agrupación en clústeres de registros único

Aunque hay menos de mil excepciones, es aún más difícil de encontrar segmentos comunes, como hay varios valores en cada columna. Puede usar [ `autocluster()` ](/azure/kusto/query/autoclusterplugin) complemento para extraer una pequeña lista de segmentos comunes al instante y buscar las interesantes de clústeres en dos minutos de especial tal como se muestra en la siguiente consulta:

```kusto
let min_peak_t=datetime(2016-08-23 15:00);
let max_peak_t=datetime(2016-08-23 15:02);
demo_clustering1
| where PreciseTimeStamp between(min_peak_t..max_peak_t)
| evaluate autocluster()
```

| SegmentId | Número | Percent | Region | ScaleUnit | DeploymentId | ServiceHost |
|-----------|-------|------------------|--------|-----------|----------------------------------|--------------------------------------|
| 0 | 639 | 65.7407407407407 | eau | su7 | b5d1d4df547d4a04ac15885617edba57 | e7f60c5d-4944-42b3-922a-92e98a8e7dec |
| 1 | 94 | 9.67078189300411 | scus | su5 | 9dbd1b161d5b4779a73cf19a7836ebd6 |  |
| 2 | 82 | 8.43621399176955 | ncus | su1 | e24ef436e02b4823ac5d5b1465a9401e |  |
| 3 | 68 | 6.99588477366255 | scus | su3 | 90d3d2fc7ecc430c9621ece335651a01 |  |
| 4 | 55 | 5.65843621399177 | weu | su4 | be1d6d7ac9574cbc9a22cb8ee20f16fc |  |

Puede ver en los resultados anteriores que el segmento predominante contiene 65.74% de los registros de excepción total y recursos compartidos de cuatro dimensiones. El siguiente segmento es mucho menos frecuente, contiene solo 9.67% de los registros y recursos compartidos de tres dimensiones. Los demás segmentos son incluso menos comunes. 

Autocluster utiliza un algoritmo propietario para varias dimensiones de minería de datos y extraer segmentos interesantes. "Interesantes" significa que cada segmento tiene una gran cobertura del conjunto de registros y el conjunto de características. Los segmentos se también han desviado, lo que significa que cada uno de ellos es significativamente diferente de los demás. Uno o varios de estos segmentos pueden ser relevante para el proceso RCA. Para minimizar la evaluación y revisión de segmento, autocluster extrae sólo una lista de segmentos pequeños.

### <a name="use-basket-for-single-record-set-clustering"></a>Use `basket()` para el conjunto de agrupación en clústeres de registros único

También puede usar el [ `basket()` ](/azure/kusto/query/basketplugin) complemento tal como se muestra en la siguiente consulta:

```kusto
let min_peak_t=datetime(2016-08-23 15:00);
let max_peak_t=datetime(2016-08-23 15:02);
demo_clustering1
| where PreciseTimeStamp between(min_peak_t..max_peak_t)
| evaluate basket()
```

| SegmentId | Número | Percent | Region | ScaleUnit | DeploymentId | Punto de seguimiento | ServiceHost |
|-----------|-------|------------------|--------|-----------|----------------------------------|------------|--------------------------------------|
| 0 | 639 | 65.7407407407407 | eau | su7 | b5d1d4df547d4a04ac15885617edba57 |  | e7f60c5d-4944-42b3-922a-92e98a8e7dec |
| 1 | 642 | 66.0493827160494 | eau | su7 | b5d1d4df547d4a04ac15885617edba57 |  |  |
| 2 | 324 | 33.3333333333333 | eau | su7 | b5d1d4df547d4a04ac15885617edba57 | 0 | e7f60c5d-4944-42b3-922a-92e98a8e7dec |
| 3 | 315 | 32.4074074074074 | eau | su7 | b5d1d4df547d4a04ac15885617edba57 | 16108 | e7f60c5d-4944-42b3-922a-92e98a8e7dec |
| 4 | 328 | 33.7448559670782 |  |  |  | 0 |  |
| 5 | 94 | 9.67078189300411 | scus | su5 | 9dbd1b161d5b4779a73cf19a7836ebd6 |  |  |
| 6 | 82 | 8.43621399176955 | ncus | su1 | e24ef436e02b4823ac5d5b1465a9401e |  |  |
| 7 | 68 | 6.99588477366255 | scus | su3 | 90d3d2fc7ecc430c9621ece335651a01 |  |  |
| 8 | 167 | 17.1810699588477 | scus |  |  |  |  |
| 9 | 55 | 5.65843621399177 | weu | su4 | be1d6d7ac9574cbc9a22cb8ee20f16fc |  |  |
| 10 | 92 | 9.46502057613169 |  |  |  | 10007007 |  |
| 11 | 90 | 9.25925925925926 |  |  |  | 10007006 |  |
| 12 | 57 | 5.8641975308642 |  |  |  |  | 00000000-0000-0000-0000-000000000000 |

Cesta implementa el algoritmo Apriori para conjunto de minería de datos de elemento y extrae todos los segmentos cuya cobertura del conjunto de registros está por encima de un umbral (valor predeterminado es 5%). Puede ver que se han extraído más segmentos con las que se parecen (por ejemplo, los segmentos de 0,1 o 2,3).

Ambos complementos son eficaces y fáciles de usar, pero su limitación importante es puesto que la un único registro que establezca de forma no supervisado (con ninguna etiqueta) del clúster. Por lo tanto, está claro si los patrones extraídos caracterizan el conjunto de registros seleccionado (los registros anómalos) o el conjunto de registros global.

## <a name="clustering-the-difference-between-two-records-sets"></a>Agrupación en clústeres de la diferencia entre los conjuntos de dos registros

El [ `diffpatterns()` ](/azure/kusto/query/diffpatternsplugin) complemento supera la limitación de `autocluster` y `basket`. `Diffpatterns` toma dos conjuntos de registros y extrae los segmentos principales que son diferentes entre ellos. Un conjunto normalmente contiene el registro anómalo establecer que se está investigando (uno analizados por `autocluster` y `basket`). El otro conjunto contiene el conjunto de registros de referencia (línea de base). 

En la siguiente consulta, usamos `diffpatterns` encontrar clústeres interesantes en las dos minutos de especial, que son diferentes de clústeres dentro de la línea base. Definimos la ventana de línea de base como los ocho minutos antes de 15:00 (cuando inicia el pico). También necesitamos ampliar mediante una columna binaria (AB) que especifica si un registro específico pertenece a la línea base o al conjunto anómalo. `Diffpatterns` implementa un algoritmo de aprendizaje supervisado, donde las etiquetas de dos clases generadas por el anómalos en comparación con la marca de línea base (AB).

```kusto
let min_peak_t=datetime(2016-08-23 15:00);
let max_peak_t=datetime(2016-08-23 15:02);
let min_baseline_t=datetime(2016-08-23 14:50);
let max_baseline_t=datetime(2016-08-23 14:58); // Leave a gap between the baseline and the spike to avoid the transition zone.
let splitime=(max_baseline_t+min_peak_t)/2.0;
demo_clustering1
| where (PreciseTimeStamp between(min_baseline_t..max_baseline_t)) or
        (PreciseTimeStamp between(min_peak_t..max_peak_t))
| extend AB=iff(PreciseTimeStamp > splitime, 'Anomaly', 'Baseline')
| evaluate diffpatterns(AB, 'Anomaly', 'Baseline')
```

| SegmentId | CountA | CountB | PercentA | PercentB | PercentDiffAB | Region | ScaleUnit | DeploymentId | Punto de seguimiento |
|-----------|--------|--------|----------|----------|---------------|--------|-----------|----------------------------------|------------|
| 0 | 639 | 21 | 65.74 | 1.7 | 64.04 | eau | su7 | b5d1d4df547d4a04ac15885617edba57 |  |
| 1 | 167 | 544 | 17.18 | 44.16 | 26.97 | scus |  |  |  |
| 2 | 92 | 356 | 9.47 | 28.9 | 19.43 |  |  |  | 10007007 |
| 3 | 90 | 336 | 9,26 | 27.27 | 18.01 |  |  |  | 10007006 |
| 4 | 82 | 318 | 8.44 | 25.81 | 17.38 | ncus | su1 | e24ef436e02b4823ac5d5b1465a9401e |  |
| 5 | 55 | 252 | 5.66 | 20.45 | 14.8 | weu | su4 | be1d6d7ac9574cbc9a22cb8ee20f16fc |  |
| 6 | 57 | 204 | 5.86 | 16.56 | 10.69 |  |  |  |  |

El segmento más dominante es el mismo segmento que se extrae mediante `autocluster`, su cobertura en la ventana anómala de dos minutos también es 65.74%. Pero su cobertura en la ventana de línea de base de ocho minutos es sólo 1.7%. La diferencia es 64.04%. Esta diferencia parece estar relacionado con el aumento anómalo. Puede comprobar esta suposición dividiendo el gráfico original en los registros que pertenecen a este segmento problemático frente a los demás segmentos tal como se muestra en la siguiente consulta:

```kusto
let min_t = toscalar(demo_clustering1 | summarize min(PreciseTimeStamp));  
let max_t = toscalar(demo_clustering1 | summarize max(PreciseTimeStamp));  
demo_clustering1
| extend seg = iff(Region == "eau" and ScaleUnit == "su7" and DeploymentId == "b5d1d4df547d4a04ac15885617edba57"
and ServiceHost == "e7f60c5d-4944-42b3-922a-92e98a8e7dec", "Problem", "Normal")
| make-series num=count() on PreciseTimeStamp from min_t to max_t step 10m by seg
| render timechart
```

![Gráfico de tiempo de validación 'diffpattern' segmento](media/machine-learning-clustering/validating-diffpattern-timechart.png)

Este gráfico nos permite ver que el pico del martes por la tarde se ha debido a las excepciones de este segmento específico, detectan mediante el `diffpatterns` complemento.

## <a name="summary"></a>Resumen

Los complementos del explorador de datos de Azure Machine Learning son útiles para muchos escenarios. El `autocluster` y `basket` implementar el algoritmo de aprendizaje y son fáciles de usar. `Diffpatterns` implementa supervisado algoritmo de aprendizaje y aunque más complejos, es más eficaz en extraer de RCA los segmentos de diferenciación.

Estos complementos se utilizan interactivamente en situaciones ad hoc y automáticos cerca de los servicios de supervisión en tiempo real. En el Explorador de datos de Azure, detección de anomalías de serie de tiempo seguida un proceso de diagnóstico que está muy optimizado para cumplir los estándares de rendimiento necesario.
