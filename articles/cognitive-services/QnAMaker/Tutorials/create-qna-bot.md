---
title: 'Bot de preguntas y respuestas con Azure Bot Service: QnA Maker'
titleSuffix: Azure Cognitive Services
description: Crear un bot de chat de QnA a partir de la página de publicación para una base de conocimiento existente. Este bot usa el v4 de Bot Framework SDK. No es necesario escribir ningún código para generar el bot, todo el código se proporciona automáticamente.
services: cognitive-services
author: tulasim88
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 04/08/2019
ms.author: tulasim
ms.openlocfilehash: 85b0004288a06a834b61f6e3d50017d35d66ce86
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60599124"
---
# <a name="tutorial-create-a-qna-bot-with-azure-bot-service-v4"></a>Tutorial: Creación de un QnA Bot con Azure Bot Service v4

Creación de un bot de chat de QnA desde el **publicar** página para una base de conocimiento existente. Este bot usa el v4 de Bot Framework SDK. No es necesario escribir ningún código para generar el bot, todo el código se proporciona automáticamente.

**En este tutorial, aprenderá a:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Crear un Azure Bot Service desde una base de conocimiento existente
> * Charlar con el bot para comprobar que el código funciona 

## <a name="prerequisites"></a>Requisitos previos

Para poder realizar este tutorial, debe tener una base de conocimiento publicada. Si no tiene uno, siga los pasos descritos en [Create y respuesta en KB](create-publish-query-in-portal.md) tutorial para crear una base de conocimiento de QnA Maker con preguntas y respuestas.

<a name="create-a-knowledge-base-bot"></a>

## <a name="create-a-qna-bot"></a>Crear un bot de QnA

Creación de un bot como una aplicación cliente para la base de conocimiento. 

1. En el portal de QnA Maker, vaya a la **publicar** página y publicar la base de conocimiento. Seleccione **crear bots**. 

    ![En el portal de QnA Maker, vaya a la página Publish (Publicar) y publique la base de conocimiento. Seleccione Create Bot (Crear bot).](../media/qnamaker-tutorials-create-bot/create-bot-from-published-knowledge-base-page.png)

    Se abre el portal de Azure con la configuración de creación de bot.

1.  Especifique la configuración para crear el bot:

    |Configuración|Value|Propósito|
    |--|--|--|
    |Nombre del bot|`my-tutorial-kb-bot`|Este es el nombre de recurso de Azure para el bot.|
    |Subscription|Consulte propósito.|Seleccione la misma suscripción que utilizó para crear los recursos de QnA Maker.|
    |Grupos de recursos|`my-tutorial-rg`|El grupo de recursos que se usa para todos los recursos Azure relacionados con el bot.|
    |Location|`west us`|Ubicación de recursos de Azure del bot.|
    |Plan de tarifa|`F0`|El nivel gratis para el servicio de Azure bot.|
    |Nombre de la aplicación|`my-tutorial-kb-bot-app`|Se trata de una aplicación web para admitir sólo el bot. No debe el mismo nombre de aplicación como el servicio QnA Maker ya está usando. No se admite el uso compartido de la aplicación web de QnA Maker con cualquier otro recurso.|
    |Idioma SDK|C#|Este es el lenguaje de programación subyacente utilizado por el SDK de framework bot. Las opciones son C# o Node.js.|
    |Clave de autenticación de QnA|**No cambie**|Este valor se rellena automáticamente.|
    |Plan de App Service/ubicación|**No cambie**|Para este tutorial, la ubicación no es importante.|
    |Azure Storage|**No cambie**|Datos de la conversación se almacenan en tablas de Azure Storage.|
    |Application Insights|**No cambie**|El registro se envía a Application Insights.|
    |Microsoft App ID (Id. de aplicación de Microsoft)|**No cambie**|Se requiere contraseña y usuario de active directory.|

    ![Cree el bot de base de conocimiento con esta configuración.](../media/qnamaker-tutorials-create-bot/create-bot-from-published-knowledge-base.png)

    Espere unos minutos hasta que la notificación de proceso de creación de bot un resultado satisfactorio.

<a name="test-the-bot"></a>

## <a name="chat-with-the-bot"></a>Chat con el bot

1. En el portal de Azure, abra el nuevo recurso de bot desde la notificación. 

    ![En el portal de Azure, abra el nuevo recurso de bot desde la notificación.](../media/qnamaker-tutorials-create-bot/azure-portal-notifications.png)

1. Desde **administración Bot**, seleccione **pruebas en Web Chat** y escriba: `How large can my KB be?`. El bot responderá con: 


    `The size of the knowledge base depends on the SKU of Azure search you choose when creating the QnA Maker service. Read [here](https://docs.microsoft.com/azure/cognitive-services/qnamaker/tutorials/choosing-capacity-qnamaker-deployment)for more details.`


    ![Probar el nuevo bot de knowledge base.](../media/qnamaker-tutorial-create-publish-query-in-portal/test-bot-in-web-chat-in-azure-portal.png)

    Para obtener más información acerca de los Bots de Azure, consulte [Use QnA Maker para responder a preguntas](https://docs.microsoft.com/azure/bot-service/bot-builder-howto-qna?view=azure-bot-service-4.0&tabs=cs)

## <a name="related-to-qna-maker-bots"></a>Relacionados con los bots de QnA Maker

* El bot de Ayuda de QnA Maker, utilizado en el portal de QnA Maker, está disponible como un [ejemplo bot](https://github.com/Microsoft/BotBuilder-Samples/tree/master/experimental/csharp_dotnetcore/qnamaker-support-bot).
    ![Icono de bot de Ayuda de QnA Maker es rojo robot](../media/qnamaker-tutorials-create-bot/answer-bot-icon.PNG)
* [Asistencia sanitaria bots](https://docs.microsoft.com/HealthBot/qna_model_howto) usar QnA Maker como uno de sus [modelos de lenguaje](https://docs.microsoft.com/HealthBot/qna_model_howto).

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando haya terminado con el bot de este tutorial, quítelo en Azure Portal. 

Si ha creado un nuevo grupo de recursos para los recursos del bot, elimine el grupo de recursos. 

Si no crea un nuevo grupo de recursos, deberá buscar los recursos asociados con el bot. La manera más fácil es buscar por nombre de la aplicación de bot y bot. Los recursos de bot incluyen:

* El plan de App Service
* El servicio Search
* El servicio Cognitive
* El servicio App
* Si lo desea, también puede incluir el servicio Application Insights y el almacenamiento para los datos de Application Insights.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Concepto: base de conocimiento](../concepts/knowledge-base.md)

## <a name="see-also"></a>Vea también

- [Administración de la base de conocimiento](https://qnamaker.ai)
- [Habilitación del bot en otros canales](https://docs.microsoft.com/azure/bot-service/bot-service-manage-channels)
