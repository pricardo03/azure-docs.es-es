---
title: 'Introducción a Knowledge Store (versión preliminar): Azure Search'
description: Aprenda los pasos para enviar documentos enriquecidos creados por canalizaciones de indexación de IA en Azure Search a un almacén de conocimiento en la cuenta de almacenamiento de Azure. Desde allí, puede ver, cambiar la forma y consumir documentos enriquecidos en Azure Search y en otras aplicaciones.
manager: cgronlun
author: HeidiSteen
services: search
ms.service: search
ms.topic: quickstart
ms.date: 05/02/2019
ms.author: heidist
ms.openlocfilehash: 2a904cfb049af413887798c8aab449561bc2b73f
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65030053"
---
# <a name="how-to-get-started-with-knowledge-store"></a>Introducción a Knowledge Store

[Knowledge Store](knowledge-store-concept-intro.md) es una nueva característica en vista previa de Azure Search que guarda enriquecimientos de IA creados en una canalización de indexación para minería de datos de conocimientos en otras aplicaciones. También puede usar enriquecimientos guardados para comprender y refinar una canalización de indexación de Azure Search.

Un almacén de conocimientos se define mediante un conjunto de aptitudes. Para escenarios normales de búsqueda de texto completo de Azure Search, el propósito de un conjunto de aptitudes es ofrecer enriquecimientos de IA para facilitar la búsqueda de contenido. Para los escenarios de un almacén de conocimientos, el rol de un conjunto de aptitudes es crear y rellenar varias estructuras de datos para minería de conocimientos.

En este ejercicio, comience con datos, servicios y herramientas de ejemplo para conocer el flujo de trabajo básico para crear y usar su primer almacén de conocimientos, con énfasis en la definición del conjunto de aptitudes.

## <a name="prerequisites"></a>Requisitos previos

En este inicio rápido se usan los siguientes servicios, herramientas y datos. 

+ [Cree un servicio Azure Search](search-create-service-portal.md) o [busque un servicio existente](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) en su suscripción actual. Puede usar un servicio gratuito para este tutorial. 

+ [Cree una cuenta de Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account) para almacenar los datos de ejemplo. El almacén de conocimientos existirá en Azure Storage.

+ [Cree un recurso de Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) en el nivel de servicio de pago por uso S0 para un acceso exhaustivo a la gama completa de aptitudes usadas en los enriquecimientos de IA.

