---
title: 'Indexación de blobs JSON desde el indexador de blobs de Azure para la búsqueda de texto completo: Azure Search'
description: Rastree el contenido de texto de los blobs JSON de Azure mediante el indexador de blobs de Azure Search. Los indexadores automatizan la ingesta de datos para orígenes de datos seleccionados, como Azure Blob Storage.
ms.date: 12/21/2018
author: HeidiSteen
manager: cgronlun
ms.author: heidist
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.custom: seodec2018
ms.openlocfilehash: cafb48f28e38794ce0757d50a5d87432b237e17c
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2019
ms.locfileid: "54467170"
---
# <a name="indexing-json-blobs-with-azure-search-blob-indexer"></a>Indexación de blobs JSON con el indizador de blobs de Azure Search
En este artículo se muestra cómo configurar un indexador de blobs de Azure Search para extraer contenido estructurado de los blobs JSON en Azure Blob Storage.

Puede usar el [portal](#json-indexer-portal), las [API REST](#json-indexer-rest) o el [SDK de .NET](#json-indexer-dotnet) para indexar el contenido JSON. En todas las opciones anteriores, los documentos JSON se encuentran en un contenedor de blob en una cuenta de almacenamiento de Azure. Para obtener instrucciones sobre la inserción de documentos JSON desde otras plataformas que no sean de Azure, consulte [Importación de datos en Azure Search](search-what-is-data-import.md).

Los blobs JSON de Azure Blobs Storage suelen ser un documento JSON único o una matriz JSON. El indexador de blobs de Azure Search puede analizar cualquier construcción, según cómo configure el parámetro **parsingMode** en la solicitud.

> [!IMPORTANT]
> La indexación de blobs de JSON está disponible con carácter general, pero el análisis de JsonArray se encuentra en versión preliminar pública y no debería utilizarse en entornos de producción. Para obtener más información, consulte [REST api-version=2017-11-11-Preview](search-api-2017-11-11-preview.md). 

<a name="json-indexer-portal"></a>

## <a name="use-the-portal"></a>Uso del portal

El método más sencillo para la indexación de documentos JSON es usar un asistente en [Azure Portal](https://portal.azure.com/). Mediante el análisis de metadatos en el contenedor de blobs de Azure, el asistente [**Importar datos**](search-import-data-portal.md) puede crear un índice predeterminado, asignar campos de origen a campos de índice de destino y cargar el índice en una sola operación. Según el tamaño y la complejidad del origen de datos, puede tener un índice de búsqueda de texto completo y operativo en cuestión de minutos.

### <a name="1---prepare-source-data"></a>1: Preparación de los datos de origen

Debe tener una cuenta de almacenamiento de Azure, con Blob Storage y un contenedor de documentos JSON. Si no está familiarizado con alguna de estas tareas, revise los requisitos previos para "Configurar el servicio de Azure Blob y cargar los datos de ejemplo" en la [guía de inicio rápido de Cognitive Search](cognitive-search-quickstart-blob.md#set-up-azure-blob-service-and-load-sample-data).

### <a name="2---start-import-data-wizard"></a>2: Inicio del asistente para la importación de datos

También puede [iniciar el asistente](search-import-data-portal.md) desde la barra de comandos en la página del servicio Azure Search o hacer clic en **Agregar Azure Search** en la sección **Blob service** del panel de navegación izquierdo de la cuenta de almacenamiento.

### <a name="3---set-the-data-source"></a>3: Configuración del origen de datos

En la página **origen de datos**, el origen debe ser **Azure Blob Storage** con las especificaciones siguientes:

+ **Datos que se extraerán** debe seleccionar el valor *Contenido y metadatos*. Si elige esta opción, permitirá que el asistente deduzca un esquema de índice y asigne los campos para la importación.
   
+ El **Modo de análisis** debe establecerse en *JSON* o *matriz JSON*. 

  La opción *JSON* articula cada blob como un documento único de búsqueda, por lo que aparecen como un elemento independiente en los resultados de búsqueda. 

  La opción *Matriz JSON* es para los blobs compuestos de varios elementos en los que quiere que cada elemento se articule como un documento de búsqueda independiente. Si los blobs son complejos y no elige la opción *matriz JSON*, todo el blob se ingiere como un único documento.
   
+ El **Contenedor de almacenamiento** debe especificar su cuenta de almacenamiento y contenedor o una cadena de conexión que se resuelva en el contenedor. Puede obtener las cadenas de conexión en la página del portal de Blob service.

   ![Definición del origen de datos del blob](media/search-howto-index-json/import-wizard-json-data-source.png)

### <a name="4---skip-the-add-cognitive-search-page-in-the-wizard"></a>4: Omisión de la página "Agregar Cognitive Search" en el asistente

No es necesario agregar conocimientos cognitivos para la importación de documentos JSON. A menos que necesite específicamente [incluir Cognitive Services APIs y transformaciones](cognitive-search-concept-intro.md) a la canalización de indización, omita este paso.

Para omitir el paso, haga clic en la página siguiente **Personalizar índice de destino**.

### <a name="5---set-index-attributes"></a>5: Configuración de los atributos de índice

En la página de **Índice**, debería mostrarse una lista de campos con un tipo de datos y una serie de casillas de verificación para configurar los atributos del índice. El asistente puede generar un índice predeterminado en función de los metadatos y mediante el muestreo de los datos de origen. 

Los índices predeterminados a menudo generan una solución operativa: se selecciona un campo (que se convierte en una cadena) para que actúe como la clave o ID del documento para identificar de forma única a cada uno, así como los campos que son buenos candidatos para la búsqueda de texto completo y que se pueden recuperar en un conjunto de resultados. En el caso de los blobs, el campo `content` es el mejor candidato para el contenido utilizable en búsquedas.

Puede aceptar los valores predeterminados o revisar la descripción de los [atributos de índice](https://docs.microsoft.com/rest/api/searchservice/create-index#bkmk_indexAttrib) y [analizadores de lenguaje](https://docs.microsoft.com/rest/api/searchservice/language-support) para reemplazar o complementar los valores iniciales. 

Dedique un momento a la revisión de las selecciones. Una vez que se ejecuta al asistente, se crean las estructuras de datos físicas y no podrá modificar estos campos sin quitar y volver a crear todos los objetos.

   ![Definición de índice de blob](media/search-howto-index-json/import-wizard-json-index.png)

### <a name="6---create-indexer"></a>6: Creación del indizador

Con todas las especificaciones agregadas, el asistente crea tres objetos distintos en el servicio de búsqueda. Un objeto de origen de datos y un objeto de índice se guardan como recursos con nombre en el servicio Azure Search. El último paso crea un objeto de indizador. Al asignarle un nombre al indizador, este puede existir como un recurso independiente que se puede programar y administrar independientemente de los objetos de origen de datos y del de índice que se crearon en la misma secuencia del asistente.

Si no está familiarizado con los indizadores, un *indizador* es un recurso en Azure Search que rastrea un origen de datos externo en busca de contenido utilizable en búsquedas. La salida del asistente **Importar datos**  es un indizador que se rastrea el origen de datos JSON, extrae el contenido utilizable en búsquedas y lo importa a un índice en Azure Search.

   ![Definición del indizador del blob](media/search-howto-index-json/import-wizard-json-indexer.png)

Haga clic en **Aceptar** para ejecutar el asistente y crear todos los objetos. La indexación comienza inmediatamente.

Puede supervisar la importación de datos en las páginas del portal. Las notificaciones de progreso indican el estado de la indexación y cuántos documentos se cargan. Cuando se complete la indización, puede usar el [Explorador de búsqueda](search-explorer.md) para consultar el índice.

<a name="json-indexer-rest"></a>

## <a name="use-rest-apis"></a>Uso de API REST

La indexación de blobs JSON es similar a la extracción normal de documentos en un flujo de trabajo de tres partes común a todos los indizadores en Azure Search: se crea un origen de datos, se crea un índice y se crea un indizador.

Para la indexación de JSON basada en código, puede usar la API REST con las API para [indizadores](https://docs.microsoft.com/rest/api/searchservice/create-indexer), [orígenes de datos](https://docs.microsoft.com/rest/api/searchservice/create-data-source) e [índices](https://docs.microsoft.com/rest/api/searchservice/create-index). A diferencia del asistente del portal, un enfoque de código requiere que tenga una índice de forma local y listo para aceptar los documentos JSON cuando envíe la solicitud **Crear indizador**.

Los blobs JSON de Azure Blobs Storage suelen ser un documento JSON único o una matriz JSON. El indexador de blobs de Azure Search puede analizar cualquier construcción, según cómo configure el parámetro **parsingMode** en la solicitud.

| Documento JSON | parsingMode | DESCRIPCIÓN | Disponibilidad |
|--------------|-------------|--------------|--------------|
| Uno por blob | `json` | Analiza los blobs JSON como un único fragmento de texto. Cada blob JSON se convierte en un único documento de Azure Search. | Por lo general, está disponible en API de [REST](https://docs.microsoft.com/rest/api/searchservice/indexer-operations) y [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexer). |
| Varios por blob | `jsonArray` | Analiza una matriz JSON en el blob, donde cada elemento de la matriz se convierte en un documento de Azure Search independiente.  | Por lo general, está disponible en API de [REST](https://docs.microsoft.com/rest/api/searchservice/indexer-operations) y [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexer). |


### <a name="step-1-create-a-data-source"></a>Paso 1: Creación de un origen de datos

El primer paso es proporcionar la información de conexión de origen de datos utilizada por el indexador. El tipo de origen de datos, especificado aquí como `azureblob`, determina qué comportamientos de extracción de datos se invocan mediante el indexador. Para la indexación de blobs JSON, la definición del origen de datos es la misma para documentos y matrices JSON. 

    POST https://[service name].search.windows.net/datasources?api-version=2017-11-11
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-container", "query" : "optional, my-folder" }
    }   

### <a name="step-2-create-a-target-search-index"></a>Paso 2: Creación de un índice de búsqueda de destino 

Los indexadores se emparejan con un esquema de índice. Si utiliza la API (en lugar del portal), prepare un índice de antemano para poder especificarlo en la operación del indexador.

El índice almacena el contenido utilizable en búsquedas en Azure Search. Para crear un índice, proporcione un esquema que especifique los campos de un documento, los atributos y otras construcciones que conforman la experiencia de búsqueda. Si crea un índice que tenga los mismos nombres de campo y tipos de datos que el origen, el indizador hará coincidir los campos de origen y de destino, por lo que se ahorrará el trabajo asignar explícitamente los campos.

El ejemplo siguiente muestra una solicitud de [Creación de índice](https://docs.microsoft.com/rest/api/searchservice/create-index). El índice tendrá un campo `content` utilizable en búsquedas para almacenar el texto extraído de los blobs:   

    POST https://[service name].search.windows.net/indexes?api-version=2017-11-11
    Content-Type: application/json
    api-key: [admin key]

    {
          "name" : "my-target-index",
          "fields": [
            { "name": "id", "type": "Edm.String", "key": true, "searchable": false },
            { "name": "content", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false }
          ]
    }


### <a name="step-3-configure-and-run-the-indexer"></a>Paso 3: Configuración y ejecución del indexador

Hasta ahora, las definiciones para el origen de datos y el índice han sido independientes de parsingMode. Sin embargo, en el paso 3 de la configuración del indexador, la ruta de acceso difiere según cómo desee que se analice y estructure el contenido de los blobs JSON en un índice de Azure Search. Entre las opciones se incluyen `json` o `jsonArray`:

+ Establezca **parsingMode** en `json` para indexar cada blob como un solo documento.

+ Establezca **parsingMode** en `jsonArray` si los blobs están conformados por matrices JSON y necesita que cada elemento de la matriz se convierta en un documento independiente en Azure Search. Un documento se puede pensar como un solo elemento en los resultados de búsqueda. Si quiere que cada elemento de la matriz aparezca en los resultados de búsqueda como un elemento independiente, utilice la opción `jsonArray`.

En la definición del indizador, utilice las **asignaciones de campo** para elegir las propiedades del documento JSON de origen que se utilizarán para completar el índice de búsqueda de destino. En el caso de las matrices JSON, si la matriz existe como propiedad de nivel inferior, puede establecer una raíz del documento que indique dónde se coloca la matriz en el blob.

> [!IMPORTANT]
> Cuando se usa el modo de análisis de `json` o `jsonArray`, Azure Search da por hecho que todos los blobs en el origen de datos contienen JSON. Si necesita admitir una mezcla de blobs JSON y que no son JSON en el mismo origen de datos, háganoslo saber en [nuestro sitio UserVoice](https://feedback.azure.com/forums/263029-azure-search).


### <a name="how-to-parse-single-json-blobs"></a>Cómo analizar blobs JSON únicos

De forma predeterminada, el [indexador de blobs de Azure Search](search-howto-indexing-azure-blob-storage.md) analiza los blobs JSON como un único fragmento de texto. A menudo se desea conservar la estructura de los documentos de JSON. Por ejemplo, suponga que tiene el siguiente documento JSON en Azure Blob Storage:

    {
        "article" : {
            "text" : "A hopefully useful article explaining how to parse JSON blobs",
            "datePublished" : "2016-04-13",
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

El indizador de blobs analiza y convierte el documento JSON en un único documento de Azure Search. El indizador carga un índice al hacer que coincidan los campos "text", "datePublished" y "tags" del origen con los campos de destino con nombre y tipo idénticos.

La configuración se proporciona en el cuerpo de una operación del indexador. Recuerde que el objeto de origen de datos, definido previamente especifica la información de conexión y de tipo de origen de datos. Además, el índice de destino también debe existir como contenedor vacío en el servicio. La programación y los parámetros son opcionales, pero, si los omite, el indexador se ejecuta de inmediato, con `json` como modo de análisis.

Una solicitud totalmente especificada se vería similar a la siguiente:

    POST https://[service name].search.windows.net/indexers?api-version=2017-11-11
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-json-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "parameters" : { "configuration" : { "parsingMode" : "json" } }
    }

Como se indicó, las asignaciones de campos no son necesarias. Dado un índice con los campos "text", "datePublished" y "tags", el indexador de blobs puede inferir la asignación correcta sin que haya presente una asignación de campos en la solicitud.

### <a name="how-to-parse-json-arrays"></a>Cómo analizar matrices JSON

Como alternativa, puede optar por la característica de las matrices JSON. Esta funcionalidad es útil cuando los blobs contienen una *matriz de objetos JSON* y quiere que cada elemento se convierta en un documento de Azure Search independiente. Por ejemplo, dado el blob JSON siguiente, puede rellenar el índice de Azure Search con tres documentos independientes, cada uno de ellos con los campos "id" y "text".  

    [
        { "id" : "1", "text" : "example 1" },
        { "id" : "2", "text" : "example 2" },
        { "id" : "3", "text" : "example 3" }
    ]

Para una matriz JSON, la definición del indizador debe ser similar a la del ejemplo siguiente. Tenga en cuenta que el parámetro parsingMode especifica al analizador `jsonArray`. Especificar el analizador correcto y tener los datos de entrada correctos son los dos requisitos específicos para las matrices para indexar blobs JSON.

    POST https://[service name].search.windows.net/indexers?api-version=2017-11-11
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-json-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "parameters" : { "configuration" : { "parsingMode" : "jsonArray" } }
    }

Nuevamente, tenga en cuenta que las asignaciones de campos pueden omitirse. Dado un índice con campos "id" y "text" del mismo nombre, el indizador de blobs puede inferir la asignación correcta sin una lista de asignación de campos explícita.

<a name="nested-json-arrays"></a>

### <a name="nested-json-arrays"></a>Matrices JSON anidadas
¿Qué sucede si quiere indexar una matriz de objetos JSON, pero está anidada en alguna parte del documento? Puede elegir la propiedad que contiene la matriz mediante la propiedad de configuración `documentRoot` . Por ejemplo, si los blobs tienen el siguiente aspecto:

    {
        "level1" : {
            "level2" : [
                { "id" : "1", "text" : "Use the documentRoot property" },
                { "id" : "2", "text" : "to pluck the array you want to index" },
                { "id" : "3", "text" : "even if it's nested inside the document" }  
            ]
        }
    }

Utilice esta configuración para indexar la matriz de la propiedad `level2`:

    {
        "name" : "my-json-array-indexer",
        ... other indexer properties
        "parameters" : { "configuration" : { "parsingMode" : "jsonArray", "documentRoot" : "/level1/level2" } }
    }

### <a name="using-field-mappings-to-build-search-documents"></a>Uso de asignaciones de campo para crear documentos de búsqueda

Cuando los campos de origen y de destino no estén perfectamente alineados, puede definir una sección de asignación de campos en el cuerpo de la solicitud para crear asociaciones explícitas de campo a campo.

Actualmente, Azure Search no puede indexar documentos JSON arbitrarios directamente, ya que admite solo tipos de datos primitivos, matrices de cadenas y puntos de GeoJSON. Sin embargo, puede usar **asignaciones de campo** para seleccionar partes del documento JSON y los "elevan" a campos de nivel superior del documento de búsqueda. Para obtener información acerca de los conceptos básicos de las asignaciones de campos, consulte [Asignaciones de campos en los indexadores de Azure Search](search-indexer-field-mappings.md).

Volviendo al documento JSON de ejemplo:

    {
        "article" : {
            "text" : "A hopefully useful article explaining how to parse JSON blobs",
            "datePublished" : "2016-04-13"
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

Supongamos que tiene un índice de búsqueda con los siguientes campos: `text` del tipo `Edm.String`, `date` del tipo `Edm.DateTimeOffset` y `tags` del tipo `Collection(Edm.String)`. Tenga en cuenta la diferencia entre "datePublished" en el origen y el campo `date` en el índice. Para asignar JSON en la forma deseada, utilice las siguientes asignaciones de campo:

    "fieldMappings" : [
        { "sourceFieldName" : "/article/text", "targetFieldName" : "text" },
        { "sourceFieldName" : "/article/datePublished", "targetFieldName" : "date" },
        { "sourceFieldName" : "/article/tags", "targetFieldName" : "tags" }
      ]

Los nombres de campo de origen en las asignaciones se especifican con la notación [puntero JSON](https://tools.ietf.org/html/rfc6901) . Comience con una barra diagonal para hacer referencia a la raíz del documento JSON y, después, seleccione en la propiedad que desee (a un nivel arbitrario de anidamiento) mediante una ruta de acceso separada por una barra diagonal.

También puede hacer referencia a elementos individuales de la matriz mediante un índice de base cero. Por ejemplo, para elegir el primer elemento de la matriz "etiquetas" del ejemplo anterior, use una asignación de campo como esta:

    { "sourceFieldName" : "/article/tags/0", "targetFieldName" : "firstTag" }

> [!NOTE]
> Si un nombre de campo de origen en una ruta de acceso de asignación de campo hace referencia a una propiedad que no existe en JSON, la asignación se omite sin errores. Esto se realiza, por lo que podemos admitir documentos con un esquema diferente (que es un caso de uso frecuente). Puesto que no hay ninguna validación, tiene que procurar evitar tipográficos en la especificación de las asignaciones de campo.
>
>

### <a name="rest-example-indexer-request-with-field-mappings"></a>Ejemplo de REST: Solicitud del indexador con asignaciones de campos

El ejemplo siguiente es una carga de indexador completamente especificada, incluidas las asignaciones de campos:

    POST https://[service name].search.windows.net/indexers?api-version=2017-11-11
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-json-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "parameters" : { "configuration" : { "parsingMode" : "json" } },
      "fieldMappings" : [
        { "sourceFieldName" : "/article/text", "targetFieldName" : "text" },
        { "sourceFieldName" : "/article/datePublished", "targetFieldName" : "date" },
        { "sourceFieldName" : "/article/tags", "targetFieldName" : "tags" }
        ]
    }

<a name="json-indexer-dotnet"></a>

## <a name="use-net-sdk"></a>Uso del SDK de .NET

El SDK de .NET totalmente tiene paridad completa con la API REST. Se recomienda que revise la sección anterior de la API REST para obtener información sobre los conceptos, el flujo de trabajo y los requisitos. A continuación, puede consultar la siguiente documentación de referencia de la API de .NET para implementar un indizador JSON en código administrado.

+ [Microsoft.Azure.Search.Models.DataSource](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.datasource?view=azure-dotnet)
+ [Microsoft.azure.search.models.datasourcetype](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.datasourcetype?view=azure-dotnet) 
+ [Microsoft.azure.search.models.index](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.index?view=azure-dotnet) 
+ [Microsoft.azure.search.models.indexer](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexer?view=azure-dotnet)

## <a name="see-also"></a>Otras referencias

+ [Indexadores de Azure Search](search-indexer-overview.md)
+ [Indexación de Azure Blob Storage con Azure Search](search-howto-index-json-blobs.md)
+ [Indexación de blobs CSV con el indexador de blobs de Azure Search](search-howto-index-csv-blobs.md)
+ [Tutorial: Búsqueda de datos semiestructurados en Azure Blob Storage](search-semi-structured-data.md)
