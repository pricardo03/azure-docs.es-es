---
title: Errores y advertencias del indexador
titleSuffix: Azure Cognitive Search
description: En este artículo se proporciona información y soluciones a errores y advertencias comunes que pueden surgir durante el enriquecimiento de IA en Azure Cognitive Search.
manager: nitinme
author: amotley
ms.author: abmotley
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 0230fbb2cb94001f7965cf1756a8a0d1061978da
ms.sourcegitcommit: 2d3740e2670ff193f3e031c1e22dcd9e072d3ad9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/16/2019
ms.locfileid: "74133218"
---
# <a name="troubleshooting-common-indexer-errors-and-warnings-in-azure-cognitive-search"></a>Solución de errores y advertencias comunes con el indexador en Azure Cognitive Search

En este artículo se proporciona información y soluciones a errores y advertencias comunes que pueden surgir durante la indexación y el enriquecimiento de IA en Azure Cognitive Search.

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

### <a name="could-not-execute-skill"></a>No se pudo ejecutar la aptitud
El indexador no pudo ejecutar una aptitud del conjunto de aptitudes.

| Motivo | Ejemplo | . |
| --- | --- | --- |
| Un campo contiene un término demasiado grande | Un término del documento es mayor que el [límite de 32 KB](search-limits-quotas-capacity.md#api-request-limits) | Para evitar esta restricción, asegúrese de que el campo no está configurado como filtrable, con facetas o que se puede ordenar.
| El documento es demasiado grande para indexarlo | Un documento es mayor que el [tamaño de solicitud de API máximo](search-limits-quotas-capacity.md#api-request-limits) | [Indexación de grandes conjuntos de datos](search-howto-large-index.md)
| Problemas de conectividad transitorios | Se produjo un error transitorio. Inténtelo de nuevo más tarde. | En ocasiones, hay problemas de conectividad inesperados. Intente volver a ejecutar el documento mediante el indexador más adelante. |
| Posible error del producto | Se ha producido un error inesperado. | Esto indica una clase desconocida de error y puede significar que hay un error del producto. Registre una [incidencia de soporte técnico](https://ms.portal.azure.com/#create/Microsoft.Support) para obtener ayuda. |
| Una aptitud detectó un error durante la ejecución | (De la Aptitud Combinación) Uno o varios valores de desplazamiento no eran válidos y no se pudieron analizar. Los elementos se insertaron al final del texto | Use la información del mensaje de error para solucionar el problema. Este tipo de error requerirá la acción de resolución. |

### <a name="could-not-execute-skill-because-the-web-api-request-failed"></a>No se pudo ejecutar la aptitud debido a un error en la solicitud a la API web
Error al ejecutar la aptitud porque no se pudo realizar la llamada a la API web. Por lo general, esta clase de error se produce cuando se usan aptitudes personalizadas, en cuyo caso necesitará depurar el código personalizado para resolver el problema. Por el contrario, si el error proviene de una aptitud integrada, consulte el mensaje de error para saber cómo corregir el problema.

### <a name="could-not-execute-skill-because-web-api-skill-response-is-invalid"></a>No se pudo ejecutar la aptitud porque la respuesta de la aptitud de API web no es válida
No se pudo ejecutar la aptitud porque la llamada a la API web devolvió una respuesta no válida. Por lo general, esta clase de error se produce cuando se usan aptitudes personalizadas, en cuyo caso necesitará depurar el código personalizado para resolver el problema. Por el contrario, si el error proviene de una aptitud integrada, registre una [incidencia de soporte técnico](https://ms.portal.azure.com/#create/Microsoft.Support) para obtener ayuda.

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
| El documento contiene demasiados objetos en la colección | Una colección del documento supera el [número máximo de elementos en el límite de todas las colecciones complejas](search-limits-quotas-capacity.md#index-limits) | Se recomienda reducir el tamaño de la colección compleja en el documento para bajar el límite y evitar un uso elevado del almacenamiento.
| Problemas de conexión con el índice de destino (que persiste después de varios reintentos) porque el servicio está bajo otra carga, como consulta o indexación. | No se puede establecer la conexión para actualizar el índice. El servicio de búsqueda está sometido a mucha carga. | [Escalar verticalmente el servicio de búsqueda](search-capacity-planning.md)
| El servicio de búsqueda se está revisando para la actualización del servicio o está en medio de una reconfiguración de la topología. | No se puede establecer la conexión para actualizar el índice. El servicio de búsqueda está inactivo o está experimentando una transición. | Configure el servicio con al menos 3 réplicas para una disponibilidad del 99,9 % según se indica en la [documentación del Acuerdo de Nivel de Servicio](https://azure.microsoft.com/support/legal/sla/search/v1_0/).
| Error en el recurso de proceso o de red subyacente (poco frecuente) | No se puede establecer la conexión para actualizar el índice. Se produjo un error desconocido. | Establezca los indexadores en [Ejecutar según una programación](search-howto-schedule-indexers.md) para que se recuperen de un estado de error.
| Una solicitud de indexación realizada al índice de destino no se confirmó en un período de tiempo de espera debido a problemas de red. | No se pudo establecer la conexión con el índice de búsqueda de manera oportuna. | Establezca los indexadores en [Ejecutar según una programación](search-howto-schedule-indexers.md) para que se recuperen de un estado de error. Además, intente reducir el [tamaño del lote](https://docs.microsoft.com/rest/api/searchservice/create-indexer#parameters) del índice si esta condición de error persiste.

### <a name="could-not-index-document-because-the-indexer-data-to-index-was-invalid"></a>No se pudo indexar el documento porque los datos del indexador para el índice no eran válidos

El documento se leyó y se procesó, pero debido a un error de coincidencia en la configuración de los campos del índice y la naturaleza de los datos extraídos por el indexador, no se pudo agregar al índice de búsqueda. Estos pueden ser los motivos:

| Motivo | Ejemplo
| --- | ---
| El tipo de datos de los campos extraídos por el indexador no es compatible con el modelo de datos del campo de índice de destino correspondiente. | El campo de datos "_data_" del documento con la clave "_data_" tiene un valor no válido "of type 'Edm.String'". El tipo esperado era "Collection(Edm.String)". |
| Error al extraer una entidad JSON de un valor de cadena. | No se pudo analizar el valor "of type 'Edm.String'" del campo "_data_" como objeto JSON. Error: "After parsing a value an unexpected character was encountered: ''. Path '_path_', line 1, position 3162". |
| Error al extraer una colección de entidades JSON de un valor de cadena.  | No se pudo analizar el valor "of type 'Edm.String'" del campo "_data_" como matriz JSON. Error: "After parsing a value an unexpected character was encountered: ''. Path '[0]', line 1, position 27". |
| Se detectó un tipo desconocido en el documento de origen. | No se puede indexar el tipo desconocido "_unknown_" |
| Se usó una notación no compatible para los puntos geográficos en el documento de origen. | No se admiten los literales de cadena WKT POINT. En su lugar, use los literales de punto GeoJSON. |

En todos estos casos, consulte [Tipos de datos admitidos](https://docs.microsoft.com/rest/api/searchservice/supported-data-types) y [Asignación de tipos de datos para indexadores](https://docs.microsoft.com/rest/api/searchservice/data-type-map-for-indexers-in-azure-search) para asegurarse de compilar correctamente el esquema de índice y de haber configurado las [asignaciones de campos de indexador](search-indexer-field-mappings.md) adecuadas. El mensaje de error incluirá detalles que pueden ayudar a realizar un seguimiento del origen del error de coincidencia.

### <a name="could-not-process-document-within-indexer-max-run-time"></a>No se pudo procesar el documento en el tiempo de ejecución máximo del indexador

Este error se produce cuando el indexador no puede finalizar el procesamiento de un único documento desde el origen de datos en el tiempo de ejecución permitido. El [tiempo de ejecución máximo](search-limits-quotas-capacity.md#indexer-limits) es más corto cuando se usan conjuntos de aptitudes. Cuando se produce este error, si tiene maxFailedItems establecido en un valor distinto de 0, el indexador omite el documento en futuras ejecuciones para que la indexación pueda continuar. Si no puede permitirse omitir ningún documento, o si ve este error sistemáticamente, podría dividir los documentos en otros más pequeños para que se pueda avanzar parcialmente dentro de una única ejecución del indexador.

##  <a name="warnings"></a>Advertencias
Las advertencias no detienen la indexación, sino que indican las condiciones que podrían dar lugar a resultados inesperados. Que realice o no una acción depende de los datos y del escenario.

### <a name="could-not-execute-skill-because-a-skill-input-was-invalid"></a>No se pudo ejecutar la aptitud porque una entrada de aptitud no era válida
El indexador no pudo ejecutar una aptitud del conjunto de aptitudes porque faltaba una entrada para la aptitud, era del tipo equivocado o no era válida.

Las aptitudes cognitivas tienen entradas obligatorias y entradas opcionales. Por ejemplo, la [aptitud Extracción de frases clave](cognitive-search-skill-keyphrases.md) tiene dos entradas obligatorias `text`, `languageCode`, y no tienen ninguna entrada opcional. Si las entradas requeridas no son válidas, la aptitud se omite y genera una advertencia. Las aptitudes omitidas no generan ninguna salida, por lo que si otras aptitudes usan salidas de la aptitud omitida, podrían generar más advertencias.

Si quiere proporcionar un valor predeterminado en caso de una entrada omitida, puede usar la [aptitud condicional](cognitive-search-skill-conditional.md) para generar un valor predeterminado y luego usar la salida de la [aptitud condicional](cognitive-search-skill-conditional.md) como la entrada de la aptitud.


```json
{
    "@odata.type": "#Microsoft.Skills.Util.ConditionalSkill",
    "context": "/document",
    "inputs": [
        { "name": "condition", "source": "= $(/document/language) == null" },
        { "name": "whenTrue", "source": "= 'en'" },
        { "name": "whenFalse", "source": "= $(/document/language)" }
    ],
    "outputs": [ { "name": "output", "targetName": "languageWithDefault" } ]
}
```

| Motivo | Ejemplo | . |
| --- | --- | --- |
| La entrada de aptitud tiene un tipo incorrecto | La entrada de aptitud obligatoria `X` no tenía el tipo esperado `String`. La entrada de aptitud obligatoria `X` no tenía el formato esperado. | Ciertas aptitudes esperan entradas de tipos determinados, por ejemplo, la [aptitud Opinión](cognitive-search-skill-sentiment.md) espera que `text` sea una cadena. Si la entrada especifica un valor que no es de cadena, la aptitud no se ejecuta y no genera ninguna salida. Asegúrese de que el conjunto de datos tiene valores de entrada con un tipo uniforme, o bien use una [aptitud API web personalizada](cognitive-search-custom-skill-web-api.md) para procesar previamente la entrada. Si va a iterar la aptitud en una matriz, revise que la entrada y el contexto de la aptitud tengan `*` en las posiciones correctas. Por lo general, tanto el contexto como el origen de entrada deben finalizar con `*` para las matrices. |
| Falta la entrada de aptitud | Falta la entrada de aptitud obligatoria `X`. | Si todos los documentos reciben esta advertencia, lo más probable es que haya un error tipográfico en las rutas de acceso de las entradas, por lo que debe revisar nuevamente el uso de mayúsculas y minúsculas en el nombre de la propiedad, ver si faltan o sobran `*` en la ruta de acceso y si los documentos del origen de datos definen las entradas obligatorias. |
| La entrada de código de idioma de aptitud no es válida | La entrada de aptitud `languageCode` tiene los códigos de idioma siguientes `X,Y,Z`, uno de los cuales es al menos no válido. | Consulte más detalles [a continuación](cognitive-search-common-errors-warnings.md#skill-input-languagecode-has-the-following-language-codes-xyz-at-least-one-of-which-is-invalid). |

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

### <a name="web-api-skill-response-contains-warnings"></a>La respuesta de la aptitud API web contiene advertencias
Los indexadores podían ejecutar una aptitud del conjunto de aptitudes, pero la respuesta de la solicitud de API web indicaban que había advertencias durante la ejecución. Revise las advertencias para entender cómo se ven afectados los datos y si es necesario realizar alguna acción o no.

### <a name="the-current-indexer-configuration-does-not-support-incremental-progress"></a>La configuración del indexador actual no admite el progreso incremental
Esta advertencia solo se produce para los orígenes de datos de Cosmos DB.

El progreso incremental durante la indexación garantiza que si se interrumpe la ejecución del indexador por errores transitorios o por el límite de tiempo de ejecución, dicho indexador puede retomar la ejecución por donde la dejó la próxima vez que se ejecute, en lugar de tener que volver a indexar toda la colección desde el principio. Esto es especialmente importante cuando se indexan colecciones grandes.

La capacidad de reanudar un trabajo de indexación sin terminar se basa en tener documentos ordenados por la columna de `_ts`. El indexador usa la marca de tiempo para determinar qué documento se debe seleccionar a continuación. Si falta la columna de `_ts` o si el indexador no puede determinar si se ha ordenado una consulta personalizada, el indexador comienza al principio y usted verá esta advertencia.

Es posible invalidar este comportamiento, habilitar el progreso incremental y suprimir esta advertencia si usa la propiedad de configuración `assumeOrderByHighWatermarkColumn`.

Para más información, consulte [Progreso incremental y consultas personalizadas](search-howto-index-cosmosdb.md#IncrementalProgress).

### <a name="some-data-was-lost-during-projection-row-x-in-table-y-has-string-property-z-which-was-too-long"></a>Algunos datos se perdieron durante la proyección. La fila 'X' de la tabla 'Y' tiene la propiedad de cadena 'Z', que era demasiado larga.
El [servicio de Table Storage](https://azure.microsoft.com/services/storage/tables) tiene límites en cuanto a qué tan grandes pueden ser las [propiedades de las entidades](https://docs.microsoft.com/rest/api/storageservices/understanding-the-table-service-data-model#property-types). Las cadenas pueden tener 32.000 caracteres como máximo. Si una fila con una propiedad de cadena de más de 32.000 caracteres se está proyectando, solo se conservan los primeros 32.000 caracteres. Para evitar este problema, evite proyectar filas con propiedades de cadena de más de 32.000 caracteres.

### <a name="truncated-extracted-text-to-x-characters"></a>El texto extraído se ha truncado a X caracteres
Los indexadores limitan la cantidad de texto que se puede extraer de cualquier documento. Este límite depende del plan de tarifa: 32 000 caracteres para el nivel Gratis, 64 000 para Básico y 4 millones para Estándar, Estándar S2 y Estándar S3. El texto que se ha truncado no se indexará. Para evitar esta advertencia, intente dividir los documentos con grandes cantidades de texto en varios documentos más pequeños. 

Para más información, consulte [Límites de índice](search-limits-quotas-capacity.md#indexer-limits).

### <a name="could-not-map-output-field-x-to-search-index"></a>No se pudo asignar el campo de salida "X" al índice de búsqueda
Las asignaciones de campos de salida que hagan referencia a datos inexistentes o nulos generarán advertencias con cada documento y producirán un campo de índice vacío. Para solucionar este problema, compruebe las rutas de acceso de origen de la asignación de campos de salida en busca de posibles errores tipográficos o establezca un valor predeterminado mediante la [aptitud condicional](cognitive-search-skill-conditional.md#sample-skill-definition-2-set-a-default-value-for-a-value-that-doesnt-exist).

### <a name="the-data-change-detection-policy-is-configured-to-use-key-column-x"></a>La directiva de detección de cambios de datos está configurada para usar la columna de clave 'X'
[Las directivas de detección de cambios de datos](https://docs.microsoft.com/rest/api/searchservice/create-data-source#data-change-detection-policies) tienen requisitos específicos para las columnas que usan para detectar cambios. Uno de estos requisitos es que esta columna se actualice cada vez que se cambia el elemento de origen. Otro requisito es que el nuevo valor de esta columna sea mayor que el valor anterior. Las columnas de clave no cumplen este requisito porque no cambian en cada actualización. Para solucionar este problema, seleccione una columna diferente para la directiva de detección de cambios.