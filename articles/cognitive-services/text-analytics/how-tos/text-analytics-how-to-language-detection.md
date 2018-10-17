---
title: 'Ejemplo: Detección de idioma con la API de REST Text Analytics'
titleSuffix: Azure Cognitive Services
description: Obtenga información sobre cómo detectar el idioma mediante la API de REST Text Analytics.
services: cognitive-services
author: HeidiSteen
manager: cgronlun
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: sample
ms.date: 09/12/2018
ms.author: heidist
ms.openlocfilehash: fa71e4ce2e5cb5967bb583c7314072830de08051
ms.sourcegitcommit: 616e63d6258f036a2863acd96b73770e35ff54f8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/14/2018
ms.locfileid: "45604559"
---
# <a name="example-how-to-detect-language-in-text-analytics"></a>Ejemplo: Cómo detectar el idioma en Text Analytics

[Language Detection API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7) evalúa la entrada de texto y, para cada documento, devuelve identificadores de idioma con una puntuación que indica la solidez del análisis. Text Analytics reconoce hasta 120 idiomas.

Esta capacidad es útil para los almacenes de contenido que recopilan texto arbitrario, donde el idioma es desconocido. Puede analizar los resultados del análisis para determinar el idioma que se usa en el documento de entrada. La respuesta también devuelve una puntuación que refleja la confianza del modelo (un valor entre 0 y 1).

## <a name="preparation"></a>Preparación

Debe tener documentos JSON en este formato: identificador, texto

El tamaño del documento debe ser inferior a 5000 caracteres por documento y puede tener hasta 1000 elementos (ID) por colección. La colección se envía en el cuerpo de la solicitud. En el siguiente ejemplo se presenta contenido que se podría enviar para la detección del idioma.

   ```
    {
        "documents": [
            {
                "id": "1",
                "text": "This document is in English."
            },
            {
                "id": "2",
                "text": "Este documento está en inglés."
            },
            {
                "id": "3",
                "text": "Ce document est en anglais."
            },
            {
                "id": "4",
                "text": "本文件为英文"
            },                
            {
                "id": "5",
                "text": "Этот документ находится на английском языке."
            }
        ]
    }
```

## <a name="step-1-structure-the-request"></a>Paso 1: Estructurar la solicitud

Se pueden encontrar detalles sobre la definición de la solicitud en [How to call the Text Analytics API](text-analytics-how-to-call-api.md) (Cómo llamar a Text Analytics API). Recapitulamos los siguientes puntos para su comodidad:

+ Cree una solicitud **POST**. Revise la documentación de la API para esta solicitud: [Language Detection API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7)

+ Establezca el punto de conexión HTTP para la detección de idioma. Debe incluir el recurso `/languages`: `https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/languages`

+ Establezca un encabezado de solicitud para incluir la clave de acceso para las operaciones de Text Analytics. Para obtener más información, consulte [How to find endpoints and access keys](text-analytics-how-to-access-key.md) (Cómo buscar puntos de conexión y claves de acceso).

+ En el cuerpo de la solicitud, proporcione la colección de documentos JSON que preparó para este análisis.

> [!Tip]
> Use [Postman](text-analytics-how-to-call-api.md) o abra la **consola de prueba de la API** en la [documentación](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7) para estructurar y enviar una solicitud POST al servicio.

## <a name="step-2-post-the-request"></a>Paso 2: Enviar la solicitud

El análisis se realiza tras la recepción de la solicitud. El servicio acepta hasta 100 solicitudes por minuto. Cada solicitud puede tener un máximo de 1 MB.

Recuerde que el servicio no tiene estado. No se almacena ningún dato en su cuenta. Los resultados se devuelven inmediatamente en la respuesta.


## <a name="step-3-view-results"></a>Paso 3: Ver los resultados

Todas las solicitudes POST devolverán una respuesta con formato JSON con los identificadores y las propiedades detectadas.

La salida se devuelve inmediatamente. Puede transmitir los resultados a una aplicación que acepte JSON o guardar la salida en un archivo en el sistema local y, a continuación, importarlo en una aplicación que permita ordenar, buscar y manipular los datos.

