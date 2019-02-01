---
title: Preguntas más frecuentes
titleSuffix: Azure Cognitive Services
description: Este artículo contiene respuestas a las preguntas más frecuentes sobre Language Understanding (LUIS).
author: diberry
manager: cgronlun
ms.custom: seodec18
services: cognitive-services
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 01/23/2019
ms.author: diberry
ms.openlocfilehash: 704c3c6b4c998526936e7532fcd92c85ccce54e9
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55222191"
---
# <a name="language-understanding-frequently-asked-questions-faq"></a>Preguntas más frecuentes de Language Understanding

Este artículo contiene respuestas a las preguntas más frecuentes sobre Language Understanding (LUIS).

## <a name="luis-authoring"></a>Creación con LUIS

### <a name="what-are-the-luis-best-practices"></a>¿Cuáles son los procedimientos recomendados de LUIS?
Comience con [Authoring Cycle](luis-concept-app-iteration.md) (Ciclo de creación) y, después, lea [Procedimientos recomendados](luis-concept-best-practices.md).

### <a name="what-is-the-best-way-to-start-building-my-app-in-luis"></a>¿Cuál es la mejor manera de empezar a compilar la aplicación en LUIS?

La mejor manera de compilar la aplicación es con un [proceso incremental](luis-concept-app-iteration.md).

### <a name="what-is-a-good-practice-to-model-the-intents-of-my-app-should-i-create-more-specific-or-more-generic-intents"></a>¿Cuál es el procedimiento recomendado para modelar las intenciones de mi aplicación? ¿Debo crear intenciones más específicas o más genéricas?

Elija intenciones que no sean tan generales como para superponerse, ni tan específicas como para que a LUIS le resulte difícil distinguir entre intenciones similares. La creación de intenciones discriminantes específicas es uno de los procedimientos recomendados para el modelado en LUIS.

### <a name="is-it-important-to-train-the-none-intent"></a>¿Es importante entrenar la intención None?

Sí, es conveniente entrenar la intención **None** con más expresiones a medida que agregue más etiquetas a otras intenciones. Una buena proporción es 1 o 2 etiquetas agregadas a **None** por cada 10 etiquetas que se agregan a una intención. Esta proporción aumenta la eficacia de distinción de LUIS.

### <a name="how-can-i-correct-spelling-mistakes-in-utterances"></a>¿Cómo se puede corregir la ortografía en las expresiones?

Consulte el tutorial de [Bing Spell Check API V7](luis-tutorial-bing-spellcheck.md). LUIS aplica los límites impuestos por Bing Spell Check API V7.

