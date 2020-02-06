---
title: 'Mejora de una base de conocimiento: QnA Maker'
titleSuffix: Azure Cognitive Services
description: Mejore la calidad de su base de conocimientos con el aprendizaje activo. Revise, acepte o rechace, o agregue sin quitar ni cambiar las preguntas existentes.
author: diberry
manager: nitinme
services: cognitive-services
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 01/28/2020
ms.author: diberry
ms.openlocfilehash: cadbf5fa88db7d5e524cb7e075745c03a844f750
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2020
ms.locfileid: "76901721"
---
# <a name="use-active-learning-to-improve-your-knowledge-base"></a>Uso del aprendizaje activo para mejorar la base de conocimiento

El aprendizaje activo le permite mejorar la calidad de la base de conocimiento al sugerir preguntas alternativas, en función de los envíos del usuario, para el par de preguntas y respuestas. Usted revisa las sugerencias, ya sea al agregarlas a preguntas existentes o rechazarlas.

La base de conocimiento no cambia automáticamente. Debe aceptar las sugerencias para que cualquier cambio surta efecto. Estas sugerencias agregan preguntas, pero no cambian ni quitan preguntas existentes.

## <a name="what-is-active-learning"></a>¿Qué es el aprendizaje activo?

QnA Maker aprende las nuevas variaciones de las preguntas con comentarios implícitos y explícitos.

