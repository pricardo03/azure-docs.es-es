---
title: Bot de LUIS con C# - Tutorial - Bot de aplicación web - Bot Framework SDK 4.0
titleSuffix: Azure Cognitive Services
description: Con C#, cree un bot de chat que se integre con la descripción de lenguaje (LUIS). Este bot de chat utiliza la aplicación de recursos humanos para implementar con rapidez una solución de bot. El bot se compila con la versión 4 de Bot Framework y el bot de aplicación web de Azure.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 09/25/2018
ms.author: diberry
ms.openlocfilehash: ce5b704a7ac251621698352608ea3eefa4629aea
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2018
ms.locfileid: "48886592"
---
# <a name="tutorial-luis-bot-in-c"></a>Tutorial: Bot de LUIS en C#
Con C#, puede crear un bot de chat que se integre con Language Understanding (LUIS). Este bot usa la aplicación HomeAutomation para implementar una solución de bot. El bot se compila con el [bot de aplicación web](https://docs.microsoft.com/azure/bot-service/) de Azure y con la [versión 4 de Bot Framework](https://github.com/Microsoft/botbuilder-js).

**En este tutorial, aprenderá a:**

> [!div class="checklist"]
> * Crear un bot de aplicación web. Este proceso crea una nueva aplicación de LUIS.
> * Agregar un dominio creado previamente para el nuevo modelo de LUIS.
> * Descargar el proyecto creado por el servicio de bot web.
> * Iniciar el emulador y el bot localmente en el equipo.
> * Modificar el código del bot para nuevas intenciones de LUIS.
> * Ver los resultados de expresiones del bot.

## <a name="prerequisites"></a>Requisitos previos