Los resultados de la solicitud de ejemplo deben parecerse al siguiente JSON. Tenga en cuenta que es un documento con varios elementos. La salida está disponible en inglés. Los identificadores de idioma incluyen un nombre descriptivo y un código de idioma en formato [ISO 639-1](https://www.iso.org/standard/22109.html).

Una puntuación positiva de 1,0 expresa el nivel más alto de confianza posible del análisis.



```
{
    "documents": [
        {
            "id": "1",
            "detectedLanguages": [
                {
                    "name": "English",
                    "iso6391Name": "en",
                    "score": 1
                }
            ]
        },
        {
            "id": "2",
            "detectedLanguages": [
                {
                    "name": "Spanish",
                    "iso6391Name": "es",
                    "score": 1
                }
            ]
        },
        {
            "id": "3",
            "detectedLanguages": [
                {
                    "name": "French",
                    "iso6391Name": "fr",
                    "score": 1
                }
            ]
        },
        {
            "id": "4",
            "detectedLanguages": [
                {
                    "name": "Chinese_Simplified",
                    "iso6391Name": "zh_chs",
                    "score": 1
                }
            ]
        },
        {
            "id": "5",
            "detectedLanguages": [
                {
                    "name": "Russian",
                    "iso6391Name": "ru",
                    "score": 1
                }
            ]
        }
    ],
```

### <a name="ambiguous-content"></a>Contenido ambiguo

Si el analizador no puede analizar la entrada (por ejemplo, suponga que ha enviado un bloque de texto que se compone únicamente de números arábigos), devuelve `(Unknown)`.

```
    {
      "id": "5",
      "detectedLanguages": [
        {
          "name": "(Unknown)",
          "iso6391Name": "(Unknown)",
          "score": "NaN"
        }
      ]
```
### <a name="mixed-language-content"></a>Contenido en varios idiomas

El contenido en varios idiomas dentro del mismo documento devuelve el idioma con mayor representación en el contenido, pero con una clasificación positiva inferior, que refleja la solidez marginal de esa evaluación. En el ejemplo siguiente, la entrada es una combinación de inglés, español y francés. El analizador cuenta los caracteres de cada segmento para determinar el idioma predominante.

**Entrada**

```
{
  "documents": [
    {
      "id": "1",
      "text": "Hello, I would like to take a class at your University. ¿Se ofrecen clases en español? Es mi primera lengua y más fácil para escribir. Que diriez-vous des cours en français?"
    }
  ]
}
```

**Salida**

La salida resultante está formada por el idioma predominante, con una puntuación de menos de 1,0, que indica un nivel de confianza más débil.

```
{
  "documents": [
    {
      "id": "1",
      "detectedLanguages": [
        {
          "name": "Spanish",
          "iso6391Name": "es",
          "score": 0.9375
        }
      ]
    }
  ],
  "errors": []
}
```

## <a name="summary"></a>Resumen

En este artículo, ha aprendido los conceptos y el flujo de trabajo de la detección del idioma mediante Text Analytics de Cognitive Services. A continuación, se muestra un recordatorio rápido de los puntos principales explicados y mostrados anteriormente:

+ [Language detection API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7) está disponible para 120 idiomas.
+ Los documentos JSON del cuerpo de la solicitud incluyen un identificador y texto.
+ La solicitud POST se realiza a un punto de conexión `/languages`, con una [clave de acceso y un punto de conexión](text-analytics-how-to-access-key.md) personalizados, que son válidos para la suscripción.
+ La salida de la respuesta, que consta de identificadores de idioma para cada id. de documento, se puede transmitir a cualquier aplicación que acepte JSON, incluidas Excel y Power BI, por nombrar algunas.

## <a name="see-also"></a>Otras referencias 

 [Información general de Text Analytics](../overview.md)  
 [Preguntas más frecuentes (P+F)](../text-analytics-resource-faq.md)</br>
 [Página del producto de Text Analytics](//go.microsoft.com/fwlink/?LinkID=759712) 

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Análisis de opinión](text-analytics-how-to-sentiment-analysis.md)
