---
title: 'Guía de inicio rápido: Identificación del idioma del texto - Translator Text, C#'
titleSuffix: Azure Cognitive Services
description: En esta guía de inicio rápido se identifica el idioma del texto de origen mediante Translator Text API con C#.
services: cognitive-services
author: noellelacharite
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: quickstart
ms.date: 06/15/2018
ms.author: nolachar
ms.openlocfilehash: b9a5dbdc1e7950400597c76359b8eee4268af6d0
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2018
ms.locfileid: "49320380"
---
# <a name="quickstart-identify-language-from-text-with-c35"></a>Guía de inicio rápido: identificación del idioma del texto con C&#35;

En esta guía de inicio rápido se identifica el idioma del texto de origen mediante Translator Text API.

## <a name="prerequisites"></a>Requisitos previos

Se requiere [Visual Studio 2017](https://www.visualstudio.com/downloads/) para ejecutar este código en Windows. (La edición gratuita de Community Edition funcionará).

Para usar Translator Text API, también necesita una clave de suscripción; consulte [Cómo suscribirse a Translator Text API](translator-text-how-to-signup.md).

## <a name="detect-request"></a>Solicitud Detect

> [!TIP]
> Obtenga el código más reciente de [Github](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-C-Sharp).

El siguiente código identifica el idioma del texto de origen mediante el método [Detect](./reference/v3-0-detect.md).

1. Cree un nuevo proyecto de C# en su IDE favorito.
2. Agregue el código que se proporciona a continuación.
3. Reemplace el valor `key` por una clave de acceso válida para la suscripción.
4. Ejecute el programa.

```csharp
using System;
using System.Net.Http;
using System.Text;
// NOTE: Install the Newtonsoft.Json NuGet package.
using Newtonsoft.Json;

namespace TranslatorTextQuickStart
{
    class Program
    {
        static string host = "https://api.cognitive.microsofttranslator.com";
        static string path = "/detect?api-version=3.0";

        static string uri = host + path;

        // NOTE: Replace this example key with a valid subscription key.
        static string key = "ENTER KEY HERE";

        static string text = "Salve mondo!";

        async static void Detect()
        {
            System.Object[] body = new System.Object[] { new { Text = text } };
            var requestBody = JsonConvert.SerializeObject(body);

            using (var client = new HttpClient())
            using (var request = new HttpRequestMessage())
            {
                request.Method = HttpMethod.Post;
                request.RequestUri = new Uri(uri);
                request.Content = new StringContent(requestBody, Encoding.UTF8, "application/json");
                request.Headers.Add("Ocp-Apim-Subscription-Key", key);

                var response = await client.SendAsync(request);
                var responseBody = await response.Content.ReadAsStringAsync();
                var result = JsonConvert.SerializeObject(JsonConvert.DeserializeObject(responseBody), Formatting.Indented);

                Console.OutputEncoding = UnicodeEncoding.UTF8;
                Console.WriteLine(result);
            }
        }

        static void Main(string[] args)
        {
            Detect();
            Console.ReadLine();
        }
    }
}
```

## <a name="detect-response"></a>Respuesta de Detect

Se devuelve una respuesta correcta en JSON, tal como se muestra en el siguiente ejemplo:

```json
[
  {
    "language": "it",
    "score": 1.0,
    "isTranslationSupported": true,
    "isTransliterationSupported": false,
    "alternatives": [
      {
        "language": "pt",
        "score": 1.0,
        "isTranslationSupported": true,
        "isTransliterationSupported": false
      },
      {
        "language": "en",
        "score": 1.0,
        "isTranslationSupported": true,
        "isTransliterationSupported": false
      }
    ]
  }
]
```

## <a name="next-steps"></a>Pasos siguientes

Explore el código de ejemplo de esta guía de inicio rápido y otros, incluida la traducción y la transliteración, así como otros proyectos de Translator Text de ejemplo de GitHub.

> [!div class="nextstepaction"]
> [Explorar ejemplos de C# en GitHub](https://aka.ms/TranslatorGitHub?type=&language=c%23)
