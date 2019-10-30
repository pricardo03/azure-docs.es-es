---
title: Solución de problemas comunes del indizador de búsqueda
titleSuffix: Azure Cognitive Search
description: Corrija errores y problemas comunes con los indizadores en Azure Cognitive Search, como la conexión del origen de datos, el firewall y documentos que falten.
manager: nitinme
author: mgottein
ms.author: magottei
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: c5a16d957f1e0414f92d0cc03442d88d438e4c92
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2019
ms.locfileid: "72793627"
---
# <a name="troubleshooting-common-indexer-issues-in-azure-cognitive-search"></a>Solución de problemas comunes con el indizador en Azure Cognitive Search

Los indizadores pueden encontrar una serie de problemas al indexar datos en Azure Cognitive Search. Las categorías principales de error son las siguientes:

* [Conexión a un origen de datos](#data-source-connection-errors)
* [Procesamiento de documentos](#document-processing-errors)
* [Ingesta de documentos en un índice](#index-errors)

## <a name="data-source-connection-errors"></a>Errores de conexión del origen de datos

### <a name="blob-storage"></a>Blob Storage

#### <a name="storage-account-firewall"></a>Firewall de la cuenta de almacenamiento

Azure Storage ofrece un firewall configurable. De manera predeterminada, el firewall está deshabilitado para que Azure Cognitive Search se pueda conectar a la cuenta de almacenamiento.

No hay ningún mensaje de error específico cuando el firewall está habilitado. Por lo general, los errores de firewall se verán así: `The remote server returned an error: (403) Forbidden`.

Puede comprobar que el firewall está habilitado en el [portal](https://docs.microsoft.com/azure/storage/common/storage-network-security#azure-portal). La única solución alternativa admitida es deshabilitar el firewall al elegir permitir el acceso desde ["Todas las redes"](https://docs.microsoft.com/azure/storage/common/storage-network-security#azure-portal).

Si el indizador no tiene un conjunto de aptitudes adjunto _puede_ intentar [agregar una excepción](https://docs.microsoft.com/azure/storage/common/storage-network-security#managing-ip-network-rules) para las direcciones IP del servicio de búsqueda. Sin embargo, este escenario no es compatible y no se garantiza que funcione.

Puede averiguar la dirección IP del servicio de búsqueda haciendo ping al FQDN (`<your-search-service-name>.search.windows.net`).

### <a name="cosmos-db"></a>Cosmos DB

#### <a name="indexing-isnt-enabled"></a>El indexado no está habilitado

Azure Cognitive Search tiene una dependencia implícita del indexado de Cosmos DB. Si desactiva el indexado automático en Cosmos DB, Azure Cognitive Search devuelve un estado correcto, pero no puede indexar el contenido del contenedor. Para instrucciones sobre cómo establecer y activar el indexado, consulte [Administración automática en Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/how-to-manage-indexing-policy#use-the-azure-portal).

## <a name="document-processing-errors"></a>Errores en el procesamiento de documentos

### <a name="unprocessable-or-unsupported-documents"></a>Documentos no procesables o no compatibles

El indizador de blobs [documenta qué formatos de documento se admiten de manera explícita](search-howto-indexing-azure-blob-storage.md#SupportedFormats). A veces, un contenedor de almacenamiento de blobs contiene documentos no compatibles. En otras ocasiones, puede haber documentos problemáticos. Para evitar que el indizador se detenga en estos documentos, [cambie las opciones de configuración](search-howto-indexing-azure-blob-storage.md#DealingWithErrors):

```
PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2019-05-06
Content-Type: application/json
api-key: [admin key]

{
  ... other parts of indexer definition
  "parameters" : { "configuration" : { "failOnUnsupportedContentType" : false, "failOnUnprocessableDocument" : false } }
}
```

### <a name="missing-document-content"></a>Contenido de documento faltante

El indizador de blobs [busca y extrae texto de los blobs de un contenedor](search-howto-indexing-azure-blob-storage.md#how-azure-search-indexes-blobs). Algunos problemas con la extracción de texto son los siguientes:

* El documento solo contiene imágenes escaneadas. Los blobs de PDF que tienen contenido no textual, como imágenes escaneadas (JPG), no generan resultados en una canalización de indexación de blobs estándar. Si tiene contenido de imagen con elementos de texto, puede usar [Cognitive Search](cognitive-search-concept-image-scenarios.md) para buscar y extraer el texto.
* El indizador de blobs está configurado para indexar solo metadatos. Para extraer contenido, el indizador de blobs se debe configurar para [extraer tanto contenido como metadatos](search-howto-indexing-azure-blob-storage.md#controlling-which-parts-of-the-blob-are-indexed):

```
PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2019-05-06
Content-Type: application/json
api-key: [admin key]

{
  ... other parts of indexer definition
  "parameters" : { "configuration" : { "dataToExtract" : "contentAndMetadata" } }
}
```

## <a name="index-errors"></a>Errores de índice

### <a name="missing-documents"></a>Documentos faltantes

Los indizadores buscan documentos de un [origen de datos](https://docs.microsoft.com/rest/api/searchservice/create-data-source). En algunas ocasiones, un documento del origen de datos que se debería haber indexado aparece como faltante en un índice. Hay un par de razones comunes por las que pueden producirse estos errores:

* El documento no se ha indexado. Revise el portal para ver si el indizador se ejecutó correctamente.
* El documento se actualizó después de la ejecución del indizador. Si el indizador sigue una [programación](https://docs.microsoft.com/rest/api/searchservice/create-indexer#indexer-schedule), a la larga se volverá a ejecutar y recogerá el documento.
* La [consulta](https://docs.microsoft.com/rest/api/searchservice/create-data-source#request-body-syntax) especificada en el origen de datos excluye el documento. Los indizadores no pueden indexar documentos que no forman parte del origen de datos.
* [Asignaciones de campo](https://docs.microsoft.com/rest/api/searchservice/create-indexer#fieldmappings) o [enriquecimiento de inteligencia artificial](https://docs.microsoft.com/azure/search/cognitive-search-concept-intro) han cambiado el documento y tiene un aspecto distinto de lo esperado.
* Use la [API Buscar documento](https://docs.microsoft.com/rest/api/searchservice/lookup-document) para buscar el documento.
