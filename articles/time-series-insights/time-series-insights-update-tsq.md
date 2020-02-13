---
title: Consulta de datos de la versión preliminar de Azure Time Series Insights | Microsoft Docs
description: Información general sobre los conceptos de consulta de datos y la API REST de HTTP en Azure Time Series Insights Preview.
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 02/07/2020
ms.custom: seodec18
ms.openlocfilehash: 898515f49672a19ed8bf1c62439128b6727afc73
ms.sourcegitcommit: cfbea479cc065c6343e10c8b5f09424e9809092e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/08/2020
ms.locfileid: "77087408"
---
# <a name="data-querying-in-azure-time-series-insights-preview"></a>Consulta de datos de la versión preliminar de Azure Time Series Insights

La versión preliminar de Azure Time Series Insights le permite consultar datos en los eventos y los metadatos almacenados en el entorno a través de las API de superficie públicas. Estas API también se usan en el [explorador de la versión preliminar de Time Series Insights](./time-series-insights-update-explorer.md).

Existen tres categorías principales de API en Time Series Insights:

* **API de entorno**: estas API permiten consultas en el propio entorno de Time Series Insights. Como ejemplos de consultas, tenemos la lista de entornos a los que el autor de la llamada tiene acceso y los metadatos del entorno.
* **API de consulta de modelo de serie temporal (TSM-Q)** : Permite crear, leer, actualizar y eliminar (CRUD) operaciones en metadatos almacenados en la parte del entorno del modelo de serie temporal. Algunos ejemplos son las instancias, los tipos y las jerarquías.
* **API de consulta de serie temporal (TSQ)** : permite la recuperación de datos de telemetría o eventos a medida que se registran desde el proveedor de origen o mediante la reducción de los datos con la parte almacenada de las variables de las funciones escalares y funciones de agregado. Estas API pueden realizar operaciones para transformar, combinar y realizar cálculos en datos de series temporales.

Time Series Insights usa un lenguaje de expresiones enriquecido basado en cadenas, [Expresión de serie temporal (TSX)](https://docs.microsoft.com/rest/api/time-series-insights/preview-tsx), para expresar los cálculos.

## <a name="azure-time-series-insights-preview-core-apis"></a>API principales de la versión preliminar de Azure Time Series Insights

Se admiten los siguientes tipos de API principal.

[![Información general sobre la consulta de serie temporal](media/v2-update-tsq/tsq.png)](media/v2-update-tsq/tsq.png#lightbox)

## <a name="environment-apis"></a>API de entorno

Las siguientes Environment API están disponibles:

* [Get Environments API](/rest/api/time-series-insights/management/environments/get): Devuelve la lista de entornos a los que el autor de la llamada puede obtener acceso.
* [Get Environments Availability API](/rest/api/time-series-insights/dataaccess(preview)/query/getavailability): Devuelve la distribución del recuento de eventos a través de la marca de tiempo `$ts` del evento. Esta API ayuda a determinar si hay cualquier evento en la marca de tiempo al devolver el recuento de eventos, si es que existe alguno.
* [Get Event Schema API](/rest/api/time-series-insights/dataaccess(preview)/query/geteventschema): Devuelve los metadatos de esquema de eventos para un intervalo de búsqueda determinado. Esta API le ayuda a recuperar todos los metadatos y propiedades disponibles en el esquema para el intervalo de búsqueda determinado.

## <a name="time-series-model-query-tsm-q-apis"></a>API de consulta de modelo de serie temporal (TSM-Q)

Están disponibles las siguientes API de consulta de modelo de serie temporal: La mayoría de estas API admiten la operación de ejecución por lotes para permitir operaciones CRUD de lote en varias entidades de modelo de serie temporal:

* [Model Settings API](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#model-settings-api): permite operaciones *GET* y *PATCH* en el tipo predeterminado y el nombre del modelo del entorno.
* [Types API](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#types-api): Habilita CRUD en tipos de serie temporal y en sus variables asociadas.
* [Hierarchies API](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#hierarchies-api): Habilita CRUD en jerarquías de serie temporal y en sus variables asociadas.
* [Instances API](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#instances-api): Habilita CRUD en jerarquías de serie temporal y en sus campos de instancias asociados. Además, Instances API admite las siguientes operaciones:
  * [Buscar](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/timeseriesinstances/search): recupera una lista parcial de aciertos en la búsqueda de instancias de serie temporal según los atributos de instancia.
  * [Sugerir](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/timeseriesinstances/suggest): busca y sugiere una lista parcial de aciertos en la búsqueda de instancias de serie temporal según los atributos de instancia.

## <a name="time-series-query-tsq-apis"></a>API de consulta de serie temporal (TSQ)

Están disponibles las siguientes API de consulta de serie temporal: Estas API están disponibles en todos los almacenamientos de varios niveles admitidos en Time Series Insights. Los parámetros de dirección URL de consulta se usan para especificar el [tipo de almacén](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/query/execute#uri-parameters) en el que se debe ejecutar la consulta:

* [Get Events API](/rest/api/time-series-insights/dataaccess(preview)/query/execute#getevents): Habilita la consulta y recuperación de datos de Time Series Insights de eventos que se registran en la instancia de Time Series Insights del proveedor de origen. Esta API permite la recuperación de eventos sin procesar según un intervalo de búsqueda y un identificador de serie temporal dados. Esta API admite la paginación para recuperar el conjunto de datos completo de la entrada seleccionada. 

* [Get Series API](/rest/api/time-series-insights/dataaccess(preview)/query/execute#getseries): Habilita la consulta y recuperación de datos de Time Series Insights de eventos capturados mediante el uso de los datos registrados en la transferencia. Los valores que se devuelven se basan en las variables que se han definido en el modelo o que se proporcionaron en línea. Esta API admite la paginación para recuperar el conjunto de datos completo de la entrada seleccionada. Esta API ayuda a definir propiedades o columnas calculadas.

    >[!NOTE]
    > Incluso si se han especificado en un modelo o si se han proporcionado en línea, se omite la cláusula Aggregation.

  Get Series API devuelve un valor de serie temporal para cada variable de cada intervalo. Un valor de serie temporal es un formato que Time Series Insights usa para los elementos de salida JSON desde una consulta. Los valores que se devuelven se basan en el identificador de serie temporal y en el conjunto de variables que se han proporcionado.

* [Aggregate Series API](/rest/api/time-series-insights/dataaccess(preview)/query/execute#aggregatevariable): Habilita la consulta y recuperación de datos de Time Series Insights de eventos capturados mediante el muestreo y la agregación de los datos registrados. Esta API admite la ejecución continua mediante [tokens de continuación](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/query/execute#queryresultpage).

  Aggregate Series API devuelve un valor de serie temporal para cada variable de cada intervalo. Los valores se basan en el identificador de serie temporal y en el conjunto de variables que se han proporcionado. Asimismo, Aggregate Series API logra una reducción mediante las variables almacenadas en el modelo de serie temporal o las que se proporcionan en línea a los datos de ejemplo o agregados.

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre [almacenamiento y entrada](./time-series-insights-update-storage-ingress.md) en la versión preliminar de Azure Time Series Insights.
- Lea el artículo [Modelado de datos](./time-series-insights-update-tsm.md) de la versión preliminar de Azure Time Series Insights.
- Descubra los [procedimientos recomendados al elegir un identificador de serie temporal](./time-series-insights-update-how-to-id.md).