* [Comentarios implícitos:](#how-qna-makers-implicit-feedback-works) el clasificador entiende cuando la pregunta de un usuario tiene varias respuestas con puntuaciones muy similares y lo considera un comentario. No es necesario hacer nada para que esto ocurra.
* [Comentarios explícitos:](#how-you-give-explicit-feedback-with-the-train-api) cuando se devuelven desde la base de conocimiento varias respuestas con poca variación en las puntuaciones, la aplicación cliente le pregunta al usuario cuál es la pregunta correcta. Los comentarios explícitos del usuario se envían a QnA Maker con la [API Train](#train-api).

Ambos métodos proporcionan al clasificador consultas similares que se agrupan en clústeres.

## <a name="how-active-learning-works"></a>Cómo funciona el aprendizaje activo

El aprendizaje activo se desencadena en función de las puntuaciones de las mejores respuestas devueltas por QnA Maker. Si las diferencias de puntuación se encuentran dentro de un intervalo pequeño, la consulta se considera una posible sugerencia (como una pregunta alternativa) para cada uno de los pares de QnA posibles. Una vez que acepta la pregunta sugerida para un par específico de QnA, se rechaza para los otros pares. Debe recordar, guardar y entrenar, después de aceptar las sugerencias.

El aprendizaje activo ofrece las mejores sugerencias posibles en los casos donde los puntos de conexión reciben una cantidad razonable y una variedad de consultas de uso. Cuando 5 o más consultas similares se agrupan en clústeres, cada 30 minutos, QnA Maker sugiere las preguntas basadas en el usuario al diseñador de la base de conocimiento para que las acepte o rechace. Todas las sugerencias se agrupan en clústeres por similitud, y las sugerencias principales para preguntas alternativas se muestran según la frecuencia de las consultas en particular por los usuarios finales.

Una vez que se sugieren preguntas en el portal de QnA Maker, deberá revisar y aceptar o rechazar las sugerencias. No hay ninguna API para administrar las sugerencias.

## <a name="how-qna-makers-implicit-feedback-works"></a>Cómo funcionan los comentarios implícitos de QnA Maker

Los comentarios implícitos de QnA Maker usan un algoritmo para determinar la proximidad de la puntuación y, después, hacer sugerencias de aprendizaje activo. El algoritmo para determinar la proximidad no es un cálculo sencillo. Los intervalos en el ejemplo siguiente no están diseñados para ser fijos, pero deben usarse como guía para comprender el impacto del algoritmo únicamente.

Cuando la puntuación de una pregunta tiene una confianza alta, por ejemplo, un 80 %, el intervalo de puntuaciones que se considera para el aprendizaje activo es amplio, dentro del 10 % aproximadamente. A medida que la puntuación de confianza se reduce, por ejemplo, un 40 %, el intervalo de puntuaciones también disminuye aproximadamente dentro del 4 %.

## <a name="how-you-give-explicit-feedback-with-the-train-api"></a>Cómo proporcionar comentarios explícitos con Train API

Es importante que QnA Maker obtenga comentarios explícitos sobre cuál de las respuestas era la mejor. La forma de determinar la mejor respuesta depende de usted y puede incluir:

* Comentarios del usuario, seleccione una de las respuestas.
* Lógica de negocios, como determinar un intervalo de puntuación aceptable.
* Una combinación de los comentarios del usuario y la lógica de negocios.

## <a name="upgrade-your-runtime-version-to-use-active-learning"></a>Actualización de la versión en tiempo de ejecución para usar el aprendizaje activo

El aprendizaje activo se admite en el tiempo de ejecución versión 4.4.0 y superior. Si la base de conocimiento se creó en una versión anterior, [actualice el tiempo de ejecución](set-up-qnamaker-service-azure.md#get-the-latest-runtime-updates) para usar esta característica.

## <a name="turn-on-active-learning-to-see-suggestions"></a>Activación del aprendizaje activo para ver sugerencias

El aprendizaje activo está desactivado de forma predeterminada. Actívelo para ver preguntas sugeridas. Una vez activado el aprendizaje activo, deberá enviar información desde la aplicación cliente a QnA Maker. Para obtener más información, consulte [Flujo arquitectónico para usar GenerateAnswer y Train API desde un bot](#architectural-flow-for-using-generateanswer-and-train-apis-from-a-bot).

1. Seleccione **Publicar** para publicar la base de conocimiento. Las consultas de aprendizaje activo se recopilan solo desde el punto de conexión de predicción de GenerateAnswer API. Las consultas en el panel de prueba del portal de QnA Maker no afectarán al aprendizaje activo.

1. Para activar el aprendizaje activo en el portal de QnA Maker, vaya a la esquina superior derecha, seleccione su **Nombre** y vaya a [**Configuración del servicio**](https://www.qnamaker.ai/UserSettings).

    ![Active las preguntas sugeridas alternativas de aprendizaje activo desde la página Configuración del servicio. Seleccione su nombre de usuario en el menú superior derecho y seleccione Continuación del servicio.](../media/improve-knowledge-base/Endpoint-Keys.png)


1. Busque el servicio QnA Maker, y active **Aprendizaje activo**.

    > [!div class="mx-imgBorder"]
    > [![En la página Configuración del servicio, active la característica Aprendizaje activo. Si no es capaz de activar o desactivar la característica, deberá actualizar el servicio.](../media/improve-knowledge-base/turn-active-learning-on-at-service-setting.png)](../media/improve-knowledge-base/turn-active-learning-on-at-service-setting.png#lightbox)

    > [!Note]
    > La versión exacta de la imagen anterior se muestra solo como un ejemplo. Su versión puede ser diferente.

    Una vez que **Aprendizaje activo** está habilitado, la base de conocimiento sugiere nuevas preguntas a intervalos regulares según las preguntas enviadas por el usuario. Para deshabilitar **Aprendizaje activo**, vuelva a cambiar la configuración.

## <a name="accept-an-active-learning-suggestion-in-the-knowledge-base"></a>Aceptación de una sugerencia de aprendizaje activo en la base de conocimiento

El aprendizaje activo modifica la base de conocimiento o el servicio de búsqueda después de aprobar la sugerencia y, a continuación, de guardarla y entrenarla. Si aprueba la sugerencia, se agregará como una pregunta alternativa.

1. Para ver las preguntas sugeridas, en la página **Editar** de la base de conocimiento, seleccione **Ver opciones** y, después, **Show active learning suggestions** (Mostrar sugerencias de aprendizaje activo).

    [![En la sección Editar del portal, seleccione Mostrar sugerencias para ver nuevas preguntas alternativas del aprendizaje activo.](../media/improve-knowledge-base/show-suggestions-button.png)](../media/improve-knowledge-base/show-suggestions-button.png#lightbox)

1. Filtre la base de conocimiento con pares de preguntas y respuestas para mostrar únicamente las sugerencias seleccionando **Filter by Suggestions** (Filtrar por sugerencias).

    [![Active o desactive el filtrado por sugerencias para ver solo las sugerencias de preguntas alternativas del aprendizaje activo.](../media/improve-knowledge-base/filter-by-suggestions.png)](../media/improve-knowledge-base/filter-by-suggestions.png#lightbox)

1. Cada par de QnA sugiere la nueva pregunta alternativa con una marca de verificación, `✔`, para aceptar la pregunta o una `x` para rechazar las sugerencias. Seleccione la marca de verificación para agregar la pregunta.

    [![Seleccione o rechace las sugerencias de preguntas alternativas del aprendizaje activo seleccionando la marca de verificación verde o la marca de eliminación de roja.](../media/improve-knowledge-base/accept-active-learning-suggestions.png)](../media/improve-knowledge-base/accept-active-learning-suggestions.png#lightbox)

    Puede agregar o eliminar _todas las sugerencias_ seleccionando **Agregar todo** o **Rechazar todo** en la barra de herramientas contextual.

1. Seleccione **Save and Train** (Guardar y entrenar) para guardar los cambios en la base de conocimiento.

1. Seleccione **Publicar** para permitir que los cambios estén disponibles desde [GenerateAnswer API](metadata-generateanswer-usage.md#generateanswer-request-configuration).

    Cuando 5 o más consultas similares se agrupan en clústeres, cada 30 minutos, QnA Maker sugiere preguntas alternativas para que las acepte o rechace.


<a name="#score-proximity-between-knowledge-base-questions"></a>

### <a name="architectural-flow-for-using-generateanswer-and-train-apis-from-a-bot"></a>Flujo arquitectónico para usar GenerateAnswer y Train API desde un bot

Un bot u otra aplicación cliente debe usar el siguiente flujo de arquitectura para usar el aprendizaje activo:

* El bot [obtiene la respuesta de la base de conocimiento](#use-the-top-property-in-the-generateanswer-request-to-get-several-matching-answers) con GenerateAnswer API, utilizando la propiedad `top` para obtener un número de respuestas.
* El bot determina comentarios explícitos:
    * Usando su propia [lógica de negocios personalizada](#use-the-score-property-along-with-business-logic-to-get-list-of-answers-to-show-user), filtre las puntuaciones bajas.
    * En el bot o aplicación cliente, muestre la lista de posibles respuestas al usuario y obtenga la respuesta seleccionada del usuario.
* El bot [envía la respuesta seleccionada a QnA Maker](#bot-framework-sample-code) con [Train API](#train-api).


### <a name="use-the-top-property-in-the-generateanswer-request-to-get-several-matching-answers"></a>Uso de la propiedad top en la solicitud GenerateAnswer para obtener varias respuestas coincidentes

Al enviar una pregunta a QnA Maker para obtener una respuesta, la propiedad `top` del cuerpo JSON define el número de respuestas a devolver.

```json
{
    "question": "wi-fi",
    "isTest": false,
    "top": 3
}
```

### <a name="use-the-score-property-along-with-business-logic-to-get-list-of-answers-to-show-user"></a>Uso de la propiedad de puntuación y la lógica de negocios para obtener la lista de respuestas que se mostrará al usuario

Cuando la aplicación cliente (por ejemplo, un bot de chat) recibe la respuesta, se devuelven las 3 preguntas principales. Use la propiedad `score` para analizar la proximidad entre las puntuaciones. Este intervalo de proximidad viene determinado por su propia lógica de negocios.

```json
{
    "answers": [
        {
            "questions": [
                "Wi-Fi Direct Status Indicator"
            ],
            "answer": "**Wi-Fi Direct Status Indicator**\n\nStatus bar icons indicate your current Wi-Fi Direct connection status:  \n\nWhen your device is connected to another device using Wi-Fi Direct, '$  \n\n+ •+ ' Wi-Fi Direct is displayed in the Status bar.",
            "score": 74.21,
            "id": 607,
            "source": "Bugbash KB.pdf",
            "metadata": []
        },
        {
            "questions": [
                "Wi-Fi - Connections"
            ],
            "answer": "**Wi-Fi**\n\nWi-Fi is a term used for certain types of Wireless Local Area Networks (WLAN). Wi-Fi communication requires access to a wireless Access Point (AP).",
            "score": 74.15,
            "id": 599,
            "source": "Bugbash KB.pdf",
            "metadata": []
        },
        {
            "questions": [
                "Turn Wi-Fi On or Off"
            ],
            "answer": "**Turn Wi-Fi On or Off**\n\nTurning Wi-Fi on makes your device able to discover and connect to compatible in-range wireless APs.  \n\n1.  From a Home screen, tap ::: Apps > e Settings .\n2.  Tap Connections > Wi-Fi , and then tap On/Off to turn Wi-Fi on or off.",
            "score": 69.99,
            "id": 600,
            "source": "Bugbash KB.pdf",
            "metadata": []
        }
    ]
}
```

## <a name="client-application-follow-up-when-questions-have-similar-scores"></a>Seguimiento de la aplicación cliente cuando las preguntas tienen puntuaciones similares

La aplicación cliente muestra las preguntas con una opción para que el usuario seleccione _la única pregunta_ que mejor representa su intención.

Una vez que el usuario selecciona una de las preguntas existentes, la aplicación cliente envía la elección del usuario como comentario mediante Train API de QnA Maker. Estos comentarios completan el bucle de comentarios de aprendizaje activo.

## <a name="train-api"></a>Train API

Los comentarios de aprendizaje activo se envían a QnA Maker con la solicitud POST de Train API. La firma de API es:

```http
POST https://<QnA-Maker-resource-name>.azurewebsites.net/qnamaker/knowledgebases/<knowledge-base-ID>/train
Authorization: EndpointKey <endpoint-key>
Content-Type: application/json
{"feedbackRecords": [{"userId": "1","userQuestion": "<question-text>","qnaId": 1}]}
```

|Propiedad de la solicitud HTTP|Nombre|Tipo|Propósito|
|--|--|--|--|
|Parámetro de la ruta de dirección URL|Id. de base de conocimiento|string|GUID de Knowledge Base.|
|Subdominio personalizado|Nombre del recurso de QnAMaker|string|El nombre del recurso se usa como subdominio personalizado para QnA Maker. Esta disponible en la página Configuración después de publicar la base de conocimiento. Se enumera como `host`.|
|Encabezado|Content-Type|string|tipo de soporte del cuerpo enviado a la API. El valor predeterminado es: `application/json`|
|Encabezado|Authorization|string|su clave de punto de conexión (EndpointKey xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx).|
|Cuerpo de POST|Objeto JSON|JSON|Comentarios de entrenamiento|

El cuerpo JSON tiene varias opciones de configuración:

|Propiedad del cuerpo de JSON|Tipo|Propósito|
|--|--|--|--|
|`feedbackRecords`|array|Lista de comentarios.|
|`userId`|string|Id. de usuario de la persona que acepta las preguntas sugeridas. El formato del id. de usuario es decisión suya. Por ejemplo, una dirección de correo electrónico puede ser un id. de usuario válido en la arquitectura. Opcional.|
|`userQuestion`|string|Texto exacto de la consulta del usuario. Necesario.|
|`qnaID`|number|Id. de la pregunta, se encuentra en la [respuesta GenerateAnswer](metadata-generateanswer-usage.md#generateanswer-response-properties). |

Un cuerpo JSON de ejemplo tiene el siguiente aspecto:

```json
{
    "feedbackRecords": [
        {
            "userId": "1",
            "userQuestion": "<question-text>",
            "qnaId": 1
        }
    ]
}
```

Una respuesta correcta devuelve el estado de 204 y ningún cuerpo de respuesta JSON.

### <a name="batch-many-feedback-records-into-a-single-call"></a>Procesar por lotes varios registros de comentarios en una sola llamada

En la aplicación del lado cliente, como un bot, puede almacenar los datos y luego enviar muchos registros en un único cuerpo JSON con la matriz `feedbackRecords`.

Un cuerpo JSON de ejemplo tiene el siguiente aspecto:

```json
{
    "feedbackRecords": [
        {
            "userId": "1",
            "userQuestion": "How do I ...",
            "qnaId": 1
        },
        {
            "userId": "2",
            "userQuestion": "Where is ...",
            "qnaId": 40
        },
        {
            "userId": "3",
            "userQuestion": "When do I ...",
            "qnaId": 33
        }
    ]
}
```



<a name="active-learning-is-saved-in-the-exported-apps-tsv-file"></a>

## <a name="bot-framework-sample-code"></a>Código de ejemplo de Bot Framework

El código de Bot Framework debe llamar a Train API, si la consulta del usuario se debe usar para el aprendizaje activo. Hay dos fragmentos de código para escribir:

* Determinar si se debe usar la consulta para el aprendizaje activo
* Envío de la consulta a Train API de QnA Maker para el aprendizaje activo

En el [ejemplo de Azure Bot](https://aka.ms/activelearningsamplebot), se han programado ambas actividades.

### <a name="example-c-code-for-train-api-with-bot-framework-4x"></a>Ejemplo del código C# para Train API con Bot Framework 4.x

En el código siguiente se muestra cómo enviar información a QnA Maker con Train API. Este [ejemplo de código completo](https://github.com/microsoft/BotBuilder-Samples/tree/master/experimental/qnamaker-activelearning/csharp_dotnetcore) está disponible en GitHub.

```csharp
public class FeedbackRecords
{
    // <summary>
    /// List of feedback records
    /// </summary>
    [JsonProperty("feedbackRecords")]
    public FeedbackRecord[] Records { get; set; }
}

/// <summary>
/// Active learning feedback record
/// </summary>
public class FeedbackRecord
{
    /// <summary>
    /// User id
    /// </summary>
    public string UserId { get; set; }

    /// <summary>
    /// User question
    /// </summary>
    public string UserQuestion { get; set; }

    /// <summary>
    /// QnA Id
    /// </summary>
    public int QnaId { get; set; }
}

/// <summary>
/// Method to call REST-based QnAMaker Train API for Active Learning
/// </summary>
/// <param name="endpoint">Endpoint URI of the runtime</param>
/// <param name="FeedbackRecords">Feedback records train API</param>
/// <param name="kbId">Knowledgebase Id</param>
/// <param name="key">Endpoint key</param>
/// <param name="cancellationToken"> Cancellation token</param>
public async static void CallTrain(string endpoint, FeedbackRecords feedbackRecords, string kbId, string key, CancellationToken cancellationToken)
{
    var uri = endpoint + "/knowledgebases/" + kbId + "/train/";

    using (var client = new HttpClient())
    {
        using (var request = new HttpRequestMessage())
        {
            request.Method = HttpMethod.Post;
            request.RequestUri = new Uri(uri);
            request.Content = new StringContent(JsonConvert.SerializeObject(feedbackRecords), Encoding.UTF8, "application/json");
            request.Headers.Add("Authorization", "EndpointKey " + key);

            var response = await client.SendAsync(request, cancellationToken);
            await response.Content.ReadAsStringAsync();
        }
    }
}
```

### <a name="example-nodejs-code-for-train-api-with-bot-framework-4x"></a>Ejemplo del código Node.js para Train API con Bot Framework 4.x

En el código siguiente se muestra cómo enviar información a QnA Maker con Train API. Este [ejemplo de código completo](https://github.com/microsoft/BotBuilder-Samples/blob/master/experimental/qnamaker-activelearning/javascript_nodejs) está disponible en GitHub.

```javascript
async callTrain(stepContext){

    var trainResponses = stepContext.values[this.qnaData];
    var currentQuery = stepContext.values[this.currentQuery];

    if(trainResponses.length > 1){
        var reply = stepContext.context.activity.text;
        var qnaResults = trainResponses.filter(r => r.questions[0] == reply);

        if(qnaResults.length > 0){

            stepContext.values[this.qnaData] = qnaResults;

            var feedbackRecords = {
                FeedbackRecords:[
                    {
                        UserId:stepContext.context.activity.id,
                        UserQuestion: currentQuery,
                        QnaId: qnaResults[0].id
                    }
                ]
            };

            // Call Active Learning Train API
            this.activeLearningHelper.callTrain(this.qnaMaker.endpoint.host, feedbackRecords, this.qnaMaker.endpoint.knowledgeBaseId, this.qnaMaker.endpoint.endpointKey);

            return await stepContext.next(qnaResults);
        }
        else{

            return await stepContext.endDialog();
        }
    }

    return await stepContext.next(stepContext.result);
}
```

## <a name="active-learning-is-saved-in-the-exported-knowledge-base"></a>Aprendizaje activo se guarda en la base de conocimiento exportada

Cuando la aplicación tiene el aprendizaje activo habilitado y exporta la aplicación, la columna `SuggestedQuestions` del archivo tsv conserva los datos de aprendizaje activo.

La columna `SuggestedQuestions` es un objeto JSON de la información de los comentarios implícitos, `autosuggested`, y explícitos, `usersuggested`. Un ejemplo de este objeto JSON para una sola pregunta enviada por un usuario de `help` es:

```JSON
[
    {
        "clusterHead": "help",
        "totalAutoSuggestedCount": 1,
        "totalUserSuggestedCount": 0,
        "alternateQuestionList": [
            {
                "question": "help",
                "autoSuggestedCount": 1,
                "userSuggestedCount": 0
            }
        ]
    }
]
```

También puede usar la API de descarga de modificaciones para revisar estas modificaciones, mediante REST o cualquiera de los SDK basados en lenguaje:
* [REST API](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75fc)
* [SDK de .NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.alterationsextensions.getasync?view=azure-dotnet)


Cuando se vuelva a importar esta aplicación, el aprendizaje activo continúa para recopilar información y sugerencias recomendadas para la base de conocimiento.



## <a name="best-practices"></a>Procedimientos recomendados

Para los procedimientos recomendados al usar el aprendizaje activo, consulte [Procedimientos recomendados](../Concepts/best-practices.md#active-learning).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Uso de metadatos con GenerateAnswer API](metadata-generateanswer-usage.md)
