---
title: 'Mejora de una base de conocimiento: QnA Maker'
titleSuffix: Azure Cognitive Services
description: El aprendizaje activo le permite mejorar la calidad de la base de conocimiento al sugerir preguntas alternativas, en función de los envíos del usuario, para el par de preguntas y respuestas. Usted revisa las sugerencias, ya sea al agregarlas a preguntas existentes o rechazarlas. La base de conocimiento no cambia automáticamente. Debe aceptar las sugerencias para cualquier cambio surta efecto. Estas sugerencias agregan preguntas, pero no cambian ni quitan preguntas existentes.
author: diberry
manager: nitinme
services: cognitive-services
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 03/21/2019
ms.author: diberry
ms.openlocfilehash: f59f281b1bf7fa2851ab7759a0167b5d39ef44c1
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59678996"
---
# <a name="use-active-learning-to-improve-your-knowledge-base"></a>Usar el aprendizaje activo para mejorar la base de conocimiento

El aprendizaje activo le permite mejorar la calidad de la base de conocimiento al sugerir preguntas alternativas, en función de los envíos del usuario, para el par de preguntas y respuestas. Usted revisa las sugerencias, ya sea al agregarlas a preguntas existentes o rechazarlas. 

La base de conocimiento no cambia automáticamente. Debe aceptar las sugerencias para que cualquier cambio surta efecto. Estas sugerencias agregan preguntas, pero no cambian ni quitan preguntas existentes.

## <a name="what-is-active-learning"></a>¿Qué es aprendizaje activo?

QnA Maker aprende las nuevas variaciones de las preguntas con comentarios implícitos y explícitos.
 
* Comentarios implícitos: El clasificador entiende cuando la pregunta de un usuario tiene varias respuestas con puntuaciones que son muy similares y considera esto como un comentario. 
* Comentarios explícitos: Cuando se devuelven desde la base de conocimiento varias respuestas con poca variación en las puntuaciones, la aplicación cliente le pregunta al usuario cuál es la pregunta correcta. Los comentarios explícitos del usuario se envían a QnA Maker con la API Train. 

Cualquiera de estos métodos proporciona al clasificador consultas similares que se agrupan en clústeres.

## <a name="how-active-learning-works"></a>Cómo funciona el aprendizaje activo

El aprendizaje activo se desencadena en función de las puntuaciones de las mejores respuestas devueltas por QnA Maker para una consulta concreta. Si las diferencias de puntuación se encuentran dentro de un intervalo pequeño, la consulta se considera una posible _sugerencia_ para cada una de las respuestas posibles. 

Todas las sugerencias se agrupan en clústeres por similitud, y las sugerencias principales para preguntas alternativas se muestran según la frecuencia de las consultas en particular por los usuarios finales. El aprendizaje activo ofrece las mejores sugerencias posibles en los casos donde los puntos de conexión reciben una cantidad razonable y una variedad de consultas de uso.

Cuando las consultas de 5 o más similares se agrupan en clústeres, cada 30 minutos, QnA Maker sugiere las preguntas basadas en usuario para el Diseñador de la base de conocimiento para aceptar o rechazar.

Una vez que se sugieren preguntas en el portal de QnA Maker, deberá revisar y Aceptar o rechazar las sugerencias. 

## <a name="upgrade-your-version-to-use-active-learning"></a>Actualizar la versión para usar el aprendizaje activo

