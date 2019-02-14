---
title: 'Metadatos con GenerateAnswer API: QnA Maker'
titleSuffix: Azure Cognitive Services
description: QnA Maker permite agregar metadatos, en forma de pares de clave-valor, a los conjuntos de preguntas y respuestas. Esta información se puede usar para filtrar los resultados de las consultas de usuario y para almacenar información adicional que se puede usar en las conversaciones de seguimiento.
services: cognitive-services
author: tulasim88
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 12/18/2018
ms.author: tulasim88
ms.openlocfilehash: 1294b714c217178d53ed69cc886cd89f23620274
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55859494"
---
# <a name="using-metadata-and-the-generateanswer-api"></a>Uso de metadatos y GenerateAnswer API

QnA Maker permite agregar metadatos, en forma de pares de clave-valor, a los conjuntos de preguntas y respuestas. Esta información se puede usar para filtrar los resultados de las consultas de usuario y para almacenar información adicional que se puede usar en las conversaciones de seguimiento. Para más información, consulte la [base de conocimiento](../Concepts/knowledge-base.md).

## <a name="qna-entity"></a>Entidad QnA

En primer lugar, es importante comprender cómo QnA Maker almacena los datos de preguntas y respuestas. En la siguiente ilustración se muestra una entidad QnA:

![Entidad QnA](../media/qnamaker-how-to-metadata-usage/qna-entity.png)

Cada entidad QnA tiene un identificador único y persistente. El identificador puede utilizarse para realizar actualizaciones en una entidad QnA determinada.

## <a name="generateanswer-api"></a>GenerateAnswer API

GenerateAnswer API se usa en un bot o aplicación para consultar la base de conocimiento con una pregunta de usuario y obtener la mejor coincidencia de los conjuntos de preguntas y respuestas.

### <a name="generateanswer-endpoint"></a>Punto de conexión de GenerateAnswer

