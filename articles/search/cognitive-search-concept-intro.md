---
title: 'Cognitive Search, extracción de datos y procesamiento de IA de lenguaje natural: Azure Search'
description: Extracción de contenido, procesamiento de lenguaje natural (NLP) y procesamiento de imágenes para crear contenido que se puede buscar en la indexación de Azure Search a través de aptitudes cognitivas y algoritmos de IA.
manager: cgronlun
author: HeidiSteen
services: search
ms.service: search
ms.devlang: NA
ms.topic: overview
ms.date: 05/28/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 8af927bee11d66c473707b603951fa693f6840e3
ms.sourcegitcommit: 8c49df11910a8ed8259f377217a9ffcd892ae0ae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66299030"
---
# <a name="what-is-cognitive-search-in-azure-search"></a>¿Qué es la "búsqueda cognitiva" en Azure Search?

La búsqueda cognitiva es una característica de IA en Azure Search, que se usa para extraer el texto de imágenes, blobs y otros orígenes de datos no estructurados, enriqueciendo el contenido para facilitar las búsquedas en un índice de Azure Search. La extracción y el enriquecimiento se implementan a través de *conocimientos cognitivos* adjuntados a una canalización de indexación. Los enriquecimientos por IA se admiten de las maneras siguientes: 

+ Las aptitudes del **procesamiento de lenguaje natural** incluyen [reconocimiento de entidades](cognitive-search-skill-entity-recognition.md), [detección de idioma](cognitive-search-skill-language-detection.md), [extracción de frases clave](cognitive-search-skill-keyphrases.md), manipulación de texto y [detección de opiniones](cognitive-search-skill-sentiment.md). Con estas aptitudes, un texto no estructurado puede asumir nuevas formas, asignado e a campos que se pueden buscar y filtrar en un índice.

+ Las aptitudes de **procesamiento de imágenes** incluyen [reconocimiento óptico de caracteres (OCR)](cognitive-search-skill-ocr.md) e identificación de [características visuales](cognitive-search-skill-image-analysis.md), como detección facial, interpretación de imágenes, reconocimiento de imágenes (personas famosas y puntos de referencia) o atributos como colores o la orientación de la imagen. Puede crear representaciones de texto de contenido de imágenes, que se puede buscar mediante todas las funcionalidades de consulta de Azure Search.

![Diagrama de canalización de Cognitive Search](./media/cognitive-search-intro/cogsearch-architecture.png "Cognitive search pipeline overview")

