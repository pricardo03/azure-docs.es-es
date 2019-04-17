---
title: 'Creación, entrenamiento y publicación de la base de conocimientos: QnA Maker'
titleSuffix: Azure Cognitive Services
description: Puede crear una base de conocimiento (KB) de QnA Maker a partir de contenido propio, como por ejemplo preguntas más frecuentes o manuales de productos. La base de conocimiento de QnA Maker de este ejemplo se crea a partir de una página web sencilla de preguntas más frecuentes sobre la recuperación de claves de BitLocker.
author: diberry
manager: nitinme
services: cognitive-services
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: quickstart
ms.date: 04/05/2019
ms.author: diberry
ms.openlocfilehash: cf6b0cd99254d5b034687598c588ce5ef13dbd53
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59272859"
---
# <a name="create-train-and-publish-your-qna-maker-knowledge-base"></a>Creación, entrenamiento y publicación de la base de conocimiento de QnA Maker

Puede crear una base de conocimiento (KB) de QnA Maker a partir de contenido propio, como por ejemplo preguntas más frecuentes o manuales de productos. La base de conocimiento de QnA Maker de este ejemplo se crea a partir de una página web sencilla de preguntas más frecuentes sobre la recuperación de claves de BitLocker.

## <a name="prerequisite"></a>Requisito previo

> [!div class="checklist"]
> * Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="create-a-qna-maker-knowledge-base"></a>Creación de una base de conocimiento de QnA Maker

