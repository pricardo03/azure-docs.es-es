---
title: 'Inicio rápido: Adición de preguntas y respuestas en el portal de QnA Maker'
titleSuffix: Azure Cognitive Services
description: Este inicio rápido le indica cómo agregar conjuntos de preguntas y respuestas con metadatos para que los usuarios puedan encontrar la respuesta correcta a su pregunta.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: quickstart
ms.date: 11/22/2019
ms.author: diberry
ms.openlocfilehash: 664d6006ab78f91a8ed0e199cf78fae9512efd73
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76843046"
---
# <a name="quickstart-add-questions-and-answer-with-qna-maker-portal"></a>Inicio rápido: Adición de preguntas y respuestas con el portal de QnA Maker

Después de crear una base de conocimiento, agregue conjuntos de preguntas y respuestas con los metadatos para que los usuarios puedan encontrar la respuesta correcta a su pregunta.

La respuesta correcta es una respuesta única, pero puede haber muchas formas en las que un cliente podría formular la pregunta que conduce a esa única respuesta.

Por ejemplo, las preguntas de la tabla siguiente son acerca de los límites de servicio de Azure, pero cada una tiene que ver con un servicio de Azure diferente.

<a name="qna-table"></a>


|Set|Preguntas|Respuesta|Metadatos|
|--|--|--|--|
|N.° 1|`How large a knowledge base can I create?`<br><br>`What is the max size of a knowledge base?`<br><br>`How many GB of data can a knowledge base hold?` |`The size of the knowledge base depends on the SKU of Azure search you choose when creating the QnA Maker service. Read [here](https://docs.microsoft.com/azure/cognitive-services/qnamaker/tutorials/choosing-capacity-qnamaker-deployment) for more details.`|`service=qna_maker`<br>`link_in_answer=true`|
|N.° 2|`How many knowledge bases can I have for my QnA Maker service?`<br><br>`I selected a Azure Cognitive Search tier that holds 15 knowledge bases, but I can only create 14 - what is going on?`<br><br>`What is the connection between the number of knowledge bases in my QnA Maker service and the Azure Cognitive Search service size?` |`Each knowledge base uses 1 index, and all the knowledge bases share a test index. You can have N-1 knowledge bases where N is the number of indexes your Azure Cognitive Search tier supports.`|`service=search`<br>`link_in_answer=false`|

Una vez que los metadatos se agregan a un conjunto de preguntas y respuestas, la aplicación cliente puede:

* Solicitar respuestas que solo coincidan con determinados metadatos
* Recibir todas las respuestas, pero procesarlas posteriormente en función de los metadatos de cada una

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Prerequisites

* Un servicio QnA Maker
* Una base de conocimiento creada en ese servicio QnA Maker

Ambos se crearon en el [primer inicio rápido](../how-to/create-knowledge-base.md).

## <a name="sign-in-to-the-qna-maker-portal"></a>Inicio de sesión en el portal de QnA Maker