Los conocimientos cognitivos de Azure Search se basan en los modelos de Machine Learning de Cognitive Services APIs: [Computer Vision](https://docs.microsoft.com/azure/cognitive-services/computer-vision/) y [análisis de texto](https://docs.microsoft.com/azure/cognitive-services/text-analytics/overview). 

El lenguaje natural y el procesamiento de imágenes se aplican durante la fase de ingesta de datos, con resultados que formarán parte de la composición de un documento en un índice que se puede buscar en Azure Search. Los datos se obtienen como un conjunto de datos de Azure y luego se insertan a través de una canalización de indexación mediante las [aptitudes integradas](cognitive-search-predefined-skills.md) que necesite. La arquitectura es extensible, por lo que si las aptitudes integradas no son suficientes, puede crear y adjuntar [aptitudes personalizadas](cognitive-search-create-custom-skill-example.md) para integrar el procesamiento personalizado. Algunos ejemplos pueden ser un módulo de entidad personalizado o un clasificador de documentos que tiene como destino un dominio específico, como finanzas, publicaciones científicas o medicina.

> [!NOTE]
> A medida que expanda el ámbito aumentando la frecuencia de procesamiento, agregando más documentos o agregando más algoritmos de IA, tendrá que [asociar un recurso facturable de Cognitive Services](cognitive-search-attach-cognitive-services.md). Los cargos se acumulan cuando se llama a las API de Cognitive Services y para la extracción de imágenes como parte de la fase de descifrado de documentos en Azure Search. No hay ningún cargo por la extracción de texto de documentos.
>
> La ejecución de aptitudes integradas se cobra según los [precios de pago por uso de Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services/) existentes. La extracción de imágenes se cobra tal y como se describe en la [página de precios de Azure Search](https://go.microsoft.com/fwlink/?linkid=2042400).
## <a name="components-of-cognitive-search"></a>Componentes de Cognitive Search

Una canalización de búsqueda cognitiva se basa en [*indizadores* de Azure Search](search-indexer-overview.md), que rastrean los orígenes de datos y ofrecen procesamiento de índices de un extremo a otro. Las aptitudes ahora se adjuntan a los indexadores, lo que permite interceptar y enriquecer los documentos en función del conjunto de aptitudes que defina. Una vez que se indexa, puede acceder al contenido mediante solicitudes de búsqueda a través de todos los [tipos de consulta compatibles con Azure Search](search-query-overview.md).  Si es la primera vez que usa los indexadores, esta sección lo guiará a través de los pasos.

### <a name="step-1-connection-and-document-cracking-phase"></a>Paso 1: Fase de conexión y descifrado de documentos

Al comienzo de la canalización, hay texto no estructurado o contenido que no es de texto, como archivos JPEG de documentos escaneados e imágenes. Los datos deben estar en un servicio de almacenamiento de datos de Azure al que un indexador puede acceder. Los indexadores pueden "descifrar" los documentos de origen para extraer texto de los datos de origen.

![Fase de descifrado de documentos](./media/cognitive-search-intro/document-cracking-phase-blowup.png "descifrado de documentos")

 Entre los orígenes compatibles se incluyen Azure Blob Storage, Azure Table Storage, Azure SQL Database y Azure Cosmos DB. El contenido basado en texto se puede extraer de estos tipos de archivo: archivos PDF, Word, PowerPoint, archivos CSV. Para ver la lista completa, consulte los [formatos compatibles](search-howto-indexing-azure-blob-storage.md#supported-document-formats).

### <a name="step-2-cognitive-skills-and-enrichment-phase"></a>Paso 2: Fase de enriquecimiento y aptitudes cognitivas

El enriquecimiento se produce a través de *aptitudes cognitivas* que realizan operaciones atómicas. Por ejemplo, una vez que obtiene el contenido de texto de un archivo PDF, puede aplicar la detección de idioma del reconocimiento de entidades o la extracción de frases clave para generar campos nuevos en el índice que no están disponibles de manera nativa en el origen. La colección completa de aptitudes que usa en la canalización se conoce como un *conjunto de aptitudes*.  

![Fase de enriquecimiento](./media/cognitive-search-intro/enrichment-phase-blowup.png "fase de enriquecimiento")

Un conjunto de aptitudes se basa en las [aptitudes cognitivas predefinidas](cognitive-search-predefined-skills.md) o en las [aptitudes predeterminadas](cognitive-search-create-custom-skill-example.md) que proporciona y conecta con el conjunto de aptitudes. Un conjunto de aptitudes puede ser mínimo o altamente complejo y no solo determina el tipo de procesamiento, sino también el orden de las operaciones. Un conjunto de aptitudes más las asignaciones de campos que se definen como parte de un indexador especifica completamente la canalización de enriquecimiento. Para más información sobre cómo unir todos estos elementos, consulte el artículo sobre cómo [definir un conjunto de aptitudes](cognitive-search-defining-skillset.md).

Internamente, la canalización genera una colección de documentos enriquecidos. Puede decidir qué elementos de los documentos enriquecidos se deben asignar a los campos del índice de búsqueda que se pueden indexar. Por ejemplo, si aplicara las aptitudes de reconocimiento de entidades y de extracción de frases clave, esos campos nuevos se volverían parte del documento enriquecido y se podrían asignar a los campos del índice. Consulte las [anotaciones](cognitive-search-concept-annotations-syntax.md) para más información sobre las formaciones de entrada/salida.

#### <a name="add-a-knowledgestore-element-to-save-enrichments"></a>Adición de un elemento knowledgeStore para guardar enriquecimientos

El elemento [api-version=2019-05-06 de REST de Search Service](search-api-preview.md) extiende los conjuntos de aptitudes con una definición de knowledgeStore que proporciona una conexión al almacenamiento de Azure y proyecciones que describen cómo se almacenan los enriquecimientos. 

Cuando agrega una instancia de Knowledge Store a un conjunto de aptitudes, puede proyectar una representación de sus enriquecimientos para los escenarios que no sean de búsqueda de texto completo. Para más información, consulte [¿Qué es Knowledge Store?](knowledge-store-concept-intro.md)

### <a name="step-3-search-index-and-query-based-access"></a>Paso 3: Índice de búsqueda y acceso basado en consulta

Cuando finaliza el procesamiento, obtiene un índice de búsqueda que consta de documentos enriquecidos con texto totalmente apto para búsquedas en Azure Search. Para acceder al contenido enriquecido que la canalización genera, los desarrolladores y usuarios [consultan el índice](search-query-overview.md). 

![Icono de índice con búsqueda](./media/cognitive-search-intro/search-phase-blowup.png "Icono de índice con búsqueda")

El índice es como cualquier otro índice que pudiera crear para Azure Search: puede complementarlo con analizadores personalizados, invocar consultas de búsqueda aproximada, agregar la búsqueda filtrada o experimentar con perfiles de puntuación para volver a dar forma a los resultados de la búsqueda.

Los índices se generan a partir de un esquema de índice que define los campos, atributos y otras construcciones adjuntas a un índice específico, como los perfiles de puntuación y las asignaciones de sinónimos. Una vez que se define y rellena un índice, puede indexar de manera incremental para seleccionar documentos de origen nuevos y actualizados. Algunas de las determinaciones requieren recompilar completamente el índice. Debe usar un conjunto de datos pequeño hasta que el diseño del esquema sea estable. Para más información, consulte el artículo sobre cómo [recompilar un índice](search-howto-reindex.md).

<a name="feature-concepts"></a>

## <a name="key-features-and-concepts"></a>Características y conceptos principales

| Concepto | DESCRIPCIÓN| Vínculos |
|---------|------------|-------|
| Conjunto de aptitudes | Recurso con nombre de nivel superior que contiene una colección de aptitudes. Un conjunto de aptitudes es la canalización de enriquecimiento. Se invoca durante la indexación por parte del indexador. | [Define a skillset](cognitive-search-defining-skillset.md) (Definición de un conjunto de aptitudes) |
| Aptitud cognitiva | Transformación atómica en una canalización de enriquecimiento. Normalmente, se trata de un componente que extrae o deduce la estructura y, por tanto, aumenta el reconocimiento de los datos de entrada. Casi siempre, la salida se basa en el texto y el procesamiento es el procesamiento de lenguaje natural o el procesamiento de imagen que extrae o genera texto a partir de entradas de imágenes. La salida de una aptitud se puede asignar a un campo en un índice o usar como entrada para el enriquecimiento de nivel inferior. Microsoft predefinida y proporciona una aptitud, o bien puede ser personalizada, es decir, el usuario puede crearla e implementarla. | [Aptitudes predefinidas](cognitive-search-predefined-skills.md) |
| Extracción de datos | Aunque abarca una amplia gama de procesamientos, en relación con la búsqueda cognitiva, la aptitud Reconocimiento de entidades, normalmente, se usa para extraer los datos (una entidad) de un origen que no proporciona esa información de forma nativa. | [Aptitud cognitiva Reconocimiento de entidades con nombre](cognitive-search-skill-entity-recognition.md)| 
| Procesamiento de imágenes | Deduce texto a partir de una imagen, como la capacidad de reconocer un punto de referencia, o bien extrae texto a partir de una imagen. Algunos ejemplos comunes incluyen OCR para levantar caracteres de un archivo de documento escaneado (JPEG) o reconocer el nombre de una calle en una fotografía que incluye un letrero con el nombre. | [Image Analysis Skill](cognitive-search-skill-image-analysis.md) (Aptitud de análisis de imágenes) o [OCR Skill](cognitive-search-skill-ocr.md) (Aptitud de OCR)
| Procesamiento de lenguaje natural | Procesamiento de texto para obtener conclusiones e información sobre entradas de texto. La detección de idioma, el análisis de sentimiento y la extracción de frases clave son aptitudes que se enmarcan en el procesamiento de lenguaje natural.  | [Key Phrase Extraction Skill](cognitive-search-skill-keyphrases.md) (Aptitud de Extracción de frases clave), [Language Detection Skill](cognitive-search-skill-language-detection.md) (Aptitud de Detección de idioma), [Sentiment Analysis Skill](cognitive-search-skill-sentiment.md) (Aptitud de Análisis de sentimiento) |
| Descifrado de documentos | Proceso de extraer o crear contenido de texto a partir de orígenes que no son de texto durante el indexado. El reconocimiento óptico de caracteres (OCR) es un ejemplo pero, en general, se refiere a la funcionalidad de indexador principal cuando el indexador extrae contenido los archivos de aplicación. El origen de datos que proporciona la ubicación del archivo de origen y la definición del indexador que brinda las asignaciones de campo son factores clave en el descifrado de documentos. | Consulte [Indexers](search-indexer-overview.md) (Indexador) |
| Formas | Consolide los fragmentos de texto en una estructura más grande o, a la inversa, desglose fragmentos de texto de gran tamaño a un tamaño que se pueda administrar para un procesamiento adicional. | [Shaper Skill](cognitive-search-skill-shaper.md) (Aptitud de conformador), [Text Merger Skill](cognitive-search-skill-textmerger.md) (Aptitud de combinación de texto), [Text Split Skill](cognitive-search-skill-textsplit.md) (Aptitud de división de texto) |
| Documentos enriquecidos | Una estructura interna transitoria, generada durante el procesamiento, con el resultado final reflejado en un índice de búsqueda. Un conjunto de aptitudes determina qué enriquecimientos se llevan a cabo. Las asignaciones de campos determinan los elementos de datos que se agregan al índice. Opcionalmente, puede crear un almacén de conocimientos para conservar y explorar documentos enriquecidos mediante herramientas como Explorador de Storage, Power BI o cualquier otra herramienta que se conecte a Azure Blob Storage. | Consulte [Knowledge Store (versión preliminar)](knowledge-store-concept-intro.md). |
| Indexer |  Un rastreador (crawler) que extrae datos y metadatos utilizables en búsquedas de un origen de datos externo y rellena un índice basado en las asignaciones de un campo a otro entre el índice y su origen de datos para el descifrado de documentos. En el caso de Cognitive Search, el indexador invoca un conjunto de aptitudes y contiene las asignaciones de campos que asocian la salida de enriquecimiento con los campos de destino del índice. La definición del indexador contiene todas las instrucciones y referencias de las operaciones de la canalización y esta se invoca cuando se ejecuta el indexador. | [Indexadores](search-indexer-overview.md) |
| Origen de datos  | Un objeto que un indexador usa para conectarse a un origen de datos externo de tipos compatibles en Azure. | Consulte [Indexers](search-indexer-overview.md) (Indexador) |
| Índice | Índice de búsqueda persistente en Azure Search; se crea a partir de un esquema de índice que define el uso y estructura de los campos. | [Indexes in Azure Search](search-what-is-an-index.md) (Índices en Azure Search) | 

<a name="where-do-i-start"></a>

## <a name="where-do-i-start"></a>¿Por dónde empiezo?

**Paso 1: [Creación de un recurso de Azure Search](search-create-service-portal.md)** 

**Paso 2: Pruebe algunos Inicios rápidos y ejemplos de experiencia práctica**

+ [Inicio rápido (portal)](cognitive-search-quickstart-blob.md)
+ [Tutorial (solicitudes HTTP)](cognitive-search-tutorial-blob.md)
+ [Aptitudes personalizadas de ejemplo (C#)](cognitive-search-create-custom-skill-example.md)

Para fines de aprendizaje se recomienda el servicio Gratis, pero tenga en cuenta que el número de transacciones gratis se limita a 20 documentos al día. Para ejecutar el inicio rápido y el tutorial en un día, use un conjunto de archivos más pequeño (10 documentos) para que pueda adaptarlo a ambos ejercicios.

**Paso 3: Revise la API**

Puede usar la versión `api-version=2019-05-06` de REST en solicitudes o el SDK. de NET. 

Este paso usa las API REST para crear una solución de búsqueda cognitiva. Solo se agregan o extienden dos API para Cognitive Search. Otras API tienen la misma sintaxis que las versiones con disponibilidad general.

| API DE REST | DESCRIPCIÓN |
|-----|-------------|
| [Create Data Source](https://docs.microsoft.com/rest/api/searchservice/create-data-source) (Creación de un origen de datos)  | Recurso que identifica un origen de datos externo que proporciona los datos de origen que se usan para crear documentos enriquecidos.  |
| [Create Skillset (api-version=2019-05-06)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)  | Recurso que coordina el uso de las [aptitudes predefinidas](cognitive-search-predefined-skills.md) y las [aptitudes cognitivas personalizadas](cognitive-search-custom-skill-interface.md) que se usan en una canalización de enriquecimiento durante el indexado. |
| [Crear índice](https://docs.microsoft.com/rest/api/searchservice/create-index)  | Esquema que expresa un índice de Azure Search. Los campos del índice se asignan a los campos de los datos de origen o a los campos fabricados durante la fase de enriquecimiento (por ejemplo, un campo para los nombres de organización creado por el reconocimiento de entidades). |
| [Create Indexer (api-version=2019-05-06)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)  | Recurso que define los componentes usados durante la indexación, los que incluyen un origen de datos, un conjunto de aptitudes, asociaciones de campos desde el origen y estructuras de datos intermedias con el índice de destino, además del destino mismo. La ejecución del indexador es el desencadenador del enriquecimiento y la ingesta de datos. El resultado es un índice de búsqueda basado en el esquema de índice, que se rellena con datos de origen enriquecidos a través de conjuntos de aptitudes.  |

**Lista de comprobación: Un flujo de trabajo típico**

1. Subdivida los datos de origen de Azure en un ejemplo representativo. La indexación toma tiempo, por lo que empiece con un conjunto de datos representativo y pequeño para ir compilando el índice de manera incremental a medida que la solución madura.

1. Cree un [objeto de origen de datos](https://docs.microsoft.com/rest/api/searchservice/create-data-source) en Azure Search para proporciona una cadena de conexión para la recuperación de datos.

1. Cree un [conjunto de aptitudes](https://docs.microsoft.com/rest/api/searchservice/create-skillset) con pasos de enriquecimiento.

1. Defina el [esquema de índice](https://docs.microsoft.com/rest/api/searchservice/create-index). La colección *Fields* (Campos) incluye campos de los datos de origen. También debe simular campos adicionales para contener valores generados para el contenido creado durante el enriquecimiento.

1. Defina el [indexador](https://docs.microsoft.com/rest/api/searchservice/create-skillset) que hace referencia al origen de datos, conjunto de aptitudes e índice.

1. Dentro del indexador, agregue *outputFieldMappings*. En esta sección, la salida se asigna desde el conjunto de aptitudes (en el paso 3) a los campos de entrada del esquema de índice (en el paso 4).

1. Envíe la solicitud *Create Indexer* (Crear indexador) que acaba de crear (una solicitud POST con una definición de indexador en el cuerpo de la solicitud) para expresar el indexador en Azure Search. Este paso se refiere a cómo ejecuta el indexador mediante la invocación de la canalización.

1. Ejecute consultas para evaluar los resultados y modifique el código para actualizar la configuración del indexador, el esquema o los conjuntos de aptitudes.

1. [Restablezca el indexador](search-howto-reindex.md) antes de recompilar la canalización.

Para más información sobre preguntas o problemas específicos, consulte las [sugerencias para solucionar problemas](cognitive-search-concept-troubleshooting.md).

## <a name="next-steps"></a>Pasos siguientes

+ [Documentación de Cognitive Search](cognitive-search-resources-documentation.md)
+ [Inicio rápido: Prueba de Cognitive Search en un tutorial del portal](cognitive-search-quickstart-blob.md)
+ [Tutorial: Aprendizaje de Cognitive Search API](cognitive-search-tutorial-blob.md)
+ [Información general de Knowledge Store](knowledge-store-concept-intro.md)
+ [Tutorial de Knowledge Store](knowledge-store-howto.md)
