---
title: 'Creación de un almacén de conocimiento mediante REST: Azure Search'
description: Cree un almacén de conocimiento de Azure Search para conservar los enriquecimientos de la canalización de búsqueda cognitiva mediante API REST y Postman.
author: lobrien
services: search
ms.service: search
ms.topic: tutorial
ms.date: 10/01/2019
ms.author: laobri
ms.openlocfilehash: 26dc66474eecffd7f5a34bcfcaf93fd49f59606c
ms.sourcegitcommit: f2d9d5133ec616857fb5adfb223df01ff0c96d0a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/03/2019
ms.locfileid: "71936512"
---
# <a name="create-an-azure-search-knowledge-store-using-rest"></a>Creación de un almacén de conocimiento de Azure Search con REST

El almacén de conocimiento es una característica de Azure Search que conserva la salida de una canalización de enriquecimiento de inteligencia artificial para su análisis posterior o para otro procesamiento descendente. Una canalización enriquecida de inteligencia artificial acepta archivos de imagen o archivos de texto no estructurados, los indexa mediante Azure Search, aplica los enriquecimientos de inteligencia artificial de Cognitive Services (como el análisis de imágenes y el procesamiento de lenguaje natural) y, a continuación, guarda los resultados en un almacén de conocimiento en Azure Storage. A continuación, puede usar herramientas como Power BI o el Explorador de Storage para explorar el almacén de conocimiento.

En este artículo usará la interfaz de API REST para ingerir, indexar y aplicar los enriquecimientos de AI a un conjunto de reseñas sobre hoteles. Las reseñas sobre hoteles se importan en Azure Blob Storage y los resultados se guardan como almacén de conocimiento en Azure Table Storage.

Después de crear el almacén de conocimiento, aprenda a acceder a él mediante el [Explorador de Storage](knowledge-store-view-storage-explorer.md) o [Power BI](knowledge-store-connect-power-bi.md).

## <a name="1---create-services"></a>1: Creación de servicios

+ [Cree un servicio Azure Search](search-create-service-portal.md) o [busque un servicio existente](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) en su suscripción actual. Puede usar un servicio gratuito para este tutorial.

+ [Cree una cuenta de Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account) para almacenar los datos de ejemplo y el almacén de conocimiento. La cuenta de almacenamiento debe usar la misma ubicación (por ejemplo, oeste de EE. UU.) para el servicio Azure Search. *Tipo de cuenta* debe ser *StorageV2 (de uso general V2)* (predeterminado) o *Storage (de uso general V1)* .