1. Inicie sesión en el [portal de QnA Maker](https://www.qnamaker.ai).

1. Seleccione la base de conocimiento existente. Si no tiene una base de conocimiento, vuelva al [inicio rápido anterior](../how-to/create-knowledge-base.md) y finalice los pasos para crear la base de conocimiento.

## <a name="add-additional-alternatively-phrased-questions"></a>Adición de preguntas alternativas adicionales

La base de conocimiento actual, del [inicio rápido anterior](../how-to/create-knowledge-base.md), tiene los conjuntos de preguntas y respuestas de solución de problemas de QnA Maker. Estos conjuntos se crearon al agregar la dirección URL a la base de conocimiento durante el proceso de creación.

Cuando se importó esta dirección URL, solo se creó una pregunta con una respuesta.

En este procedimiento, agregue preguntas adicionales.

1. En la página **Edit** (Editar), use el cuadro de texto de búsqueda situado encima de los conjuntos de preguntas y respuestas para encontrar la pregunta `How large a knowledge base can I create?`.

1. En la columna **Question** (Pregunta), seleccione **+ Add alternative phrasing** (+Agregar expresión alternativa) y, luego, agregue cada nueva expresión, que se proporciona en la tabla siguiente.

    |Expresión alternativa|
    |--|
    |`What is the max size of a knowledge base?`|
    |`How many GB of data can a knowledge base hold?`|

1. Seleccione **Save and train** (Guardar y entrenar) para volver a entrenar la base de conocimiento.

1. Seleccione **Test** (Probar), escriba una pregunta parecida a la nueva expresión alternativa, pero no con las mismas palabras:

    `What GB size can a knowledge base be?`

    La respuesta correcta se devuelve en formato Markdown: `The size of the knowledge base depends on the SKU of Azure search you choose when creating the QnA Maker service. Read [here](https://docs.microsoft.com/azure/cognitive-services/qnamaker/tutorials/choosing-capacity-qnamaker-deployment) for more details.`

    Si selecciona **Inspect** (Inspeccionar) en la respuesta devuelta, puede ver que hay más respuestas que corresponden a la pregunta, pero no con el mismo nivel de confianza.

    No agregue todas las combinaciones posibles de expresiones alternativas. Active el [aprendizaje activo](../how-to/improve-knowledge-base.md) de QnA Maker para encontrar las expresiones alternativas que mejor ayuden a su base de conocimiento a satisfacer las necesidades de los usuarios.

1. Seleccione de nuevo **Test** (Probar) para cerrar la ventana de prueba.

## <a name="add-metadata-to-filter-the-answers"></a>Adición de metadatos para filtrar las respuestas

Agregar metadatos a un conjunto de preguntas y respuestas permite a la aplicación cliente solicitar respuestas filtradas. Este filtro se aplica antes de que se apliquen los [clasificadores primero y segundo](../concepts/query-knowledge-base.md#ranker-process).

1. Agregue el segundo conjunto de preguntas y respuestas, sin los metadatos, de la [primera tabla de este inicio rápido](#qna-table) y, luego, continúe con los pasos siguientes.

1. Seleccione **View options** (Ver opciones) y, luego, seleccione **Show metadata** (Mostrar metadatos).

1. Para el conjunto de preguntas y respuestas que acaba de agregar, seleccione **Add metadata tags** (Agregar etiquetas de metadatos) y, luego, agregue el nombre de `service` y el valor de `search`, `service:search`.

1. Agregue otras etiquetas de metadatos con el nombre de `link_in_answer` y el valor de `false`, `link_in_answer:false`.

1. Busque la primera respuesta en la tabla, `How large a knowledge base can I create?`.
1. Agregue pares de metadatos para las mismas dos etiquetas de metadatos:

    `link_in_answer`: `true`<br>
    `server`: `qna_maker`

    Ahora tiene dos preguntas con las mismas etiquetas de metadatos con valores diferentes.

1. Seleccione **Save and train** (Guardar y entrenar) para volver a entrenar la base de conocimiento.

1. Seleccione **Publish** (Publicar) en el menú superior para ir a la página de publicación.
1. Seleccione el botón **Publish** (Publicar) para publicar la base de conocimiento actual en un punto de conexión consultable.
1. Después de publicar la base de conocimiento, seleccione la pestaña **Curl** para ver un ejemplo de comando de cURL usado para generar una respuesta desde la base de conocimiento.
1. Copie el comando en un bloc de notas u otro entorno editable para poder editarlo. Use sus propios valores de nombre de recurso, identificador de base de conocimiento y clave de punto de conexión:

    |Replace|
    |--|
    |`your-resource-name`|
    |`your-knowledge-base-id`|
    |`your-endpoint-key`|

    ```curl
    curl -X POST https://your-resource-name.azurewebsites.net/qnamaker/knowledgebases/your-knowledge-base-id/generateAnswer -H "Authorization: EndpointKey your-endpoint-key" -H "Content-type: application/json" -d "{'top':30, 'question':'size','strictFilters': [{'name':'service','value':'qna_maker'}]}"
    ```

    Observe que la pregunta es solo una palabra, `size`, que puede devolver cualquier conjunto de preguntas y respuestas. La matriz `strictFilters` indica a la respuesta que se reduzca a solo las contestaciones de `qna_maker`.

    [!INCLUDE [Tip for debug property to JSON request](../includes/tip-debug-json.md)]

1. La respuesta incluye solo la contestación que cumple los criterios de filtro.

    A la siguiente respuesta de cURL se le ha cambiado el formato para mayor legibilidad:

    ```JSON
    {
        "answers": [
            {
                "questions": [
                    "How large a knowledge base can I create?",
                    "What is the max size of a knowledge base?",
                    "How many GB of data can a knowledge base hold?"
                ],
                "answer": "The size of the knowledge base depends on the SKU of Azure search you choose when creating the QnA Maker service. Read [here](https://docs.microsoft.com/azure/cognitive-services/qnamaker/tutorials/choosing-capacity-qnamaker-deployment)for more details.",
                "score": 68.76,
                "id": 3,
                "source": "https://docs.microsoft.com/azure/cognitive-services/qnamaker/troubleshooting",
                "metadata": [
                    {
                        "name": "link_in_answer",
                        "value": "true"
                    },
                    {
                        "name": "service",
                        "value": "qna_maker"
                    }
                ],
                "context": {
                    "isContextOnly": false,
                    "prompts": []
                }
            }
        ],
        "debugInfo": null
    }
    ```

    Si hay un conjunto de preguntas y respuestas que no cumple el término de búsqueda pero sí el filtro, no se devolverá. En su lugar, se devuelve la respuesta general `No good match found in KB.`.

    Asegúrese de mantener los pares de nombre y valor de los metadatos dentro de los límites necesarios.

## <a name="clean-up-resources"></a>Limpieza de recursos

Si quiere limpiar y eliminar una suscripción a Cognitive Services, puede eliminar el recurso o grupo de recursos. Al eliminar el grupo de recursos, también se elimina cualquier otro recurso que esté asociado a él.

* [Portal](../../cognitive-services-apis-create-account.md#clean-up-resources)
* [CLI de Azure](../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Obtención de una respuesta con Postman o cURL](get-answer-from-knowledge-base-using-url-tool.md)