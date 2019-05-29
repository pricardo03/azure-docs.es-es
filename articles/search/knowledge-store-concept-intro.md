---
title: 'Introducción a Knowledge Store (versión preliminar): Azure Search'
description: Envíe documentos enriquecidos a Azure Storage, donde puede ver, cambiar la forma y consumir documentos enriquecidos en Azure Search y en otras aplicaciones.
manager: cgronlun
author: HeidiSteen
services: search
ms.service: search
ms.devlang: NA
ms.topic: overview
ms.date: 05/02/2019
ms.author: heidist
ms.openlocfilehash: 4a27e4d8f2fbaafe6d27a3e3cabd31aa715b9d80
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/11/2019
ms.locfileid: "65540751"
---
# <a name="what-is-knowledge-store-in-azure-search"></a>¿Qué es Knowledge Store en Azure Search?

> [!Note]
> Knowledge Store se encuentra en versión preliminar y no está pensado para su uso en producción. En la [API REST, versión 2019-05-06-Preview](search-api-preview.md) se proporciona esta característica. Por el momento, no hay compatibilidad con .NET SDK.
>

Knowledge Store es una característica opcional de Azure Search que guarda documentos enriquecidos y metadatos creados por una canalización de indexación basada en IA [(búsqueda cognitiva)](cognitive-search-concept-intro.md). Knowledge Store tiene el respaldo de una cuenta de Azure Storage que se configura como parte de la canalización. Cuando se habilita, el servicio de búsqueda usa esta cuenta de almacenamiento para guardar en caché una representación de cada documento enriquecido. 

Si ha usado la búsqueda cognitiva anteriormente, ya sabe que los conjuntos de aptitudes pueden usarse para mover un documento a través de una secuencia de enriquecimientos. El resultado puede ser un índice de Azure Search o (algo nuevo en esta versión preliminar) proyecciones en un almacén de conocimientos.

Las proyecciones son el mecanismo para estructurar los datos para su consumo en una aplicación de nivel inferior. Puede usar el [Explorador de Storage](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows) creado para Azure Storage, o cualquier aplicación que se conecte a Azure Storage, lo que abre nuevas posibilidades para consumir documentos enriquecidos. Algunos ejemplos son canalizaciones de ciencia de datos y análisis personalizados.

![Diagrama de Knowledge Store en una canalización](./media/knowledge-store-concept-intro/annotationstore_sans_internalcache.png "Knowledge store in pipeline diagram")

Para usar Knowledge Store, agregue un elemento `knowledgeStore` a un conjunto de aptitudes que defina las operaciones escalonadas en una canalización de indexación. Durante la ejecución, Azure Search crea un espacio en la cuenta de Azure Storage y lo llena con las definiciones y el contenido creados por la canalización.

## <a name="benefits-of-knowledge-store"></a>Ventajas de Knowledge Store

Un almacén de conocimientos le brinda estructura, contexto y contenido real, recopilados a partir de archivos de datos no estructurados y semiestructurados, como blobs, archivos de imagen que se han sometido a análisis o, incluso, datos estructurados cuya forma ha cambiado. En una [guía paso a paso](knowledge-store-howto.md) escrita para esta versión preliminar, puede ver de primera mano cómo un documento JSON denso se particiona en subestructuras, se reconstituye en nuevas estructuras y, además, se pone a disposición para procesos de nivel inferior, como cargas de trabajo de ciencia de datos y aprendizaje automático.

Aunque es útil ver lo que puede producir una canalización de indexación basada en IA, el verdadero poder de Knowledge Store está en la capacidad de remodelar los datos. Puede comenzar con un conjunto de aptitudes básico y, luego, iterarlo para agregar niveles crecientes de estructura que luego se pueden combinar en nuevas estructuras, y que pueden consumirse en otras aplicaciones, además de Azure Search.

Entre las ventajas de Knowledge Store, se incluyen las siguientes:

