---
title: Bot de Language Understanding C# v4
titleSuffix: Azure Cognitive Services
description: Con C#, cree un bot de chat que se integre con la descripción de lenguaje (LUIS). El bot se compila con la versión 4 de Bot Framework y el servicio de bot de aplicación web de Azure.
services: cognitive-services
author: diberry
ms.custom: seodec18
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 06/24/2019
ms.author: diberry
ms.openlocfilehash: 210724e8a8b9b585a3e308b8e321d809e4e897a1
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2019
ms.locfileid: "68560660"
---
# <a name="tutorial-use-a-web-app-bot-enabled-with-language-understanding-in-c"></a>Tutorial: usar un bot de aplicación web habilitado con Language Understanding en C#

Use C# para crear un bot de chat que se integre con Language Understanding (LUIS). El bot se ha compilado con el recurso [bot de aplicación web](https://docs.microsoft.com/azure/bot-service/) de Azure y [Bot Framework versión](https://github.com/Microsoft/botbuilder-dotnet) V4.

**En este tutorial, aprenderá a:**

> [!div class="checklist"]
> * Crear un bot de aplicación web. Este proceso crea una nueva aplicación de LUIS.
> * Descargar el proyecto de bot creado por el servicio de bot web
> * Iniciar el emulador y el bot localmente en el equipo.
> * Ver los resultados de expresiones del bot.

## <a name="prerequisites"></a>Requisitos previos

