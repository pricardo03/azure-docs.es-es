---
title: Cómo usar la API del Detector de anomalías en datos de series temporales
description: Aprenda a detectar anomalías en los datos como un lote, o sobre los datos transmitidos.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: article
ms.date: 03/26/2019
ms.author: aahi
ms.openlocfilehash: 63ede8fe90d5c19c2473ffb315bf6096599ffb9c
ms.sourcegitcommit: fbfe56f6069cba027b749076926317b254df65e5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2019
ms.locfileid: "58473334"
---
# <a name="how-to-use-the-anomaly-detector-api-on-your-time-series-data"></a>Procedimientos para: Usar la API del Detector de anomalías en datos de series temporales  

El [API del Detector de anomalías](https://westus2.dev.cognitive.microsoft.com/docs/services/AnomalyDetector/operations/post-timeseries-entire-detect) proporciona dos métodos de detección de anomalías. O bien pueden detectar anomalías como un lote a lo largo de los tiempos de serie, o los datos se generan al detectar el estado de anomalías del punto de datos más reciente. El modelo de detección devuelve resultados anomalías junto con el valor esperado de cada punto de datos y las anomalías superior e inferior los límites de detección. Puede utilizar estos valores para visualizar el intervalo de valores normales y las anomalías en los datos.

## <a name="anomaly-detection-modes"></a>Modos de detección de anomalías 

La API del Detector de anomalías proporciona modos de detección: proceso por lotes y streaming.

> [!NOTE]
> La siguiente solicitud que las direcciones URL se deben combinar con el punto de conexión adecuado para su suscripción. Por ejemplo: `https://westus2.api.cognitive.microsoft.com/anomalydetector/v1.0/timeseries/entire/detect`


### <a name="batch-detection"></a>Detección de lote

Para detectar anomalías a lo largo de un lote de puntos de datos a través de un intervalo de tiempo determinado, utilice el siguiente URI de solicitud con datos de series temporales: 

`/timeseries/entire/detect`. 

Mediante el envío de datos de series temporales a la vez, la API generará un modelo con toda la serie y analizar cada punto de datos con ella.  

### <a name="streaming-detection"></a>Detección de transmisión por secuencias

Para continuamente, detectar anomalías en datos de streaming, use el siguiente URI de solicitud con el punto de datos más reciente: 

`/timeseries/last/detect'`. 

Mediante el envío de nuevos puntos de datos como generarlos, puede supervisar los datos en tiempo real. Se generará un modelo con los puntos de datos que envíe y la API determinará si el punto más reciente de la serie temporal es una anomalía.

## <a name="adjusting-lower-and-upper-anomaly-detection-boundaries"></a>Ajustar los límites de detección de anomalías inferior y superior

De forma predeterminada, los límites superiores e inferiores de la detección de anomalías se calculan con `expectedValue`, `upperMargin`, y `lowerMargin`. Si necesita límites diferentes, se recomienda aplicar un `marginScale` a `upperMargin` o `lowerMargin`. Los límites se calcularía del siguiente modo:

|Límite  |Cálculo  |
|---------|---------|
|`upperBoundary` | `expectedValue + (100 - marginScale) * upperMargin`        |
|`lowerBoundary` | `expectedValue - (100 - marginScale) * lowerMargin`        |

Los ejemplos siguientes muestran un resultado de la API del Detector de anomalías en los efectos diferentes.

### <a name="example-with-sensitivity-at-99"></a>Ejemplo con la sensibilidad a 99

![Sensibilidad predeterminada](../media/sensitivity_99.png)

### <a name="example-with-sensitivity-at-95"></a>Ejemplo con la sensibilidad a 95

![Sensibilidad 99](../media/sensitivity_95.png)

### <a name="example-with-sensitivity-at-85"></a>Ejemplo con la sensibilidad a 85

![Sensibilidad 85](../media/sensitivity_85.png)

## <a name="next-steps"></a>Pasos siguientes

* [¿Qué es la API del Detector de anomalías?](../overview.md)
* [Inicio rápido: Detectar anomalías en datos de series temporales mediante la API de REST del Detector de anomalías](../quickstarts/detect-data-anomalies-csharp.md)