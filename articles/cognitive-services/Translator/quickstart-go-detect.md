---
title: 'Inicio rápido: Identificación del idioma del texto con Go: Translator Text API'
titleSuffix: Azure Cognitive Services
description: En esta guía de inicio rápido se identifica el idioma del texto de origen mediante Translator Text API con Go.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: quickstart
ms.date: 12/05/2018
ms.author: erhopf
ms.openlocfilehash: ee67c4402515ffede1a665bc07bedbeba6a57598
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55880736"
---
# <a name="quickstart-use-the-translator-text-api-to-detect-text-language-using-go"></a>Inicio rápido: Uso de Translator Text API para detectar el idioma del texto con Go

En este inicio rápido, obtendrá información sobre cómo detectar el idioma del texto proporcionado mediante Go y la API de REST Translator Text.

En esta guía de inicio rápido, se requiere una [cuenta de Azure Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) con un recurso de Translator Text. Si no tiene una cuenta, puede usar la [evaluación gratuita](https://azure.microsoft.com/try/cognitive-services/) para obtener una clave de suscripción.

## <a name="prerequisites"></a>Requisitos previos

Esta guía de inicio rápido requiere:

* [Go](https://golang.org/doc/install)
* Una clave de suscripción de Azure para Translator Text

## <a name="create-a-project-and-import-required-modules"></a>Creación de un proyecto e importación de los módulos necesarios

Cree un proyecto de Go con su IDE o editor favorito. A continuación, copie este fragmento de código en un archivo llamado `detect-language.go`.

```go
package main

import (
    "bytes"
    "encoding/json"
    "fmt"
    "log"
    "net/http"
    "net/url"
    "os"
)
```

## <a name="create-the-main-function"></a>Creación de la función main

En este ejemplo se intenta leer la clave de suscripción de Translator Text desde la variable de entorno `TRANSLATOR_TEXT_KEY`. Si no está familiarizado con las variables de entorno, puede establecer `subscriptionKey` como una cadena y convertir en comentario la instrucción condicional.

Copie este código en el proyecto:

```go
func main() {
    /*
     * Read your subscription key from an env variable.
     * Please note: You can replace this code block with
     * var subscriptionKey = "YOUR_SUBSCRIPTION_KEY" if you don't
     * want to use env variables.
     */
    subscriptionKey := os.Getenv("TRANSLATOR_TEXT_KEY")
    if subscriptionKey == "" {
       log.Fatal("Environment variable TRANSLATOR_TEXT_KEY is not set.")
    }
    /*
     * This calls our detect function, which we'll
     * create in the next section. It takes a single argument,
     * the subscription key.
     */
    detect(subscriptionKey)
}
```

## <a name="create-a-function-to-detect-the-text-language"></a>Creación de una función para detectar el idioma del texto

Vamos a crear una función para detectar el idioma del texto. Esta función tendrá un solo argumento, su clave de suscripción de Translator Text.

```go
func detect(subscriptionKey string) {
    /*  
     * In the next few sections, we'll add code to this
     * function to make a request and handle the response.
     */
}
```

A continuación, vamos a construir la dirección URL. La dirección URL se crea mediante los métodos `Parse()` y `Query()`.

Copie este código en la función `detect`.

```go
// Build the request URL. See: https://golang.org/pkg/net/url/#example_URL_Parse
u, _ := url.Parse("https://api.cognitive.microsofttranslator.com/detect?api-version=3.0")
q := u.Query()
u.RawQuery = q.Encode()
```

>[!NOTE]
> Para más información sobre los puntos de conexión, las rutas y los parámetros de la solicitud, consulte [Translator Text API 3.0: Detección](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-detect).

## <a name="create-a-struct-for-your-request-body"></a>Creación de una estructura para el cuerpo de la solicitud

A continuación, cree una estructura anónima para el cuerpo de la solicitud y codifíquelo como JSON con `json.Marshal()`. Agregue este código a la función `detect`.

```go
// Create an anonymous struct for your request body and encode it to JSON
body := []struct {
    Text string
}{
    {Text: "Salve, Mondo!"},
}
b, _ := json.Marshal(body)
```

## <a name="build-the-request"></a>Compilar la solicitud

Ahora que se ha codificado el cuerpo de la solicitud como JSON, puede crear la solicitud POST y llamar a Translator Text API.

```go
// Build the HTTP POST request
req, err := http.NewRequest("POST", u.String(), bytes.NewBuffer(b))
if err != nil {
    log.Fatal(err)
}
// Add required headers
req.Header.Add("Ocp-Apim-Subscription-Key", subscriptionKey)
req.Header.Add("Content-Type", "application/json")

// Call the Translator Text API
res, err := http.DefaultClient.Do(req)
if err != nil {
    log.Fatal(err)
}
```

## <a name="handle-and-print-the-response"></a>Control e impresión de la respuesta

Agregue este código a la función `detect` para descodificar la respuesta JSON y luego dé formato al resultado e imprímalo.

```go
// Decode the JSON response
var result interface{}
if err := json.NewDecoder(res.Body).Decode(&result); err != nil {
    log.Fatal(err)
}
// Format and print the response to terminal
prettyJSON, _ := json.MarshalIndent(result, "", "  ")
fmt.Printf("%s\n", prettyJSON)
```

## <a name="put-it-all-together"></a>Colocación de todo junto

Eso es todo, ha creado un sencillo programa que llama a Translator Text API y devuelve una respuesta JSON. Ahora es el momento de ejecutar el programa:

```console
go run detect-language.go
```

Si desea comparar su código con el nuestro, el ejemplo completo está disponible en [GitHub](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-Go).

## <a name="sample-response"></a>Respuesta de muestra

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

## <a name="see-also"></a>Otras referencias

Aprenda a usar Translator Text API para:

* [Traducir texto](quickstart-go-translate.md)
* [Transliterar texto](quickstart-go-transliterate.md)
* [Obtener traducciones alternativas](quickstart-go-dictionary.md)
* [Obtener una lista de idiomas admitidos](quickstart-go-languages.md)
* [Determinar la longitud de las oraciones de una entrada](quickstart-go-sentences.md)
