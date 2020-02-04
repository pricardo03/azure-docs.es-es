---
title: 'Tutorial: Creación, publicación y respuesta en QnA Maker'
description: En este tutorial se muestra cómo crear una base de conocimiento con preguntas y respuestas de una página pública de preguntas más frecuentes basada en web. Guarde, entrene y publique la base de conocimiento. Una vez publicada la base de conocimiento, envíe una pregunta y reciba una respuesta con un comando cURL. A continuación, cree un bot y pruébelo con la misma pregunta.
ms.topic: tutorial
ms.date: 01/28/2020
ms.openlocfilehash: 07a92b0f24aa95fc4a5188f54c7194fd342409fb
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76844208"
---
# <a name="tutorial-from-the-qna-maker-portal-create-a-knowledge-base"></a>Tutorial: En el portal de QnA Maker, creación de una base de conocimiento

Cree una nueva base de conocimiento con preguntas y respuestas de una página pública de preguntas más frecuentes basada en web. Guarde, entrene y publique la base de conocimiento. Una vez publicada la base de conocimiento, envíe una pregunta y reciba una respuesta con un comando cURL. A continuación, cree un bot y pruébelo con la misma pregunta.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear una base de conocimiento en el portal de QnA Maker.
> * Revisar, guardar y entrenar la base de conocimiento.
> * Publicar la base de conocimiento.
> * Usar Curl para consultar la base de conocimiento.
> * Crear un bot.


> [!NOTE]
> La versión basada en programación de este tutorial está disponible con una solución completa en el repositorio [**Azure-Samples/cognitive-services-qnamaker-csharp** de GitHub](https://github.com/Azure-Samples/cognitive-services-qnamaker-csharp/tree/master/documentation-samples/tutorials/create-publish-answer-knowledge-base).

## <a name="prerequisites"></a>Prerequisites

Este tutorial requiere que exista un [servicio QnA Maker](../How-To/set-up-qnamaker-service-azure.md).

## <a name="create-a-knowledge-base"></a>Creación de una base de conocimientos

1. Inicie sesión en el portal de [QnA Maker](https://www.qnamaker.ai).

1. Seleccione **Create a knowledge base** (Crear una base de conocimiento) en el menú superior.

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla del portal de QnA Maker](../media/qnamaker-tutorial-create-publish-query-in-portal/create-kb-step-1.png)

1. Omita el primer paso, ya que va a usar el servicio QnA Maker existente.

1. Seleccione la configuración existente:

    |Configuración|Propósito|
    |--|--|
    |Identificador de directorio de Microsoft Azure|Este identificador está asociado con la cuenta que se usa para iniciar sesión tanto en Azure Portal como en el portal de QnA Maker. |
    |Nombre de la suscripción de Azure|La cuenta de facturación en la que ha creado el recurso de QnA Maker.|
    |Servicio Azure QnA|El recurso de QnA Maker existente.|
    |Idioma|Idioma que se usa para todas las bases de conocimiento del servicio. No mezcle varios idiomas en un único recurso, ya que sería perjudicial para la calidad de las respuestas.|

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla del portal de QnA Maker](../media/qnamaker-tutorial-create-publish-query-in-portal/create-kb-step-2.png)

1. Escriba el nombre de la base de conocimiento, `My Tutorial kb`.

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla del portal de QnA Maker](../media/qnamaker-tutorial-create-publish-query-in-portal/create-kb-step-3.png)

1. Rellene la base de conocimiento con los valores siguientes:

    |Nombre del valor|Valor del valor|Propósito|
    |--|--|--|
    |Habilitar la extracción en varios turnos|Activado|Permite solicitudes de seguimiento.|
    |Texto de respuesta predeterminado|`No good answer was found.`|Se devuelve cuando no hay ninguna coincidencia con la pregunta.|
    |URL|`https://docs.microsoft.com/azure/cognitive-services/qnamaker/faqs` |El formato del contenido de las preguntas frecuentes de esa dirección URL es una pregunta seguida de una respuesta. QnA Maker puede interpretar este formato para extraer las preguntas y las respuestas asociadas.|
    |Archivo |_no se usa en este tutorial_|Carga los archivos para las preguntas y respuestas. |
    |Personalidad de charla|Descriptiva|Proporciona una [personalidad](../Concepts/best-practices.md#chit-chat) descriptiva y desenfadada a las preguntas y respuestas frecuentes. Dichas preguntas y respuestas de pueden editar más adelante. |

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla del portal de QnA Maker](../media/qnamaker-tutorial-create-publish-query-in-portal/create-kb-step-4.png)

1. Seleccione **Create your KB** (Crear base de conocimiento) para finalizar el proceso de creación.

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla del portal de QnA Maker](../media/qnamaker-tutorial-create-publish-query-in-portal/create-kb-step-5.png)

## <a name="review-save-and-train-the-knowledge-base"></a>Revisar, guardar y entrenar la base de conocimiento

