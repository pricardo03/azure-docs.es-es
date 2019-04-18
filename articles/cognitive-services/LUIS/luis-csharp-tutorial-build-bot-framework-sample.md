---
title: Bot - C# - v3
titleSuffix: Language Understanding - Azure Cognitive Services
description: Con C#, cree un bot de chat que se integre con la descripción de lenguaje (LUIS). Este bot de chat utiliza el dominio HomeAutomation creado previamente para implementar con rapidez una solución de bot.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 01/09/2019
ms.author: diberry
ms.openlocfilehash: f23cf78bfca48b3a78e234520d645abdb354038f
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "58878475"
---
# <a name="luis-bot-in-c-with-the-bot-framework-3x-and-the-azure-web-app-bot"></a>Bot de LUIS en C# con Bot Framework 3.x y el bot de aplicación web de Azure

Con C#, cree un bot de chat que se integre con la descripción de lenguaje (LUIS). Este bot de chat utiliza el dominio HomeAutomation creado previamente para implementar con rapidez una solución de bot. El bot se compila con Bot Framework 3.x y el bot de aplicación web de Azure.

## <a name="prerequisite"></a>Requisito previo

* [Aplicación de LUIS HomeAutomation](luis-get-started-create-app.md). Las intenciones de esta aplicación de LUIS se asignan a los controladores del cuadro de diálogo del bot. 

## <a name="luis-homeautomation-intents"></a>Intenciones de HomeAutomation de LUIS

| Intención | Expresión de ejemplo | Funcionalidad del bot |
|:----:|:----------:|---|
| HomeAutomation.TurnOn | Encender las luces. | Cuando se detecta la intención de LUIS `HomeAutomation.TurnOn`, el bot invoca al controlador del cuadro de diálogo `OnIntent`. Este cuadro de diálogo es donde llamaría a un servicio de IoT para activar un dispositivo e indicar al usuario que se ha activado. |
| HomeAutomation.TurnOff | Apagar las luces de la habitación. | Cuando se detecta la intención de LUIS `HomeAutomation.TurnOff`, el bot invoca al controlador del cuadro de diálogo `OffIntent`. Este cuadro de diálogo es donde llamaría a un servicio de IoT para desactivar un dispositivo e indicar al usuario que se ha desactivado. |

## <a name="create-a-language-understanding-bot-with-bot-service"></a>Crear un bot de Language Understanding con Bot Service

