---
title: Introducción a los almacenes de conocimiento (versión preliminar)
titleSuffix: Azure Cognitive Search
description: Envíe documentos enriquecidos a Azure Storage, donde puede ver, cambiar la forma y consumir documentos enriquecidos en Azure Cognitive Search y en otras aplicaciones.
author: HeidiSteen
manager: nitinme
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 82f8606f4b4201833667347d3ed16fdd73f70a36
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2019
ms.locfileid: "72790369"
---
# <a name="introduction-to-knowledge-stores-in-azure-cognitive-search"></a>Introducción a los almacenes de conocimiento de Azure Cognitive Search

> [!Note]
> Knowledge Store se encuentra en versión preliminar y no está pensado para su uso en producción. En la [API REST, versión 2019-05-06-Preview](search-api-preview.md) se proporciona esta característica. Por el momento, no hay compatibilidad con .NET SDK.
>

El almacén de conocimiento es una característica de Azure Cognitive Search que conserva la salida de una [canalización de enriquecimiento con inteligencia artificial](cognitive-search-concept-intro.md) para su análisis posterior o para otro procesamiento descendente. Un *documento enriquecido* es la salida de una canalización creada a partir del contenido que se ha extraído, estructurado y analizado mediante procesos de inteligencia artificial. En una canalización estándar de inteligencia artificial, los documentos enriquecidos son transitorios, solo se usan durante la indexación y después se descartan. Los documentos enriquecidos se conservan mediante el almacén de conocimiento. 

Si ha usado las aptitudes cognitivas con Azure Cognitive Search en el pasado, ya sabe que los *conjuntos de aptitudes* mueven un documento a través de una secuencia de enriquecimientos. El resultado puede ser un índice de búsqueda o (una novedad de esta versión preliminar) proyecciones en un almacén de conocimientos. Las dos salidas, el índice de búsqueda y el almacén de conocimiento, comparten el mismo contenido, pero se almacenan y se usan de maneras muy distintas.

Físicamente, un almacén de conocimiento es como [Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-account-overview), ya sea Azure Table Storage, Azure Blob Storage o ambos. Cualquier herramienta o proceso que pueda conectarse a Azure Storage puede consumir el contenido de un almacén de conocimiento.

![Almacén de conocimiento en un diagrama de canalización](./media/knowledge-store-concept-intro/knowledge-store-concept-intro.svg "Almacén de conocimiento en un diagrama de canalización")

Las proyecciones son el mecanismo para estructurar los datos en un almacén de conocimiento. Por ejemplo, a través de las proyecciones, puede elegir si la salida se guarda como un solo blob o como una colección de tablas relacionadas. 

Para usar Knowledge Store, agregue un elemento `knowledgeStore` a un conjunto de aptitudes que defina las operaciones escalonadas en una canalización de indexación. Durante la ejecución, Azure Cognitive Search crea un espacio en la cuenta de Azure Storage y proyecta los documentos enriquecidos como blobs o en tablas, según la configuración.

## <a name="benefits-of-knowledge-store"></a>Ventajas de Knowledge Store

Un almacén de conocimientos le brinda estructura, contexto y contenido real, recopilados a partir de archivos de datos no estructurados y semiestructurados, como blobs, archivos de imagen que se han sometido a análisis o, incluso, datos estructurados cuya forma ha cambiado. En una [guía paso a paso](knowledge-store-howto.md) escrita para esta versión preliminar, puede ver de primera mano cómo un documento JSON denso se particiona en subestructuras, se reconstituye en nuevas estructuras y, además, se pone a disposición para procesos de nivel inferior, como cargas de trabajo de ciencia de datos y aprendizaje automático.

Aunque es útil ver lo que puede producir una canalización de enriquecimiento con inteligencia artificial, la verdadera ventaja del almacén de conocimiento radica en la capacidad de remodelar los datos. Puede comenzar con un conjunto de aptitudes básico y, luego, iterarlo para agregar niveles crecientes de estructura que luego se pueden combinar en nuevas estructuras, y que pueden consumirse en otras aplicaciones, además de Azure Cognitive Search.

Entre las ventajas de Knowledge Store, se incluyen las siguientes:

