---
title: 'Guía de inicio rápido: Project URL Preview, JavaScript'
titlesuffix: Azure Cognitive Services
description: Ejemplo de script para empezar a usar rápidamente Bing URL Preview API con JavaScript.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: url-preview
ms.topic: quickstart
ms.date: 03/16/2018
ms.author: rosh
ms.openlocfilehash: f36609448819ed197cb92c0bc4d9cc0237fe6df8
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2018
ms.locfileid: "49466943"
---
# <a name="quickstart-url-preview-in-javascript"></a>Guía de inicio rápido: URL Preview en JavaScript 

La siguiente aplicación de página única utiliza JavaScript para crear una instancia de URL Preview para el sitio de SwiftKey: https://swiftkey.com/en. 

## <a name="prerequisites"></a>Requisitos previos

Obtenga una clave de acceso para la evaluación gratuita de los [Laboratorios de Cognitive Services](https://labs.cognitive.microsoft.com/en-us/project-url-preview).

## <a name="code-scenario"></a>Escenario de código
En el ejemplo de JavaScript siguiente se incluye un objeto de entrada de cuadro de texto donde el usuario escribe la dirección URL de la que se va a obtener una vista previa.  Cuando el usuario hace clic en el botón **Vista previa**, el método OnClick se enruta a `getPreview`, donde el código genera una solicitud web para el punto de conexión **UrlPreview**.

El código crea un objeto *XMLHttpRequest*, agrega el encabezado y la clave *Ocp-Apim-Subscription-Key*, y envía la solicitud.  Agrega un controlador de eventos asincrónicos para procesar la respuesta.

Si la respuesta se devuelve correctamente, el controlador asigna el texto JSON de la respuesta al párrafo `demo` en la página. Otros elementos de la respuesta se establecen en los párrafos siguientes para su presentación.

**Respuesta JSON sin formato**

````
{
  "_type": "WebPage",
  "name": "SwiftKey - Smart prediction technology for easier mobile typing",
  "url": "https://swiftkey.com/en",
  "description": "Discover the best Android and iPhone and iPad apps for faster, easier typing with emoji, colorful themes and more - download SwiftKey Keyboard free today.",
  "isFamilyFriendly": true,
  "primaryImageOfPage": {
    "contentUrl": "https://swiftkey.com/images/og/default.jpg"
  }
}

````

**Demostración en ejecución**

![Ejemplo de URL Preview de JavaScript](./media/java-script-demo.png)

## <a name="running-the-application"></a>Ejecución de la aplicación

Para ejecutar la aplicación:

1. Reemplace el valor `YOUR-SUBSCRIPTION-KEY` por una clave de acceso válida para la suscripción.
2. Guarde el código HTML y el script en un archivo con la extensión .html.
3. Ejecute la página web en un explorador.
4. Use la dirección URL existente, o especifique otra en el cuadro de texto.
5. Haga clic en el botón **Vista previa**.

**Código fuente:**

```
<!DOCTYPE html>

<html lang="en" xmlns="http://www.w3.org/1999/xhtml">
<head>
    <meta charset="utf-8" />
    <title>urlPreview Demo</title>
</head>
<body>
    <h3>URL Preview Demo</h3>

    Page to preview: <input type="url" id="myURL" value="https://swiftkey.com/en">
    <button onclick="getPreview()">Preview</button>

    <p id="demo"></p>
    <br />
    <p id="jsonDesc"></p>
    <p><a id="familyFriendly"></a></p>
    <a id="contentUrl"></a>
    <p />
    <img id="jsonImage" />

    <script>

        var BING_ENDPOINT = "https://api.labs.cognitive.microsoft.com/urlpreview/v7.0/search"; 
        var key = "YOUR-SUBSCRIPTION-KEY";
        var xhr;

        function getPreview() {
            xhr = new XMLHttpRequest();

            var queryUrl = BING_ENDPOINT + "?q=" +
                encodeURIComponent(document.getElementById("myURL").value);
            xhr.open('GET', queryUrl, true);
            xhr.setRequestHeader("Ocp-Apim-Subscription-Key", key);

            xhr.send();

            xhr.addEventListener("readystatechange", processRequest, false);
        }

        function processRequest(e) {

            if (xhr.readyState == 4 && xhr.status == 200) {
                document.getElementById("demo").innerHTML = xhr.responseText;
                var obj = JSON.parse(xhr.responseText);
                document.getElementById("jsonDesc").innerHTML = obj.description;
                document.getElementById("familyFriendly").innerHTML = "Family Friendly: " + obj.isFamilyFriendly;
                document.getElementById("contentUrl").innerHTML = obj.url;
                document.getElementById("contentUrl").href = obj.url;
                document.getElementById("jsonImage").width = 350;
                document.getElementById("jsonImage").src = obj.primaryImageOfPage.contentUrl;

            }

        }

    </script>

</body>
</html>

```

## <a name="next-steps"></a>Pasos siguientes
- [Inicio rápido de C#](csharp.md)
- [Inicio rápido de Java](java-quickstart.md)
- [Inicio rápido de Node](node-quickstart.md)
- [Inicio rápido de Python](python-quickstart.md)
