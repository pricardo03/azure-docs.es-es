---
title: Procedimientos recomendados cuando se usa Anomaly Detector API
description: Obtenga información sobre los procedimientos recomendados al detectar anomalías con la API del Detector de anomalías.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: article
ms.date: 03/26/2019
ms.author: aahi
ms.openlocfilehash: 766d009be3cd664d928a3c12f5fea38c26bbbdde
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2019
ms.locfileid: "64692196"
---
# <a name="best-practices-for-using-the-anomaly-detector-api"></a>Procedimientos recomendados para usar la API del Detector de anomalías

La API del Detector de anomalías es un servicio de detección de anomalías sin estado. La precisión y el rendimiento de sus resultados pueden verse afectados por:

* Cómo se preparan los datos de series temporales.
* Los parámetros de la API del Detector de anomalías que se usaron.
* El número de puntos de datos de la solicitud de API. 

Use este artículo para obtener información sobre procedimientos recomendados para usar la API de obtener los mejores resultados para los datos. 

## <a name="when-to-use-batch-entire-or-latest-last-point-anomaly-detection"></a>Cuándo usar batch (completa) o versión más reciente (última) elija la detección de anomalías

Punto de conexión de detección de la API Detector de anomalías batch le permite detectar anomalías en todo el veces los datos de la serie. En este modo de detección, se crea un único modelo estadístico y se aplica a cada punto en el conjunto de datos. Si la serie temporal tiene el debajo de características, se recomienda usar la detección de batch para obtener una vista previa de los datos en una llamada API estándar.

* Serie temporal estacional, las anomalías ocasionales.
* Una serie temporal de tendencia plana, con picos ocasionales/DIP. 

No se recomienda usar la detección de anomalías de lote de datos en tiempo real de supervisión o uso en los datos de series temporales que no tienen por encima de las características. 

* Detección de lote crea y aplica a un único modelo, se realiza la detección para cada punto en el contexto de la serie completa. Si las tendencias de datos de series de tiempo arriba y abajo sin estacionalidad, algunos puntos de cambian (DIP y picos de actividad en los datos) puede faltar por el modelo. De forma similar, algunos puntos de cambio que son menos importantes que aquellos más adelante en el conjunto de datos no se contarán como suficientemente importante como para incorporarlo en el modelo.

* Detección de lote es más lenta que detectar el estado de anomalías del punto más reciente al realizar la supervisión de los datos en tiempo real, debido al número de puntos que se está analizando.

Para la supervisión de datos en tiempo real, se recomienda detectar el estado de anomalías de su punto más reciente de datos. Aplicando continuamente la detección de punto más reciente, supervisión de los datos de transmisión por secuencias se puede hacer más eficaz y precisa.

El ejemplo siguiente describe el impacto de que estos modos de detección pueden tener en el rendimiento. La primera imagen muestra el resultado de detección de forma continua el punto más reciente de estado de anomalías a lo largo de los puntos de datos vistos con anterioridad 28. Los puntos rojos son anomalías.

![Una imagen que muestra la detección de anomalías con el punto más reciente](../media/last.png)

A continuación es el mismo conjunto de datos mediante la detección de anomalías de batch. El modelo creado para la operación ha ignorado varias anomalías, marcadas con rectángulos.

![Una imagen que muestra la detección de anomalías con el método de proceso por lotes](../media/entire.png)

## <a name="data-preparation"></a>Preparación de los datos

La API del Detector de anomalías acepta la serie temporal de datos con formato en un objeto de solicitud JSON. Una serie temporal puede ser cualquier dato numérico registrado con el tiempo en orden secuencial. Puede enviar ventanas de datos de series temporales para el punto de conexión de API del Detector de anomalías para mejorar el rendimiento de la API. El número mínimo de puntos de datos que puede enviar es 12, y el máximo es 8640 puntos. 

Puntos de datos que se envían a la API del Detector de anomalías deben tener una marca de tiempo válida en hora Universal coordinada (UTC) y el valor numérico. 

```json
{
    "granularity": "daily",
    "series": [
      {
        "timestamp": "2018-03-01T00:00:00Z",
        "value": 32858923
      },
      {
        "timestamp": "2018-03-02T00:00:00Z",
        "value": 29615278
      },
    ]
}
```

### <a name="missing-data-points"></a>Puntos de datos que faltan

Los puntos de datos que faltan son comunes en conjuntos de datos de series de tiempo distribuido uniformemente, especialmente aquellos con una granularidad fina (un intervalo de muestreo pequeño. Por ejemplo, los datos muestrean cada pocos minutos). Falta menos del 10% del número esperado de puntos en los datos no debería tener un impacto negativo en los resultados de la detección. Tenga en cuenta rellenando los huecos en los datos según sus características, como sustitución de los puntos de datos de un período anterior, interpolación lineal o una media móvil.

### <a name="aggregate-distributed-data"></a>Agregar datos distribuidos

La API del Detector de anomalías funciona mejor en una serie de tiempo distribuido uniformemente. Si los datos se distribuyen aleatoriamente, debe agregarla como una unidad de tiempo, como por minuto, hora, o por día.

## <a name="anomaly-detection-on-data-with-seasonal-patterns"></a>Detección de anomalías en datos con patrones estacionales

Si sabe que los datos de serie temporal tienen un patrón estacional (uno que se produce a intervalos regulares), puede mejorar la precisión y tiempo de respuesta de API. 

Especificar un `period` al construir la solicitud JSON puede reducir la latencia de detección de anomalías en hasta un 50%. El `period` es un entero que especifica la serie temporal de los puntos de datos aproximadamente cuántos necesarias para repetir un patrón. Por ejemplo, tendría una serie temporal con un punto de datos al día una `period` como `7`, y una serie temporal con un punto por cada hora (con el mismo patrón semanal) tendría un `period` de `7*24`. Si no está seguro de los patrones de sus datos, no tiene que especificar este parámetro.

Para obtener mejores resultados, proporcione 4 `period`del valor de punto de datos, además de otra adicional. Por ejemplo, los datos por hora con un patrón semanal como se describió anteriormente deben proporcionar 673 puntos de datos en el cuerpo de solicitud (`7 * 24 * 4 + 1`).

### <a name="sampling-data-for-real-time-monitoring"></a>Datos de muestreo para la supervisión en tiempo real

Si se muestrean los datos de streaming en un breve intervalo (por ejemplo, segundos o minutos), envío el número recomendado de puntos de datos puede superar máximo número permitido (8640 puntos de datos) de la API Detector de anomalías. Si los datos muestran un patrón estacional estable, considere la posibilidad de enviar una muestra de datos de series temporales en un intervalo de tiempo mayor, como horas. Muestrear datos de esta manera puede mejorar notablemente también el tiempo de respuesta de API. 

## <a name="next-steps"></a>Pasos siguientes

* [¿Qué es la API del Detector de anomalías?](../overview.md)
* [Inicio rápido: Detectar anomalías en datos de series temporales mediante la API de REST del Detector de anomalías](../quickstarts/detect-data-anomalies-csharp.md)
