---
title: 'Idiomas admitidos: LUIS'
titleSuffix: Azure Cognitive Services
description: LUIS tiene una gran variedad de características dentro del servicio. No todas las características están en la misma paridad de lenguaje. Asegúrese de que las características que le interesan se admiten en la referencia cultural del idioma de destino. Una aplicación de LUIS es específica de la referencia cultural y no se puede cambiar después de establecerse.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: diberry
ms.openlocfilehash: 3b1b31da68d821ff27fa30e75ec1522b9d4875c8
ms.sourcegitcommit: f176e5bb926476ec8f9e2a2829bda48d510fbed7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/04/2019
ms.locfileid: "70307431"
---
# <a name="language-and-region-support-for-luis"></a>Compatibilidad de idiomas y regiones para LUIS

LUIS tiene una gran variedad de características dentro del servicio. No todas las características están en la misma paridad de lenguaje. Asegúrese de que las características que le interesan se admiten en la referencia cultural del idioma de destino. Una aplicación de LUIS es específica de la referencia cultural y no se puede cambiar después de establecerse.

## <a name="multi-language-luis-apps"></a>Aplicaciones de LUIS de varios idiomas

Si necesita una aplicación cliente de LUIS multilingüe como un bot de chat, dispone de varias opciones. Si LUIS admite todos los idiomas, desarrolle una aplicación de LUIS para cada uno. Cada aplicación de LUIS tiene un id. de la aplicación único y un registro de punto de conexión. Si tiene que proporcionar la descripción del idioma para un idioma que LUIS no admite, puede usar la [API de Microsoft Translator](../Translator/translator-info-overview.md) para convertir la expresión a un idioma compatible, enviarla al punto de conexión de LUIS y recibir las puntuaciones resultantes.

## <a name="languages-supported"></a>Idiomas admitidos

LUIS entiende expresiones en los idiomas siguientes:

