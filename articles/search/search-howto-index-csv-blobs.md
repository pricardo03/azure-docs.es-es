---
title: Búsqueda en BLOb CSV
titleSuffix: Azure Cognitive Search
description: Extraiga e importe archivos .csv de Azure Blob Storage mediante el modo de análisis delimitedText, actualmente en versión preliminar pública.
manager: nitinme
author: mgottein
ms.author: magottei
ms.devlang: rest-api
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 2166e100f03f21c218618d19dc37ee70c6ab29ef
ms.sourcegitcommit: 598c5a280a002036b1a76aa6712f79d30110b98d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2019
ms.locfileid: "74113034"
---
# <a name="how-to-index-csv-blobs-using-delimitedtext-parsing-mode-and-blob-indexers-in-azure-cognitive-search"></a>Indexación de blobs CSV mediante el modo de análisis delimitedText e indexadores de blobs en Azure Cognitive Search 

> [!IMPORTANT] 
> El modo de análisis delimitedText está actualmente en versión preliminar pública. La funcionalidad de versión preliminar se ofrece sin un Acuerdo de Nivel de Servicio y no es aconsejable usarla para cargas de trabajo de producción. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). En la [API REST, versión 2019-05-06-Preview](search-api-preview.md) se proporciona esta característica. Actualmente no hay compatibilidad con el portal ni con el SDK de .NET.

De forma predeterminada, el [indizador de blobs de Azure Cognitive Search](search-howto-indexing-azure-blob-storage.md) analiza los blobs de texto delimitados como un único fragmento de texto. Sin embargo, con blobs que contienen datos CSV, a menudo se desea tratar cada línea del blob como un documento independiente. Por ejemplo, dado el siguiente texto delimitado, tal vez prefiera analizarlo en dos documentos, donde cada uno incluya los campos "id", "datePublished" y "tags": 

    id, datePublished, tags
    1, 2016-01-12, "azure-search,azure,cloud" 
    2, 2016-07-07, "cloud,mobile" 

En este artículo, aprenderá a analizar blobs CSV con un indizador de blobs de Azure Cognitive Search mediante el establecimiento del modo de análisis `delimitedText`. 

> [!NOTE]
> Siga las recomendaciones de configuración de indexadores de [Indexación de uno a varios](search-howto-index-one-to-many-blobs.md) para generar varios documentos de búsqueda a partir de un blob de Azure.

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
> Cuando se utiliza el modo de análisis de texto delimitado, Azure Cognitive Search supone que todos los blobs del origen de datos serán CSV. Si necesita compatibilidad con una combinación de blobs CSV y no CSV en el mismo origen de datos, vote por ella en [UserVoice](https://feedback.azure.com/forums/263029-azure-search).
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

## <a name="help-us-make-azure-cognitive-search-better"></a>Ayúdenos a mejorar Azure Cognitive Search
Si tiene solicitudes o ideas para mejorar las características, comuníquelas en [UserVoice](https://feedback.azure.com/forums/263029-azure-search/).

