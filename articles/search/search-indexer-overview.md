---
title: 'Los indizadores para rastrear orígenes de datos durante la indexación: Azure Search'
description: Rastree Azure SQL Database, Azure Cosmos DB o Azure Storage para extraer los datos utilizables en búsquedas y rellenar un índice de Azure Search.
author: HeidiSteen
manager: nitinme
services: search
ms.service: search
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: heidist
ms.openlocfilehash: e50d88181a27dcc46da858f220404eb09ad9b4bd
ms.sourcegitcommit: f176e5bb926476ec8f9e2a2829bda48d510fbed7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/04/2019
ms.locfileid: "70308969"
---
# <a name="indexers-in-azure-search"></a>Indexadores de Azure Search

Un *indexador* de Azure Search es un rastreador (crawler) que extrae datos y metadatos utilizables en búsquedas de un origen de datos de Azure externo y rellena un índice basado en las asignaciones de un campo a otro entre el índice y su origen de datos. Este enfoque se denomina a veces "modelo de extracción", porque el servicio extrae datos sin que sea preciso escribir código que agregue datos en un índice.

Los indexadores se basan en tipos de orígenes de datos o plataformas, y hay un indexador individual para SQL Server en Azure, Cosmos DB, Azure Table Storage y Blob Storage. Los indexadores de Blob Storage tienen propiedades adicionales específicas de tipos de contenido del blob.

Puede utilizar un indexador como único medio para la ingesta de datos. o bien utilizar una combinación de técnicas que incluyan el uso de un indexador para cargar solo algunos de los campos en el índice.

Puede ejecutar los indexadores a petición o con una programación de actualización periódica de datos que se ejecuta con una frecuencia de incluso cada cinco minutos. Actualizaciones más frecuentes requieren un modelo de inserción que actualiza simultáneamente los datos en Azure Search y su origen de datos externo.

## <a name="approaches-for-creating-and-managing-indexers"></a>Enfoques para crear y administrar indexadores

Puede crear y administrar indexadores mediante estos enfoques:

