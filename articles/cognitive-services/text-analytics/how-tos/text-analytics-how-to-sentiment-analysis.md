---
title: Análisis de sentimiento mediante Text Analytics de Azure Cognitive Services | Microsoft Docs
description: Obtenga información sobre cómo detectar opiniones mediante la API de REST Text Analytics.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: sample
ms.date: 02/26/2019
ms.author: aahi
ms.openlocfilehash: e17b68dfd63952d0c8c81415b090b047c5808e2e
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2019
ms.locfileid: "67797802"
---
# <a name="example-how-to-detect-sentiment-with-text-analytics"></a>Ejemplo: Detección de opiniones con Text Analytics

La [API de análisis de sentimiento](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c9) evalúa entradas de texto y devuelve una puntuación de las opiniones para cada documento, de 0 (negativo) a 1 (positivo).

Esta funcionalidad es útil para detectar opiniones positivas y negativas en redes sociales, revisiones de los clientes y foros de discusión. El contenido lo proporciona el usuario; los modelos y los datos de entrenamiento los proporciona el servicio.

Actualmente, Análisis de sentimiento admite inglés, alemán, español y francés. Otros idiomas están en versión preliminar. Para más información, consulte [Lenguajes admitidos](../text-analytics-supported-languages.md).

> [!TIP]
> Text Analytics proporciona también una imagen de contenedor de Docker basada en Linux para el análisis de opinión, por lo que puede [instalar y ejecutar el contenedor de Text Analytics](text-analytics-how-to-install-containers.md) cerca de los datos.

## <a name="concepts"></a>Conceptos

