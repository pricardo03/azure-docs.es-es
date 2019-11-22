---
title: Realización de análisis de opiniones con Text Analytics API de REST
titleSuffix: Azure Cognitive Services
description: En este artículo se muestra cómo detectar opiniones en texto con la API de REST de Azure Cognitive Services Text Analytics.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: sample
ms.date: 09/23/2019
ms.author: aahi
ms.openlocfilehash: f7a62d1d2b695726421ba990d0f118eb3ae199da
ms.sourcegitcommit: 49cf9786d3134517727ff1e656c4d8531bbbd332
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2019
ms.locfileid: "74031459"
---
# <a name="example-detect-sentiment-with-text-analytics"></a>Ejemplo: Detección de sentimiento con Text Analytics

[Azure Sentiment Analysis API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c9) evalúa el texto que se escribe y devuelve una puntuación de las opiniones de cada documento. Las puntuaciones oscilan entre 0 (negativo) y 1 (positivo).

Esta funcionalidad es útil para detectar opiniones positivas y negativas en redes sociales, revisiones de los clientes y foros de discusión. El contenido lo proporciona el usuario. Los modelos y los datos de entrenamiento los proporciona el servicio.

Actualmente, Sentiment Analysis API admite inglés, alemán, español y francés. Otros idiomas están en versión preliminar. Para más información, consulte [Lenguajes admitidos](../text-analytics-supported-languages.md).

> [!TIP]
> Azure Text Analytics API también proporciona una imagen de contenedor de Docker basada en Linux para el análisis de sentimiento, por lo que se puede [instalar y ejecutar el contenedor de Text Analytics](text-analytics-how-to-install-containers.md) cerca de los datos.

## <a name="concepts"></a>Conceptos

