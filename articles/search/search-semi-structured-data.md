---
title: Tutorial para buscar datos semiestructurados desde el almacenamiento en la nube de Azure en Azure Search | Microsoft Docs
description: En este tutorial, aprenderá a buscar datos semiestructurados de blobs de Azure con Azure Search.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.topic: tutorial
ms.date: 07/12/2018
ms.author: heidist
ms.openlocfilehash: a7b006bd8469ddce1415ab6cb7c52c0171ae11cd
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/13/2018
ms.locfileid: "39005084"
---
# <a name="tutorial-search-semi-structured-data-in-azure-cloud-storage"></a>Tutorial: Búsqueda de datos semiestructurados en el almacenamiento en la nube de Azure

En una serie de tutoriales de dos partes, aprenderá a buscar datos semiestructurados y no estructurados con Azure Search. La [Parte 1](../storage/blobs/storage-unstructured-search.md) le guiaba en la búsqueda a través de datos no estructurados, además de incluir requisitos previos importantes para este tutorial, como la creación de la cuenta de almacenamiento. 

En la Parte 2, en enfoque cambia a los datos semiestructurados, como JSON, almacenados en blobs de Azure. Los datos semiestructurados contienen etiquetas o marcas que separan el contenido dentro de los datos. Divide la diferencia entre los datos no estructurados que se deben indexar de manera holística y los datos estructurados formalmente que se ajustan a un modelo de datos, como un esquema de base de datos relacional, que se pueden rastrear por campo.

La Parte 2 incluye información sobre cómo:

> [!div class="checklist"]
> * Configurar un origen de datos de Azure Search para un contenedor de blobs de Azure.
> * Crear y rellenar un índice de Azure Search y un indexador para rastrear el contenedor y extraer el contenido utilizable en búsquedas.
> * Buscar el índice que acaba de crear.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

* Completar el [tutorial anterior](../storage/blobs/storage-unstructured-search.md) y proporcionar la cuenta de almacenamiento y el servicio de búsqueda creados en este.