1. Inicie sesión en el portal [QnAMaker.ai](https://QnAMaker.ai) con las credenciales de Azure.

1. En el portal de QnA Maker, seleccione **Create a knowledge base** (Crear una base de conocimiento).

   ![Creación de una base de conocimiento](../media/qna-maker-create-kb.png)

1. En la página **Create** (Crear), en el paso 1, haga clic en **Create a QnA service** (Crear un servicio QnA). Se le dirigirá a [Azure Portal](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker) para configurar un servicio QnA Maker en la suscripción. Si se agota el tiempo de espera de Azure Portal, haga clic en **Intentar de nuevo** en el sitio. Después de conectarse, aparecerá el panel de Azure.

1. Después de crear correctamente un servicio QnA Maker en Azure, vuelva a qnamaker.ai/create. Seleccione el servicio QnA en las listas desplegables del paso 2. Si ha creado un servicio QnA nuevo, asegúrese de actualizar la página.

   ![Selección de una base de conocimiento del servicio QnA](../media/qnamaker-quickstart-kb/qnaservice-selection.png)

1. En el paso 3, asigne el nombre **Mi KB de QnA de ejemplo** a la base de conocimiento.

1. Para agregar contenido a la base de conocimiento, seleccione tres tipos de orígenes de datos. En el paso 4, en **Populate your KB** (Rellenar la base de conocimiento), agregue la dirección URL de [Preguntas más frecuentes sobre recuperación de BitLocker](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-overview-and-requirements-faq) en el cuadro **URL**.

   ![Selección de una base de conocimiento del servicio QnA](../media/qnamaker-quickstart-kb/add-datasources.png)

1. En el paso 5, haga clic en **Create your KB** (Crear la base de conocimiento).

1. Mientras se crea la base de conocimiento, aparece una ventana emergente. El proceso de extracción tarda unos minutos en leer la página HTML e identificar las preguntas y respuestas.

1. Una vez que se haya creado correctamente la base de conocimiento, se abre la página **Knowledge base** (Base de conocimiento). En esta página puede modificar el contenido de la base de conocimiento.

## <a name="edit-the-knowledge-base"></a>Edición de la base de conocimiento

1. En el portal de QnA Maker, en la sección de **edición** seleccione **Add QnA pair** (Agregar par de QnA) para agregar una nueva fila a la base de conocimiento. Bajo **Question** (Pregunta), escriba **Hi.** (Hola). Bajo **Answer** (Respuesta), escriba **Hello. Ask me bitlocker questions.** (Hola. Hazme preguntas sobre BitLocker).

    ![Adición de un par de QnA](../media/qnamaker-quickstart-kb/add-qna-pair.png)

1. En la esquina superior derecha, haga clic en **Save and train** (Guardar y entrenar) para guardar las modificaciones y entrenar el modelo de QnA Maker. Las modificaciones no se conservan a menos que se guarden.

## <a name="test-the-knowledge-base"></a>Prueba de la base de conocimiento

1. En la esquina superior derecha del portal de QnA Maker, haga clic en **Test** (Probar) para comprobar que los cambios realizados han surtido efecto. Escriba `hi there` (hola) en el cuadro y seleccione Entrar. Debería ver la respuesta que creó como respuesta.

1. Haga clic en **Inspect** (Inspeccionar) para examinar la respuesta con más detalle. La ventana de prueba se usa para probar los cambios realizados en la base de conocimiento antes de que se publiquen.

    ![Panel de prueba](../media/qnamaker-quickstart-kb/inspect-panel.png)

1. Vuelva a hacer clic en **Test** (Prueba) para cerrar el panel **Test**.

## <a name="publish-the-knowledge-base"></a>Publicación de una base de conocimiento

Cuando se publica una base de conocimiento, el contenido de preguntas y respuestas de la base de conocimiento se mueve desde el índice de prueba a un índice de producción en Azure Search.

![Publicación de un índice de prueba de producción](../media/qnamaker-how-to-publish-kb/publish-prod-test.png)

1. En el portal de QnA Maker, en el menú situado junto a **Edit** (Editar), seleccione **Publish** (Publicar). Después, para confirmar, haga clic en **Publish** (Publicar) en la página.

1. El servicio QnA Maker se publica correctamente. Puede usar el punto de conexión del código de la aplicación o el bot.

    ![Publicar](../media/qnamaker-quickstart-kb/publish-sucess.png)

## <a name="create-a-bot"></a>Creación de un bot

La creación de un bot siempre está disponible después de publicar mediante la página **Publish** (Publicar). 

* Puede crear varios bots rápidamente, todos ellos apuntando a la misma base de conocimiento para diferentes regiones o planes de precios para los bots individuales. 
* Si desea solo un bot para la base de conocimiento, utilice el vínculo **View all your bots on the Azure portal** (Ver todos los bots en Azure Portal) para ver una lista de los bots actuales. 
* Si realiza cambios en la base de conocimiento y vuelve a publicarla, no es necesario realizar ninguna acción más con el bot. Ya está configurado para funcionar con la base de conocimiento y funcionará con todos los cambios futuros de la base de conocimiento. Cada vez que publique una base de conocimiento, todos los bots conectados a ella se actualizarán automáticamente.

1. En el portal de QnA Maker, en la página **Publish** (Publicar), seleccione **Create bot** (Crear bot). Este botón solo aparece cuando se publica la base de conocimiento.

    ![En el portal de QnA Maker, vaya a la página Publish (Publicar) y publique la base de conocimiento. Seleccione Create Bot (Crear bot).](../media/qnamaker-create-publish-knowledge-base/create-bot-from-published-knowledge-base-page.png)

1. Se abre una nueva pestaña del explorador para Azure Portal, con la página de creación de Azure Bot Service. Configure Azure Bot Service. Para más información sobre estas opciones de configuración, consulte el tutorial [Create a QnA Bot with Azure Bot Service v4](../tutorials/create-qna-bot.md) (Creación de un bot de QnA con Azure Bot Service v4).
    
    * No cambie las siguientes opciones en Azure Portal al crear el bot. Se rellenan automáticamente para la base de conocimiento existente: 
        * Clave de autenticación de QnA
        * Plan de App Service/ubicación
        * Azure Storage
    * El bot y QnA Maker pueden compartir el _plan_ del servicio de aplicación web pero no se puede compartir la aplicación web. Esto significa que el **nombre de la aplicación** debe ser diferente del nombre de la aplicación que utilizó cuando creó el servicio QnA Maker. 


## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Creación de una base de conocimientos](../How-To/create-knowledge-base.md)
