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
ms.date: 05/14/2019
ms.author: diberry
ms.openlocfilehash: f12b55e9b00e933e13f84832b8cc36267a1da05f
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2019
ms.locfileid: "65954867"
---
# <a name="use-follow-up-prompts-to-create-multiple-turns-of-a-conversation"></a>Seguimiento de uso le insta a crear varios intentos de una conversación

Usar mensajes de seguimiento y el contexto para administrar las múltiples vueltas, conocidas como _activar varios_, para el bot de una pregunta a otro.

Aprenda de la [vídeo de demostración](https://aka.ms/multiturnexample).

## <a name="what-is-a-multi-turn-conversation"></a>¿Qué es una conversación de varios a su vez?

Algunos tipos de conversación no se puede completar en un único giro. Al diseñar sus conversaciones (bot de chat) de la aplicación de cliente, un usuario puede formular una pregunta que se debe filtrar o refinado para determinar la respuesta correcta. Este flujo a través de las preguntas es posible mediante la presentación del usuario con **solicita seguimiento**.

Cuando el usuario solicita la pregunta, QnA Maker devuelve la respuesta _y_ los mensajes de seguimiento. Esto le permite presentar la preguntas como las opciones de seguimiento. 

## <a name="example-multi-turn-conversation-with-chat-bot"></a>Conversación de varios a su vez de ejemplo con bot de chat

Un bot de chat administra la conversación, pregunta por pregunta por el usuario para determinar la respuesta final.

![Dentro del flujo de conversación, administrar el estado de la conversación en un sistema de cuadro de diálogo activar varios proporcionando mensajes en las respuestas que se presentan como opciones para continuar la conversación.](../media/conversational-context/conversation-in-bot.png)

En la imagen anterior, la pregunta del usuario debe ser refinado antes de devolver la respuesta. En la base de conocimiento, la pregunta (1), tiene cuatro mensajes de seguimiento, presentados en el bot de chat como cuatro opciones (2). 

Cuando el usuario selecciona una opción (3), se presenta la siguiente lista de refinar opciones (4). Esto puede ocurrir (5) hasta que se determina la respuesta correcta y final (6).

Deberá cambiar la aplicación cliente para administrar la conversación contextual.

## <a name="create-a-multi-turn-conversation-from-documents-structure"></a>Creación de una conversación de varios a su vez desde la estructura del documento
Cuando se crea una base de conocimiento, verá una casilla de verificación opcional para habilitar la extracción de varios a su vez. Si selecciona esta opción, al importar un documento, la conversación múltiples a su vez puede implicarse desde la estructura. Si no existe esa estructura, QnA Maker crea automáticamente el seguimiento pares de QnA de símbolo del sistema. Activar varios estructura solo se puede inferir de DOCX, PDF o direcciones URL de archivos. 

La siguiente imagen de un Microsoft Surface [archivo PDF](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/product-manual.pdf) está pensado para usarse como un manual. 

![! [Si importa un documento, conversación contextual puede ser implícita de la estructura. Si no existe esa estructura, QnA Maker crea automáticamente el seguimiento pares de QnA de símbolo del sistema, como parte de la importación del documento.] (.. / media/conversational-context/import-file-with-conversational-structure.png)](../media/conversational-context/import-file-with-conversational-structure.png#lightbox)

Al importar el documento PDF, QnA Maker determina los mensajes de seguimiento de la estructura para crear un flujo de conversación. 

![! [Al importar el documento PDF, QnA Maker determina los mensajes de seguimiento de la estructura para crear un flujo de conversación. ](../media/conversational-context/surface-manual-pdf-follow-up-prompt.png)](../media/conversational-context/surface-manual-pdf-follow-up-prompt.png#lightbox)

## <a name="show-questions-and-answers-with-context"></a>Mostrar las preguntas y respuestas con contexto

1. Reduzca los pares de preguntas y respuestas muestra sólo a aquellos con las conversaciones contextuales. Seleccione **ver opciones**, a continuación, seleccione **Show contexto (versión preliminar)**. La lista estará vacía hasta que agregue el primer par de preguntas y respuestas con un símbolo del sistema de seguimiento. 

    ![Filtrar la pregunta y responda a pares por las conversaciones contextuales](../media/conversational-context/filter-question-and-answers-by-context.png)

## <a name="add-new-qna-pair-as-follow-up-prompt"></a>Agregar nuevo par de QnA como símbolo del sistema de seguimiento

1. Seleccione **par agregar QnA**. 
1. Escriba el nuevo texto de pregunta, `Give feedback.` con una respuesta de `What kind of feedback do you have?`.

1. En el **respuesta** columna para esta pregunta, seleccione **símbolo del sistema de agregar seguimiento**. 
1. El **símbolo del sistema de seguimiento (versión preliminar)** ventana emergente le permite buscar una pregunta existente o escriba una pregunta nueva. Cree un nuevo símbolo del sistema escribiendo los siguientes valores: 

    |Campo de texto|Valor|
    |--|--|
    |**Mostrar texto**|`Feedback on an QnA Maker service`|
    |**Vínculo para responder a**|`How would you rate QnA Maker??`|
    |||

    ![Crear nuevo QnA de símbolo del sistema](../media/conversational-context/create-child-prompt-from-parent.png)

1. Comprobar **solo contexto**. El **solo contexto** opción indica que el texto de este usuario se entiendan _sólo_ si dar en respuesta a la pregunta anterior. En este escenario, el texto del mensaje no tiene sentido como una pregunta independiente, solo tiene sentido desde el contexto de la pregunta anterior.
1. Seleccione **crear nuevo** , a continuación, seleccione **guardar**. 

    Esto crea un nuevo par de preguntas y respuestas y había vinculado a la pregunta seleccionada como un símbolo del sistema de seguimiento. El **contexto** columna ambas preguntas, indica una relación de símbolo del sistema de seguimiento. 

    ![! [La columna de contexto, para ambas preguntas, indica una relación de símbolo del sistema de seguimiento.] (.. / media/conversational-context/child-prompt-created.png)](../media/conversational-context/child-prompt-created.png#lightbox)

1. Seleccione **símbolo del sistema de agregar seguimiento** para el `Give feedback` pregunta para agregar otro símbolo del sistema de seguimiento. Se abrirá el **símbolo del sistema de seguimiento (versión preliminar)** ventana emergente.

1. Cree un nuevo símbolo del sistema escribiendo los siguientes valores:

    |Campo de texto|Valor|
    |--|--|
    |**Mostrar texto**|`Feedback on an existing feature`|
    |**Vínculo para responder a**|`Which feature would you like to give feedback on?`|
    |||

1. Comprobar **solo contexto**. El **solo contexto** opción indica que el texto de este usuario se entiendan _sólo_ si dar en respuesta a la pregunta anterior. En este escenario, el texto del mensaje no tiene sentido como una pregunta independiente, solo tiene sentido desde el contexto de la pregunta anterior.

1. Seleccione **Guardar**. 

    Esto crea una nueva pregunta y vincula la pregunta como una pregunta de símbolo del sistema de seguimiento para el `Give feedback` pregunta.
    
    En este momento, la pregunta superior tiene dos mensajes de seguimiento le gustó a la pregunta anterior, `Give feedback`.

    ![! [En este momento, la pregunta superior tiene dos mensajes de seguimiento le gustó a la pregunta anterior, "Comentarios"]. (.. / media/conversational-context/all-child-prompts-created.png)](../media/conversational-context/all-child-prompts-created.png#lightbox)

1. Seleccione **guardar y entrenar** para entrenar la base de conocimiento con las nuevas preguntas. 

## <a name="add-existing-qna-pair-as-follow-up-prompt"></a>Agregar par de QnA existente como símbolo del sistema de seguimiento

1. Si desea vincular un par de QnA existente como un símbolo del sistema de seguimiento, seleccione la fila para el par de preguntas y respuestas.
1. Seleccione **símbolo del sistema de agregar seguimiento** en esa fila.
1. En el **símbolo del sistema de seguimiento (versión preliminar)** ventana emergente, escriba el texto de respuesta en el cuadro de búsqueda. Se devuelven todas las coincidencias. Seleccione la respuesta que desee como el seguimiento y comprobar **solo contexto**, a continuación, seleccione **guardar**. 

    ![Busque el vínculo de la solicitud seguimiento al cuadro de diálogo de respuesta para una respuesta existente, utilizando el texto de la respuesta.](../media/conversational-context/search-follow-up-prompt-for-existing-answer.png)

    Una vez haya agregado el símbolo del sistema de seguimiento, no olvide seleccionar **guardar y entrenar**.
  
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

## <a name="test-the-qna-set-to-get-all-the-follow-up-prompts"></a>Solicita la QnA conjunto para obtener todo el seguimiento de pruebas

Cuando prueba la pregunta con seguimiento le pida en el **prueba** panel, seleccione **habilitar múltiples a su vez**y escriba su pregunta. La respuesta incluye los mensajes de seguimiento.

![Cuando se prueba la pregunta en el panel de prueba, la respuesta incluye los mensajes de seguimiento.](../media/conversational-context/test-pane-with-question-having-follow-up-prompts.png)

## <a name="json-request-to-return-initial-answer-and-follow-up-prompts"></a>Solicitud JSON para devolver una respuesta inicial y mensajes de seguimiento

Utilice el vacío `context` objeto para solicitar la respuesta a la pregunta del usuario e incluir mensajes de seguimiento. 

```JSON
{
  "question": "accounts and signing in",
  "top": 30,
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
            "answer": "**Accounts and signing in**\n\nWhen you set up your Surface, an account is set up for you. You can create additional accounts later for family and friends, so each person using your Surface can set it up just the way they like. For more info, see All about accounts on Surface.com. \n\nThere are several ways to sign in to your Surface Pro 4: ",
            "score": 86.96,
            "id": 37,
            "source": "surface-pro-4-user-guide-EN .pdf",
            "metadata": [],
            "context": {
                "isContextOnly": true,
                "prompts": [
                    {
                        "displayOrder": 0,
                        "qnaId": 38,
                        "qna": null,
                        "displayText": "Use the sign-in screen"
                    },
                    {
                        "displayOrder": 1,
                        "qnaId": 39,
                        "qna": null,
                        "displayText": "Use Windows Hello to sign in"
                    },
                    {
                        "displayOrder": 2,
                        "qnaId": 40,
                        "qna": null,
                        "displayText": "Sign out"
                    }
                ]
            }
        },
        {
            "questions": [
                "Use the sign-in screen"
            ],
            "answer": "**Use the sign-in screen**\n\n1.  \n\nTurn on or wake your Surface by pressing the power button. \n\n2.  \n\nSwipe up on the screen or tap a key on the keyboard. \n\n3.  \n\nIf you see your account name and account picture, enter your password and select the right arrow or press Enter on your keyboard. \n\n4.  \n\nIf you see a different account name, select your own account from the list at the left. Then enter your password and select the right arrow or press Enter on your keyboard. ",
            "score": 32.27,
            "id": 38,
            "source": "surface-pro-4-user-guide-EN .pdf",
            "metadata": [],
            "context": {
                "isContextOnly": true,
                "prompts": []
            }
        },
        {
            "questions": [
                "Sign out"
            ],
            "answer": "**Sign out**\n\nHere's how to sign out: \n\n Go to Start , and right-click your name. Then select Sign out. ",
            "score": 27.0,
            "id": 40,
            "source": "surface-pro-4-user-guide-EN .pdf",
            "metadata": [],
            "context": {
                "isContextOnly": true,
                "prompts": []
            }
        }
    ],
    "debugInfo": null
}
```

El `prompts` matriz proporciona el texto en el `displayText` propiedad y el `qnaId` valor por lo que puede aparecer estas respuestas como las siguientes opciones mostradas en el flujo de conversación. 

## <a name="json-request-to-return-non-initial-answer-and-follow-up-prompts"></a>Solicitud JSON para devolver la respuesta no inicial y mensajes de seguimiento

Rellenar el `context` objeto va a incluir contexto anterior.

En la siguiente solicitud JSON, la pregunta actual es `Use Windows Hello to sign in` y la pregunta anterior era `Accounts and signing in`. 

```JSON
{
  "question": "Use Windows Hello to sign in",
  "top": 30,
  "userId": "Default",
  "isTest": false,
  "qnaId": 39,
  "context": {
    "previousQnAId": 37,
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
                "Give feedback"
            ],
            "answer": "What kind of feedback do you have?",
            "score": 100.0,
            "id": 288,
            "source": "Editorial",
            "metadata": [],
            "context": {
                "isContextOnly": true,
                "prompts": [
                    {
                        "displayOrder": 0,
                        "qnaId": 291,
                        "qna": null,
                        "displayText": "Feedback on an QnA Maker service"
                    },
                    {
                        "displayOrder": 0,
                        "qnaId": 292,
                        "qna": null,
                        "displayText": "Feedback on an existing feature"
                    }
                ]
            }
        }
    ]
}
```

## <a name="displaying-prompts-and-sending-context-in-the-client-application"></a>Mostrar mensajes y el envío de contexto en la aplicación cliente 

Si ha agregado indicaciones de la base de conocimiento y probado el flujo en el panel prueba, los mensajes no iniciará automáticamente que se muestran en las aplicaciones cliente. Puede mostrar los mensajes como botones o las acciones sugeridas como parte de la respuesta a la consulta del usuario en el cliente aplicaciones mediante la inclusión de este [ejemplo Bot Framework](https://aka.ms/qnamakermultiturnsample) en el código. La aplicación cliente debe almacenar el Id. de QnA actual y la consulta del usuario y pasarlos en la [objeto de contexto de la API GenerateAnswer](#json-request-to-return-non-initial-answer-and-follow-up-prompts) en la siguiente consulta de usuario.

## <a name="display-order-supported-in-api"></a>Orden de visualización compatible con API

Es compatible con el orden de visualización, devuelto en la respuesta JSON, solo la API de edición. 

## <a name="next-steps"></a>Pasos siguientes

Más información acerca de las conversaciones contextuales desde el [ejemplo diálogo](https://aka.ms/qnamakermultiturnsample) u obtenga más información [bot conceptual de diseño para las conversaciones de varios a su vez](https://docs.microsoft.com/azure/bot-service/bot-builder-conversations?view=azure-bot-service-4.0).

> [!div class="nextstepaction"]
> [Migrar una base de conocimiento](../Tutorials/migrate-knowledge-base.md)
