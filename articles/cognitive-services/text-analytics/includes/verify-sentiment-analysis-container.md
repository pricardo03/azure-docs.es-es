---
title: Compatibilidad con los contenedores
titleSuffix: Azure Cognitive Services
description: Obtenga información sobre cómo comprobar la instancia del contenedor de análisis de opinión.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 06/26/2019
ms.author: dapine
ms.openlocfilehash: f68d9c7098f2b1ca782e2522c632c2e267b35336
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2019
ms.locfileid: "67455115"
---
## <a name="verify-the-sentiment-analysis-container-instance"></a>Comprobar la instancia del contenedor de análisis de sentimiento

1. Seleccione la pestaña de **información general** y copie la dirección IP.
1. Abra una nueva pestaña del explorador y use la dirección IP; por ejemplo, `http://<IP-address>:5000 (http://55.55.55.55:5000`). Verá la página principal del contenedor, que le permitirá saber que el contenedor se está ejecutando.

    ![Ver la página principal de contenedor para comprobar que se está ejecutando](../media/how-tos/container-instance/swagger-docs-on-container.png)

1. Seleccione el vínculo de **descripción de la API de servicio** para ir a la página de contenedores de Swagger.

1. Elija cualquiera de las API **POST** y seleccione **Probar**.  Se muestran los parámetros junto con la entrada de ejemplo.

    ```json
    {
      "documents": [
        {
          "language": "en",
          "id": "1",
          "text": "Hello world. This is some input text that I love."
        },
        {
          "language": "fr",
          "id": "2",
          "text": "Bonjour tout le monde"
        },
        {
          "language": "es",
          "id": "3",
          "text": "La carretera estaba atascada. Había mucho tráfico el día de ayer."
        }
      ]
    }
    ```

1. Reemplace la entrada con el código JSON siguiente:

    ```json
    {
      "documents": [
        {
          "language": "en",
          "id": "7",
          "text": "I was fortunate to attend the KubeCon Conference in Barcelona, it is one of the best conferences I have ever attended. Great people, great sessions and I thoroughly enjoyed it!"
        }
      ]
    }
    ```

1. Establezca **showStats** en "true".

1. Seleccione **Execute** (Ejecutar) para determinar la opinión del texto.

    El modelo de empaquetado en el contenedor genera una puntuación comprendida entre 0 y 1, donde 0 es negativo y 1 es positivo.

    La respuesta JSON devuelta incluye las opiniones de la entrada de texto actualizado:

    ```json
    {
      "documents": [
      {
        "id": "7",
        "score": 0.9826303720474243,
        "statistics": {
          "charactersCount": 176,
            "transactionsCount": 1
          }
        }
      ],
      "errors": [],
      "statistics": {
        "documentsCount": 1,
        "validDocumentsCount": 1,
        "erroneousDocumentsCount": 0,
        "transactionsCount": 1
      }
    }
    ```

Ahora podemos correlacionar el documento `id` de la carga útil de respuesta JSON con el documento de la carga útil de solicitud original `id`, y comprobar que hubo una puntuación de más de `.98` que indica una opinión muy positiva.