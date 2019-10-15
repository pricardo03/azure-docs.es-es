---
title: 'Creación de un almacén de conocimiento mediante REST: Azure Search'
description: Cree un almacén de conocimiento de Azure Search para conservar los enriquecimientos de la canalización de búsqueda cognitiva mediante API REST y Postman.
author: lobrien
services: search
ms.service: search
ms.topic: tutorial
ms.date: 10/01/2019
ms.author: laobri
ms.openlocfilehash: b67f0cf60d279c7bc52b4114d29c37847f5c57f1
ms.sourcegitcommit: 824e3d971490b0272e06f2b8b3fe98bbf7bfcb7f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2019
ms.locfileid: "72244462"
---
# <a name="create-an-azure-search-knowledge-store-by-using-rest"></a>Creación de un almacén de conocimiento de Azure Search con REST

El almacén de conocimiento es una característica de Azure Search que conserva la salida de una canalización de enriquecimiento de inteligencia artificial para su análisis o procesamiento posteriores. Una canalización enriquecida de inteligencia artificial acepta archivos de imagen o de texto no estructurados, los indexa mediante Azure Search, aplica los enriquecimientos de inteligencia artificial de Azure Cognitive Services (como el análisis de imágenes y el procesamiento de lenguaje natural) y guarda los resultados en un almacén de conocimiento en Azure Storage. Puede usar herramientas como Power BI o el Explorador de Storage de Azure Portal para explorar el almacén de conocimiento.

En este artículo se usa la interfaz de API REST para ingerir, indexar y aplicar los enriquecimientos de inteligencia artificial a un conjunto de reseñas sobre hoteles. Las reseñas sobre hoteles se importan en Azure Blob Storage. Los resultados se guardan como almacén de información en Azure Table Storage.

Después de crear el almacén de conocimiento, puede aprender a acceder a este mediante el [Explorador de Storage](knowledge-store-view-storage-explorer.md) o [Power BI](knowledge-store-connect-power-bi.md).

## <a name="create-services"></a>Creación de servicios

Cree los servicios siguientes:

- Cree un [servicio Azure Search](search-create-service-portal.md) o [busque un servicio existente](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) en su suscripción actual. Puede usar un servicio gratuito para este tutorial.

- Cree una [cuenta de Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account) para almacenar los datos de ejemplo y el almacén de conocimiento. La cuenta de almacenamiento debe usar la misma ubicación (por ejemplo, oeste de EE. UU.) para el servicio Azure Search. El valor de **Tipo de cuenta** debe ser **StorageV2 (de uso general V2)** (predeterminado) o **Storage (de uso general V1)** .