| Idioma |Configuración regional  |  Dominio creado previamente | Entidad creada previamente | Recomendaciones de la lista de frases | \**[Análisis de texto](https://docs.microsoft.com/azure/cognitive-services/text-analytics/text-analytics-supported-languages)<br>(Las opiniones y<br>palabras clave)|
|--|--|:--:|:--:|:--:|:--:|
| Inglés de Estados Unidos |`en-US` | ✔ | ✔  |✔|✔|
| *[Chino](#chinese-support-notes) |`zh-CN` | ✔ | ✔ |✔|-|
| Neerlandés |`nl-NL` |-|  -   |-|✔|
| Francés (Francia) |`fr-FR` |-| ✔ |✔ |✔|
| Francés (Canadá) |`fr-CA` |-|   -   |-|✔|
| Alemán |`de-DE` |-| ✔ |✔ |✔|
| Hindi | `hi-IN`|-|-|-|-|
| Italiano |`it-IT` |-| ✔ |✔|✔|
| *[Japonés](#japanese-support-notes) |`ja-JP` |-| ✔ |✔|Solo la frase clave|
| Coreano |`ko-KR` |-|   -   |-|Solo la frase clave|
| Portugués (Brasil) |`pt-BR` |-| ✔ |✔ |No todas las referencias culturales secundarias|
| Español (España) |`es-ES` |-| ✔ |✔|✔|
| Español (México)|`es-MX` |-|  -   |✔|✔|
| Turco | `tr-TR` |-|-|-|Solo opiniones|


La compatibilidad con idiomas varía para las [entidades creadas previamente](luis-reference-prebuilt-entities.md) y los [dominios creados previamente](luis-reference-prebuilt-domains.md).

### <a name="chinese-support-notes"></a>*Notas de compatibilidad para chino

 - En la referencia cultural `zh-cn`, LUIS espera el juego de caracteres de chino simplificado en lugar del juego de caracteres tradicional.
 - Los nombres de las intenciones, entidades, características y expresiones regulares pueden estar en caracteres chinos o romanos.
 - Consulte la [referencia de dominios creados previamente](luis-reference-prebuilt-domains.md) para obtener información sobre los dominios creados previamente que se admiten en la referencia cultural `zh-cn`.
<!--- When writing regular expressions in Chinese, do not insert whitespace between Chinese characters.-->

### <a name="japanese-support-notes"></a>*Notas de compatibilidad para Japonés

 - Dado que LUIS no proporciona análisis sintáctico y no puede comprender la diferencia entre Keigo y japonés informal, debe incorporar los distintos niveles de formalidad como ejemplos de entrenamiento para las aplicaciones.
     - でございます no es lo mismo que です.
     - です no es lo mismo que だ.

### <a name="text-analytics-support-notes"></a>\*\*Notas de compatibilidad para el análisis de texto
El análisis de texto incluye el análisis de opinión y la entidad precompilada de keyPhrase. Solo se admite el portugués para las subreferencias culturales: `pt-PT` y `pt-BR`. Todas las demás referencias culturales se admiten en el nivel de la referencia cultural principal. Obtenga más información sobre los [idiomas admitidos](https://docs.microsoft.com/azure/cognitive-services/text-analytics/text-analytics-supported-languages) de Text Analytics.

### <a name="speech-api-supported-languages"></a>Idiomas admitidos en Speech API
Vea los [idiomas admitidos](https://docs.microsoft.com/azure/cognitive-services/Speech/api-reference-rest/supportedlanguages##interactive-and-dictation-mode) en Voz para obtener los idiomas de modo de dictado de Voz.

### <a name="bing-spell-check-supported-languages"></a>Idiomas admitidos de Bing Spell Check
Vea los [idiomas admitidos](https://docs.microsoft.com/azure/cognitive-services/bing-spell-check/bing-spell-check-supported-languages) de Bing Spell Check para obtener una lista de los idiomas admitidos y el estado.

## <a name="rare-or-foreign-words-in-an-application"></a>Palabras poco frecuentes o extranjeras en una aplicación
En la referencia cultural `en-us`, LUIS aprende a distinguir la mayoría de las palabras en inglés, incluido el argot. En la referencia cultural `zh-cn`, LUIS aprende a distinguir la mayoría de los caracteres chinos. Si se usa una palabra poco frecuente en `en-us` o un carácter en `zh-cn`, y ve que LUIS parece incapaz de distinguir esa palabra o carácter, puede agregarla a una [característica de lista de frases](luis-how-to-add-features.md). Por ejemplo, las palabras externas a la referencia cultural de la aplicación, es decir, las palabras en otros idiomas, se deben agregar a una característica de lista de frases. Esta lista de frases debería marcarse como no intercambiable, para indicar que el conjunto de palabras poco frecuentes constituye una clase que LUIS debe aprender a reconocer, pero que no son sinónimos ni intercambiables entre sí.

### <a name="hybrid-languages"></a>Idiomas híbridos
Los idiomas híbridos combinan palabras de dos referencias culturales como el inglés y el chino. Estos idiomas no se admiten en LUIS porque una aplicación se basa en una única referencia cultural.

## <a name="tokenization"></a>Tokenización
Para realizar el aprendizaje automático, LUIS divide una expresión en [tokens](luis-glossary.md#token) en función de la referencia cultural.

|Idioma|  todos los espacios o caracteres especiales | nivel de carácter|palabras compuestas|[se devuelve una entidad con tokens](luis-concept-data-extraction.md#tokenized-entity-returned)
|--|:--:|:--:|:--:|:--:|
|Chino||✔||✔|
|Neerlandés|||✔|✔|
|Español (es-es)|✔ ||||
|Francés (fr-FR)|✔||||
|Francés (fr-CA)|✔||||
|Alemán|||✔|✔|
| Hindi |✔|-|-|-|-|
|Italiano|✔||||
|Japonés||||✔|
|Coreano||✔||✔|
|Portugués (Brasil)|✔||||
|Español (es-ES)|✔||||
|Español (es-MX)|✔||||

### <a name="custom-tokenizer-versions"></a>Versiones de tokenizador personalizadas

Las referencias culturales siguientes tienen versiones de tokenizador personalizadas:

|Referencia cultural|Versión|Propósito|
|--|--|--|
|Alemán<br>`de-de`|1.0.0|Acorta las palabras mediante su división por medio de un tokenizador basado en aprendizaje automático que intenta desglosar las palabras compuestas en sus componentes únicos.<br>Si un usuario escribe `Ich fahre einen krankenwagen` como expresión, se convierte en `Ich fahre einen kranken wagen`. Esto permite el marcado de `kranken` y `wagen` por separado como entidades diferentes.|
|Alemán<br>`de-de`|1.0.2|Acorta las palabras mediante su división en espacios.<br> Si un usuario escribe `Ich fahre einen krankenwagen` como expresión, sigue siendo un token único. Por lo tanto, `krankenwagen` se marca como una única entidad. |

### <a name="migrating-between-tokenizer-versions"></a>Migración entre versiones de tokenizador
<!--
Your first choice is to change the tokenizer version in the app file, then import the version. This action changes how the utterances are tokenized but allows you to keep the same app ID. 

Tokenizer JSON for 1.0.0. Notice the property value for  `tokenizerVersion`. 

```JSON
{
    "luis_schema_version": "3.2.0",
    "versionId": "0.1",
    "name": "german_app_1.0.0",
    "desc": "",
    "culture": "de-de",
    "tokenizerVersion": "1.0.0",
    "intents": [
        {
            "name": "i1"
        },
        {
            "name": "None"
        }
    ],
    "entities": [
        {
            "name": "Fahrzeug",
            "roles": []
        }
    ],
    "composites": [],
    "closedLists": [],
    "patternAnyEntities": [],
    "regex_entities": [],
    "prebuiltEntities": [],
    "model_features": [],
    "regex_features": [],
    "patterns": [],
    "utterances": [
        {
            "text": "ich fahre einen krankenwagen",
            "intent": "i1",
            "entities": [
                {
                    "entity": "Fahrzeug",
                    "startPos": 23,
                    "endPos": 27
                }
            ]
        }
    ],
    "settings": []
}
```

Tokenizer JSON for version 1.0.1. Notice the property value for  `tokenizerVersion`. 

```JSON
{
    "luis_schema_version": "3.2.0",
    "versionId": "0.1",
    "name": "german_app_1.0.1",
    "desc": "",
    "culture": "de-de",
    "tokenizerVersion": "1.0.1",
    "intents": [
        {
            "name": "i1"
        },
        {
            "name": "None"
        }
    ],
    "entities": [
        {
            "name": "Fahrzeug",
            "roles": []
        }
    ],
    "composites": [],
    "closedLists": [],
    "patternAnyEntities": [],
    "regex_entities": [],
    "prebuiltEntities": [],
    "model_features": [],
    "regex_features": [],
    "patterns": [],
    "utterances": [
        {
            "text": "ich fahre einen krankenwagen",
            "intent": "i1",
            "entities": [
                {
                    "entity": "Fahrzeug",
                    "startPos": 16,
                    "endPos": 27
                }
            ]
        }
    ],
    "settings": []
}
```
-->

La tokenización se produce en el nivel de aplicación. No hay ninguna compatibilidad con la tokenización de nivel de versión. 

[Importe el archivo como una nueva aplicación](luis-how-to-start-new-app.md#import-an-app-from-file), en lugar de una versión. Esta acción significa que la nueva aplicación tiene un identificador de aplicación diferente, pero usa la versión de tokenizador especificada en el archivo. 