Después de publicar la base de conocimiento, ya sea desde el [portal de QnA Maker](https://www.qnamaker.ai) o mediante la [API](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff), puede obtener los detalles de su punto de conexión de GenerateAnswer.

Para obtener los detalles del punto de conexión:
1. Inicie sesión en [https://www.qnamaker.ai](https://www.qnamaker.ai).
2. En **My knowledge bases** (Mis bases de conocimiento), haga clic en la opción **View Code** (Ver código) correspondiente a su base de conocimiento.
![mis bases de conocimiento](../media/qnamaker-how-to-metadata-usage/my-knowledge-bases.png)
3. Obtenga los detalles del punto de conexión de GenerateAnswer.

![detalles del punto de conexión](../media/qnamaker-how-to-metadata-usage/view-code.png)

También puede obtener los detalles del punto de conexión en la pestaña **Settings** (Configuración) de la base de conocimiento.

### <a name="generateanswer-request"></a>Solicitud de GenerateAnswer

Para llamar a GenerateAnswer, se utiliza una solicitud HTTP POST. Para ver código de ejemplo que muestra cómo llamar a GenerateAnswer, consulte las [guías de inicio rápido](../quickstarts/csharp.md).

- **URL de la solicitud**: https://{QnA Maker endpoint}/knowledgebases/{knowledge base ID}/generateAnswer

- **Parámetros de la solicitud** 
    - **Knowledge base ID** (cadena): GUID de Knowledge Base.
    - **QnAMaker endpoint** (cadena): nombre de host del punto de conexión implementado en la suscripción de Azure.
- **Encabezados de solicitud**
    - **Content-Type** (cadena): tipo de soporte del cuerpo enviado a la API.
    - **Authorization** (cadena): su clave de punto de conexión (EndpointKey xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx).
- **Cuerpo de la solicitud**
    - **question** (cadena): pregunta del usuario que realizará la consulta a la base de conocimiento.
    - **top** (opcional, entero): número de resultados clasificados por orden de prioridad que se incluirá en la salida. El valor predeterminado es 1.
    - **userId** (opcional, cadena): identificador único que identifica el usuario. Este identificador se registrará en los registros de chat.
    - **strictFilters** (opcional, cadena): si se especifica, indica a QnA Maker que solo devuelva las respuestas que tienen los metadatos especificados. Para obtener más información, consulte más adelante.
    ```json
    {
        "question": "qna maker and luis",
        "top": 6,
        "strictFilters": [
        {
            "name": "category",
            "value": "api"
        }],
        "userId": "sd53lsY="
    }
    ```

### <a name="generateanswer-response"></a>Respuesta de GenerateAnswer

- **Respuesta 200**: una llamada correcta devuelve el resultado de la pregunta. La respuesta contiene los siguientes campos:
    - **answers**: lista de las respuestas de la consulta del usuario, en orden decreciente de puntuación de clasificación.
        - **score**: puntuación de clasificación entre 0 y 100.
        - **questions**: preguntas proporcionadas por el usuario.
        - **answer**: la respuesta a la pregunta.
        - **source**: nombre del origen desde el que se extrajo la respuesta o se guardó en la base de conocimiento.
        - **metadata**: metadatos asociados a la respuesta.
            - name: nombre de los metadatos. (cadena, longitud máx.: 100, requerido)
            - value: valor de los metadatos. (cadena, longitud máx.: 100, requerido)
        - **Id**: identificador único asignado a la respuesta.
    ```json
    {
        "answers": [
            {
                "score": 28.54820341616869,
                "Id": 20,
                "answer": "There is no direct integration of LUIS with QnA Maker. But, in your bot code, you can use LUIS and QnA Maker together. [View a sample bot](https://github.com/Microsoft/BotBuilder-CognitiveServices/tree/master/Node/samples/QnAMaker/QnAWithLUIS)",
                "source": "Custom Editorial",
                "questions": [
                    "How can I integrate LUIS with QnA Maker?"
                ],
                "metadata": [
                    {
                        "name": "category",
                        "value": "api"
                    }
                ]
            }
        ]
    }
    ```

## <a name="metadata-example"></a>Ejemplo de metadatos

Tenga en cuenta los siguientes datos de preguntas más frecuentes. Para agregar metadatos a Knowledge Base, haga clic en el icono de metadatos.

![agregar metadatos](../media/qnamaker-how-to-metadata-usage/add-metadata.png)

### <a name="filter-results-with-strictfilters-for-metadata-tags"></a>Filtrar los resultados con strictFilters para las etiquetas de metadatos

Tomemos la pregunta del usuario "¿Cuándo cierra este hotel?" donde la intención es implícita para el restaurante "Paradise".

Puesto que los resultados solo son necesarios para el restaurante "Paradise", puede establecer un filtro en la llamada GenerateAnswer para los metadatos "Nombre del restaurante", tal y como se indica a continuación.

```json
{
    "question": "When does this hotel close?",
    "top": 1,
    "strictFilters": [
      {
        "name": "restaurant",
        "value": "paradise"
      }]
}
```

### <a name="keep-context"></a>Mantener el contexto
La respuesta a GenerateAnswer contiene la información de los metadatos correspondientes al conjunto de preguntas y respuestas que coinciden, de la siguiente manera.

```json
{
    "answers": [
        {
            "questions": [
                "What is the closing time?"
            ],
            "answer": "10.30 PM",
            "score": 100,
            "id": 1,
            "source": "Editorial",
            "metadata": [
                {
                    "name": "restaurant",
                    "value": "paradise"
                },
                {
                    "name": "location",
                    "value": "secunderabad"
                }
            ]
        }
    ]
}
```

Esta información puede utilizarse para registrar el contexto de la conversación anterior para usarlo en conversaciones posteriores. 

## <a name="next-steps"></a>Pasos siguientes

La página de publicación también proporciona información para generar una respuesta [Postman](../Quickstarts/get-answer-from-kb-using-postman.md) y [cURL](../Quickstarts/get-answer-from-kb-using-curl.md). 

> [!div class="nextstepaction"]
> [Creación de una base de conocimientos](./create-knowledge-base.md)