1. Reviste las preguntas y respuestas. En la primera página encontrará preguntas y respuestas de la dirección URL.

    > [!div class="mx-imgBorder"]
    > ![Captura de pantalla del portal de QnA Maker](../media/qnamaker-tutorial-create-publish-query-in-portal/save-and-train-kb.png)

1. Seleccione la última página de preguntas y respuestas en la parte inferior de la tabla. La página muestra preguntas y respuestas de la personalidad de la charla.

1. En la barra de herramientas que hay encima de la lista de preguntas y respuestas, seleccione el icono **Opciones de vista** y, después, seleccione **Mostrar metadatos**. Muestra las etiquetas de metadatos de cada pregunta y respuesta. Las preguntas de la charla tienen los metadatos **editorial: chit chat** ya establecido. Estos metadatos se devuelven a la aplicación cliente junto con la respuesta seleccionada. La aplicación cliente, como un bot de chat, puede usar estos metadatos filtrados para determinar un procesamiento adicional o las interacciones con el usuario.

    ![Captura de pantalla del portal de QnA Maker](../media/qnamaker-tutorial-create-publish-query-in-portal/save-and-train-kb-chit-chat.png)

1. Seleccione **Save and train** (Guardar y entrenar) en la barra de menús superior.

## <a name="publish-to-get-knowledge-base-endpoints"></a>Publicación para obtener los puntos de conexión de la base de conocimientos

Seleccione el botón **Publicar** en el menú superior. En la página de publicación, seleccione **Publicar**.

![Captura de pantalla del portal de QnA Maker](../media/qnamaker-tutorial-create-publish-query-in-portal/publish-1.png)

Una vez publicada la base de conocimiento, se muestra el punto de conexión.

![Captura de pantalla de la configuración del punto de conexión](../media/qnamaker-tutorial-create-publish-query-in-portal/publish-2.png)

No cierre esta página **Publicar**. La necesitará más adelante en el tutorial para crear un bot.

## <a name="use-curl-to-query-for-an-faq-answer"></a>Uso de cURL para consultar una respuesta a preguntas más frecuentes

1. Seleccione la pestaña **Curl**.

    ![Captura de pantalla de la pestaña Curl](../media/qnamaker-tutorial-create-publish-query-in-portal/publish-3-curl.png)

1. Copie el texto de la pestaña **Curl** y ejecútelo en un terminal con cURL habilitado o en la línea de comandos. El valor del encabezado de autorización incluye el texto `Endpoint`, con un espacio al final y, después, la clave.

1. Reemplace `<Your question>` por `How large can my KB be?`. Es muy parecida a la pregunta, `How large a knowledge base can I create?`, pero no es exactamente igual. QnA Maker aplica el procesamiento de lenguaje natural para determinar si las dos preguntas son iguales.

1. Ejecute el comando cURL y reciba la respuesta JSON, que incluye la puntuación y la respuesta.

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

## <a name="use-curl-to-query-for-a-chit-chat-answer"></a>Uso de cURL para consultar una respuesta de charla

1. En el terminal con cURL habilitado, reemplace `How large can my KB be?` por una instrucción de fin de conversación de bot desde el usuario, como `Thank you`.

1. Ejecute el comando cURL y reciba la respuesta JSON, que incluye la puntuación y la respuesta.

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

## <a name="use-curl-to-query-for-the-default-answer"></a>Uso de cURL para consultar la respuesta predeterminada

Cualquier pregunta de la que QnA Maker no esté seguro recibe la respuesta predeterminada. Esta opción se configura en Azure Portal.

1. En el terminal habilitado con cURL, reemplace `Thank you` por `x`.

1. Ejecute el comando cURL y reciba la respuesta JSON, que incluye la puntuación y la respuesta.

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

    QnA Maker ha devuelto una puntuación de `0`, lo que significa ninguna confianza. También ha devuelto la respuesta predeterminada.

## <a name="create-a-knowledge-base-bot"></a>Creación de un bot de base de conocimientos

Para más información, consulte [Create a chat bot with this knowledge base](create-qna-bot.md) (Creación de un bot de chat con esta base de conocimiento).

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando haya terminado con el bot de base de conocimientos, elimine el grupo de recursos, `my-tutorial-rg`, para eliminar todos los recursos de Azure creados en el proceso del bot.

Cuando haya terminado con la base de conocimiento, en el portal de QnA Maker, seleccione **My knowledge bases** (Mis bases de conocimiento). A continuación, seleccione la base de conocimiento, **My Tutorial kb**y seleccione el icono de eliminación en el extremo derecho de esa fila.

## <a name="next-steps"></a>Pasos siguientes

Para más información acerca de los formatos de archivo admitidos, consulte [Orígenes de datos admitidos](../Concepts/content-types.md).

Más información acerca de las [personalidades](../Concepts/best-practices.md#chit-chat) de charla.

Para más información acerca de la respuesta predeterminada, consulte [No se encontraron coincidencias](../Concepts/confidence-score.md#no-match-found).

> [!div class="nextstepaction"]
> [Creación de un bot de chat con esta base de conocimiento](create-qna-bot.md)