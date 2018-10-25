---
title: 'Bot de preguntas y respuestas con Azure Bot Service: QnA Maker'
titleSuffix: Azure Cognitive Services
description: Este tutorial le guía a través de la creación de un bot de preguntas y respuestas con Azure Bot Service v3 en Azure Portal.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: tulasim
ms.openlocfilehash: 30400b04ec08d936242b022f10cf1485e009e6d2
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2018
ms.locfileid: "49647330"
---
# <a name="create-a-qna-bot-with-azure-bot-service-v3"></a>Creación de un bot de preguntas y respuestas con Azure Bot Service v3
Este tutorial le guía a través de la creación de un bot de preguntas y respuestas con Azure Bot Service v3 en Azure Portal.

## <a name="prerequisite"></a>Requisito previo
Antes de la compilación, siga los pasos descritos en [Creación de una base de conocimiento](../How-To/create-knowledge-base.md) para crear un servicio QnA Maker con preguntas y respuestas.

El bot responde a las preguntas de la base de conocimiento que creó, a través de QnAMakerDialog.

## <a name="create-a-qna-bot"></a>Crear un bot de QnA
1. En [Azure Portal](https://portal.azure.com), haga clic en **Crear** un recurso en la hoja de menú y, después, haga clic en **Ver todo**.

    ![Creación del servicio de bot](../media/qnamaker-tutorials-create-bot/bot-service-creation.png)

2. En el cuadro de búsqueda, busque **Bot de aplicación web**.

    ![Selección del servicio de bot](../media/qnamaker-tutorials-create-bot/bot-service-selection.png)

3. En la **hoja Bot Service**, proporcione la información necesaria:

    - Establezca **Nombre de la aplicación** en el nombre del bot. El nombre se usa como el subdominio cuando el bot se implementa en la nube (por ejemplo, mynotesbot.azurewebsites.net).
    - Seleccione la suscripción, el grupo de recursos, el plan de App Service y la ubicación.

4. Para consultar las instrucciones para crear un bot de preguntas y respuestas con SDK v4: consulte [Plantilla de bot de QnA v4](https://aka.ms/qna-bot-v4). Para usar las plantillas de v3, seleccione la versión del SDK de **SDK v3** y el idioma del SDK de **C#** o **Node.js**.

    ![configuración de sdk de bot](../media/qnamaker-tutorials-create-bot/bot-v3.png)

5. Seleccione la plantilla **Preguntas y respuestas** para el campo de plantilla de bot y, a continuación, guarde la configuración de la plantilla con **Seleccionar**.

    ![Selección del servicio de bot](../media/qnamaker-tutorials-create-bot/bot-v3-template.png)

6. Revise la configuración y seleccione **Crear**. Esto crea e implementa el servicio de bot con QnAMakerDialog en Azure.

    ![Selección del servicio de bot](../media/qnamaker-tutorials-create-bot/bot-blade-settings-v3.png)

7. Confirme que se ha implementado el servicio de bot.

    - Haga clic en **Notificaciones** (el icono de la campana que se encuentra en el borde superior de Azure Portal). La notificación cambiará de **Implementación iniciada** a **Implementación correcta**.
    - Después de que la notificación cambie a **Implementación correcta**, seleccione **Ir al recurso** en esa notificación.

## <a name="chat-with-the-bot"></a>Chat con el bot
Al seleccionar **Ir al recurso** se accede a la hoja de recursos del bot.

Una vez registrado el bot, haga clic en **Test in Web Chat** (Probar en Chat en web) para abrir el panel Chat en web. Escriba "hola" en el Chat en web.

![Chat en web del bot de QnA](../media/qnamaker-tutorials-create-bot/qna-bot-web-chat.PNG)

El bot responde con "Establezca QnAKnowledgebaseId y QnASubscriptionKey en la configuración de la aplicación. Obtenga información sobre cómo obtenerlas en https://aka.ms/qnaabssetup". Esta respuesta confirma que el bot de QnA ha recibido el mensaje, pero todavía no hay ninguna base de conocimiento de QnA Maker asociada. Lo hará en el paso siguiente.

## <a name="connect-your-qna-maker-knowledge-base-to-the-bot"></a>Conectar la base de conocimiento de QnA Maker con el bot

1. Abra **Configuración de la aplicación** y modifique los campos **QnAKnowledgebaseId**, **QnAAuthKey** y **QnAEndpointHostName** para incluir los valores de la base de conocimiento de QnA Maker.

    ![configuración de aplicaciones](../media/qnamaker-tutorials-create-bot/application-settings.PNG)

2. Obtenga el identificador de la base de conocimiento, la dirección URL del host y la clave de punto de conexión de la pestaña Configuración de la base de conocimiento en https://qnamaker.ai.
    - Inicio de sesión en [QnA Maker](https://qnamaker.ai)
    - Vaya a la base de conocimiento.
    - Haga clic en la pestaña **Configuración**.
    - **Publique** la base de conocimiento, si todavía no lo ha hecho.

    ![Valores de QnA Maker](../media/qnamaker-tutorials-create-bot/qnamaker-settings-kbid-key.PNG)

> [!NOTE]
> Si quiere conectar la versión preliminar de la base de conocimiento con el bot de QnA, establezca el valor de **Ocp-Apim-Subscription-Key** en **QnAAuthKey**. Deje **QnAEndpointHostName** vacío.

## <a name="test-the-bot"></a>Probar el bot
En Azure Portal, haga clic en **Test in Web Chat** (Probar en Chat en web) para probar el bot. 

![Bot de QnA Maker](../media/qnamaker-tutorials-create-bot/qna-bot-web-chat-response.PNG)

Ahora el bot de QnA Bot responde desde la base de conocimiento.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Integración de QnA Maker y LUIS](./integrate-qnamaker-luis.md)

## <a name="see-also"></a>Otras referencias

- [Administración de la base de conocimiento](https://qnamaker.ai)
- [Habilitación del bot en otros canales](https://docs.microsoft.com/azure/bot-service/bot-service-manage-channels)
