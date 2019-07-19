---
title: Conversaciones multiturno
titleSuffix: Azure Cognitive Services
description: Use solicitudes y contexto para administrar los múltiples turnos del bot, lo que se conoce como multiturno, de una pregunta a otra. Multiturno es la capacidad de mantener una conversación fluida en la que el contexto de la pregunta anterior influye en la siguiente pregunta y en su respuesta.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 06/26/2019
ms.author: diberry
ms.openlocfilehash: 10249375922b47a40f71a60938cdd12ffe0f9b54
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2019
ms.locfileid: "67508144"
---
# <a name="use-follow-up-prompts-to-create-multiple-turns-of-a-conversation"></a>Uso de avisos de seguimiento para crear múltiples turnos de una conversación

Use avisos de seguimiento y contexto para administrar los múltiples turnos del bot, lo que se conoce como _multiturno_, de una pregunta a otra.

Para ver cómo funciona el multiturno, vea el siguiente vídeo de demostración:

[![Multi-turn conversation in QnA Maker](../media/conversational-context/youtube-video.png)](https://aka.ms/multiturnexample) (Conversaciones multiturno en QnA Maker)

## <a name="what-is-a-multi-turn-conversation"></a>¿Qué es una conversación multiturno?

Algunas preguntas no se pueden responder en un único turno. Cuando diseña conversaciones de la aplicación cliente (bot de chat), el usuario puede formular una pregunta que debe filtrarse o mejorarse para determinar la respuesta correcta. Puede crear este flujo de las preguntas posibles presentando al usuario *avisos de seguimiento*.

Cuando un usuario realiza una pregunta, QnA Maker devuelve la respuesta _y_ los avisos de seguimiento. Esta respuesta le permite presentar las opciones de seguimiento como posibles respuestas. 

## <a name="example-multi-turn-conversation-with-chat-bot"></a>Ejemplo de conversación multiturno con bot de chat

Con multiturno, un bot de chat administra una conversación con un usuario para determinar la respuesta final como se indica en la imagen siguiente:

![Un diálogo multiturno con preguntas que guían a un usuario por una conversación](../media/conversational-context/conversation-in-bot.png)

En la imagen anterior, un usuario ha iniciado una conversación escribiendo **Mi cuenta**. La base de conocimiento tiene tres pares de preguntas y respuestas vinculados. Para mejorar la respuesta, el usuario selecciona una de las tres opciones en la base de conocimiento. La pregunta (1), incluye tres avisos de seguimiento, que se presentan en el bot de chat como tres posibles opciones (2). 

Cuando el usuario selecciona una opción (3), se presenta la siguiente lista de opciones de mejora (4). Esta secuencia continúa (5) hasta que el usuario determina la respuesta correcta, final (6).

> [!NOTE]
> En la imagen anterior, se ha seleccionado la casilla **Enable multi-turn** (Habilitar multiturno) para asegurarse de que se muestran las preguntas de seguimiento. 

### <a name="use-multi-turn-in-a-bot"></a>Uso del multiturno en un bot

Para administrar la conversación contextual, cambie la aplicación cliente mediante la [adición de código al bot](https://github.com/microsoft/BotBuilder-Samples/tree/master/experimental/qnamaker-prompting). La adición de código permite a los usuarios ver las preguntas.  

## <a name="create-a-multi-turn-conversation-from-a-documents-structure"></a>Creación de una conversación multiturno a partir de la estructura de un documento

Cuando crea una base de conocimiento, la sección **Populate your KB** (Rellenar la base de conocimiento) muestra la casilla **Enable multi-turn extraction from URLs, .pdf or .docx files** (Habilitar extracción multiturno a partir de URL o de archivos .pdf o .docx). 

![Casilla para habilitar la extracción multiturno](../media/conversational-context/enable-multi-turn.png)

Cuando se selecciona esta opción para un documento importado, la conversación multiturno se puede suponer a partir de la estructura del documento. Si esa estructura existe, QnA Maker crea la opción de seguimiento que empareja las preguntas y respuestas en su lugar como parte del proceso de importación. 

Solo se puede inferir la estructura multiturno a partir de direcciones URL y de archivos PDF o DOCX. Para obtener un ejemplo de estructura, vea una imagen de un [archivo PDF de manual de usuario de Microsoft Surface](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/product-manual.pdf). Debido al tamaño de este archivo PDF, el recurso de QnA Maker requiere un **plan de tarifa de Search** **B** (15 índices) o superior. 

![![Ejemplo de estructura en un manual de usuario](../media/conversational-context/import-file-with-conversational-structure.png)](../media/conversational-context/import-file-with-conversational-structure.png#lightbox)

Al importar el documento PDF, QnA Maker determina los avisos de seguimiento a partir de la estructura para crear un flujo de conversación. 

1. En QnA Maker, seleccione **Create a knowledge base** (Crear una base de conocimiento).
1. Cree o use un servicio QnA Maker existente. En el ejemplo anterior de Microsoft Surface, como el archivo PDF es demasiado grande para un nivel más pequeño, utilice un servicio QnA Maker con un **servicio de búsqueda** **B** (15 índices) o superior.
1. Escriba un nombre para la base de conocimiento, como **Manual de Surface**.
1. Seleccione la casilla **Enable multi-turn extraction from URLs, .pdf or .docx files** (Habilitar extracción multiturno a partir de URL o de archivos .pdf o .docx). 
1. Seleccione la dirección URL del manual de Surface, **https://github.com/Azure-Samples/cognitive-services-sample-data-files/raw/master/qna-maker/data-source-formats/product-manual.pdf** .

1. Seleccione el botón **Create your KB** (Crear la base de conocimiento). 

    Una vez creada la base de conocimiento, se muestra una vista de los pares de preguntas y respuestas.

## <a name="show-questions-and-answers-with-context"></a>Mostrar preguntas y respuestas con contexto

Reduzca los pares de preguntas y respuestas que aparecen a solo aquellos con conversaciones contextuales. 

Seleccione **Ver opciones** y, a continuación, seleccione **Mostrar contexto (VERSIÓN PRELIMINAR)** . La lista muestra los pares de preguntas y respuestas que contienen avisos de seguimiento. 

![Filtrar pares de preguntas y respuestas por conversaciones contextuales](../media/conversational-context/filter-question-and-answers-by-context.png)

El contexto multiturno se muestra en la primera columna.

![![Columna "Contexto (VERSIÓN PRELIMINAR)"](../media/conversational-context/surface-manual-pdf-follow-up-prompt.png)](../media/conversational-context/surface-manual-pdf-follow-up-prompt.png#lightbox)

En la imagen anterior, **1** indica texto en negrita en la columna, lo cual indica la pregunta actual. La pregunta principal es el elemento superior de la fila. Todas las preguntas por debajo de ella son pares de preguntas y respuestas vinculadas. Estos elementos son seleccionables, por lo que puede pasar inmediatamente a los demás elementos de contexto. 

## <a name="add-an-existing-question-and-answer-pair-as-a-follow-up-prompt"></a>Incorporación de un par de pregunta y respuesta existente como opción de seguimiento

La pregunta original, **Mi cuenta**, tiene avisos de seguimiento, como **Accounts and signing in** (Cuentas e inicio de sesión). 

![Respuesta "Accounts and signing in" (Cuentas e inicio de sesión) y avisos de seguimiento](../media/conversational-context/detected-and-linked-follow-up-prompts.png)

Agregue una opción de seguimiento a un par de pregunta y respuesta que no esté actualmente vinculado. Como la pregunta no está vinculada a ningún par de pregunta y respuesta, se debe cambiar la configuración actual de la vista.

1. Para vincular un par de pregunta y respuesta existente como opción de seguimiento, seleccione la fila del par de pregunta y respuesta. Para el manual de Surface, busque **Sign out** (Cerrar sesión) para reducir la lista.
1. En la fila de **Signout** (Cerrar sesión), en la columna **Respuesta**, seleccione **Add follow-up prompt** (Agregar opción de seguimiento).
1. En los campos de la ventana emergente **Follow-up prompt (PREVIEW)** [Aviso de seguimiento (VERSIÓN PRELIMINAR)], escriba los siguientes valores:

    |Campo|Valor|
    |--|--|
    |Display text|Escriba **Apagar el dispositivo**. Este es el texto personalizado que se mostrará en el aviso de seguimiento.|
    |Context-only (Solo contexto)| Seleccione esta casilla. Solo se devolverá una respuesta si la pregunta especifica contexto.|
    |Link to answer (Vínculo a la respuesta)|Escriba **Usar la pantalla de inicio de sesión** para encontrar el par de pregunta y respuesta existente.|


1.  Se devuelve una coincidencia. Seleccione esta respuesta como seguimiento y, a continuación, seleccione **Guardar**. 

    ![Página "Follow-up prompt (PREVIEW)" [Aviso de seguimiento (VERSIÓN PRELIMINAR)]](../media/conversational-context/search-follow-up-prompt-for-existing-answer.png)

1. Una vez que haya agregado el aviso de seguimiento, seleccione **Save and train** (Guardar y entrenar) en la barra de navegación superior.
  
### <a name="edit-the-display-text"></a>Edición del texto para mostrar 

Cuando se crea un aviso de seguimiento, y se especifica un par de pregunta y respuesta como **vínculo a la respuesta**, puede escribir un nuevo **texto para mostrar**. Este texto no reemplaza la pregunta existente y no agrega una nueva pregunta alternativa. Es independiente de esos valores. 

1. Para editar el texto para mostrar, busque y seleccione la pregunta en el campo **Context** (Contexto).
1. En la fila de esa pregunta, seleccione el aviso de seguimiento de la columna de respuesta. 
1. Seleccione el texto para mostrar que desea editar y, a continuación, seleccione **Editar**.

    ![Comando Editar texto para mostrar](../media/conversational-context/edit-existing-display-text.png)

1. En la ventana emergente **Follow-up prompt** (Aviso de seguimiento), cambie el texto para mostrar existente. 
1. Cuando haya terminado de editar el texto para mostrar, seleccione **Guardar**. 
1. En la barra de navegación superior, seleccione **Save and train** (Guardar y entrenar).


<!--

## To find the best prompt answer, add metadata to follow-up prompts 

If you have several follow-up prompts for a specific question-and-answer pair but you know, as the knowledge base manager, that not all prompts should be returned, use metadata to categorize the prompts in the knowledge base. You can then send the metadata from the client application as part of the GenerateAnswer request.

In the knowledge base, when a question-and-answer pair is linked to follow-up prompts, the metadata filters are applied first, and then the follow-ups are returned.

1. Add metadata to each of the two follow-up question-and-answer pairs:

    |Question|Add metadata|
    |--|--|
    |*Feedback on a QnA Maker service*|"Feature":"all"|
    |*Feedback on an existing feature*|"Feature":"one"|
    
    ![The "Metadata tags" column for adding metadata to a follow-up prompt](../media/conversational-context/add-metadata-feature-to-follow-up-prompt.png) 

1. Select **Save and train**. 

    When you send the question **Give feedback** with the metadata filter **Feature** with a value of **all**, only the question-and-answer pair with that metadata is returned. QnA Maker doesn't return both question-and-answer pairs, because both don't match the filter. 

-->

## <a name="add-a-new-question-and-answer-pair-as-a-follow-up-prompt"></a>Incorporación de un nuevo par de pregunta y respuesta como aviso de seguimiento

Cuando se agrega un nuevo par de pregunta y respuesta a la base de conocimiento, cada par se debe vincular a una pregunta existente como un aviso de seguimiento.

1. En la barra de herramientas de la base de conocimiento, busque y seleccione el par de pregunta y respuesta existente para **Accounts and signing in** (Cuentas e inicio de sesión). 

1. En la columna **Respuesta** seleccione **Add follow-up prompt** (Agregar opción de seguimiento). 
1. En **Follow-up prompt (PREVIEW)** [Aviso de seguimiento (VERSIÓN PRELIMINAR)], cree un nuevo aviso de seguimiento mediante la especificación de los valores siguientes: 

    |Campo|Valor|
    |--|--|
    |Display text|*Create a Windows Account* (Crear una cuenta de Windows). El texto personalizado que se mostrará en el aviso de seguimiento.|
    |Context-only (Solo contexto)|Seleccione esta casilla. Solo se devolverá esta respuesta si la pregunta especifica contexto.|
    |Link to answer (Vínculo a la respuesta)|Escriba el texto siguiente como respuesta:<br>*[Crear](https://account.microsoft.com/) una cuenta de Windows con una cuenta de correo electrónico nueva o existente*.<br>Cuando guarde y entrene la base de datos, este texto se convertirá. |
    |||

    ![Crear una nueva pregunta y respuesta de seguimiento](../media/conversational-context/create-child-prompt-from-parent.png)


1. Seleccione **Crear nuevo** y, a continuación, seleccione **Guardar**. 

    Esta acción crea un nuevo par de pregunta y respuesta, y vincula la pregunta seleccionada como un aviso de seguimiento. La columna **Contexto** de ambas preguntas, indica una relación de aviso de seguimiento. 

1. Seleccione **Ver opciones** y, a continuación, seleccione [**Mostrar contexto (VERSIÓN PRELIMINAR)** ](#show-questions-and-answers-with-context).

    La nueva pregunta muestra cómo se vincula.

    ![Crear un nuevo aviso de seguimiento](../media/conversational-context/new-qna-follow-up-prompt.png)

    La pregunta primaria muestra una nueva pregunta como una de sus opciones.

    ![![La columna Contexto de ambas preguntas, indica una relación de aviso de seguimiento](../media/conversational-context/child-prompt-created.png)](../media/conversational-context/child-prompt-created.png#lightbox)

1. Una vez que haya agregado el aviso de seguimiento, seleccione **Save and train** (Guardar y entrenar) en la barra de navegación superior.

## <a name="enable-multi-turn-during-testing-of-follow-up-prompts"></a>Habilitar el multiturno durante la prueba de los avisos de seguimiento

Cuando pruebe la pregunta con avisos de seguimiento en el panel **Probar**, seleccione **Enable multi-turn** (Habilitar el multiturno) y, a continuación, escriba la pregunta. La respuesta incluye los avisos de seguimiento.

![La respuesta incluye los avisos de seguimiento](../media/conversational-context/test-pane-with-question-having-follow-up-prompts.png)

Si no habilita el multiturno, se devolverá la respuesta, pero no los avisos de seguimiento.

## <a name="a-json-request-to-return-an-initial-answer-and-follow-up-prompts"></a>Una solicitud JSON para devolver una respuesta inicial y avisos de seguimiento

Utilice el objeto `context` vacío para solicitar la respuesta a la pregunta del usuario e incluir avisos de seguimiento. 

```JSON
{
  "question": "accounts and signing in",
  "top": 10,
  "userId": "Default",
  "isTest": false,
  "context": {}
}
```

## <a name="a-json-response-to-return-an-initial-answer-and-follow-up-prompts"></a>Una respuesta JSON para devolver una respuesta inicial y avisos de seguimiento

En la sección anterior se solicitó una respuesta y todos los avisos de seguimiento para **Accounts and signing in** (Cuentas e inicio de sesión). La respuesta incluye la información del aviso, que se encuentra en *answers[0].context* y el texto para mostrar al usuario. 

```JSON
{
    "answers": [
        {
            "questions": [
                "Accounts and signing in"
            ],
            "answer": "**Accounts and signing in**\n\nWhen you set up your Surface, an account is set up for you. You can create additional accounts later for family and friends, so each person using your Surface can set it up just the way he or she likes. For more info, see All about accounts on Surface.com. \n\nThere are several ways to sign in to your Surface Pro 4: ",
            "score": 100.0,
            "id": 15,
            "source": "product-manual.pdf",
            "metadata": [],
            "context": {
                "isContextOnly": true,
                "prompts": [
                    {
                        "displayOrder": 0,
                        "qnaId": 16,
                        "qna": null,
                        "displayText": "Use the sign-in screen"
                    }
                ]
            }
        },
        {
            "questions": [
                "Sign out"
            ],
            "answer": "**Sign out**\n\nHere's how to sign out: \n\n Go to Start, and right-click your name. Then select Sign out. ",
            "score": 38.01,
            "id": 18,
            "source": "product-manual.pdf",
            "metadata": [],
            "context": {
                "isContextOnly": true,
                "prompts": [
                    {
                        "displayOrder": 0,
                        "qnaId": 16,
                        "qna": null,
                        "displayText": "Turn off the device"
                    }
                ]
            }
        },
        {
            "questions": [
                "Use the sign-in screen"
            ],
            "answer": "**Use the sign-in screen**\n\n1.  \n\nTurn on or wake your Surface by pressing the power button. \n\n2.  \n\nSwipe up on the screen or tap a key on the keyboard. \n\n3.  \n\nIf you see your account name and account picture, enter your password and select the right arrow or press Enter on your keyboard. \n\n4.  \n\nIf you see a different account name, select your own account from the list at the left. Then enter your password and select the right arrow or press Enter on your keyboard. ",
            "score": 27.53,
            "id": 16,
            "source": "product-manual.pdf",
            "metadata": [],
            "context": {
                "isContextOnly": true,
                "prompts": []
            }
        }
    ]
}
```

La matriz `prompts` proporciona el texto en la propiedad `displayText` y el valor `qnaId`. Puede mostrar estas respuestas como las siguientes opciones mostradas en el flujo de conversación y, a continuación, enviar el `qnaId` seleccionado a QnA Maker en la siguiente solicitud. 

<!--

The `promptsToDelete` array provides the ...

-->

## <a name="a-json-request-to-return-a-non-initial-answer-and-follow-up-prompts"></a>Una solicitud JSON para devolver una respuesta que no es la inicial y avisos de seguimiento

Rellene el objeto `context` para que incluya el contexto anterior.

En la siguiente solicitud JSON, la pregunta actual es *Use Windows Hello to sign in* (Usar Windows Hello para iniciar sesión) y la pregunta anterior era *Accounts and signing in* (Cuentas e inicio de sesión). 

```JSON
{
  "question": "Use Windows Hello to sign in",
  "top": 10,
  "userId": "Default",
  "isTest": false,
  "qnaId": 17,
  "context": {
    "previousQnAId": 15,
    "previousUserQuery": "accounts and signing in"
  }
}
``` 

##  <a name="a-json-response-to-return-a-non-initial-answer-and-follow-up-prompts"></a>Una respuesta JSON para devolver una respuesta que no es la inicial y avisos de seguimiento

La respuesta JSON _GenerateAnswer_ de QnA Maker incluye los avisos de seguimiento de la propiedad `context` del primer elemento del objeto `answers`:

```JSON
{
    "answers": [
        {
            "questions": [
                "Use Windows Hello to sign in"
            ],
            "answer": "**Use Windows Hello to sign in**\n\nSince Surface Pro 4 has an infrared (IR) camera, you can set up Windows Hello to sign in just by looking at the screen. \n\nIf you have the Surface Pro 4 Type Cover with Fingerprint ID (sold separately), you can set up your Surface sign you in with a touch. \n\nFor more info, see What is Windows Hello? on Windows.com. ",
            "score": 100.0,
            "id": 17,
            "source": "product-manual.pdf",
            "metadata": [],
            "context": {
                "isContextOnly": true,
                "prompts": []
            }
        },
        {
            "questions": [
                "Meet Surface Pro 4"
            ],
            "answer": "**Meet Surface Pro 4**\n\nGet acquainted with the features built in to your Surface Pro 4. \n\nHere’s a quick overview of Surface Pro 4 features: \n\n\n\n\n\n\n\nPower button \n\n\n\n\n\nPress the power button to turn your Surface Pro 4 on. You can also use the power button to put it to sleep and wake it when you’re ready to start working again. \n\n\n\n\n\n\n\nTouchscreen \n\n\n\n\n\nUse the 12.3” display, with its 3:2 aspect ratio and 2736 x 1824 resolution, to watch HD movies, browse the web, and use your favorite apps. \n\nThe new Surface G5 touch processor provides up to twice the touch accuracy of Surface Pro 3 and lets you use your fingers to select items, zoom in, and move things around. For more info, see Surface touchscreen on Surface.com. \n\n\n\n\n\n\n\nSurface Pen \n\n\n\n\n\nEnjoy a natural writing experience with a pen that feels like an actual pen. Use Surface Pen to launch Cortana in Windows or open OneNote and quickly jot down notes or take screenshots. \n\nSee Using Surface Pen (Surface Pro 4 version) on Surface.com for more info. \n\n\n\n\n\n\n\nKickstand \n\n\n\n\n\nFlip out the kickstand and work or play comfortably at your desk, on the couch, or while giving a hands-free presentation. \n\n\n\n\n\n\n\nWi-Fi and Bluetooth® \n\n\n\n\n\nSurface Pro 4 supports standard Wi-Fi protocols (802.11a/b/g/n/ac) and Bluetooth 4.0. Connect to a wireless network and use Bluetooth devices like mice, printers, and headsets. \n\nFor more info, see Add a Bluetooth device and Connect Surface to a wireless network on Surface.com. \n\n\n\n\n\n\n\nCameras \n\n\n\n\n\nSurface Pro 4 has two cameras for taking photos and recording video: an 8-megapixel rear-facing camera with autofocus and a 5-megapixel, high-resolution, front-facing camera. Both cameras record video in 1080p, with a 16:9 aspect ratio. Privacy lights are located on the right side of both cameras. \n\nSurface Pro 4 also has an infrared (IR) face-detection camera so you can sign in to Windows without typing a password. For more info, see Windows Hello on Surface.com. \n\nFor more camera info, see Take photos and videos with Surface and Using autofocus on Surface 3, Surface Pro 4, and Surface Book on Surface.com. \n\n\n\n\n\n\n\nMicrophones \n\n\n\n\n\nSurface Pro 4 has both a front and a back microphone. Use the front microphone for calls and recordings. Its noise-canceling feature is optimized for use with Skype and Cortana. \n\n\n\n\n\n\n\nStereo speakers \n\n\n\n\n\nStereo front speakers provide an immersive music and movie playback experience. To learn more, see Surface sound, volume, and audio accessories on Surface.com. \n\n\n\n\n",
            "score": 21.92,
            "id": 3,
            "source": "product-manual.pdf",
            "metadata": [],
            "context": {
                "isContextOnly": true,
                "prompts": [
                    {
                        "displayOrder": 0,
                        "qnaId": 4,
                        "qna": null,
                        "displayText": "Ports and connectors"
                    }
                ]
            }
        },
        {
            "questions": [
                "Use the sign-in screen"
            ],
            "answer": "**Use the sign-in screen**\n\n1.  \n\nTurn on or wake your Surface by pressing the power button. \n\n2.  \n\nSwipe up on the screen or tap a key on the keyboard. \n\n3.  \n\nIf you see your account name and account picture, enter your password and select the right arrow or press Enter on your keyboard. \n\n4.  \n\nIf you see a different account name, select your own account from the list at the left. Then enter your password and select the right arrow or press Enter on your keyboard. ",
            "score": 19.04,
            "id": 16,
            "source": "product-manual.pdf",
            "metadata": [],
            "context": {
                "isContextOnly": true,
                "prompts": []
            }
        }
    ]
}
```

## <a name="query-the-knowledge-base-with-the-qna-maker-id"></a>Consulta de la base de conocimiento con el identificador de QnA Maker

En la respuesta de la pregunta inicial, se devuelven los avisos de seguimiento y sus identificadores `qnaId` asociados. Ahora que tiene el identificador, puede pasarlo en el cuerpo de la solicitud del aviso de seguimiento. Si el cuerpo de la solicitud contiene el identificador `qnaId` y el objeto de contexto (que contiene las propiedades anteriores de QnA Maker), GenerateAnswer devolverá la pregunta exacta por identificador en lugar de usar el algoritmo de clasificación para encontrar la respuesta por el texto de la pregunta. 

## <a name="display-prompts-and-send-context-in-the-client-application"></a>Mostrar avisos y enviar contexto en la aplicación cliente 

Ha agregado avisos en la base de conocimiento y probado el flujo en el panel de prueba. Ahora deberá usar estos avisos en la aplicación cliente. Para Bot Framework, los avisos no se muestran automáticamente en las aplicaciones cliente. Puede mostrar los avisos como botones o acciones sugeridas como parte de la respuesta a la consulta del usuario en aplicaciones cliente mediante la inclusión de este [ejemplo de Bot Framework](https://aka.ms/qnamakermultiturnsample) en el código. La aplicación cliente almacenará el identificador de QnA Maker actual y la consulta del usuario y los pasará en el [objeto de contexto de GenerateAnswer API](#a-json-request-to-return-a-non-initial-answer-and-follow-up-prompts) para la siguiente consulta de usuario. 

## <a name="display-order-is-supported-in-the-update-api"></a>Update API admite el orden de presentación

El [texto para mostrar y el orden de presentación](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/update#promptdto) que se devuelve en la respuesta JSON, es compatible para su edición con [Update API](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/update). 

<!--

FIX - Need to go to parent, then answer column, then edit answer. 

-->

## <a name="create-knowledge-base-with-multi-turn-prompts-with-the-create-api"></a>Creación de la base de conocimiento con avisos multiturno con Create API

Puede crear una base de conocimiento con avisos multiturno mediante [Create API de QnA Maker](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/create). Los avisos se agregan en la matriz `prompts` de la propiedad `context`. 


## <a name="add-or-delete-multi-turn-prompts-with-the-update-api"></a>Incorporación o eliminación de avisos multiturno con Update API

Puede agregar o eliminar avisos multiturno mediante [Update API de QnA Maker](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/update).  Los avisos se agregan en la matriz `promptsToAdd` y `promptsToDelete` de la propiedad `context`. 


## <a name="next-steps"></a>Pasos siguientes

Más información acerca de las conversaciones contextuales de este [ejemplo de diálogo](https://aka.ms/qnamakermultiturnsample) o más información acerca del [diseño de bots conceptuales para conversaciones multiturno](https://docs.microsoft.com/azure/bot-service/bot-builder-conversations?view=azure-bot-service-4.0).

> [!div class="nextstepaction"]
> [Migración de una base de conocimiento](../Tutorials/migrate-knowledge-base.md)
