---
title: Migración de API v2 a v3
titleSuffix: Azure Cognitive Services
description: Se cambiaron las API de punto de conexión versión 3. Use esta guía para aprender a migrar a la versión 3 de las API de punto de conexión.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 07/30/2019
ms.author: diberry
ms.openlocfilehash: a61f196a509c3e84b518fffb4eb78f5f7430cb28
ms.sourcegitcommit: fecb6bae3f29633c222f0b2680475f8f7d7a8885
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/30/2019
ms.locfileid: "68667578"
---
# <a name="preview-migrate-to-api-version-3x-for-luis-apps"></a>Vista previa: migración a la API versión 3.x para aplicaciones de LUIS

Se cambiaron las API de punto de conexión para la predicción de consultas. Use esta guía para aprender a migrar a la versión 3 de las API de punto de conexión. 

Esta API v3 proporciona las siguientes características nuevas, que incluyen cambios significativos de solicitud o respuesta JSON: 

* [Entidades externas](#external-entities-passed-in-at-prediction-time)
* [Listas dinámicas](#dynamic-lists-passed-in-at-prediction-time)
* [Cambios de JSON en las entidades precompiladas](#prebuilt-entities-with-new-json)

<!--
* [Multi-intent detection of utterance](#detect-multiple-intents-within-single-utterance)
-->

La [solicitud](#request-changes) y [respuesta](#response-changes) del punto de conexión para predicciones de consultas recibió cambios importantes para admitir las nuevas características enumeradas anteriormente, incluido lo siguiente:

* [Cambios al objeto de respuesta](#top-level-json-changes)
* [Referencias al nombre del rol de entidad en lugar del nombre de entidad](#entity-role-name-instead-of-entity-name)
* [Propiedades para marcar las entidades en expresiones](#marking-placement-of-entities-in-utterances)

Las siguientes características de LUIS **no son compatibles** con la API v3:

* Bing Spell Check v7

Puede consultar la [documentación de referencia](https://aka.ms/luis-api-v3) para la v3.

## <a name="endpoint-url-changes-by-slot-name"></a>Cambios en la dirección URL de punto de conexión por nombre de ranura

Se cambió el formato para la llamada HTTP al punto de conexión v3.

|MÉTODO|URL|
|--|--|
|GET|https://<b>{REGIÓN}</b>.api.cognitive.microsoft.com/luis/<b>v3.0-preview</b>/apps/<b>{ID-DE-APLICACIÓN}</b>/slots/<b>{NOMBRE-DE-RANURA}</b>/predict?query=<b>{CONSULTA}</b>|
|POST|https://<b>{REGIÓN}</b>.api.cognitive.microsoft.com/luis/<b>v3.0-preview</b>/apps/<b>{ID-DE-APLICACIÓN}</b>/slots/<b>{NOMBRE-DE-RANURA}</b>/predict|
|||

Los valores válidos para las ranuras son:

* `production`
* `staging`

## <a name="endpoint-url-changes-by-version-id"></a>Cambios en la dirección URL de punto de conexión por id. de versión

Si realizar una consulta por versión, primero debe [publicarla a través de API](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c3b) con el parámetro `"directVersionPublish":true`. Consulte el punto de conexión que hace referencia al Id. de versión en lugar del nombre de la ranura.


|MÉTODO|URL|
|--|--|
|GET|https://<b>{REGIÓN}</b>.api.cognitive.microsoft.com/luis/<b>v3.0-preview</b>/apps/<b>{ID-DE-APLICACIÓN}</b>/versions/<b>{ID-DE-VERSIÓN}</b>/predict?query=<b>{CONSULTA}</b>|
|POST|https://<b>{REGIÓN}</b>.api.cognitive.microsoft.com/luis/<b>v3.0-preview</b>/apps/<b>{ID-DE-APLICACIÓN}</b>/versions/<b>{ID-DE-VERSIÓN}</b>/predict|
|||

## <a name="prebuilt-entities-with-new-json"></a>Entidades precompiladas con un nuevo JSON

Los cambios en el objeto de respuesta v3 incluyen a las [entidades precompiladas](luis-reference-prebuilt-entities.md). 

## <a name="request-changes"></a>Solicitud de cambios 

### <a name="query-string-parameters"></a>Parámetros de cadena de consulta

La API v3 tiene parámetros de cadena de consulta diferentes.

|Nombre del parámetro|type|Versión|Valor predeterminado|Propósito|
|--|--|--|--|--|
|`log`|boolean|v2 y v3|false|Almacena la consulta en el archivo de registro.| 
|`query`|string|Solo v3|No tiene valor predeterminado: es obligatorio en la solicitud GET.|**En la versión 2**, la expresión que va a predecirse se almacena en el parámetro `q`. <br><br>**En la versión 3**, la funcionalidad se pasa en el parámetro `query`.|
|`show-all-intents`|boolean|Solo v3|false|Devuelve todas las intenciones con la puntuación correspondiente en el objeto **prediction.intents**. Las intenciones se devuelven como objetos en un objeto `intents` primario. Esto habilita el acceso mediante programación sin necesidad de encontrar la intención en una matriz: `prediction.intents.give`. En la versión 2, se devuelven en una matriz. |
|`verbose`|boolean|v2 y v3|false|**En la versión 2**, cuando se establece en true, se devuelven todas las intenciones previstas. Si necesita todas las intenciones previstas, use el parámetro v3 de `show-all-intents`.<br><br>**En la versión 3**, este parámetro solo proporciona detalles de los metadatos de la entidad para la predicción de entidad.  |



<!--
|`multiple-segments`|boolean|V3 only|Break utterance into segments and predict each segment for intents and entities.|
-->


### <a name="the-query-prediction-json-body-for-the-post-request"></a>Cuerpo JSON de la predicción de consulta para la solicitud `POST`

```JSON
{
    "query":"your utterance here",
    "options":{
        "datetimeReference": "2019-05-05T12:00:00",
        "overridePredictions": true
    },
    "externalEntities":[],
    "dynamicLists":[]
}
```

|Propiedad|type|Versión|Valor predeterminado|Propósito|
|--|--|--|--|--|
|`dynamicLists`|array|Solo v3|No se requiere.|Las [listas dinámicas](#dynamic-lists-passed-in-at-prediction-time) permiten extender una entidad de lista existente entrenada y publicada, ya presente en la aplicación de LUIS.|
|`externalEntities`|array|Solo v3|No se requiere.|Las [entidades externas](#external-entities-passed-in-at-prediction-time) permiten que la aplicación de LUIS identifique y etiquete las entidades durante el tiempo de ejecución, que pueden usarse como características para las entidades existentes. |
|`options.datetimeReference`|string|Solo v3|Sin valor predeterminado|Permite determinar el [desplazamiento de datetimeV2](luis-concept-data-alteration.md#change-time-zone-of-prebuilt-datetimev2-entity). El formato de datetimeReference es [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601).|
|`options.overridePredictions`|boolean|Solo v3|false|Especifica si la [entidad externa (con el mismo nombre que la entidad existente)](#override-existing-model-predictions) del usuario o la entidad existente en el modelo se usa para la predicción. |
|`query`|string|Solo v3|Necesario.|**En la versión 2**, la expresión que va a predecirse se almacena en el parámetro `q`. <br><br>**En la versión 3**, la funcionalidad se pasa en el parámetro `query`.|



## <a name="response-changes"></a>Cambios de respuesta

El elemento JSON de la respuesta de consulta se cambió para permitir el acceso mediante programación a los datos usados con más frecuencia. 

### <a name="top-level-json-changes"></a>Cambios JSON de nivel superior

Las siguientes son las principales propiedades JSON para la v2, cuando `verbose` está establecido en true, de forma que devuelve todas las intenciones y sus puntuaciones en la propiedad `intents`:

```JSON
{
    "query":"this is your utterance you want predicted",
    "topScoringIntent":{},
    "intents":[],
    "entities":[],
    "compositeEntities":[]
}
```

Las principales propiedades JSON para la versión 3 son:

```JSON
{
    "query": "this is your utterance you want predicted",
    "prediction":{
        "normalizedQuery": "this is your utterance you want predicted - after normalization",
        "topIntent": "intent-name-1",
        "intents": {}, 
        "entities":{}
    }
}
```

<!--
The `alteredQuery` contains spelling corrections. This corresponds to the V2 API property `alteredQuery`.  
-->

El objeto `intents` es una lista sin ordenar. No suponga que el primer elemento secundario en `intents` corresponde a `topIntent`. En su lugar, use el valor `topIntent` para buscar la puntuación:

```nodejs
const topIntentName = response.prediction.topIntent;
const score = intents[topIntentName];
```

Los cambios en el esquema JSON de la respuesta permiten lo siguiente:

* Una clara distinción entre la expresión original, `query`, y la predicción devuelta, `prediction`.
* Fácil acceso mediante programación a los datos previstos. En lugar de enumerar a través de una matriz en la v2, puede tener acceso a los valores mediante el **nombre** de intenciones y entidades. Para los roles de entidad prevista, se devuelve el nombre del rol porque es único en toda la aplicación.
* Los tipos de datos, si están establecidos, se respetan. Los valores numéricos ya no se devuelven como cadenas.
* Distinción entre la información de predicciones de máxima prioridad y los metadatos adicionales, que se devuelven en el objeto `$instance`. 

### <a name="access-instance-for-entity-metadata"></a>Acceso a `$instance` para obtener metadatos de entidad

Si tiene metadatos de entidad, la cadena de consulta debe usar la marca `verbose=true` y la respuesta contiene los metadatos en el objeto `$instance`. En las respuestas JSON de las secciones siguientes se muestran ejemplos.

### <a name="each-predicted-entity-is-represented-as-an-array"></a>Todas las entidades de predicción se representan como una matriz.

El objeto `prediction.entities.<entity-name>` contiene una matriz, dado que cada entidad se puede predecir más de una vez en la expresión. 

### <a name="list-entity-prediction-changes"></a>Cambios en la predicción de entidad de lista

El código JSON de una predicción de entidad la lista se cambió para que sea una matriz de matrices:

```JSON
"entities":{
    "my_list_entity":[
        ["canonical-form-1","canonical-form-2"],
        ["canonical-form-2"]
    ]
}
```
Cada matriz interior corresponde al texto dentro de la expresión. El objeto interior es una matriz porque el mismo texto puede aparecer en más de una sublista de una entidad de lista. 

Cuando se realiza una asignación del objeto `entities` y el objeto `$instance`, se conserva el orden de los objetos para las predicciones de entidad de lista.

```nodejs
const item = 0; // order preserved, use same enumeration for both
const predictedCanonicalForm = entities.my_list_entity[item];
const associatedMetadata = entities.$instance.my_list_entity[item];
```

### <a name="entity-role-name-instead-of-entity-name"></a>Nombre del rol de entidad en lugar del nombre de entidad 

En la versión 2, la matriz `entities` devuelve todas las entidades previstas, cuyo el nombre de entidad es el identificador único. En la versión 3, si la entidad usa roles y la predicción es para un rol de entidad, el identificador principal es el nombre del rol. Esto es posible porque los nombres de rol de entidad deben ser únicos en toda la aplicación, incluidos otros nombres del modelo (entidad, intención).

En el siguiente ejemplo: considere la posibilidad de que una expresión incluya el texto `Yellow Bird Lane`. Este texto se prevé como el rol `Destination` de una entidad de `Location` personalizada.

|Texto de expresión|Nombre de entidad|Nombre de rol|
|--|--|--|
|`Yellow Bird Lane`|`Location`|`Destination`|

En la versión 2, la entidad se identifica mediante el _nombre de entidad_, con el rol como una propiedad del objeto:

```JSON
"entities":[
    {
        "entity": "Yellow Bird Lane",
        "type": "Location",
        "startIndex": 13,
        "endIndex": 20,
        "score": 0.786378264,
        "role": "Destination"
    }
]
```

En la versión 3, se hace referencia a la entidad mediante el _rol de entidad_ si la predicción es para el rol:

```JSON
"entities":{
    "Destination":[
        "Yellow Bird Lane"
    ]
}
```

En la versión 3, el mismo resultado con la marca `verbose` para devolver los metadatos de entidad:

```JSON
"entities":{
    "Destination":[
        "Yellow Bird Lane"
    ],
    "$instance":{
        "Destination": [
            {
                "role": "Destination",
                "type": "Location",
                "text": "Yellow Bird Lane",
                "startIndex": 25,
                "length":16,
                "score": 0.9837309,
                "modelTypeId": 1,
                "modelType": "Entity Extractor"
            }
        ]
    }
}
```

## <a name="external-entities-passed-in-at-prediction-time"></a>Entidades externas que se pasan al momento de la predicción

Las entidades externas permiten que la aplicación de LUIS identifique y etiquete las entidades durante el tiempo de ejecución, que pueden usarse como características para las entidades existentes. Esto le permite utilizar sus propios extractores de entidades personalizados e independientes antes de enviar consultas al punto de conexión de predicción. Puesto que este proceso se realiza en el punto de conexión de predicción de consulta, no es necesario volver a entrenar y publicar el modelo.

La aplicación cliente proporciona su propio extractor de entidades al administrar la entidad coincidente y determinar su ubicación dentro de la expresión de la entidad coincidente. A continuación, envía esa información con la solicitud. 

Las entidades externas son el mecanismo para extender cualquier tipo de entidad mientras aún está en uso como señal para otros modelos, como roles, compuestos y otros.

Esto resulta útil para una entidad que tiene datos disponibles solo durante el tiempo de ejecución de la predicción de consulta. Los ejemplos de este tipo de datos son los datos en cambio constante o específicos de un usuario. Puede ampliar una entidad de contacto de LUIS con información externa desde la lista de contactos de un usuario. 

### <a name="entity-already-exists-in-app"></a>La entidad ya existe en la aplicación

El valor de `entityName` para la entidad externa (que se pasa en el cuerpo POST de la solicitud de punto de conexión) ya debe existir en la aplicación publicada y entrenada al momento de realizar la solicitud. El tipo de entidad no importa, ya que todos los tipos son compatibles.

### <a name="first-turn-in-conversation"></a>Primera entrega en una conversación

Imagine la primera expresión de la conversación con un bot de chat, en la que un usuario escribe la siguiente información incompleta:

`Send Hazem a new message`

La solicitud del bot de chat a LUIS puede pasar información en el cuerpo POST sobre `Hazem`, por lo que coincide directamente con uno de los contactos del usuario.

```json
    "externalEntities": [
        {
            "entityName":"contacts",
            "startIndex": 5,
            "entityLength": 5,
            "resolution": {
                "employeeID": "05013",
                "preferredContactType": "TeamsChat"
            }
        }
    ]
```

La respuesta de predicción incluye esa entidad externa con todas las demás entidades previstas porque está definida en la solicitud.  

### <a name="second-turn-in-conversation"></a>Segunda entrega en una conversación

La siguiente expresión del usuario para el bot de chat usa un término más vago:

`Send him a calendar reminder for the party.`

En la expresión anterior, se usa `him` como una referencia a `Hazem`. El bot de chat de conversación puede asignar dentro del cuerpo POST a `him` con el valor de entidad extraído de la primera expresión, `Hazem`.

```json
    "externalEntities": [
        {
            "entityName":"contacts",
            "startIndex": 5,
            "entityLength": 3,
            "resolution": {
                "employeeID": "05013",
                "preferredContactType": "TeamsChat"
            }
        }
    ]
```

La respuesta de predicción incluye esa entidad externa con todas las demás entidades previstas porque está definida en la solicitud.  

### <a name="override-existing-model-predictions"></a>Invalidación de las predicciones del modelo existente

La propiedad de opciones `overridePredictions` especifica que si el usuario envía una entidad externa que se superpone con una entidad de predicción con el mismo nombre, LUIS elige la entidad que se pasó o la entidad existente en el modelo. 

Por ejemplo, considere la consulta `today I'm free`. LUIS detecta que `today` es un datetimeV2 con la siguiente respuesta:

```JSON
"datetimeV2": [
    {
        "type": "date",
        "values": [
            {
                "timex": "2019-06-21",
                "value": "2019-06-21"
            }
        ]
    }
]
```

Si el usuario envía la entidad externa:

```JSON
{
    "entityName": "datetimeV2",
    "startIndex": 0,
    "entityLength": 5,
    "resolution": {
        "date": "2019-06-21"
    }
}
```

Si `overridePredictions` está establecida en `false`, LUIS devuelve una respuesta como si la entidad externa no se hubiera enviado. 

```JSON
"datetimeV2": [
    {
        "type": "date",
        "values": [
            {
                "timex": "2019-06-21",
                "value": "2019-06-21"
            }
        ]
    }
]
```

Si `overridePredictions` está establecida en `true`, LUIS devuelve una respuesta que incluye lo siguiente:

```JSON
"datetimeV2": [
    {
        "date": "2019-06-21"
    }
]
```



#### <a name="resolution"></a>Resolución

La propiedad _opcional_ `resolution` se devuelve en la respuesta de predicción, lo que le permite pasar los metadatos asociados con la entidad externa y, a continuación, recibirlos de vuelta en la respuesta. 

La finalidad principal es extender entidades precompiladas, pero no se limita a ese tipo de entidad. 

La propiedad `resolution` puede ser un número, una cadena, un objeto o una matriz:

* "Dallas"
* {"text": "value"}
* 12345 
* ["a", "b", "c"]



## <a name="dynamic-lists-passed-in-at-prediction-time"></a>Listas dinámicas que se pasan al momento de la predicción

Las listas dinámicas permiten extender una entidad de lista existente entrenada y publicada, ya presente en la aplicación de LUIS. 

Use esta característica cuando necesite cambiar periódicamente los valores de la entidad de lista. Esta característica le permite extender una entidad de lista ya entrenada y publicada:

* Cuando se realiza la solicitud de punto de conexión de predicción de consulta.
* Para una sola solicitud.

La entidad de lista puede estar vacía en la aplicación de LUIS, pero debe existir. La entidad de lista en la aplicación de LUIS no cambia, pero la capacidad de predicción en el punto de conexión se extiende para incluir hasta 2 listas con aproximadamente 1000 elementos.

### <a name="dynamic-list-json-request-body"></a>Cuerpo de la solicitud JSON de una lista dinámica

Envíe el siguiente cuerpo JSON para agregar una nuevo sublista con sinónimos a la lista y predecir la entidad de lista del texto `LUIS` mediante la solicitud de predicción de consulta `POST`:

```JSON
{
    "query": "Send Hazem a message to add an item to the meeting agenda about LUIS.",
    "options":{
        "timezoneOffset": "-8:00"
    },
    "dynamicLists": [
        {
            "listEntityName":"ProductList",
            "requestLists":[
                {
                    "name": "Azure Cognitive Services",
                    "canonicalForm": "Azure-Cognitive-Services",
                    "synonyms":[
                        "language understanding",
                        "luis",
                        "qna maker"
                    ]
                }
            ]
        }
    ]
}
```

La respuesta de predicción incluye esa entidad de lista con todas las demás entidades previstas porque está definida en la solicitud. 

## <a name="timezoneoffset-renamed-to-datetimereference"></a>TimezoneOffset se cambió a datetimeReference

**En la versión 2**, el [parámetro](luis-concept-data-alteration.md#change-time-zone-of-prebuilt-datetimev2-entity) `timezoneOffset` se envía en la solicitud de predicción como un parámetro de cadena de consulta, independientemente de si la solicitud se envía como una solicitud GET o POST. 

**En la versión 3** se proporciona la misma funcionalidad con el parámetro de cuerpo POST `datetimeReference`. 

## <a name="marking-placement-of-entities-in-utterances"></a>Selección de la ubicación de las entidades en las expresiones

**En la versión 2**, una entidad se marca dentro de una expresión con `startIndex` y `endIndex`. 

**En la versión 3**, la entidad se marca con `startIndex` y `entityLength`.

## <a name="deprecation"></a>Desuso 

La API v2 no entrará en desuso durante un mínimo de 9 meses después del lanzamiento de la versión preliminar v3. 

## <a name="next-steps"></a>Pasos siguientes

Use la documentación de la API v3 para actualizar las llamadas REST existentes al [punto de conexión](https://aka.ms/luis-api-v3) de LUIS. 