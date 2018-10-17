---
title: 'Guía de inicio rápido: Bing Spell Check API con Ruby'
titlesuffix: Azure Cognitive Services
description: Obtenga información y ejemplos de código que le ayuden a empezar a usar rápidamente Bing Spell Check API.
services: cognitive-services
author: v-jaswel
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-spell-check
ms.topic: quickstart
ms.date: 09/14/2017
ms.author: v-jaswel
ms.openlocfilehash: 3fe8729a9e2524cc2ccda168a857d58664a98b10
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2018
ms.locfileid: "48801087"
---
# <a name="quickstart-for-bing-spell-check-api-with-ruby"></a>Guía de inicio rápido para Bing Spell Check API con Ruby 

En este artículo se muestra cómo usar [Bing Spell Check API](https://azure.microsoft.com/services/cognitive-services/spell-check/) con Ruby. Spell Check API devuelve una lista de palabras que no reconoce, junto con los reemplazos sugeridos. Normalmente, enviaría texto a esta API y, luego, realizaría los reemplazos sugeridos en el texto o los mostraría al usuario de la aplicación para que pueda decidir si realizarlos no. En este artículo se muestra cómo enviar una solicitud que contiene el texto "Hollo wrld!" Los reemplazos sugeridos son "Hello" y "world".

## <a name="prerequisites"></a>Requisitos previos

Necesitará [Ruby 2.4](https://www.ruby-lang.org/en/downloads/) o una versión posterior para ejecutar el código.

Debe tener una [cuenta de la API de Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) con **Bing Spell Check API v7**. La [cuenta de evaluación gratuita](https://azure.microsoft.com/try/cognitive-services/#lang) es suficiente para esta guía de inicio rápido. Necesita la clave de acceso que se le proporciona al activar la versión de evaluación gratuita, o puede usar una clave de suscripción de pago desde su panel de Azure.

## <a name="get-spell-check-results"></a>Obtención de resultados de Spell Check

1. Cree un nuevo proyecto de Ruby en su IDE favorito.
2. Agregue el código que se proporciona a continuación.
3. Reemplace el valor `subscriptionKey` por una clave de acceso válida para la suscripción.
4. Ejecute el programa.

```ruby
require 'net/http'
require 'uri'
require 'json'

uri = 'https://api.cognitive.microsoft.com'
path = '/bing/v7.0/spellcheck?'
params = 'mkt=en-us&mode=proof'

uri = URI(uri + path + params)
uri.query = URI.encode_www_form({
    # Request parameters
    'text' => 'Hollo, wrld!'
})

# NOTE: Replace this example key with a valid subscription key.
key = 'ENTER KEY HERE'

# The headers in the following example 
# are optional but should be considered as required:
#
# X-MSEdge-ClientIP: 999.999.999.999  
# X-Search-Location: lat: +90.0000000000000;long: 00.0000000000000;re:100.000000000000
# X-MSEdge-ClientID: <Client ID from Previous Response Goes Here>
#
# See below for more information.

request = Net::HTTP::Post.new(uri)
request['Content-Type'] = "application/x-www-form-urlencoded"

request['Ocp-Apim-Subscription-Key'] = key

response = Net::HTTP.start(uri.host, uri.port, :use_ssl => uri.scheme == 'https') do |http|
    http.request(request)
end

result = JSON.pretty_generate(JSON.parse(response.body))
puts result
```

**Respuesta**

Se devuelve una respuesta correcta en JSON, como se muestra en el siguiente ejemplo: 

```json
{
   "_type": "SpellCheck",
   "flaggedTokens": [
      {
         "offset": 0,
         "token": "Hollo",
         "type": "UnknownToken",
         "suggestions": [
            {
               "suggestion": "Hello",
               "score": 0.9115257530801
            },
            {
               "suggestion": "Hollow",
               "score": 0.858039839213461
            },
            {
               "suggestion": "Hallo",
               "score": 0.597385084464481
            }
         ]
      },
      {
         "offset": 7,
         "token": "wrld",
         "type": "UnknownToken",
         "suggestions": [
            {
               "suggestion": "world",
               "score": 0.9115257530801
            }
         ]
      }
   ]
}
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Tutorial de Bing Spell Check](../tutorials/spellcheck.md)

## <a name="see-also"></a>Otras referencias

- [Introducción a Bing Spell Check](../proof-text.md)
- [Referencia de Bing Spell Check API v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v7-reference)
