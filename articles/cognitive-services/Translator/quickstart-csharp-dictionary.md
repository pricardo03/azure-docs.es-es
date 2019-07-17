---
title: 'Inicio rápido: Búsqueda de palabras con diccionario bilingüe, C#: Translator Text API'
titleSuffix: Azure Cognitive Services
description: En esta guía de inicio rápido aprenderá a obtener posibles traducciones alternativas de un término y se proporcionan ejemplos de uso mediante .NET Core y Translator Text API.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: quickstart
ms.date: 06/04/2019
ms.author: swmachan
ms.openlocfilehash: 1f80f9b0f044fe8b32a555b0509e14cd2172dd0a
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2019
ms.locfileid: "67704599"
---
# <a name="quickstart-look-up-words-with-bilingual-dictionary-using-c"></a>Inicio rápido: Búsqueda de palabras con un diccionario bilingüe mediante C#

En esta guía de inicio rápido aprenderá a obtener posibles traducciones alternativas de un término y se proporcionan ejemplos de uso mediante .NET Core y Translator Text API.

En esta guía de inicio rápido, se requiere una [cuenta de Azure Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) con un recurso de Translator Text. Si no tiene una cuenta, puede usar la [evaluación gratuita](https://azure.microsoft.com/try/cognitive-services/) para obtener una clave de suscripción.

>[!TIP]
> Si quiere ver todo el código a la vez, el código fuente de este ejemplo está disponible en [GitHub](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-C-Sharp).

## <a name="prerequisites"></a>Requisitos previos

* [SDK de .NET](https://www.microsoft.com/net/learn/dotnet/hello-world-tutorial)
* [Paquete NuGet de Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/)
* [Visual Studio](https://visualstudio.microsoft.com/downloads/), [Visual Studio Code](https://code.visualstudio.com/download) o su editor favorito de código.
* Una clave de suscripción de Azure para Translator Text

## <a name="create-a-net-core-project"></a>Creación de un proyecto de .NET Core

Abra un nuevo símbolo del sistema (o una sesión de terminal) y ejecute estos comandos:

```console
dotnet new console -o alternate-sample
cd alternate-sample
```

El primer comando hace dos cosas. Crea una nueva aplicación de consola .NET y crea un directorio denominado `alternate-sample`. El segundo comando cambia al directorio del proyecto.

A continuación, deberá instalar Json.Net. En el directorio del proyecto, ejecute:

```console
dotnet add package Newtonsoft.Json --version 11.0.2
```

## <a name="add-required-namespaces-to-your-project"></a>Incorporación de los espacios de nombres necesarios al proyecto

El comando `dotnet new console` que ejecutó anteriormente creó un proyecto que incluía `Program.cs`. En este archivo es donde deberá colocar el código de la aplicación. Abra `Program.cs` y reemplace las instrucciones using existentes. Estas instrucciones garantizan que tiene acceso a todos los tipos necesarios para compilar y ejecutar la aplicación de ejemplo.

```csharp
using System;
using System.Net.Http;
using System.Text;
using Newtonsoft.Json;
```

## <a name="create-a-function-to-get-alternate-translations"></a>Crear una función para obtener posibles traducciones alternativas

Dentro de la clase `Program`, cree una función denominada `AltTranslation`. Esta clase encapsula el código que se usa para llamar al recurso Dictionary e imprime el resultado en la consola.

```csharp
static void AltTranslation()
{
  /*
   * The code for your call to the translation service will be added to this
   * function in the next few sections.
   */
}
```

## <a name="set-the-subscription-key-host-name-and-path"></a>Establecimiento de la clave de suscripción, nombre de host y la ruta de acceso

Agregue estas líneas a la función `AltTranslation`. Observe que junto con la `api-version`, se han anexado dos parámetros adicionales a `route`. Estos parámetros se usan para establecer la entrada y la salida de la traducción. En este ejemplo, se trata de inglés (`en`) y español (`es`).

```csharp
string host = "https://api.cognitive.microsofttranslator.com";
string route = "/dictionary/lookup?api-version=3.0&from=en&to=es";
string subscriptionKey = "YOUR_SUBSCRIPTION_KEY";
```

A continuación, se debe crear y serializar el objeto JSON que incluye el texto que desea traducir. Tenga en cuenta que puede pasar más de un objeto en la matriz `body`.

```csharp
System.Object[] body = new System.Object[] { new { Text = @"Elephants" } };
var requestBody = JsonConvert.SerializeObject(body);
```



## <a name="instantiate-the-client-and-make-a-request"></a>Creación de una instancia de cliente y realización de una solicitud

Estas líneas crean instancias de `HttpClient` y `HttpRequestMessage`:

```csharp
using (var client = new HttpClient())
using (var request = new HttpRequestMessage())
{
  // In the next few sections you'll add code to construct the request.
}
```

## <a name="construct-the-request-and-print-the-response"></a>Construcción de la solicitud e impresión de la respuesta

Dentro de `HttpRequestMessage`:

* Declarará el método HTTP
* Construirá el URI de solicitud
* Insertará el cuerpo de la solicitud (el objeto JSON serializado)
* Agregará los encabezados necesarios
* Realizará una solicitud asincrónica
* Impresión de la respuesta

Agregue este código a `HttpRequestMessage`:

```csharp
// Set the method to POST
request.Method = HttpMethod.Post;

// Construct the full URI
request.RequestUri = new Uri(host + route);

// Add the serialized JSON object to your request
request.Content = new StringContent(requestBody, Encoding.UTF8, "application/json");

// Add the authorization header
request.Headers.Add("Ocp-Apim-Subscription-Key", subscriptionKey);

// Send request, get response
var response = client.SendAsync(request).Result;
var jsonResponse = response.Content.ReadAsStringAsync().Result;

// Print the response
Console.WriteLine(PrettyPrint(jsonResponse));
Console.WriteLine("Press any key to continue.");
```

Agregue `PrettyPrint` para aplicar formato a la respuesta JSON:
```csharp
static string PrettyPrint(string s)
{
    return JsonConvert.SerializeObject(JsonConvert.DeserializeObject(s), Formatting.Indented);
}
```

Si usa una suscripción a varios servicios de Cognitive Services, también debe incluir `Ocp-Apim-Subscription-Region` en los parámetros de la solicitud. [Más información sobre la autenticación con la suscripción a varios servicios](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication).

## <a name="put-it-all-together"></a>Colocación de todo junto

El último paso consiste en llamar a `AltTranslation()` en la función `Main`. Busque `static void Main(string[] args)` y agregue estas líneas:

```csharp
AltTranslation();
Console.ReadLine();
```

## <a name="run-the-sample-app"></a>Ejecutar la aplicación de ejemplo

Eso es todo, ya está listo para ejecutar la aplicación de ejemplo. Desde la línea de comandos (o sesión de terminal), vaya al directorio del proyecto y ejecute:

```console
dotnet run
```

## <a name="sample-response"></a>Respuesta de muestra

```json
[
    {
        "displaySource": "elephants",
        "normalizedSource": "elephants",
        "translations": [
            {
                "backTranslations": [
                    {
                        "displayText": "elephants",
                        "frequencyCount": 1207,
                        "normalizedText": "elephants",
                        "numExamples": 5
                    }
                ],
                "confidence": 1.0,
                "displayTarget": "elefantes",
                "normalizedTarget": "elefantes",
                "posTag": "NOUN",
                "prefixWord": ""
            }
        ]
    }
]
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Asegúrese de quitar cualquier información confidencial del código fuente de la aplicación de ejemplo como, por ejemplo, las claves de suscripción.

## <a name="next-steps"></a>Pasos siguientes

Eche un vistazo a la referencia de API para comprender todo lo que puede hacer con Translator Text API.

> [!div class="nextstepaction"]
> [Referencia de API](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference)

## <a name="see-also"></a>Otras referencias

* [Traducir texto](quickstart-csharp-translate.md)
* [Transliterar texto](quickstart-csharp-transliterate.md)
* [Identificar el idioma de entrada](quickstart-csharp-detect.md)
* [Obtener una lista de idiomas admitidos](quickstart-csharp-languages.md)
* [Determinar la longitud de las oraciones de una entrada](quickstart-csharp-sentences.md)