+ Consumir documentos enriquecidos en [herramientas de informes y análisis](#tools-and-apps), además de buscar. Power BI con Power Query es una opción atractiva, pero cualquier herramienta o aplicación que se pueda conectar a Azure Storage puede beneficiarse de un almacén de conocimientos que cree.

+ Refinar una canalización de indexación de IA durante la depuración de pasos y definiciones de conjuntos de aptitudes. Un almacén de conocimientos le muestra el producto de la definición de un conjunto de aptitudes en una canalización de indexación de IA. Puede usar estos resultados para diseñar un mejor conjunto de aptitudes, ya que puede ver exactamente la apariencia de los enriquecimientos. Puede usar el [Explorador de Storage](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows) en Azure Storage para ver el contenido de un almacén de conocimientos.

+ Cambiar la forma de los datos. El cambio de formas se codifica en conjuntos de aptitudes, pero la idea es que ahora un conjunto de aptitudes puede proporcionar esta funcionalidad. La [aptitud Conformador](cognitive-search-skill-shaper.md) en Azure Search se ha ampliado para dar cabida a esta tarea. Cambiar la forma le permite definir una proyección que se alinee con el uso previsto de los datos, al tiempo que conserva las relaciones.

> [!Note]
> ¿No está familiarizado con la indexación basada en IA mediante Cognitive Services? Azure Search se integra en las características Vision y Language de Cognitive Services para extraer y enriquecer los datos de origen mediante el reconocimiento óptico de caracteres (OCR) en archivos de imágenes, el reconocimiento de entidades y la extracción de frases clave de archivos de texto y mucho más. Para más información, consulte [¿Qué es la búsqueda cognitiva?](cognitive-search-concept-intro.md)

## <a name="create-a-knowledge-store"></a>Crear un almacén de conocimientos

Un almacén de conocimientos es parte de la definición de un conjunto de aptitudes. En esta versión preliminar, su creación requiere la API de REST mediante `api-version=2019-05-06-Preview` o el asistente para **importar datos** del portal.

El siguiente código JSON especifica un `knowledgeStore`, que forma parte de un conjunto de aptitudes, al que invoca un indexador (no se muestra). La especificación de proyecciones dentro de `knowledgeStore` determina si se crean tablas u objetos en Azure Storage.

Si ya está familiarizado con la indexación basada en IA, la definición de un conjunto de aptitudes determina la creación, la organización y la sustancia de cada documento enriquecido.

```json
{
  "name": "my-new-skillset",
  "description": 
  "Example showing knowledgeStore placement, supported in api-version=2019-05-06-Preview. You need at least one skill, most likely a Shaper skill if you are modulating data structures.",
  "skills":
  [
    {
    "@odata.type": "#Microsoft.Skills.Util.ShaperSkill",
    "context": "/document/content/phrases/*",
    "inputs": [
        {
        "name": "text",
        "source": "/document/content/phrases/*"
        },
        {
        "name": "sentiment",
        "source": "/document/content/phrases/*/sentiment"
        }
    ],
    "outputs": [
        {
        "name": "output",
        "targetName": "analyzedText"
        }
    ]
    },
  ],
  "cognitiveServices": 
    {
    "@odata.type": "#Microsoft.Azure.Search.CognitiveServicesByKey",
    "description": "mycogsvcs resource in West US 2",
    "key": "<your key goes here>"
    },
  "knowledgeStore": { 
    "storageConnectionString": "<your connection string goes here>", 
    "projections": [ 
        { 
            "tables": [  
            { "tableName": "Reviews", "generatedKeyName": "ReviewId", "source": "/document/Review" , "sourceContext": null, "inputs": []}, 
            { "tableName": "Sentences", "generatedKeyName": "SentenceId", "source": "/document/Review/Sentences/*", "sourceContext": null, "inputs": []}, 
            { "tableName": "KeyPhrases", "generatedKeyName": "KeyPhraseId", "source": "/document/Review/Sentences/*/KeyPhrases", "sourceContext": null, "inputs": []}, 
            { "tableName": "Entities", "generatedKeyName": "EntityId", "source": "/document/Review/Sentences/*/Entities/*" ,"sourceContext": null, "inputs": []} 

            ], 
            "objects": [ 
                { 
                "storageContainer": "Reviews", 
                "format": "json", 
                "source": "/document/Review", 
                "key": "/document/Review/Id" 
                } 
            ]      
        }    
    ]     
    } 
}
```

## <a name="components-backing-a-knowledge-store"></a>Componentes que respaldan un almacén de conocimientos

Para crear un almacén de conocimientos, necesita los servicios y artefactos siguientes.

### <a name="1---source-data"></a>1. Datos de origen

Los datos o los documentos que quiere enriquecer deben existir en un origen de datos de Azure compatible con los indexadores de Azure Search: 

* [SQL de Azure](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)

* [Azure Cosmos DB](search-howto-index-cosmosdb.md)

* [Almacenamiento de blobs de Azure](search-howto-indexing-azure-blob-storage.md)

[Azure Table Storage](search-howto-indexing-azure-tables.md) puede usarse para datos de salida en un almacén de conocimientos, pero no puede usarse como recurso de datos de entrada a una canalización de indexación basada en IA.

### <a name="2---azure-search-service"></a>2. Servicio Azure Search

También necesita un servicio Azure Search y la API de REST para crear y configurar objetos usados para el enriquecimiento de datos. La API de REST para crear un almacén de conocimientos es `api-version=2019-05-06-Preview`.

Azure Search proporciona la característica de indexador, y los indexadores se usan para controlar el proceso completo, de un extremo a otro, lo que genera documentos enriquecidos persistentes en Azure Storage. Los indexadores usan un origen de datos, un índice y un conjunto de aptitudes, todos ellos necesarios para crear y rellenar un almacén de conocimientos.

| Objeto | API DE REST | DESCRIPCIÓN |
|--------|----------|-------------|
| origen de datos | [Create Data Source](https://docs.microsoft.com/rest/api/searchservice/create-data-source) (Creación de un origen de datos)  | Recurso que identifica un origen de datos de Azure externo que proporciona los datos de origen que se usan para crear documentos enriquecidos.  |
| conjunto de aptitudes | [Create Skillset (api-version=2019-05-06)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)  | Recurso que coordina el uso de las [aptitudes integradas](cognitive-search-predefined-skills.md) y las [aptitudes cognitivas personalizadas](cognitive-search-custom-skill-interface.md) que se usan en una canalización de enriquecimiento durante el indexado. |
| index | [Crear índice](https://docs.microsoft.com/rest/api/searchservice/create-index)  | Esquema que expresa un índice de Azure Search. Los campos del índice se asignan a los campos de los datos de origen o a los campos fabricados durante la fase de enriquecimiento (por ejemplo, un campo para los nombres de organización creado por el reconocimiento de entidades). |
| indexador | [Create Indexer (api-version=2019-05-06)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)  | Recurso que define los componentes usados durante la indexación, los que incluyen un origen de datos, un conjunto de aptitudes, asociaciones de campos desde el origen y estructuras de datos intermedias con el índice de destino, además del destino mismo. La ejecución del indexador es el desencadenador del enriquecimiento y la ingesta de datos. El resultado es un índice de búsqueda basado en el esquema de índice, que se rellena con datos de origen enriquecidos a través de conjuntos de aptitudes.  |

### <a name="3---cognitive-services"></a>3. Cognitive Services

Los enriquecimientos especificados en un conjunto de aptitudes se basan en las características Computer Vision y Language de Cognitive Services. La funcionalidad de Cognitive Services se aprovecha durante la indexación a través del conjunto de aptitudes. Un conjunto de aptitudes es una composición de aptitudes, y las aptitudes están enlazadas a las características específicas Computer Vision y Language. Para integrar Cognitive Services, deberá [adjuntar un recurso de Cognitive Services](cognitive-search-attach-cognitive-services.md) a un conjunto de aptitudes.

### <a name="4---storage-account"></a>4. Cuenta de almacenamiento

En la cuenta de Azure Storage, Azure Search crea un contenedor de blobs o tablas, según cómo configure un conjunto de aptitudes. Si los datos proceden de Azure Blob o Table Storage, ya está listo. En caso contrario, tendrá que crear una cuenta de Azure Storage. Las tablas y los objetos en Azure Storage contienen los documentos enriquecidos creados por la canalización de indexación basada en IA.

La cuenta de almacenamiento se especifica en el conjunto de aptitudes. En `api-version=2019-05-06-Preview`, la definición de un conjunto de aptitudes incluye una definición de almacén de conocimientos para que pueda proporcionar la información de la cuenta.

<a name="tools-and-apps"></a>

### <a name="5---access-and-consume"></a>5. Acceso y consumo

Una vez que el enriquecimiento existe en el almacenamiento, puede usarse cualquier herramienta o tecnología que se conecte a Azure Blob o Table Storage para explorar, analizar o consumir el contenido. La lista siguiente es un comienzo:

+ El [Explorador de Storage](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows) para ver la estructura y el contenido de los documentos enriquecidos. Considere esto como la herramienta de base de referencia para ver el contenido del almacén de conocimientos.

+ [Power BI con Power Query](https://support.office.com/article/connect-to-microsoft-azure-blob-storage-power-query-f8165faa-4589-47b1-86b6-7015b330d13e) para consultas en lenguaje natural, o use las herramientas de informes y análisis si tiene datos numéricos.

+ [Azure Data Factory](https://docs.microsoft.com/azure/data-factory/) para manipulación adicional.

+ Índice de Azure Search para búsqueda de texto completo en el contenido que ha indexado mediante la [búsqueda cognitiva](cognitive-search-concept-intro.md).

## <a name="document-persistence"></a>Persistencia de documentos

Dentro de la cuenta de almacenamiento, los enriquecimientos pueden expresarse como tablas dentro de Azure Table Storage o como objetos en Azure Blob Storage. Recuerde que los enriquecimientos, una vez almacenados, pueden usarse como origen para cargar datos en otras las bases de datos y herramientas.

+ Table Storage es útil cuando quiere una representación compatible con esquemas de los datos en formato tabular. Si quiere cambiar la forma o recombinar elementos de nuevas maneras, Table Storage le ofrece la granularidad necesaria.

+ Blob Storage crea una representación JSON exhaustiva de cada documento. Puede usar ambas opciones de almacenamiento en un conjunto de aptitudes para obtener una gama completa de expresiones.

+ Azure Search conserva el contenido en un índice. Si su escenario no se relaciona con búsquedas, por ejemplo, si su objetivo es el análisis en otra herramienta, puede eliminar el índice que crea la canalización. Pero también puede conservar el índice y usar una herramienta integrada, como el [Explorador de búsqueda](search-explorer.md) como tercer medio (detrás del Explorador de Storage y la aplicación de análisis) para interactuar con el contenido.

Junto con el contenido de los documentos, los documentos enriquecidos incluyen los metadatos de la versión del conjunto de aptitudes que generó los enriquecimientos.  

## <a name="inside-a-knowledge-store"></a>Dentro de un almacén de conocimientos

El almacén de conocimientos consta de una caché de anotación y proyecciones. El servicio usa la *caché* internamente para almacenar en caché los resultados de las aptitudes y hacer un seguimiento de los cambios. Una *proyección* define el esquema y la estructura de los enriquecimientos que coinciden con el uso previsto. Hay una memoria caché por cada almacén de conocimientos, pero varias proyecciones. 

La memoria caché es siempre un contenedor de blobs, pero las proyecciones pueden articularse como tablas u objetos:

+ Como objeto, la proyección se asigna a Blob Storage, donde la proyección se guarda en un contenedor, en el cual se encuentran los objetos o las representaciones jerárquicas en JSON para escenarios como una canalización de ciencia de datos.

+ Como tabla, la proyección se asigna a Table Storage. Una representación tabular conserva las relaciones para escenarios como análisis de datos o exportación como dataframes para aprendizaje automático. Las proyecciones enriquecidas luego pueden importarse fácilmente a otros almacenes de datos. 

Puede crear varias proyecciones en un almacén de conocimientos para dar cabida a varias circunscripciones de su organización. Un desarrollador podría necesitar acceso a la representación JSON completa de un documento enriquecido, mientras que los científicos o analistas de datos podrían querer estructuras de datos granulares o modulares formadas por su conjunto de aptitudes.

Por ejemplo, si uno de los objetivos del proceso de enriquecimiento consiste en crear también un conjunto de datos que se use para entrenar un modelo, proyectar los datos en el almacén de objetos sería una forma de usar los datos en las canalizaciones de ciencia de datos. Como alternativa, si quiere crear un panel rápido de Power BI basado en los documentos enriquecidos, la proyección tabular funcionaría correctamente.

<!---
## Data lifecycle and billing

Each time you run the indexer, the cache in Azure storage is updated if the skillset definition or underlying source data has changed. As input documents are edited or deleted, changes are propagated through the annotation cache to the projections, ensuring that your projected data is a current representation of your inputs at the end of the indexer run. 

Generally speaking, pipeline processing can be an all-or-nothing operation, but Azure Search can process incremental changes, which saves you time and money.

If a document is new or updated, all skills are run. If only the skillset changes, reprocessing is scoped to just those skills and documents affected by your edit.

### Changes to a skillset
Suppose that you have a pipeline composed of multiple skills, operating over a large body of static data (for example, scanned documents), that takes 8 hours and costs $200 to create the knowledge store. Now suppose you need to tweak one of the skills in the skillset. Rather than starting over, Azure Search can determine which skill is affected, and reprocess only that skill. Cached data and projections that are unaffected by the change remain intact in the knowledge store.

### Changes in the data
Scenarios can vary considerably, but let's suppose instead of static data, you have volatile data that changes between indexer invocations. Given no changes to the skillset, you are charged for processing the delta of new and modified document. The timestamp information varies by data source, but for illustration, in a Blob container, Azure Search looks at the `lastmodified` date to determine which blobs need to be ingested.

> [!Note]
> While you can edit the data in the projections, any edits will be overwritten on the next pipeline invocation, assuming the document in source data is updated. 

### Deletions

Although Azure Search creates and updates structures and content in Azure storage, it does not delete them. Projections and cached documents continue to exist even when the skillset is deleted. As the owner of the storage account, you should delete a projection if it is no longer needed. 

### Tips for development

+ Start small with a representative sample of your data as you make significant changes to skillset composition. As your design finalizes, you can slowly add more data during later-stage development, and then roll in the entire data set when you are comfortable with the pipeline composition.

+ Retain control over indexer invocation. Indexers can run on a schedule, which is helpful for solutions that are rolled into production, but less helpful if you are actively developing your pipeline. During development, avoid schedules so that you don’t lose track of cache or projection state. Once your solution is in production and skillset composition is static, you can put the indexer on a schedule to pick up routine changes in the external source data. 

-->

## <a name="where-do-i-start"></a>¿Por dónde empiezo?

Para fines de aprendizaje se recomienda el servicio Gratis, pero tenga en cuenta que el número de transacciones gratis se limita a 20 documentos al día, por suscripción.

Al usar varios servicios, cree todos los servicios en la misma región para obtener el mejor rendimiento y minimizar los costos. No se cobra el ancho de banda de los datos de entrada ni los datos de salida que van a otro servicio en la misma región.

**Paso 1: [Creación de un recurso de Azure Search](search-create-service-portal.md)** 

**Paso 2: [Creación de una cuenta de Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=azure-portal)** 

**Paso 3: [Creación de un recurso de Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account)** 

**Paso 4: [Introducción al portal](cognitive-search-quickstart-blob.md) o [introducción a datos de ejemplo con REST y Postman](knowledge-store-howto.md)** 

Puede usar REST `api-version=2019-05-06-Preview` para construir una canalización basada en IA que incluya Knowledge Store. En la API más reciente en versión preliminar, el objeto Skillset proporciona la definición `knowledgeStore`.

## <a name="takeaways"></a>Puntos clave

Knowledge Store ofrece diversas ventajas, entre ellas, permite el uso de los documentos enriquecidos en escenarios distintos de búsquedas, el control de los costos y la administración del desfase en el proceso de enriquecimiento. Todas estas características están disponibles para usarse con solo agregar una cuenta de almacenamiento al conjunto de aptitudes y usar el lenguaje de expresiones actualizado, como se describe en [Introducción a Knowledge Store](knowledge-store-howto.md). 

## <a name="next-steps"></a>Pasos siguientes

El enfoque más sencillo para crear documentos enriquecidos es a través del asistente para **importar datos**.

> [!div class="nextstepaction"]
> [Inicio rápido: Prueba de Cognitive Search en un tutorial del portal](cognitive-search-quickstart-blob.md)