### <a name="how-do-i-edit-my-luis-app-programmatically"></a>¿Cómo se edita la aplicación LUIS mediante programación?
Para editar la aplicación LUIS mediante programación, use la [API de creación](https://aka.ms/luis-authoring-apis). Consulte [Llamada a la API de creación de LUIS](./luis-quickstart-node-add-utterance.md) y [Build a LUIS app programmatically using Node.js](./luis-tutorial-node-import-utterances-csv.md) (Compilar la aplicación LUIS mediante programación con Node.js) para obtener ejemplos de cómo llamar a la API de creación. La API de creación requiere el uso de una [clave de creación](luis-concept-keys.md#authoring-key) en lugar de una clave de punto de conexión. Crear mediante programación permite hasta 1 000 000 llamadas al mes y cinco transacciones por segundo. Para obtener más información sobre las claves que se utilizan con LUIS, consulte [Administrar claves](./luis-concept-keys.md).

### <a name="where-is-the-pattern-feature-that-provided-regular-expression-matching"></a>¿Dónde está la característica de patrón que proporcionaba coincidencias con una expresión regular?
La **característica de patrón** anterior está en desuso actualmente, y se reemplazó por **[Patrones](luis-concept-patterns.md)**.

### <a name="how-do-i-use-an-entity-to-pull-out-the-correct-data"></a>¿Cómo se puede usar una entidad para extraer los datos correctos?
Consulte las [entidades](luis-concept-entity-types.md) y la [extracción de datos](luis-concept-data-extraction.md).

### <a name="should-variations-of-an-example-utterance-include-punctuation"></a>¿Las variaciones de una expresión de ejemplo deberían incluir signos de puntuación?
Puede agregar las variaciones distintas como expresiones de ejemplo de la intención o agregar el patrón de la expresión de ejemplo con la [sintaxis para pasar por alto](luis-concept-patterns.md#pattern-syntax) los signos de puntuación.

### <a name="does-luis-currently-support-cortana"></a>¿LUIS es compatible actualmente con Cortana?

Las aplicaciones precompiladas de Cortana están en desuso desde 2017. Ya no se admiten.

## <a name="luis-endpoint"></a>Punto de conexión de LUIS

### <a name="my-endpoint-query-returned-unexpected-results-what-should-i-do"></a>Mi consulta de punto de conexión devuelve resultados inesperados. ¿Cuál debo hacer?

Los resultados inesperados de las consultas dependen del estado del modelo publicado. Para corregir el modelo, tendrá que hacer cambios, entrenarlo y publicarlo de nuevo. 

La corrección del modelo comienza con un [aprendizaje activo](luis-how-to-review-endoint-utt.md).

Puede quitar el aprendizaje no determinista actualizando la [API de configuración de versión de la aplicación](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/versions-update-application-version-settings) para que así pueda utilizar todos los datos de aprendizaje. 

Consulte los [procedimientos recomendados](luis-concept-best-practices.md) para encontrar más sugerencias. 

### <a name="why-does-luis-add-spaces-to-the-query-around-or-in-the-middle-of-words"></a>¿Por qué LUIS agrega espacios a la consulta alrededor o en medio de las palabras?
LUIS [acorta](luis-glossary.md#token) la expresión según la [referencia cultural](luis-language-support.md#tokenization). El valor original y el valor acortado están disponibles para la [extracción de datos](luis-concept-data-extraction.md#tokenized-entity-returned).

### <a name="how-do-i-create-and-assign-a-luis-endpoint-key"></a>¿Cómo se crean y asignan las claves de punto de conexión para LUIS?
[Cree la clave de punto de conexión](luis-how-to-azure-subscription.md) en Azure para su nivel de [servicio](https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/). [Asigne la clave](luis-how-to-azure-subscription.md) en la página **[Claves y puntos de conexión](luis-how-to-azure-subscription.md)**. No hay ninguna API correspondiente a esta acción. Después, debe cambiar la solicitud HTTP al punto de conexión para [usar la nueva clave de punto de conexión](luis-concept-keys.md#use-endpoint-key-in-query).

### <a name="how-do-i-interpret-luis-scores"></a>¿Cómo se interpretan las puntuaciones de LUIS?
El sistema debe utilizar la intención de mayor puntuación independientemente de su valor. Por ejemplo, una puntuación menor a 0,5 (de menos del 50 %) no significa necesariamente que LUIS tiene un nivel de confiabilidad bajo. Proporcionar más datos de aprendizaje puede ayudar a aumentar la [puntuación](luis-concept-prediction-score.md) de la intención más probable.

### <a name="why-dont-i-see-my-endpoint-hits-in-my-apps-dashboard"></a>¿Por qué no son visibles las visitas del punto de conexión en el panel de información de la aplicación?
Las visitas totales del punto de conexión se actualizan periódicamente en el panel de información de la aplicación, pero las métricas asociadas con su clave de punto de conexión de LUIS en Azure Portal se actualizan con mayor frecuencia.

Si las visitas del punto de conexión no están actualizadas en el panel de información, inicie sesión en Azure Portal, busque el recurso asociado con su clave de punto de conexión de LUIS y abra **Métrica** para seleccionar la métrica del **Total de llamadas**. Si la clave de punto de conexión se utiliza para más de una aplicación de LUIS, la métrica en Azure Portal muestra el número agregado de llamadas de todas las aplicaciones de LUIS que la usan.

### <a name="is-there-a-powershell-command-to-the-endpoint-quota"></a>¿Existe un comando de PowerShell para la cuota de punto de conexión?

Puede usar un comando de PowerShell para ver la cuota de punto de conexión:

```powershell
Get-AzureRmCognitiveServicesAccountUsage -ResourceGroupName <your-resource-group> -Name <your-resource-name>
``` 

### <a name="my-luis-app-was-working-yesterday-but-today-im-getting-403-errors-i-didnt-change-the-app-how-do-i-fix-it"></a>La aplicación LUIS funcionaba ayer, pero hoy recibo errores 403. No hice cambios en la aplicación. ¿Cómo puedo corregirlo?
Siga las [instrucciones](#how-do-i-create-and-assign-a-luis-endpoint-key) en la siguiente pregunta frecuente para crear una clave de punto de conexión de LUIS y asignarla a la aplicación. Después, debe cambiar la solicitud HTTP al punto de conexión para [usar la nueva clave de punto de conexión](luis-concept-keys.md#use-endpoint-key-in-query).

### <a name="how-do-i-secure-my-luis-endpoint"></a>¿Cómo se protege el punto de conexión de LUIS?
Consulte [Securing the endpoint](luis-concept-security.md#securing-the-endpoint) (Proteger el punto de conexión).

## <a name="working-within-luis-limits"></a>Trabajo dentro de los límites de LUIS

### <a name="what-is-the-maximum-number-of-intents-and-entities-that-a-luis-app-can-support"></a>¿Cuál es el número máximo de intenciones y entidades que puede admitir una aplicación de LUIS?
Consulte la referencia sobre [límites](luis-boundaries.md).

### <a name="i-want-to-build-a-luis-app-with-more-than-the-maximum-number-of-intents-what-should-i-do"></a>Quiero compilar una aplicación de LUIS con un número de intenciones superior al máximo. ¿Cuál debo hacer?

Consulte los [Procedimientos recomendados para intenciones](luis-concept-intent.md#if-you-need-more-than-the-maximum-number-of-intents).

### <a name="i-want-to-build-an-app-in-luis-with-more-than-the-maximum-number-of-entities-what-should-i-do"></a>Quiero compilar una aplicación de LUIS con un número de intenciones superior al máximo. ¿Cuál debo hacer?

Consulte los [Procedimientos recomendados para entidades](luis-concept-entity-types.md#if-you-need-more-than-the-maximum-number-of-entities)

### <a name="what-are-the-limits-on-the-number-and-size-of-phrase-lists"></a>¿Cuáles son los límites de número y tamaño de las listas de frases?
Para saber más sobre la longitud máxima de una [lista de frases](./luis-concept-feature.md), consulte la referencia sobre [límites](luis-boundaries.md).

### <a name="what-are-the-limits-on-example-utterances"></a>¿Cuáles son los límites de las expresiones de ejemplo?
Consulte la referencia sobre [límites](luis-boundaries.md).

## <a name="testing-and-training"></a>Prueba y entrenamiento

### <a name="i-see-some-errors-in-the-batch-testing-pane-for-some-of-the-models-in-my-app-how-can-i-address-this-problem"></a>Hay errores en el panel de pruebas en lote para algunos de los modelos de mi aplicación. ¿Cómo puedo solucionar este problema?

Los errores indican que hay discrepancias entre las etiquetas y las predicciones de los modelos. Para solucionar el problema, realice una de las siguientes tareas, o ambas:
* Para ayudar a que LUIS mejore la distinción entre intenciones, agregue más etiquetas.
* Para ayudar a que LUIS aprenda más rápido, agregue características de la lista de frases que introduzcan vocabulario específico de dominio.

Consulte el tutorial sobre [Pruebas en lote](luis-tutorial-batch-testing.md).

### <a name="when-an-app-is-exported-then-reimported-into-a-new-app-with-a-new-app-id-the-luis-prediction-scores-are-different-why-does-this-happen"></a>Cuando se exporta una aplicación y, más tarde, se reimporta a una aplicación nueva (con un id. de aplicación nuevo), las puntuaciones de predicción de LUIS son distintas. ¿Por qué ocurre esto?

Consulte las [diferencias de predicción entre las copias de la misma aplicación](luis-concept-prediction-score.md#differences-with-predictions).

### <a name="some-utterances-go-to-the-wrong-intent-after-i-made-changes-to-my-app-the-issue-seems-to-disappear-at-random-how-do-i-fix-it"></a>Después de haber hecho cambios en la aplicación, algunas expresiones van a la intención incorrecta. El problema parece desaparecer de forma aleatoria. ¿Cómo puedo corregirlo? 

Vea [Entrenamiento con todos los datos](luis-how-to-train.md#train-with-all-data).

## <a name="app-publishing"></a>Publicación de la aplicación

### <a name="what-is-the-tenant-id-in-the-add-a-key-to-your-app-window"></a>¿Qué es el identificador de inquilino de la ventana Add a key to your app (Agregar una clave a la aplicación)?
En Azure, un inquilino representa al cliente o a la organización que están asociados con un servicio. Para encontrar el identificador de inquilino en Azure Portal en el cuadro **Id. de directorio**, seleccione **Azure Active Directory** > **Administrar** > **Propiedades**.

![Identificador de inquilino en Azure Portal](./media/luis-manage-keys/luis-assign-key-tenant-id.png)

<a name="why-are-there-more-subscription-keys-on-my-apps-publish-page-than-i-assigned-to-the-app"></a>
<a name="why-are-there-more-endpoint-keys-on-my-apps-publish-page-than-i-assigned-to-the-app"></a>


### <a name="why-are-there-more-endpoint-keys-assigned-to-my-app-than-i-assigned"></a>¿Por qué hay más claves de punto de conexión asignadas a mi aplicación de las que yo he asignado?
Cada aplicación de LUIS tiene la clave de creación/inicio en la lista de puntos de conexión para su comodidad. Esta clave permite solo unas pocas visitas al punto de conexión para que pueda probar LUIS.  

Si la aplicación ya existía antes de que LUIS estuviera disponible con carácter general (GA), las claves de punto de conexión de LUIS de la suscripción se asignan automáticamente. Esto facilita la migración de GA. Cualquier nueva clave de punto de conexión de LUIS de Azure Portal _no_ se asigna automáticamente a LUIS.

## <a name="app-management"></a>Administración de la aplicación

### <a name="how-do-i-transfer-ownership-of-a-luis-app"></a>¿Cómo puedo transferir la propiedad de la aplicación de LUIS?
Para transferir una aplicación de LUIS a otra suscripción de Azure, exporte la aplicación de LUIS e impórtela con una cuenta nueva. Actualice el id. de aplicación de LUIS en la aplicación cliente que lo llama. La aplicación nueva puede devolver puntuaciones de LUIS ligeramente distintas a las de la aplicación original.

### <a name="how-do-i-download-a-log-of-user-utterances"></a>¿Cómo puedo descargar un registro de las expresiones de los usuarios?
De forma predeterminada, la aplicación de LUIS registra las expresiones de los usuarios. Para descargar un registro de las expresiones que los usuarios envían a su aplicación de LUIS, vaya a **Mis aplicaciones** y seleccione la aplicación. En la barra de herramientas contextual, seleccione **Export Endpoint Logs** (Exportar registros de punto de conexión). El registro está en un archivo con formato de valores separados por comas (CSV).

### <a name="how-can-i-disable-the-logging-of-utterances"></a>¿Cómo puedo deshabilitar el registro de expresiones?
Puede desactivar el registro de expresiones de usuario si establece `log=false` en la dirección URL del punto de conexión que la aplicación cliente usa para consultar a LUIS. Sin embargo, desactivar el registro deshabilita la capacidad de la aplicación de LUIS para sugerir expresiones o mejorar su rendimiento basada en el [aprendizaje activo](luis-concept-review-endpoint-utterances.md#what-is-active-learning). Si establece `log=false` por motivos de privacidad de los datos, no puede descargar un registro de esas expresiones de usuario desde LUIS ni usar esas expresiones para mejorar su aplicación.

El registro es el único almacenamiento de expresiones.

### <a name="why-dont-i-want-all-my-endpoint-utterances-logged"></a>¿Por qué no querría registrar todas las expresiones del punto de conexión?
Si utiliza el registro para el análisis de predicción, no capture las expresiones de prueba en el registro.

## <a name="data-management"></a>Administración de datos

### <a name="can-i-delete-data-from-luis"></a>¿Puedo eliminar datos de LUIS?

* Siempre puede eliminar expresiones de ejemplo que se usaron para entrenar a LUIS. Si elimina una expresión de ejemplo desde la aplicación de LUIS, se elimina del servicio web de LUIS y no está disponible para su exportación.
* Puede eliminar expresiones de la lista de expresiones de usuario que LUIS sugiere en la página **Review endpoint utterances** (Revisar expresiones de punto de conexión). Eliminar la expresiones de esta lista evita que se sugieran, pero no las elimina de los registros.
* Si elimina una cuenta, se eliminan todas las aplicaciones, incluso las expresiones de ejemplo y los registros. Los datos se conservan en los servidores durante 60 días antes de eliminarse permanentemente.

### <a name="how-does-microsoft-manage-data-i-send-to-luis"></a>¿Cómo administra Microsoft los datos que envío a LUIS?

En el [Centro de confianza](https://www.microsoft.com/trustcenter) se explican nuestros compromisos y sus opciones para la administración y el acceso a los datos en los servicios de Azure.

## <a name="language-and-translation-support"></a>Compatibilidad de idioma y traducción

### <a name="i-have-an-app-in-one-language-and-want-to-create-a-parallel-app-in-another-language-what-is-the-easiest-way-to-do-so"></a>Tengo una aplicación en un idioma y quiero crear una aplicación paralela en otro idioma. ¿Cuál es la manera más fácil de hacerlo?
1. Exporte su aplicación.
2. Traduzca al idioma de destino las expresiones con etiquetas en el archivo JSON de la aplicación exportada.
3. Puede que necesite cambiar los nombres de las intenciones y de las entidades, o puede dejarlos como están.
4. Por último, importe la aplicación para tener una aplicación de LUIS en el idioma de destino.

## <a name="app-notification"></a>Notificaciones de la aplicación

### <a name="why-did-i-get-an-email-saying-im-almost-out-of-quota"></a>¿Por qué recibí un correo que dice que casi he agotado mi cuota?
La clave de creación/inicio solo permite 1000 consultas de punto de conexión por mes. Cree una clave de punto de conexión de LUIS (gratuita o de pago) y úsela para realizar consultas de punto de conexión. Si está realizando consultas de punto de conexión desde un bot u otra aplicación cliente, debe cambiar la clave de punto de conexión de LUIS ahí.

## <a name="integrating-luis"></a>Integración de LUIS

### <a name="where-is-my-luis-app-created-during-the-azure-web-app-bot-subscription-process"></a>¿Dónde se crea la aplicación LUIS durante el proceso de suscripción al bot de la aplicación web de Azure?
Si selecciona una plantilla de LUIS y hace clic en el botón de **selección** en el panel de la plantilla, el panel de la izquierda cambia para incluir el tipo de plantilla y le pregunta en qué región quiere crear la plantilla de LUIS. Sin embargo, el proceso del bot de aplicación web no crea una suscripción de LUIS.

![Región del bot de aplicación web de la plantilla de LUIS](./media/luis-faq/web-app-bot-location.png)

### <a name="what-luis-regions-support-bot-framework-speech-priming"></a>¿Qué regiones de LUIS son compatibles con la preparación para la voz de Framework Bot?
La [preparación para la voz](https://docs.microsoft.com/bot-framework/bot-service-manage-speech-priming) solo es compatible con aplicaciones de LUIS en la instancia central (EE. UU.).

## <a name="api-programming-strategies"></a>Estrategias de programación de la API

### <a name="how-do-i-programmatically-get-the-luis-region-of-a-resource"></a>¿Cómo puedo obtener mediante programación la región LUIS de un recurso? 

Use el ejemplo de LUIS para [buscar regiones](https://github.com/Azure-Samples/cognitive-services-language-understanding/tree/master/documentation-samples/find-region) mediante programación con C# o Node.Js. 

## <a name="luis-service"></a>Servicio de LUIS

### <a name="is-language-understanding-luis-available-on-premises-or-in-private-cloud"></a>¿Está Language Understanding (LUIS) disponible localmente o en la nube privada?

Sí, puede usar el [contenedor](luis-container-howto.md) de LUIS para estos escenarios, si tiene la conectividad necesaria para medir el uso. 

### <a name="at-the-build-2018-conference-i-heard-about-a-language-understanding-feature-or-demo-but-i-dont-remember-what-it-was-called"></a>Escuché hablar de una característica o demo de Language Understanding en la conferencia Build 2018, pero no recuerdo cómo se llama

Las siguientes características se publicaron en la conferencia Build 2018:

|NOMBRE|Contenido|
|--|--|
|Mejoras|La entidad de [expresión regular](luis-concept-data-extraction.md##regular-expression-entity-data) y la entidad de [frase clave](luis-concept-data-extraction.md#key-phrase-extraction-entity-data)
|Patrones|[Conceptos](luis-concept-patterns.md), [tutoriales](luis-tutorial-pattern.md) y [procedimientos](luis-how-to-model-intent-pattern.md) sobre patrones<br>Concepto de la entidad [Patterns.Any](luis-concept-entity-types.md) que incluye las [listas explícitas](luis-concept-patterns.md#explicit-lists) para excepciones<br>Concepto de [roles](luis-concept-roles.md)|
|Integraciones|Integración de [Text Analytics](https://docs.microsoft.com/azure/cognitive-services/text-analytics/) del [análisis de sentimiento](luis-how-to-publish-app.md#enable-sentiment-analysis)<br>Integración de [Speech](https://docs.microsoft.com/azure/cognitive-services/speech) de preparación para la voz con [Speech SDK](https://aka.ms/SpeechSDK)|
|Herramienta de distribución|[Herramienta](luis-concept-enterprise.md#when-you-need-to-combine-several-luis-and-qna-maker-apps) de distribución por línea de comandos que es parte de [BotBuilder-tools](https://github.com/Microsoft/botbuilder-tools) y combina varias aplicaciones de LUIS y QnA Maker en una única aplicación de LUIS para optimizar el reconocimiento de intención en un bot

Se incluyeron [rutas de API](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/authoring-routes.md) de creación adicionales.

Videos:
* [Azure Friday en la compilación de 2018: Cognitive Services - Language (LUIS)](https://channel9.msdn.com/Shows/Azure-Friday/At-Build-2018-Cognitive-Services-Language-LUIS/player)
* [Build 2018 AI Show - What’s New with Language Understanding Service](https://channel9.msdn.com/Shows/AI-Show/Whats-New-with-Language-Understanding-Service-LUIS/player) (Presentación de IA en Build 2018: Novedades de Language Understanding Service)
* [Build 2018 Session - Bot intelligence, Speech Capabilities, and NLU best practices](https://channel9.msdn.com/events/Build/2018/BRK3208) (Sesión de Build 2018: inteligencia de bot, funcionalidades de voz y procedimientos recomendados de NLU)
* [Build 2018 - LUIS Updates](https://channel9.msdn.com/events/Build/2018/THR3118/player) (Build 2018: actualizaciones de LUIS)

Proyectos:
* Demostración [Contoso Cafe bot](https://github.com/botbuilderbuild2018/build2018demo) (Bot de Contoso Cafe): código fuente en GitHub

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre LUIS, consulte los siguientes recursos:
* [Preguntas de Stack Overflow con etiquetas de LUIS](https://stackoverflow.com/questions/tagged/luis)
* [Foro MSDN de Language Understanding Intelligent Services (LUIS)](https://social.msdn.microsoft.com/forums/azure/home?forum=LUIS)
