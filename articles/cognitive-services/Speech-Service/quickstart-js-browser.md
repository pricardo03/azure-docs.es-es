---
title: 'Inicio rápido: Reconocimiento de voz, JavaScript (explorador): servicios de Voz'
titleSuffix: Azure Cognitive Services
description: Aprenda a reconocer la voz en JavaScript en un explorador mediante el SDK de Voz
services: cognitive-services
author: fmegen
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 2/20/2019
ms.author: fmegen
ms.openlocfilehash: a418966ce72876d58d39b939ecb7dd5a05e41b9b
ms.sourcegitcommit: e43ea344c52b3a99235660960c1e747b9d6c990e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/04/2019
ms.locfileid: "59008993"
---
# <a name="quickstart-recognize-speech-in-javascript-in-a-browser-using-the-speech-sdk"></a>Inicio rápido: Reconocimiento de voz en JavaScript en un explorador mediante el SDK de Voz

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

En este artículo, aprenderá a crear un sitio web mediante el enlace de JavaScript del SDK de Voz de Cognitive Services para transcribir voz a texto.
La aplicación se basa en el SDK de Voz para JavaScript ([descargar la versión 1.4.0](https://aka.ms/csspeech/jsbrowserpackage)).

## <a name="prerequisites"></a>Requisitos previos

* Clave de suscripción para Speech Service. Vea [Prueba gratuita de los servicios de Voz](get-started.md).
* Un PC o Mac con un micrófono operativo.
* Un editor de texto.
* Una versión actual de Chrome, Microsoft Edge o Safari.
* Opcionalmente, un servidor web que admite el hospedaje de scripts PHP.

## <a name="create-a-new-website-folder"></a>Creación de una nueva carpeta Website

Cree una nueva carpeta vacía. En caso de que desee hospedar el ejemplo en un servidor web, asegúrese de que este puede acceder a la carpeta.

## <a name="unpack-the-speech-sdk-for-javascript-into-that-folder"></a>Desempaquete el SDK de Voz para JavaScript en esa carpeta

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

Descargue el SDK de Voz en forma de [paquete .zip](https://aka.ms/csspeech/jsbrowserpackage) y desempaquételo en la carpeta recién creada. Como resultado se desempaquetan dos archivos, `microsoft.cognitiveservices.speech.sdk.bundle.js` y `microsoft.cognitiveservices.speech.sdk.bundle.js.map`.
El último archivo es opcional y es útil para la depuración en el código del SDK.

## <a name="create-an-indexhtml-page"></a>Creación de una página index.html

Cree un nuevo archivo llamado `index.html` en la carpeta y ábralo con un editor de texto.

1. Cree el siguiente esqueleto HTML:

   ```html
   <html>
   <head>
      <title>Speech SDK JavaScript Quickstart</title>
   </head>
   <body>
    <!-- UI code goes here -->

    <!-- SDK reference goes here -->

    <!-- Optional authorization token request goes here -->

    <!-- Sample code goes here -->
   </body>
   </html>
   ```

1. Agregue el siguiente código de la interfaz de usuario al archivo, debajo del primer comentario:

   [!code-html[](~/samples-cognitive-services-speech-sdk/quickstart/js-browser/index.html#uidiv)]

1. Agregue una referencia al SDK de Voz

   [!code-html[](~/samples-cognitive-services-speech-sdk/quickstart/js-browser/index.html#speechsdkref)]

1. Conecte los controladores del botón de reconocimiento, el resultado del reconocimiento y los campos relacionados con la suscripción definidos por el código de la interfaz de usuario:

   [!code-html[](~/samples-cognitive-services-speech-sdk/quickstart/js-browser/index.html#quickstartcode)]

## <a name="create-the-token-source-optional"></a>Creación del origen del token (opcional)

En caso de que desea hospedar la página web en un servidor web, opcionalmente puede proporcionar un origen del token para la aplicación de demostración.
De esa forma, la clave de la suscripción nunca saldrá el servidor, lo que permitirá a los usuarios utilizar las funcionalidades de voz sin escribir ningún código de autorización.

1. Cree un nuevo archivo llamado `token.php`. En este ejemplo se supone que el servidor web admite el lenguaje de scripting PHP. Escriba el siguiente código:

   [!code-php[](~/samples-cognitive-services-speech-sdk/quickstart/js-browser/token.php)]

1. Edite el archivo `index.html` y agregue el siguiente código al archivo:

   [!code-html[](~/samples-cognitive-services-speech-sdk/quickstart/js-browser/index.html#authorizationfunction)]

> [!NOTE]
> Los tokens de autorización tienen una vigencia limitada.
> Este ejemplo simplificado no muestra cómo actualizar automáticamente tokens de autorización. Como usuario, puede volver a cargar la página manualmente o presione la tecla F5 para actualizarla.

## <a name="build-and-run-the-sample-locally"></a>Compilación y ejecución local del ejemplo

Para iniciar la aplicación, haga doble clic en el archivo index.html o ábralo con el explorador web que prefiera. Presentará una interfaz gráfica de usuario simple que le permitirá escribir su clave de suscripción y [región](regions.md), y desencadenará un reconocimiento mediante el micrófono.

> [!NOTE]
> Este método no funciona en el explorador Safari.
> En Safari, la página web de ejemplo debe estar hospedada en un servidor web; Safari no permite la carga de sitios web desde un archivo local para usar el micrófono.

## <a name="build-and-run-the-sample-via-a-web-server"></a>Compilación y ejecución del ejemplo mediante un servidor web

Para iniciar la aplicación, abra el explorador web que prefiera y apunte a la dirección URL pública en la que hospeda la carpeta, escriba su [región](regions.md)y desencadene un reconocimiento mediante el micrófono. Si lo ha configurado, adquirirá un token del origen del token.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Exploración de ejemplos de JavaScript en GitHub](https://aka.ms/csspeech/samples)
