---
title: 'Cognitive Search, extracción de datos y procesamiento de IA de lenguaje natural: Azure Search'
description: Extracción de contenido, procesamiento de lenguaje natural (NLP) y procesamiento de imágenes para crear contenido que se puede buscar en la indexación de Azure Search a través de aptitudes cognitivas y algoritmos de IA.
manager: cgronlun
author: HeidiSteen
services: search
ms.service: search
ms.devlang: NA
ms.topic: overview
ms.date: 03/17/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 187e40716bc55f71623ef758722eb58d27651d4d
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2019
ms.locfileid: "58882511"
---
# <a name="what-is-cognitive-search-in-azure-search"></a>¿Qué es la "búsqueda cognitiva" en Azure Search?

La búsqueda cognitiva es una característica de IA en Azure Search, que se usa para extraer el texto de imágenes, blobs y otros orígenes de datos no estructurados, enriqueciendo el contenido para facilitar las búsquedas en un índice de Azure Search. La extracción y el enriquecimiento se implementan a través de *conocimientos cognitivos* adjuntados a una canalización de indexación. Los enriquecimientos por IA se admiten de las maneras siguientes: 

+ Las aptitudes del **procesamiento de lenguaje natural** incluyen [reconocimiento de entidades](cognitive-search-skill-entity-recognition.md), [detección de idioma](cognitive-search-skill-language-detection.md), [extracción de frases clave](cognitive-search-skill-keyphrases.md), manipulación de texto y [detección de opiniones](cognitive-search-skill-sentiment.md). Con estas aptitudes, un texto no estructurado puede asumir nuevas formas, asignado e a campos que se pueden buscar y filtrar en un índice.

+ Las aptitudes de **procesamiento de imágenes** incluyen [reconocimiento óptico de caracteres (OCR)](cognitive-search-skill-ocr.md) e identificación de [características visuales](cognitive-search-skill-image-analysis.md), como detección facial, interpretación de imágenes, reconocimiento de imágenes (personas famosas y puntos de referencia) o atributos como colores o la orientación de la imagen. Puede crear representaciones de texto de contenido de imágenes, que se puede buscar mediante todas las funcionalidades de consulta de Azure Search.

![Diagrama de la canalización de Cognitive Search](./media/cognitive-search-intro/cogsearch-architecture.png "Diagrama de la canalización de Cognitive Search")

