---
title: 'Guía de inicio rápido: Conversión de texto a voz (.NET Core) - Servicio Voz'
titleSuffix: Azure Cognitive Services
description: En este tutorial, obtendrá información sobre cómo convertir texto a voz con la API REST Texto a voz. El texto de ejemplo incluido en esta guía se estructura como lenguaje de marcado de síntesis de voz (SSML). Esto le permite elegir la voz y el idioma de la respuesta de voz.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 11/28/2018
ms.author: erhopf
ms.openlocfilehash: 4f424e73fbe1f05155e7f051d90dd72d57405adf
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/30/2018
ms.locfileid: "52641574"
---
# <a name="quickstart-convert-text-to-speech-using-net-core"></a>Inicio rápido: Conversión de texto a voz con .NET Core

En este tutorial, obtendrá información sobre cómo convertir texto a voz con .NET Core y la API REST Texto a voz. El texto de ejemplo incluido en esta guía se estructura como [lenguaje de marcado de síntesis de voz (SSML)](speech-synthesis-markup.md), que permite elegir la voz y el idioma de la respuesta.

En esta guía de inicio rápido, se requiere una [cuenta de Azure Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) con un recurso del servicio Voz. Si no tiene una cuenta, puede usar la [evaluación gratuita](https://azure.microsoft.com/try/cognitive-services/) para obtener una clave de suscripción.

## <a name="prerequisites"></a>Requisitos previos

Esta guía de inicio rápido requiere:

* [SDK de .NET](https://www.microsoft.com/net/learn/dotnet/hello-world-tutorial)
* [Visual Studio](https://visualstudio.microsoft.com/downloads/), [Visual Studio Code](https://code.visualstudio.com/download) o su editor favorito de código
* Una clave de suscripción de Azure para el servicio Voz

## <a name="create-a-net-core-project"></a>Creación de un proyecto de .NET Core

Abra un nuevo símbolo del sistema (o una sesión de terminal) y ejecute estos comandos:

```console
dotnet new console -o tts-sample
cd tts-sample
```

El primer comando hace dos cosas. Crea una nueva aplicación de consola .NET y crea un directorio denominado `tts-sample`. El segundo comando cambia al directorio del proyecto.

## <a name="select-the-c-language-version"></a>Selección de la versión del lenguaje C#

Esta guía de inicio rápido requiere C# 7.1 o posterior. Hay algunas maneras de cambiar la versión de C# del proyecto. En esta guía le mostraremos cómo ajustar el archivo `tts-sample.csproj`. Para todas las opciones disponibles, como el cambio de idioma en Visual Studio, consulte [Selección de la versión del lenguaje C#](https://docs.microsoft.com/dotnet/csharp/language-reference/configure-language-version).

Abra el proyecto y, después, abra `tts-sample.csproj`. Asegúrese de que `LangVersion` está establecido en 7.1 o posterior. Si no hay un grupo de propiedades para la versión del lenguaje, añada estas líneas:

```csharp
<PropertyGroup>
   <LangVersion>7.1</LangVersion>
</PropertyGroup>
```

Asegúrese de guardar los cambios.

## <a name="add-required-namespaces-to-your-project"></a>Incorporación de los espacios de nombres necesarios al proyecto

El comando `dotnet new console` que ejecutó anteriormente creó un proyecto que incluía `Program.cs`. En este archivo es donde deberá colocar el código de la aplicación. Abra `Program.cs` y reemplace las instrucciones using existentes. Estas instrucciones garantizan que tiene acceso a todos los tipos necesarios para compilar y ejecutar la aplicación de ejemplo.

```csharp
using System;
using System.Net.Http;
using System.Text;
using System.IO;
using System.Threading.Tasks;
```

## <a name="create-a-class-for-token-exchange"></a>Creación de una clase para el intercambio de tokens

La API REST Texto a voz requiere un token de acceso para la autenticación. Para obtener un token de acceso, es necesario un intercambio. En este ejemplo se intercambia la clave de suscripción de Speech Service para obtener un token de acceso usando el punto de conexión `issueToken`.

En este ejemplo se da por supuesto que su suscripción de Speech Service está en la región Oeste de EE. UU. Si usa una región diferente, actualice el valor de `FetchTokenUri`. Para obtener una lista completa, consulte [Regiones](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#rest-apis).

```csharp
public class Authentication
{
    private string subscriptionKey;
    private string tokenFetchUri;

    public Authentication(string tokenFetchUri, string subscriptionKey)
    {
        if (string.IsNullOrWhiteSpace(tokenFetchUri))
        {
            throw new ArgumentNullException(nameof(tokenFetchUri));
        }
        if (string.IsNullOrWhiteSpace(subscriptionKey))
        {
            throw new ArgumentNullException(nameof(subscriptionKey));
        }
        this.tokenFetchUri = tokenFetchUri;
        this.subscriptionKey = subscriptionKey;
    }

    public async Task<string> FetchTokenAsync()
    {
        using (var client = new HttpClient())
        {
            client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", this.subscriptionKey);
            UriBuilder uriBuilder = new UriBuilder(this.tokenFetchUri);

            var result = await client.PostAsync(uriBuilder.Uri.AbsoluteUri, null).ConfigureAwait(false);
            return await result.Content.ReadAsStringAsync().ConfigureAwait(false);
        }
    }
}
```

> [!NOTE]
> Para más información, consulte [Cómo obtener un token de acceso](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis#how-to-get-an-access-token).

## <a name="get-an-access-token-and-set-the-host-url"></a>Obtención de un token de acceso y establecimiento de la dirección URL del host

Busque `static void Main(string[] args)` y reemplácelo por `static async Task Main(string[] args)`.

A continuación, copie este código en el método main. Hace algunas cosas, pero lo más importante es que toma texto como entrada y llama a la función `Authentication` para cambiar la clave de suscripción por un token de acceso. Si algo va mal, el error se imprime en la consola.

No se olvide de agregar la clave de suscripción antes de ejecutar la aplicación.

```csharp
// Prompts the user to input text for TTS conversion
Console.Write("What would you like to convert to speech? ");
string text = Console.ReadLine();

// Gets an access token
string accessToken;
Console.WriteLine("Attempting token exchange. Please wait...\n");

// Add your subscription key here
// If your resource isn't in WEST US, change the endpoint
Authentication auth = new Authentication("https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken", "YOUR_SUBSCRIPTION_KEY");
try
{
    accessToken = await auth.FetchTokenAsync().ConfigureAwait(false);
    Console.WriteLine("Successfully obtained an access token. \n");
}
catch (Exception ex)
{
    Console.WriteLine("Failed to obtain an access token.");
    Console.WriteLine(ex.ToString());
    Console.WriteLine(ex.Message);
    return;
}
```

A continuación, establezca el host y la ruta para texto a voz:

```csharp
string host = "https://westus.tts.speech.microsoft.com/cognitiveservices/v1";
```

## <a name="build-the-ssml-request"></a>Creación de la solicitud SSML

Texto que se envía como cuerpo de una solicitud `POST`. Con SSML puede especificar el idioma y la voz. En esta guía de inicio rápido, vamos a usar SSML con el idioma ajustado a `en-US` y la voz ajustada a `ZiraRUS`. Vamos a construir el SSML para su solicitud:

```csharp
string body = @"<speak version='1.0' xmlns='http://www.w3.org/2001/10/synthesis' xml:lang='en-US'>
              <voice name='Microsoft Server Speech Text to Speech Voice (en-US, ZiraRUS)'>" +
              text + "</voice></speak>";
```

> [!NOTE]
> Este ejemplo se utiliza la fuente de voz `ZiraRUS`. Para obtener una lista completa de los idiomas y las voces que proporciona Microsoft, consulte [Compatibilidad con idiomas](https://review.docs.microsoft.com/azure/cognitive-services/speech-service/language-support). Si está interesado en crear una voz única y reconocible para su marca, vea [Crear fuentes de voz personalizada](https://review.docs.microsoft.com/azure/cognitive-services/speech-service/how-to-customize-voice-font).

## <a name="instantiate-the-client-make-a-request-and-save-synthesized-audio-to-a-file"></a>Creación de una instancia del cliente, realización de una solicitud y guardado del audio sintetizado en un archivo

Hay mucho que hacer en este ejemplo de código. Veamos rápidamente lo que sucede:

* Se crea una instancia de la solicitud y del cliente.
* El método HTTP se establece como `POST`.
* Los encabezados necesarios se agregan a la solicitud.
* La solicitud se envía y se comprueba el código de estado.
* La respuesta se lee de forma asincrónica y se escribe en un archivo denominado sample.wav.

Copie este código en el proyecto. Asegúrese de reemplazar el valor del encabezado `User-Agent` por el nombre del recurso de Azure Portal.

```csharp
using (var client = new HttpClient())
{
    using (var request = new HttpRequestMessage())
    {
        // Set the HTTP method
        request.Method = HttpMethod.Post;
        // Construct the URI
        request.RequestUri = new Uri(host);
        // Set the content type header
        request.Content = new StringContent(body, Encoding.UTF8, "application/ssml+xml");
        // Set additional header, such as Authorization and User-Agent
        request.Headers.Add("Authorization", "Bearer " + accessToken);
        request.Headers.Add("Connection", "Keep-Alive");
        // Update your resource name
        request.Headers.Add("User-Agent", "YOUR_RESOURCE_NAME");
        request.Headers.Add("X-Microsoft-OutputFormat", "riff-24khz-16bit-mono-pcm");
        request.Headers.Add("Connection", "Keep-Alive");
        // Create a request
        Console.WriteLine("Calling the TTS service. Please wait... \n");
        using (var response = await client.SendAsync(request).ConfigureAwait(false))
        {
            response.EnsureSuccessStatusCode();
            // Asynchronously read the response
            using (var dataStream = await response.Content.ReadAsStreamAsync().ConfigureAwait(false))
            {
                Console.WriteLine("Your speech file is being written to file...");
                using (var fileStream = new FileStream(@"sample.wav", FileMode.Create, FileAccess.Write, FileShare.Write))
                {
                    await dataStream.CopyToAsync(fileStream).ConfigureAwait(false);
                    fileStream.Close();
                }
                Console.WriteLine("\nYour file is ready. Press any key to exit.");
                Console.ReadLine();
            }
        }
    }
}
```

## <a name="run-the-sample-app"></a>Ejecutar la aplicación de ejemplo

Eso es todo; ya está listo para ejecutar la aplicación de Texto a voz. Desde la línea de comandos (o sesión de terminal), vaya al directorio del proyecto y ejecute:

```console
dotnet run
```

Si se realiza correctamente, el archivo se guardará en la carpeta del proyecto. Reprodúzcalo con el reproductor que prefiera.

## <a name="clean-up-resources"></a>Limpieza de recursos

Si ha codificado de forma rígida la clave de suscripción en el programa, asegúrese de quitarla cuando haya terminado con esta guía de inicio rápido.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Referencia de Text-to-speech API](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis#text-to-speech-api)

## <a name="see-also"></a>Otras referencias

* [Tutorial: Reconocimiento de las intenciones de voz](how-to-recognize-intents-from-speech-csharp.md)
