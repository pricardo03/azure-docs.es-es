---
title: 'Inicio rápido: Reconocer la entrada de lápiz digital con la API de REST de módulo de reconocimiento de tinta y Node.js'
description: Use la API de reconocimiento de tinta para iniciar reconocer trazos de tinta digital.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: ink-recognizer
ms.topic: article
ms.date: 05/02/2019
ms.author: aahi
ms.openlocfilehash: 651474fd538123e760022ac59efbbaf0b9b83d70
ms.sourcegitcommit: 17411cbf03c3fa3602e624e641099196769d718b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/10/2019
ms.locfileid: "65519671"
---
# <a name="quickstart-recognize-digital-ink-with-the-ink-recognizer-rest-api-and-javascript"></a>Inicio rápido: Reconocer la entrada de lápiz digital con la API de REST de módulo de reconocimiento de tinta y JavaScript

Use esta guía de inicio rápido para empezar a usar la API de reconocimiento de tinta en trazos de tinta digital. Esta aplicación de JavaScript envía una solicitud de API que contiene los datos del trazo de tinta con formato JSON y muestra la respuesta.

Aunque esta aplicación está escrita en Javascript y se ejecuta en el explorador web, la API es un servicio web RESTful compatible con la mayoría de lenguajes de programación.

Normalmente se llamaría a la API desde una aplicación de entrada de lápiz digital. En este tutorial rápido envía los datos del trazo de tinta para el siguiente ejemplo escritas a mano desde un archivo JSON.

![una imagen de texto escrito a mano](../media/handwriting-sample.jpg)