Los conocimientos cognitivos de Azure Search se basan en los modelos de Machine Learning de Cognitive Services APIs: [Computer Vision](https://docs.microsoft.com/azure/cognitive-services/computer-vision/) y [análisis de texto](https://docs.microsoft.com/azure/cognitive-services/text-analytics/overview). 

El lenguaje natural y el procesamiento de imágenes se aplican durante la fase de ingesta de datos, con resultados que formarán parte de la composición de un documento en un índice que se puede buscar en Azure Search. Los datos se obtienen como un conjunto de datos de Azure y luego se insertan a través de una canalización de indexación mediante las [aptitudes integradas](cognitive-search-predefined-skills.md) que necesite. La arquitectura es extensible, por lo que si las aptitudes integradas no son suficientes, puede crear y adjuntar [aptitudes personalizadas](cognitive-search-create-custom-skill-example.md) para integrar el procesamiento personalizado. Algunos ejemplos pueden ser un módulo de entidad personalizado o un clasificador de documentos que tiene como destino un dominio específico, como finanzas, publicaciones científicas o medicina.

> [!NOTE]
> A partir del 21 de diciembre de 2018, podrá [asociar un recurso de Cognitive Services](cognitive-search-attach-cognitive-services.md) con un conjunto de aptitudes de Azure Search. Esto nos permite empezar a cobrar por la ejecución del conjunto de aptitudes. En esta fecha, también empezamos a cobrar por la extracción de imágenes como parte de la fase de descifrado de documentos. La extracción de texto de documentos continuará ofreciéndose sin costo adicional.
>
> La ejecución de aptitudes integradas se cobra con Cognitive Services, y se factura con el [precio de pago por uso](https://azure.microsoft.com/pricing/details/cognitive-services/) existente. La extracción de imágenes se cobra con Azure Search, que se factura actualmente al precio de versión preliminar, según se describe en la [página de precios de Azure Search](https://go.microsoft.com/fwlink/?linkid=2042400).

## <a name="components-of-cognitive-search"></a>Componentes de Cognitive Search

Cognitive Search es una característica en vista previa de [Azure Search](search-what-is-azure-search.md).

La canalización de Cognitive Search se basa en los [*indexadores* de Azure Search ](search-indexer-overview.md) que rastrean los orígenes de datos y proporcionan el procesamiento completo de los índices. Las aptitudes ahora se adjuntan a los indexadores, lo que permite interceptar y enriquecer los documentos en función del conjunto de aptitudes que defina. Una vez que se indexa, puede acceder al contenido mediante solicitudes de búsqueda a través de todos los [tipos de consulta compatibles con Azure Search](search-query-overview.md).  Si es la primera vez que usa los indexadores, esta sección lo guiará a través de los pasos.

### <a name="step-1-connection-and-document-cracking-phase"></a>Paso 1: Fase de conexión y descifrado de documentos

Al comienzo de la canalización, hay texto no estructurado o contenido que no es de texto, como archivos JPEG de documentos escaneados e imágenes. Los datos deben estar en un servicio de almacenamiento de datos de Azure al que un indexador puede acceder. Los indexadores pueden "descifrar" los documentos de origen para extraer texto de los datos de origen.

![Fase de descifrado de documentos](./media/cognitive-search-intro/document-cracking-phase-blowup.png "descifrado de documentos")

 Entre los orígenes compatibles se incluyen Azure Blob Storage, Azure Table Storage, Azure SQL Database y Azure Cosmos DB. El contenido basado en texto se puede extraer de estos tipos de archivo: archivos PDF, Word, PowerPoint, archivos CSV. Para ver la lista completa, consulte los [formatos compatibles](search-howto-indexing-azure-blob-storage.md#supported-document-formats).

### <a name="step-2-cognitive-skills-and-enrichment-phase"></a>Paso 2: Fase de enriquecimiento y aptitudes cognitivas

El enriquecimiento se produce a través de *aptitudes cognitivas* que realizan operaciones atómicas. Por ejemplo, una vez que obtiene el contenido de texto de un archivo PDF, puede aplicar la detección de idioma del reconocimiento de entidades o la extracción de frases clave para generar campos nuevos en el índice que no están disponibles de manera nativa en el origen. La colección completa de aptitudes que usa en la canalización se conoce como un *conjunto de aptitudes*.  

![Fase de enriquecimiento](./media/cognitive-search-intro/enrichment-phase-blowup.png "fase de enriquecimiento")

Un conjunto de aptitudes se basa en las [aptitudes cognitivas predefinidas](cognitive-search-predefined-skills.md) o en las [aptitudes predeterminadas](cognitive-search-create-custom-skill-example.md) que proporciona y conecta con el conjunto de aptitudes. Un conjunto de aptitudes puede ser mínimo o altamente complejo y no solo determina el tipo de procesamiento, sino también el orden de las operaciones. Un conjunto de aptitudes más las asignaciones de campos que se definen como parte de un indexador especifica completamente la canalización de enriquecimiento. Para más información sobre cómo unir todos estos elementos, consulte el artículo sobre cómo [definir un conjunto de aptitudes](cognitive-search-defining-skillset.md).

Internamente, la canalización genera una colección de documentos enriquecidos. Puede decidir qué elementos de los documentos enriquecidos se deben asignar a los campos del índice de búsqueda que se pueden indexar. Por ejemplo, si aplicara las aptitudes de reconocimiento de entidades y de extracción de frases clave, esos campos nuevos se volverían parte del documento enriquecido y se podrían asignar a los campos del índice. Consulte las [anotaciones](cognitive-search-concept-annotations-syntax.md) para más información sobre las formaciones de entrada/salida.

### <a name="step-3-search-index-and-query-based-access"></a>Paso 3: Índice de búsqueda y acceso basado en consulta

Cuando finalice el procesamiento, tendrá un corpus de búsqueda compuesto de documentos enriquecidos donde se pueden realizar búsquedas completas de texto en Azure Search. Para acceder al contenido enriquecido que la canalización genera, los desarrolladores y usuarios [consultan el índice](search-query-overview.md). 

![Icono de índice con búsqueda](./media/cognitive-search-intro/search-phase-blowup.png "Icono de índice con búsqueda")

El índice es como cualquier otro índice que pudiera crear para Azure Search: puede complementarlo con analizadores personalizados, invocar consultas de búsqueda aproximada, agregar la búsqueda filtrada o experimentar con perfiles de puntuación para volver a dar forma a los resultados de la búsqueda.

Los índices se generan a partir de un esquema de índice que define los campos, atributos y otras construcciones adjuntas a un índice específico, como los perfiles de puntuación y las asignaciones de sinónimos. Una vez que se define y rellena un índice, puede indexar de manera incremental para seleccionar documentos de origen nuevos y actualizados. Algunas de las determinaciones requieren recompilar completamente el índice. Debe usar un conjunto de datos pequeño hasta que el diseño del esquema sea estable. Para más información, consulte el artículo sobre cómo [recompilar un índice](search-howto-reindex.md).

<a name="feature-concepts"></a>

## <a name="key-features-and-concepts"></a>Características y conceptos principales

| Concepto | DESCRIPCIÓN| Vínculos |
|---------|------------|-------|
| Conjunto de aptitudes | Recurso con nombre de nivel superior que contiene una colección de aptitudes. Un conjunto de aptitudes es la canalización de enriquecimiento. Se invoca durante la indexación por parte del indexador. | [Definición de un conjunto de habilidades](cognitive-search-defining-skillset.md) |
| Aptitud cognitiva | Transformación atómica en una canalización de enriquecimiento. Normalmente, se trata de un componente que extrae o deduce la estructura y, por tanto, aumenta el reconocimiento de los datos de entrada. Casi siempre, la salida se basa en el texto y el procesamiento es el procesamiento de lenguaje natural o el procesamiento de imagen que extrae o genera texto a partir de entradas de imágenes. La salida de una aptitud se puede asignar a un campo en un índice o usar como entrada para el enriquecimiento de nivel inferior. Microsoft predefinida y proporciona una aptitud, o bien puede ser personalizada, es decir, el usuario puede crearla e implementarla. | [Habilidades predefinidas](cognitive-search-predefined-skills.md) |
| Extracción de datos | Abarca una amplia variedad de procesamiento, pero respecto a Cognitive Search, la aptitud de reconocimiento de entidades con nombre se usa principalmente para extraer datos (una entidad) a partir de un origen que no proporciona de manera nativa esa información. | [Named Entity Recognition Skill (Aptitud de reconocimiento de entidades con nombre)](cognitive-search-skill-named-entity-recognition.md)| 
| Procesamiento de imágenes | Deduce texto a partir de una imagen, como la capacidad de reconocer un punto de referencia, o bien extrae texto a partir de una imagen. Algunos ejemplos comunes incluyen OCR para levantar caracteres de un archivo de documento escaneado (JPEG) o reconocer el nombre de una calle en una fotografía que incluye un letrero con el nombre. | [Image Analysis Skill](cognitive-search-skill-image-analysis.md) (Aptitud de análisis de imágenes) o [OCR Skill](cognitive-search-skill-ocr.md) (Aptitud de OCR)
| Procesamiento de lenguaje natural | Procesamiento de texto para obtener conclusiones e información sobre entradas de texto. La detección de idioma, el análisis de sentimiento y la extracción de frases clave son aptitudes que se enmarcan en el procesamiento de lenguaje natural.  | [Key Phrase Extraction Skill](cognitive-search-skill-keyphrases.md) (Aptitud de Extracción de frases clave), [Language Detection Skill](cognitive-search-skill-language-detection.md) (Aptitud de Detección de idioma), [Sentiment Analysis Skill](cognitive-search-skill-sentiment.md) (Aptitud de Análisis de sentimiento) |
| Descifrado de documentos | Proceso de extraer o crear contenido de texto a partir de orígenes que no son de texto durante el indexado. El reconocimiento óptico de caracteres (OCR) es un ejemplo pero, en general, se refiere a la funcionalidad de indexador principal cuando el indexador extrae contenido los archivos de aplicación. El origen de datos que proporciona la ubicación del archivo de origen y la definición del indexador que brinda las asignaciones de campo son factores clave en el descifrado de documentos. | Consulte [Indexers](search-indexer-overview.md) (Indexador) |
| Formas | Consolide los fragmentos de texto en una estructura más grande o, a la inversa, desglose fragmentos de texto de gran tamaño a un tamaño que se pueda administrar para un procesamiento adicional. | [Shaper Skill](cognitive-search-skill-shaper.md) (Aptitud de conformador), [Text Merger Skill](cognitive-search-skill-textmerger.md) (Aptitud de combinación de texto), [Text Split Skill](cognitive-search-skill-textsplit.md) (Aptitud de división de texto) |
| Documentos enriquecidos | Estructura interna transitoria, sin acceso directo en el código. Los documentos enriquecidos se generan durante el procesamiento, pero solo las salidas finales persisten en un índice de búsqueda. Las asignaciones de campos determinan los elementos de datos que se agregan al índice. | Consulte [Accessing enriched documents](cognitive-search-tutorial-blob.md#access-enriched-document) (Acceso a documentos enriquecidos). |
| Indexer |  Un rastreador (crawler) que extrae datos y metadatos utilizables en búsquedas de un origen de datos externo y rellena un índice basado en las asignaciones de un campo a otro entre el índice y su origen de datos para el descifrado de documentos. En el caso de Cognitive Search, el indexador invoca un conjunto de aptitudes y contiene las asignaciones de campos que asocian la salida de enriquecimiento con los campos de destino del índice. La definición del indexador contiene todas las instrucciones y referencias de las operaciones de la canalización y esta se invoca cuando se ejecuta el indexador. | [Indexadores](search-indexer-overview.md) |
| Origen de datos  | Un objeto que un indexador usa para conectarse a un origen de datos externo de tipos compatibles en Azure. | Consulte [Indexers](search-indexer-overview.md) (Indexador) |
| Índice | Corpus de búsqueda persistente en Azure Search, compilado a partir de un esquema de índice que define la estructura y el uso de los campos. | [Índices en Azure Search](search-what-is-an-index.md) | 

<a name="where-do-i-start"></a>

## <a name="where-do-i-start"></a>¿Por dónde empiezo?

**Paso 1: [Creación de un recurso de Azure Search](search-create-service-portal.md)** 

**Paso 2: Pruebe algunos Inicios rápidos y ejemplos de experiencia práctica**

+ [Inicio rápido (portal)](cognitive-search-quickstart-blob.md)
+ [Tutorial (solicitudes HTTP)](cognitive-search-tutorial-blob.md)
+ [Aptitudes personalizadas de ejemplo (C#)](cognitive-search-create-custom-skill-example.md)

**Paso 3: Revisión de la API (solo para REST)**

Actualmente, solo se proporcionan API de REST. Use `api-version=2017-11-11-Preview` en todas las solicitudes. Use las API siguientes para compilar una solución de Cognitive Search. Solo se agregan o extienden dos API para Cognitive Search. Otras API tienen la misma sintaxis que las versiones con disponibilidad general.

| API DE REST | DESCRIPCIÓN |
|-----|-------------|
| [Crear origen de datos](https://docs.microsoft.com/rest/api/searchservice/create-data-source)  | Recurso que identifica un origen de datos externo que proporciona los datos de origen que se usan para crear documentos enriquecidos.  |
| [Crear un conjunto de aptitudes (versión-api=2017-11-11-Versión preliminar)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)  | Recurso que coordina el uso de las [aptitudes predefinidas](cognitive-search-predefined-skills.md) y las [aptitudes cognitivas personalizadas](cognitive-search-custom-skill-interface.md) que se usan en una canalización de enriquecimiento durante el indexado. |
| [Crear índice](https://docs.microsoft.com/rest/api/searchservice/create-index)  | Esquema que expresa un índice de Azure Search. Los campos del índice se asignan a los campos de los datos de origen o a los campos fabricados durante la fase de enriquecimiento (por ejemplo, un campo para los nombres de organización creado por el reconocimiento de entidades). |
| [Create Indexer (api-version=2017-11-11-Preview) (Creación de indexador [api-version=2017-11-11-Preview])](https://docs.microsoft.com/rest/api/searchservice/create-skillset)  | Recurso que define los componentes usados durante la indexación, los que incluyen un origen de datos, un conjunto de aptitudes, asociaciones de campos desde el origen y estructuras de datos intermedias con el índice de destino, además del destino mismo. La ejecución del indexador es el desencadenador del enriquecimiento y la ingesta de datos. La salida es un corpus de búsqueda basado en el esquema del índice, que se rellena con los datos de origen y se enriquece a través de los conjuntos de aptitudes.  |

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