* [Bot Emulator](https://aka.ms/abs/build/emulatordownload)
* [Visual Studio](https://visualstudio.microsoft.com/downloads/)


## <a name="create-web-app-bot"></a>Creación de una aplicación web

1. En [Azure Portal](https://portal.azure.com), haga clic en **Crear un recurso**.

2. En el cuadro de búsqueda, busque y seleccione **Bot de aplicación web**. Seleccione **Crear**.

3. En **Bot Service**, proporcione la información necesaria:

    |Configuración|Propósito|Configuración sugerida|
    |--|--|--|
    |Nombre del bot|Nombre del recurso|`luis-csharp-bot-` + `<your-name>`, por ejemplo, `luis-csharp-bot-johnsmith`|
    |Subscription|Suscripción donde se creará el bot.|Suscripción principal.
    |Grupos de recursos|Grupo lógico de recursos de Azure|Crear un nuevo grupo para almacenar todos los recursos utilizados con este bot, denominando el grupo `luis-csharp-bot-resource-group`.|
    |Ubicación|Región de Azure: no tiene que ser la misma que la región de creación o publicación de LUIS.|`westus`|
    |Plan de tarifa|Se usa para los límites de solicitud de servicio y facturación.|`F0` es el nivel Gratis.
    |Nombre de la aplicación|El nombre se usa como subdominio cuando el bot se implementa en la nube (por ejemplo, humanresourcesbot.azurewebsites.net).|`luis-csharp-bot-` + `<your-name>`, por ejemplo, `luis-csharp-bot-johnsmith`|
    |Plantilla de bot|Configuración de Bot Framework: ver la tabla siguiente|
    |Ubicación de la aplicación de LUIS|Debe ser la misma que la región de recursos de LUIS.|`westus`|

4. En **Bot template settings** (Configuración de la plantilla del bot), seleccione lo siguiente y luego elija el botón **Seleccionar** en esta configuración:

    |Configuración|Propósito|Número de selección|
    |--|--|--|
    |Versión del SDK|Versión de Bot Framework|**SDK v4**|
    |Lenguaje de SDK|Lenguaje de programación del bot|**C#**|
    |Bot básico o de eco|Tipo de bot|**Bot básico**|
    
5. Seleccione **Crear**. Esto crea e implementa el servicio de bots en Azure. Parte de este proceso crea una aplicación de LUIS denominada `luis-csharp-bot-XXXX`. Este nombre se basa en el nombre del bot y de la aplicación de la sección anterior.

    [ ![Creación de una aplicación web](./media/bfv4-csharp/create-web-app-service.png) ](./media/bfv4-csharp/create-web-app-service.png#lightbox)

6. Deje esta pestaña del explorador abierta. Para cualquier paso con el portal de LUIS, abra una nueva pestaña del explorador. Continúe con la sección siguiente cuando se implemente el nuevo servicio de bots.

## <a name="add-prebuilt-domain-to-model"></a>Incorporación de un dominio creado previamente al modelo
Parte de la implementación del servicio de bots crea una nueva aplicación de LUIS con intenciones y expresiones de ejemplo. El bot proporciona asignación de intenciones a la nueva aplicación de LUIS para las intenciones siguientes: 

|Intenciones de LUIS del bot básico|Expresión de ejemplo|
|--|--|
|Cancelar|`stop`|
|Greeting|`hello`|
|Ayuda|`help`|
|None|Nada fuera del dominio de la aplicación.|

Agregue al modelo la aplicación HomeAutomation creada previamente para controlar expresiones como: `Turn off the living room lights`

1. Vaya al portal de [LUIS](https://www.luis.ai) e inicie sesión.
2. En la página **Mis aplicaciones**, seleccione la columna **Fecha de creación** para ordenar por la fecha en la que se creó la aplicación. Azure Bot Service creó una nueva aplicación en la sección anterior. Su nombre es `luis-csharp-bot-` + `<your-name>` + 4 caracteres aleatorios.
3. Abra la aplicación y seleccione la sección **Compilar** en el panel de navegación superior.
4. En el panel de navegación izquierdo, seleccione **Prebuilt Domains** (Dominios creados previamente).
5. Seleccione el dominio **HomeAutomation** en **Agregar dominio** de su tarjeta.
6. Seleccione **Train** (Entrenar) en el menú de la esquina superior derecha.
7. Seleccione **Publicar** (Publicar) en el menú de la esquina superior derecha. 

    La aplicación creada por Azure Bot Service ahora tiene intenciones nuevas:

    |Nuevas intenciones del bot básico|Expresión de ejemplo|
    |--|--|
    |HomeAutomation.TurnOn|`turn the fan to high`
    |HomeAutomation.TurnOff|`turn off ac please`|

## <a name="download-the-web-app-bot"></a>Descarga del bot de aplicación web 
Con el fin de desarrollar el código del bot de aplicación web, descargue el código y úselo en el equipo local. 

1. En Azure Portal, aún en el recurso del bot de aplicación web, seleccione **Configuración de la aplicación** y copie los valores de **botFilePath** y **botFileSecret**. Deberá agregarlos a un archivo de entorno más adelante. 

2. En Azure Portal, seleccione **Compilar** en la sección **Bot Management** (Administración del bot). 

3. Seleccione **Download Bot source code** (Descargar el código fuente del bot). 

    [ ![Descargar el código fuente del bot de aplicación web para el bot básico](../../../includes/media/cognitive-services-luis/bfv4/download-code.png) ](../../../includes/media/cognitive-services-luis/bfv4/download-code.png#lightbox)

4. Cuando el código fuente se haya comprimido, un mensaje proporcionará un vínculo para descargar el código. Seleccione el vínculo. 

5. Guarde el archivo ZIP en el equipo local y extraiga los ficheros. Abra el proyecto. 

6. Abra el archivo bot.cs y busque `_services.LuisServices`. Aquí es donde la expresión del usuario especificada en el bot se envía a LUIS.

    ```csharp
    /// <summary>
    /// Run every turn of the conversation. Handles orchestration of messages.
    /// </summary>
    /// <param name="turnContext">Bot Turn Context.</param>
    /// <param name="cancellationToken">Task CancellationToken.</param>
    /// <returns>A <see cref="Task"/> representing the asynchronous operation.</returns>
    public async Task OnTurnAsync(ITurnContext turnContext, CancellationToken cancellationToken)
    {
        var activity = turnContext.Activity;

        if (activity.Type == ActivityTypes.Message)
        {
            // Perform a call to LUIS to retrieve results for the current activity message.
            var luisResults = await _services.LuisServices[LuisConfiguration].RecognizeAsync(turnContext, cancellationToken).ConfigureAwait(false);

            // If any entities were updated, treat as interruption.
            // For example, "no my name is tony" will manifest as an update of the name to be "tony".
            var topScoringIntent = luisResults?.GetTopScoringIntent();

            var topIntent = topScoringIntent.Value.intent;
            switch (topIntent)
            {
                case GreetingIntent:
                    await turnContext.SendActivityAsync("Hello.");
                    break;
                case HelpIntent:
                    await turnContext.SendActivityAsync("Let me try to provide some help.");
                    await turnContext.SendActivityAsync("I understand greetings, being asked for help, or being asked to cancel what I am doing.");
                    break;
                case CancelIntent:
                    await turnContext.SendActivityAsync("I have nothing to cancel.");
                    break;
                case NoneIntent:
                default:
                    // Help or no intent identified, either way, let's provide some help.
                    // to the user
                    await turnContext.SendActivityAsync("I didn't understand what you just said to me.");
                    break;
            }
        }
        else if (activity.Type == ActivityTypes.ConversationUpdate)
        {
            if (activity.MembersAdded.Any())
            {
                // Iterate over all new members added to the conversation.
                foreach (var member in activity.MembersAdded)
                {
                    // Greet anyone that was not the target (recipient) of this message.
                    // To learn more about Adaptive Cards, see https://aka.ms/msbot-adaptivecards for more details.
                    if (member.Id != activity.Recipient.Id)
                    {
                        var welcomeCard = CreateAdaptiveCardAttachment();
                        var response = CreateResponse(activity, welcomeCard);
                        await turnContext.SendActivityAsync(response).ConfigureAwait(false);
                    }
                }
            }
        }

    }
    ```

    El bot envía la expresión del usuario a LUIS y obtiene los resultados. La intención principal determina el flujo de conversación. 


## <a name="start-the-bot"></a>Inicio del bot
Antes de cambiar cualquier código o configuración, compruebe los trabajos del bot. 

1. En Visual Studio, abra el archivo de solución. 

2. Crear un archivo `appsettings.json` que contendrá las variables del bot que busca el código del bot:

    ```JSON
    {
    "botFileSecret": "",
    "botFilePath": ""

    }
    ```

    Establezca los valores de las variables en los valores que copió de la configuración de la aplicación de Azure Bot Service en el paso 1 de la sección **[Descarga del bot de aplicación web](#download-the-web-app-bot)**.

3. En Visual Studio, inicie el bot. Se abre una ventana del explorador con el sitio web del bot de aplicación web en `http://localhost:3978/`.

## <a name="start-the-emulator"></a>Inicio del emulador

1. Inicie Bot Emulator.

2. En Bot Emulator, seleccione el archivo *.bot en la raíz del proyecto. Este archivo `.bot` incluye el punto de conexión de dirección URL del bot para mensajes:

    [ ![Bot Emulator v4](../../../includes/media/cognitive-services-luis/bfv4/bot-emulator-v4.png) ](../../../includes/media/cognitive-services-luis/bfv4/bot-emulator-v4.png#lightbox)

3. Introduzca el secreto de bot que copió de la configuración de la aplicación de Azure Bot Service en el paso 1 de la sección **[Descargar el bot de aplicación web](#download-the-web-app-bot)**. Esto permite que el emulador obtenga acceso a los campos cifrados del archivo `.bot`.

    ![Secreto de Bot Emulator v4](../../../includes/media/cognitive-services-luis/bfv4/bot-secret.png)

4. En Bot Emulator, escriba `Hello` y obtendrá la respuesta apropiada para el bot básico.

    [ ![Respuesta de bot básico en el emulador](../../../includes/media/cognitive-services-luis/bfv4/emulator-test.png) ](../../../includes/media/cognitive-services-luis/bfv4/emulator-test.png#lightbox)

## <a name="modify-bot-code"></a>Modificación del código del bot 

En el archivo `BasicBot.cs`, agregue código para controlar las intenciones nuevas. 

1. En la parte superior del archivo, busque la sección **Supported LUIS Intents** (Intenciones admitidas de LUIS) y agregue constantes para las intenciones de HomeAutomation:

    ```csharp
    // Supported LUIS Intents
    public const string GreetingIntent = "Greeting";
    public const string CancelIntent = "Cancel";
    public const string HelpIntent = "Help";
    public const string NoneIntent = "None";
    public const string TurnOnIntent = "HomeAutomation_TurnOn"; // new intent
    public const string TurnOffIntent = "HomeAutomation_TurnOff"; // new intent
    ```

    Tenga en cuenta que el punto, `.`, entre el dominio y la intención de la aplicación del portal de LUIS se reemplaza por un guion bajo, `_`. 

2. Busque el método **OnTurnAsync** que recibe la predicción de LUIS de la expresión. Agregue el código en la instrucción de intercambio para devolver la respuesta de LUIS para las dos intenciones de HomeAutomation. 

    ```csharp
    case TurnOnIntent:
        await turnContext.SendActivityAsync("TurnOn intent found, JSON response: " + luisResults?.Entities.ToString());
        break;
    case TurnOffIntent:
        await turnContext.SendActivityAsync("TurnOff intent found, JSON response: " + luisResults?.Entities.ToString());
        break;
    ```

    El bot no tiene la misma respuesta exacta como una solicitud de API REST de LUIS, por lo que es importante conocer las diferencias examinando el JSON de respuesta. Las propiedades de texto y de las intenciones son las mismas, pero se han modificado los valores de propiedad de las entidades. 

    ```JSON
    {
        "$instance": {
            "HomeAutomation_Device": [
                {
                    "startIndex": 23,
                    "endIndex": 29,
                    "score": 0.9776345,
                    "text": "lights",
                    "type": "HomeAutomation.Device"
                }
            ],
            "HomeAutomation_Room": [
                {
                    "startIndex": 12,
                    "endIndex": 22,
                    "score": 0.9079433,
                    "text": "livingroom",
                    "type": "HomeAutomation.Room"
                }
            ]
        },
        "HomeAutomation_Device": [
            "lights"
        ],
        "HomeAutomation_Room": [
            "livingroom"
        ]
    }
    ```



## <a name="view-results-in-bot"></a>Visualización de los resultados en el bot

1. En Bot Emulator, escriba la expresión: `Turn on the livingroom lights to 50%`

2. El bot responde con:

    ```JSON
    TurnOn intent found, JSON response: {"$instance":{“HomeAutomation_Device”:[{“startIndex”:23,“endIndex”:29,“score”:0.9776345,“text”:“lights”,“type”:“HomeAutomation.Device”}],“HomeAutomation_Room”:[{“startIndex”:12,“endIndex”:22,“score”:0.9079433,“text”:“livingroom”,“type”:“HomeAutomation.Room”}]},“HomeAutomation_Device”:[“lights”],“HomeAutomation_Room”:[“livingroom”]}
    ```    

## <a name="learn-more-about-bot-framework"></a>Más información sobre Bot Framework
Azure Bot Service usa el SDK de Bot Framework. Más información sobre el SDK y bot framework:

* Documentación sobre [Azure Bot Service](https://docs.microsoft.com/en-us/azure/bot-service/bot-service-overview-introduction?view=azure-bot-service-4.0) v4
* [Ejemplos de Bot Builder](https://github.com/Microsoft/botbuilder-samples)
* [SDK de Bot Builder](https://docs.microsoft.com/en-us/javascript/api/botbuilder-core/?view=botbuilder-ts-latest)
* [Herramientas de Bot Builder](https://github.com/Microsoft/botbuilder-tools):

## <a name="next-steps"></a>Pasos siguientes

Creó una instancia de Azure Bot Service, copió el secreto del bot y la ruta de acceso al archivo `.bot` y descargó el archivo ZIP del código. Agregó el dominio HomeAutomation creado previamente a la aplicación LUIS que se creó como parte de la nueva instancia de Azure Bot Service y luego entrenó y publicó nuevamente la aplicación. Extrajo el código del proyecto, creó un archivo de entorno (`.env`) y estableció el secreto del bot y la ruta de acceso al archivo `.bot`. En el archivo bot.js, agregó código para controlar las dos intenciones nuevas. Luego probó el bot en Bot Emulator para ver la respuesta de LUIS para una expresión de una de las intenciones nuevas. 


> [!div class="nextstepaction"]
> [Compilación de un dominio personalizado en LUIS](luis-quickstart-intents-only.md)
