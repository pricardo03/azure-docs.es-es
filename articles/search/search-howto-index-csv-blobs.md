---
title: 'Indexación de blobs CSV con el indexador de blobs de Azure Search: Azure Search'
description: Rastree los blob CSV en Azure Blob Storage para la búsqueda de texto completo mediante un índice de Azure Search. Los indexadores automatizan la ingesta de datos para orígenes de datos seleccionados, como Azure Blob Storage.
ms.date: 05/02/2019
author: mgottein
manager: nitinme
ms.author: magottei
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.custom: seodec2018
ms.openlocfilehash: b135fd1a0758567a7b504996bf442a913741fe59
ms.sourcegitcommit: bb8e9f22db4b6f848c7db0ebdfc10e547779cccc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/20/2019
ms.locfileid: "69656755"
---
# <a name="indexing-csv-blobs-with-azure-search-blob-indexer"></a>Indexación de blobs CSV con el indexador de blobs de Azure Search

> [!Note]
> El modo de análisis delimitedText se encuentra en versión preliminar y no está pensado para su uso en producción. En la [API REST, versión 2019-05-06-Preview](search-api-preview.md) se proporciona esta característica. Por el momento, no hay compatibilidad con .NET SDK.
>

De forma predeterminada, el [indexador de blobs de Azure Search](search-howto-indexing-azure-blob-storage.md) analiza los blobs de texto delimitados como un único fragmento de texto. Sin embargo, con blobs que contienen datos CSV, a menudo se desea tratar cada línea del blob como un documento independiente. Por ejemplo, dado el siguiente texto delimitado, tal vez prefiera analizarlo en dos documentos, donde cada uno incluya los campos "id", "datePublished" y "tags": 

    id, datePublished, tags
    1, 2016-01-12, "azure-search,azure,cloud" 
    2, 2016-07-07, "cloud,mobile" 

En este artículo, obtendrá información sobre cómo analizar blobs de CSV con un indizador de blobs de Azure Search mediante el establecimiento del modo de análisis `delimitedText`. 

> [!NOTE]
> Siga las recomendaciones de configuración de indizadores de [Indexación de uno a varios](search-howto-index-one-to-many-blobs.md) para generar varios documentos de búsqueda a partir de un blob de Azure.

## <a name="setting-up-csv-indexing"></a>Configuración de la indexación de CSV
Para indexar blobs de CSV, cree o actualice una definición de indizador con el modo de análisis `delimitedText` en una solicitud [Crear indizador](https://docs.microsoft.com/rest/api/searchservice/create-indexer):

    {
      "name" : "my-csv-indexer",
      ... other indexer properties
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "firstLineContainsHeaders" : true } }
    }

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

    POST https://[service name].search.windows.net/datasources?api-version=2019-05-06-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-container", "query" : "<optional, my-folder>" }
    }   

Indexador:

    POST https://[service name].search.windows.net/indexers?api-version=2019-05-06-Preview
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

