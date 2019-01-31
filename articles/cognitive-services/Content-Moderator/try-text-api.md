---
title: 'Moderación del texto mediante Text Moderation API: Content Moderator'
titlesuffix: Azure Cognitive Services
description: Pruebe la moderación de texto mediante Text Moderation API en la consola en línea.
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 08/05/2017
ms.author: sajagtap
ms.openlocfilehash: 926c291c3f81214c543e7c9f8ca36285e4e59400
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55226594"
---
# <a name="moderate-text-from-the-api-console"></a>Moderación de texto desde la consola de API

Use [Text Moderation API](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66f) en Azure Content Moderator para examinar el contenido del texto. La operación examina si hay palabras soeces en el contenido y también lo compara con listas negras compartidas y personalizadas.


## <a name="get-your-api-key"></a>Obtener la clave de API
Antes de probar la API en la consola en línea, necesita la clave de suscripción. Se encuentra en la pestaña **Configuración**, en el cuadro **Ocp-Apim-Subscription-Key**. Para más información, consulte [Información general](overview.md).

## <a name="navigate-to-the-api-reference"></a>Navegar hasta la referencia de API
Vaya a la [referencia de Text Moderation API](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66f). 

  Se abre la página **Text - Screen** (Texto: filtrar).

## <a name="open-the-api-console"></a>Abrir la consola de API
En **Open API testing console** (Abrir consola de prueba de API), seleccione la región que mejor describa su ubicación. 

  ![Selección de la región en la página Text - Screen (Texto: filtrar)](images/test-drive-region.png)

  Se abre la consola de API de **Text - Screen** (Texto: filtrar).

## <a name="select-the-inputs"></a>Seleccionar las entradas

### <a name="parameters"></a>Parámetros
Seleccione los parámetros de consulta que quiere usar en la pantalla de texto. En este ejemplo, utilice el valor predeterminado de **language** (idioma). También puede dejarlo en blanco, ya que la operación detectará automáticamente el idioma probable como parte de su ejecución.