El código fuente de este inicio rápido está disponible en [GitHub](https://go.microsoft.com/fwlink/?linkid=2089905).

## <a name="prerequisites"></a>Requisitos previos

- Un explorador web
- Los datos del trazo de tinta de ejemplo para este inicio rápido pueden encontrarse en [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/javascript/InkRecognition/quickstart/example-ink-strokes.json).


[!INCLUDE [cognitive-services-ink-recognizer-signup-requirements](../../../../includes/cognitive-services-ink-recognizer-signup-requirements.md)]

## <a name="create-a-new-application"></a>Creación de una aplicación

1. En el IDE o editor favorito, cree un nuevo `.html` archivo. A continuación, agregarle HTML básico para el código que se agregará más adelante.
    
    ```html
    <!DOCTYPE html>
    <html>
    
        <head>
            <script type="text/javascript">
            </script>
        </head>
        
        <body>
        </body>
    
    </html>
    ```

2. Dentro de la `<body>` etiqueta, agregue el siguiente código html:
    1. Dos áreas de texto para mostrar la solicitud JSON y la respuesta.
    2. Un botón para llamar a la `recognizeInk()` función que se crearán más adelante.
    
    ```HTML
    <!-- <body>-->
        <h2>Send a request to the Ink Recognition API</h2>
        <p>Request:</p>
        <textarea id="request" style="width:800px;height:300px"></textarea>
        <p>Response:</p>
        <textarea id="response" style="width:800px;height:300px"></textarea>
        <br>
        <button type="button" onclick="recognizeInk()">Recognize Ink</button>
    <!--</body>-->
    ```

## <a name="load-the-example-json-data"></a>Cargar lo datos JSON de ejemplo

1. Dentro de la `<script>` etiquetar, cree una variable para el sampleJson. A continuación, cree una función de JavaScript denominada `openFile()` que abre un explorador de archivos para que pueda seleccionar el archivo JSON. Cuando el `Recognize ink` se hace clic en el botón, se llama a esta función y empezar a leer el archivo.
2. Use un `FileReader` del objeto `onload()` función para procesar el archivo de forma asincrónica. 
    1. Reemplazar cualquier `\n` o `\r` caracteres en el archivo con una cadena vacía. 
    2. Use `JSON.parse()` para convertir el texto en un archivo JSON válido
    3. Actualización de la `request` cuadro de texto de la aplicación. Use `JSON.stringify()` para dar formato a la cadena JSON. 
    
    ```javascript
    var sampleJson = "";
    function openFile(event) {
        var input = event.target;
    
        var reader = new FileReader();
        reader.onload = function(){
            sampleJson = reader.result.replace(/(\\r\\n|\\n|\\r)/gm, "");
            sampleJson = JSON.parse(sampleJson);
            document.getElementById('request').innerHTML = JSON.stringify(sampleJson, null, 2);
        };
        reader.readAsText(input.files[0]);
    };
    ```

## <a name="send-a-request-to-the-ink-recognizer-api"></a>Enviar una solicitud a la API de reconocimiento de tinta

1. Dentro de la `<script>` etiquetar, cree una función denominada `recognizeInk()`. Esta función más adelante llamará a la API y actualice la página con la respuesta. Agregue el código de los pasos siguientes en esta función. 
        
    ```javascript
    function recognizeInk() {
    // add the code from the below steps here 
    }
    ```

    1. Cree variables para la dirección URL del punto de conexión, la clave de suscripción y el ejemplo de JSON. A continuación, cree un `XMLHttpRequest` objeto va a enviar la solicitud de API. 
        
        ```javascript
        // Replace the below URL with the correct one for your subscription. 
        // Your endpoint can be found in the Azure portal. For example: https://westus2.api.cognitive.microsoft.com
        var SERVER_ADDRESS = "YOUR-SUBSCRIPTION-URL";
        var ENDPOINT_URL = SERVER_ADDRESS + "/inkrecognizer/v1.0-preview/recognize";
        // Replace the subscriptionKey string value with your valid subscription key.
        var SUBSCRIPTION_KEY = "YOUR-SUBSCRIPTION-KEY";
        var xhttp = new XMLHttpRequest();
        ```
    2. Crear la función de devolución para la `XMLHttpRequest` objeto. Esta función se analizar la respuesta de API desde una solicitud correcta y mostrarla en la aplicación. 
            
        ```javascript
        function returnFunction(xhttp) {
            var response = JSON.parse(xhttp.responseText);
            console.log("Response: %s ", response);
            document.getElementById('response').innerHTML = JSON.stringify(response, null, 2);
        }
        ```
    3. Cree la función de error para el objeto de solicitud. Esta función registra el error en la consola. 
            
        ```javascript
        function errorFunction() {
            console.log("Error: %s, Detail: %s", xhttp.status, xhttp.responseText);
        }
        ```

    4. Creación de una función para el objeto de solicitud `onreadystatechange` propiedad. Cuando se cambia el estado de preparación del objeto de solicitud, se aplicarán las funciones de valor devuelto y de error anteriores.
            
        ```javascript
        xhttp.onreadystatechange = function () {
            if (this.readyState === 4) {
                if (this.status === 200) {
                    returnFunction(xhttp);
                } else {
                    errorFunction(xhttp);
                }
            }
        };
        ```
    
    5. Envíe la solicitud de API. Agregue su clave de suscripción para la `Ocp-Apim-Subscription-Key` encabezado y establezca el `content-type` a `application/json`
    
        ```javascript
        xhttp.open("PUT", ENDPOINT_URL, true);
        xhttp.setRequestHeader("Ocp-Apim-Subscription-Key", SUBSCRIPTION_KEY);
        xhttp.setRequestHeader("content-type", "application/json");
        xhttp.send(JSON.stringify(sampleJson));
        };

## Run the application and view the response

This application can be run within your web browser. A successful response is returned in JSON format. You can also find the JSON response on [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/javascript/InkRecognition/quickstart/example-response.json):

## Next steps

> [!div class="nextstepaction"]
> [REST API reference](https://go.microsoft.com/fwlink/?linkid=2089907)

To see how the Ink Recognition API works in a digital inking app, take a look at the following sample applications on GitHub:
* [C# and Universal Windows Platform(UWP)](https://go.microsoft.com/fwlink/?linkid=2089803)  
* [C# and Windows Presentation Foundation(WPF)](https://go.microsoft.com/fwlink/?linkid=2089804)
* [Javascript web-browser app](https://go.microsoft.com/fwlink/?linkid=2089908)       
* [Java and Android mobile app](https://go.microsoft.com/fwlink/?linkid=2089906)
* [Swift and iOS mobile app](https://go.microsoft.com/fwlink/?linkid=2089805)