* Instalar un cliente de REST y comprender cómo se crea una solicitud HTTP. Para este tutorial, estamos usando [Postman](https://www.getpostman.com/). No dude en usar otro cliente de REST si ya está familiarizado con uno determinado.

> [!NOTE]
> Este tutorial se basa en la compatibilidad de la matriz de JSON, que actualmente es una característica de versión preliminar de Azure Search. No está disponible en el portal. Por esta razón, estamos usando la versión preliminar de la API de REST, que proporciona esta característica, y una herramienta del cliente de REST para llamar a la API.

## <a name="set-up-postman"></a>Configuración de Postman

Inicie Postman y configure una solicitud HTTP. Si no está familiarizado con esta herramienta, consulte [Exploración de las API de REST de Azure Search mediante Fiddler o Postman](search-fiddler.md) para obtener más información.

El método de solicitud para todas las llamadas de este tutorial es "POST". Las claves de encabezado son "Content-type" y "api-key". Los valores de las claves de encabezado son, respectivamente, "application/json" y su "clave de administrador" (la clave de administrador es un marcador de posición para su clave principal de búsqueda). El cuerpo es donde se coloca el contenido real de la llamada. En función del cliente que esté usando, puede haber algunas variaciones en cómo se crea la consulta, pero estos son los conceptos básicos.

  ![Búsqueda de datos semiestructurados](media/search-semi-structured-data/postmanoverview.png)

Para las llamadas de REST que se tratan en este tutorial, se requiere la clave api-key de búsqueda. Puede encontrarla en **Claves** en el servicio de búsqueda. Esta clave api-key debe estar en el encabezado de todas las llamadas API (reemplace la "clave de administrador" en la captura de pantalla anterior por esta clave) que este tutorial le solicite realizar. Conserve la clave puesto que la necesitará para cada una de las llamadas.

  ![Búsqueda de datos semiestructurados](media/search-semi-structured-data/keys.png)

## <a name="download-the-sample-data"></a>Descargar los datos de ejemplo

Dispone de un conjunto de datos de ejemplo que se ha preparado para usted. **Descargue [clinical-trials-json.zip](https://github.com/Azure-Samples/storage-blob-integration-with-cdn-search-hdi/raw/master/clinical-trials-json.zip)**  y descomprímalo en su propia carpeta.

En esta muestra también se incluyen archivos JSON de ejemplo, que eran originalmente archivos de texto de [clinicaltrials.gov](https://clinicaltrials.gov/ct2/results). Los hemos convertido a JSON para su comodidad.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en el [Azure Portal](http://portal.azure.com).

## <a name="upload-the-sample-data"></a>Cargar los datos de ejemplo

En Azure Portal, vaya a la cuenta de almacenamiento que creó en el [tutorial anterior](../storage/blobs/storage-unstructured-search.md). A continuación, abra el contenedor de **datos** y haga clic en **Cargar**.

Haga clic en **Avanzado**, escriba "clinical-trials-json" y, después, cargue todos los archivos de JSON que descargó.

  ![Búsqueda de datos semiestructurados](media/search-semi-structured-data/clinicalupload.png)

Una vez finalizada la carga, los archivos deberían aparecer en su propia subcarpeta dentro del contenedor de datos.

## <a name="connect-your-search-service-to-your-container"></a>Conectar el servicio de búsqueda al contenedor

Estamos usando Postman para realizar tres llamadas API al servicio de búsqueda con el fin de crear un origen de datos, un índice y un indexador. El origen de datos incluye un puntero a la cuenta de almacenamiento y a los datos JSON. El servicio de búsqueda realiza la conexión al cargar los datos.

La cadena de consulta debe contener **api-version=2016-09-01-Preview** y cada llamada debe devolver **201 Created**. El parámetro api-version normalmente disponible aún no tiene la capacidad de administrar JSON como jsonArray, actualmente esto solo lo hace la versión preliminar del parámetro api-version.

Ejecute las siguientes tres llamadas API desde el cliente de REST.

### <a name="create-a-datasource"></a>Crear un origen de datos

Un origen de datos especifica qué datos se deben indexar.

El punto de conexión de esta llamada es `https://[service name].search.windows.net/datasources?api-version=2016-09-01-Preview`. Reemplace `[service name]` por el nombre del servicio de búsqueda.

Para esta llamada, necesita el nombre de la cuenta de almacenamiento y la clave de la cuenta de almacenamiento. Encontrará la clave de la cuenta de almacenamiento en Azure Portal, en la sección **Access Keys** (Claves de acceso) de la cuenta de almacenamiento. La ubicación se muestra en la imagen siguiente:

  ![Búsqueda de datos semiestructurados](media/search-semi-structured-data/storagekeys.png)

Asegúrese de reemplazar `[storage account name]` y `[storage account key]` en el cuerpo de la llamada antes de ejecutarla.

```json
{
    "name" : "clinical-trials-json",
    "type" : "azureblob",
    "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=[storage account name];AccountKey=[storage account key];" },
    "container" : { "name" : "data", "query" : "clinical-trials-json" }
}
```

La respuesta debería tener este aspecto:

```json
{
    "@odata.context": "https://exampleurl.search.windows.net/$metadata#datasources/$entity",
    "@odata.etag": "\"0x8D505FBC3856C9E\"",
    "name": "clinical-trials-json",
    "description": null,
    "type": "azureblob",
    "subtype": null,
    "credentials": {
        "connectionString": "DefaultEndpointsProtocol=https;AccountName=[mystorageaccounthere];AccountKey=[[myaccountkeyhere]]];"
    },
    "container": {
        "name": "data",
        "query": "clinical-trials-json"
    },
    "dataChangeDetectionPolicy": null,
    "dataDeletionDetectionPolicy": null
}
```

### <a name="create-an-index"></a>Creación de un índice
    
La segunda llamada API crea un índice. Un índice especifica todos los parámetros y sus atributos.

La dirección URL de esta llamada es `https://[service name].search.windows.net/indexes?api-version=2016-09-01-Preview`. Reemplace `[service name]` por el nombre del servicio de búsqueda.

Primero reemplace la dirección URL. A continuación, copie y pegue el código siguiente en el cuerpo y ejecute la consulta.

```json
{
  "name": "clinical-trials-json-index",  
  "fields": [
  {"name": "FileName", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": false, "filterable": false, "sortable": true},
  {"name": "Description", "type": "Edm.String", "searchable": true, "retrievable": false, "facetable": false, "filterable": false, "sortable": false},
  {"name": "MinimumAge", "type": "Edm.Int32", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": true},
  {"name": "Title", "type": "Edm.String", "searchable": true, "retrievable": true, "facetable": false, "filterable": true, "sortable": true},
  {"name": "URL", "type": "Edm.String", "searchable": false, "retrievable": false, "facetable": false, "filterable": false, "sortable": false},
  {"name": "MyURL", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": false, "filterable": false, "sortable": false},
  {"name": "Gender", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": false},
  {"name": "MaximumAge", "type": "Edm.Int32", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": true},
  {"name": "Summary", "type": "Edm.String", "searchable": true, "retrievable": true, "facetable": false, "filterable": false, "sortable": false},
  {"name": "NCTID", "type": "Edm.String", "key": true, "searchable": true, "retrievable": true, "facetable": false, "filterable": true, "sortable": true},
  {"name": "Phase", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": false},
  {"name": "Date", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": false, "filterable": false, "sortable": true},
  {"name": "OverallStatus", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": false},
  {"name": "OrgStudyId", "type": "Edm.String", "searchable": true, "retrievable": true, "facetable": false, "filterable": true, "sortable": false},
  {"name": "HealthyVolunteers", "type": "Edm.String", "searchable": false, "retrievable": true, "facetable": true, "filterable": true, "sortable": false},
  {"name": "Keywords", "type": "Collection(Edm.String)", "searchable": true, "retrievable": true, "facetable": true, "filterable": false, "sortable": false},
  {"name": "metadata_storage_last_modified", "type":"Edm.DateTimeOffset", "searchable": false, "retrievable": true, "filterable": true, "sortable": false},
  {"name": "metadata_storage_size", "type":"Edm.String", "searchable": false, "retrievable": true, "filterable": true, "sortable": false},
  {"name": "metadata_content_type", "type":"Edm.String", "searchable": true, "retrievable": true, "filterable": true, "sortable": false}
  ],
  "suggesters": [
  {
    "name": "sg",
    "searchMode": "analyzingInfixMatching",
    "sourceFields": ["Title"]
  }
  ]
}
```

La respuesta debería tener este aspecto:

```json
{
    "@odata.context": "https://exampleurl.search.windows.net/$metadata#indexes/$entity",
    "@odata.etag": "\"0x8D505FC00EDD5FA\"",
    "name": "clinical-trials-json-index",
    "fields": [
        {
            "name": "FileName",
            "type": "Edm.String",
            "searchable": false,
            "filterable": false,
            "retrievable": true,
            "sortable": true,
            "facetable": false,
            "key": false,
            "indexAnalyzer": null,
            "searchAnalyzer": null,
            "analyzer": null,
            "synonymMaps": []
        },
        {
            "name": "Description",
            "type": "Edm.String",
            "searchable": true,
            "filterable": false,
            "retrievable": false,
            "sortable": false,
            "facetable": false,
            "key": false,
            "indexAnalyzer": null,
            "searchAnalyzer": null,
            "analyzer": null,
            "synonymMaps": []
        },
        ...
          "scoringProfiles": [],
    "defaultScoringProfile": null,
    "corsOptions": null,
    "suggesters": [],
    "analyzers": [],
    "tokenizers": [],
    "tokenFilters": [],
    "charFilters": []
}
```

### <a name="create-an-indexer"></a>Crear un indexador

Un indexador conecta el origen de datos a un índice de búsqueda de destino y proporciona opcionalmente una programación para automatizar la actualización de datos.

La dirección URL de esta llamada es `https://[service name].search.windows.net/indexers?api-version=2016-09-01-Preview`. Reemplace `[service name]` por el nombre del servicio de búsqueda.

Primero reemplace la dirección URL. A continuación, copie y pegue el código siguiente en el cuerpo y ejecute la consulta.

```json
{
  "name" : "clinical-trials-json-indexer",
  "dataSourceName" : "clinical-trials-json",
  "targetIndexName" : "clinical-trials-json-index",
  "parameters" : { "configuration" : { "parsingMode" : "jsonArray" } }
}
```

La respuesta debería tener este aspecto:

```json
{
    "@odata.context": "https://exampleurl.search.windows.net/$metadata#indexers/$entity",
    "@odata.etag": "\"0x8D505FDE143D164\"",
    "name": "clinical-trials-json-indexer",
    "description": null,
    "dataSourceName": "clinical-trials-json",
    "targetIndexName": "clinical-trials-json-index",
    "schedule": null,
    "parameters": {
        "batchSize": null,
        "maxFailedItems": null,
        "maxFailedItemsPerBatch": null,
        "base64EncodeKeys": null,
        "configuration": {
            "parsingMode": "jsonArray"
        }
    },
    "fieldMappings": [],
    "enrichers": [],
    "disabled": null
}
```

## <a name="search-your-json-files"></a>Buscar los archivos JSON

Ahora que conectó el servicio de búsqueda al contenedor de datos, puede empezar a buscar en los archivos.

Abra Azure Portal y vuelva al servicio de búsqueda. Tal y como hizo en el tutorial anterior.

  ![Búsqueda de datos no estructurados](media/search-semi-structured-data/indexespane.png)

### <a name="user-defined-metadata-search"></a>Búsqueda de metadatos definidos por el usuario

Como antes, los datos pueden consultarse de diferentes maneras: búsqueda de texto completo, propiedades del sistema o metadatos definidos por el usuario. Las propiedades del sistema y los metadatos definidos por el usuario solo pueden buscarse con el parámetro `$select` si se marcaron como **recuperables** durante la creación del índice de destino. Los parámetros del índice no pueden modificarse una vez creados. Sin embargo, se pueden agregar parámetros adicionales.

Un ejemplo de una consulta básica es `$select=Gender,metadata_storage_size`, que limita la devolución a estos dos parámetros.

  ![Búsqueda de datos semiestructurados](media/search-semi-structured-data/lastquery.png)

Un ejemplo de consulta más compleja sería `$filter=MinimumAge ge 30 and MaximumAge lt 75`, que devuelve solo resultados en los que el parámetro MinimumAge es mayor o igual a 30 y el parámetro MaximumAge es menor que 75.

  ![Búsqueda de datos semiestructurados](media/search-semi-structured-data/metadatashort.png)

Si quiere, puede experimentar y probar más consultas usted mismo. Tenga en cuenta que puede usar operadores lógicos (and, or, not) y operadores de comparación (eq, ne, gt, lt, ge, le). La comparación de cadenas distingue mayúsculas de minúsculas.

El parámetro `$filter` solo funciona con los metadatos que se marcaron como filtrables al crear el índice.

## <a name="clean-up-resources"></a>Limpieza de recursos

La manera más rápida de borrar el contenido después de un tutorial es eliminar el grupo de recursos que contiene el servicio Azure Search. Para eliminar de forma definitiva todo lo que contenga el grupo de recursos, elimine el grupo. En el portal, el nombre del grupo de recursos está en la página Información general de cada servicio Azure Search.

## <a name="next-steps"></a>Pasos siguientes

Puede asociar algoritmos con inteligencia artificial a una canalización del indexador. Como paso siguiente, continúe con el siguiente tutorial.

> [!div class="nextstepaction"]
> [Indexación de documentos en Azure Blob Storage](search-howto-indexing-azure-blob-storage.md)