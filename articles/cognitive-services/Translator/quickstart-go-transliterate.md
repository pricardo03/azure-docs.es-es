---
title: 'Inicio rápido: Conversión de script de texto con Go: Translator Text API'
titleSuffix: Azure Cognitive Services
description: En esta guía de inicio rápido se convierte texto en un idioma de un script en otro mediante Translator Text API con Go.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: quickstart
ms.date: 12/05/2018
ms.author: erhopf
ms.openlocfilehash: 976ca2623667abc4e49ceb91ec97706723bc32c2
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55211753"
---
# <a name="quickstart-use-the-translator-text-api-to-transliterate-text-using-go"></a>Inicio rápido: Uso de Translator Text API para transliterar texto mediante Go

En esta guía de inicio rápido se convierte texto en un idioma de un script en otro mediante Translator Text API.

En esta guía de inicio rápido, se requiere una [cuenta de Azure Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) con un recurso de Translator Text. Si no tiene una cuenta, puede usar la [evaluación gratuita](https://azure.microsoft.com/try/cognitive-services/) para obtener una clave de suscripción.

## <a name="prerequisites"></a>Requisitos previos

Esta guía de inicio rápido requiere:

* [Go](https://golang.org/doc/install)
* Una clave de suscripción de Azure para Translator Text

## <a name="create-a-project-and-import-required-modules"></a>Creación de un proyecto e importación de los módulos necesarios

Cree un proyecto de Go con su IDE o editor favorito. A continuación, copie este fragmento de código en un archivo llamado `transliterate-text.go`.

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
     * This calls our transliterate function, which we'll
     * create in the next section. It takes a single argument,
     * the subscription key.
     */
    transliterate(subscriptionKey)
}
```

## <a name="create-a-function-to-transliterate-text"></a>Creación de una función para transliterar texto

Vamos a crear una función para transliterar texto. Esta función tendrá un solo argumento, su clave de suscripción de Translator Text.

```go
func transliterate(subscriptionKey string) {
    /*  
     * In the next few sections, we'll add code to this
     * function to make a request and handle the response.
     */
}
```

A continuación, vamos a construir la dirección URL. La dirección URL se crea mediante los métodos `Parse()` y `Query()`. Observará que se han agregado los parámetros con el método `Add()`. En este ejemplo, vamos a transcribir de japonés al alfabeto latino.

Copie este código en la función `transliterate`.

```go
// Build the request URL. See: https://golang.org/pkg/net/url/#example_URL_Parse
u, _ := url.Parse("https://api.cognitive.microsofttranslator.com/transliterate?api-version=3.0")
q := u.Query()
q.Add("language", "ja")
q.Add("fromScript", "jpan")
q.Add("toScript", "latn")
u.RawQuery = q.Encode()
```

>[!NOTE]
> Para más información sobre los puntos de conexión, las rutas y los parámetros de la solicitud, consulte [Translator Text API 3.0: transliteración](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-transliterate).

## <a name="create-a-struct-for-your-request-body"></a>Creación de una estructura para el cuerpo de la solicitud

A continuación, cree una estructura anónima para el cuerpo de la solicitud y codifíquelo como JSON con `json.Marshal()`. Agregue este código a la función `transliterate`.

```go
// Create an anonymous struct for your request body and encode it to JSON
body := []struct {
    Text string
}{
    {Text: "こんにちは"},
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
// Add required headers to the request
req.Header.Add("Ocp-Apim-Subscription-Key", subscriptionKey)
req.Header.Add("Content-Type", "application/json")

// Call the Translator Text API
res, err := http.DefaultClient.Do(req)
if err != nil {
    log.Fatal(err)
}
```

## <a name="handle-and-print-the-response"></a>Control e impresión de la respuesta

Agregue este código a la función `transliterate` para descodificar la respuesta JSON y luego dé formato al resultado e imprímalo.

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
go run transliterate-text.go
```

Si desea comparar su código con el nuestro, el ejemplo completo está disponible en [GitHub](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-Go).

## <a name="sample-response"></a>Respuesta de muestra

```json
[
  {
    "script": "latn",
    "text": "konnnichiha"
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
* [Identificar el idioma de entrada](quickstart-go-detect.md)
* [Obtener traducciones alternativas](quickstart-go-dictionary.md)
* [Obtener una lista de idiomas admitidos](quickstart-go-languages.md)
* [Determinar la longitud de las oraciones de una entrada](quickstart-go-sentences.md)
