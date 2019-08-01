---
title: 'Bot de preguntas y respuestas con Azure Bot Service: QnA Maker'
titleSuffix: Azure Cognitive Services
description: Cree un bot de chat de preguntas y respuestas desde la página Publicar para una base de conocimiento existente. Este bot usa el SDK de Bot Framework v4. No es necesario escribir ningún código para generar el bot, todo el código se proporciona automáticamente.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: tutorial
ms.date: 07/31/2019
ms.author: diberry
ms.openlocfilehash: 4bb987a5a091871bec2c0cc8cec6d9ab804bb244
ms.sourcegitcommit: 800f961318021ce920ecd423ff427e69cbe43a54
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/31/2019
ms.locfileid: "68697991"
---
# <a name="tutorial-create-a-qna-bot-with-azure-bot-service-v4"></a>Tutorial: Creación de un bot de preguntas y respuestas con Azure Bot Service v4

Cree un bot de chat de preguntas y respuestas desde la página **Publicar** para una base de conocimiento existente. Este bot usa el SDK de Bot Framework v4. No es necesario escribir ningún código para generar el bot, todo el código se proporciona automáticamente.

**En este tutorial, aprenderá a:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Creación de una instancia de Azure Bot Service desde una base de conocimiento existente
> * Charlar con el bot para comprobar que el código funciona 

## <a name="prerequisites"></a>Requisitos previos

Para poder realizar este tutorial, debe tener una base de conocimiento publicada. Si no tiene ninguna, siga los pasos que se describen en [Tutorial: En el portal de QnA Maker, creación de una base de conocimiento](create-publish-query-in-portal.md) para crear una base de conocimiento de QnA Maker con preguntas y respuestas.

<a name="create-a-knowledge-base-bot"></a>

## <a name="create-a-qna-bot"></a>Crear un bot de QnA

Cree un bot como una aplicación cliente para la base de conocimiento. 

1. En el portal de QnA Maker, vaya a la página **Publish** (Publicar) y publique la base de conocimiento. Seleccione **Create Bot** (Crear bot). 

    ![En el portal de QnA Maker, vaya a la página Publish (Publicar) y publique la base de conocimiento. Seleccione Create Bot (Crear bot).](../media/qnamaker-tutorials-create-bot/create-bot-from-published-knowledge-base-page.png)

    Se abre Azure Portal con la configuración de la creación del bot.

1.  Especifique la configuración para crear el bot:

    |Configuración|Valor|Propósito|
    |--|--|--|
    |Nombre del bot|`my-tutorial-kb-bot`|Este es el nombre de recurso de Azure para el bot.|
    |Subscription|Vea el propósito.|Seleccione la misma suscripción que utilizó para crear los recursos de QnA Maker.|
    |Resource group|`my-tutorial-rg`|El grupo de recursos que se usa para todos los recursos Azure relacionados con el bot.|
    |Location|`west us`|Ubicación de los recursos de Azure del bot.|
    |Plan de tarifa|`F0`|El nivel de servicio gratuito para Azure Bot Service.|
    |Nombre de la aplicación|`my-tutorial-kb-bot-app`|Se trata de una aplicación web para admitir solo el bot. No debe ser el mismo nombre de aplicación que el del servicio QnA Maker que ya esté usando. No se admite el uso compartido de la aplicación web de QnA Maker con cualquier otro recurso.|
    |Lenguaje de SDK|C#|Este es el lenguaje de programación subyacente utilizado por el SDK del marco de bot. Las opciones son [C#](https://github.com/Microsoft/botbuilder-dotnet) o [Node.js](https://github.com/Microsoft/botbuilder-js).|
    |Clave de autenticación de QnA|**No cambiar**|Este valor se rellena automáticamente.|
    |Plan de App Service/ubicación|**No cambiar**|En este tutorial, la ubicación no es importante.|
    |Azure Storage|**No cambiar**|Los datos de la conversación se almacenan en tablas de Azure Storage.|
    |Application Insights|**No cambiar**|El registro se envía a Application Insights.|
    |Microsoft App ID (Id. de aplicación de Microsoft)|**No cambiar**|Se requiere el usuario y la contraseña de Active Directory.|

    ![Cree el bot de la base de conocimiento con esta configuración.](../media/qnamaker-tutorials-create-bot/create-bot-from-published-knowledge-base.png)

    Espere unos minutos hasta que la notificación del proceso de creación de bot indique un resultado satisfactorio.

<a name="test-the-bot"></a>

## <a name="chat-with-the-bot"></a>Chat con el bot

1. En Azure Portal, abra el nuevo recurso de bot desde la notificación. 

    ![En Azure Portal, abra el nuevo recurso de bot desde la notificación.](../media/qnamaker-tutorials-create-bot/azure-portal-notifications.png)

1. En **Bot management** (Administración del bot), seleccione **Test in Web Chat** (Probar en Chat en web) y escriba `How large can my KB be?`. El bot responderá con: 


    `The size of the knowledge base depends on the SKU of Azure search you choose when creating the QnA Maker service. Read [here](https://docs.microsoft.com/azure/cognitive-services/qnamaker/tutorials/choosing-capacity-qnamaker-deployment)for more details.`


    ![Pruebe el nuevo bot de la base de conocimiento.](../media/qnamaker-tutorial-create-publish-query-in-portal/test-bot-in-web-chat-in-azure-portal.png)

    Para más información acerca de Azure Bots, consulte [Uso de QnA Maker para responder preguntas](https://docs.microsoft.com/azure/bot-service/bot-builder-howto-qna?view=azure-bot-service-4.0&tabs=cs)

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando haya terminado con el bot de este tutorial, quítelo en Azure Portal. 

Si ha creado un nuevo grupo de recursos para los recursos del bot, elimínelo. 

Si no creó un grupo de recursos, deberá buscar los recursos asociados con el bot. La manera más fácil es buscar por el nombre del bot y de la aplicación de bot. Entre los recursos de bot se incluyen:

* El plan de App Service
* El servicio Search
* El servicio Cognitive
* El servicio App
* Si lo desea, también puede incluir el servicio Application Insights y el almacenamiento para los datos de Application Insights.


## <a name="related-to-qna-maker-bots"></a>Relacionado con los bots de QnA Maker

* El bot de Ayuda de QnA Maker, utilizado en el portal de QnA Maker, está disponible como una [muestra de bot](https://github.com/microsoft/BotBuilder-Samples/tree/master/experimental/qnamaker-support).
    ![El icono del bot de Ayuda de QnA Maker es un robot rojo](../media/qnamaker-tutorials-create-bot/answer-bot-icon.PNG)
* [Los bots para asistencia sanitaria](https://docs.microsoft.com/HealthBot/qna_model_howto) usan QnA Maker como uno de sus [modelos de idioma](https://docs.microsoft.com/HealthBot/qna_model_howto).


[!INCLUDE [Bot Information](../../../../includes/cognitive-services-qnamaker-luis-bot-info.md)]

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Concepto: base de conocimiento](../concepts/knowledge-base.md)

## <a name="see-also"></a>Otras referencias

- [Administración de la base de conocimiento](https://qnamaker.ai)
- [Habilitación del bot en otros canales](https://docs.microsoft.com/azure/bot-service/bot-service-manage-channels)
