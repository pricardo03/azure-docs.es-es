---
title: Extracción de datos
titleSuffix: Language Understanding - Azure Cognitive Services
description: Extraer datos de texto utterance (dictado) con las intenciones y entidades. Obtenga información sobre qué tipo de datos se puede extraer de Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 04/01/2019
ms.author: diberry
ms.openlocfilehash: 35f1521884de3a4a0971b6e1c00f92a9094a8550
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60812819"
---
# <a name="extract-data-from-utterance-text-with-intents-and-entities"></a>Extraer datos de texto utterance (dictado) con las intenciones y entidades
LUIS ofrece la capacidad de obtener información de expresiones de lenguaje natural de un usuario. La información se extrae de manera que pueda ser usada por un programa, una aplicación o un bot de chat para tomar medidas. En las secciones siguientes, obtendrá información sobre qué datos se devuelven de las intenciones y entidades con ejemplos de JSON.

Los datos más difíciles de extraer son los datos de aprendizaje automático, porque no son una coincidencia de texto exacta. El proceso de extracción de datos de [entidades](luis-concept-entity-types.md) de aprendizaje automático debe formar parte del [ciclo de creación](luis-concept-app-iteration.md) hasta que esté seguro de que recibirá los datos que espera.

## <a name="data-location-and-key-usage"></a>Ubicación de los datos y uso de la clave
LUIS proporciona los datos del [punto de conexión](luis-glossary.md#endpoint) publicado. La **solicitud HTTPS** (POST o GET) contiene la expresión, así como algunas configuraciones opcionales como los entornos de producción o de almacenamiento provisional.

`https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/<appID>?subscription-key=<subscription-key>&verbose=true&timezoneOffset=0&q=book 2 tickets to paris`

El valor de `appID` está disponible en la página **Configuración** de la aplicación LUIS, así como parte de la dirección URL (después de `/apps/`) cuando edite esa aplicación LUIS. El valor de `subscription-key` es la clave de punto de conexión que se ha usado para consultar a la aplicación. Aunque puede usar la clave de inicio o creación gratis mientras se familiariza con LUIS, es importante que cambie la clave del punto de conexión por una clave que admita el [uso esperado de LUIS](luis-boundaries.md#key-limits). La unidad de `timezoneOffset` es minutos.

La **respuesta HTTPS** contiene toda la información de la intención y la entidad que LUIS puede determinar en función del modelo actual publicado de un punto de conexión de producción o de almacenamiento provisional. La dirección URL del punto de conexión se encuentra en el sitio web de [LUIS](luis-reference-regions.md), en la sección **Administrar**, en la página **Claves y puntos de conexión**.

## <a name="data-from-intents"></a>Datos de intenciones
Los datos principales son el **nombre de la intención** de puntuación superior. Mediante el [inicio rápido](luis-quickstart-intents-only.md) `MyStore`, la respuesta del punto de conexión es:

```JSON
{
  "query": "when do you open next?",
  "topScoringIntent": {
    "intent": "GetStoreInfo",
    "score": 0.984749258
  },
  "entities": []
}
```

|Objeto de datos|Tipo de datos|Ubicación de los datos|Value|
|--|--|--|--|
|Intención|String|topScoringIntent.intent|"GetStoreInfo"|

Si el bot de chat o aplicación de llamada a LUIS toma una decisión en función de más de una puntuación de intención, devuelve todas las puntuaciones de intenciones al configurar el parámetro de querystring `verbose=true`. La respuesta del punto de conexión es:

```JSON
{
  "query": "when do you open next?",
  "topScoringIntent": {
    "intent": "GetStoreInfo",
    "score": 0.984749258
  },
  "intents": [
    {
      "intent": "GetStoreInfo",
      "score": 0.984749258
    },
    {
      "intent": "None",
      "score": 0.2040639
    }
  ],
  "entities": []
}
```

Las intenciones se ordenan de mayor a menor puntuación.

|Objeto de datos|Tipo de datos|Ubicación de los datos|Value|Score|
|--|--|--|--|:--|
|Intención|String|intents[0].intent|"GetStoreInfo"|0.984749258|
|Intención|String|intents[1].intent|"None"|0.0168218873|

Si agrega dominios creados previamente, el nombre de la intención indica el dominio, como `Utilties` o `Communication`, así como la intención:

```JSON
{
  "query": "Turn on the lights next monday at 9am",
  "topScoringIntent": {
    "intent": "Utilities.ShowNext",
    "score": 0.07842206
  },
  "intents": [
    {
      "intent": "Utilities.ShowNext",
      "score": 0.07842206
    },
    {
      "intent": "Communication.StartOver",
      "score": 0.0239675418
    },
    {
      "intent": "None",
      "score": 0.0168218873
    }],
  "entities": []
}
```

|Dominio|Objeto de datos|Tipo de datos|Ubicación de los datos|Value|
|--|--|--|--|--|
|Sectores públicos|Intención|String|intents[0].intent|"<b>Utilities</b>.ShowNext"|
|Comunicación|Intención|String|intents[1].intent|<b>Communication</b>.StartOver"|
||Intención|String|intents[2].intent|"None"|


## <a name="data-from-entities"></a>Datos de entidades
La mayoría de bots de chat y aplicaciones necesitan más que el nombre de la intención. Estos datos adicionales y opcionales proceden de las entidades que se han detectado en la expresión. Cada tipo de entidad devuelve otra información sobre la coincidencia.

Una sola palabra o frase en una expresión puede coincidir con más de una entidad. En ese caso, se devuelve cada entidad coincidente con su puntuación.

Se devuelven todas las entidades de la matriz **entities** de la respuesta desde el punto de conexión:

```JSON
"entities": [
  {
    "entity": "bob jones",
    "type": "Name",
    "startIndex": 0,
    "endIndex": 8,
    "score": 0.473899543
  },
  {
    "entity": "3",
    "type": "builtin.number",
    "startIndex": 16,
    "endIndex": 16,
    "resolution": {
      "value": "3"
    }
  }
]
```

## <a name="tokenized-entity-returned"></a>Entidad tokenizada devuelta
Varias [referencias culturales](luis-language-support.md#tokenization) devuelven el objeto entidad con el valor `entity` [tokenizado](luis-glossary.md#token). Los valores de startIndex y endIndex que devuelve LUIS en el objeto de entidad no se asignan al nuevo valor tokenizado, sino a la consulta original para extraer la entidad sin formato mediante programación. 

Por ejemplo, en alemán, la palabra `das Bauernbrot` se tokeniza de esta forma: `das bauern brot`. Se devuelve el valor tokenizado, `das bauern brot`, y se puede determinar mediante programación el valor original de los valores de startIndex y endIndex de la consulta original, lo que le proporciona `das Bauernbrot`.

## <a name="simple-entity-data"></a>Datos de entidad simple

Una [entidad simple](luis-concept-entity-types.md) es un valor de aprendizaje automático. Puede ser una palabra o frase.

`Bob Jones wants 3 meatball pho`

En la expresión anterior, `Bob Jones` se etiqueta como una entidad `Customer` simple.

Los datos devueltos desde el punto de conexión incluyen el nombre de la entidad, el texto de la expresión que se ha detectado, la ubicación del texto detectado y la puntuación:

```JSON
"entities": [
  {
  "entity": "bob jones",
  "type": "Customer",
  "startIndex": 0,
  "endIndex": 8,
  "score": 0.473899543
  }
]
```

|Objeto de datos|Nombre de entidad|Value|
|--|--|--|
|Entidad simple|`Customer`|`bob jones`|

## <a name="hierarchical-entity-data"></a>Datos de entidad jerárquica

**Entidades jerárquicas quedará obsoleto finalmente. Use [roles entidad](luis-concept-roles.md) para determinar los subtipos de entidad, en lugar de entidades jerárquicas.**

Las entidades [jerárquicas](luis-concept-entity-types.md) son de aprendizaje automático y pueden incluir una palabra o frase. Los elementos secundarios se identifican mediante el contexto. Si quiere obtener una relación de elementos primarios y secundarios con coincidencia de texto exacta, use una entidad de [lista](#list-entity-data).

`book 2 tickets to paris`

En la expresión anterior, `paris` se etiqueta como un elemento secundario `Location::ToLocation` de la entidad jerárquica `Location`.

Los datos devueltos desde el punto de conexión incluyen el nombre de la entidad y del elemento secundario, el texto de la expresión que se ha detectado, la ubicación del texto detectado y la puntuación:

```JSON
"entities": [
  {
    "entity": "paris",
    "type": "Location::ToLocation",
    "startIndex": 18,
    "endIndex": 22,
    "score": 0.6866132
  }
]
```

|Objeto de datos|Elemento principal|Elemento secundario|Value|
|--|--|--|--|
|Entidad jerárquica|Location|ToLocation|"paris"|

## <a name="composite-entity-data"></a>Datos de entidad compuesta
Las entidades [compuestas](luis-concept-entity-types.md) son de aprendizaje automático y pueden incluir una palabra o frase. Por ejemplo, considere una entidad compuesta de las entidades `number` y `Location::ToLocation` creadas previamente con la expresión siguiente:

`book 2 tickets to paris`

Fíjese en que `2`, el número, y `paris`, el valor de ToLocation, contienen palabras entre ellos que no forman parte de ninguna de las entidades. El subrayado verde, que se usa en una expresión con etiqueta en el sitio web de [LUIS](luis-reference-regions.md), indica una entidad compuesta.

![Entidad compuesta](./media/luis-concept-data-extraction/composite-entity.png)

Las entidades compuestas se devuelven en una matriz `compositeEntities` y todas las entidades dentro de la composición también se devuelven en la matriz `entities`:

```JSON
  "entities": [
    {
      "entity": "paris",
      "type": "Location::ToLocation",
      "startIndex": 18,
      "endIndex": 22,
      "score": 0.956998169
    },
    {
      "entity": "2",
      "type": "builtin.number",
      "startIndex": 5,
      "endIndex": 5,
      "resolution": {
        "value": "2"
      }
    },
    {
      "entity": "2 tickets to paris",
      "type": "Order",
      "startIndex": 5,
      "endIndex": 22,
      "score": 0.7714499
    }
  ],
  "compositeEntities": [
    {
      "parentType": "Order",
      "value": "2 tickets to paris",
      "children": [
        {
          "type": "builtin.number",
          "value": "2"
        },
        {
          "type": "Location::ToLocation",
          "value": "paris"
        }
      ]
    }
  ]
```    

|Objeto de datos|Nombre de entidad|Value|
|--|--|--|
|Entidad creada previamente de número|"builtin.number"|"2"|
|Entidad jerárquica de ubicación|"Location::ToLocation"|"paris"|

## <a name="list-entity-data"></a>Datos de entidad de lista

Una entidad de [lista](luis-concept-entity-types.md) no está relacionada con el aprendizaje automático. Es una coincidencia de texto exacta. Una lista representa elementos de la lista junto con sinónimos de esos elementos. LUIS marca todas las coincidencias de un elemento de cualquier lista como una entidad en la respuesta. Un sinónimo puede estar en más de una lista.

Imagine que la aplicación tiene una lista, denominada `Cities`, que permite variaciones de nombres de ciudades, incluidos la ciudad del aeropuerto (Sea-tac), el código del aeropuerto (SEA), el código postal (98101) y el código de área telefónica (206).

|Elemento de lista|Sinónimos del elemento|
|---|---|
|Seattle|sea-tac, sea, 98101, 206, +1 |
|París|cdg, roissy, ory, 75001, 1, +33|

`book 2 tickets to paris`

En la expresión anterior, la palabra `paris` se asigna al elemento paris como parte de la entidad de lista `Cities`. La entidad de lista coincide con el nombre normalizado del elemento y con los sinónimos del elemento.

```JSON
"entities": [
  {
    "entity": "paris",
    "type": "Cities",
    "startIndex": 18,
    "endIndex": 22,
    "resolution": {
      "values": [
        "Paris"
      ]
    }
  }
]
```

Otra expresión de ejemplo, con un sinónimo de París:

`book 2 tickets to roissy`

```JSON
"entities": [
  {
    "entity": "roissy",
    "type": "Cities",
    "startIndex": 18,
    "endIndex": 23,
    "resolution": {
      "values": [
        "Paris"
      ]
    }
  }
]
```

## <a name="prebuilt-entity-data"></a>Datos de entidades creadas previamente
Las entidades [creadas previamente](luis-concept-entity-types.md) se detectan en función de una coincidencia de expresión regular mediante el proyecto de código abierto [Recognizers-Text](https://github.com/Microsoft/Recognizers-Text). Las entidades creadas previamente se devuelven en la matriz de entidades y usan el nombre de tipo con el prefijo `builtin::`. El texto siguiente es una expresión de ejemplo con las entidades creadas previamente devueltas:

`Dec 5th send to +1 360-555-1212`

```JSON
"entities": [
    {
      "entity": "dec 5th",
      "type": "builtin.datetimeV2.date",
      "startIndex": 0,
      "endIndex": 6,
      "resolution": {
        "values": [
          {
            "timex": "XXXX-12-05",
            "type": "date",
            "value": "2017-12-05"
          },
          {
            "timex": "XXXX-12-05",
            "type": "date",
            "value": "2018-12-05"
          }
        ]
      }
    },
    {
      "entity": "1",
      "type": "builtin.number",
      "startIndex": 18,
      "endIndex": 18,
      "resolution": {
        "value": "1"
      }
    },
    {
      "entity": "360",
      "type": "builtin.number",
      "startIndex": 20,
      "endIndex": 22,
      "resolution": {
        "value": "360"
      }
    },
    {
      "entity": "555",
      "type": "builtin.number",
      "startIndex": 26,
      "endIndex": 28,
      "resolution": {
        "value": "555"
      }
    },
    {
      "entity": "1212",
      "type": "builtin.number",
      "startIndex": 32,
      "endIndex": 35,
      "resolution": {
        "value": "1212"
      }
    },
    {
      "entity": "5th",
      "type": "builtin.ordinal",
      "startIndex": 4,
      "endIndex": 6,
      "resolution": {
        "value": "5"
      }
    },
    {
      "entity": "1 360 - 555 - 1212",
      "type": "builtin.phonenumber",
      "startIndex": 18,
      "endIndex": 35,
      "resolution": {
        "value": "1 360 - 555 - 1212"
      }
    }
  ]
```

## <a name="regular-expression-entity-data"></a>Datos de entidades de expresiones regulares
Las entidades de [expresiones regulares](luis-concept-entity-types.md) se detectan en función de una coincidencia de expresión regular mediante una expresión que proporcione al crear la entidad. Al usar `kb[0-9]{6}` como la definición de la entidad de expresión regular, la siguiente respuesta JSON es una expresión de ejemplo con las entidades de expresión regular devueltas para la consulta `When was kb123456 published?`:

```JSON
{
  "query": "when was kb123456 published?",
  "topScoringIntent": {
    "intent": "FindKBArticle",
    "score": 0.933641255
  },
  "intents": [
    {
      "intent": "FindKBArticle",
      "score": 0.933641255
    },
    {
      "intent": "None",
      "score": 0.04397359
    }
  ],
  "entities": [
    {
      "entity": "kb123456",
      "type": "KB number",
      "startIndex": 9,
      "endIndex": 16
    }
  ]
}
```

## <a name="extracting-names"></a>Extraer nombres
Obtener nombres de una expresión es difícil porque un nombre puede ser casi cualquier combinación de letras y palabras. En función de qué tipo de nombre vaya a extraer, tiene varias opciones. Las sugerencias siguientes no son reglas a seguir, si no más bien instrucciones.

### <a name="add-prebuilt-personname-and-geographyv2-entities"></a>Agregar las entidades PersonName y GeographyV2 creadas previamente

Las entidades [PersonName](luis-reference-prebuilt-person.md) y [GeographyV2](luis-reference-prebuilt-geographyV2.md) están disponibles en algunas [referencias culturales del idioma](luis-reference-prebuilt-entities.md). 

### <a name="names-of-people"></a>Nombres de personas

Los nombres de personas pueden tener un pequeño formato en función del idioma y la referencia cultural. Usar un precompiladas **[personName](luis-reference-prebuilt-person.md)** entidad o un **[entidad simple](luis-concept-entity-types.md#simple-entity)** con [roles](luis-concept-roles.md) de primera y Apellidos. 

Si usa la entidad simple, asegúrese de que proporcionar ejemplos que usan el nombre y apellidos en distintas partes de la declaración, en las declaraciones de distintas longitudes y grabaciones de voz a través de todas las calidades incluidos ningún intento. [Revise](luis-how-to-review-endoint-utt.md) las expresiones del punto de conexión de forma regular para etiquetar los nombres que no se predijeron correctamente.

### <a name="names-of-places"></a>Nombres de lugares

Los nombres de ubicaciones se establecen y conocen, por ejemplo, ciudades, condados, estados, provincias y países. Usar la entidad creada previamente **[geographyV2](luis-reference-prebuilt-geographyv2.md)** para extraer información de ubicación.

### <a name="new-and-emerging-names"></a>Nombres nuevos y emergentes

Algunas aplicaciones necesitan poder encontrar nombres nuevos y emergentes, como productos o empresas. Estos tipos de nombres son el tipo más difícil de extracción de datos. Comenzar con un **[entidad simple](luis-concept-entity-types.md#simple-entity)** y agregue un [lista de frases](luis-concept-feature.md). [Revise](luis-how-to-review-endoint-utt.md) las expresiones del punto de conexión de forma regular para etiquetar los nombres que no se predijeron correctamente.

## <a name="pattern-roles-data"></a>Datos de roles de patrón
Los roles son diferencias contextuales de entidades.

```JSON
{
  "query": "move bob jones from seattle to redmond",
  "topScoringIntent": {
    "intent": "MoveAssetsOrPeople",
    "score": 0.9999998
  },
  "intents": [
    {
      "intent": "MoveAssetsOrPeople",
      "score": 0.9999998
    },
    {
      "intent": "None",
      "score": 1.02040713E-06
    },
    {
      "intent": "GetEmployeeBenefits",
      "score": 6.12244548E-07
    },
    {
      "intent": "GetEmployeeOrgChart",
      "score": 6.12244548E-07
    },
    {
      "intent": "FindForm",
      "score": 1.1E-09
    }
  ],
  "entities": [
    {
      "entity": "bob jones",
      "type": "Employee",
      "startIndex": 5,
      "endIndex": 13,
      "score": 0.922820568,
      "role": ""
    },
    {
      "entity": "seattle",
      "type": "Location",
      "startIndex": 20,
      "endIndex": 26,
      "score": 0.948008537,
      "role": "Origin"
    },
    {
      "entity": "redmond",
      "type": "Location",
      "startIndex": 31,
      "endIndex": 37,
      "score": 0.7047979,
      "role": "Destination"
    }
  ]
}
```

## <a name="patternany-entity-data"></a>Datos de la entidad Pattern.any
Las entidades Pattern.any son entidades de longitud variable que se usan en expresiones de plantilla de un [patrón](luis-concept-patterns.md).

```JSON
{
  "query": "where is the form Understand your responsibilities as a member of the community and who needs to sign it after I read it?",
  "topScoringIntent": {
    "intent": "FindForm",
    "score": 0.999999464
  },
  "intents": [
    {
      "intent": "FindForm",
      "score": 0.999999464
    },
    {
      "intent": "GetEmployeeBenefits",
      "score": 4.883697E-06
    },
    {
      "intent": "None",
      "score": 1.02040713E-06
    },
    {
      "intent": "GetEmployeeOrgChart",
      "score": 9.278342E-07
    },
    {
      "intent": "MoveAssetsOrPeople",
      "score": 9.278342E-07
    }
  ],
  "entities": [
    {
      "entity": "understand your responsibilities as a member of the community",
      "type": "FormName",
      "startIndex": 18,
      "endIndex": 78,
      "role": ""
    }
  ]
}
```


## <a name="sentiment-analysis"></a>análisis de opiniones
Si el análisis de sentimiento está configurado, la respuesta JSON de LUIS incluye el análisis de sentimiento. Obtenga más información sobre el análisis de sentimiento en la documentación de [Text Analytics](https://docs.microsoft.com/azure/cognitive-services/text-analytics/).

### <a name="sentiment-data"></a>Datos de opinión
Los datos de opinión son una puntuación entre 1 y 0 que indica el valor de opinión positiva (más cercano a 1) o negativa (más cercano a 0) de los datos.

Cuando la referencia cultural es `en-us`, la respuesta es:

```JSON
"sentimentAnalysis": {
  "label": "positive",
  "score": 0.9163064
}
```

Para todas las demás referencias culturales, la respuesta es:

```JSON
"sentimentAnalysis": {
  "score": 0.9163064
}
```


### <a name="key-phrase-extraction-entity-data"></a>Datos de entidad de extracción de frases clave
La entidad de extracción de frases clave devuelve frases clave en la expresión, proporcionadas por [Text Analytics](https://docs.microsoft.com/azure/cognitive-services/text-analytics/).

```JSON
{
  "query": "Is there a map of places with beautiful views on a favorite trail?",
  "topScoringIntent": {
    "intent": "GetJobInformation",
    "score": 0.764368951
  },
  "intents": [
    ...
  ],
  "entities": [
    {
      "entity": "beautiful views",
      "type": "builtin.keyPhrase",
      "startIndex": 30,
      "endIndex": 44
    },
    {
      "entity": "map of places",
      "type": "builtin.keyPhrase",
      "startIndex": 11,
      "endIndex": 23
    },
    {
      "entity": "favorite trail",
      "type": "builtin.keyPhrase",
      "startIndex": 51,
      "endIndex": 64
    }
  ]
}
```

## <a name="data-matching-multiple-entities"></a>Datos que coinciden con varias entidades

LUIS devuelve todas las entidades que ha detectado la expresión. Como consecuencia, es posible que el bot de chat tenga que tomar una decisión en función de los resultados. Una expresión puede tener varias entidades en una expresión:

`book me 2 adult business tickets to paris tomorrow on air france`

El punto de conexión de LUIS puede detectar los mismos datos en diferentes entidades:

```JSON
{
  "query": "book me 2 adult business tickets to paris tomorrow on air france",
  "topScoringIntent": {
    "intent": "BookFlight",
    "score": 1.0
  },
  "intents": [
    {
      "intent": "BookFlight",
      "score": 1.0
    },
    {
      "intent": "Concierge",
      "score": 0.04216196
    },
    {
      "intent": "None",
      "score": 0.03610297
    }
  ],
  "entities": [
    {
      "entity": "air france",
      "type": "Airline",
      "startIndex": 54,
      "endIndex": 63,
      "score": 0.8291798
    },
    {
      "entity": "adult",
      "type": "Category",
      "startIndex": 10,
      "endIndex": 14,
      "resolution": {
        "values": [
          "adult"
        ]
      }
    },
    {
      "entity": "paris",
      "type": "Cities",
      "startIndex": 36,
      "endIndex": 40,
      "resolution": {
        "values": [
          "Paris"
        ]
      }
    },
    {
      "entity": "tomorrow",
      "type": "builtin.datetimeV2.date",
      "startIndex": 42,
      "endIndex": 49,
      "resolution": {
        "values": [
          {
            "timex": "2018-02-21",
            "type": "date",
            "value": "2018-02-21"
          }
        ]
      }
    },
    {
      "entity": "paris",
      "type": "Location::ToLocation",
      "startIndex": 36,
      "endIndex": 40,
      "score": 0.9730773
    },
    {
      "entity": "2",
      "type": "builtin.number",
      "startIndex": 8,
      "endIndex": 8,
      "resolution": {
        "value": "2"
      }
    },
    {
      "entity": "business",
      "type": "Seat",
      "startIndex": 16,
      "endIndex": 23,
      "resolution": {
        "values": [
          "business"
        ]
      }
    },
    {
      "entity": "2 adult business",
      "type": "TicketSeatOrder",
      "startIndex": 8,
      "endIndex": 23,
      "score": 0.8784727
    }
  ],
  "compositeEntities": [
    {
      "parentType": "TicketSeatOrder",
      "value": "2 adult business",
      "children": [
        {
          "type": "Category",
          "value": "adult"
        },
        {
          "type": "builtin.number",
          "value": "2"
        },
        {
          "type": "Seat",
          "value": "business"
        }
      ]
    }
  ]
}
```

## <a name="data-matching-multiple-list-entities"></a>Datos que coinciden con varias entidades de lista

Si una palabra o frase coincide con más de una entidad de lista, la consulta de punto de conexión devolverá todas las entidades de lista.

Para la consulta `when is the best time to go to red rock?`, si la aplicación tiene la palabra `red` en más de una lista, LUIS reconocerá todas las entidades y devolverá una matriz de entidades como parte de la respuesta del punto de conexión JSON: 

```JSON
{
  "query": "when is the best time to go to red rock?",
  "topScoringIntent": {
    "intent": "Calendar.Find",
    "score": 0.06701678
  },
  "entities": [
    {
      "entity": "red",
      "type": "Colors",
      "startIndex": 31,
      "endIndex": 33,
      "resolution": {
        "values": [
          "Red"
        ]
      }
    },
    {
      "entity": "red rock",
      "type": "Cities",
      "startIndex": 31,
      "endIndex": 38,
      "resolution": {
        "values": [
          "Destinations"
        ]
      }
    }
  ]
}
```

## <a name="next-steps"></a>Pasos siguientes

Vea [Add entities](luis-how-to-add-entities.md) (Agregar entidades) para obtener más información sobre cómo agregar entidades a la aplicación de LUIS.
