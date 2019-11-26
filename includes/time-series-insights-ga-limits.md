---
title: archivo de inclusión
description: archivo de inclusión
services: digital-twins
ms.service: digital-twins
ms.topic: include
ms.date: 10/16/2019
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.custom: include file
ms.openlocfilehash: e8c040bc186932680ce7eb4418a787dc061d02ce
ms.sourcegitcommit: 92d42c04e0585a353668067910b1a6afaf07c709
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/28/2019
ms.locfileid: "72991164"
---
A continuación se resumen los límites principales en la disponibilidad general.

### <a name="sku-ingress-rates-and-capacities"></a>Capacidades y tasas de entrada de SKU

Las capacidades y tasas de entrada de SKU S1 y S2 proporcionan flexibilidad al configurar un nuevo entorno de Time Series Insights.

| Capacidad de SKU de S1 | Velocidad de entrada | Capacidad máxima de almacenamiento
| --- | --- | --- |
| 1 | 1 GB (1 millones de eventos) | 30 GB (30 millones de eventos) al mes |
| 10 | 10 GB (10 millones de eventos) | 300 GB (300 millones de eventos) al mes |

| Capacidad de SKU de S2 | Velocidad de entrada | Capacidad máxima de almacenamiento
| --- | --- | --- |
| 1 | 10 GB (10 millones de eventos) | 300 GB (300 millones de eventos) al mes |
| 10 | 100 GB (100 millones de eventos) | 3 TB (3 mil millones de eventos) al mes |

> [!NOTE]
> Las capacidades se escalan linealmente, por lo que una SKU de S1 con capacidad 2 admite una velocidad de entrada de 2 GB (2 millones) de eventos al día y 60 GB (60 millones de eventos) al mes.

Los entornos de SKU de S2 admiten muchos más eventos al mes y tienen una capacidad de entrada mucho mayor.

| SKU  | Recuento de eventos cada mes  | Tamaño de eventos cada mes  | Recuento de eventos cada minuto | Tamaño de eventos cada minuto  |
|---------|---------|---------|---------|---------|
| S1     |   30 millones     |  30 GB     |  720    |  720 KB   |
 |S2     |   300 millones    |   < 300 GB   | 7200   | 7200 KB  |

### <a name="property-limits"></a>Límites de propiedad

Los límites de propiedad de la disponibilidad general dependen del entorno de SKU seleccionado. Las propiedades de evento proporcionadas tienen las columnas JSON, CSV y de gráfico correspondientes que se pueden ver en el [Explorador de Time Series Insights](https://docs.microsoft.com/azure/time-series-insights/time-series-quickstart).

| SKU | Propiedades máximas |
| --- | --- |
| S1 | 600 propiedades (columnas) |
| S2 | 800 propiedades (columnas) |

### <a name="event-sources"></a>Orígenes de eventos

Se admite un máximo de dos orígenes del evento por instancia. 

* Obtenga información sobre cómo [agregar un origen de Event Hub](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-how-to-add-an-event-source-eventhub).
* Configure [un origen de centro de IoT](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-how-to-add-an-event-source-iothub).

### <a name="api-limits"></a>Límites de API

Los límites de la API REST para la disponibilidad general de Time Series Insights están especificadas en la [documentación de referencia de la API REST](https://docs.microsoft.com/rest/api/time-series-insights/ga-query-api#limits).