+ Consumir documentos enriquecidos en [herramientas de informes y análisis](#tools-and-apps), además de buscar. Power BI con Power Query es una opción atractiva, pero cualquier herramienta o aplicación que se pueda conectar a Azure Storage puede beneficiarse de un almacén de conocimientos que cree.

+ Refinar una canalización de indexación de IA durante la depuración de pasos y definiciones de conjuntos de aptitudes. Un almacén de conocimientos le muestra el producto de la definición de un conjunto de aptitudes en una canalización de indexación de IA. Puede usar estos resultados para diseñar un mejor conjunto de aptitudes, ya que puede ver exactamente la apariencia de los enriquecimientos. Puede usar el [Explorador de Storage](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows) en Azure Storage para ver el contenido de un almacén de conocimientos.

+ Cambiar la forma de los datos. El cambio de formas se codifica en conjuntos de aptitudes, pero la idea es que ahora un conjunto de aptitudes puede proporcionar esta funcionalidad. La [aptitud Conformador](cognitive-search-skill-shaper.md) de Azure Cognitive Search se ha ampliado para dar cabida a esta tarea. Cambiar la forma le permite definir una proyección que se alinee con el uso previsto de los datos, al tiempo que conserva las relaciones.

> [!Note]
> ¿No está familiarizado con el enriquecimiento con inteligencia artificial mediante Cognitive Services? Azure Cognitive Search se integra en las características Vision y Language de Cognitive Services para extraer y enriquecer los datos de origen mediante el reconocimiento óptico de caracteres (OCR) en archivos de imágenes, el reconocimiento de entidades y la extracción de frases clave de archivos de texto, etc. Para más información, consulte [Enriquecimiento con inteligencia artificial en Azure Cognitive Search](cognitive-search-concept-intro.md).

## <a name="creating-a-knowledge-store"></a>Creación de un almacén de conocimiento

Un almacén de conocimiento forma parte de un [conjunto de aptitudes](cognitive-search-working-with-skillsets.md) que, a su vez, forma parte de un [indexador](search-indexer-overview.md). 

En esta versión preliminar, puede crear un almacén de conocimiento mediante la API REST y `api-version=2019-05-06-Preview`, o mediante el Asistente para la **importación de datos** en el portal.

### <a name="json-representation-of-a-knowledge-store"></a>Representación JSON de un almacén de conocimiento

El siguiente código JSON especifica un `knowledgeStore`, que forma parte de un conjunto de aptitudes, al que invoca un indexador (no se muestra). Si ya está familiarizado con el enriquecimiento con inteligencia artificial, un conjunto de aptitudes determina la creación, organización y sustento de cada documento enriquecido. Un conjunto de aptitudes debe contener al menos una aptitud y, lo más probable, es que se trate de una aptitud Conformador si está modulando estructuras de datos.

Un `knowledgeStore` consta de una conexión y de proyecciones. 

+ La conexión es a una cuenta de almacenamiento en la misma región que Azure Search. 

+ Las proyecciones son pares de tablas-objetos. `Tables` define la expresión física de los documentos enriquecidos en Azure Table Storage. `Objects` define los objetos físicos en Azure Blob Storage.

```json
{
  "name": "my-new-skillset",
  "description": "Example showing knowledgeStore placement in a skillset.",
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
               
            ]      
        },
        { 
            "tables": [ 
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

### <a name="sources-of-data-for-a-knowledge-store"></a>Orígenes de datos para un almacén de conocimiento

Si un almacén de conocimiento es la salida de una canalización de enriquecimiento con inteligencia artificial, ¿cuáles son las entradas? Los datos originales que desea extraer, enriquecer y guardar en última instancia en un almacén de conocimiento pueden originarse en cualquier origen de datos de Azure compatible con los indexadores de búsqueda: 

* [Azure Cosmos DB](search-howto-index-cosmosdb.md)

* [Almacenamiento de blobs de Azure](search-howto-indexing-azure-blob-storage.md)

* [Azure Table Storage](search-howto-indexing-azure-tables.md)

* [SQL de Azure](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)

Los indexadores y aptitudes que ha creado extraen y enriquecen o transforman este contenido como parte de una carga de trabajo de indexación y, después, guardan los resultados en un almacén de conocimiento.

### <a name="rest-apis-used-in-creation-of-a-knowledge-store"></a>API REST usadas en la creación de un almacén de conocimiento

Solo dos API tienen las extensiones necesarias para crear un almacén de conocimiento (Create Skillset y Create Indexer). Otras API se usan tal cual.

| Object | API DE REST | DESCRIPCIÓN |
|--------|----------|-------------|
| origen de datos | [Create Data Source](https://docs.microsoft.com/rest/api/searchservice/create-data-source) (Creación de un origen de datos)  | Recurso que identifica un origen de datos de Azure externo que proporciona los datos de origen que se usan para crear documentos enriquecidos.  |
| conjunto de aptitudes | [Crear conjunto de aptitudes (api-version=2019-05-06-Preview)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)  | Recurso que coordina el uso de las [aptitudes integradas](cognitive-search-predefined-skills.md) y las [aptitudes cognitivas personalizadas](cognitive-search-custom-skill-interface.md) que se usan en una canalización de enriquecimiento durante el indexado. Una aptitud tiene una definición de `knowledgeStore` como elemento secundario. |
| index | [Crear índice](https://docs.microsoft.com/rest/api/searchservice/create-index)  | Esquema que expresa un índice de búsqueda. Los campos del índice se asignan a los campos de los datos de origen o a los campos fabricados durante la fase de enriquecimiento (por ejemplo, un campo para los nombres de organización creado por el reconocimiento de entidades). |
| indexador | [Create Indexer (api-version=2019-05-06)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)  | Recurso que define los componentes usados durante la indexación, los que incluyen un origen de datos, un conjunto de aptitudes, asociaciones de campos desde el origen y estructuras de datos intermedias con el índice de destino, además del destino mismo. La ejecución del indexador es el desencadenador del enriquecimiento y la ingesta de datos. El resultado es un índice de búsqueda basado en el esquema de índice, que se rellena con datos de origen enriquecidos a través de conjuntos de aptitudes.  |

### <a name="physical-composition-of-a-knowledge-store"></a>Composición física de un almacén de conocimiento

 Una *proyección*, que es un elemento de una definición de `knowledgeStore`, que articula el esquema y la estructura de salida para que coincida con el uso previsto. Puede definir varias proyecciones si tiene aplicaciones que consuman los datos en diferentes formatos y formas. 

Las proyecciones se pueden articular como objetos o tablas:

+ Como objeto, la proyección se asigna a Blob Storage, donde la proyección se guarda en un contenedor, en el cual se encuentran los objetos o las representaciones jerárquicas en JSON para escenarios como una canalización de ciencia de datos.

+ Como tabla, la proyección se asigna a Table Storage. Una representación tabular conserva las relaciones para escenarios como análisis de datos o exportación como dataframes para aprendizaje automático. Las proyecciones enriquecidas luego pueden importarse fácilmente a otros almacenes de datos. 

Puede crear varias proyecciones en un almacén de conocimientos para dar cabida a varias circunscripciones de su organización. Un desarrollador podría necesitar acceso a la representación JSON completa de un documento enriquecido, mientras que los científicos o analistas de datos podrían querer estructuras de datos granulares o modulares formadas por su conjunto de aptitudes.

Por ejemplo, si uno de los objetivos del proceso de enriquecimiento consiste en crear también un conjunto de datos que se use para entrenar un modelo, proyectar los datos en el almacén de objetos sería una forma de usar los datos en las canalizaciones de ciencia de datos. Como alternativa, si quiere crear un panel rápido de Power BI basado en los documentos enriquecidos, la proyección tabular funcionaría correctamente.

<a name="tools-and-apps"></a>

## <a name="connecting-with-tools-and-apps"></a>Conexión con herramientas y aplicaciones

Una vez que el enriquecimiento existe en el almacenamiento, puede usarse cualquier herramienta o tecnología que se conecte a Azure Blob o Table Storage para explorar, analizar o consumir el contenido. La lista siguiente es un comienzo:

+ El [Explorador de Storage](knowledge-store-view-storage-explorer.md) para ver la estructura y el contenido de los documentos enriquecidos. Considere esto como la herramienta de base de referencia para ver el contenido del almacén de conocimientos.

+ [Power BI](knowledge-store-connect-power-bi.md) para las herramientas de informes y análisis si tiene datos numéricos.

+ [Azure Data Factory](https://docs.microsoft.com/azure/data-factory/) para manipulación adicional.


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

<!-- ## Where do I start?

We recommend the Free service for learning purposes, but be aware that the number of free transactions is limited to 20 documents per day, per subscription.

When using multiple services, create all of your services in the same region for best performance and to minimize costs. You are not charged for bandwidth for inbound data or outbound data that goes to another service in the same region.

**Step 1: [Create an Azure Cognitive Search resource](search-create-service-portal.md)** 

**Step 2: [Create an Azure storage account](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=azure-portal)** 

**Step 3: [Create a Cognitive Services resource](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account)** 

**Step 4: [Get started with the portal](cognitive-search-quickstart-blob.md) - or - [Get started with sample data using REST and Postman](knowledge-store-howto.md)** 

You can use REST `api-version=2019-05-06-Preview` to construct an AI-based pipeline that includes knowledge store. In the newest preview API, the Skillset object provides the `knowledgeStore` definition. -->

## <a name="next-steps"></a>Pasos siguientes

El almacén de conocimiento ofrece persistencia de documentos enriquecidos, lo cual resulta útil al diseñar un conjunto de aptitudes o durante la creación de nuevas estructuras y contenido para su consumo por parte de cualquier aplicación cliente capaz de acceder a una cuenta de Azure Storage.

El enfoque más sencillo para crear documentos enriquecidos es mediante el Asistente para la **importación de datos**, pero también puede usar Postman y la API REST, lo que resulta más útil si desea obtener información sobre cómo se crean y se hace referencia a los objetos.

> [!div class="nextstepaction"]
> [Creación de un almacén de conocimiento mediante el portal](knowledge-store-create-portal.md)
> [Creación de un almacén de conocimiento mediante Postman y la API REST](knowledge-store-create-rest.md)
