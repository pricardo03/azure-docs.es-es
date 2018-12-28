---
title: Consulta de datos de la versión preliminar de Azure Time Series Insights | Microsoft Docs
description: Consulta de datos de la versión preliminar de Azure Time Series Insights.
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 48a9a1d0f79a7a36b90fa87651a5283cba87de20
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2018
ms.locfileid: "53275812"
---
# <a name="data-querying"></a>Consultas de datos

La versión preliminar de Azure Time Series Insights le permite consultar datos en los eventos y los metadatos almacenados en el entorno a través de las API de superficie públicas. Estas API también se usan en el [explorador de la versión preliminar de Time Series Insights](./time-series-insights-update-explorer.md).

Existen tres categorías principales de API en Time Series Insights:

* **API de entorno**: Permite las consultas del mismo entorno de Time Series Insights. Como ejemplos de consultas, tenemos la lista de entornos a los que el autor de la llamada tiene acceso y los metadatos del entorno.

* **API de consulta de modelo de serie temporal (TSM-Q)**: Permite crear, leer, actualizar y eliminar operaciones en metadatos almacenados en la parte del entorno del modelo de serie temporal. Algunos ejemplos son las instancias, los tipos y las jerarquías.

* **API de consulta de serie temporal (TSQ)**: Permite la recuperación de datos de eventos a medida que se registra en el proveedor de origen. Estas API pueden realizar operaciones para transformar, combinar y realizar cálculos en datos de series temporales.

El [lenguaje Time Series Expression (TSX)](https://docs.microsoft.com/rest/api/time-series-insights/preview-tsx) es una cuarta categoría extremadamente eficaz. Usa los modelos de serie temporal para permitir la composición de cálculo avanzado.

## <a name="azure-time-series-insights-preview-core-apis"></a>API principales de la versión preliminar de Azure Time Series Insights

Se admiten los siguientes tipos de API principal.

![tsq][1]

### <a name="environment-apis"></a>API de entorno

Las siguientes Environment API están disponibles:

* [Get Environment API](https://docs.microsoft.com/rest/api/time-series-insights/preview-env#get-environments-api): Devuelve la lista de entornos a los que el autor de la llamada puede obtener acceso.
* [Get Environment Availability API](https://docs.microsoft.com/rest/api/time-series-insights/preview-env#get-environment-availability-api): Devuelve la distribución del recuento de eventos a través de la marca de tiempo `$ts` del evento. Esta API ayuda a determinar si hay cualquier evento en la marca de tiempo al devolver el recuento de eventos, si es que existe alguno.
* [Get Event Schema API](https://docs.microsoft.com/rest/api/time-series-insights/preview-env#get-event-schema-api): Devuelve los metadatos de esquema de eventos para un intervalo de búsqueda determinado. Esta API le ayuda a recuperar todos los metadatos y propiedades disponibles en el esquema para el intervalo de búsqueda determinado.

### <a name="time-series-model-query-tsm-q-apis"></a>API de consulta de modelo de serie temporal (TSM-Q)

Las siguientes API de consulta de modelo de serie temporal están disponibles:

* [Model Settings API](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#model-settings-api): Permite obtener y aplicar revisiones en el tipo de valor predeterminado y en el nombre del modelo del entorno.
* [Types API](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#types-api): Habilita CRUD en tipos de serie temporal y en sus variables asociadas.
* [Hierarchies API](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#hierarchies-api): Habilita CRUD en jerarquías de serie temporal y en sus variables asociadas.
* [Instances API](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#instances-api): Habilita CRUD en jerarquías de serie temporal y en sus campos de instancias asociados.

### <a name="time-series-query-tsq-apis"></a>API de consulta de serie temporal (TSQ)

Las siguientes API de consulta de serie temporal están disponibles:

* [Get Events API](https://docs.microsoft.com/rest/api/time-series-insights/preview-query#get-events-api): Habilita la consulta y recuperación de datos de Time Series Insights de eventos que se registran en la instancia de Time Series Insights del proveedor de origen.

* [Get Series API](https://docs.microsoft.com/rest/api/time-series-insights/preview-query#get-series-api): Habilita la consulta y recuperación de datos de Time Series Insights de eventos capturados mediante el uso de los datos registrados en la transferencia. Los valores que se devuelven se basan en las variables que se han definido en el modelo o que se proporcionaron en línea.

    >[!NOTE]
    > Incluso si se han especificado en un modelo o si se han proporcionado en línea, se omite la cláusula Aggregation.

  Get Series API devuelve un valor de serie temporal para cada variable de cada intervalo. Un valor de serie temporal es un formato que Time Series Insights usa para los elementos de salida JSON desde una consulta. Los valores que se devuelven se basan en el identificador de serie temporal y en el conjunto de variables que se han proporcionado.

* [Aggregate Series API](https://docs.microsoft.com/rest/api/time-series-insights/preview-query#aggregate-series-api): Habilita la consulta y recuperación de datos de Time Series Insights de eventos capturados mediante el muestreo y la agregación de los datos registrados.

  Aggregate Series API devuelve un valor de serie temporal para cada variable de cada intervalo. Los valores se basan en el identificador de serie temporal y en el conjunto de variables que se han proporcionado. Asimismo, Aggregate Series API logra una reducción mediante las variables almacenadas en el modelo de serie temporal o las que se proporcionan en línea a los datos de ejemplo o agregados.

  Tipos de agregación admitidos: `Min`, `Max`, `Sum`, `Count`, `Average`.

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información:

- [Azure Time Series Insights Preview storage and ingress](./time-series-insights-update-storage-ingress.md) (Almacenamiento y entrada de datos en la versión preliminar de Azure Time Series Insights)
- [Data modeling](./time-series-insights-update-tsm.md) (Modelado de datos)
- [Best practices when choosing a Time Series ID](./time-series-insights-update-how-to-id.md) (Procedimientos recomendados al elegir un id. de serie temporal)

<!-- Images -->
[1]: media/v2-update-tsq/tsq.png
