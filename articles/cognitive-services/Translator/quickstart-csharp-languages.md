---
title: 'Guía de inicio rápido: obtención de los idiomas admitidos (C#): Translator Text API'
titleSuffix: Azure Cognitive Services
description: En esta guía de inicio rápido se obtiene una lista de los idiomas admitidos para la traducción, la transliteración, la búsqueda en el diccionario y ejemplos mediante Translator Text API con C#.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: quickstart
ms.date: 06/15/2018
ms.author: erhopf
ms.openlocfilehash: 9ac881adcf7d273c9a3bcea55d084acced59c107
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2018
ms.locfileid: "49645412"
---
# <a name="quickstart-get-supported-languages-with-the-translator-text-rest-api-c"></a>Guía de inicio rápido: obtención de los idiomas admitidos con la API REST Translator Text (C#)

En esta guía de inicio rápido se obtiene una lista de los idiomas admitidos para la traducción, la transliteración, la búsqueda en el diccionario y ejemplos mediante Translator Text API.

## <a name="prerequisites"></a>Requisitos previos

Se requiere [Visual Studio 2017](https://www.visualstudio.com/downloads/) para ejecutar este código en Windows. (La edición gratuita de Community Edition funcionará).

Para usar Translator Text API, también necesita una clave de suscripción; consulte [Cómo suscribirse a Translator Text API](translator-text-how-to-signup.md).

## <a name="languages-request"></a>Solicitud Languages

> [!TIP]
> Obtenga el código más reciente de [Github](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-C-Sharp).

Con el siguiente código se obtiene una lista de los idiomas admitidos para la traducción, la transliteración, la búsqueda en el diccionario y ejemplos mediante el método [Languages](./reference/v3-0-languages.md).

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
        static string path = "/languages?api-version=3.0";

        // NOTE: Replace this example key with a valid subscription key.
        static string key = "ENTER KEY HERE";

        async static void GetLanguages()
        {
            HttpClient client = new HttpClient();
            client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", key);
            var uri = host + path;
            var response = await client.GetAsync(uri);
            var result = await response.Content.ReadAsStringAsync();
            var json = JsonConvert.SerializeObject(JsonConvert.DeserializeObject(result), Formatting.Indented);
            // Note: If writing to the console, set this.
            // Console.OutputEncoding = UnicodeEncoding.UTF8;
            System.IO.File.WriteAllBytes("output.txt", Encoding.UTF8.GetBytes(json));
        }

        static void Main(string[] args)
        {
            GetLanguages();
            Console.ReadLine();
        }
    }
}
```

## <a name="languages-response"></a>Respuesta de Languages

Se devuelve una respuesta correcta en JSON, tal como se muestra en el siguiente ejemplo:

```json
{
  "translation": {
    "af": {
      "name": "Afrikaans",
      "nativeName": "Afrikaans",
      "dir": "ltr"
    },
    "ar": {
      "name": "Arabic",
      "nativeName": "العربية",
      "dir": "rtl"
    },
...
  },
  "transliteration": {
    "ar": {
      "name": "Arabic",
      "nativeName": "العربية",
      "scripts": [
        {
          "code": "Arab",
          "name": "Arabic",
          "nativeName": "العربية",
          "dir": "rtl",
          "toScripts": [
            {
              "code": "Latn",
              "name": "Latin",
              "nativeName": "اللاتينية",
              "dir": "ltr"
            }
          ]
        },
        {
          "code": "Latn",
          "name": "Latin",
          "nativeName": "اللاتينية",
          "dir": "ltr",
          "toScripts": [
            {
              "code": "Arab",
              "name": "Arabic",
              "nativeName": "العربية",
              "dir": "rtl"
            }
          ]
        }
      ]
    },
...
  },
  "dictionary": {
    "af": {
      "name": "Afrikaans",
      "nativeName": "Afrikaans",
      "dir": "ltr",
      "translations": [
        {
          "name": "English",
          "nativeName": "English",
          "dir": "ltr",
          "code": "en"
        }
      ]
    },
    "ar": {
      "name": "Arabic",
      "nativeName": "العربية",
      "dir": "rtl",
      "translations": [
        {
          "name": "English",
          "nativeName": "English",
          "dir": "ltr",
          "code": "en"
        }
      ]
    },
...
  }
}
```

## <a name="next-steps"></a>Pasos siguientes

Explore el código de ejemplo de esta guía de inicio rápido y otros, incluida la traducción y la transliteración, así como otros proyectos de Translator Text de ejemplo de GitHub.

> [!div class="nextstepaction"]
> [Explorar ejemplos de C# en GitHub](https://aka.ms/TranslatorGitHub?type=&language=c%23)
