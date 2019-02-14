---
title: 'Inicio rápido: obtención de los idiomas admitidos (Ruby): Translator Text API'
titleSuffix: Azure Cognitive Services
description: En esta guía de inicio rápido se obtiene una lista de los idiomas admitidos para la traducción, la transliteración, la búsqueda en el diccionario y ejemplos mediante Translator Text API con Ruby.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: quickstart
ms.date: 02/08/2019
ms.author: erhopf
ms.openlocfilehash: 1b92adcf528a1ccd00983e6c0dd952fefab5dd7d
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/09/2019
ms.locfileid: "55975473"
---
# <a name="quickstart-get-supported-languages-with-the-translator-text-rest-api-ruby"></a>Inicio rápido: obtención de los idiomas admitidos con la API REST Translator Text (Ruby)

En esta guía de inicio rápido se obtiene una lista de los idiomas admitidos para la traducción, la transliteración, la búsqueda en el diccionario y ejemplos mediante Translator Text API.

## <a name="prerequisites"></a>Requisitos previos

Se requiere [Ruby 2.4](https://www.ruby-lang.org/en/downloads/) o una versión posterior para ejecutar el código.

## <a name="languages-request"></a>Solicitud Languages

Con el siguiente código se obtiene una lista de los idiomas admitidos para la traducción, la transliteración, la búsqueda en el diccionario y ejemplos mediante el método [Languages](./reference/v3-0-languages.md).

1. Cree un nuevo proyecto de Ruby en el editor de código que prefiera.
2. Agregue el código que se proporciona a continuación.
3. Ejecute el programa.

```ruby
require 'net/https'
require 'uri'
require 'cgi'
require 'json'

host = 'https://api.cognitive.microsofttranslator.com'
path = '/languages?api-version=3.0'

uri = URI (host + path)

request = Net::HTTP::Get.new(uri)

response = Net::HTTP.start(uri.host, uri.port, :use_ssl => uri.scheme == 'https') do |http|
    http.request (request)
end

result = response.body.force_encoding("utf-8")

json = JSON.pretty_generate(JSON.parse(result))

output_path = 'output.txt'

# Write response to file
File.open(output_path, 'w' ) do |output|
    output.print json
end
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
> [Explorar ejemplos de Ruby en GitHub](https://aka.ms/TranslatorGitHub?type=&language=ruby)