+ Se recomienda: La [aplicación de escritorio Postman](https://www.getpostman.com/) se usa para enviar solicitudes a Azure Search. Puede usar API REST con cualquier herramienta capaz de trabajar con solicitudes y respuestas HTTP. Postman es una buena opción para explorar las API REST y se usará en este artículo. Además, el [código fuente](https://github.com/Azure-Samples/azure-search-postman-samples/blob/master/knowledge-store/KnowledgeStore.postman_collection.json) de este artículo incluye una colección de solicitudes de Postman. 

## <a name="2---store-the-data"></a>2: Almacenamiento de los datos

Cargue el archivo CSV de reseñas de hoteles en Azure Blob Storage para que un indexador de Azure Search pueda acceder a él y se pueda realizar el suministro mediante la canalización de enriquecimiento de inteligencia artificial.

### <a name="create-an-azure-blob-container-with-the-data"></a>Creación de un contenedor de blobs de Azure con los datos

1. [Descargue los datos de reseñas de hoteles guardados en un archivo CSV (HotelReviews_Free.csv)](https://knowledgestoredemo.blob.core.windows.net/hotel-reviews/HotelReviews_Free.csv?st=2019-07-29T17%3A51%3A30Z&se=2021-07-30T17%3A51%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=LnWLXqFkPNeuuMgnohiz3jfW4ijePeT5m2SiQDdwDaQ%3D). Estos datos proceden de Kaggle.com y contienen comentarios de los clientes sobre hoteles.
1. [Inicie sesión en Azure Portal](https://portal.azure.com) y vaya a la cuenta de Azure Storage.
1. [Cree un contenedor de blobs](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal). Para crear el contenedor, en la barra de navegación izquierda de la cuenta de almacenamiento, haga clic en **Blobs** y en **+ Contenedor** en la barra de comandos.
1. En el **Nombre** del nuevo contenedor, escriba `hotel-reviews`.
1. Seleccione cualquier **Nivel de acceso público**. Usamos el valor predeterminado.
1. Haga clic en **Aceptar** para crear el contenedor de blobs de Azure.
1. Abra el nuevo contenedor `hotels-review`, haga clic en **Cargar** y seleccione el archivo **HotelReviews-Free.csv** que descargó en el primer paso.

    ![Carga de los datos](media/knowledge-store-create-portal/upload-command-bar.png "Carga de las reseñas de hoteles")

1. Haga clic en **Cargar** para importar el archivo CSV en Azure Blob Storage. Aparecerá el nuevo contenedor.

    ![Creación del contenedor de blobs de Azure](media/knowledge-store-create-portal/hotel-reviews-blob-container.png "Create the Azure Blob container")

## <a name="3---configure-postman"></a>3: Configuración de Postman

Descargue el [código fuente de la colección de Postman](https://github.com/Azure-Samples/azure-search-postman-samples/blob/master/knowledge-store/KnowledgeStore.postman_collection.json) e impórtelo en Postman mediante **File, Import...** (Archivo, Importar...). Cambie a la pestaña **Collections** (Colecciones), haga clic en el botón **...** y seleccione **Edit** (Editar). 

![Aplicación Postman que muestra la navegación](media/knowledge-store-create-rest/postman-edit-menu.png "Dirección al menú Edit (Editar) en Postman")

En el cuadro de diálogo Edit (Editar) resultante, vaya a la pestaña **Variables** (Variables). 

La pestaña **Variables** (Variables) permite agregar valores que Postman intercambiará cada vez que los encuentre entre llaves dobles. Por ejemplo, Postman reemplazará el símbolo `{{admin-key}}` por el "valor actual" de `admin-key`. Postman realizará esta sustitución en las direcciones URL, los encabezados, el cuerpo de la solicitud, etc. 

Encontrará el valor de `admin-key` en la pestaña **Keys** (Claves) de Search Service. Deberá cambiar `search-service-name` y `storage-account-name` a los valores que eligió en el [paso 1](#1---create-services). Establezca `storage-connection-string`con el valor de la pestaña **Claves de acceso** de la cuenta de almacenamiento. Los demás valores se pueden dejar sin cambios.

![Pestaña de variables de la aplicación Postman](media/knowledge-store-create-rest/postman-variables-window.png "Ventana de variables de Postman")


| Variable    | Dónde obtenerla |
|-------------|-----------------|
| `admin-key` | Search Service, pestaña **Keys** (Claves)              |
| `api-version` | Dejar como "2019-05-06-Preview" |
| `datasource-name` | Dejar como "hotel-reviews-ds" | 
| `indexer-name` | Dejar como "hotel-reviews-ixr" | 
| `index-name` | Dejar como "hotel-reviews-ix" | 
| `search-service-name` | Search Service, nombre principal. La dirección URL es `https://{{search-service-name}}.search.windows.net` | 
| `skillset-name` | Dejar como "hotel-reviews-ss" | 
| `storage-account-name` | Cuenta de almacenamiento, nombre principal | 
| `storage-connection-string` | Cuenta de almacenamiento, pestaña **Claves de acceso**, **key1**, **Cadena de conexión** | 
| `storage-container-name` | Dejar como "hotel-reviews" | 

### <a name="review-the-request-collection-in-postman"></a>Revisión de la colección de solicitudes en Postman

La creación de un almacén de conocimiento requiere que se emitan cuatro solicitudes HTTP: 

1. Una solicitud PUT para crear el índice. Este índice contiene los datos usados y devueltos por Azure Search.
1. Una solicitud POST para crear el origen de datos. Este origen de datos conecta su comportamiento de Azure Search con la cuenta de almacenamiento de datos y del almacén de conocimiento. 
1. Una solicitud PUT para crear el conjunto de aptitudes. El conjunto de aptitudes especifica las características enriquecidas aplicadas a los datos y a la estructura del almacén de conocimiento.
1. Una solicitud PUT para crear el indexador. Al ejecutar el indexador, se leen los datos, se aplica el conjunto de aptitudes y se almacenan los resultados. Debe ejecutar esta solicitud en último lugar.

El [código fuente](https://github.com/Azure-Samples/azure-search-postman-samples/blob/master/knowledge-store/KnowledgeStore.postman_collection.json) contiene una colección de Postman con estas cuatro solicitudes. Para emitir las solicitudes, cambie a la pestaña de la solicitud en Postman y agregue los encabezados de solicitud `api-key` y `Content-Type`. Establezca el valor de `api-key` en `{{admin-key}}`. Establezca el valor `Content-type` en `application/json`. 

> [!div class="mx-imgBorder"]
> ![Captura de pantalla que muestra la interfaz de Postman para los encabezados](media/knowledge-store-create-rest/postman-headers-ui.png)

> [!Note]
> Tendrá que establecer los encabezados `api-key` y `Content-type` en todas las solicitudes. Si Postman reconoce una variable, se representará en texto naranja, como `{{admin-key}}` en la captura de pantalla. Si la variable está mal escrita, se representará en texto rojo.
>

## <a name="4---create-an-azure-search-index"></a>4: Creación de un índice de Azure Search

Debe crear un índice de Azure Search para representar los datos en los que desee realizar búsquedas, filtros y mejoras. Para crear el índice, emita una solicitud PUT a `https://{{search-service-name}}.search.windows.net/indexes/{{index-name}}?api-version={{api-version}}`. Postman reemplazará los símbolos entre llaves dobles, como `{{search-service-name}}`, `{{index-name}}` y `{{api-version}}`, por los valores especificados en el [paso 3](#3---configure-postman). Si usa otra herramienta para emitir los comandos REST, tendrá que sustituir esas variables por su cuenta.

Especifique la estructura del índice de Azure Search en el cuerpo de la solicitud. En Postman, después de establecer los encabezados `api-key` y `Content-type`, cambie al panel **Body** (Cuerpo) de la solicitud. Debería ver el siguiente código JSON, pero, si no es así, elija **Raw** (Sin formato) y **JSON (application/json)** (JSON [aplicación/json]) y pegue el código siguiente como cuerpo:

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

Verá que esta definición de índice es una combinación de los datos que le gustaría presentar al usuario (el nombre del hotel, la revisión del contenido, la fecha, etc.), los metadatos de búsqueda y los datos de mejora de AI (opinión, frases clave e idioma).

Presione el botón **Send** (Enviar) para emitir la solicitud PUT. Debería recibir el mensaje de estado `201 - Created`. Si recibe un estado diferente, en el panel **Body** (Cuerpo) se mostrará una respuesta JSON con un mensaje de error. 

## <a name="5---create-the-datasource"></a>5: Creación del origen de datos

Ahora, debe conectar Azure Search a los datos de hotel que almacenó en el [paso 2](#2---store-the-data). La creación del origen de datos se realiza con una solicitud POST a `https://{{search-service-name}}.search.windows.net/datasources?api-version={{api-version}}`. De nuevo, deberá establecer los encabezados `api-key` y `Content-Type` tal como se especificó anteriormente. 

En Postman, abra la solicitud "Create Datasource". Cambie al panel **Body** (Cuerpo), que debe tener el código siguiente:

```json
{
  "name" : "{{datasource-name}}",
  "description" : "Demo files to demonstrate knowledge store capabilities.",
  "type" : "azureblob",
  "credentials" : { "connectionString" : "{{storage-connection-string}}" },
  "container" : { "name" : "{{storage-container-name}}" }
}
```

Presione el botón **Send** (Enviar) para emitir la solicitud POST. 

## <a name="6---create-the-skillset"></a>6: Creación del conjunto de aptitudes 

El siguiente paso consiste en especificar el conjunto de aptitudes, que especifica las mejoras que se van a aplicar y el almacén de conocimiento donde se almacenarán los resultados. En Postman, abra la pestaña "Create the Skillset" (Crear el conjunto de aptitudes). Así se envía una solicitud PUT a `https://{{search-service-name}}.search.windows.net/skillsets/{{skillset-name}}?api-version={{api-version}}`.
Establezca los encabezados `api-key` y `Content-type` tal como lo ha hecho anteriormente. 

Hay dos grandes objetos de nivel superior: `"skills"` y `"knowledgeStore"`. Cada objeto del objeto `"skills"` es un servicio de enriquecimiento. Cada servicio de enriquecimiento tiene `"inputs"` y `"outputs"`. Observe cómo `LanguageDetectionSkill` tiene una salida `targetName` de `"Language"`. El valor de este nodo se usa en la mayoría de las demás aptitudes como entrada, con `document/Language` como origen. Esta capacidad de utilizar la salida de un nodo como entrada de otro es incluso más evidente en `ShaperSkill`, que especifica cómo fluyen los datos en las tablas del almacén de conocimiento.

El objeto `"knowledge_store"` se conecta a la cuenta de almacenamiento mediante la variable de Postman `{{storage-connection-string}}`. Contiene un conjunto de asignaciones entre el documento mejorado y las tablas y columnas que estarán disponibles en el propio almacén de conocimiento. 

Para generar el conjunto de aptitudes, coloque la solicitud PUT al presionar el botón **Send** (Enviar) de Postman.

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

## <a name="7---create-the-indexer"></a>7: Creación del indexador

El paso final consiste en crear el indexador, que realmente lee los datos y activa el conjunto de aptitudes. En Postman, cambie a la solicitud "Create Indexer" y revise el cuerpo. Como puede ver, la definición del indexador hace referencia a otros recursos que ya ha creado: el origen de datos, el índice y el conjunto de aptitudes. 

El objeto `"parameters/configuration"` controla cómo ingiere el indexador los datos. En este caso, los datos de entrada se encuentran en un único documento con una línea de encabezado y valores separados por comas. La clave del documento es un identificador único de este, que antes de la codificación es la dirección URL del documento de origen. Por último, los valores de salida del conjunto de aptitudes, como el código de idioma, la opinión y las frases clave, se asignan a sus ubicaciones correspondientes en el documento. Observe que, aunque hay un valor único para `Language`, se aplica `Sentiment` a cada elemento de la matriz de `pages`. `Keyphrases` es en sí una matriz y también se aplica a cada elemento de la matriz `pages`.

Después de establecer los encabezados `api-key` y `Content-type` y de confirmar que el cuerpo de la solicitud es similar al código fuente que sigue, presione **Send** (Enviar) en Postman. Postman colocará la solicitud PUT en `https://{{search-service-name}}.search.windows.net/indexers/{{indexer-name}}?api-version={{api-version}}`. Azure Search creará y ejecutará el indexador. 

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

## <a name="8---run-the-indexer"></a>8: Ejecución del indexador 

En Azure Portal, vaya a la **Información general** de Search Service y seleccione la pestaña **Indexers** (Indexadores). Haga clic en el **hotels-reviews-ixr** que creó en el paso anterior. Si el indexador aún no se ha ejecutado, presione el botón **Run** (Ejecutar). La tarea de indexación puede generar algunas advertencias relacionadas con el reconocimiento de idioma, ya que los datos incluyen algunas revisiones escritas en idiomas que todavía no son compatibles con los conocimientos cognitivos. 

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha enriquecido los datos con servicios cognitivos y ha proyectado los resultados en un almacén de conocimiento, puede usar el Explorador de Storage o Power BI para explorar el conjunto de datos enriquecido.

Para obtener información sobre cómo explorar este almacén de conocimiento con el Explorador de Storage, consulte el siguiente tutorial.

> [!div class="nextstepaction"]
> [Visualización con el Explorador de Storage](knowledge-store-view-storage-explorer.md)

Para obtener información sobre cómo conectar este almacén de conocimiento a Power BI, consulte el siguiente tutorial.

> [!div class="nextstepaction"]
> [Conexión con Power BI](knowledge-store-connect-power-bi.md)

Si desea repetir este ejercicio o realizar otro tutorial de enriquecimiento de inteligencia artificial, elimine el indexador *hotel-reviews-idxr*. La eliminación del indexador restablece el contador de transacciones diarias gratis a cero.