> [!NOTE]
> Para el parámetro **language** (idioma), asigne `eng` o déjelo en blanco para ver asistida la respuesta de **classification** (clasificación) asistida por máquina. **Esta característica solo admite inglés**.
>
> Para la detección de **profanity terms** (palabras soeces), use el [código ISO 639-3](http://www-01.sil.org/iso639-3/codes.asp) de los idiomas admitidos que se enumeran en este artículo, o deje el campo en blanco.

Para **autocorrect** (autocorrección), **PII** (DCP) y **classify (preview)** [clasificar (versión preliminar)], seleccione **true**. Deje el campo **ListId** vacío.

  ![Parámetros de la consulta de la consola Text - Screen (Texto: filtrar)](images/text-api-console-inputs.PNG)

### <a name="content-type"></a>Tipo de contenido
Para **Content-Type**, seleccione el tipo de contenido que quiera filtrar. Para este ejemplo, use el tipo predeterminado **text/plain** (texto/sin formato). En el cuadro **Ocp-Apim-Subscription-Key**, especifique la clave de suscripción.

### <a name="sample-text-to-scan"></a>Texto de ejemplo que se va a examinar
En el cuadro **Request body** (Cuerpo de la solicitud), escriba algo de texto. En el ejemplo siguiente se muestra un error intencionado en el texto.

> [!NOTE]
> El número de la seguridad social no válido del texto de ejemplo siguiente es intencionado. El propósito es transmitir la entrada de ejemplo y el formato de salida.

```
    Is this a grabage or crap email abcdef@abcd.com, phone: 6657789887, IP: 255.255.255.255, 1 Microsoft Way, Redmond, WA 98052.
    These are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 0800 820 3300.
    Also, 999-99-9999 looks like a social security number (SSN).
```

### <a name="text-classification-feature"></a>Característica de clasificación de texto

En el ejemplo siguiente, puede ver la respuesta de clasificación de texto asistida por máquina de Content Moderator. Ayuda a detectar contenido potencialmente no deseado. El contenido marcado puede interpretarse como inapropiado según el contexto. Además de transmitir la probabilidad de cada categoría, puede recomendar una revisión humana del contenido. La característica utiliza un modelo entrenado para identificar posible lenguaje ofensivo, despectivo o discriminatorio. Esto incluye jerga, palabras abreviadas, palabras ofensivas y palabras mal escritas intencionadamente. 

#### <a name="explanation"></a>Explicación

- `Category1` representa la posible presencia de lenguaje que se puede considerar sexualmente explícito o para adultos en ciertas situaciones.
- `Category2` representa la posible presencia de lenguaje que se puede considerar sexualmente insinuante o para adultos en ciertas situaciones.
- `Category3` representa la posible presencia de lenguaje que se puede considerar ofensivo en ciertas situaciones.
- `Score` se encuentra entre 0 y 1. Cuanto más alta sea la puntuación, mayor será la predicción del modelo sobre la aplicabilidad de la categoría. Esta versión preliminar usa un modelo estadístico, en lugar de resultados codificados manualmente. Se recomienda realizar pruebas con contenido propio para determinar cómo se alinea cada categoría en función de sus propios requisitos.
- `ReviewRecommended` es true o false según los umbrales de puntuación internos. Los clientes deben evaluar si quieren usar este valor o decidir umbrales personalizados basados en sus directivas de contenido.

### <a name="analyze-the-response"></a>Análisis de la respuesta
La respuesta siguiente muestra las distintas conclusiones desde la API. Contiene posibles palabras soeces, DCP, clasificación (versión preliminar) y la versión corregida automáticamente.

> [!NOTE]
> La característica "Classification" (Clasificación) asistida por máquina está en versión preliminar y solo admite inglés.

```
{
    "OriginalText": "Is this a grabage or crap email abcdef@abcd.com, phone: 6657789887, IP: 255.255.255.255, 1 Microsoft Way, Redmond, WA 98052.\r\nThese are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 0800 820 3300.\r\nAlso, 544-56-7788 looks like a social security number (SSN).",
    "NormalizedText": "Is this a grabage or crap email abcdef@ abcd. com, phone: 6657789887, IP: 255. 255. 255. 255, 1 Microsoft Way, Redmond, WA 98052. \r\nThese are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 0800 820 3300. \r\nAlso, 544- 56- 7788 looks like a social security number ( SSN) .",
"Misrepresentation": null,
    "PII": {
            "Email": [{
                "Detected": "abcdef@abcd.com",
                "SubType": "Regular",
                "Text": "abcdef@abcd.com",
                "Index": 32
                }],
            "IPA": [{
                "SubType": "IPV4",
                "Text": "255.255.255.255",
                "Index": 72
                }],
            "Phone": [{
                "CountryCode": "US",
                "Text": "6657789887",
                "Index": 56
                }, {
                "CountryCode": "US",
                "Text": "870 608 4000",
                "Index": 211
                }, {
                "CountryCode": "UK",
                "Text": "+44 870 608 4000",
                "Index": 207
                }, {
                "CountryCode": "UK",
                "Text": "0344 800 2400",
                "Index": 227
                }, {
                "CountryCode": "UK",
                "Text": "0800 820 3300",
                "Index": 244
            }],
         "Address": [{
                 "Text": "1 Microsoft Way, Redmond, WA 98052",
                "Index": 89
            }],
            "SSN": [{
                "Text": "999999999",
                "Index": 56
            }, {
                "Text": "999-99-9999",
                "Index": 266
            }]
        },
    "Classification": {
        "ReviewRecommended": true,
        "Category1": {
            "Score": 1.5113095059859916E-06
        },
        "Category2": {
            "Score": 0.12747249007225037
        },
        "Category3": {
            "Score": 0.98799997568130493
        }
        },
    "Language": "eng",
    "Terms": [{
            "Index": 21,
            "OriginalIndex": 21,
            "ListId": 0,
         "Term": "crap"
        }],
    "Status": {
            "Code": 3000,
            "Description": "OK",
            "Exception": null
        },
     "TrackingId": "2eaa012f-1604-4e36-a8d7-cc34b14ebcb4"
}
```

Para obtener una explicación detallada de todas las secciones de la respuesta JSON, consulte la [información general de Text Moderation API](text-moderation-api.md).

## <a name="next-steps"></a>Pasos siguientes

Usar la API de REST en el código o empezar en [Text Moderation .NET quickstart](text-moderation-quickstart-dotnet.md) (Inicio rápido de Moderación de texto con .NET) para integrar la aplicación.
