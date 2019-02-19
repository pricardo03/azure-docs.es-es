---
title: Análisis de sentimiento mediante Text Analytics de Azure Cognitive Services | Microsoft Docs
description: Obtenga información sobre cómo detectar opiniones mediante la API de REST Text Analytics.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: sample
ms.date: 02/13/2019
ms.author: aahi
ms.openlocfilehash: add284a3a001d5bc2e756f9ad4a2a3b9550b212c
ms.sourcegitcommit: b3d74ce0a4acea922eadd96abfb7710ae79356e0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/14/2019
ms.locfileid: "56242142"
---
# <a name="example-how-to-detect-sentiment-with-text-analytics"></a>Ejemplo: Detección de opiniones con Text Analytics

La [API de análisis de sentimiento](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c9) evalúa entradas de texto y devuelve una puntuación de las opiniones para cada documento, de 0 (negativo) a 1 (positivo).

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

El tamaño del documento debe ser inferior a 5000 caracteres por documento y puede tener hasta 1000 elementos (identificadores) por colección. La colección se envía en el cuerpo de la solicitud. En el siguiente ejemplo se presenta contenido que se podría enviar para el análisis de opiniones.

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

+ Cree una solicitud **POST**. Revise la documentación de la API para esta solicitud: [Sentiment Analysis API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c9)

+ Establezca el punto de conexión de HTTP para el análisis de opinión, mediante un recurso de Text Analytics en Azure o un [contenedor de Text Analytics](text-analytics-how-to-install-containers.md) con instancias. Debe incluir el recurso `/sentiment`: `https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/sentiment`

+ Establezca un encabezado de solicitud para incluir la clave de acceso para las operaciones de Text Analytics. Para más información, consulte [How to find endpoints and access keys](text-analytics-how-to-access-key.md) (Cómo buscar puntos de conexión y claves de acceso).

+ En el cuerpo de la solicitud, proporcione la colección de documentos JSON que preparó para este análisis.

> [!Tip]
> Use [Postman](text-analytics-how-to-call-api.md) o abra la **consola de prueba de la API** en la [documentación](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c9) para estructurar y enviar una solicitud POST al servicio.

## <a name="step-2-post-the-request"></a>Paso 2: Publicar la solicitud

El análisis se realiza tras la recepción de la solicitud. El servicio acepta hasta 100 solicitudes por minuto. Cada solicitud puede tener un máximo de 1 MB.

Recuerde que el servicio no tiene estado. No se almacena ningún dato en su cuenta. Los resultados se devuelven inmediatamente en la respuesta.


## <a name="step-3-view-results"></a>Paso 3: Visualización de los resultados

El analizador de opiniones clasifica el texto como predominantemente positivo o negativo, y le asigna una puntuación de 0 a 1. Los valores próximos a 0,5 son neutros o indeterminados. Una puntuación de 0,5 indica neutralidad. Cuando no se pueden analizar las opiniones de una cadena o no tiene ninguna opinión, el resultado siempre es 0,5 exactamente. Por ejemplo, si se pasa una cadena de español con un código de idioma inglés, la puntuación es 0,5.

La salida se devuelve inmediatamente. Puede transmitir los resultados a una aplicación que acepte JSON o guardar la salida en un archivo en el sistema local y, a continuación, importarlo en una aplicación que permita ordenar, buscar y manipular los datos.

En el ejemplo siguiente se muestra la respuesta para la colección de documentos de este artículo.

```
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

En este artículo, ha aprendido los conceptos y el flujo de trabajo del análisis de opiniones mediante Text Analytics de Cognitive Services. En resumen:

+ La [API de análisis de sentimiento](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c9) está disponible para los idiomas seleccionados.
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
