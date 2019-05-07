---
title: V2 para la migración de la API de V3
titleSuffix: Azure Cognitive Services
description: El punto de conexión de la versión 3 que han cambiado las API. Use esta guía para comprender cómo migrar a las API de punto de conexión de la versión 3.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 05/07/2019
ms.author: diberry
ms.openlocfilehash: d0b5537f31edf330b54b91ddf7268338df0c0a6b
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65148273"
---
# <a name="preview-migrate-to-api-version-3x--for-luis-apps"></a>Vista previa: Migrar a la API versión 3.x para aplicaciones de LUIS

El punto de conexión de predicción de consulta han cambiado las API. Use esta guía para comprender cómo migrar a las API de punto de conexión de la versión 3. 

Esta API V3 proporciona las siguientes características nuevas, que incluyen cambios significativos de solicitud o respuesta JSON: 

* [Entidades externas](#external-entities-passed-in-at-prediction-time)
* [Listas dinámicas](#dynamic-lists-passed-in-at-prediction-time)
* [Cambios de JSON de entidades precompiladas](#prebuilt-entities-with-new-json)

<!--
* [Multi-intent detection of utterance](#detect-multiple-intents-within-single-utterance)
-->

El punto de conexión de consulta predicción [solicitud](#request-changes) y [respuesta](#response-changes) tiene cambios importantes para admitir las nuevas características enumeradas anteriormente, incluido lo siguiente:

* [Cambios del objeto de respuesta](#top-level-json-changes)
* [Referencias de nombre de rol de entidad en lugar del nombre de entidad](#entity-role-name-instead-of-entity-name)
* [Propiedades de para marcar las entidades de grabaciones de voz](#marking-placement-of-entities-in-utterances)

Son las siguientes características de LUIS **no admite** en la API de V3:

* Bing Spell Check V7

[Documentación de referencia](https://aka.ms/luis-api-v3) está disponible para V3.

## <a name="prebuilt-domains-with-new-models-and-language-coverage"></a>Dominios creados previamente con los nuevos modelos y la cobertura de idiomas

Revise el [lista de API de V3 de dominios creados previamente](luis-reference-prebuilt-domains.md). Estos dominios son más completos, tanto en el modelo y la cobertura de idiomas. 

## <a name="prebuilt-entities-with-new-json"></a>Entidades precompiladas con JSON nuevo

Los cambios de objeto de respuesta V3 incluyen [entidades precompiladas](luis-reference-prebuilt-entities.md). 

## <a name="request-changes"></a>Solicitud de cambios 

### <a name="query-string-parameters"></a>Parámetros de cadena de consulta

La API de V3 tiene parámetros de cadena de consulta diferentes.

|Nombre de parámetro|Type|Version|Propósito|
|--|--|--|--|
|`query`|string|Solo v3|**En la versión 2**, se encuentra en la declaración para predecir la `q` parámetro. <br><br>**En V3**, la funcionalidad que se pasa en el `query` parámetro.|
|`show-all-intents`|boolean|Solo v3|Devolver todos los intentos con el resultado correspondiente en el **prediction.intents** objeto. Las intenciones se devuelven como objetos en un elemento primario `intents` objeto. Esto permite el acceso mediante programación sin necesidad de encontrar la intención en una matriz: `prediction.intents.give`. En la versión 2, estos se devuelven en una matriz. |
|`verbose`|boolean|V2 Y V3|**En la versión 2**, cuando se establece en true, predecir todos los intentos se han devuelto. Si necesita intenciones todos los previstos, use el parámetro V3 de `show-all-intents`.<br><br>**En V3**, este parámetro solo proporciona entidad detalles de los metadatos de la predicción de la entidad.  |

<!--
|`multiple-segments`|boolean|V3 only|Break utterance into segments and predict each segment for intents and entities.|
-->


### <a name="the-query-prediction-json-body-for-the-post-request"></a>La predicción de consulta JSON del cuerpo para el `POST` solicitud

```JSON
{
    "query":"your utterance here",
    "options":{
        "timezoneOffset": "-8:00"
    },
    "externalEntities":[],
    "dynamicLists":[]
}
```

## <a name="response-changes"></a>Cambios de respuesta

La respuesta de consulta JSON cambiado para permitir el acceso mediante programación mayor a los datos usados con más frecuencia. 

### <a name="top-level-json-changes"></a>Cambios JSON de nivel superiores

Son las principales propiedades JSON para la versión 2, cuando `verbose` está establecida en true, que devuelve todas las intenciones y sus puntuaciones en la `intents` propiedad:

```JSON
{
    "query":"this is your utterance you want predicted",
    "topScoringIntent":{},
    "intents":[],
    "entities":[],
    "compositeEntities":[]
}
```

Las principales propiedades JSON para V3 son:

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

La `intents` objeto es una lista sin ordenar. No suponga que el primer elemento secundario en el `intents` corresponde a la `topIntent`. En su lugar, use la `topIntent` valor para buscar la puntuación:

```nodejs
const topIntentName = response.prediction.topIntent;
const score = intents[topIntentName];
```

Permitir que los cambios de esquema de respuesta JSON para:

* Desactive la distinción entre la declaración original, `query`y devuelve la predicción, `prediction`.
* Fácil acceso mediante programación a los datos previstos. En lugar de enumerar a través de una matriz en la versión 2, puede tener acceso a los valores por **denominado** de intenciones y entidades. Para las funciones de predicción de entidad, se devuelve el nombre del rol porque es único en toda la aplicación.
* Tipos de datos, si determinado, se respetan. Ya no se devuelven valores numéricos como cadenas.
* Distinción entre la primera información de predicción de prioridad y metadatos adicionales, se devuelven en el `$instance` objeto. 

### <a name="access-instance-for-entity-metadata"></a>Acceso `$instance` para metadatos de entidad

Si tiene metadatos de entidad, la cadena de consulta debe usar el `verbose=true` marca y la respuesta contiene los metadatos en el `$instance` objeto. En las respuestas JSON en las secciones siguientes se muestran ejemplos.

### <a name="each-predicted-entity-is-represented-as-an-array"></a>Cada entidad de predicción se representa como una matriz

La `prediction.entities.<entity-name>` objeto contiene una matriz, dado que cada entidad se puede predecir más de una vez en la declaración. 

### <a name="list-entity-prediction-changes"></a>Cambios de predicción de entidad de lista

El JSON de una predicción de entidad de la lista ha cambiado para que sea una matriz de matrices:

```JSON
"entities":{
    "my_list_entity":[
        ["canonical-form-1","canonical-form-2"],
        ["canonical-form-2"]
    ]
}
```
Cada matriz interior corresponde al texto dentro de la declaración. El objeto interior es una matriz porque el mismo texto puede aparecer en más de una sublista de una entidad de la lista. 

Cuando la asignación entre el `entities` de objeto para el `$instance` de objeto, se conserva el orden de los objetos de las predicciones de entidad de la lista.

```nodejs
const item = 0; // order preserved, use same enumeration for both
const predictedCanonicalForm = entities.my_list_entity[item];
const associatedMetadata = entities.$instance.my_list_entity[item];
```

### <a name="entity-role-name-instead-of-entity-name"></a>Nombre de rol de entidad en lugar del nombre de entidad 

En la versión 2, el `entities` matriz devuelve todas las entidades de predicción con el nombre de entidad es el identificador único. En V3, si la entidad usa roles y la predicción es para un rol de entidad, el identificador principal es el nombre del rol. Esto es posible porque los nombres de rol de entidad deben ser únicos en toda la aplicación, incluidos otros nombres de modelo (entity intención).

En el siguiente ejemplo: considere la posibilidad de una declaración que incluye el texto, `Yellow Bird Lane`. Este texto se prevé sea un personalizado `Location` rol de la entidad de `Destination`.

|Texto utterance (dictado)|Nombre de entidad|Nombre de rol|
|--|--|--|
|`Yellow Bird Lane`|`Location`|`Destination`|

En la versión 2, la entidad se identifica mediante el _nombre de entidad_ con el rol como una propiedad del objeto:

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

En V3, se hace referencia a la entidad por el _rol de entidad_, si la predicción es para el rol:

```JSON
"entities":{
    "Destination":[
        "Yellow Bird Lane"
    ]
}
```

En V3, el mismo resultado con el `verbose` marca para devolver los metadatos de entidad:

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

## <a name="external-entities-passed-in-at-prediction-time"></a>Entidades externas que se pasa en tiempo de predicción

Las entidades externas conceder a la aplicación de LUIS la capacidad para identificar y etiquetar las entidades en tiempo de ejecución, que puede usarse como características para las entidades existentes. Esto le permite utilizar sus propios extractores personalizados e independiente de las entidades antes de enviar consultas al extremo de la predicción. Puesto que se realiza en el punto de conexión de predicción de consulta, no es necesario volver a entrenar y publicar el modelo.

La aplicación cliente proporciona su propio extractor de entidades mediante la administración de la entidad coincidente y determinar la ubicación dentro de la declaración de la entidad coincidente y, a continuación, enviar esa información con la solicitud. 

Las entidades externas son el mecanismo para extender cualquier tipo de entidad mientras aún está en uso como señales a otros modelos, como roles, compuesto y otros.

Esto es útil para una entidad que tiene datos disponibles sólo en tiempo de ejecución de predicción de consulta. Ejemplos de este tipo de datos cambian constantemente datos o específicas por usuario. Puede ampliar una entidad contact de LUIS con información externa desde la lista de contactos de un usuario. 

### <a name="entity-already-exists-in-app"></a>Entidad ya existe en la aplicación

El valor de `entityName` para la entidad externa, pasada la solicitud del extremo del cuerpo POST, ya debe existir en la aplicación publicada y entrenada en el momento en que se realiza la solicitud. Se admiten todos los tipos, no importa el tipo de entidad.

### <a name="first-turn-in-conversation"></a>Convertir conversación

Tenga en cuenta una primera declaración en una conversación de bot de chat donde un usuario escribe la siguiente información incompleta:

`Send Hazem a new message`

La solicitud desde el bot de chat a LUIS puede pasar información en el cuerpo de POST sobre `Hazem` por lo que se compara directamente como uno de los contactos del usuario.

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

La respuesta de predicción incluye esa entidad externa, con todas las demás predichas entidades, porque está definido en la solicitud.  

### <a name="second-turn-in-conversation"></a>En segundo lugar, gire en la conversación

La siguiente declaración de usuario en el bot de chat usa un término más vago:

`Send him a calendar reminder for the party.`

En la declaración anterior, se usa la declaración `him` como una referencia a `Hazem`. Puede asignar el bot de chat de conversación, en el cuerpo POST, `him` para el valor de la entidad extraído de la primera declaración, `Hazem`.

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

La respuesta de predicción incluye esa entidad externa, con todas las demás predichas entidades, porque está definido en la solicitud.  

#### <a name="resolution"></a>Resolución

El _opcional_ `resolution` propiedad devuelve en la respuesta de predicción, lo que le permite pasar en los metadatos asociados con la entidad externa, a continuación, reciben revertir en la respuesta. 

El propósito principal es ampliar entidades precompiladas pero no se limita a ese tipo de entidad. 

El `resolution` propiedad puede ser un número, una cadena, un objeto o una matriz:

* "Dallas"
* {"text": "value"}
* 12345 
* ["a", "b", "c"]


## <a name="dynamic-lists-passed-in-at-prediction-time"></a>Listas dinámicas que se pasa en tiempo de predicción

Las listas dinámicas permiten extender una entidad existente de la lista entrenado y publicado, ya está en la aplicación de LUIS. 

Utilice esta característica cuando necesitan cambiar periódicamente sus valores de entidad de la lista. Esta característica le permite ampliar una entidad de la lista ya entrenado y publicada:

* En el momento de la solicitud de punto de conexión de predicción de consulta.
* Para una única solicitud.

La entidad de la lista puede estar vacía en la aplicación de LUIS, pero tiene que existir. No se cambia la entidad de la lista en la aplicación de LUIS, pero la capacidad de predicción en el punto de conexión se extiende para incluir hasta 2 listas con aproximadamente 1.000 elementos.

### <a name="dynamic-list-json-request-body"></a>Lista dinámica de cuerpo de solicitud JSON

En el siguiente cuerpo JSON de envío para agregar una nuevo sublista con sinónimos a la lista y predecir la entidad de la lista para el texto, `LUIS`, con el `POST` solicitud de consulta de predicción:

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

La respuesta de predicción incluye esa entidad de lista con todas las demás predichas entidades, porque está definido en la solicitud. 

## <a name="timezoneoffset-renamed-to-datetimereference"></a>TimezoneOffset ha cambiado a datetimeReference

**En la versión 2**, `timezoneOffset` [parámetro](luis-concept-data-alteration.md#change-time-zone-of-prebuilt-datetimev2-entity) se envía en la solicitud de predicción como un parámetro de cadena de consulta, sin importar si la solicitud se envía como una solicitud GET o POST. 

**En V3**, se proporciona la misma funcionalidad con el parámetro de cuerpo POST, `datetimeReference`. 

## <a name="marking-placement-of-entities-in-utterances"></a>Marcar selección de ubicación de las entidades de grabaciones de voz

**En la versión 2**, una entidad se ha marcado como en una declaración con el `startIndex` y `endIndex`. 

**En V3**, la entidad está marcada con `startIndex` y `entityLength`.


## <a name="next-steps"></a>Pasos siguientes

Use la documentación de API de V3 REST existente de actualizar las llamadas a LUIS [extremo](https://aka.ms/luis-api-v3) API. 