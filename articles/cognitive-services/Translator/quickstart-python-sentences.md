---
title: 'Guía de inicio rápido: Obtención de longitudes de frase con Translator Text API en Python'
titleSuffix: Azure Cognitive Services
description: En esta guía de inicio rápido se determina la longitud de las oraciones del texto mediante Translator Text API con Python.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: quickstart
ms.date: 06/21/2018
ms.author: erhopf
ms.openlocfilehash: 73906c9b6f5164aff905c4f647d1b1b74a92587c
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2018
ms.locfileid: "49648095"
---
# <a name="quickstart-get-sentence-lengths-with-the-translator-text-rest-api-python"></a>Guía de inicio rápido: Obtención de longitudes de frase con Translator Text REST API (Python)

En esta guía de inicio rápido se determina la longitud de las oraciones del texto mediante Translator Text API.

## <a name="prerequisites"></a>Requisitos previos

Se requiere [Python 3.x](https://www.python.org/downloads/) para ejecutar este código.

Para usar Translator Text API, también necesita una clave de suscripción; consulte [Cómo suscribirse a Translator Text API](translator-text-how-to-signup.md).

## <a name="breaksentence-request"></a>Solicitud BreakSentence

El siguiente código divide el texto de origen en oraciones mediante el método [BreakSentence](./reference/v3-0-break-sentence.md).

1. Cree un nuevo proyecto de Python en el editor de código que prefiera.
2. Agregue el código que se proporciona a continuación.
3. Reemplace el valor `subscriptionKey` por una clave de acceso válida para la suscripción.
4. Ejecute el programa.

```python
# -*- coding: utf-8 -*-

import http.client, urllib.parse, uuid, json

# **********************************************
# *** Update or verify the following values. ***
# **********************************************

# Replace the subscriptionKey string value with your valid subscription key.
subscriptionKey = 'ENTER KEY HERE'

host = 'api.cognitive.microsofttranslator.com'
path = '/breaksentence?api-version=3.0'

params = ''

text = 'How are you? I am fine. What did you do today?'

def breakSentences (content):

    headers = {
        'Ocp-Apim-Subscription-Key': subscriptionKey,
        'Content-type': 'application/json',
        'X-ClientTraceId': str(uuid.uuid4())
    }

    conn = http.client.HTTPSConnection(host)
    conn.request ("POST", path + params, content, headers)
    response = conn.getresponse ()
    return response.read ()

requestBody = [{
    'Text' : text,
}]
content = json.dumps(requestBody, ensure_ascii=False).encode('utf-8')
result = breakSentences (content)

# Note: We convert result, which is JSON, to and from an object so we can pretty-print it.
# We want to avoid escaping any Unicode characters that result contains. See:
# https://stackoverflow.com/questions/18337407/saving-utf-8-texts-in-json-dumps-as-utf8-not-as-u-escape-sequence
output = json.dumps(json.loads(result), indent=4, ensure_ascii=False)

print (output)
```

## <a name="breaksentence-response"></a>Respuesta de BreakSentence

Se devuelve una respuesta correcta en JSON, tal como se muestra en el siguiente ejemplo:

```json
[
  {
    "detectedLanguage": {
      "language": "en",
      "score": 1.0
    },
    "sentLen": [
      13,
      11,
      22
    ]
  }
]
```

## <a name="next-steps"></a>Pasos siguientes

Explore el código de ejemplo de esta guía de inicio rápido y otros, incluida la traducción y la transliteración, así como otros proyectos de Translator Text de ejemplo de GitHub.

> [!div class="nextstepaction"]
> [Explorar ejemplos de Python en GitHub](https://aka.ms/TranslatorGitHub?type=&language=python)
