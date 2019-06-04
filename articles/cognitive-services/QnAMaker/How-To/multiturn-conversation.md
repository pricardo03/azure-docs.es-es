---
title: Activar varias conversaciones
titleSuffix: Azure Cognitive Services
description: Usar mensajes y el contexto para administrar las múltiples vueltas, conocidas como múltiples a su vez, para el bot de una pregunta a otro. Activar varias es la capacidad que tiene una copia de perspectivas que influye en el contexto de la pregunta anterior la siguiente pregunta y respuesta de conversación.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.topic: article
ms.date: 05/20/2019
ms.author: diberry
ms.openlocfilehash: 3ca166b287858b3e42aeda1421d1733fe24c81ab
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66479653"
---
# <a name="use-follow-up-prompts-to-create-multiple-turns-of-a-conversation"></a>Seguimiento de uso le insta a crear varios intentos de una conversación

Usar mensajes de seguimiento y el contexto para administrar las múltiples vueltas, conocidas como _activar varios_, para el bot de una pregunta a otro.

Vea el siguiente vídeo de demostración para ver cómo se hace.

[![](../media/conversational-context/youtube-video.png)](https://aka.ms/multiturnexample).

## <a name="what-is-a-multi-turn-conversation"></a>¿Qué es una conversación de varios a su vez?

No se pueden responder algunas preguntas en un único giro. Al diseñar sus conversaciones (bot de chat) de la aplicación de cliente, un usuario puede formular una pregunta que se debe filtrar o refinado para determinar la respuesta correcta. Este flujo a través de las preguntas es posible mediante la presentación del usuario con **solicita seguimiento**.

Cuando el usuario solicita la pregunta, QnA Maker devuelve la respuesta _y_ los mensajes de seguimiento. Esto le permite presentar la preguntas como las opciones de seguimiento. 

## <a name="example-multi-turn-conversation-with-chat-bot"></a>Conversación de varios a su vez de ejemplo con bot de chat

Un bot de chat administra la conversación con el usuario pregunta por pregunta, para determinar la respuesta final.

![Dentro del flujo de conversación, administrar el estado de la conversación en un sistema de cuadro de diálogo activar varios proporcionando mensajes en las respuestas que se presentan como opciones para continuar la conversación.](../media/conversational-context/conversation-in-bot.png)

En la imagen anterior, el usuario especificó `My account`. La base de conocimiento tiene 3 parejas QnA vinculados. El usuario debe seleccionar una de las tres opciones para refinar la respuesta. En la base de conocimiento, la pregunta (1), tiene tres mensajes de seguimiento, presentados en el bot de chat como tres opciones (2). 

Cuando el usuario selecciona una opción (3), se presenta la siguiente lista de refinar opciones (4). Esto puede ocurrir (5) hasta que se determina la respuesta correcta y final (6).

La imagen anterior tiene **habilitar múltiples a su vez** seleccionado con el fin de mostrar mensajes. 

### <a name="using-multi-turn-in-a-bot"></a>Uso de múltiples a su vez en un bot

Deberá cambiar la aplicación cliente para administrar la conversación contextual. Deberá agregar [código al bot](https://github.com/microsoft/BotBuilder-Samples/tree/master/experimental/qnamaker-prompting) para ver los mensajes.  

## <a name="create-a-multi-turn-conversation-from-a-documents-structure"></a>Creación de una conversación de varios a su vez desde la estructura de un documento

Cuando se crea una base de conocimiento, verá una casilla de verificación opcional para habilitar la extracción de varios a su vez. 

![Cuando se crea una base de conocimiento, verá una casilla de verificación opcional para habilitar la extracción de varios a su vez.](../media/conversational-context/enable-multi-turn.png)

Si selecciona esta opción, al importar un documento, la conversación múltiples a su vez puede implicarse desde la estructura. Si no existe esa estructura, QnA Maker crea automáticamente el seguimiento pares de QnA de símbolo del sistema. 

Activar varios estructura solo se puede inferir de DOCX, PDF o direcciones URL de archivos. 

La siguiente imagen de un Microsoft Surface [archivo PDF](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/product-manual.pdf) está pensado para usarse como un manual. Debido al tamaño de este archivo PDF, el recurso de Azure QnA Maker requiere la búsqueda de tarifa de B (15 índices) o superior. 

![! [Si importa un documento, conversación contextual puede ser implícita de la estructura. Si no existe esa estructura, QnA Maker crea automáticamente el seguimiento pares de QnA de símbolo del sistema, como parte de la importación del documento.] (.. / media/conversational-context/import-file-with-conversational-structure.png)](../media/conversational-context/import-file-with-conversational-structure.png#lightbox)

Al importar el documento PDF, QnA Maker determina los mensajes de seguimiento de la estructura para crear un flujo de conversación. 

1. En **paso 1**, seleccione **crear una base de conocimiento** desde el panel de navegación superior.
1. En **paso 2**, cree o use un servicio QnA existente. Asegúrese de usar un servicio QnA con un servicio de búsqueda de B (15 índices) o superior porque el archivo PDF de superficie Manual es demasiado grande para un nivel más pequeño.
1. En **paso 3**, escriba un nombre para la base de conocimiento, como `Surface manual`.
1. En **paso 4**, seleccione **habilitar extracción activar varias de las direcciones URL, archivos .pdf o. docx.** Seleccione la dirección URL de la superficie Manual

    ```text
    https://github.com/Azure-Samples/cognitive-services-sample-data-files/raw/master/qna-maker/data-source-formats/product-manual.pdf
    ```

1. Seleccione el **crear su KB** botón. 

    Una vez creado el conocimiento, se muestra una vista de los pares de preguntas y respuestas.

## <a name="show-questions-and-answers-with-context"></a>Mostrar las preguntas y respuestas con contexto

Reduzca los pares de preguntas y respuestas muestra sólo a aquellos con las conversaciones contextuales. 

1. Seleccione **ver opciones**, a continuación, seleccione **Show contexto (versión preliminar)** . La lista muestra los pares de preguntas y respuestas que contiene mensajes de seguimiento. 

    ![Filtrar la pregunta y responda a pares por las conversaciones contextuales](../media/conversational-context/filter-question-and-answers-by-context.png)

2. El contexto de varios a su vez se muestra en la primera columna.

    ![! [Al importar el documento PDF, QnA Maker determina los mensajes de seguimiento de la estructura para crear un flujo de conversación. ](../media/conversational-context/surface-manual-pdf-follow-up-prompt.png)](../media/conversational-context/surface-manual-pdf-follow-up-prompt.png#lightbox)

    En la imagen anterior, #1 indica el texto en negrita en la columna, lo que significa la pregunta actual. La cuestión principal es el elemento superior de la fila. Las siguientes preguntas están vinculados los pares de preguntas y respuestas. Estos elementos son seleccionables, por lo que puede pasar inmediatamente a los demás elementos de contexto. 

## <a name="add-existing-qna-pair-as-follow-up-prompt"></a>Agregar par de QnA existente como símbolo del sistema de seguimiento

La pregunta original de `My account` tiene mensajes de seguimiento como `Accounts and signing in`. 

![La pregunta original de "Mi cuenta" devuelve correctamente el "Cuentas e iniciar sesión" responder y ya ha vinculado los mensajes de seguimiento.](../media/conversational-context/detected-and-linked-follow-up-prompts.png)

Agregar un símbolo del sistema de seguimiento a un par de QnA existente que no está vinculado actualmente. Dado que la pregunta no está vinculada a cualquier par de QnA, debe cambiar la configuración de la vista actual.

1. Para vincular un par de QnA existente como un símbolo del sistema de seguimiento, seleccione la fila para el par de preguntas y respuestas. El manual de la superficie, busque `Sign out` para reducir la lista.
1. En la fila de `Signout`, seleccione **símbolo del sistema de agregar seguimiento** desde el **respuesta** columna.
1. En el **símbolo del sistema de seguimiento (versión preliminar)** ventana emergente, escriba lo siguiente:

    |Campo|Valor|
    |--|--|
    |Display text|`Turn off the device`. Esto es texto personalizado que elija para mostrar en el símbolo del sistema de seguimiento.|
    |Solo contexto|Seleccionado. Esta respuesta solo se devolverá si la pregunta especifica el contexto.|
    |Vínculo a la respuesta|Escriba `Use the sign-in screen` para encontrar el par de QnA existente.|


1.  Se devuelve una coincidencia. Seleccione esta respuesta como el seguimiento y luego seleccione **guardar**. 

    ![Busque el vínculo de la solicitud seguimiento al cuadro de diálogo de respuesta para una respuesta existente, utilizando el texto de la respuesta.](../media/conversational-context/search-follow-up-prompt-for-existing-answer.png)

1. Una vez haya agregado el símbolo del sistema de seguimiento, no olvide seleccionar **guardar y entrenar** en el panel de navegación superior.
  
### <a name="edit-the-display-text"></a>Editar el texto para mostrar 

Cuando se crea un símbolo del sistema de seguimiento, y se selecciona un par de QnA existente como el **vínculo a la respuesta**, puede escribir nuevo **mostrar texto**. Este texto no sustituye a la pregunta existente y no agrega una nueva pregunta alternativa. Es independiente de esos valores. 

1. Para editar el texto para mostrar, buscar y seleccionar la pregunta de la **contexto** campo.
1. En la fila de esa pregunta, seleccione el símbolo del sistema de seguimiento de la columna de la respuesta. 
1. Seleccione el texto para mostrar que desea editar y luego seleccione **editar**.

    ![Seleccione el texto para mostrar que desea editar y luego seleccione Editar.](../media/conversational-context/edit-existing-display-text.png)

1. El **símbolo del sistema de seguimiento** elemento emergente le permite cambiar el texto existente. 
1. Cuando haya terminado editando el texto para mostrar, seleccione **guardar**. 
1. No olvide seleccionar **guardar y entrenar** en el panel de navegación superior.


<!--

## To find best prompt answer, add metadata to follow-up prompts 

If you have several follow-up prompts for a given QnA pair, but you know as the knowledge base manager, that not all prompts should be returned, use metadata to categorize the prompts in the knowledge base, then send the metadata from the client application as part of the GenerateAnswer request.

In the knowledge base, when a question-and-answer pair is linked to follow-up prompts, the metadata filters are applied first, then the follow-ups are returned.

1. For the two follow-up QnA pairs, add metadata to each one:

    |Question|Add metadata|
    |--|--|
    |`Feedback on an QnA Maker service`|"Feature":"all"|
    |`Feedback on an existing feature`|"Feature":"one"|
    
    ![Add metadata to follow-up prompt so it can be filtered in conversation response from service](../media/conversational-context/add-metadata-feature-to-follow-up-prompt.png) 

1. Save and train. 

    When you send the question `Give feedback` with the metadata filter `Feature` with a value of `all`, only the QnA pair with that metadata will be returned. Both QnA pairs are not returned because they both do not match the filter. 

-->

## <a name="add-new-qna-pair-as-follow-up-prompt"></a>Agregar nuevo par de QnA como símbolo del sistema de seguimiento

Agregar un nuevo par de QnA a la base de conocimiento. El par de QnA debe estar vinculado a una pregunta existente como un símbolo del sistema de seguimiento.

1. En la barra de herramientas de la base de conocimiento, busque y seleccione el par de QnA existente para `Accounts and Signing In`. 

1. En el **respuesta** columna para esta pregunta, seleccione **símbolo del sistema de agregar seguimiento**. 
1. El **símbolo del sistema de seguimiento (versión preliminar)** , cree un nuevo mensaje de seguimiento mediante la especificación de los valores siguientes: 

    |Campo de texto|Valor|
    |--|--|
    |**Mostrar texto**|`Create a Windows Account`. Esto es texto personalizado que elija para mostrar en el símbolo del sistema de seguimiento.|
    |**Context-only**|Seleccionado. Esta respuesta solo se devolverá si la pregunta especifica el contexto.|
    |**Vínculo para responder a**|Escriba el texto siguiente como la respuesta:<br>`[Create](https://account.microsoft.com/) a Windows account with a new or existing email account.`<br>Cuando se guarda y entrenar la base de datos, este texto se convertirán en |
    |||

    ![Crear nuevo QnA de símbolo del sistema](../media/conversational-context/create-child-prompt-from-parent.png)


1. Seleccione **crear nuevo** , a continuación, seleccione **guardar**. 

    Esto crea un nuevo par de preguntas y respuestas y había vinculado a la pregunta seleccionada como un símbolo del sistema de seguimiento. El **contexto** columna ambas preguntas, indica una relación de símbolo del sistema de seguimiento. 

1. Cambiar el **ver opciones** a [Mostrar contexto](#show-questions-and-answers-with-context).

    La nueva pregunta muestra cómo se vincula.

    ![Crear un nuevo mensaje de seguimiento ](../media/conversational-context/new-qna-follow-up-prompt.png)

    La pregunta principal muestra la nueva pregunta como uno de sus decisiones.

    ![! [La columna de contexto, para ambas preguntas, indica una relación de símbolo del sistema de seguimiento.] (.. / media/conversational-context/child-prompt-created.png)](../media/conversational-context/child-prompt-created.png#lightbox)

1. Una vez haya agregado el símbolo del sistema de seguimiento, no olvide seleccionar **guardar y entrenar** en el panel de navegación superior.

## <a name="enable-multi-turn-when-testing-follow-up-prompts"></a>Habilitar múltiples a su vez cuando se solicita un seguimiento de pruebas

Cuando prueba la pregunta con seguimiento le pida en el **prueba** panel, seleccione **habilitar múltiples a su vez**y escriba su pregunta. La respuesta incluye los mensajes de seguimiento.

![Cuando se prueba la pregunta en el panel de prueba, la respuesta incluye los mensajes de seguimiento.](../media/conversational-context/test-pane-with-question-having-follow-up-prompts.png)

Si no habilita múltiples a su vez, se devolverá la respuesta, pero no se devuelven mensajes de seguimiento.

## <a name="json-request-to-return-initial-answer-and-follow-up-prompts"></a>Solicitud JSON para devolver una respuesta inicial y mensajes de seguimiento

Utilice el vacío `context` objeto para solicitar la respuesta a la pregunta del usuario e incluir mensajes de seguimiento. 

```JSON
{
  "question": "accounts and signing in",
  "top": 10,
  "userId": "Default",
  "isTest": false,
  "context": {}
}
```

## <a name="json-response-to-return-initial-answer-and-follow-up-prompts"></a>Respuesta JSON para devolver una respuesta inicial y mensajes de seguimiento

La sección anterior solicitó una respuesta y los mensajes de seguimiento a `Accounts and signing in`. La respuesta incluye la información de símbolo del sistema, ubicada en `answers[0].context`, incluya el texto para mostrar al usuario. 

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
                    },
                    {
                        "displayOrder": 1,
                        "qnaId": 17,
                        "qna": null,
                        "displayText": "Use Windows Hello to sign in"
                    },
                    {
                        "displayOrder": 2,
                        "qnaId": 18,
                        "qna": null,
                        "displayText": "Sign out"
                    },
                    {
                        "displayOrder": 0,
                        "qnaId": 79,
                        "qna": null,
                        "displayText": "Create a Windows Account"
                    }
                ]
            }
        },
        {
            "questions": [
                "Sign out"
            ],
            "answer": "**Sign out**\n\nHere's how to sign out: \n\n Go to Start , and right-click your name. Then select Sign out. ",
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

El `prompts` matriz proporciona el texto en el `displayText` propiedad y el `qnaId` de flujo de valor, por lo que puede aparecer estas respuestas como las siguientes opciones mostradas en la conversación, a continuación, enviar el valor seleccionado a QnA Maker en la siguiente solicitud. 

## <a name="json-request-to-return-non-initial-answer-and-follow-up-prompts"></a>Solicitud JSON para devolver la respuesta no inicial y mensajes de seguimiento

Rellenar el `context` objeto va a incluir contexto anterior.

En la siguiente solicitud JSON, la pregunta actual es `Use Windows Hello to sign in` y la pregunta anterior era `Accounts and signing in`. 

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

##  <a name="json-response-to-return-non-initial-answer-and-follow-up-prompts"></a>Respuesta JSON para devolver la respuesta no inicial y mensajes de seguimiento

QnA Maker _GenerateAnswer_ respuesta JSON incluye los mensajes de seguimiento en el `context` propiedad del primer elemento en el `answers` objeto:

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

## <a name="query-the-knowledge-base-with-the-qna-id"></a>Consultar la base de conocimiento con el ID de QnA.

En la respuesta de la pregunta inicial, los mensajes de seguimiento y sus asociados `qnaId` se devuelve. Ahora que tiene el identificador, puede pasar del cuerpo del mensaje seguimiento de solicitud. Si el cuerpo de solicitud contiene el `qnaId`y el objeto de contexto (que contiene las propiedades de QnA anteriores), GenerateAnswer devolverá la misma pregunta por identificador, en lugar de usar el algoritmo de clasificación para encontrar la respuesta por el texto de pregunta. 

## <a name="displaying-prompts-and-sending-context-in-the-client-application"></a>Mostrar mensajes y el envío de contexto en la aplicación cliente 

Ha agregado indicaciones de la base de conocimiento y probado el flujo en el panel de prueba. Ahora deberá usar estos mensajes en la aplicación cliente. Para Bot Framework, los mensajes no iniciará automáticamente que se muestran en las aplicaciones cliente. Puede mostrar los mensajes como botones o las acciones sugeridas como parte de la respuesta a la consulta del usuario en el cliente aplicaciones mediante la inclusión de este [ejemplo Bot Framework](https://aka.ms/qnamakermultiturnsample) en el código. La aplicación cliente debe almacenar el Id. de QnA actual y la consulta del usuario y pasarlos en la [objeto de contexto de la API GenerateAnswer](#json-request-to-return-non-initial-answer-and-follow-up-prompts) en la siguiente consulta de usuario. 

## <a name="display-order-supported-in-api"></a>Orden de visualización compatible con API

El [mostrar texto y el orden de visualización](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/update#promptdto), devuelto en la respuesta JSON, se admite para la edición del [API Update](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/update). 

<!--

FIX - Need to go to parent, then answer column, then edit answer. 

-->

## <a name="next-steps"></a>Pasos siguientes

Más información acerca de las conversaciones contextuales desde el [ejemplo diálogo](https://aka.ms/qnamakermultiturnsample) u obtenga más información [bot conceptual de diseño para las conversaciones de varios a su vez](https://docs.microsoft.com/azure/bot-service/bot-builder-conversations?view=azure-bot-service-4.0).

> [!div class="nextstepaction"]
> [Migrar una base de conocimiento](../Tutorials/migrate-knowledge-base.md)
