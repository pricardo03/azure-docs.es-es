---
title: límites
titleSuffix: Language Understanding - Azure Cognitive Services
description: En este artículo contiene los límites conocidos de Language Understanding (LUIS) de Azure Cognitive Services. LUIS tiene varias áreas de límites. El límite de modelo controla las intenciones, entidades y características de LUIS. Los límites de cuota se basan en el tipo de clave. La combinación de teclado controla el sitio web de LUIS.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 04/18/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: 357ed4c42cc2758766b9ccd45a3fafa541338d11
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65154572"
---
# <a name="boundaries-for-your-luis-model-and-keys"></a>Límites de las claves y el modelo de LUIS
LUIS tiene varias áreas de límites. La primera es el [límite de modelo](#model-boundaries), que controla las intenciones, entidades y características de LUIS. La segunda área son los [límites de cuota](#key-limits) según el tipo de clave. Una tercera área de límites es la [combinación de teclas](#keyboard-controls), para controlar el sitio web de LUIS. Una cuarta área es la [asignación de regiones del mundo](luis-reference-regions.md) entre el sitio web de creación de LUIS y las API de [punto de conexión](luis-glossary.md#endpoint) de LUIS. 


## <a name="model-boundaries"></a>Límites de modelo

Si la aplicación supera los límites del modelo LUIS, puede usar una aplicación para [envíos LUIS](luis-concept-enterprise.md#dispatch-tool-and-model) o un [contenedor LUIS](luis-container-howto.md). 

|Ámbito|Límite|
|--|:--|
| [Nombre de aplicación][luis-get-started-create-app] | *Máximo de caracteres predeterminado |
| [Pruebas por lotes][batch-testing]| 10 conjuntos de datos, 1000 expresiones por cada conjunto de datos|
| Lista explícita | 50 por aplicación|
| Entidades externas | sin límites |
| [Intenciones][intents]|500 por aplicación: 499 intenciones personalizadas y la intención _Ninguno_ necesaria.<br>La aplicación [basada en la distribución](https://aka.ms/dispatch-tool) tiene 500 orígenes de distribución correspondientes.|
| [Listar entidades](./luis-concept-entity-types.md) | Elemento principal: 50, elemento secundario: 20 000 elementos. El nombre canónico es *Máximo de caracteres predeterminado. Los valores de los sinónimos no tienen ninguna restricción de longitud. |
| [Entidades con aprendizaje automático y roles](./luis-concept-entity-types.md):<br> compuesta,<br>simple,<br>rol de entidad|Un límite de 100 entidades primarias o 330 entidades, el que sea que alcance primero el usuario. Un rol cuenta como una entidad para los fines de este límite. Un ejemplo es una composición con una entidad sencilla que tiene 2 roles: 1 compuesta + 1 simple + 2 roles = 4 de las 330 entidades.|
| [Versión preliminar: entidades de lista dinámica](https://aka.ms/luis-api-v3-doc#dynamic-lists-passed-in-at-prediction-time)|2 listas de ~1 K por solicitud de punto de conexión de predicción de consulta|
| [Patrones](luis-concept-patterns.md)|500 patrones por aplicación.<br>El patrón puede contener 400 caracteres como máximo.<br>3 entidades Pattern.any por patrón<br>Máximo de 2 textos opcionales anidados en el patrón|
| [Pattern.any](./luis-concept-entity-types.md)|100 por aplicación, 3 entidades Pattern.any por patrón |
| [Lista de frases][phrase-list]|10 listas de frases, 5000 elementos por lista|
| [Entidades precompiladas](./luis-prebuilt-entities.md) | ilimitado|
| [Entidades de expresión regular](./luis-concept-entity-types.md)|20 entidades<br>Máximo de 500 caracteres por patrón de la entidad de expresiones regulares|
| [Roles](luis-concept-roles.md)|300 roles por aplicación. 10 roles por entidad|
| [Expresión][utterances] | 500 caracteres|
| [Expresiones][utterances] | 15 000 por aplicación: no hay ningún límite en el número de expresiones por intención|
| [Versiones](luis-concept-version.md)| ilimitado |
| [Nombre de la versión][luis-how-to-manage-versions] | 10 caracteres, restringido a caracteres alfanuméricos y punto (.) |

*El valor máximo predeterminado de caracteres es 50. 

<a name="intent-and-entity-naming"></a>

## <a name="object-naming"></a>Denominación de objeto

No use los siguientes caracteres en los siguientes nombres.

|Objeto|Excluir caracteres|
|--|--|
|Nombres de intención, entidad y rol|`:`<br>`$`|
|Nombre de la versión|`\`<br> `/`<br> `:`<br> `?`<br> `&`<br> `=`<br> `*`<br> `+`<br> `(`<br> `)`<br> `%`<br> `@`<br> `$`<br> `~`<br> `!`<br> `#`|

## <a name="key-usage"></a>Uso de las claves

Language Understanding tiene claves independientes, un tipo para la creación y un tipo para las consultas en el punto de conexión de la predicción. Para más información sobre las diferencias entre los tipos de claves, consulte [Claves de creación y del punto de conexión de consulta de predicciones en LUIS](luis-concept-keys.md).

## <a name="key-limits"></a>Límites de la clave

La clave de creación tiene distintos límites para la creación y el punto de conexión. La clave de punto de conexión de servicio de LUIS solo es válida para las consultas del punto de conexión.


|Clave|Creación|Punto de conexión|Propósito|
|--|--|--|--|
|Creación/Inicio de Language Understanding|1 millón/mes, 5/segundo|1000/mes, 5/segundo|Creación de la aplicación de LUIS|
|[Suscripción] de Language Understanding[pricing] - F0 - nivel Gratis |no válido|10 000/mes, 5/segundo|Consultar el punto de conexión de LUIS|
|[Suscripción] de Language Understanding[pricing] - S0 - nivel Básico|no válido|50/segundo|Consultar el punto de conexión de LUIS|
|[Suscripción] de Cognitive Services[pricing] - S0 - nivel Estándar|no válido|50/segundo|Consultar el punto de conexión de LUIS|
|[Integración del análisis de sentimiento](luis-how-to-publish-app.md#enable-sentiment-analysis)|no válido|sin cargo|Agregar información de sentimiento, incluida la extracción de datos de frases clave |
|Integración de voz|no válido|5,50 USD/1000 solicitudes de punto de conexión|Convertir las expresiones habladas en expresiones de texto y devolver resultados de LUIS|

## <a name="keyboard-controls"></a>Controles de teclado

|Entrada de teclado | DESCRIPCIÓN | 
|--|--|
|Control+E|cambia entre los tokens y entidades en la lista de expresiones|

## <a name="website-sign-in-time-period"></a>Período de tiempo de inicio de sesión en el sitio web

El acceso de inicio de sesión es válido durante **60 minutos**. Después de este tiempo, recibirá este error. Debe volver a iniciar sesión.

[luis-get-started-create-app]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-get-started-create-app
[batch-testing]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-test#batch-testing
[intents]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-intent
[phrase-list]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-feature
[utterances]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-utterance
[luis-how-to-manage-versions]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-how-to-manage-versions
[pricing]: https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/
<!-- TBD: fix this link -->
[speech-to-intent-pricing]: https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/
