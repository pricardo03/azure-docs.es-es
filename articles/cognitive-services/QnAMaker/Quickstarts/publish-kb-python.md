---
title: 'Guía de inicio rápido: Publicación de la base de conocimiento en QnA Maker con REST y Python'
titleSuffix: Azure Cognitive Services
description: En esta guía de inicio rápido basada en REST se explica el proceso de publicación de la base de conocimiento que le permite insertar la versión más reciente probada en un índice de Azure Search dedicado que representa la base de conocimiento publicada. También se crea un punto de conexión al que puede llamar en su aplicación o bot de chat.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: quickstart
ms.date: 10/19/2018
ms.author: diberry
ms.openlocfilehash: e58b344102eb900ffe41bb90e541258eb3b59286
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2018
ms.locfileid: "49646821"
---
# <a name="quickstart-publish-a-knowledge-base-in-qna-maker-using-python"></a>Guía de inicio rápido: Publicación de una base de conocimiento en QnA Maker con Python

Esta guía de inicio rápido describe la publicación mediante programación de una base de conocimiento (KB). La publicación inserta la versión más reciente de la base de conocimiento en un índice de Azure Search dedicado y crea un punto de conexión que se puede llamar en su aplicación o bot de chat.

En esta guía de inicio rápido se llama a las siguientes API de QnA Maker:
* [Publicar](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75fe): esta API no requiere ninguna información en el cuerpo de la solicitud.

[!INCLUDE [Code is available in Azure-Samples Github repo](../../../../includes/cognitive-services-qnamaker-python-repo-note.md)]

1. Cree un nuevo proyecto de Python en su IDE favorito.
2. Agregue el código que se proporciona a continuación.
3. Reemplace el valor `key` por una clave de acceso válida para la suscripción.
4. Ejecute el programa.

```python
# -*- coding: utf-8 -*-

import http.client, urllib.parse, json, time

# **********************************************
# *** Update or verify the following values. ***
# **********************************************

# Replace this with a valid subscription key.
subscriptionKey = 'ENTER KEY HERE'

# Replace this with a valid knowledge base ID.
kb = 'ENTER ID HERE';

host = 'westus.api.cognitive.microsoft.com'
service = '/qnamaker/v4.0'
method = '/knowledgebases/'

def pretty_print (content):
# Note: We convert content to and from an object so we can pretty-print it.
    return json.dumps(json.loads(content), indent=4)

def publish_kb (path, content):
    print ('Calling ' + host + path + '.')
    headers = {
        'Ocp-Apim-Subscription-Key': subscriptionKey,
        'Content-Type': 'application/json',
        'Content-Length': len (content)
    }
    conn = http.client.HTTPSConnection(host)
    conn.request ("POST", path, content, headers)
    response = conn.getresponse ()

    if response.status == 204:
        return json.dumps({'result' : 'Success.'})
    else:
        return response.read ()

path = service + method + kb
result = publish_kb (path, '')
print (pretty_print(result))
```

## <a name="the-publish-a-knowledge-base-response"></a>Respuesta de la publicación de una base de conocimiento

Se devuelve una respuesta correcta en JSON, como se muestra en el siguiente ejemplo:

```json
{
  "result": "Success."
}
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Referencia de QnA Maker (V4) REST API](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff)

[Introducción de QnA Maker](../Overview/overview.md)