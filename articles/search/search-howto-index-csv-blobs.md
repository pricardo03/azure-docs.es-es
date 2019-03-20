---
title: 'Indexación de blobs CSV con el indexador de blobs de Azure Search: Azure Search'
description: Rastree los blob CSV en Azure Blob Storage para la búsqueda de texto completo mediante un índice de Azure Search. Los indexadores automatizan la ingesta de datos para orígenes de datos seleccionados, como Azure Blob Storage.
ms.date: 03/01/2019
author: mgottein
manager: cgronlun
ms.author: magottei
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.custom: seodec2018
ms.openlocfilehash: 0bbb131b5fb155443c8c3dc340185f3a6fa950a3
ms.sourcegitcommit: 3f4ffc7477cff56a078c9640043836768f212a06
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2019
ms.locfileid: "57312933"
---
# <a name="indexing-csv-blobs-with-azure-search-blob-indexer"></a>Indexación de blobs CSV con el indexador de blobs de Azure Search
De forma predeterminada, el [indexador de blobs de Azure Search](search-howto-indexing-azure-blob-storage.md) analiza los blobs de texto delimitados como un único fragmento de texto. Sin embargo, con blobs que contienen datos CSV, a menudo se desea tratar cada línea del blob como un documento independiente. Por ejemplo, dado el siguiente texto delimitado, tal vez prefiera analizarlo en dos documentos, donde cada uno incluya los campos "id", "datePublished" y "tags": 

    id, datePublished, tags
    1, 2016-01-12, "azure-search,azure,cloud" 
    2, 2016-07-07, "cloud,mobile" 

En este artículo se puede obtener información sobre cómo analizar blobs de CSV con un indexador de blobs de Azure Search. 

> [!NOTE]
> Siga las recomendaciones de configuración de indexador de [indización de uno a varios](search-howto-index-one-to-many-blobs.md) para generar varios documentos de búsqueda de un blob de Azure.

## <a name="setting-up-csv-indexing"></a>Configuración de la indexación de CSV
Para indexar blobs CSV, cree o actualice una definición de indexador con el modo de análisis `delimitedText` :  

    {
      "name" : "my-csv-indexer",
      ... other indexer properties
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "firstLineContainsHeaders" : true } }
    }

Para más información sobre la API Create Indexer, consulte [Crear indexador](https://docs.microsoft.com/rest/api/searchservice/create-indexer).

`firstLineContainsHeaders` indica que la primera línea (no en blanco) de cada blob contiene encabezados.
Si el blob no contienen una línea de encabezado inicial, los encabezados deben especificarse en la configuración del indexador: 

    "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } } 

Puede personalizar el carácter delimitador mediante la configuración de `delimitedTextDelimiter`. Por ejemplo: 

    "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextDelimiter" : "|" } }

> [!NOTE]
> Actualmente, solo se admite la codificación UTF-8. Si necesita compatibilidad con otras codificaciones, vote a favor de ella en [UserVoice](https://feedback.azure.com/forums/263029-azure-search).

> [!IMPORTANT]
> Cuando se utiliza el modo de análisis de texto delimitado, Azure Search supone que todos los blobs del origen de datos serán CSV. Si necesita compatibilidad con una combinación de blobs CSV y no CSV en el mismo origen de datos, vote por ella en [UserVoice](https://feedback.azure.com/forums/263029-azure-search).
> 
> 

## <a name="request-examples"></a>Ejemplos de solicitud
Resumiendo, estos son los ejemplos de cargas útiles completas. 

Origen de datos: 

    POST https://[service name].search.windows.net/datasources?api-version=2017-11-11-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-container", "query" : "<optional, my-folder>" }
    }   

Indexador:

    POST https://[service name].search.windows.net/indexers?api-version=2017-11-11-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-csv-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } }
    }

## <a name="help-us-make-azure-search-better"></a>Ayúdenos a mejorar Azure Search
Si tiene solicitudes o ideas para mejorar las características, comuníquelas en [UserVoice](https://feedback.azure.com/forums/263029-azure-search/).