- Se recomienda: Obtenga la [aplicación de escritorio Postman](https://www.getpostman.com/) para enviar solicitudes a Azure Search. Puede usar API REST con cualquier herramienta capaz de trabajar con solicitudes y respuestas HTTP. Postman es una buena opción para explorar las API REST. En este artículo se usa Postman. Además, el [código fuente](https://github.com/Azure-Samples/azure-search-postman-samples/blob/master/knowledge-store/KnowledgeStore.postman_collection.json) de este artículo incluye una recopilación de solicitudes de Postman. 

## <a name="store-the-data"></a>Almacenamiento de los datos

Cargue el archivo CSV de reseñas de hoteles en Azure Blob Storage para que un indexador de Azure Search pueda acceder a él y se pueda realizar el suministro mediante la canalización de enriquecimiento de inteligencia artificial.

### <a name="create-a-blob-container-by-using-the-data"></a>Creación de un contenedor de blobs mediante los datos

1. Descargue los [datos de reseñas sobre hoteles](https://knowledgestoredemo.blob.core.windows.net/hotel-reviews/HotelReviews_Free.csv?st=2019-07-29T17%3A51%3A30Z&se=2021-07-30T17%3A51%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=LnWLXqFkPNeuuMgnohiz3jfW4ijePeT5m2SiQDdwDaQ%3D) guardados en un archivo CSV (HotelReviews_Free.csv). Estos datos proceden de Kaggle.com y contienen comentarios de los clientes sobre hoteles.
1. Inicie sesión en [Azure Portal](https://portal.azure.com) y vaya a la cuenta de Azure Storage.
1. Cree un [contenedor de blobs](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal). Para crear el contenedor, en el menú de la izquierda de la cuenta de almacenamiento, seleccione **Blobs** y **Contenedor**.
1. En **Nombre** del nuevo contenedor, escriba **hotel-reviews**.
1. Para **Nivel de acceso público**, seleccione cualquier valor. Usamos el valor predeterminado.
1. Seleccione **Aceptar** para crear el contenedor de blobs.
1. Abra el nuevo contenedor **hotel-reviews**, seleccione **Cargar** y, después, el archivo HotelReviews-Free.csv que descargó en el primer paso.

    ![Carga de los datos](media/knowledge-store-create-portal/upload-command-bar.png "Carga de las reseñas de hoteles")

1. Seleccione **Cargar** para importar el archivo CSV en Azure Blob Storage. Aparecerá el nuevo contenedor:

    ![Creación del contenedor de blobs](media/knowledge-store-create-portal/hotel-reviews-blob-container.png "Create the blob container")

## <a name="configure-postman"></a>Configuración de Postman

Instale y configure Postman.

### <a name="download-and-install-postman"></a>Descarga e instalación de Postman

1. Descargue el [código fuente de la recopilación de Postman](https://github.com/Azure-Samples/azure-search-postman-samples/blob/master/knowledge-store/KnowledgeStore.postman_collection.json).
1. Seleccione **Archivo** > **Importar** para importar el código fuente en Postman.
1. Seleccione la pestaña **Recopilaciones** el botón **...** (puntos suspensivos).
1. Seleccione **Editar**. 
   
   ![Aplicación Postman que muestra la navegación](media/knowledge-store-create-rest/postman-edit-menu.png "Dirección al menú Edit (Editar) en Postman")
1. En el cuadro de diálogo **Edit** (Editar), seleccione la pestaña **Variables** (Variables). 

En la pestaña **Variables** (Variables) puede agregar valores para los intercambios de Postman cada vez que encuentre una variable específica entre llaves dobles. Por ejemplo, Postman reemplaza el símbolo `{{admin-key}}` por el valor actual que estableciera para `admin-key`. Postman realiza esta sustitución en las direcciones URL, los encabezados, el cuerpo de la solicitud, etc. 

Para obtener el valor de `admin-key`, vaya al servicio Azure Search y seleccione la pestaña **Claves**. Cambie `search-service-name` y `storage-account-name` por los valores que eligió en [Creación de servicios](#create-services). Establezca `storage-connection-string` con el valor de la pestaña **Claves de acceso** de la cuenta de almacenamiento. Puede dejar los valores predeterminados para los demás valores.

![Pestaña de variables de la aplicación Postman](media/knowledge-store-create-rest/postman-variables-window.png "Ventana de variables de Postman")


| Variable    | Dónde obtenerla |
|-------------|-----------------|
| `admin-key` | En la pestaña **Claves** del servicio Azure Search.  |
| `api-version` | Déjela como **2019-05-06-Preview**. |
| `datasource-name` | Déjela como **hotel-reviews-ds**. | 
| `indexer-name` | Déjela como **hotel-reviews-ixr**. | 
| `index-name` | Déjela como **hotel-reviews-ix**. | 
| `search-service-name` | Nombre principal del servicio Azure Search. La dirección URL es `https://{{search-service-name}}.search.windows.net`. | 
| `skillset-name` | Déjela como **hotel-reviews-ss**. | 
| `storage-account-name` | Nombre principal de la cuenta de almacenamiento. | 
| `storage-connection-string` | En la cuenta de almacenamiento, en la pestaña **Claves de acceso**, seleccione **key1** > **Cadena de conexión**. | 
| `storage-container-name` | Déjela como **hotel-reviews**. | 

### <a name="review-the-request-collection-in-postman"></a>Revisión de la colección de solicitudes en Postman

Al crear un almacén de información, debe emitir cuatro solicitudes HTTP: 

- **Una solicitud PUT para crear el índice**: Este índice contiene los datos usados y devueltos por Azure Search.
- **Una solicitud POST para crear el origen de datos**: Este origen de datos conecta su comportamiento de Azure Search con la cuenta de almacenamiento de datos y del almacén de conocimiento. 
- **Una solicitud PUT para crear el conjunto de aptitudes**: El conjunto de aptitudes especifica las características enriquecidas que se aplican a los datos y a la estructura del almacén de conocimiento.
- **Una solicitud PUT para crear el indexador**: Al ejecutar el indexador, se leen los datos, se aplica el conjunto de aptitudes y se almacenan los resultados. Debe ejecutar esta solicitud en último lugar.

El [código fuente](https://github.com/Azure-Samples/azure-search-postman-samples/blob/master/knowledge-store/KnowledgeStore.postman_collection.json) contiene una recopilación de Postman con estas cuatro solicitudes. Para emitir las solicitudes, seleccione en Postman la pestaña de la solicitud. Después, agregue los encabezados de solicitud `api-key` y `Content-Type`. Establezca el valor de `api-key` en `{{admin-key}}`. Establezca el valor `Content-type` en `application/json`. 

![Captura de pantalla que muestra la interfaz de Postman para los encabezados](media/knowledge-store-create-rest/postman-headers-ui.png)

> [!Note]
> Debe establecer los encabezados `api-key` y `Content-type` en todas las solicitudes. Si Postman reconoce una variable, esta aparece en texto naranja, como `{{admin-key}}` en la captura de pantalla anterior. Si la variable está mal escrita, aparece en texto rojo.
>

## <a name="create-an-azure-search-index"></a>Creación de un índice de Azure Search

Cree un índice de Azure Search para representar los datos en los que desee realizar búsquedas, filtros y mejoras. Emita una solicitud PUT a `https://{{search-service-name}}.search.windows.net/indexes/{{index-name}}?api-version={{api-version}}` para crear el índice. Postman reemplaza los símbolos que se incluyen entre llaves dobles (como `{{search-service-name}}`, `{{index-name}}` y `{{api-version}}`) por los valores establecidos en [Configuración de Postman](#configure-postman). Si usa otra herramienta para emitir los comandos REST, tendrá que sustituir esas variables por su cuenta.

Establezca la estructura del índice de Azure Search en el cuerpo de la solicitud. En Postman, después de establecer los encabezados `api-key` y `Content-type`, vaya al panel **Body** (Cuerpo) de la solicitud. Debe ver el código JSON siguiente. Si no es el caso, seleccione **Sin formato** > **JSON (application/json)** (JSON [aplicación/json]) y pegue el código siguiente como cuerpo:

```JSON
{
    "name": "{{index-name}}",
    "fields": [
        { "name": "address", "type": "Edm.String", "searchable": false, "filterable": false, "sortable": false, "facetable": false },
        { "name": "categories", "type": "Edm.String", "searchable": false, "filterable": false, "sortable": false, "facetable": false },
        { "name": "city", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false },
        { "name": "country", "type": "Edm.String", "searchable": false, "filterable": false, "sortable": false, "facetable": false },
        { "name": "latitude", "type": "Edm.String", "searchable": false, "filterable": false, "sortable": false, "facetable": false },
        { "name": "longitude", "type": "Edm.String", "searchable": false, "filterable": false, "sortable": false, "facetable": false },
        { "name": "name", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false },
        { "name": "postalCode", "type": "Edm.String", "searchable": false, "filterable": false, "sortable": false, "facetable": false },
        { "name": "province", "type": "Edm.String", "searchable": false, "filterable": false, "sortable": false, "facetable": false },
        { "name": "reviews_date", "type": "Edm.DateTimeOffset", "searchable": false, "filterable": false, "sortable": false, "facetable": false },
        { "name": "reviews_dateAdded", "type": "Edm.DateTimeOffset", "searchable": false, "filterable": false, "sortable": false, "facetable": false },
        { "name": "reviews_rating", "type": "Edm.String", "searchable": false, "filterable": false, "sortable": false, "facetable": false },
        { "name": "reviews_text", "type": "Edm.String", "filterable": false,  "sortable": false, "facetable": false },
        { "name": "reviews_title", "type": "Edm.String", "searchable": false, "filterable": false, "sortable": false, "facetable": false },
        { "name": "reviews_username", "type": "Edm.String", "searchable": false, "filterable": false, "sortable": false, "facetable": false },
        { "name": "AzureSearch_DocumentKey", "type": "Edm.String", "searchable": false, "filterable": false, "sortable": false, "facetable": false, "key": true },
        { "name": "metadata_storage_content_type", "type": "Edm.String", "searchable": false, "filterable": false, "sortable": false, "facetable": false },
        { "name": "metadata_storage_size", "type": "Edm.Int64", "searchable": false, "filterable": false, "sortable": false, "facetable": false},
        { "name": "metadata_storage_last_modified", "type": "Edm.DateTimeOffset", "searchable": false, "filterable": false, "sortable": false, "facetable": false },
        { "name": "metadata_storage_name", "type": "Edm.String", "searchable": false, "filterable": false, "sortable": false, "facetable": false },
        { "name": "metadata_storage_path", "type": "Edm.String", "searchable": false, "filterable": false, "sortable": false, "facetable": false },
        { "name": "Sentiment", "type": "Collection(Edm.Double)", "searchable": false, "filterable": true, "retrievable": true, "sortable": false, "facetable": true },
        { "name": "Language", "type": "Edm.String", "filterable": true, "sortable": false, "facetable": true },
        { "name": "Keyphrases", "type": "Collection(Edm.String)", "filterable": true, "sortable": false, "facetable": true }
    ]
}

```

Esta definición de índice es una combinación de los datos que le gustaría presentar al usuario (el nombre del hotel, contenido de la reseña, la fecha), los metadatos de búsqueda y los datos de mejora de inteligencia artificial (opinión, frases clave e idioma).

Seleccione **Enviar** para emitir la solicitud PUT. Debería ver el estado `201 - Created`. Si ve un estado diferente, en el panel **Body** (Cuerpo), busque una respuesta JSON que contenga un mensaje de error. 

## <a name="create-the-datasource"></a>Creación del origen de datos

A continuación, conecte Azure Search a los datos de hotel que almacenó en [Almacenamiento de los datos](#store-the-data). Para crear el origen de datos, envíe una solicitud POST a `https://{{search-service-name}}.search.windows.net/datasources?api-version={{api-version}}`. Debe establecer los encabezados `api-key` y `Content-Type` como se explicó anteriormente. 

En Postman, vaya a la solicitud **Create Datasource** (Crear origen de datos) y al panel **Body** (Cuerpo). Debería ver el código siguiente:

```json
{
  "name" : "{{datasource-name}}",
  "description" : "Demo files to demonstrate knowledge store capabilities.",
  "type" : "azureblob",
  "credentials" : { "connectionString" : "{{storage-connection-string}}" },
  "container" : { "name" : "{{storage-container-name}}" }
}
```

Seleccione **Enviar** para emitir la solicitud POST. 

## <a name="create-the-skillset"></a>Creación del conjunto de aptitudes 

El siguiente paso consiste en especificar el conjunto de aptitudes, que especifica las mejoras que se van a aplicar y el almacén de conocimiento donde se almacenarán los resultados. En Postman, seleccione la pestaña **Create the Skillset** (Crear el conjunto de aptitudes). Así se envía una solicitud PUT a `https://{{search-service-name}}.search.windows.net/skillsets/{{skillset-name}}?api-version={{api-version}}`. Establezca los encabezados `api-key` y `Content-type` como hizo anteriormente. 

Hay dos grandes objetos de nivel superior: `skills` y `knowledgeStore`. Cada objeto del objeto `skills` es un servicio de enriquecimiento. Cada servicio de enriquecimiento tiene `inputs` y `outputs`. Observe cómo `LanguageDetectionSkill` tiene una salida `targetName` de `Language`. El valor de este nodo se usa en la mayoría de las demás aptitudes como entrada. El origen es `document/Language`. Esta capacidad de utilizar la salida de un nodo como entrada de otro es incluso más evidente en `ShaperSkill`, que especifica cómo fluyen los datos en las tablas del almacén de conocimiento.

El objeto `knowledge_store` se conecta a la cuenta de almacenamiento mediante la variable de Postman `{{storage-connection-string}}`. `knowledge_store` contiene un conjunto de asignaciones entre el documento mejorado y las tablas y columnas del almacén de conocimiento. 

Para generar el conjunto de aptitudes, seleccione el botón **Send** (Enviar) de Postman para colocar la solicitud PUT:

```json
{
    "name": "{{skillset-name}}",
    "description": "Skillset to detect language, extract key phrases, and detect sentiment",
    "skills": [ 
        {
            "@odata.type": "#Microsoft.Skills.Text.SplitSkill", 
            "context": "/document/reviews_text", "textSplitMode": "pages", "maximumPageLength": 5000,
            "inputs": [ 
                { "name": "text", "source": "/document/reviews_text" },
                { "name": "languageCode", "source": "/document/Language" }
            ],
            "outputs": [
                { "name": "textItems", "targetName": "pages" }
            ]
        },
        {
            "@odata.type": "#Microsoft.Skills.Text.SentimentSkill",
            "context": "/document/reviews_text/pages/*",
            "inputs": [
                { "name": "text", "source": "/document/reviews_text/pages/*" },
                { "name": "languageCode", "source": "/document/Language" }
            ],
            "outputs": [
                { "name": "score", "targetName": "Sentiment" }
            ]
        },
        {
            "@odata.type": "#Microsoft.Skills.Text.LanguageDetectionSkill",
            "context": "/document",
            "inputs": [
                { "name": "text", "source": "/document/reviews_text" }
            ],
            "outputs": [
                { "name": "languageCode", "targetName": "Language" }
            ]
        },
        {
            "@odata.type": "#Microsoft.Skills.Text.KeyPhraseExtractionSkill",
            "context": "/document/reviews_text/pages/*",
            "inputs": [
                { "name": "text",  "source": "/document/reviews_text/pages/*" },
                { "name": "languageCode",  "source": "/document/Language" }
            ],
            "outputs": [
                { "name": "keyPhrases" , "targetName": "Keyphrases" }
            ]
        },
        {
            "@odata.type": "#Microsoft.Skills.Util.ShaperSkill",
            "context": "/document",
            "inputs": [
                { "name": "name",  "source": "/document/name" },
                { "name": "reviews_date",  "source": "/document/reviews_date" },
                { "name": "reviews_rating",  "source": "/document/reviews_rating" },
                { "name": "reviews_text",  "source": "/document/reviews_text" },
                { "name": "reviews_title",  "source": "/document/reviews_title" },
                { "name": "AzureSearch_DocumentKey",  "source": "/document/AzureSearch_DocumentKey" },
                { 
                    "name": "pages",
                    "sourceContext": "/document/reviews_text/pages/*",
                    "inputs": [
                        { "name": "SentimentScore", "source": "/document/reviews_text/pages/*/Sentiment" },
                        { "name": "LanguageCode", "source": "/document/Language" },
                        { "name": "Page", "source": "/document/reviews_text/pages/*" },
                        { 
                            "name": "keyphrase", "sourceContext": "/document/reviews_text/pages/*/Keyphrases/*",
                            "inputs": [
                                { "name": "Keyphrases", "source": "/document/reviews_text/pages/*/Keyphrases/*" }
                            ]
                        }
                    ]
                }
            ],
            "outputs": [
                { "name": "output" , "targetName": "tableprojection" }
            ]
        }
    ],
    "knowledgeStore": {
        "storageConnectionString": "{{storage-connection-string}}",
        "projections": [
            {
                "tables": [
                    { "tableName": "hotelReviewsDocument", "generatedKeyName": "Documentid", "source": "/document/tableprojection" },
                    { "tableName": "hotelReviewsPages", "generatedKeyName": "Pagesid", "source": "/document/tableprojection/pages/*" },
                    { "tableName": "hotelReviewsKeyPhrases", "generatedKeyName": "KeyPhrasesid", "source": "/document/tableprojection/pages/*/keyphrase/*" },
                    { "tableName": "hotelReviewsSentiment", "generatedKeyName": "Sentimentid", "source": "/document/tableprojection/pages/*/sentiment/*" }
                ],
                "objects": []
            },
            {
                "tables": [
                    { 
                        "tableName": "hotelReviewsInlineDocument", "generatedKeyName": "Documentid", "sourceContext": "/document",
                        "inputs": [
                            { "name": "name", "source": "/document/name"},
                            { "name": "reviews_date", "source": "/document/reviews_date"},
                            { "name": "reviews_rating", "source": "/document/reviews_rating"},
                            { "name": "reviews_text", "source": "/document/reviews_text"},
                            { "name": "reviews_title", "source": "/document/reviews_title"},
                            { "name": "AzureSearch_DocumentKey", "source": "/document/AzureSearch_DocumentKey" }
                        ]
                    },
                    { 
                        "tableName": "hotelReviewsInlinePages", "generatedKeyName": "Pagesid", "sourceContext": "/document/reviews_text/pages/*",
                        "inputs": [
                            { "name": "SentimentScore", "source": "/document/reviews_text/pages/*/Sentiment"},
                            { "name": "LanguageCode", "source": "/document/Language"},
                            { "name": "Page", "source": "/document/reviews_text/pages/*" }
                        ]
                    },
                    { 
                        "tableName": "hotelReviewsInlineKeyPhrases", "generatedKeyName": "kpidv2", "sourceContext": "/document/reviews_text/pages/*/Keyphrases/*",
                        "inputs": [
                            { "name": "Keyphrases", "source": "/document/reviews_text/pages/*/Keyphrases/*" }
                        ]
                    }
                ],
                "objects": []
            }
        ]
    }
}
```

## <a name="create-the-indexer"></a>Creación del indexador

El último paso es crear el indexador. Este lee los datos y activa el conjunto de aptitudes. En Postman, seleccione la solicitud **Create Indexer** (Crear indexador) y revise el cuerpo de esta. La definición del indexador hace referencia a otros recursos que ya ha creado: el origen de datos, el índice y el conjunto de aptitudes. 

El objeto `parameters/configuration` controla cómo ingiere el indexador los datos. En este caso, los datos de entrada se encuentran en un único documento con una línea de encabezado y valores separados por comas. La clave del documento es un identificador único de este. Antes de la codificación, la clave del documento es la dirección URL del documento de origen. Por último, los valores de salida del conjunto de aptitudes, como el código de idioma, la opinión y las frases clave, se asignan a sus ubicaciones correspondientes en el documento. Aunque hay un valor único para `Language`, se aplica `Sentiment` a cada elemento de la matriz de `pages`. `Keyphrases` es una matriz y también se aplica a cada elemento de la matriz `pages`.

Después de establecer los encabezados `api-key` y `Content-type`, y de confirmar que el cuerpo de la solicitud es similar al código fuente siguiente, seleccione **Send** (Enviar) en Postman. Postman envía una solicitud PUT a `https://{{search-service-name}}.search.windows.net/indexers/{{indexer-name}}?api-version={{api-version}}`. Azure Search crea y ejecuta el indexador. 

```json
{
    "name": "{{indexer-name}}",
    "dataSourceName": "{{datasource-name}}",
    "skillsetName": "{{skillset-name}}",
    "targetIndexName": "{{index-name}}",
    "parameters": {
        "configuration": {
            "dataToExtract": "contentAndMetadata",
            "parsingMode": "delimitedText",
            "firstLineContainsHeaders": true,
            "delimitedTextDelimiter": ","
        }
    },
    "fieldMappings": [
        {
            "sourceFieldName": "AzureSearch_DocumentKey",
            "targetFieldName": "AzureSearch_DocumentKey",
            "mappingFunction": { "name": "base64Encode" }
        }
    ],
    "outputFieldMappings": [
        { "sourceFieldName": "/document/reviews_text/pages/*/Keyphrases/*", "targetFieldName": "Keyphrases" },
        { "sourceFieldName": "/document/Language", "targetFieldName": "Language" },
        { "sourceFieldName": "/document/reviews_text/pages/*/Sentiment", "targetFieldName": "Sentiment" }
    ]
}
```

## <a name="run-the-indexer"></a>Ejecución del indexador 

En Azure Portal, vaya a la página **Introducción** del servicio Azure Search. Seleccione la pestaña **Indexadores** y **hotel-reviews-ixr**. Si el indexador todavía no se ha ejecutado, seleccione **Ejecutar**. La tarea de indexación podría generar algunas advertencias relacionadas con el reconocimiento de idioma. Los datos incluyen algunas reseñas escritas en idiomas que las aptitudes cognitivas todavía no admiten. 

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha enriquecido los datos con Cognitive Services y ha proyectado los resultados en un almacén de conocimiento, puede usar el Explorador de Storage o Power BI para explorar el conjunto de datos enriquecido.

Para aprender a explorar este almacén de conocimiento con el Explorador de Storage, consulte el siguiente tutorial:

> [!div class="nextstepaction"]
> [Visualización con el Explorador de Storage](knowledge-store-view-storage-explorer.md)

Para aprender a conectar este almacén de conocimiento a Power BI, consulte el siguiente tutorial:

> [!div class="nextstepaction"]
> [Conexión con Power BI](knowledge-store-connect-power-bi.md)

Si desea repetir este ejercicio o realizar otro tutorial de enriquecimiento de inteligencia artificial, elimine el indexador **hotel-reviews-idxr**. La eliminación del indexador restablece el contador de transacciones diarias gratis a cero.