+ La [aplicación de escritorio Postman](https://www.getpostman.com/) se usa para enviar solicitudes a Azure Search.

+ [Colección de Postman](https://github.com/Azure-Samples/azure-search-postman-samples/tree/master/caselaw) con solicitudes preparadas para crear un origen de datos, un índice, un conjunto de aptitudes y un indexador. Varias definiciones de objeto son demasiado largas para incluirlas en este artículo. Debe obtener esta colección para ver las definiciones de índices y conjuntos de aptitudes en su totalidad.

+ [Datos de ejemplo de Caselaw](https://github.com/Azure-Samples/azure-search-sample-data/tree/master/caselaw) que se originan en la página de descarga de datos públicos masivos de [Caselaw Access Project](https://case.law/bulk/download/). En concreto, el ejercicio usa los 10 primeros documentos de la primera descarga (Arkansas). Para este ejercicio, hemos cargado un ejemplo de 10 documentos en GitHub.

## <a name="get-a-key-and-url"></a>Obtención de una clave y una dirección URL

Las llamadas de REST requieren la dirección URL del servicio y una clave de acceso en cada solicitud. Con ambos se crea un servicio de búsqueda, por lo que si ha agregado Azure Search a su suscripción, siga estos pasos para obtener la información necesaria:

1. [Inicie sesión en Azure Portal](https://portal.azure.com/) y en la página **Introducción** del servicio de búsqueda, obtenga la dirección URL. Un punto de conexión de ejemplo podría ser similar a `https://mydemo.search.windows.net`.

1. En **Configuración** > **Claves**, obtenga una clave de administrador para tener derechos completos en el servicio. Se proporcionan dos claves de administrador intercambiables para lograr la continuidad empresarial, por si necesitara sustituir una de ellas. Puede usar la clave principal o secundaria en las solicitudes para agregar, modificar y eliminar objetos.

    ![Obtención de una clave de acceso y un punto de conexión HTTP](media/search-fiddler/get-url-key.png "Get an HTTP endpoint and access key")

Todas las solicitudes requieren una clave de API en cada solicitud enviada al servicio.

## <a name="prepare-sample-data"></a>Preparación de datos de ejemplo

1. [Inicie sesión en Azure Portal](https://portal.azure.com), vaya a su cuenta de Azure Storage, haga clic en **Blobs** y, después, en **+Contenedor**.

1. [Cree un contenedor de blobs](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal) que contenga los datos de ejemplo. Puede establecer el nivel de acceso público a cualquiera de sus valores válidos.

1. Una vez creado el contenedor, ábralo y seleccione **Cargar** en la barra de comandos.

   ![Cargar en la barra de comandos](media/search-semi-structured-data/upload-command-bar.png "Upload on command bar")

1. Vaya a la carpeta que contiene el archivo de ejemplo **caselaw-sample.json**. Seleccione el archivo y, luego, haga clic en **Cargar**.


## <a name="set-up-postman"></a>Configuración de Postman

Inicie Postman y configure una solicitud HTTP. Si no está familiarizado con esta herramienta, consulte [Exploración de las API REST de Azure Search mediante Postman](search-fiddler.md).

+ El método de solicitud de todas las llamadas en esta guía es **POST**.
+ Los encabezados de solicitud (2) incluyen los siguientes: "Content-type" establecido en "application/json", "api-key" establecido en la "clave de administrador" (la clave de administrador es un marcador de posición para su clave principal de búsqueda), respectivamente. 
+ El cuerpo de la solicitud es donde se coloca el contenido real de la llamada. 

  ![Búsqueda de datos semiestructurados](media/search-semi-structured-data/postmanoverview.png)

Estamos usando Postman para realizar cuatro llamadas API al servicio de búsqueda con el fin de crear un origen de datos, un índice, conjunto de aptitudes y un indexador. El origen de datos incluye un puntero a la cuenta de almacenamiento y a los datos JSON. El servicio de búsqueda realiza la conexión al importar los datos.

[Crear un conjunto de aptitudes](#create-skillset) es el objetivo de esta guía: especifica los pasos de enriquecimiento y cómo persisten los datos en un almacén de conocimientos.

El punto de conexión de dirección URL debe especificar api-version. Además, cada llamada debe devolver el mensaje **"201 - Creado"**. La versión preliminar de api-version para crear un conjunto de aptitudes con compatibilidad del almacén de conocimientos es `2019-05-06-Preview`.

Ejecute las siguientes llamadas API desde el cliente de REST.

## <a name="create-a-data-source"></a>Creación de un origen de datos

[Create Data Source API](https://docs.microsoft.com/rest/api/searchservice/create-data-source) crea un objeto de Azure Search que especifica qué datos se indexan.

El punto de conexión de esta llamada es `https://[service name].search.windows.net/datasources?api-version=2019-05-06-Preview`. 

1. Reemplace `[service name]` por el nombre del servicio de búsqueda. 

2. Para esta llamada, el cuerpo de solicitud debe incluir la cadena de conexión de la cuenta de almacenamiento y el nombre del contenedor de blobs. Encontrará la conexión en Azure Portal, en la sección **Claves de acceso** de la cuenta de almacenamiento. 

   Asegúrese de reemplazar la cadena de conexión y el nombre del contenedor de blobs en el cuerpo de la solicitud antes de ejecutar la llamada.

    ```json
    {
        "name": "caselaw-ds",
        "description": null,
        "type": "azureblob",
        "subtype": null,
        "credentials": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<your storage account>;AccountKey=<your storage key>;EndpointSuffix=core.windows.net"
        },
        "container": {
            "name": "<your blob container name>",
            "query": null
        },
        "dataChangeDetectionPolicy": null,
        "dataDeletionDetectionPolicy": null
    }
    ```

3. Envíe la solicitud. La respuesta debe ser **201**, y el cuerpo de la respuesta debe ser casi idéntico a la carga de la solicitud proporcionada.

    ```json
    {
        "name": "caselaw-ds",
        "description": null,
        "type": "azureblob",
        "subtype": null,
        "credentials": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<your storage account>;AccountKey=<your storage key>;EndpointSuffix=core.windows.net"
        },
        "container": {
            "name": "<your blob container name>",
            "query": null
        },
        "dataChangeDetectionPolicy": null,
        "dataDeletionDetectionPolicy": null
    }
    ```

## <a name="create-an-index"></a>Creación de un índice
    
La segunda llamada es [Create Index API](https://docs.microsoft.com/rest/api/searchservice/create-data-source), que crea un índice de Azure Search que almacena todos los datos en los que se pueden realizar búsquedas. Un índice especifica todos los campos, parámetros y atributos.

No es necesario un índice para la minería de conocimientos, pero un indexador no se ejecutará a menos que se proporcione un índice. 

La dirección URL de esta llamada es `https://[service name].search.windows.net/indexes?api-version=2019-05-06-Preview`.

1. Reemplace `[service name]` por el nombre del servicio de búsqueda.

2. Copie la definición del índice desde la solicitud Create Index en la colección de Postman hacia el cuerpo de solicitud. La definición del índice tiene varios cientos de líneas, es demasiado larga para imprimirla aquí. 

   El shell exterior de un índice consta de los siguientes elementos. 

   ```json
   {
      "name": "caselaw",
      "defaultScoringProfile": null,
      "fields": [],
      "scoringProfiles": [],
      "corsOptions": null,
      "suggesters": [],
      "analyzers": [],
      "tokenizers": [],
      "tokenFilters": [],
      "charFilters": [],
      "encryptionKey": null
   }
   ```

3. La colección `fields` contiene la mayor parte de la definición del índice. Incluye campos simples, [campos complejos](search-howto-complex-data-types.md) con subestructuras anidadas, y colecciones.

   Revise la definición del campo para `casebody` en las líneas 302 a 384. Tenga en cuenta que un campo complejo puede contener otros campos complejos cuando se necesitan representaciones jerárquicas.

   ```json
   {
    "name": "casebody",
    "type": "Edm.ComplexType",
    "fields": [
        {
            "name": "status",
            "type": "Edm.String",
            "searchable": true,
            "filterable": true,
            "retrievable": true,
            "sortable": true,
            "facetable": true,
            "key": false,
            "indexAnalyzer": null,
            "searchAnalyzer": null,
            "analyzer": null,
            "synonymMaps": []
        },
        {
            "name": "data",
            "type": "Edm.ComplexType",
            "fields": [
                {
                    "name": "head_matter",
                    "type": "Edm.String",
                    "searchable": true,
                    "filterable": false,
                    "retrievable": true,
                    "sortable": false,
                    "facetable": false,
                    "key": false,
                    "indexAnalyzer": null,
                    "searchAnalyzer": null,
                    "analyzer": null,
                    "synonymMaps": []
                },
                {
                    "name": "opinions",
                    "type": "Collection(Edm.ComplexType)",
                    "fields": [
                        {
                            "name": "author",
                            "type": "Edm.String",
                            "searchable": true,
                            "filterable": true,
                            "retrievable": true,
                            "sortable": false,
                            "facetable": true,
                            "key": false,
                            "indexAnalyzer": null,
                            "searchAnalyzer": null,
                            "analyzer": null,
                            "synonymMaps": []
                        },
                        {
                            "name": "text",
                            "type": "Edm.String",
                            "searchable": true,
                            "filterable": false,
                            "retrievable": true,
                            "sortable": false,
                            "facetable": false,
                            "key": false,
                            "indexAnalyzer": null,
                            "searchAnalyzer": null,
                            "analyzer": null,
                            "synonymMaps": []
                        },
                        {
                            "name": "type",
                            "type": "Edm.String",
                            "searchable": true,
                            "filterable": true,
                            "retrievable": true,
                            "sortable": false,
                            "facetable": true,
                            "key": false,
                            "indexAnalyzer": null,
                            "searchAnalyzer": null,
                            "analyzer": null,
                            "synonymMaps": []
                        }
                    ]
                },
    . . .
   ```

4. Envíe la solicitud. 

   La respuesta debe ser **201** y tener un aspecto similar al ejemplo siguiente; se muestran los primeros campos:

    ```json
    {
        "name": "caselaw",
        "defaultScoringProfile": null,
        "fields": [
            {
                "name": "id",
                "type": "Edm.String",
                "searchable": true,
                "filterable": true,
                "retrievable": true,
                "sortable": true,
                "facetable": true,
                "key": true,
                "indexAnalyzer": null,
                "searchAnalyzer": null,
                "analyzer": null,
                "synonymMaps": []
            },
            {
                "name": "name",
                "type": "Edm.String",
                "searchable": true,
                "filterable": true,
                "retrievable": true,
                "sortable": true,
                "facetable": true,
                "key": false,
                "indexAnalyzer": null,
                "searchAnalyzer": null,
                "analyzer": null,
                "synonymMaps": []
            },
      . . .
    ```

<a name="create-skillset"></a>

## <a name="create-a-skillset-and-knowledge-store"></a>Crear un conjunto de aptitudes y un almacén de conocimientos

[Create Skillset API](https://docs.microsoft.com/rest/api/searchservice/create-skillset) crea un objeto de Azure Search que especifica a qué aptitudes cognitivas se llamará, cómo encadenar aptitudes y, lo más importante para esta guía, cómo especificar un almacén de conocimientos.

El punto de conexión de esta llamada es `https://[service name].search.windows.net/skillsets?api-version=2019-05-06-Preview`.

1. Reemplace `[service name]` por el nombre del servicio de búsqueda.

2. Copie la definición del conjunto de aptitudes desde la solicitud Create Skillset en la colección de Postman hacia el cuerpo de solicitud. La definición del conjunto de aptitudes tiene varios cientos de líneas; es demasiado larga para imprimirla aquí, pero esta guía se basa en ella.

   El shell exterior de un conjunto de aptitudes consta de los siguientes elementos. La colección `skills` define los enriquecimientos en memoria, pero la definición `knowledgeStore` especifica cómo se almacena la salida. La definición `cognitiveServices` es la conexión a los motores de enriquecimiento de IA.

   ```json
   {
    "name": "caselaw-ss",
    "description": null,
    "skills": [],
    "cognitiveServices": [],
    "knowledgeStore": []
   }
   ```

3. En primer lugar, establezca la clave y la cadena de conexión `cognitiveServices` y `knowledgeStore`. En el ejemplo, estas cadenas se encuentran después de la definición del conjunto de aptitudes, hacia el final del cuerpo de la solicitud.

    ```json
    "cognitiveServices": {
        "@odata.type": "#Microsoft.Azure.Search.CognitiveServicesByKey",
        "description": "<your cognitive services resource name>",
        "key": "<your cognitive services key>"
    },
    "knowledgeStore": {
        "storageConnectionString": "DefaultEndpointsProtocol=https;AccountName=<your storage account name>;AccountKey=<your storage account key>;EndpointSuffix=core.windows.net",
    ```

3. Revise la colección de aptitudes, en particular las aptitudes de Conformador en las líneas 85 y 170, respectivamente. La aptitud Conformador es importante porque reúne las estructuras de datos que quiere para minería de conocimientos. Durante la ejecución del conjunto de aptitudes, estas estructuras solo están en memoria, pero conforme avanza al siguiente paso, verá cómo se puede guardar esta salida en un almacén de conocimientos para una exploración más a fondo.

   Es el siguiente fragmento de código proviene de la línea 207. 

    ```json
    {
    "name": "Opinions",
    "source": null,
    "sourceContext": "/document/casebody/data/opinions/*",
    "inputs": [
        {
            "name": "Text",
            "source": "/document/casebody/data/opinions/*/text"
        },
        {
            "name": "Author",
            "source": "/document/casebody/data/opinions/*/author"
        },
        {
            "name": "Entities",
            "source": null,
            "sourceContext": "/document/casebody/data/opinions/*/text/pages/*/entities/*",
            "inputs": [
                {
                    "name": "Entity",
                    "source": "/document/casebody/data/opinions/*/text/pages/*/entities/*/value"
                },
                {
                    "name": "EntityType",
                    "source": "/document/casebody/data/opinions/*/text/pages/*/entities/*/category"
                }
             ]
          }
     ]
   }
   . . .
   ```

3. Revise el elemento `projections` en `knowledgeStore`, a partir de la línea 253. Las proyecciones especifican la composición del almacén de conocimientos. Las proyecciones se especifican en pares de tablas-objetos, pero actualmente solo una a la vez. Como puede ver en la primera proyección, se especifica `tables`, pero no `objects`. En la segunda, sucede lo contrario.

   En Azure Storage, las tablas se crearán en el almacenamiento de tablas para cada tabla que cree, y cada objeto obtiene un contenedor en el almacenamiento de blobs.

   Normalmente, los objetos contienen la expresión completa de un enriquecimiento. Normalmente, las tablas contienen enriquecimientos parciales, en combinaciones que usted organiza para fines específicos. En este ejemplo, se muestra una tabla Cases, pero no se muestran otras tablas como Entities, Judges ni Opinions.

    ```json
    "projections": [
    {
        "tables": [
            {
              "tableName": "Opinions",
              "generatedKeyName": "OpinionId",
              "source": "/document/Case/OpinionsSnippets/*"
            },
          . . . 
        ],
        "objects": []
    },
    {
        "tables": [],
        "objects": [
            {
                "storageContainer": "enrichedcases",
                "key": "/document/CaseFull/Id",
                "source": "/document/CaseFull"
            }
          ]
        }
      ]
    }
    ```

5. Envíe la solicitud. La respuesta debe ser **201** y tener un aspecto similar al ejemplo siguiente; se muestran la primera parte de la respuesta.

    ```json
    {
    "name": "caselaw-ss",
    "description": null,
    "skills": [
        {
            "@odata.type": "#Microsoft.Skills.Text.SplitSkill",
            "name": "SplitSkill#1",
            "description": null,
            "context": "/document/casebody/data/opinions/*/text",
            "defaultLanguageCode": "en",
            "textSplitMode": "pages",
            "maximumPageLength": 5000,
            "inputs": [
                {
                    "name": "text",
                    "source": "/document/casebody/data/opinions/*/text
                }
            ],
            "outputs": [
                {
                    "name": "textItems",
                    "targetName": "pages"
                }
            ]
        },
        . . .
    ```

## <a name="create-and-run-an-indexer"></a>Creación y ejecución de un indexador

[Create Indexer API](https://docs.microsoft.com/rest/api/searchservice/create-indexer) crea y ejecuta de inmediato un indexador. Todas las definiciones que ha creado hasta ahora comienzan a moverse con este paso. El indexador se ejecuta de inmediato porque no existe en el servicio. Después de existir, una llamada POST a un indexador existente es una operación de actualización.

El punto de conexión de esta llamada es `https://[service name].search.windows.net/indexers?api-version=2019-05-06-Preview`.

1. Reemplace `[service name]` por el nombre del servicio de búsqueda. 

2. Para esta llamada, el cuerpo de la solicitud especifica el nombre del indexador. El indexador requiere el origen de datos y el índice. Un conjunto de aptitudes es opcional para un indexador, pero es obligatorio para el enriquecimiento de IA.

    ```json
    {
        "name": "caselaw-idxr",
        "description": null,
        "dataSourceName": "caselaw-ds",
        "skillsetName": "caselaw-ss",
        "targetIndexName": "caselaw",
        "disabled": null,
        "schedule": null,
        "parameters": {
            "batchSize": 1,
            "maxFailedItems": null,
            "maxFailedItemsPerBatch": null,
            "base64EncodeKeys": null,
            "configuration": {
                "parsingMode": "jsonLines"
            }
        },
        "fieldMappings": [],
        "outputFieldMappings": [
            {
                "sourceFieldName": "/document/casebody/data/opinions/*/text/pages/*/people/*",
                "targetFieldName": "people",
                "mappingFunction": null
            },
            {
                "sourceFieldName": "/document/casebody/data/opinions/*/text/pages/*/organizations/*",
                "targetFieldName": "orginizations",
                "mappingFunction": null
            },
            {
                "sourceFieldName": "/document/casebody/data/opinions/*/text/pages/*/locations/*",
                "targetFieldName": "locations",
                "mappingFunction": null
            },
            {
                "sourceFieldName": "/document/Case/OpinionsSnippets/*/Entities/*",
                "targetFieldName": "entities",
                "mappingFunction": null
            },
            {
                "sourceFieldName": "/document/casebody/data/opinions/*/text/pages/*/keyPhrases/*",
                "targetFieldName": "keyPhrases",
                "mappingFunction": null
            }
        ]
    }
    ```

3. Envíe la solicitud. La respuesta debe ser **201**, y el cuerpo de la respuesta debe ser casi idéntico a la carga de la solicitud proporcionada (recortada para abreviar).

    ```json
    {
        "name": "caselaw-idxr",
        "description": null,
        "dataSourceName": "caselaw-ds",
        "skillsetName": "caselaw-ss",
        "targetIndexName": "caselaw",
        "disabled": null,
        "schedule": null,
        "parameters": {
            "batchSize": 1,
            "maxFailedItems": null,
            "maxFailedItemsPerBatch": null,
            "base64EncodeKeys": null,
            "configuration": {
                "parsingMode": "jsonLines"
            }
        },
        "fieldMappings": [],
        "outputFieldMappings": [
            {
                "sourceFieldName": "/document/casebody/data/opinions/*/text/pages/*/people/*",
                "targetFieldName": "people",
                "mappingFunction": null
            }
        ]
    }
    ```

## <a name="explore-knowledge-store"></a>Explorar el almacén de conocimientos

Puede empezar a explorar en cuanto se importe el primer documento. Para esta tarea, use el [**Explorador de Storage**](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-storage-explorer) en el portal.

Es importante tener en cuenta que un almacén de conocimientos está totalmente separado de Azure Search. El índice de Azure Search y el almacén de conocimientos contienen la representación de datos y el contenido, pero de manera independiente. Use el índice para búsqueda de texto completo, búsqueda filtrada y todos los escenarios admitidos en Azure Search. O bien, avance solo con el almacén de conocimientos y asocie otras herramientas para analizar el contenido.

## <a name="takeaways"></a>Puntos clave

Ahora ha creado su primer almacén de conocimientos en Azure Storage y ha usado el Explorador de Storage para ver los enriquecimientos. Esta es la experiencia fundamental para trabajar con enriquecimientos almacenados. 

## <a name="next-steps"></a>Pasos siguientes

La aptitud Conformador hace el trabajo pesado de creación de formularios de datos pormenorizados que se pueden combinar de formas nuevas. Como paso siguiente, revise la página de referencia para esta habilidad para obtener más información sobre cómo se usa.

> [!div class="nextstepaction"]
> [Referencia de la aptitud Conformador](cognitive-search-skill-shaper.md)


<!---
## Keep This

How to convert unformatted JSON into an indented JSON document structure that allows you to quickly identify nested structures. Useful for creating an index that includes complex types.

1. Use Visual Studio Code.
2. Open data.jsonl
--->