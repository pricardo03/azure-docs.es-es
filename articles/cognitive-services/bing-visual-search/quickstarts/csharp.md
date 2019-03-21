---
title: 'Inicio rápido: Obtención de conclusiones de imágenes mediante la API de REST Bing Visual Search y C#'
titleSuffix: Azure Cognitive Services
description: Aprenda a cargar una imagen en Bing Visual Search API y obtener conclusiones sobre ella.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: quickstart
ms.date: 5/16/2018
ms.author: scottwhi
ms.openlocfilehash: 7961fb05f7ca9c6e6b61330e7dff53f2d5a41001
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/07/2019
ms.locfileid: "57535321"
---
# <a name="quickstart-get-image-insights-using-the-bing-visual-search-rest-api-and-c"></a>Inicio rápido: Obtención de conclusiones de imágenes mediante la API de REST Bing Visual Search y C#

Use este inicio rápido para realizar la primera llamada a Bing Visual Search API y ver los resultados de la búsqueda. Esta sencilla aplicación de C# carga una imagen en la API y muestra la información que se devuelve sobre ella.

## <a name="prerequisites"></a>Requisitos previos

* Cualquier edición de [Visual Studio 2017](https://www.visualstudio.com/downloads/).
* El marco [Json.NET](https://www.newtonsoft.com/json), disponible como un paquete NuGet.
* Si usa Linux/MacOS, esta aplicación puede ejecutarse con [Mono](https://www.mono-project.com/).

[!INCLUDE [cognitive-services-bing-visual-search-signup-requirements](../../../../includes/cognitive-services-bing-visual-search-signup-requirements.md)]

## <a name="create-and-initialize-a-project"></a>Creación e inicialización de un proyecto

1. Cree una solución de consola denominada `BingSearchApisQuickStart` en Visual Studio. Después, agregue los siguientes espacios de nombres en el archivo de código principal.

    ```csharp
    using System;
    using System.Text;
    using System.Net;
    using System.IO;
    using System.Collections.Generic;
    ```

2. Cree variables para la clave de suscripción, el punto de conexión y la ruta de acceso a la imagen que quiere cargar.

    ```csharp
        const string accessKey = "<yoursubscriptionkeygoeshere>";
        const string uriBase = "https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch";
        static string imagePath = @"<pathtoimagegoeshere>";
    ```


1. Cree un método llamado `GetImageFileName()` para obtener la ruta de acceso a la imagen.
    
    ```csharp
    static string GetImageFileName(string path)
            {
                return new FileInfo(path).Name;
            }
    ```

2. Cree un método para obtener los caracteres binarios de la imagen.

    ```csharp
    static byte[] GetImageBinary(string path)
    {
        return File.ReadAllBytes(path);
    }
    ```

## <a name="build-the-form-data"></a>Creación de los datos del formulario

Al cargar una imagen local, los datos de formulario enviados a la API deben tener el formato correcto. Debe incluir el encabezado Content-Disposition. Asimismo, su parámetro `name` se debe establecer en "imagen" y el parámetro `filename` puede establecerse en cualquier cadena. El contenido del formulario incluye el archivo binario de la imagen. Recuerde que el tamaño de imagen máximo que puede cargar es de 1 MB.

    ```
    --boundary_1234-abcd
    Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"
    
    ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...
    
    --boundary_1234-abcd--
    ```

1. Para aplicar formato a los datos de formulario, agregue cadenas de límite para dar formato a los datos del formulario POST correctamente, que determinan el principio, el final y los caracteres de nueva línea de los datos.

    ```csharp
    // Boundary strings for form data in body of POST.
    const string CRLF = "\r\n";
    static string BoundaryTemplate = "batch_{0}";
    static string StartBoundaryTemplate = "--{0}";
    static string EndBoundaryTemplate = "--{0}--";
    ```

2. Las variables siguientes se usarán para agregar parámetros a los datos del formulario. 

    ```csharp
    const string CONTENT_TYPE_HEADER_PARAMS = "multipart/form-data; boundary={0}";
    const string POST_BODY_DISPOSITION_HEADER = "Content-Disposition: form-data; name=\"image\"; filename=\"{0}\"" + CRLF +CRLF;
    ```

3. Cree una función denominada `BuildFormDataStart()` para crear la parte inicial de los datos del formulario necesarios, con las cadenas de límite y la ruta de acceso a la imagen.
    
    ```csharp
        static string BuildFormDataStart(string boundary, string filename)
        {
            var startBoundary = string.Format(StartBoundaryTemplate, boundary);

            var requestBody = startBoundary + CRLF;
            requestBody += string.Format(POST_BODY_DISPOSITION_HEADER, filename);

            return requestBody;
        }
    ```

4. Genere una función llamada `BuildFormDataEnd()` para crear la parte final de los datos del formulario necesarios, con las cadenas de límite.
    
    ```csharp
        static string BuildFormDataEnd(string boundary)
        {
            return CRLF + CRLF + string.Format(EndBoundaryTemplate, boundary) + CRLF;
        }
    ```

## <a name="call-the-bing-visual-search-api"></a>Llamada a Bing Visual Search API

1. Cree una función para llamar al punto de conexión de Bing Visual Search y devolver la respuesta JSON. La función debe tener las partes inicial y final de los datos del formulario, una matriz de bytes que contenga los datos de la imagen y un valor contentType.

2. Use `WebRequest` para almacenar el identificador URI, el valor contentType y los encabezados.  

3. Use `request.GetRequestStream()` para escribir los datos del formulario y la imagen. Después, obtenga la respuesta. Esta función debe ser similar al código siguiente:
        
    ```csharp
        static string BingImageSearch(string startFormData, string endFormData, byte[] image, string contentTypeValue)
        {
            WebRequest request = HttpWebRequest.Create(uriBase);
            request.ContentType = contentTypeValue;
            request.Headers["Ocp-Apim-Subscription-Key"] = accessKey;
            request.Method = "POST";

            // Writes the boundary and Content-Disposition header, then writes
            // the image binary, and finishes by writing the closing boundary.
            using (Stream requestStream = request.GetRequestStream())
            {
                StreamWriter writer = new StreamWriter(requestStream);
                writer.Write(startFormData);
                writer.Flush();
                requestStream.Write(image, 0, image.Length);
                writer.Write(endFormData);
                writer.Flush();
                writer.Close();
            }

            HttpWebResponse response = (HttpWebResponse)request.GetResponseAsync().Result;
            string json = new StreamReader(response.GetResponseStream()).ReadToEnd();

            return json;
        }
    ```

## <a name="create-the-main-method"></a>Creación del método principal

1. En el método principal de la aplicación, obtenga el nombre de archivo y el archivo binario de la imagen. 

    ```csharp
    var filename = GetImageFileName(imagePath);
    var imageBinary = GetImageBinary(imagePath);
    ```

2. Configure el cuerpo POST dando formato a su límite. A continuación, llame a `startFormData()` y `endFormData` para crear los datos del formulario. 

    ```csharp
    // Set up POST body.
    var boundary = string.Format(BoundaryTemplate, Guid.NewGuid());
    var startFormData = BuildFormDataStart(boundary, filename);
    var endFormData = BuildFormDataEnd(boundary);
    ```

3. Cree el valor ContentType dando formato a `CONTENT_TYPE_HEADER_PARAMS` y al límite de los datos del formulario.

    ```csharp
    var contentTypeHdrValue = string.Format(CONTENT_TYPE_HEADER_PARAMS, boundary);
    ```

4. Obtenga la respuesta de la API mediante una llamada a `BingImageSearch()`. Luego, imprima la respuesta.

    ```csharp
    var json = BingImageSearch(startFormData, endFormData, imageBinary, contentTypeHdrValue);
    Console.WriteLine(json);
    Console.WriteLine("enter any key to continue");
    Console.readKey();
    ```

## <a name="using-httpclient"></a>Usar HttpClient

Si usa HttpClient, puede usar MultipartFormDataContent para compilar los datos del formulario. Puede usar las siguientes secciones de código para reemplazar los métodos con el mismo nombre en el ejemplo anterior.

Reemplace el método Main con el código siguiente:

```csharp
        static void Main()
        {
            try
            {
                Console.OutputEncoding = System.Text.Encoding.UTF8;

                if (accessKey.Length == 32)
                {
                    if (IsImagePathSet(imagePath))
                    {
                        var filename = GetImageFileName(imagePath);
                        Console.WriteLine("Getting image insights for image: " + filename);
                        var imageBinary = GetImageBinary(imagePath);

                        var boundary = string.Format(BoundaryTemplate, Guid.NewGuid());
                        var json = BingImageSearch(imageBinary, boundary, uriBase, accessKey);

                        Console.WriteLine("\nJSON Response:\n");
                        Console.WriteLine(JsonPrettyPrint(json));
                    }
                }
                else
                {
                    Console.WriteLine("Invalid Bing Visual Search API subscription key!");
                    Console.WriteLine("Please paste yours into the source code.");
                }

                Console.Write("\nPress Enter to exit ");
                Console.ReadLine();
            }
            catch (Exception e)
            {
                Console.WriteLine(e.Message);
            }
        }
```

Reemplace el método BingImageSearch con el código siguiente:

```csharp
        /// <summary>
        /// Calls the Bing visual search endpoint and returns the JSON response.
        /// </summary>
        static string BingImageSearch(byte[] image, string boundary, string uri, string subscriptionKey)
        {
            var requestMessage = new HttpRequestMessage(HttpMethod.Post, uri);
            requestMessage.Headers.Add("Ocp-Apim-Subscription-Key", accessKey);

            var content = new MultipartFormDataContent(boundary);
            content.Add(new ByteArrayContent(image), "image", "myimage");
            requestMessage.Content = content;

            var httpClient = new HttpClient();

            Task<HttpResponseMessage> httpRequest = httpClient.SendAsync(requestMessage, HttpCompletionOption.ResponseContentRead, CancellationToken.None);
            HttpResponseMessage httpResponse = httpRequest.Result;
            HttpStatusCode statusCode = httpResponse.StatusCode;
            HttpContent responseContent = httpResponse.Content;

            string json = null;

            if (responseContent != null)
            {
                Task<String> stringContentsTask = responseContent.ReadAsStringAsync();
                json = stringContentsTask.Result;
            }

            return json;
        }
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Creación de una página web de Custom Search](../tutorial-bing-visual-search-single-page-app.md)
