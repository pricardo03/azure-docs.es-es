---
title: Crear, publicar, responder en QnA Maker
titleSuffix: Azure Cognitive Services
description: Cree una nueva base de conocimiento con preguntas y respuestas de una página pública de preguntas más frecuentes basada en web. Guarde, entrene y publique la base de conocimiento. Una vez publicada la base de conocimiento, envíe una pregunta y reciba una respuesta con un comando CURL. A continuación, cree un bot y pruébelo con la misma pregunta.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: tutorial
ms.date: 04/08/2019
ms.author: diberry
ms.openlocfilehash: 299dd61055503f0b5a11cbe97e137e4760edadda
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59266960"
---
# <a name="tutorial-from-qna-maker-portal-create-a-knowledge-base"></a>Tutorial: Creación de una base de conocimiento desde el portal de QnA Maker

Cree una nueva base de conocimiento con preguntas y respuestas de una página pública de preguntas más frecuentes basada en web. Guarde, entrene y publique la base de conocimiento. Una vez publicada la base de conocimiento, envíe una pregunta y reciba una respuesta con un comando cURL. A continuación, cree un bot y pruébelo con la misma pregunta. 

En este tutorial, aprenderá a: 

> [!div class="checklist"]
> * Creación de una base de conocimiento en el portal de QnA Maker
> * Revisar, guardar y entrenar la base de conocimiento
> * Publicación de una base de conocimiento
> * Use Curl para consultar la base de conocimiento
> * Creación de un bot
> 
> [!NOTE]
> La versión basada en programación de este tutorial está disponible con una solución completa en el repositorio [**Azure-Samples/cognitive-services-qnamaker-csharp** de GitHub](https://github.com/Azure-Samples/cognitive-services-qnamaker-csharp/tree/master/documentation-samples/tutorials/create-publish-answer-knowledge-base).

## <a name="prerequisites"></a>Requisitos previos

Este tutorial requiere que exista un [servicio QnA Maker](../How-To/set-up-qnamaker-service-azure.md). 

## <a name="create-a-knowledge-base"></a>Creación de una base de conocimientos 

1. Inicie sesión en el portal de [QnA Maker](https://www.qnamaker.ai). 

1. Seleccione **Create a knowledge base** (Crear una base de conocimiento) en el menú superior.

    ![Paso 1 del proceso de creación de KB](../media/qnamaker-tutorial-create-publish-query-in-portal/create-kb-step-1.png)

1. Omita el primer paso, ya que va a usar el servicio QnA Maker existente. 

1. En el paso siguiente, seleccione la configuración existente:  

    |Configuración|Propósito|
    |--|--|
    |Id. de directorio de Microsoft Azure|El _Id. de directorio de Microsoft Azure_ está asociado con la cuenta que se usa para iniciar sesión tanto en Azure Portal como en el portal de QnA Maker. |
    |Nombre de la suscripción de Azure|La cuenta de facturación en que ha creado el recurso de QnA Maker.|
    |Servicio Azure QnA|El recurso de QnA Maker existente.|

    ![Paso 2 del proceso de creación de KB](../media/qnamaker-tutorial-create-publish-query-in-portal/create-kb-step-2.png)

1. En el paso siguiente, escriba el nombre de la base de conocimiento, `My Tutorial kb`.

    ![Paso 3 del proceso de creación de KB](../media/qnamaker-tutorial-create-publish-query-in-portal/create-kb-step-3.png)

1. En el paso siguiente, rellene la base de conocimiento con los siguientes valores:  

    |Nombre del valor|Valor del valor|Propósito|
    |--|--|--|
    |URL|`https://docs.microsoft.com/azure/cognitive-services/qnamaker/faqs` |El formato del contenido de las preguntas frecuentes de esa dirección URL es una pregunta seguida de una respuesta. QnA Maker puede interpretar este formato para extraer las preguntas y las respuestas asociadas.|
    |Archivo |_no se usa en este tutorial_|Carga los archivos para las preguntas y respuestas. |
    |Personalidad de charla|El amigo|Proporciona una personalidad descriptiva y desenfadada a las preguntas y respuestas frecuentes. Dichas preguntas y respuestas de pueden editar más adelante. |

    ![Paso 4 del proceso de creación de KB](../media/qnamaker-tutorial-create-publish-query-in-portal/create-kb-step-4.png)

1. Seleccione **Create your KB** (Crear base de conocimiento) para finalizar el proceso de creación.

    ![Paso 5 del proceso de creación de KB](../media/qnamaker-tutorial-create-publish-query-in-portal/create-kb-step-5.png)

## <a name="review-kb-save-and-train"></a>Revisión de la base de conocimiento, guardado y entrenamiento

1. Reviste las preguntas y respuestas. En la primera página encontrará preguntas y respuestas de la dirección URL. 

    ![Guardar y entrenar](../media/qnamaker-tutorial-create-publish-query-in-portal/save-and-train-kb.png)

1. Seleccione la última página de preguntas y respuestas en la parte inferior de la tabla. La página muestra preguntas y respuestas de la personalidad de la charla. 

1. En la barra de herramientas que hay encima de la lista de preguntas y respuestas, seleccione el icono de los metadatos. Muestra las etiquetas de metadatos de cada pregunta y respuesta. Las preguntas de la charla tienen los metadatos **editorial: chit chat** ya establecido. Estos metadatos se devuelven a la aplicación cliente junto con la respuesta seleccionada. La aplicación cliente, como un bot de chat, puede usar estos metadatos filtrados para determinar un procesamiento adicional o las interacciones con el usuario.

    ![Visualización de etiquetas de metadatos](../media/qnamaker-tutorial-create-publish-query-in-portal/save-and-train-kb-chit-chat.png)

1. Seleccione **Save and train** (Guardar y entrenar) en la barra de menús superior.

## <a name="publish-to-get-kb-endpoints"></a>Publicación para obtener los puntos de conexión de la base de conocimiento

Seleccione el botón **Publicar** en el menú superior. Una vez que se encuentre en la página de publicación, seleccione **Publicar**, junto al botón **Cancelar**.

![Publicar](../media/qnamaker-tutorial-create-publish-query-in-portal/publish-1.png)

Una vez publicada la base de conocimiento, se muestra el punto de conexión

![Configuración del punto de conexión de la página de publicación](../media/qnamaker-tutorial-create-publish-query-in-portal/publish-2.png)

No cierre esta página **Publicar** ya que la usará para crear un bot más adelante en el tutorial. 

## <a name="use-curl-to-query-for-an-faq-answer"></a>Uso de curl para consultar una respuesta a preguntas más frecuentes

1. Seleccione la pestaña **Curl**. 

    ![Comando Curl](../media/qnamaker-tutorial-create-publish-query-in-portal/publish-3-curl.png)

1. Copie el texto de la pestaña **Curl** y ejecútelo en un terminal con Curl habilitado o en la línea de comandos. El valor del encabezado de la autorización incluye el texto `Endpoint` con un espacio al final y, después, la clave.

1. Reemplace `<Your question>` por `How large can my KB be?`. Es muy parecida a la pregunta, `How large a knowledge base can I create?`, pero no es exactamente igual. QnA Maker aplica el procesamiento de lenguaje natural para determinar si las dos preguntas son iguales.     

1. Ejecute el comando CURL y reciba la respuesta JSON, que incluye la puntuación y la respuesta. 

    ```TXT
      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
    100   581  100   543  100    38    418     29  0:00:01  0:00:01 --:--:--   447{
      "answers": [
        {
          "questions": [
            "How large a knowledge base can I create?"
          ],
          "answer": "The size of the knowledge base depends on the SKU of Azure search you choose when creating the QnA Maker service. Read [here](https://docs.microsoft.com/azure/cognitive-services/qnamaker/tutorials/choosing-capacity-qnamaker-deployment)for more details.",
          "score": 42.81,
          "id": 2,
          "source": "https://docs.microsoft.com/azure/cognitive-services/qnamaker/faqs",
          "metadata": []
        }
      ]
    }
    
    ```

    QnA Maker está más o menos seguro con la puntuación de 42,81 %.  

## <a name="use-curl-to-query-for-a-chit-chat-answer"></a>Use CURL para consultar una respuesta de charla

1. En el terminal con CURL habilitado, reemplace `How large can my KB be?` por una instrucción de fin de conversación de bot desde el usuario, como `Thank you`.   

1. Ejecute el comando CURL y reciba la respuesta JSON, que incluye la puntuación y la respuesta. 

    ```TXT
      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
    100   525  100   501  100    24    525     25 --:--:-- --:--:-- --:--:--   550{
      "answers": [
        {
          "questions": [
            "Thank you",
            "Thanks",
            "Thnx",
            "Kthx",
            "I appreciate it",
            "Thank you so much",
            "I thank you",
            "My sincere thank"
          ],
          "answer": "You're very welcome.",
          "score": 100.0,
          "id": 109,
          "source": "qna_chitchat_the_friend.tsv",
          "metadata": [
            {
              "name": "editorial",
              "value": "chitchat"
            }
          ]
        }
      ]
    }
   
    ```

    Dado que la pregunta de `Thank you` coincidía exactamente con una pregunta de charla, QnA Maker está completamente seguro con la puntuación de 100. QnA Maker también ha devuelto todas las preguntas relacionadas, así como la propiedad metadata que contiene la información de la etiqueta de metadatos de charla.  

## <a name="use-curl-to-query-for-the-default-answer"></a>Uso de CURL para consultar la respuesta predeterminada

Cualquier pregunta de cuya respuesta QnA Maker no esté seguro recibe la respuesta predeterminada. Esta opción se configura en Azure Portal. 

1. En el terminal habilitado con Curl, reemplace `Thank you` por `x`. 

1. Ejecute el comando CURL y reciba la respuesta JSON, que incluye la puntuación y la respuesta. 

    ```TXT
      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
    100   186  100   170  100    16    272     25 --:--:-- --:--:-- --:--:--   297{
      "answers": [
        {
          "questions": [],
          "answer": "No good match found in KB.",
          "score": 0.0,
          "id": -1,
          "metadata": []
        }
      ]
    }
    ```
    
    QnA Maker devuelve una puntuación de `0`, lo que significa que no tiene ninguna confianza, pero también devuelve la respuesta predeterminada. 

## <a name="create-a-knowledge-base-bot"></a>Creación de un bot de base de conocimientos

Para más información, consulte [Create a chat bot with this knowledge base](create-qna-bot.md) (Creación de un bot de chat con esta base de conocimiento).

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando haya terminado con el bot de base de conocimientos, elimine el grupo de recursos, `my-tutorial-rg`, para eliminar todos los recursos de Azure creados en el proceso del bot.

Cuando haya terminado con la base de conocimiento, en el portal de QnA Maker, seleccione **My knowledge bases** (Mis bases de conocimientos), después, seleccione la base de conocimiento, **My Tutorial kb** y, a continuación, seleccione el icono de eliminación situado en el extremo derecho de esa fila.  

## <a name="next-steps"></a>Pasos siguientes

Para más información acerca de los formatos de archivo admitidos, consulte [Orígenes de datos admitidos](../Concepts/data-sources-supported.md). 

Más información acerca de las [personalidades](../Concepts/best-practices.md#chit-chat) de charla.

Para más información acerca de la respuesta predeterminada, consulte [No se encontraron coincidencias](../Concepts/confidence-score.md#no-match-found). 

> [!div class="nextstepaction"]
> [Create a chat bot with this knowledge base (Creación de un bot de chat con esta base de conocimiento)](create-qna-bot.md)