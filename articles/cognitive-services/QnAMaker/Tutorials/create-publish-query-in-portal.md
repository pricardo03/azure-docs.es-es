---
title: 'Tutorial: Creación, publicación y respuesta desde una base de conocimiento en el portal de QnA Maker'
titleSuffix: Azure Cognitive Services
description: Este tutorial basado en portal le guía mediante la creación y publicación de una base de conocimiento mediante programación y de la posterior respuesta a una pregunta desde dicha base de conocimiento.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.technology: qna-maker
ms.topic: tutorial
ms.date: 10/29/2018
ms.author: diberry
ms.openlocfilehash: 08f708f740b90f27af5443b46c5d03bef688bd45
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2018
ms.locfileid: "50221682"
---
# <a name="tutorial-create-a-knowledge-base-then-answer-question-via-the-qna-maker-portal"></a>Tutorial: Creación de una base de conocimiento y posterior respuesta a una pregunta mediante el portal de QnA Maker

Este tutorial le guía mediante la creación y publicación de una base de conocimiento y de la posterior respuesta a una pregunta desde dicha base de conocimiento.

En este tutorial, aprenderá a: 

> [!div class="checklist"]
* Creación de una base de conocimiento en el portal de QnA Maker
* Revisar, guardar y entrenar la base de conocimiento
* Publicación de una base de conocimiento
* Use Curl para consultar la base de conocimiento

> [!NOTE] 
> La versión mediante programación de este tutorial está disponible con una solución completa en el repositorio [**Azure-Samples/cognitive-services-qnamaker-csharp** de GitHub](https://github.com/Azure-Samples/cognitive-services-qnamaker-csharp/tree/master/documentation-samples/tutorials/create-publish-answer-knowledge-base).

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

1. En la barra de herramientas que hay encima de la lista de preguntas y respuestas, seleccione el icono del engranaje. Muestra los filtros de cada pregunta y respuesta. Las preguntas de la charla tienen el filtro **editorial: chit chat** ya establecido. Este filtro se devuelve a la aplicación cliente junto con la respuesta seleccionada. La aplicación cliente, como un bot de chat, puede usar este filtro para determinar las interacciones con el usuario o un procesamiento adicional.

    ![Ver filtros](../media/qnamaker-tutorial-create-publish-query-in-portal/save-and-train-kb-chit-chat.png)

1. Seleccione **Save and train** (Guardar y entrenar) en la barra de menús superior.

## <a name="publish-to-get-kb-endpoints"></a>Publicación para obtener los puntos de conexión de la base de conocimiento

Seleccione el botón **Publicar** en el menú superior. Una vez que se encuentre en la página de publicación, seleccione **Publicar**, junto al botón **Cancelar**.

![Publicar](../media/qnamaker-tutorial-create-publish-query-in-portal/publish-1.png)

Una vez publicada la base de conocimiento, se muestra el punto de conexión

![Publicar](../media/qnamaker-tutorial-create-publish-query-in-portal/publish-2.png)

## <a name="use-curl-to-query-for-an-faq-answer"></a>Uso de curl para consultar una respuesta a preguntas más frecuentes

1. Seleccione la pestaña **Curl**. 

    ![Comando Curl](../media/qnamaker-tutorial-create-publish-query-in-portal/publish-3-curl.png)

1. Copie el texto de la pestaña **Curl** y ejecútelo en un terminal con Curl habilitado o en la línea de comandos. El valor del encabezado de la autorización incluye el texto `Endpoint ` con un espacio al final y, después, la clave.

1. Reemplace `<Your question>` por `How large can my KB be?`. Es muy parecida a la pregunta, `How large a knowledge base can I create?`, pero no es exactamente igual. QnA Maker aplica el procesamiento de lenguaje natural para determinar si las dos preguntas son iguales.     

1. Ejecute el comando CURL y recibir la respuesta de JSON, que incluye la puntuación y la respuesta. 

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

## <a name="use-curl-to-query-for-a-chit-chat-answer"></a>Use curl para consulta una respuesta de la charla

1. En el terminal con Curl habilitado, reemplace `How large can my KB be?` por una instrucción de fin de conversación de bot desde el usuario, como `Thank you`.   

1. Ejecute el comando CURL y recibir la respuesta de JSON, que incluye la puntuación y la respuesta. 

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

    Dado que la pregunta de `Thank you` coincidía exactamente con una pregunta de charla, QnA Maker está completamente seguro con la puntuación de 100. QnA Maker también ha devuelto todas las preguntas relacionadas, así como la propiedad metadata que contiene la información del filtro de charla.  

## <a name="use-curl-to-query-for-the-default-answer"></a>Uso de curl para consultar la respuesta predeterminada

Cualquier pregunta de cuya respuesta QnA Maker no esté seguro recibe la respuesta predeterminada. Esta opción se configura en Azure Portal. 

1. En el terminal habilitado con Curl, reemplace `Thank you` por `x`. 

1. Ejecute el comando CURL y recibir la respuesta de JSON, que incluye la puntuación y la respuesta. 

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
    
    QnA Maker devuelve una puntuación de 0, lo que significa que no tiene ninguna confianza, pero también devuelve la respuesta predeterminada. 

## <a name="next-steps"></a>Pasos siguientes

Para más información acerca de los formatos de archivo admitidos, consulte [Orígenes de datos admitidos](../Concepts/data-sources-supported.md). 

Más información acerca de las [personalidades](../Concepts/best-practices.md#chit-chat) de charla.

Para más información acerca de la respuesta predeterminada, consulte [No se encontraron coincidencias](../Concepts/confidence-score.md#no-match-found). 

> [!div class="nextstepaction"]
> [Conceptos de bases de conocimiento](../Concepts/knowledge-base.md)