1. En [Azure Portal](https://portal.azure.com), haga clic en **Crear nuevo recurso** en el menú superior izquierdo.

    ![Creación de un recurso en Azure Portal](./media/luis-tutorial-cscharp-web-bot/bot-service-creation.png)

2. En el cuadro de búsqueda, busque **Bot de aplicación web**. 

    ![Selección del bot de la aplicación web como tipo de recurso](./media/luis-tutorial-cscharp-web-bot/bot-service-selection.png)

3. En la ventana del bot de aplicación web, haga clic en **Crear**.

4. En **Servicio de bots**, proporcione la información necesaria y haga clic en **Crear**. Esto crea e implementa el servicio de bots y la aplicación de LUIS en Azure. Si quiere usar la [preparación para la voz](https://docs.microsoft.com/bot-framework/bot-service-manage-speech-priming), consulte [los requisitos de región](troubleshooting.md#what-luis-regions-support-bot-framework-speech-priming) antes de crear el bot. 
   * Establezca **Nombre de la aplicación** en el nombre del bot. El nombre se usa como el subdominio cuando el bot se implementa en la nube (por ejemplo, mynotesbot.azurewebsites.net). <!-- This name is also used as the name of the LUIS app associated with your bot. Copy it to use later, to find the LUIS app associated with the bot. -->
   * Seleccione la suscripción, el [grupo de recursos](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview), el plan de App Service y la [ubicación](https://azure.microsoft.com/regions/).
   * En **Bot template** (Plantilla de bot), seleccione:
       * **SDK v3**
       * **C#**
       * **Comprensión del lenguaje**
   * Seleccione el valor de **LUIS App Location** (Ubicación de la aplicación de LUIS). Esta es la [región](luis-reference-regions.md) de creación en la que se crea la aplicación.
   * Active la casilla de confirmación del aviso legal. Los términos del aviso legal están debajo de la casilla.

     ![Servicio de bots](./media/luis-tutorial-cscharp-web-bot/bot-service-setting-callout-template.png)


5. Confirme que se ha implementado el servicio de bots.
    * Haga clic en Notificaciones (el icono de la campana que se encuentra en el borde superior de Azure Portal). La notificación cambia de **Implementación iniciada** a **Implementación correcta**.
    * Después de que la notificación cambie a **Implementación correcta**, haga clic en **Ir al recurso** en esa notificación.

> [!Note]
> Este proceso de creación del bot de aplicación web también crea una aplicación de LUIS. Se ha entrenado y publicado de forma automática. 

## <a name="try-the-default-bot"></a>Probar el bot predeterminado

Para confirmar que el bot se ha implementado, active la casilla **Notificaciones**. Las notificaciones cambian de **Implementación en curso...** a **Implementación correcta**. Haga clic en el botón **Ir al recurso** para abrir los recursos del bot.

Una vez implementado el bot, haga clic en **Test in Web Chat** (Probar en Chat en web) para abrir el panel Chat en web. Escriba "hello" ("hola") en el Chat en web.

  ![Probar el bot en Chat en web](./media/luis-tutorial-cscharp-web-bot/bot-service-web-chat.png)

El bot responde "You have reached Greeting. You said: hello" ("Se ha puesto en contacto con Saludo. Ha dicho: hola").  Esta respuesta confirma que el bot ha recibido el mensaje y lo ha pasado a una aplicación de LUIS predeterminada que ha creado. Esta aplicación de LUIS predeterminada ha detectado una intención Saludo. En el paso siguiente, conectará el bot con la aplicación de LUIS que ha creado anteriormente en lugar de la aplicación de LUIS predeterminada.

## <a name="connect-your-luis-app-to-the-bot"></a>Conectar la aplicación de LUIS con el bot

Abra **Configuración de la aplicación** y edite el campo **LuisAppId** para que contenga el identificador de la aplicación de LUIS. Si ha creado la aplicación de LUIS HomeAutomation en una región que no sea Oeste de EE. UU., debe cambiar también el valor de **LuisAPIHostName**. El valor de **LuisAPIKey** está establecido actualmente en la clave de creación. Lo cambiará por la clave de punto de conexión cuando el tráfico supere la cuota de nivel gratis. 

  ![Actualizar el identificador de la aplicación de LUIS en Azure](./media/luis-tutorial-cscharp-web-bot/bot-service-app-settings.png)

> [!Note]
> Si no tiene el identificador de la aplicación de LUIS de la [aplicación HomeAutomation](luis-get-started-create-app.md), inicie sesión en el sitio web de [LUIS](luis-reference-regions.md) con la misma cuenta que usa para iniciar sesión en Azure. 
> 1. Haga clic en **Mis aplicaciones**. 
> 2. Busque la aplicación de LUIS que ha creado anteriormente, que contiene las intenciones y entidades del dominio HomeAutomation.
> 3. En la página **Configuración** de la aplicación de LUIS, busque y copie el identificador de la aplicación. Asegúrese de que está [entrenada](luis-interactive-test.md) y [publicada](luis-how-to-publish-app.md). 
> 
> [!WARNING]
> Si elimina la clave de LUIS o de Id. de aplicación, el bot dejará de funcionar.

## <a name="modify-the-bot-code"></a>Modificar el código del bot

1. Haga clic en **Compilar** y después en **Open online code editor** (Abrir el editor de código en línea).

   ![Abrir el editor de código en línea](./media/luis-tutorial-cscharp-web-bot/bot-service-build.png)

2. Haga clic con el botón derecho en `build.cmd` y elija **Run from Console** (Ejecutar desde la consola) para compilar la aplicación. El servicio completa automáticamente varios pasos de compilación. La compilación se completa cuando finaliza con el mensaje "Finalizado correctamente".

3. En el editor de código, abra `/Dialogs/BasicLuisDialog.cs`. Contiene el siguiente código:

   [!code-csharp[Default BasicLuisDialog.cs](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/Default_BasicLuisDialog.cs "Default BasicLuisDialog.cs")]

## <a name="change-code-to-homeautomation-intents"></a>Cambiar el código para las intenciones de HomeAutomation


1. Quite los tres atributos y métodos de intención de **Saludo**, **Cancelar** y **Ayuda**. Estas intenciones no se usan en el dominio HomeAutomation creado previamente. Asegúrese de mantener el atributo y el método de la intención **None**. 

2. Agregue dependencias a la parte superior del archivo, con las demás dependencias:

   [!code-csharp[Dependencies](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/BasicLuisDialog.cs?range=4-5&dedent=8 "dependencies")]

3. Agregue constantes para administrar las cadenas en la parte superior de la clase `BasicLuisDialog`:

   [!code-csharp[Add Intent and Entity Constants](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/BasicLuisDialog.cs?range=23-32&dedent=8 "Add Intent and Entity Constants")]

4. Agregue el código de las nuevas intenciones de `HomeAutomation.TurnOn` y `HomeAutomation.TurnOff` dentro de la clase `BasicLuisDialog`:

   [!code-csharp[Add Intents](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/BasicLuisDialog.cs?range=61-71&dedent=8 "Add Intents")]

5. Agregue el código para obtener las entidades que encuentre LUIS dentro de la clase `BasicLuisDialog`:

   [!code-csharp[Collect entities](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/BasicLuisDialog.cs?range=34-53&dedent=8 "Collect entities")]

6. Cambie el método **ShowLuisResult** de la clase `BasicLuisDialog` para que redondee la puntuación, recopile las entidades y muestre el mensaje de respuesta en el bot de chat:

   [!code-csharp[Display message in chatbot](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/BasicLuisDialog.cs?range=73-83&dedent=8 "Display message in chatbot")]

## <a name="build-the-bot"></a>Compilar el bot
En el editor de código, haga clic con el botón derecho en `build.cmd` y seleccione **Run from Console** (Ejecutar desde la consola).

![Compilar el bot web](./media/luis-tutorial-cscharp-web-bot/bot-service-build-run-from-console.png)

La vista de código se reemplaza con una ventana de terminal en la que se muestran el progreso y los resultados de la compilación.

![Compilación correcta del bot web](./media/luis-tutorial-cscharp-web-bot/bot-service-build-success.png)

> [!TIP]
> Un método alternativo para compilar el bot consiste en seleccionar el nombre del bot en la barra azul superior y seleccionar **Open Kudu Console** (Abrir la consola de Kudu). La consola se abre en **D:\home**. 
> 
> Cambie el directorio a **D:\home\site\wwwroot**; para ello, escriba: `cd site\wwwroot`
>
> Ejecute el script de compilación; para ello, escriba: `build.cmd`

## <a name="test-the-bot"></a>Probar el bot

En Azure Portal, haga clic en **Test in Web Chat** (Probar en Chat en web) para probar el bot. Escriba mensajes como "Turn on the lights" ("Encender las luces") o "Turn off my heater" ("Apagar la calefacción") para invocar las intenciones que le haya agregado.

   ![Probar el bot HomeAutomation en Chat en web](./media/luis-tutorial-cscharp-web-bot/bot-service-chat-results.png)

> [!TIP]
> Puede volver a entrenar la aplicación de LUIS sin modificar el código del bot. Vea [Add example utterances](https://docs.microsoft.com/azure/cognitive-services/LUIS/add-example-utterances) (Agregar expresiones de ejemplo) y [Train and test your LUIS app](https://docs.microsoft.com/azure/cognitive-services/LUIS/luis-interactive-test) (Entrenar y probar la aplicación de LUIS). 

## <a name="download-the-bot-to-debug"></a>Descargar el bot para depurarlo
Si el bot no funciona, descargue el proyecto en el equipo local y siga [depurándolo](https://docs.microsoft.com/bot-framework/bot-service-debug-bot). 

## <a name="learn-more-about-bot-framework"></a>Más información sobre Bot Framework
Obtenga más información sobre [Bot Framework](https://dev.botframework.com/) y las versiones [3.x](https://github.com/Microsoft/BotBuilder) y [4.x](https://github.com/Microsoft/botbuilder-dotnet) de los SDK.

## <a name="next-steps"></a>Pasos siguientes

Agregue las intenciones de LUIS y los cuadros de diálogo de Bot Service para controlar las intenciones **Ayuda**, **Cancelar** y **Saludo**. No olvide entrenar, publicar y [compilar](#build-the-bot) el bot de aplicación web. Tanto LUIS como el bot deberían tener las mismas intenciones.

Consulte más [ejemplos](https://github.com/Microsoft/AI) con bots de conversación. 

> [!div class="nextstepaction"]
> [Add intents (Agregar intenciones)](./luis-how-to-add-intents.md)
> [Speech priming (Preparación para la voz)](https://docs.microsoft.com/bot-framework/bot-service-manage-speech-priming)

<!-- tested on Win10 -->
