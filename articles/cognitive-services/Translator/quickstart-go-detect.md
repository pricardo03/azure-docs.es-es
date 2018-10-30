---
title: 'Guía de inicio rápido: Identificación del idioma del texto con Translator Text API en Go'
titleSuffix: Azure Cognitive Services
description: En esta guía de inicio rápido se identifica el idioma del texto de origen mediante Translator Text API con Go.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: quickstart
ms.date: 06/29/2018
ms.author: erhopf
ms.openlocfilehash: 0275b408e71ec967f6453c94566b4799b3dd4396
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2018
ms.locfileid: "49647211"
---
# <a name="quickstart-identify-language-from-text-with-the-translator-text-rest-api-go"></a>Guía de inicio rápido: Identificación del idioma del texto con la API REST Translator Text (Go)

En esta guía de inicio rápido se identifica el idioma del texto de origen mediante Translator Text API.

## <a name="prerequisites"></a>Requisitos previos

Tendrá que instalar [Go distribution](https://golang.org/doc/install) para poder ejecutar este código. El código de ejemplo usa solo bibliotecas **principales**, por lo que no hay dependencias externas.

Para usar Translator Text API, también necesita una clave de suscripción; consulte [Cómo suscribirse a Translator Text API](translator-text-how-to-signup.md).

## <a name="detect-request"></a>Solicitud Detect

El siguiente código identifica el idioma del texto de origen mediante el método [Detect](./reference/v3-0-detect.md).

1. Cree un nuevo proyecto de Go en su editor de código favorito.
2. Agregue el código que se proporciona a continuación.
3. Reemplace el valor `subscriptionKey` por una clave de acceso válida para la suscripción.
4. Guarde el archivo con la extensión ".go".
5. Abra un símbolo del sistema en un equipo que tenga instalado Go.
6. Compile el archivo, por ejemplo: "'go build quickstart-detect.go"'.
7. Ejecute el archivo, por ejemplo: "quickstart-detect".

```golang
package main

import (
    "encoding/json"
    "fmt"
    "io/ioutil"
    "net/http"
    "strconv"
    "strings"
    "time"
)

func main() {
    // Replace the subscriptionKey string value with your valid subscription key
    const subscriptionKey = "<Subscription Key>"

    const uriBase = "https://api.cognitive.microsofttranslator.com"
    const uriPath = "/detect?api-version=3.0"

    const uri = uriBase + uriPath

    const text = "Salve, mondo!"

    r := strings.NewReader("[{\"Text\" : \"" + text + "\"}]")

    client := &http.Client{
        Timeout: time.Second * 2,
    }

    req, err := http.NewRequest("POST", uri, r)
    if err != nil {
        fmt.Printf("Error creating request: %v\n", err)
        return
    }

    req.Header.Add("Content-Type", "application/json")
    req.Header.Add("Content-Length", strconv.FormatInt(req.ContentLength, 10))
    req.Header.Add("Ocp-Apim-Subscription-Key", subscriptionKey)

    resp, err := client.Do(req)
    if err != nil {
        fmt.Printf("Error on request: %v\n", err)
        return
    }
    defer resp.Body.Close()

    body, err := ioutil.ReadAll(resp.Body)
    if err != nil {
        fmt.Printf("Error reading response body: %v\n", err)
        return
    }

    var f interface{}
    json.Unmarshal(body, &f)

    jsonFormatted, err := json.MarshalIndent(f, "", "  ")
    if err != nil {
        fmt.Printf("Error producing JSON: %v\n", err)
        return
    }
    fmt.Println(string(jsonFormatted))
}
```

## <a name="detect-response"></a>Respuesta de Detect

Se devuelve una respuesta correcta en JSON, tal como se muestra en el siguiente ejemplo:

```json
[
  {
    "alternatives": [
      {
        "isTranslationSupported": true,
        "isTransliterationSupported": false,
        "language": "pt",
        "score": 1
      },
      {
        "isTranslationSupported": true,
        "isTransliterationSupported": false,
        "language": "en",
        "score": 1
      }
    ],
    "isTranslationSupported": true,
    "isTransliterationSupported": false,
    "language": "it",
    "score": 1
  }
]
```

## <a name="next-steps"></a>Pasos siguientes

Explorar los paquetes de Go para Cognitive Services APIs desde el [Azure SDK para Go](https://github.com/Azure/azure-sdk-for-go) en GitHub.

> [!div class="nextstepaction"]
> [Explore los paquetes de Go en GitHub](https://github.com/Azure/azure-sdk-for-go/tree/master/services/cognitiveservices)
