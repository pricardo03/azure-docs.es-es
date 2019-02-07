---
title: 'Bot de preguntas y respuestas con Azure Bot Service: QnA Maker'
titleSuffix: Azure Cognitive Services
description: Este tutorial le guía a través de la creación de un bot de preguntas y respuestas con Azure Bot Service v3 en Azure Portal.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: qna-maker`
ms.topic: article
ms.date: 01/24/2019
ms.author: tulasim
ms.openlocfilehash: 1e1152f810bffb2ab4dfe230c2d3b3c2a8198492
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55811926"
---
# <a name="tutorial-create-a-qna-bot-with-azure-bot-service-v3"></a>Tutorial: Creación de un bot de preguntas y respuestas con Azure Bot Service v3

Este tutorial le lleva por la creación de un bot de preguntas y respuestas con Azure Bot Service v3 en [Azure Portal](https://portal.azure.com) sin escribir código. La conexión de una base de conocimiento publicada (KB) a un bot es tan sencillo como cambiar la configuración de la aplicación de bot. 

> [!Note] 
> Este tema es para la versión 3 del SDK de Bot. Puede encontrar la versión 4 [aquí](https://docs.microsoft.com/azure/bot-service/bot-builder-howto-qna?view=azure-bot-service-4.0&tabs=cs). 

**En este tutorial, aprenderá a:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Crear una instancia de Azure Bot Service con la plantilla de QnA Maker
> * Charlar con el bot para comprobar que el código funciona 
> * Conectar la base de conocimiento publicada al bot
> * Probar el bot con una pregunta

En este artículo, puede usar el [servicio](../how-to/set-up-qnamaker-service-azure.md) de QnA Maker gratuito.

## <a name="prerequisites"></a>Requisitos previos

Para poder realizar este tutorial, debe tener una base de conocimiento publicada. Si no tiene una, siga los pasos que se describen en [Crear una base de conocimiento](../How-To/create-knowledge-base.md) para crear un servicio de QnA Maker con preguntas y respuestas.

## <a name="create-a-qna-bot"></a>Crear un bot de QnA

1. En Azure Portal, haga clic en **Crear un recurso**.

    ![Creación del servicio de bot](../media/qnamaker-tutorials-create-bot/bot-service-creation.png)

2. En el cuadro de búsqueda, busque **Bot de aplicación web**.

    ![Selección del servicio de bot](../media/qnamaker-tutorials-create-bot/bot-service-selection.png)

3. En **Bot Service**, proporcione la información necesaria:

    - Establezca **Nombre de la aplicación** en el nombre del bot. El nombre se usa como el subdominio cuando el bot se implementa en la nube (por ejemplo, mynotesbot.azurewebsites.net).
    - Seleccione la suscripción, el grupo de recursos, el plan de App Service y la ubicación.

4. Para usar las plantillas de v3, seleccione la versión del SDK de **SDK v3** y el idioma del SDK de **C#** o **Node.js**.

    ![configuración de sdk de bot](../media/qnamaker-tutorials-create-bot/bot-v3.png)

5. Seleccione la plantilla **Preguntas y respuestas** para el campo de plantilla de bot y, a continuación, guarde la configuración de la plantilla con **Seleccionar**.

    ![guardar la selección de plantilla de Bot Service](../media/qnamaker-tutorials-create-bot/bot-v3-template.png)

6. Revise la configuración y seleccione **Crear**. Esta acción crea el servicio de bot con Azure y lo implementa en esta plataforma.

    ![crear bot](../media/qnamaker-tutorials-create-bot/bot-blade-settings-v3.png)

7. Confirme que se ha implementado el servicio de bots.

    - Haga clic en **Notificaciones** (el icono de la campana que se encuentra en el borde superior de Azure Portal). La notificación cambiará de **Implementación iniciada** a **Implementación correcta**.
    - Después de que la notificación cambie a **Implementación correcta**, seleccione **Ir al recurso** en esa notificación.

## <a name="chat-with-the-bot"></a>Chat con el bot

Al seleccionar **Ir al recurso** se accede a la hoja de recursos del bot.

Seleccione **Test in Web Chat** (Probar en Chat en web) para abrir el panel Web Chat (Chat en web). Escriba "hi" en Web Chat (Chat en web).

![Chat en web del bot de QnA](../media/qnamaker-tutorials-create-bot/qna-bot-web-chat.PNG)

El bot responde con "Establezca QnAKnowledgebaseId y QnASubscriptionKey en la configuración de la aplicación. Esta respuesta confirma que el bot de QnA ha recibido el mensaje, pero todavía no hay ninguna base de conocimiento de QnA Maker asociada. 

## <a name="connect-your-qna-maker-knowledge-base-to-the-bot"></a>Conectar la base de conocimiento de QnA Maker con el bot

1. Abra **Configuración de la aplicación** y modifique los campos **QnAKnowledgebaseId**, **QnAAuthKey** y **QnAEndpointHostName** para incluir los valores de la base de conocimiento de QnA Maker.

    ![configuración de aplicaciones](../media/qnamaker-tutorials-create-bot/application-settings.PNG)

1. Obtenga el identificador de la base de conocimiento, la dirección URL del host y la clave de punto de conexión de la pestaña de configuración de la base de conocimiento del portal de QnA Maker.

    - Inicio de sesión en [QnA Maker](https://qnamaker.ai)
    - Vaya a la base de conocimiento.
    - Seleccione la pestaña **Settings** (Configuración).
    - **Publique** la base de conocimiento, si todavía no lo ha hecho.

    ![Valores de QnA Maker](../media/qnamaker-tutorials-create-bot/qnamaker-settings-kbid-key.PNG)

## <a name="test-the-bot"></a>Probar el bot

En Azure Portal, haga clic en **Test in Web Chat** (Probar en Chat en web) para probar el bot. 

![Bot de QnA Maker](../media/qnamaker-tutorials-create-bot/qna-bot-web-chat-response.PNG)

El bot de QnA responde desde la base de conocimiento.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando haya terminado con el bot de este tutorial, quítelo en Azure Portal. Los servicios de bot incluyen:

* El plan de App Service
* El servicio Search
* El servicio Cognitive
* El servicio App
* Si lo desea, también puede incluir el servicio Application Insights y el almacenamiento para los datos de Application Insights.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Concepto: base de conocimiento](../concepts/knowledge-base.md)

## <a name="see-also"></a>Otras referencias

- [Administración de la base de conocimiento](https://qnamaker.ai)
- [Habilitación del bot en otros canales](https://docs.microsoft.com/azure/bot-service/bot-service-manage-channels)
