---
title: 'Inicio rápido: Conversión de script de texto con Translator Text API en Ruby'
titleSuffix: Azure Cognitive Services
description: En esta guía de inicio rápido se convierte texto en un idioma de un script en otro mediante Translator Text API con Ruby.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: quickstart
ms.date: 06/22/2018
ms.author: erhopf
ms.openlocfilehash: 10f74e04349984885f9c69ac17ac1aa9b41e710f
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2019
ms.locfileid: "55461192"
---
# <a name="quickstart-transliterate-text-with-the-translator-text-rest-api-ruby"></a>Inicio rápido: Transcripción de texto con Translator Text REST API (Ruby)

En esta guía de inicio rápido se convierte texto en un idioma de un script en otro mediante Translator Text API.

## <a name="prerequisites"></a>Requisitos previos

Se requiere [Ruby 2.4](https://www.ruby-lang.org/en/downloads/) o una versión posterior para ejecutar el código.

Para usar Translator Text API, también necesita una clave de suscripción; consulte [Cómo suscribirse a Translator Text API](translator-text-how-to-signup.md).

## <a name="transliterate-request"></a>Solicitud Transliterate

La siguiente convierte texto en un idioma de un script en otro script mediante el método [Transliterate](./reference/v3-0-transliterate.md).

1. Cree un nuevo proyecto de Ruby en el editor de código que prefiera.
2. Agregue el código que se proporciona a continuación.
3. Reemplace el valor `key` por una clave de acceso válida para la suscripción.
4. Ejecute el programa.

```ruby
require 'net/https'
require 'uri'
require 'cgi'
require 'json'
require 'securerandom'

# **********************************************
# *** Update or verify the following values. ***
# **********************************************

# Replace the key string value with your valid subscription key.
key = 'ENTER KEY HERE'

host = 'https://api.cognitive.microsofttranslator.com'
path = '/transliterate?api-version=3.0'

# Transliterate text in Japanese from Japanese script (i.e. Hiragana/Katakana/Kanji) to Latin script.
params = "&language=ja&fromScript=jpan&toScript=latn";

uri = URI (host + path + params)

# Transliterate "good afternoon".
text = 'こんにちは'

content = '[{"Text" : "' + text + '"}]'

request = Net::HTTP::Post.new(uri)
request['Content-type'] = 'application/json'
request['Content-length'] = content.length
request['Ocp-Apim-Subscription-Key'] = key
request['X-ClientTraceId'] = SecureRandom.uuid
request.body = content

response = Net::HTTP.start(uri.host, uri.port, :use_ssl => uri.scheme == 'https') do |http|
    http.request (request)
end

result = response.body.force_encoding("utf-8")

json = JSON.pretty_generate(JSON.parse(result))
puts json
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
> [Explorar ejemplos de Ruby en GitHub](https://aka.ms/TranslatorGitHub?type=&language=ruby)
