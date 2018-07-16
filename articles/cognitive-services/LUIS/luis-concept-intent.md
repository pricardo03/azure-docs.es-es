---
title: Reconocimiento de las intenciones en las aplicaciones de LUIS en Azure | Microsoft Docs
description: Describe qué intenciones están en las aplicaciones de Language Understanding Intelligent Service (LUIS).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/04/2018
ms.author: v-geberr
ms.openlocfilehash: 5c2feb0240b676d4e106cbda65aaaed7604a35c5
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/19/2018
ms.locfileid: "36265159"
---
# <a name="intents-in-luis"></a>Intenciones en LUIS

Una intención representa una tarea o acción que el usuario quiere realizar. Es una finalidad o un objetivo que se expresa en la [expresión](luis-concept-utterance.md) de un usuario.

Defina un conjunto de intenciones que se correspondan con las acciones que los usuarios quieren realizar en la aplicación. Por ejemplo, una aplicación de viajes define varias intenciones:

Intenciones de la aplicación de viajes   |   Expresiones de ejemplo   | 
------|------|
 BookFlight     |   "Resérvame un vuelto a Río la próxima semana" <br/> "Llévame a Río en avión el 24" <br/> "Necesito un billete de avión para el próximo domingo a Río de Janeiro"    |
 Greeting     |   "Buenas" <br/>"Hola" <br/>"Buenos días"  |
 CheckWeather | "¿Qué tiempo hace en Boston?" <br/> "Muéstrame la previsión de este fin de semana" |
 None         | "Dame una receta de galletas"<br>"¿Ganaron los Lakers?" |

Todas las aplicaciones vienen con la intención predefinida "[None](#none-intent-is-fallback-for-app)", que es la intención de reserva. 

## <a name="prebuilt-domains-provide-intents"></a>Los dominios creados previamente proporcionan intenciones
Además de las intenciones que defina, puede usar intenciones creadas previamente desde uno de los dominios creados previamente. Para obtener más información, vea [Uso de dominios creados previamente en aplicaciones de LUIS](luis-how-to-use-prebuilt-domains.md) para conocer cómo personalizar las intenciones de un dominio creado previamente para usarlas en su aplicación.

