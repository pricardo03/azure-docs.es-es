---
title: 'Advertencias y errores comunes: Azure Search'
description: En este artículo se proporciona información y soluciones a errores y advertencias comunes que pueden surgir durante el enriquecimiento de IA en Azure Search.
services: search
manager: heidist
author: amotley
ms.service: search
ms.workload: search
ms.topic: conceptual
ms.date: 09/18/2019
ms.author: abmotley
ms.openlocfilehash: b5a161e570489e6382f2226ab5dc9a1c34dc67df
ms.sourcegitcommit: 11265f4ff9f8e727a0cbf2af20a8057f5923ccda
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/08/2019
ms.locfileid: "72028319"
---
# <a name="common-errors-and-warnings-of-the-ai-enrichment-pipeline-in-azure-search"></a>Errores y advertencias comunes de la canalización de enriquecimiento de IA en Azure Search

En este artículo se proporciona información y soluciones a errores y advertencias comunes que pueden surgir durante el enriquecimiento de IA en Azure Search.

## <a name="errors"></a>Errors
La indexación se detiene cuando el recuento de errores supera ["maxfaileditems"](cognitive-search-concept-troubleshooting.md#tip-3-see-what-works-even-if-there-are-some-failures). 

Si desea que los indexadores omitan estos errores (y omitan los "documentos con errores"), considere la posibilidad de actualizar `maxFailedItems` y `maxFailedItemsPerBatch` como se describe [aquí](https://docs.microsoft.com/rest/api/searchservice/create-indexer#general-parameters-for-all-indexers).

> [!NOTE]
> Cada documento con error junto con su clave de documento (si está disponible) se mostrará como un error en el estado de ejecución del indexador. Puede utilizar la [API de índice](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) para cargar manualmente los documentos en un momento posterior si ha configurado el indexador para tolerar errores.

Las secciones siguientes pueden ayudarle a resolver errores, de forma que la indexación continúe.

### <a name="could-not-read-document"></a>No se pudo leer el documento
El indexador no pudo leer el documento del origen de datos. Estos pueden ser los motivos:

| Motivo | Ejemplo | . |
| --- | --- | --- |
| tipos de campo incoherentes en distintos documentos | El tipo de valor no coincide con el tipo de columna. No se pudo almacenar `'{47.6,-122.1}'` en la columna de autores.  El tipo esperado es JArray. | Asegúrese de que el tipo de cada campo sea el mismo en los distintos documentos. Por ejemplo, si el campo `'startTime'` del primer documento es DateTime y el del segundo documento es una cadena, se mostrará este error. |
| errores del servicio subyacente del origen de datos | (de Cosmos DB) `{"Errors":["Request rate is large"]}` | Compruebe la instancia de almacenamiento para asegurarse de que es correcta. Es posible que tenga que ajustar el escalado o la creación de particiones. |
| problemas transitorios | Error en el nivel del transporte al recibir los resultados del servidor. (proveedor: Proveedor TCP, error: 0: El host remoto forzó el cierre de la conexión existente. | En ocasiones, hay problemas de conectividad inesperados. Intente volver a ejecutar el documento mediante el indexador más adelante. |

### <a name="could-not-extract-document-content"></a>No se pudo extraer el contenido del documento
El indexador con un origen de datos de blob no pudo extraer el contenido del documento (por ejemplo, un archivo PDF). Estos pueden ser los motivos:

| Motivo | Ejemplo | . |
| --- | --- | --- |
| el blob está por encima del límite de tamaño | El documento tiene `'150441598'` bytes, lo que supera el tamaño máximo de `'134217728'` bytes para la extracción de documentos con el nivel de servicio actual. | [errores de indexación de blobs](search-howto-indexing-azure-blob-storage.md#dealing-with-errors) |
| el blob tiene un tipo de contenido no admitido | El documento tiene un tipo de contenido no admitido `'image/png'` | [errores de indexación de blobs](search-howto-indexing-azure-blob-storage.md#dealing-with-errors) |
| el blob está cifrado | No se pudo procesar el documento; puede que esté cifrado o protegido con contraseña. | [configuración de blob](search-howto-indexing-azure-blob-storage.md#controlling-which-parts-of-the-blob-are-indexed) |
| problemas transitorios | Error al procesar el blob: Se anuló la solicitud: Se canceló la solicitud. | En ocasiones, hay problemas de conectividad inesperados. Intente volver a ejecutar el documento mediante el indexador más adelante. |

### <a name="could-not-parse-document"></a>No se pudo analizar el documento
El indexador leyó el documento desde el origen de datos, pero hubo un problema al convertir el contenido del documento en el esquema de asignación de campos especificado. Estos pueden ser los motivos:

| Motivo | Ejemplo | . |
| --- | --- | --- |
| Falta la clave del documento. | No puede faltar la clave del documento ni estar vacía | Asegúrese de que todos los documentos tengan claves de documento válidas. |
| La clave del documento no es válida | La clave del documento no puede tener más de 1024 caracteres | Modifique la clave del documento para que cumpla los requisitos de validación. |
| No se pudo aplicar la asignación de campos a un campo | No se pudo aplicar la función de asignación `'functionName'` al campo `'fieldName'`. La matriz no puede ser NULL. Nombre de parámetro: bytes | Compruebe las [asignaciones de campos](search-indexer-field-mappings.md) definidas en el indexador y compárelas con los datos del campo especificado del documento con errores. Puede que sea necesario modificar las asignaciones de campos o los datos del documento. |
| No se pudo leer el valor del campo | No se pudo leer el valor de la columna `'fieldName'` en el índice `'fieldIndex'`. Error en el nivel del transporte al recibir los resultados del servidor. (proveedor: Proveedor TCP, error: 0: El host remoto forzó el cierre de la conexión existente). | Normalmente, estos errores se deben a problemas de conectividad inesperados con el servicio subyacente del origen de datos. Intente volver a ejecutar el documento mediante el indexador más adelante. |

### <a name="could-not-index-document"></a>No se pudo indexar el documento
El documento se leyó y se procesó, pero el indexador no pudo agregarlo al índice de búsqueda. Estos pueden ser los motivos:

| Motivo | Ejemplo | . |
| --- | --- | --- |
| Un campo contiene un término demasiado grande | Un término del documento es mayor que el [límite de 32 KB](search-limits-quotas-capacity.md#api-request-limits) | Para evitar esta restricción, asegúrese de que el campo no está configurado como filtrable, con facetas o que se puede ordenar.
| El documento es demasiado grande para indexarlo | Un documento es mayor que el [tamaño de solicitud de API máximo](search-limits-quotas-capacity.md#api-request-limits) | [Indexación de grandes conjuntos de datos](search-howto-large-index.md)

### <a name="skill-input-languagecode-has-the-following-language-codes-xyz-at-least-one-of-which-is-invalid"></a>La entrada de aptitud "languageCode" tiene los siguientes códigos de idioma "X,Y,Z", uno de los cuales es al menos válido.
Uno o varios de los valores que pasan a la entrada `languageCode` opcional de una aptitud de nivel inferior no se admiten. Esta situación puede darse si se pasa la salida de [LanguageDetectionSkil](cognitive-search-skill-language-detection.md) a aptitudes posteriores y la salida consta de más idiomas de los que se admiten en esas aptitudes de nivel inferior.

Si sabe que el conjunto de datos solo tiene un idioma, debe quitar el elemento [LanguageDetectionSkill](cognitive-search-skill-language-detection.md) y la entrada de aptitud `languageCode` y usar en cambio el parámetro de aptitud `defaultLanguageCode` para esa aptitud, suponiendo que el idioma sea compatible con ella.

Si sabe que el conjunto de datos contiene varios idiomas y, por tanto, necesita el elemento [LanguageDetectionSkill](cognitive-search-skill-language-detection.md) y la entrada `languageCode`, considere agregar un elemento [ConditionalSkill](cognitive-search-skill-conditional.md) para filtrar el texto con los idiomas que no se admiten antes de pasar el texto a la aptitud de nivel inferior.  Este es un ejemplo de cuál podría ser el aspecto del elemento EntityRecognitionSkill:

```json
{
    "@odata.type": "#Microsoft.Skills.Util.ConditionalSkill",
    "context": "/document",
    "inputs": [
        { "name": "condition", "source": "= $(/document/language) == 'de' || $(/document/language) == 'en' || $(/document/language) == 'es' || $(/document/language) == 'fr' || $(/document/language) == 'it'" },
        { "name": "whenTrue", "source": "/document/content" },
        { "name": "whenFalse", "source": "= null" }
    ],
    "outputs": [ { "name": "output", "targetName": "supportedByEntityRecognitionSkill" } ]
}
```

Estas son algunas referencias de los idiomas admitidos actualmente en cada una de las aptitudes que pueden producir este mensaje de error:
* [Idiomas compatibles con Text Analytics](https://docs.microsoft.com/azure/cognitive-services/text-analytics/text-analytics-supported-languages) (para [KeyPhraseExtractionSkill](cognitive-search-skill-keyphrases.md), [EntityRecognitionSkill](cognitive-search-skill-entity-recognition.md) y [SentimentSkill](cognitive-search-skill-sentiment.md))
* [Idiomas compatibles con Translator](https://docs.microsoft.com/azure/cognitive-services/translator/language-support) (para [Text TranslationSkill](cognitive-search-skill-text-translation.md))
* Idiomas compatibles con [Text SplitSkill](cognitive-search-skill-textsplit.md): `da, de, en, es, fi, fr, it, ko, pt`

### <a name="skill-did-not-execute-within-the-time-limit"></a>La aptitud no se ejecutó dentro del límite de tiempo
Hay dos casos en los que puede encontrarse con este mensaje de error, cada uno de los cuales debe tratarse de forma diferente. Siga las instrucciones que se indican a continuación según la aptitud que devuelva este error en su caso.

#### <a name="built-in-cognitive-service-skills"></a>Aptitudes integradas del servicio cognitivo
Muchas de las aptitudes cognitivas integradas, como la detección del idioma, el reconocimiento de entidades o el método OCR, están respaldadas por un punto de conexión de API de servicio cognitivo. En ocasiones, hay problemas transitorios con estos puntos de conexión y se agota el tiempo de espera de la solicitud. En estos casos, la única solución es esperar y volver a intentarlo. Como medida de mitigación, podría configurar un indexador para que [se ejecute según una programación](search-howto-schedule-indexers.md). La indexación programada reanuda el proceso donde se detuvo. Suponiendo que se resuelvan los problemas transitorios, la indexación y el procesamiento de aptitudes cognitivas podrán continuar en la siguiente ejecución programada.

#### <a name="custom-skills"></a>Aptitudes personalizadas
Si se produce un error de tiempo de espera con una aptitud personalizada que haya creado, hay un par de cosas que puede intentar. En primer lugar, revise la aptitud personalizada y asegúrese de que no se quede atascada en un bucle y que devuelva un resultado coherente. Cuando haya confirmado que ese es el caso, determine cuál es el tiempo de ejecución de su aptitud. Si no estableció explícitamente un valor `timeout` en la definición de aptitud personalizada, el valor `timeout` predeterminado es 30 segundos. Si 30 segundos no es un tiempo lo bastante largo para que se ejecute su aptitud, puede especificar un valor de `timeout` superior en la definición de aptitud personalizada. Este es un ejemplo de una definición de aptitud personalizada en la que el tiempo de espera se establece en 90 segundos:

```json
  {
        "@odata.type": "#Microsoft.Skills.Custom.WebApiSkill",
        "uri": "<your custom skill uri>",
        "batchSize": 1,
        "timeout": "PT90S",
        "context": "/document",
        "inputs": [
          {
            "name": "input",
            "source": "/document/content"
          }
        ],
        "outputs": [
          {
            "name": "output",
            "targetName": "output"
          }
        ]
      }
```

El valor máximo que puede establecer para el parámetro `timeout` es de 230 segundos.  Si su aptitud personalizada no se puede ejecutar de forma coherente en 230 segundos, considere la posibilidad de reducir el valor de `batchSize` de su aptitud personalizada para que tenga menos documentos que procesar en una sola ejecución.  Si ya ha establecido `batchSize` en 1, tendrá que volver a escribir la aptitud para que se pueda ejecutar en menos de 230 segundos; o también puede dividirla en varias aptitudes personalizadas para que el tiempo de ejecución de una de ellas sea como máximo 230 segundos. Para más información, revise la [documentación de las aptitudes personalizadas](cognitive-search-custom-skill-web-api.md).

### <a name="could-not-mergeorupload--delete-document-to-the-search-index"></a>No se pudo "`MergeOrUpload`" | "`Delete`" al índice de búsqueda

El documento se leyó y se procesó, pero el indexador no pudo agregarlo al índice de búsqueda. Estos pueden ser los motivos:

| Motivo | Ejemplo | . |
| --- | --- | --- |
| Un término del documento es mayor que el [límite de 32 KB](search-limits-quotas-capacity.md#api-request-limits) | Un campo contiene un término demasiado grande | Para evitar esta restricción, asegúrese de que el campo no está configurado como filtrable, con facetas o que se puede ordenar.
| Un documento es mayor que el [tamaño de solicitud de API máximo](search-limits-quotas-capacity.md#api-request-limits) | El documento es demasiado grande para indexarlo | [Indexación de grandes conjuntos de datos](search-howto-large-index.md)
| Problemas de conexión con el índice de destino (que persiste después de varios reintentos) porque el servicio está bajo otra carga, como consulta o indexación. | No se puede establecer la conexión para actualizar el índice. El servicio de búsqueda está sometido a mucha carga. | [Escalar verticalmente el servicio de búsqueda](search-capacity-planning.md)
| El servicio de búsqueda se está revisando para la actualización del servicio o está en medio de una reconfiguración de la topología. | No se puede establecer la conexión para actualizar el índice. El servicio de búsqueda está inactivo o está experimentando una transición. | Configure el servicio con al menos 3 réplicas para una disponibilidad del 99,9 % según se indica en la [documentación del Acuerdo de Nivel de Servicio](https://azure.microsoft.com/support/legal/sla/search/v1_0/).
| Error en el recurso de proceso o de red subyacente (poco frecuente) | No se puede establecer la conexión para actualizar el índice. Se produjo un error desconocido. | Establezca los indexadores en [Ejecutar según una programación](search-howto-schedule-indexers.md) para que se recuperen de un estado de error.

##  <a name="warnings"></a>Advertencias
Las advertencias no detienen la indexación, sino que indican las condiciones que podrían dar lugar a resultados inesperados. Que realice o no una acción depende de los datos y del escenario.

### <a name="skill-input-was-truncated"></a>La entrada de aptitud se truncó
Las aptitudes cognitivas tienen límites en cuanto a la longitud del texto que se puede analizar a la vez. Si la entrada de texto de estas aptitudes está por encima de ese límite, se truncará el texto para cumplir el límite y, luego, se realizará el enriquecimiento sobre ese texto truncado. Esto significa que la aptitud se ejecuta, pero no sobre todos los datos.

En el ejemplo de LanguageDetectionSkill que se indica a continuación, el campo de entrada `'text'` puede desencadenar esta advertencia si está por encima del límite de caracteres. Puede encontrar los límites de entrada de aptitudes en la [documentación de las aptitudes](cognitive-search-predefined-skills.md).

```json
 {
    "@odata.type": "#Microsoft.Skills.Text.LanguageDetectionSkill",
    "inputs": [
      {
        "name": "text",
        "source": "/document/text"
      }
    ],
    "outputs": [...]
  }
```

Si quiere asegurarse de que se analice todo el texto, considere la posibilidad de usar la [aptitud dividida](cognitive-search-skill-textsplit.md).