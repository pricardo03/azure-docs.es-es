---
title: 'Inicio rápido: Convertir script de texto - Translator Text, C#'
titleSuffix: Azure Cognitive Services
description: En esta guía de inicio rápido se convierte texto en un idioma de un script en otro mediante Translator Text API con C#.
services: cognitive-services
author: noellelacharite
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: quickstart
ms.date: 06/15/2018
ms.author: nolachar
ms.openlocfilehash: 53ac3f4aa85e889c86d09927ece7df012427e4a9
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/19/2018
ms.locfileid: "46367600"
---
# <a name="quickstart-transliterate-text-with-c35"></a>Guía de inicio rápido: transliteración de texto con C&#35;

En esta guía de inicio rápido se convierte texto en un idioma de un script en otro mediante Translator Text API.

El código fuente del ejemplo está disponible en [GitHub](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-C-Sharp).

## <a name="prerequisites"></a>Requisitos previos

Se requiere [Visual Studio 2017](https://www.visualstudio.com/downloads/) para ejecutar este código en Windows. (La edición gratuita de Community Edition funcionará).

Para usar Translator Text API, también necesita una clave de suscripción; consulte [Cómo suscribirse a Translator Text API](translator-text-how-to-signup.md).

## <a name="transliterate-request"></a>Solicitud Transliterate

La siguiente convierte texto en un idioma de un script en otro script mediante el método [Transliterate](./reference/v3-0-transliterate.md).

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
        static string path = "/transliterate?api-version=3.0";
        // Transliterate text in Japanese from Japanese script (i.e. Hiragana/Katakana/Kanji) to Latin script.
        static string params_ = "&language=ja&fromScript=jpan&toScript=latn";

        static string uri = host + path + params_;

        // NOTE: Replace this example key with a valid subscription key.
        static string key = "ENTER KEY HERE";

        // Transliterate "good afternoon".
        static string text = "こんにちは";

        async static void Transliterate()
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
            Transliterate();
            Console.ReadLine();
        }
    }
}
```

## <a name="transliterate-response"></a>Respuesta de Transliterate

Se devuelve una respuesta correcta en JSON, tal como se muestra en el siguiente ejemplo:

```json
[
  {
    "text": "konnnichiha",
    "script": "latn"
  }
]
```

## <a name="next-steps"></a>Pasos siguientes

Explore el código de ejemplo de esta guía de inicio rápido y otros, incluida la traducción y la identificación del idioma, así como otros proyectos de Translator Text de ejemplo de GitHub.

> [!div class="nextstepaction"]
> [Explorar ejemplos de C# en GitHub](https://aka.ms/TranslatorGitHub?type=&language=c%23)