* [Bot Emulator](https://aka.ms/abs/build/emulatordownload)
* [Visual Studio](https://visualstudio.microsoft.com/downloads/)


## <a name="create-a-web-app-bot-resource"></a>Crear un nuevo recurso de bot de aplicación web

1. En [Azure Portal](https://portal.azure.com), haga clic en **Crear un recurso**.

1. En el cuadro de búsqueda, busque y seleccione **Bot de aplicación web**. Seleccione **Crear**.

1. En **Servicio de bots**, proporcione la información necesaria:

    |Configuración|Propósito|Configuración sugerida|
    |--|--|--|
    |Nombre del bot|Nombre del recurso|`luis-csharp-bot-` + `<your-name>`, por ejemplo, `luis-csharp-bot-johnsmith`|
    |Subscription|Suscripción donde se desea crear el bot.|Suscripción principal.
    |Grupos de recursos|Grupo lógico de recursos de Azure|Crear un nuevo grupo para almacenar todos los recursos utilizados con este bot, denominando el grupo `luis-csharp-bot-resource-group`.|
    |Ubicación|Región de Azure: no tiene que ser la misma que la región de creación o publicación de LUIS.|`westus`|
    |Plan de tarifa|Se usa para los límites de solicitud de servicio y facturación.|`F0` es el plan gratuito.
    |Nombre de la aplicación|El nombre se usa como el subdominio cuando el bot se implementa en la nube (por ejemplo, humanresourcesbot.azurewebsites.net).|`luis-csharp-bot-` + `<your-name>`, por ejemplo, `luis-csharp-bot-johnsmith`|
    |Plantilla de bot|Configuración de Bot Framework: ver la tabla siguiente|
    |Ubicación de la aplicación de LUIS|Debe ser la misma que la región de recursos de LUIS.|`westus`|
    |Plan de App Service/ubicación|No cambie el valor predeterminado proporcionado.|
    |Application Insights|No cambie el valor predeterminado proporcionado.|
    |Id. y contraseña de aplicación de Microsoft|No cambie el valor predeterminado proporcionado.|

1. En **Bot template** (Plantilla del bot), seleccione lo siguiente y luego elija el botón **Seleccionar** en esta configuración:

    |Configuración|Propósito|Número de selección|
    |--|--|--|
    |Versión del SDK|Versión de Bot Framework|**SDK v4**|
    |Lenguaje de SDK|Lenguaje de programación del bot|**C#**|
    |Bot|Tipo de bot|**Bot básico**|
    
1. Seleccione **Crear**. Esto crea e implementa el servicio de bots en Azure. Parte de este proceso crea una aplicación de LUIS denominada `luis-csharp-bot-XXXX`. Este nombre se basa en el nombre de la aplicación /Azure Bot Service.

    [![Creación de un bot de aplicación web](./media/bfv4-csharp/create-web-app-service.png)](./media/bfv4-csharp/create-web-app-service.png#lightbox)

    Antes de continuar, espere hasta que se crea el servicio bot.

## <a name="the-bot-has-a-language-understanding-model"></a>El bot tiene un modelo de Language Understanding.

El proceso de creación del servicio de bot también crea una nueva aplicación LUIS con intenciones y expresiones de ejemplo. El bot proporciona asignación de intenciones a la nueva aplicación de LUIS para las intenciones siguientes: 

|Intenciones de LUIS del bot básico|Expresión de ejemplo|
|--|--|
|Reservar vuelo|`Travel to Paris`|
|Cancelar|`bye`|
|None|Nada fuera del dominio de la aplicación.|

## <a name="test-the-bot-in-web-chat"></a>Probar el bot en Chat en web

1. Mientras sigue en Azure Portal para el nuevo bot, seleccione **Test in Web Chat** (Probar en chat web). 
1. En el cuadro de texto **Type your message** (Escriba su mensaje), escriba el texto `hello`. El bot responde con información sobre el marco del bot, así como consultas de ejemplo para el modelo LUIS específico, como la reserva de un vuelo a París. 

    ![Captura de pantalla de Azure Portal, escribir el texto "hello".](./media/bfv4-csharp/ask-bot-question-in-portal-test-in-web-chat.png)

    Puede usar la funcionalidad de prueba para probar rápidamente su bot. Para pruebas más completas, como la depuración, descargue el código de bot y use Visual Studio. 

## <a name="download-the-web-app-bot-source-code"></a>Descarga del código fuente del bot de aplicación web
Con el fin de desarrollar el código del bot de aplicación web, descargue el código y úselo en el equipo local. 

1. En Azure Portal, seleccione **Compilar** en la sección **Bot Management** (Administración del bot). 

1. Seleccione **Download Bot source code** (Descargar el código fuente del bot). 

    [![Descargar el código fuente del bot de aplicación web para el bot básico](../../../includes/media/cognitive-services-luis/bfv4/download-code.png)](../../../includes/media/cognitive-services-luis/bfv4/download-code.png#lightbox)

1. Cuando el cuadro de diálogo emergente le pregunte **Include app settings in the downloaded zip file?** (¿Incluir la configuración de la aplicación en el archivo zip descargado?), seleccione **Sí**.

1. Cuando el código fuente se haya comprimido, un mensaje proporcionará un vínculo para descargar el código. Seleccione el vínculo. 

1. Guarde el archivo ZIP en el equipo local y extraiga los ficheros. Abra el proyecto con Visual Studio. 

## <a name="review-code-to-send-utterance-to-luis-and-get-response"></a>Revise el código para enviar la expresión a LUIS y obtener respuesta

1. Abra el archivo **LuisHelper.cs**. Aquí es donde la expresión del usuario especificada en el bot se envía a LUIS. La respuesta de LUIS se devuelve desde el método como un objeto **BookDetails**. Al crear su bot propio, también debe crear su propio objeto para devolver los detalles de LUIS. 


    ```csharp
    // Copyright (c) Microsoft Corporation. All rights reserved.
    // Licensed under the MIT License.
    
    using System;
    using System.Linq;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Bot.Builder;
    using Microsoft.Bot.Builder.AI.Luis;
    using Microsoft.Extensions.Configuration;
    using Microsoft.Extensions.Logging;
    
    namespace Microsoft.BotBuilderSamples
    {
        public static class LuisHelper
        {
            public static async Task<BookingDetails> ExecuteLuisQuery(IConfiguration configuration, ILogger logger, ITurnContext turnContext, CancellationToken cancellationToken)
            {
                var bookingDetails = new BookingDetails();
    
                try
                {
                    // Create the LUIS settings from configuration.
                    var luisApplication = new LuisApplication(
                        configuration["LuisAppId"],
                        configuration["LuisAPIKey"],
                        "https://" + configuration["LuisAPIHostName"]
                    );
    
                    var recognizer = new LuisRecognizer(luisApplication);
    
                    // The actual call to LUIS
                    var recognizerResult = await recognizer.RecognizeAsync(turnContext, cancellationToken);
    
                    var (intent, score) = recognizerResult.GetTopScoringIntent();
                    if (intent == "Book_flight")
                    {
                        // We need to get the result from the LUIS JSON which at every level returns an array.
                        bookingDetails.Destination = recognizerResult.Entities["To"]?.FirstOrDefault()?["Airport"]?.FirstOrDefault()?.FirstOrDefault()?.ToString();
                        bookingDetails.Origin = recognizerResult.Entities["From"]?.FirstOrDefault()?["Airport"]?.FirstOrDefault()?.FirstOrDefault()?.ToString();
    
                        // This value will be a TIMEX. And we are only interested in a Date so grab the first result and drop the Time part.
                        // TIMEX is a format that represents DateTime expressions that include some ambiguity. e.g. missing a Year.
                        bookingDetails.TravelDate = recognizerResult.Entities["datetime"]?.FirstOrDefault()?["timex"]?.FirstOrDefault()?.ToString().Split('T')[0];
                    }
                }
                catch (Exception e)
                {
                    logger.LogWarning($"LUIS Exception: {e.Message} Check your LUIS configuration.");
                }
    
                return bookingDetails;
            }
        }
    }
    ```

1. Abra **BookingDetails.cs** para ver cómo el objeto abstrae la información de LUIS. 

    ```csharp
    // Copyright (c) Microsoft Corporation. All rights reserved.
    // Licensed under the MIT License.
    
    namespace Microsoft.BotBuilderSamples
    {
        public class BookingDetails
        {
            public string Destination { get; set; }
    
            public string Origin { get; set; }
    
            public string TravelDate { get; set; }
        }
    }
    ```

1. Abra **Cuadros de diálogo -> BookingDialog.cs** para saber cómo se usa el objeto BookingDetails para administrar el flujo de conversación. Los detalles de viaje se solicitan en pasos, luego la reserva completa se confirma y, por último, esta se repite al usuario. 

    ```csharp
    // Copyright (c) Microsoft Corporation. All rights reserved.
    // Licensed under the MIT License.
    
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Bot.Builder;
    using Microsoft.Bot.Builder.Dialogs;
    using Microsoft.Recognizers.Text.DataTypes.TimexExpression;
    
    namespace Microsoft.BotBuilderSamples.Dialogs
    {
        public class BookingDialog : CancelAndHelpDialog
        {
            public BookingDialog()
                : base(nameof(BookingDialog))
            {
                AddDialog(new TextPrompt(nameof(TextPrompt)));
                AddDialog(new ConfirmPrompt(nameof(ConfirmPrompt)));
                AddDialog(new DateResolverDialog());
                AddDialog(new WaterfallDialog(nameof(WaterfallDialog), new WaterfallStep[]
                {
                    DestinationStepAsync,
                    OriginStepAsync,
                    TravelDateStepAsync,
                    ConfirmStepAsync,
                    FinalStepAsync,
                }));
    
                // The initial child Dialog to run.
                InitialDialogId = nameof(WaterfallDialog);
            }
    
            private async Task<DialogTurnResult> DestinationStepAsync(WaterfallStepContext stepContext, CancellationToken cancellationToken)
            {
                var bookingDetails = (BookingDetails)stepContext.Options;
    
                if (bookingDetails.Destination == null)
                {
                    return await stepContext.PromptAsync(nameof(TextPrompt), new PromptOptions { Prompt = MessageFactory.Text("Where would you like to travel to?") }, cancellationToken);
                }
                else
                {
                    return await stepContext.NextAsync(bookingDetails.Destination, cancellationToken);
                }
            }
    
            private async Task<DialogTurnResult> OriginStepAsync(WaterfallStepContext stepContext, CancellationToken cancellationToken)
            {
                var bookingDetails = (BookingDetails)stepContext.Options;
    
                bookingDetails.Destination = (string)stepContext.Result;
    
                if (bookingDetails.Origin == null)
                {
                    return await stepContext.PromptAsync(nameof(TextPrompt), new PromptOptions { Prompt = MessageFactory.Text("Where are you traveling from?") }, cancellationToken);
                }
                else
                {
                    return await stepContext.NextAsync(bookingDetails.Origin, cancellationToken);
                }
            }
            private async Task<DialogTurnResult> TravelDateStepAsync(WaterfallStepContext stepContext, CancellationToken cancellationToken)
            {
                var bookingDetails = (BookingDetails)stepContext.Options;
    
                bookingDetails.Origin = (string)stepContext.Result;
    
                if (bookingDetails.TravelDate == null || IsAmbiguous(bookingDetails.TravelDate))
                {
                    return await stepContext.BeginDialogAsync(nameof(DateResolverDialog), bookingDetails.TravelDate, cancellationToken);
                }
                else
                {
                    return await stepContext.NextAsync(bookingDetails.TravelDate, cancellationToken);
                }
            }
    
            private async Task<DialogTurnResult> ConfirmStepAsync(WaterfallStepContext stepContext, CancellationToken cancellationToken)
            {
                var bookingDetails = (BookingDetails)stepContext.Options;
    
                bookingDetails.TravelDate = (string)stepContext.Result;
    
                var msg = $"Please confirm, I have you traveling to: {bookingDetails.Destination} from: {bookingDetails.Origin} on: {bookingDetails.TravelDate}";
    
                return await stepContext.PromptAsync(nameof(ConfirmPrompt), new PromptOptions { Prompt = MessageFactory.Text(msg) }, cancellationToken);
            }
    
            private async Task<DialogTurnResult> FinalStepAsync(WaterfallStepContext stepContext, CancellationToken cancellationToken)
            {
                if ((bool)stepContext.Result)
                {
                    var bookingDetails = (BookingDetails)stepContext.Options;
    
                    return await stepContext.EndDialogAsync(bookingDetails, cancellationToken);
                }
                else
                {
                    return await stepContext.EndDialogAsync(null, cancellationToken);
                }
            }
    
            private static bool IsAmbiguous(string timex)
            {
                var timexProperty = new TimexProperty(timex);
                return !timexProperty.Types.Contains(Constants.TimexTypes.Definite);
            }
        }
    }
    ```


## <a name="start-the-bot-code-in-visual-studio"></a>Iniciar el código del bot en Visual Studio

En Visual Studio, inicie el bot. Se abre una ventana del explorador con el sitio web del bot de aplicación web en `http://localhost:3978/`. Se muestra una página principal con información sobre su bot.

![Se muestra una página principal con información sobre su bot.](./media/bfv4-csharp/running-bot-web-home-page-success.png)

## <a name="use-the-bot-emulator-to-test-the-bot"></a>Usar el emulador de bot para probar el bot

1. Inicie el emulador de bots y seleccione **Open Bot** (Abrir bot).
1. En el cuadro de diálogo emergente **Open a bot** (Abrir un bot), escriba la dirección URL del bot, por ejemplo, `http://localhost:3978/api/messages`. La ruta `/api/messages` es la dirección web para el bot.
1. Escriba valores en **Microsoft App ID** (Id. de aplicación de Microsoft) y **Microsoft App password** (Contraseña de aplicación de Microsoft), que se encuentran en el archivo **appsettings.json** en la raíz del código de bot que descargó.

    Si lo desea, puede crear una nueva configuración de bot y copiar los valores `appId` y `appPassword` desde el archivo **appsettings.json** en el proyecto de Visual Studio para el bot. El nombre del archivo de configuración de bot debe ser el mismo que el nombre del bot. 

    ```json
    {
        "name": "<bot name>",
        "description": "<bot description>",
        "services": [
            {
                "type": "endpoint",
                "appId": "<appId from appsettings.json>",
                "appPassword": "<appPassword from appsettings.json>",
                "endpoint": "http://localhost:3978/api/messages",
                "id": "<don't change this value>",
                "name": "http://localhost:3978/api/messages"
            }
        ],
        "padlock": "",
        "version": "2.0",
        "overrides": null,
        "path": "<local path to .bot file>"
    }
    ```

1. En el emulador de bot, escriba `Hello` y obtenga la misma respuesta para el bot básico que la que recibió en el **Test in Web Chat** (Probar en chat web).

    [![Respuesta de bot básico en el emulador](./media/bfv4-csharp/ask-bot-emulator-a-question-and-get-response.png)](./media/bfv4-csharp/ask-bot-emulator-a-question-and-get-response.png#lightbox)


## <a name="ask-bot-a-question-for-the-book-flight-intent"></a>Formule una pregunta al bot para la intención de reservar vuelo

1. En el emulador de bot, reserve un vuelo. Para ello, especifique la expresión siguiente: 

    ```bot
    Book a flight from Paris to Berlin on March 22, 2020
    ```

    El emulador de bot solicita confirmación. 

1. Seleccione **Sí**. El bot responde con un resumen de sus acciones. 
1. En el registro del emulador bot, seleccione la línea que incluye `Luis Trace`. Esto muestra la respuesta JSON de LUIS para la intención y las entidades de la expresión.

    [![Respuesta de bot básico en el emulador](./media/bfv4-csharp/ask-luis-book-flight-question-get-json-response-in-bot-emulator.png)](./media/bfv4-csharp/ask-luis-book-flight-question-get-json-response-in-bot-emulator.png#lightbox)

[!INCLUDE [Bot Information](../../../includes/cognitive-services-qnamaker-luis-bot-info.md)]

## <a name="next-steps"></a>Pasos siguientes

Consulte más [ejemplos](https://github.com/microsoft/botframework-solutions) con bots de conversación. 

> [!div class="nextstepaction"]
> [Build a Language Understanding app with a custom subject domain](luis-quickstart-intents-only.md) (Compilación de una aplicación Language Understanding con un dominio de sujeto personalizado)
