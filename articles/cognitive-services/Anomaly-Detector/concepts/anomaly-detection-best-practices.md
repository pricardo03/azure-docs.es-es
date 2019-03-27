---
title: Procedimientos recomendados al usar la API del Detector de anomalías
description: Obtenga información sobre los procedimientos recomendados al detectar anomalías con la API del Detector de anomalías.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: article
ms.date: 03/26/2019
ms.author: aahi
ms.openlocfilehash: 467ac4e475a1c23e25b62c76cfbc959e7ed49465
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2019
ms.locfileid: "58484049"
---
# <a name="best-practices-for-using-the-anomaly-detector-api"></a>Procedimientos recomendados para usar la API del Detector de anomalías

La API del Detector de anomalías es un servicio de detección de anomalías sin estado. La precisión y el rendimiento de sus resultados pueden verse afectados por:

* Cómo se preparan los datos de series temporales.
* Los parámetros de la API del Detector de anomalías que se usaron.
* El número de puntos de datos de la solicitud de API. 

Use este artículo para obtener información sobre procedimientos recomendados para usar la API de obtener los mejores resultados para los datos. 

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