El aprendizaje activo se admite en el tiempo de ejecución versión 4.4.0 y superior. Si la base de conocimiento se creó en una versión anterior, [actualice el tiempo de ejecución](troubleshooting-runtime.md#how-to-get-latest-qnamaker-runtime-updates) para usar esta característica. 

## <a name="best-practices"></a>Procedimientos recomendados

Para los procedimientos recomendados al usar el aprendizaje activo, consulte [Procedimientos recomendados](../Concepts/best-practices.md#active-learning).

## <a name="score-proximity-between-knowledge-base-questions"></a>Proximidad de puntuación entre preguntas de la base de conocimiento

Cuando la puntuación de una pregunta tiene una confianza alta, por ejemplo, un 80 %, el intervalo de puntuaciones que se considera para el aprendizaje activo es amplio, dentro del 10 % aproximadamente. A medida que la puntuación de confianza se reduce, por ejemplo, un 40 %, el intervalo de puntuaciones también disminuye aproximadamente dentro del 4 %. 

El algoritmo para determinar la proximidad no es un cálculo sencillo. Los intervalos en los ejemplos anteriores no están diseñados para ser fijos, pero deben usarse como guía para comprender el impacto del algoritmo únicamente.

## <a name="turn-on-active-learning"></a>Activación del aprendizaje activo

El aprendizaje activo está desactivado de forma predeterminada. Actívelo para ver preguntas sugeridas. 

1. Seleccione **publicar** para publicar la base de conocimiento. Las consultas de aprendizaje activo se recopilan desde el extremo de API GenerateAnswer predicción sólo. Las consultas en el panel de prueba en el portal de Qna Maker no afectará el aprendizaje activo.

1. Para activar el aprendizaje activo, haga clic en su **Nombre** y vaya a [**Configuración del servicio**](https://www.qnamaker.ai/UserSettings) en el portal de QnA Maker, en la esquina superior derecha.  

    ![Activar alternativas de pregunta sugerido del aprendizaje activo en la página de configuración de servicio. Seleccione su nombre de usuario en el menú superior derecho y luego seleccione la configuración del servicio.](../media/improve-knowledge-base/Endpoint-Keys.png)


1. Busque el servicio QnA Maker, y active **Aprendizaje activo**. 

    [![En la página de configuración de servicio, activar o desactivar en función de aprendizaje activo. Si no es capaz de activar o desactivar la característica, deberá actualizar el servicio.](../media/improve-knowledge-base/turn-active-learning-on-at-service-setting.png)](../media/improve-knowledge-base/turn-active-learning-on-at-service-setting.png#lightbox)

    Una vez que **Aprendizaje activo** esté habilitado, el conocimiento sugiere nuevas preguntas a intervalos regulares según las preguntas enviadas por el usuario. Para deshabilitar **Aprendizaje activo**, vuelva a cambiar la configuración.

## <a name="add-active-learning-suggestion-to-knowledge-base"></a>Adición de sugerencias de aprendizaje activo a la base de conocimiento

1. Para ver las preguntas sugeridas, en la página **Editar** de la base de conocimiento, seleccione **Mostrar sugerencias**. 

    [![En la sección Edición del portal, seleccione Mostrar sugerencias para ver nuevas alternativas de pregunta del aprendizaje activo.](../media/improve-knowledge-base/show-suggestions-button.png)](../media/improve-knowledge-base/show-suggestions-button.png#lightbox)

1. Filtrar la base de conocimiento con pares de preguntas y respuestas para mostrar solo las sugerencias seleccionando **filtrar por sugerencias**.

    [![Utilice el filtro de alternar sugerencias para ver solo alternativas de pregunta sugerido del aprendizaje activo.](../media/improve-knowledge-base/filter-by-suggestions.png)](../media/improve-knowledge-base/filter-by-suggestions.png#lightbox)

1.  Cada sección de preguntas con sugerencias muestra la nueva pregunta con una marca de verificación, `✔`, para aceptar la pregunta o una `x` para rechazar las sugerencias. Seleccione la marca de verificación para agregar la pregunta. 

    [![Seleccione o rechazar las alternativas de pregunta sugerido del aprendizaje activo seleccionando la marca de verificación verde o la marca de eliminación de color rojo.](../media/improve-knowledge-base/accept-active-learning-suggestions.png)](../media/improve-knowledge-base/accept-active-learning-suggestions.png#lightbox)

    Puede agregar o eliminar _todas las sugerencias_ seleccionando **Agregar todo** o **Rechazar todo**.

1. Seleccione **Save and Train** (Guardar y entrenar) para guardar los cambios en la base de conocimiento.

1. Seleccione **publicar** para permitir que los cambios estén disponibles desde la API GenerateAnswer.

    Cuando las consultas de 5 o más similares se agrupan en clústeres, cada 30 minutos, QnA Maker sugiere las preguntas basadas en usuario para el Diseñador de la base de conocimiento para aceptar o rechazar.

## <a name="determine-best-choice-when-several-questions-have-similar-scores"></a>Determinación de la mejor opción cuando varias preguntas tienen puntuaciones similares

Cuando una pregunta está demasiado cerca de la puntuación de otras preguntas, el desarrollador de aplicaciones cliente puede pedir aclaraciones.

### <a name="use-the-top-property-in-the-generateanswer-request"></a>Uso de la propiedad top en la solicitud GenerateAnswer

Al enviar una pregunta a QnA Maker para obtener una respuesta, parte del cuerpo JSON permite devolver más de una respuesta principal:

```json
{
    "question": "wi-fi",
    "isTest": false,
    "top": 3
}
```

Cuando la aplicación cliente (por ejemplo, un bot de chat) recibe la respuesta, se devuelven las 3 preguntas principales:

```json
{
    "answers": [
        {
            "questions": [
                "Wi-Fi Direct Status Indicator"
            ],
            "answer": "**Wi-Fi Direct Status Indicator**\n\nStatus bar icons indicate your current Wi-Fi Direct connection status:  \n\nWhen your device is connected to another device using Wi-Fi Direct, '$  \n\n+ •+ ' Wi-Fi Direct is displayed in the Status bar.",
            "score": 74.21,
            "id": 607,
            "source": "Bugbash KB.pdf",
            "metadata": []
        },
        {
            "questions": [
                "Wi-Fi - Connections"
            ],
            "answer": "**Wi-Fi**\n\nWi-Fi is a term used for certain types of Wireless Local Area Networks (WLAN). Wi-Fi communication requires access to a wireless Access Point (AP).",
            "score": 74.15,
            "id": 599,
            "source": "Bugbash KB.pdf",
            "metadata": []
        },
        {
            "questions": [
                "Turn Wi-Fi On or Off"
            ],
            "answer": "**Turn Wi-Fi On or Off**\n\nTurning Wi-Fi on makes your device able to discover and connect to compatible in-range wireless APs.  \n\n1.  From a Home screen, tap ::: Apps > e Settings .\n2.  Tap Connections > Wi-Fi , and then tap On/Off to turn Wi-Fi on or off.",
            "score": 69.99,
            "id": 600,
            "source": "Bugbash KB.pdf",
            "metadata": []
        }
    ]
}
```

### <a name="client-application-follow-up-when-questions-have-similar-scores"></a>Seguimiento de la aplicación cliente cuando las preguntas tienen puntuaciones similares

La aplicación cliente muestra todas las preguntas con una opción para que el usuario seleccione la pregunta que mejor representa su intención. 

Una vez que el usuario selecciona una de las preguntas existentes, la aplicación cliente envía la elección del usuario como comentarios mediante la API de QnA Maker "Train". Estos comentarios completa la activa el bucle de comentarios de aprendizaje. 

Use la [ejemplo Azure Bot](https://aka.ms/activelearningsamplebot) para ver el aprendizaje activo en un escenario de extremo a otro.

## <a name="train-api"></a>API de "Train"

Comentarios de aprendizaje activo se envían a QnA Maker con la solicitud POST de la API de "Train". La firma de la API es:

```http
POST https://<QnA-Maker-resource-name>.azurewebsites.net/qnamaker/knowledgebases/<knowledge-base-ID>/train
Authorization: EndpointKey <endpoint-key>
Content-Type: application/json
{"feedbackRecords": [{"userId": "1","userQuestion": "<question-text>","qnaId": 1}]}
```

|Propiedad de la solicitud HTTP|NOMBRE|Type|Propósito|
|--|--|--|--|
|Parámetro de ruta de dirección URL|Id. de base de conocimiento|string|GUID de Knowledge Base.|
|Subdominio de host|Nombre de recurso de QnAMaker|string|El nombre de host para su QnA Maker en su suscripción de Azure. Esto está disponible en la página de configuración después de publicar la base de conocimiento. |
|Encabezado|Content-Type|string|tipo de soporte del cuerpo enviado a la API. Valor predeterminado es: `application/json`|
|Encabezado|Autorización|string|su clave de punto de conexión (EndpointKey xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx).|
|Cuerpo de POST|Objeto JSON|JSON|Los comentarios de entrenamiento|

El cuerpo JSON tiene varias opciones:

|Propiedad de cuerpo JSON|Type|Propósito|
|--|--|--|--|
|`feedbackRecords`|array|Lista de comentarios.|
|`userId`|string|El identificador de usuario de la persona que acepta las preguntas sugeridas. El formato de Id. de usuario es decisión suya. Por ejemplo, una dirección de correo electrónico puede ser un identificador de usuario válido en la arquitectura. Opcional.|
|`userQuestion`|string|Texto exacto de la pregunta. Necesario.|
|`qnaID`|número|Id. de la pregunta, se encuentra en la [GenerateAnswer respuesta](metadata-generateanswer-usage.md#generateanswer-response-properties). |

Un cuerpo JSON de ejemplo tiene el siguiente aspecto:

```json
{
    "feedbackRecords": [
        {
            "userId": "1",
            "userQuestion": "<question-text>",
            "qnaId": 1
        }
    ]
}
```

Una respuesta correcta devuelve el estado de 204 y ningún cuerpo de respuesta JSON. 

<a name="active-learning-is-saved-in-the-exported-apps-tsv-file"></a>

## <a name="active-learning-is-saved-in-the-exported-knowledge-base"></a>Aprendizaje activo se guarda en la base de conocimiento exportada

Cuando la aplicación tiene el aprendizaje activo habilitado y exportar la aplicación, la `SuggestedQuestions` columna en el archivo tsv conserva los datos de aprendizaje activo. 

El `SuggestedQuestions` columna es un objeto JSON de la información de implícito (`autosuggested`) y explícita (`usersuggested`) comentarios. Un ejemplo de este objeto JSON para una sola pregunta enviado por un usuario de `help` es:

```JSON
[
    {
        "clusterHead": "help",
        "totalAutoSuggestedCount": 1,
        "totalUserSuggestedCount": 0,
        "alternateQuestionList": [
            {
                "question": "help",
                "autoSuggestedCount": 1,
                "userSuggestedCount": 0
            }
        ]
    }
]
```

Cuando se vuelva a importar esta aplicación, continúa el aprendizaje activo recopilar información y sugerencias para la base de conocimiento de recomendar. 

## <a name="next-steps"></a>Pasos siguientes
 
> [!div class="nextstepaction"]
> [Uso de metadatos con API de GenerateAnswer](metadata-generateanswer-usage.md)