Text Analytics utiliza un algoritmo de clasificación de aprendizaje automático para generar una puntuación de las opiniones entre 0 y 1. Las puntuaciones cercanas a 1 indican una opinión positiva y las puntuaciones cercanas a 0 indican una opinión negativa. El modelo se entrena previamente con un cuerpo de texto amplio con las asociaciones de opinión. Actualmente, no puede proporcionar sus propios datos de entrenamiento. El modelo usa una combinación de técnicas durante el análisis de texto, incluido el procesamiento de texto, análisis de las categorías gramaticales, ubicación de las palabras y las asociaciones de palabras. Para más información acerca del algoritmo, consulte la [introducción a Text Analytics](https://blogs.technet.microsoft.com/machinelearning/2015/04/08/introducing-text-analytics-in-the-azure-ml-marketplace/).

El análisis de las opiniones se realiza en todo el documento, en lugar de extraer la opinión de una entidad determinada en el texto. En la práctica, se observa una mejoría de la puntuación cuando los documentos incluyen una o dos frases en lugar de un bloque grande de texto. Durante la fase de evaluación de la objetividad, el modelo determina si un documento completo es objetivo o contiene opiniones. Un documento que es mayoritariamente objetivo no continúa a la fase de detección de opiniones, lo que genera una puntuación de 0,50 y no se sigue procesando. En el caso de los documentos que continúan en la canalización, la siguiente fase genera una puntuación superior o inferior a 0,50, según el grado de opiniones detectado en el documento.

## <a name="preparation"></a>Preparación

El análisis de las opiniones genera un resultado de mayor calidad cuando se le proporcionan fragmentos de texto más pequeños para trabajar. Esto es opuesto a la extracción de frases clave, que funciona mejor en bloques de texto más grandes. Para obtener los mejores resultados de ambas operaciones, considere la posibilidad de reestructurar las entradas en consecuencia.

Debe tener documentos JSON en este formato: identificador, texto, idioma

El tamaño del documento debe ser inferior a 5120 caracteres por documento y puede tener hasta 1000 elementos (identificadores) por colección. La colección se envía en el cuerpo de la solicitud. En el siguiente ejemplo se presenta contenido que se podría enviar para el análisis de opiniones.

```
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

Se pueden encontrar detalles sobre la definición de la solicitud en [How to call the Text Analytics API](text-analytics-how-to-call-api.md) (Cómo llamar a Text Analytics API). Recapitulamos los siguientes puntos para su comodidad:

+ Cree una solicitud **POST**. Revise la documentación de la API para esta solicitud: [Sentiment Analysis API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c9)

+ Establezca el punto de conexión de HTTP para el análisis de opinión, mediante un recurso de Text Analytics en Azure o un [contenedor de Text Analytics](text-analytics-how-to-install-containers.md) con instancias. Debe incluir el recurso `/sentiment`: `https://westus.api.cognitive.microsoft.com/text/analytics/v2.1/sentiment`

+ Establezca un encabezado de solicitud para incluir la clave de acceso para las operaciones de Text Analytics. Para más información, consulte [How to find endpoints and access keys](text-analytics-how-to-access-key.md) (Cómo buscar puntos de conexión y claves de acceso).

+ En el cuerpo de la solicitud, proporcione la colección de documentos JSON que preparó para este análisis.

> [!Tip]
> Use [Postman](text-analytics-how-to-call-api.md) o abra la **consola de prueba de la API** en la [documentación](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c9) para estructurar y enviar una solicitud POST al servicio.

## <a name="step-2-post-the-request"></a>Paso 2: Publicar la solicitud

El análisis se realiza tras la recepción de la solicitud. Consulte la sección de [límites de datos](../overview.md#data-limits) de la introducción para obtener información sobre el tamaño y el número de solicitudes que puede enviar por minuto y segundo.

Recuerde que el servicio no tiene estado. No se almacena ningún dato en su cuenta. Los resultados se devuelven inmediatamente en la respuesta.


## <a name="step-3-view-results"></a>Paso 3: Visualización de los resultados

El analizador de opiniones clasifica el texto como predominantemente positivo o negativo, y le asigna una puntuación de 0 a 1. Los valores próximos a 0,5 son neutros o indeterminados. Una puntuación de 0,5 indica neutralidad. Cuando no se pueden analizar las opiniones de una cadena o no tiene ninguna opinión, el resultado siempre es 0,5 exactamente. Por ejemplo, si se pasa una cadena de español con un código de idioma inglés, la puntuación es 0,5.

La salida se devuelve inmediatamente. Puede transmitir los resultados a una aplicación que acepte JSON o guardar la salida en un archivo en el sistema local y, a continuación, importarlo en una aplicación que permita ordenar, buscar y manipular los datos.

En el ejemplo siguiente se muestra la respuesta para la colección de documentos de este artículo.

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

## <a name="sentiment-analysis-v3-public-preview"></a>Versión preliminar pública de Análisis de sentimiento V3

La [próxima versión de Análisis de sentimiento](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-0-preview/operations/56f30ceeeda5650db055a3c9) ya está disponible para la versión preliminar pública, que proporciona mejoras significativas en la precisión y el detalle de la categorización y puntuación del texto de la API. 

> [!NOTE]
> * El formato de la solicitud del Análisis de sentimiento v3 y los [límites de datos](../overview.md#data-limits) son los mismos que en la versión anterior.
> * En este momento, Análisis de sentimiento V3: 
>    * Actualmente solo admite el idioma inglés.  
>    * Está disponible en las siguientes regiones: `Central US`, `Central Canada`, `East Asia`. 

|Característica |DESCRIPCIÓN  |
|---------|---------|
|Precisión mejorada     | Mejora significativa en la detección de opiniones positivas, neutras, negativas y mixtas en documentos de texto en comparación con versiones anteriores.           |
|Puntuación de opiniones a nivel de frases y de documentos     | Detecte la opinión de un documento y sus frases individuales. Si el documento incluye varias frases, a cada frase también se le asigna una puntuación de opiniones.         |
|Categoría y puntuación de opiniones     | La API ahora devuelve categorías de opiniones (`positive`, `negative`, `neutral` y `mixed`) para texto, además de una puntuación de opiniones.        |
| Salida mejorada | El Análisis de sentimiento ahora devuelve información tanto para un documento de texto completo como para sus frases individuales. |

### <a name="sentiment-labeling"></a>Etiquetado de opiniones

Análisis de sentimiento V3 puede devolver las puntuaciones y etiquetas (`positive`, `negative` y `neutral`) en un nivel de documento y de frases. En el nivel de documento, la etiqueta de opinión `mixed` (no de puntuación) también se puede devolver. La opinión del documento se determina sumando las puntuaciones de sus frases.

| Opiniones de frases                                                        | Etiqueta de documento devuelta |
|---------------------------------------------------------------------------|----------------|
| Al menos una frase positiva y el resto de las frases son neutrales. | `positive`     |
| Al menos una frase negativa y el resto de las frases son neutrales.  | `negative`     |
| Al menos una frase negativa y al menos una frase positiva.         | `mixed`        |
| Todas las frases son neutrales.                                                 | `neutral`      |

### <a name="sentiment-analysis-v3-example-request"></a>Solicitud de ejemplo de Análisis de sentimiento V3

El siguiente JSON es un ejemplo de una solicitud realizada a la nueva versión del Análisis de sentimiento. Tenga en cuenta que el formato de la solicitud es la misma que la versión anterior:

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
  ]
}
```

### <a name="sentiment-analysis-v3-example-response"></a>Respuesta de ejemplo de Análisis de sentimiento V3

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

Puede encontrar un ejemplo de aplicación C# que llama a esta versión del Análisis de sentimiento en [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/tree/master/dotnet/Language/SentimentV3.cs).

## <a name="summary"></a>Resumen

En este artículo, ha aprendido los conceptos y el flujo de trabajo del análisis de opiniones mediante Text Analytics de Cognitive Services. En resumen:

+ La [API de análisis de sentimiento](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c9) está disponible para los idiomas seleccionados.
+ Los documentos JSON del cuerpo de la solicitud incluyen un identificador, un texto y el código de idioma.
+ La solicitud POST se realiza a un punto de conexión `/sentiment`, con una [clave de acceso y un punto de conexión](text-analytics-how-to-access-key.md) personalizados, que son válidos para la suscripción.
+ La salida de la respuesta, que consta de la puntuación de las opiniones para cada identificador de documento, se puede transmitir a cualquier aplicación que acepte JSON, incluidas Excel y Power BI, por nombrar algunas.

## <a name="see-also"></a>Otras referencias 

 [Información general de Text Analytics](../overview.md)  
 [Preguntas más frecuentes (P+F)](../text-analytics-resource-faq.md)</br>
 [Página del producto de Text Analytics](//go.microsoft.com/fwlink/?LinkID=759712) 

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Extracción de frases clave](text-analytics-how-to-keyword-extraction.md)
