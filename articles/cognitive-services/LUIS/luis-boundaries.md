---
title: Límites de Language Understanding (LUIS) | Microsoft Docs
titleSuffix: Azure
description: Este artículo contiene los límites conocidos de LUIS.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: diberry
ms.openlocfilehash: ea4f31094e27f1abbe57c212f262845fd16dd984
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/24/2018
ms.locfileid: "39225285"
---
# <a name="luis-boundaries"></a>Límites de LUIS
LUIS tiene varias áreas de límites. La primera es el [límite de modelo](#model-boundaries), que controla las intenciones, entidades y características de LUIS. La segunda área son los [límites de cuota](#key-limits) según el tipo de clave. Una tercera área de límites es la [combinación de teclas](#keyboard-controls), para controlar el sitio web de LUIS. Una cuarta área es la [asignación de regiones del mundo](luis-reference-regions.md) entre el sitio web de creación de LUIS y las API de [punto de conexión](luis-glossary.md#endpoint) de LUIS. 


## <a name="model-boundaries"></a>Límites de modelo

|Ámbito|Límite|
|--|:--|--|
| [Nombre de aplicación][luis-get-started-create-app] | *Máximo de caracteres predeterminado |
| [Pruebas por lotes][batch-testing]| 10 conjuntos de datos, 1000 expresiones por cada conjunto de datos|
| **[Composición](./luis-concept-entity-types.md)|100 con un máximo de 10 elementos secundarios |
| Lista explícita | 50 por aplicación|
| **[Jerárquico](./luis-concept-entity-types.md) |100 con un máximo de 10 elementos secundarios |
| [Intenciones][intents]|500 por aplicación<br>La aplicación [por distribución](https://github.com/Microsoft/botbuilder-tools/tree/master/Dispatch) tiene 500 orígenes de distribución correspondientes|
| [Listar entidades](./luis-concept-entity-types.md) | Elementos primarios: 50, secundarios: 20 000. El nombre canónico es *Máximo de caracteres predeterminado. Los sinónimos no tienen ninguna restricción de longitud. |
| [Patrones](luis-concept-patterns.md)|500 patrones por aplicación.<br>El patrón puede contener 400 caracteres como máximo.<br>3 entidades Pattern.any por patrón<br>Máximo de 2 textos opcionales anidados en el patrón|
| [Pattern.any](./luis-concept-entity-types.md)|100 por aplicación, 3 entidades Pattern.any por patrón |
| [Lista de frases][phrase-list]|10 listas de frases, 5000 elementos por lista|
| [Entidades precompiladas](./luis-prebuilt-entities.md) | ilimitado|
| [Entidades de expresión regular](./luis-concept-entity-types.md)|20 entidades<br>Máximo de 500 caracteres por patrón de la entidad de expresiones regulares|
| [Roles](luis-concept-roles.md)|300 roles por aplicación. 10 roles por entidad|
| **[Simple](./luis-concept-entity-types.md)| 100 entidades|
| [Expresión][utterances] | 500 caracteres|
| [Expresiones][utterances] | 15 000 por aplicación|
| [Nombre de la versión][luis-how-to-manage-versions] | 10 caracteres, restringido a caracteres alfanuméricos y punto (.) |

*El valor máximo predeterminado de caracteres es 50. 

**El número total de entidades simples, jerárquicas y compuestas no puede ser superior a 100. El número total de entidades jerárquicas, entidades compuestas, entidades sencillas y entidades de elementos secundarios jerárquicas no ser superior a 330. 

## <a name="intent-and-entity-naming"></a>Nomenclatura de intenciones y entidades
No use los siguientes caracteres en los nombres de intenciones y entidades:

|Character|NOMBRE|
|--|--|
|`{`|Llave de apertura|
|`}`|Llave de cierre|
|`[`|Corchete de apertura|
|`]`|Corchete de cierre|
|`\`|Barra diagonal inversa|

## <a name="key-limits"></a>Límites de la clave
La clave de creación tiene distintos límites para la creación y el punto de conexión. La clave de punto de conexión de servicio de LUIS solo es válida para las consultas del punto de conexión.

|Clave|Creación|Punto de conexión|Propósito|
|--|--|--|--|
|Creación/Inicio|1 millón/mes, 5/segundo|1000/mes, 5/segundo|Creación de la aplicación de LUIS|
|[Suscripción][pricing]: F0: nivel gratis |no válido|10 000/mes, 5/segundo|Consultar el punto de conexión de LUIS|
|[Suscripción][pricing]: S0: nivel básico|no válido|50/segundo|Consultar el punto de conexión de LUIS|
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