## <a name="return-all-intents-scores"></a>Devolver las puntuaciones de todas las intenciones
Asigne una expresión a una única intención. Cuando LUIS recibe una expresión en el punto de conexión, devuelve la intención superior para esa expresión. Si quiere puntuaciones para todas las intenciones de la expresión, puede proporcionar la marca `verbose=true` en la cadena de consulta de la [llamada de punto de conexión](https://aka.ms/v1-endpoint-api-docs) de la API. 

## <a name="intent-compared-to-entity"></a>Comparación de intención con entidad
La intención representa la acción que el bot de chat debe realizar para el usuario y se basa en la expresión completa. La entidad representa palabras o frases que se encuentran dentro de la expresión. Una expresión solo puede tener una intención de puntuación superior, pero puede tener muchas entidades. 

<a name="how-do-intents-relate-to-entities"></a> Cree una intención cuando la _intención_ del usuario desencadenaría una acción en la aplicación cliente, como una llamada a la función checkweather(). Después, cree una entidad para representar los parámetros necesarios para ejecutar la acción. 

|Intención de ejemplo   | Entidad | Entidad en las expresiones de ejemplo   | 
|------------------|------------------------------|------------------------------|
| CheckWeather | { "type": "location", "entity": "seattle" }<br>{ "type": "builtin.datetimeV2.date","entity": "tomorrow","resolution":"2018-05-23" } | What's the weather like in `Seattle` `tomorrow`? (¿Qué tiempo hará en Seattle mañana?) |
| CheckWeather | { "type": "date_range", "entity": "this weekend" } | Show me the forecast for `this weekend` (Muéstrame la previsión de este fin de semana) | 

## <a name="custom-intents"></a>Intenciones personalizadas

De igual forma, las [expresiones](luis-concept-utterance.md) con intención se corresponden a una única intención. Las expresiones de la intención pueden usar cualquier [entidad](luis-concept-entity-types.md) en la aplicación, ya que las entidades no son específicas de la intención. 

## <a name="prebuilt-domain-intents"></a>Intenciones de dominio creados previamente

Los [dominios creados previamente](luis-how-to-use-prebuilt-domains.md) tienen intenciones con expresiones.  

## <a name="none-intent-is-fallback-for-app"></a>La intención None es una reserva de la aplicación
La intención **None** es una intención general o de reserva. Se usa para enseñar a LUIS expresiones que no son importantes en el dominio de la aplicación (área de asunto). La intención **None** debe tener entre un 10 y un 20 % de las expresiones totales de la aplicación. No la deje en blanco. 

### <a name="none-intent-helps-conversation-direction"></a>La intención None ayuda a dirigir la conversación
Cuando una expresión se ha predicho como la intención None y se ha devuelto al bot de chat con esa predicción, el bot puede hacer más preguntas o proporcionar un menú para dirigir al usuario a opciones válidas en el bot de chat. 

### <a name="no-utterances-in-none-intent-skews-predictions"></a>Ninguna expresión de la intención None sesga las predicciones
Si no agrega ninguna expresión a la intención **None**, LUIS fuerza una expresión que está fuera del dominio en una de las intenciones del dominio. Esto sesgará las puntuaciones de la predicción al enseñar a LUIS la intención incorrecta para la expresión. 

### <a name="add-utterances-to-the-none-intent"></a>Agregar expresiones a la intención None
La intención **None** se ha creado y dejado en blanco de forma intencionada. Rellénela con expresiones que se encuentren fuera de su dominio. Una buena expresión para **None** es algo que esté completamente fuera de la aplicación, así como el sector que atiende la aplicación. Por ejemplo, una aplicación de viajes no debería usar ninguna expresión en **None** que se pueda asociar con los viajes, como reservas, facturas, comida, hostelería, carga o entretenimiento durante el vuelo. 

¿Qué tipo de expresiones se dejan para la intención None? Comience con algo específico que el bot no debería responder, como "¿Qué tipo de dinosaurio tiene los dientes azules?". Esta es una pregunta muy específica que no encaja nada en una aplicación de viajes. 

### <a name="none-is-a-required-intent"></a>La intención None es obligatoria
La intención **None** es obligatoria y no se puede eliminar ni se puede cambiar su nombre.

## <a name="negative-intentions"></a>Intenciones negativas 
Si quiere determinar intenciones positivas y negativas, como "**Quiero** un coche" y "**No** quiero un coche", puede crear dos intenciones (una positiva y otra negativa) y agregar expresiones adecuadas para cada una. O bien, puede crear una única intención y marcar los dos términos positivos y negativos diferentes como una entidad.  

## <a name="intent-balance"></a>Equilibrio entre las intenciones
Las intenciones del dominio de la aplicación deben tener un equilibrio entre las expresiones de cada intención. No tenga una intención con 10 expresiones y otra con 500, ya que no están equilibradas. Si se encuentra en esta situación, revise la intención con 500 expresiones para ver si muchas de ellas se pueden reorganizar en un [patrón](luis-concept-patterns.md). 

La intención **None** no se incluye en este equilibrio. Esa intención debe contener un 10 % de las expresiones totales de la aplicación.

## <a name="intent-limits"></a>Límites de las intenciones
Consulte los [límites](luis-boundaries.md#model-boundaries) para comprender cuántas intenciones puede agregar a un modelo. 

### <a name="if-you-need-more-than-the-maximum-number-of-intents"></a>Si necesita más del número máximo de intenciones 
En primer lugar, evalúe si el sistema usa demasiadas intenciones. 

### <a name="can-multiple-intents-be-combined-into-single-intent-with-entities"></a>Varias intenciones se pueden combinar en una única intención con entidades 
Si hay intenciones muy similares, esto puede dificultar que LUIS las distinga. Las intenciones deben ser lo suficientemente diferentes como para capturar las tareas principales que pregunta el usuario, pero no tienen que capturar cada ruta que realice el código. Por ejemplo, es posible que "BookFlight" y "FlightCustomerService" deban ser intenciones diferentes en una aplicación de viajes, pero "BookInternationalFlight" y "BookDomesticFlight" son demasiado parecidas. Si el sistema necesita distinguirlas, use entidades u otra lógica en lugar de intenciones. 

### <a name="dispatcher-model"></a>Modelo de distribuidor
Obtenga más información sobre cómo combinar LUIS y aplicaciones de QnA Maker con el [modelo de distribución](luis-concept-enterprise.md#when-you-need-to-combine-several-luis-and-qna-maker-apps). 

### <a name="request-help-for-apps-with-significant-number-of-intents"></a>Solicitar ayuda para las aplicaciones con un número considerable de intenciones
Si reducir el número de intenciones o dividir las intenciones en varias aplicaciones no funciona, póngase en contacto con el soporte técnico. Si la suscripción a Azure incluye servicios de soporte técnico, póngase en contacto con el [soporte técnico de Azure](https://azure.microsoft.com/support/options/). 

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre las [entidades](luis-concept-entity-types.md), que son palabras importantes y relevantes para las intenciones.
* Obtenga información sobre cómo [agregar y administrar intenciones](luis-how-to-add-intents.md) en su aplicación de LUIS.
* Revise los [procedimientos recomendados](luis-concept-best-practices.md) de las intenciones.

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website