* [Portal > Asistente para la importación de datos](search-import-data-portal.md)
* [API de REST de servicio](https://docs.microsoft.com/rest/api/searchservice/Indexer-operations)
* [SDK de .NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.iindexersoperations)

Inicialmente, un nuevo indexador se anuncia como una característica en versión preliminar. Las características en versión preliminar se introdujeron en las API (REST y. NET) y luego se integraron en el portal después de aprobarse para disponibilidad general. Si va a evaluar un nuevo indexador, debe pensar en escribir código.

## <a name="permissions"></a>Permisos

Todas las operaciones relacionadas con los indexadores, incluidas las solicitudes GET para estado o definiciones, requieren una [clave de API de administración](search-security-api-keys.md). 

<a name="supported-data-sources"></a>

## <a name="supported-data-sources"></a>Orígenes de datos admitidos

Los indexadores rastrean los almacenes de datos en Azure.

* [SQL de Azure](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
* [Azure Cosmos DB](search-howto-index-cosmosdb.md)
* [Azure Blob Storage](search-howto-indexing-azure-blob-storage.md)
* [Azure Table Storage](search-howto-indexing-azure-tables.md)

## <a name="basic-configuration-steps"></a>Pasos básicos de configuración
Los indexadores pueden ofrecer características que son exclusivas del origen de datos. En este sentido, algunos aspectos de la configuración de orígenes de datos o indexadores varían según el tipo de indexador. No obstante, todos los indexadores comparten composición básica y requisitos. Más adelante, se explican los pasos que son comunes a todos los indexadores.

### <a name="step-1-create-a-data-source"></a>Paso 1: Creación de un origen de datos
Un indexador obtiene la conexión de origen de datos desde un objeto de *origen de datos*. La definición del origen de datos proporciona una cadena de conexión y, posiblemente, las credenciales. Para crear el recurso, llame a la API de REST [Create Datasource](https://docs.microsoft.com/rest/api/searchservice/create-data-source) o [clase DataSource](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.datasource).

Los orígenes de datos se configuran y administran independientemente de los indexadores que los usan, lo que significa que varios indexadores pueden usar un origen de datos para cargar más de un índice a la vez.

### <a name="step-2-create-an-index"></a>Paso 2: Creación de un índice
Un indexador automatizará algunas tareas relacionadas con la ingesta de datos, pero la creación de un índice no suele ser una de ellas. Como requisito previo, debe tener un índice predefinido con campos que coincidan con los del origen de datos externo. Los campos deben coincidir por nombre y tipo de datos. Para más información sobre la estructura de un índice, consulte [Create an Index (Azure Search REST API)](https://docs.microsoft.com/rest/api/searchservice/Create-Index) [Creación de un índice (API de REST de Azure Search)] o [clase Index](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.index). Para obtener ayuda con las asociaciones de campo, consulte [Asignaciones de campos en los indexadores de Azure Search](search-indexer-field-mappings.md).

> [!Tip]
> Aunque los indexadores no pueden generar un índice, el Asistente para **importar datos** del portal puede ayudarle. En la mayoría de los casos, el asistente puede inferir un esquema de índice de los metadatos existentes en el origen y presentar un esquema de índice preliminar que se pueda modificar en línea mientras el asistente esté activo. Una vez que el índice se crea en el servicio, la mayoría de las posteriores modificaciones se limitan a agregar nuevos campos. Tenga en cuenta al asistente para crear índices, pero no para revisarlos. Para obtener conocimientos prácticos, recorra el [tutorial del portal](search-get-started-portal.md).

### <a name="step-3-create-and-schedule-the-indexer"></a>Paso 3: Creación y programación del indizador
La definición del indexador es una construcción que reúne todos los elementos relacionados con la ingesta de datos. Los elementos necesarios incluyen un origen de datos y un índice. Los elementos opcionales incluyen una programación y asignaciones de campos. La asignación de campos solo es opcional si los campos de origen y los campos de índice se corresponden claramente. Un indexador puede hacer referencia a un origen de datos desde otro servicio, siempre que ese origen de datos pertenezca a la misma suscripción. Para más información sobre la estructura de un indexador, consulte [Crear el indizador (API de REST de Azure Search)](https://docs.microsoft.com/rest/api/searchservice/Create-Indexer).

<a id="RunIndexer"></a>

## <a name="run-indexers-on-demand"></a>Ejecutar indexadores a petición

Aunque es común para programar la indexación, un indexador también puede invocarse a petición mediante el [comando Ejecutar](https://docs.microsoft.com/rest/api/searchservice/run-indexer):

    POST https://[service name].search.windows.net/indexers/[indexer name]/run?api-version=2019-05-06
    api-key: [Search service admin key]

> [!NOTE]
> Cuando la API de Run se devuelve correctamente, se ha programado la invocación del indexador, pero el procesamiento real se produce de forma asincrónica. 

El estado del indexador se puede supervisar en el portal o mediante la API de Obtener el estado del indexador. 

<a name="GetIndexerStatus"></a>

## <a name="get-indexer-status"></a>Obtener el estado del indexador

Puede recuperar el estado y el historial de ejecución de un indexador a través del [comando Obtener estado del indexador](https://docs.microsoft.com/rest/api/searchservice/get-indexer-status):


    GET https://[service name].search.windows.net/indexers/[indexer name]/status?api-version=2019-05-06
    api-key: [Search service admin key]

La respuesta contiene el estado general del indexador, la última vez que se invocó al indexador (o en curso) y el historial de las recientes invocaciones al indexador.

    {
        "status":"running",
        "lastResult": {
            "status":"success",
            "errorMessage":null,
            "startTime":"2018-11-26T03:37:18.853Z",
            "endTime":"2018-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
         },
        "executionHistory":[ {
            "status":"success",
             "errorMessage":null,
            "startTime":"2018-11-26T03:37:18.853Z",
            "endTime":"2018-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
        }]
    }

El historial de ejecución contiene como máximo las 50 ejecuciones completadas más recientemente en orden cronológico inverso (la ejecución más reciente aparece en primer lugar).

## <a name="next-steps"></a>Pasos siguientes
Ahora que tiene el concepto básico, el paso siguiente consiste en revisar los requisitos y las tareas específicos de cada tipo de origen de datos.

* [Azure SQL Database o SQL Server en una máquina virtual de Azure](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
* [Azure Cosmos DB](search-howto-index-cosmosdb.md)
* [Azure Blob Storage](search-howto-indexing-azure-blob-storage.md)
* [Azure Table Storage](search-howto-indexing-azure-tables.md)
* [Indexación de blobs CSV con el indexador de blobs de Azure Search](search-howto-index-csv-blobs.md)
* [Indexación de blobs JSON con el indexador de blobs de Azure Search](search-howto-index-json-blobs.md)