Text Analytics utiliza un algoritmo de clasificación de aprendizaje automático para generar una puntuación de las opiniones entre 0 y 1. Las puntuaciones cercanas a 1 indican una opinión positiva y las puntuaciones cercanas a 0 indican una opinión negativa. El modelo se entrena previamente con un cuerpo de texto amplio con las asociaciones de opinión. Actualmente, no puede proporcionar sus propios datos de entrenamiento. El modelo utiliza una combinación de técnicas durante el análisis de texto. Dichas técnicas incluyen el procesamiento de texto, el análisis de funciones de sintaxis, la colocación de palabras y las asociaciones de palabras. Para más información acerca del algoritmo, consulte la [introducción a Text Analytics](https://blogs.technet.microsoft.com/machinelearning/2015/04/08/introducing-text-analytics-in-the-azure-ml-marketplace/).

El análisis de las opiniones se realiza en todo el documento, en lugar de extraer la opinión de una entidad determinada en el texto. En la práctica, se observa una tendencia de mejora de la puntuación cuando los documentos contienen una o dos frases, en lugar de un bloque grande de texto. Durante la fase de evaluación de la objetividad, el modelo determina si un documento completo es objetivo o contiene opiniones. Un documento que es mayoritariamente objetivo no pasa a la fase de detección de opiniones, lo que genera una puntuación de 0,5 y detiene el procesamiento. En el caso de los documentos que continúan en la canalización, la siguiente fase genera una puntuación por encima o por debajo de 0,50. Dicha puntuación depende del grado de opinión detectado en el documento.

## <a name="sentiment-analysis-v3-public-preview"></a>Versión preliminar pública de la versión 3 de Análisis de sentimiento

La [siguiente versión de Análisis de sentimiento](https://cognitiveusw2ppe.portal.azure-api.net/docs/services/TextAnalytics-v3-0-Preview-1/operations/Sentiment) ahora está disponible en versión preliminar pública. Dicha versión incluye importantes mejoras significativas en la precisión y el detalle de la categorización del texto y la puntuación de la API.

> [!NOTE]
> * El formato de las solicitudes de la versión 3 de Análisis de sentimiento y los [límites de datos](../overview.md#data-limits) son los mismos que en la versión anterior.
> * En este momento, la versión 3 de Análisis de sentimiento:
>    * Actualmente admite inglés (`en`), japonés (`ja`), chino simplificado (`zh-Hans`), chino tradicional (`zh-Hant`), francés (`fr`), italiano (`it`), español (`es`), neerlandés (`nl`), portugués (`pt`) y alemán (`de`).
>    * Disponible en las siguientes regiones: `Australia East`, `Central Canada`, `Central US`, `East Asia`, `East US`, `East US 2`, `North Europe`, `Southeast Asia`, `South Central US`, `UK South`, `West Europe` y `West US 2`.

|Característica |DESCRIPCIÓN  |
|---------|---------|
|Precisión mejorada     | Mejora significativa en la detección de opiniones positivas, neutras, negativas y mixtas en documentos de texto en comparación con versiones anteriores.           |
|Puntuación de opiniones a nivel de frases y de documentos     | Detecte la opinión de un documento y sus frases individuales. Si el documento incluye varias frases, a cada frase también se le asigna una puntuación de opiniones.         |
|Etiquetado de opiniones y puntuación     | La API ahora devuelve categorías de opiniones para texto, además de una puntuación de opiniones. Las categorías son `positive` `negative`,`neutral` y `mixed`.       |
| Salida mejorada | Análisis de sentimiento ahora devuelve información tanto de documentos de texto enteros como de sus frases individuales. |
| Parámetro model-version | Un parámetro opcional para elegir qué versión del modelo de Text Analytics se usa en los datos. |

### <a name="sentiment-labeling"></a>Etiquetado de opiniones

La versión 3 de Análisis de sentimiento puede devolver las puntuaciones y etiquetas tanto a nivel de documento como a nivel de frase. Las puntuaciones y las etiquetas son `positive`, `negative`, y `neutral`. En el nivel de documento, la etiqueta de opinión `mixed` (no de puntuación) también se puede devolver. La opinión del documento se determina sumando las puntuaciones de las frases.

| Opiniones de frases                                                        | Etiqueta de documento devuelta |
|---------------------------------------------------------------------------|----------------|
| Al menos una frase positiva y el resto de las frases son neutrales. | `positive`     |
| Al menos una frase negativa y el resto de las frases son neutrales.  | `negative`     |
| Al menos una frase negativa y al menos una frase positiva.         | `mixed`        |
| Todas las frases son neutrales.                                                 | `neutral`      |

### <a name="model-versioning"></a>Control de versiones de los modelos

> [!NOTE]
> El control de versiones del modelo para el análisis de opiniones está disponible a partir de la versión `v3.0-preview.1`.

[!INCLUDE [v3-model-versioning](../includes/model-versioning.md)]

### <a name="sentiment-analysis-v3-example-request"></a>Solicitud de ejemplo de la versión 3 de Análisis de sentimiento

El siguiente código de JSON un ejemplo de una solicitud realizada a la nueva versión de Análisis de sentimiento. El formato de la solicitud es el mismo que en la versión anterior:

```json
    {
        "documents": [
        {
            "language": "en",
            "id": "1",
            "text": "Hello world. This is some input text that I love."
        },
        {
            "language": "en",
            "id": "2",
            "text": "It's incredibly sunny outside! I'm so happy."
        }
        ],
    }
```

### <a name="sentiment-analysis-v3-example-response"></a>Respuesta de ejemplo de la versión 3 de Análisis de sentimiento

Aunque el formato de la solicitud es el mismo que el de la versión anterior, el formato de respuesta ha cambiado. El siguiente JSON es un ejemplo de la respuesta de la nueva versión de la API:

```json
    {
        "documents": [
            {
                "id": "1",
                "sentiment": "positive",
                "documentScores": {
                    "positive": 0.98570585250854492,
                    "neutral": 0.0001625834556762,
                    "negative": 0.0141316400840878
                },
                "sentences": [
                    {
                        "sentiment": "neutral",
                        "sentenceScores": {
                            "positive": 0.0785155147314072,
                            "neutral": 0.89702343940734863,
                            "negative": 0.0244610067456961
                        },
                        "offset": 0,
                        "length": 12
                    },
                    {
                        "sentiment": "positive",
                        "sentenceScores": {
                            "positive": 0.98570585250854492,
                            "neutral": 0.0001625834556762,
                            "negative": 0.0141316400840878
                        },
                        "offset": 13,
                        "length": 36
                    }
                ]
            },
            {
                "id": "2",
                "sentiment": "positive",
                "documentScores": {
                    "positive": 0.89198976755142212,
                    "neutral": 0.103382371366024,
                    "negative": 0.0046278294175863
                },
                "sentences": [
                    {
                        "sentiment": "positive",
                        "sentenceScores": {
                            "positive": 0.78401315212249756,
                            "neutral": 0.2067587077617645,
                            "negative": 0.0092281140387058
                        },
                        "offset": 0,
                        "length": 30
                    },
                    {
                        "sentiment": "positive",
                        "sentenceScores": {
                            "positive": 0.99996638298034668,
                            "neutral": 0.0000060341349126,
                            "negative": 0.0000275444017461
                        },
                        "offset": 31,
                        "length": 13
                    }
                ]
            }
        ],
        "errors": []
    }
```

### <a name="example-c-code"></a>Ejemplo de código C#

Puede encontrar un ejemplo de aplicación C# que llama a esta versión dl Análisis de sentimiento en [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/tree/master/dotnet/Language/SentimentV3.cs).

## <a name="preparation"></a>Preparación

El análisis de sentimiento genera un resultado de mayor calidad cuando se le proporcionan fragmentos de texto más pequeños para trabajar. Esto es opuesto a la extracción de frases clave, que funciona mejor en bloques de texto más grandes. Para obtener los mejores resultados de ambas operaciones, considere la posibilidad de reestructurar las entradas en consecuencia.

Debe tener documentos JSON en este formato: identificador, texto e idioma.

El tamaño del documento debe ser inferior a 5120 caracteres por documento. Puede tener hasta 1000 elementos (identificadores) por colección. La colección se envía en el cuerpo de la solicitud. En el siguiente ejemplo se presenta contenido que se podría enviar para el análisis de sentimiento:

```json
    {
        "documents": [
            {
                "language": "en",
                "id": "1",
                "text": "We love this trail and make the trip every year. The views are breathtaking and well worth the hike!"
            },
            {
                "language": "en",
                "id": "2",
                "text": "Poorly marked trails! I thought we were goners. Worst hike ever."
            },
            {
                "language": "en",
                "id": "3",
                "text": "Everyone in my family liked the trail but thought it was too challenging for the less athletic among us. Not necessarily recommended for small children."
            },
            {
                "language": "en",
                "id": "4",
                "text": "It was foggy so we missed the spectacular views, but the trail was ok. Worth checking out if you are in the area."
            },
            {
                "language": "en",
                "id": "5",
                "text": "This is my favorite trail. It has beautiful views and many places to stop and rest"
            }
        ]
    }
```

## <a name="step-1-structure-the-request"></a>Paso 1: Estructurar la solicitud

Para obtener más información sobre la definición de la solicitud, consulte [Llamada a la API de Text Analytics](text-analytics-how-to-call-api.md). Recapitulamos los siguientes puntos para su comodidad:

+ Cree una solicitud POST. Para consultar la documentación de la API para esta solicitud, vea [Sentiment Analysis API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c9).

+ Establezca el punto de conexión de HTTP para el análisis de sentimiento mediante un recurso de Text Analytics en Azure o un [contenedor de Text Analytics](text-analytics-how-to-install-containers.md) con instancias. Debe incluir `/text/analytics/v2.1/sentiment` en la dirección URL. Por ejemplo: `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v2.1/sentiment`.

+ Establezca un encabezado de solicitud para incluir la [clave de acceso](../../cognitive-services-apis-create-account.md#get-the-keys-for-your-resource) para las operaciones de Text Analytics.

+ En el cuerpo de la solicitud, proporcione la colección de documentos JSON que preparó para este análisis.

> [!Tip]
> Use [Postman](text-analytics-how-to-call-api.md) o abra la **consola de pruebas de la API** en la [documentación](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c9) para estructurar la solicitud y enviarla al servicio.

## <a name="step-2-post-the-request"></a>Paso 2: Publicar la solicitud

El análisis se realiza tras la recepción de la solicitud. Para obtener información sobre el tamaño y el número de solicitudes que puede enviar por minuto y segundo, consulte la sección de [límites de datos](../overview.md#data-limits) de la introducción.

Recuerde que el servicio no tiene estado. No se almacena ningún dato en su cuenta. Los resultados se devuelven inmediatamente en la respuesta.


## <a name="step-3-view-the-results"></a>Paso 3: View the results

El analizador de sentimiento clasifica el texto como predominantemente positivo o negativo. Asigna una puntuación en el rengo que oscila entre 0 y 1. Los valores próximos a 0,5 son neutros o indeterminados. Una puntuación de 0,5 indica neutralidad. Cuando el sentimiento de una cadena no se puede analizar o no tiene sentimiento, la puntuación es siempre 0,5. Por ejemplo, si se pasa una cadena de español con un código de idioma inglés, la puntuación es 0,5.

La salida se devuelve inmediatamente. Puede transmitir los resultados a una aplicación que acepte JSON o guardar la salida en un archivo en el sistema local. Después, importe el resultado en una aplicación que pueda usar para ordenar los datos, realizar búsquedas en ellos y manipularlos.

En el ejemplo siguiente se muestra la respuesta para la colección de documentos de este artículo:

```json
    {
        "documents": [
            {
                "score": 0.9999237060546875,
                "id": "1"
            },
            {
                "score": 0.0000540316104888916,
                "id": "2"
            },
            {
                "score": 0.99990355968475342,
                "id": "3"
            },
            {
                "score": 0.980544924736023,
                "id": "4"
            },
            {
                "score": 0.99996328353881836,
                "id": "5"
            }
        ],
        "errors": []
    }
```

## <a name="summary"></a>Resumen

En este artículo, ha aprendido los conceptos y el flujo de trabajo del análisis de sentimiento mediante Text Analytics de Azure Cognitive Services. En resumen:

+ [Sentiment Analysis API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c9) está disponible para determinados idiomas.
+ Los documentos JSON del cuerpo de la solicitud incluyen un identificador, un texto y el código de idioma.
+ La solicitud POST se realiza a un punto de conexión `/sentiment`, mediante una [clave de acceso y un punto de conexión](../../cognitive-services-apis-create-account.md#get-the-keys-for-your-resource) personalizados, que sean válidos para la suscripción.
+ La salida de la respuesta, que consta de una puntuación de las opiniones de cada identificador de documento, se puede transmitir en secuencias a cualquier aplicación que acepte JSON. Entre las aplicaciones de ejemplo se incluyen Excel y Power BI, por nombrar algunas.

## <a name="see-also"></a>Otras referencias

 [Introducción a Text Analytics](../overview.md) [Preguntas más frecuentes](../text-analytics-resource-faq.md)</br>
 [Página del producto de Text Analytics](//go.microsoft.com/fwlink/?LinkID=759712)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Extracción de frases clave](text-analytics-how-to-keyword-extraction.md)
