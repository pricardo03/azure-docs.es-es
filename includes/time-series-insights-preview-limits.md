---
title: archivo de inclusión
description: archivo de inclusión
services: digital-twins
ms.service: digital-twins
ms.topic: include
ms.date: 12/06/2019
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.custom: include file
ms.openlocfilehash: be46a0dda24dc990e895a3e983c730337f9cf31d
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2019
ms.locfileid: "74981393"
---
### <a name="general-availability-and-preview-comparison"></a>Comparación entre disponibilidad general y versión preliminar

En la tabla siguiente se resumen varias diferencias clave entre las instancias de disponibilidad general (GA) y versión preliminar de Azure Time Series Insights.

| | GA | Vista previa |
| --- | --- | ---|
| Ciudadano de primera clase | Centrada en eventos | Centrada en series temporales |
| Razonamiento semántico | Bajo nivel (datos de referencia) | Alto nivel (modelos) |
| Contextualización de datos | Nivel no de dispositivo | Nivel de dispositivo y no de dispositivo |
| Almacenamiento de lógica de proceso | Sin | Almacenado en la parte de variables de tipo del modelo |
| Control de almacenamiento y acceso | Sin | Habilitado a través del modelo |
| Agregaciones/muestreo | Sin | Evento ponderado y tiempo ponderado |
| Reconstrucción de señal | Sin | Interpolación |
| Producción de series temporales derivadas | Sin | Sí, combinaciones y uniones |
| Flexibilidad de lenguaje | No admite composición | Admite composición |
| Lenguaje de expresiones | Cadena de predicados | Expresiones de series temporales (cadenas de predicado, valores, expresiones y funciones) |

### <a name="property-limits"></a>Límites de propiedad

Los límites de propiedad de Time Series Insights han aumentado a 1000 desde un límite máximo de 800 en GA. Las propiedades de evento proporcionadas tienen columnas JSON, CSV y de gráfico correspondientes que puede ver en el [explorador de la versión preliminar de Time Series Insights](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-update-quickstart).

| SKU | Propiedades máximas |
| --- | --- |
| Versión preliminar de pago por uso | Propiedades de 1000 (columnas) |
| GA S1 | 600 propiedades (columnas) |
| GA S2 | 800 propiedades (columnas) |

### <a name="event-sources"></a>Orígenes de eventos

Se admite un máximo de dos orígenes de evento por instancia. 

* Obtenga información sobre cómo [agregar un origen de Event Hub](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-how-to-add-an-event-source-eventhub).
* Configure [un origen de centro de IoT](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-how-to-add-an-event-source-iothub).

### <a name="api-limits"></a>Límites de API

Los límites de la API de REST para la versión preliminar de Time Series Insights están especificadas en la [documentación de referencia de la API de REST](https://docs.microsoft.com/rest/api/time-series-insights/preview-query#limits).