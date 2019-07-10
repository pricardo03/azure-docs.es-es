---
title: 'Tutorial: Inicio del Lector inmersivo (Node.js)'
titleSuffix: Azure Cognitive Services
description: En este tutorial, creará una aplicación de Node.js que inicia el Lector inmersivo.
services: cognitive-services
author: metanMSFT
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: tutorial
ms.date: 06/20/2019
ms.author: metan
ms.openlocfilehash: ac90496c950d8a563bf8794b4c1bb105b6c12232
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2019
ms.locfileid: "67444061"
---
# <a name="tutorial-launch-the-immersive-reader-nodejs"></a>Tutorial: Inicio del Lector inmersivo (Node.js)

En la [introducción](./overview.md), se indica lo que es el Lector inmersivo y cómo implementa técnicas demostradas para mejorar la comprensión lectora de nuevos lectores, alumnos de idiomas y alumnos con diferencias de aprendizaje. En este tutorial, se explica cómo crear una aplicación web de Node.js que inicia el Lector inmersivo. En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear una aplicación web de Node.js con Express
> * Adquisición de un token de acceso
> * Iniciar el Lector inmersivo con contenido de muestra
> * Especificar el idioma del contenido
> * Especificar el idioma de la interfaz del Lector inmersivo
> * Iniciar el Lector inmersivo con contenido de matemáticas

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

* Una clave de suscripción para el Lector inmersivo. Obtenga una; para ello, siga [estas instrucciones](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account).
* [Node.js](https://nodejs.org/) y [Yarn](https://yarnpkg.com)
* Un IDE como [Visual Studio Code](https://code.visualstudio.com/)

## <a name="create-a-nodejs-web-app-with-express"></a>Crear una aplicación web de Node.js con Express

Cree una aplicación web de Node.js con la herramienta `express-generator`.

```bash
npm install express-generator -g
express --view=pug myapp
cd myapp
```

Instale las dependencias de yarn y agregue las dependencias `request` y `dotenv`, que se utilizarán más adelante en el tutorial.

```bash
yarn
yarn add request
yarn add dotenv
```

## <a name="acquire-an-access-token"></a>Adquisición de un token de acceso

A continuación, escriba una API de back-end para recuperar un token de acceso con su clave de suscripción. Necesitará la clave de suscripción y el punto de conexión para este siguiente paso. Puede encontrar la clave de suscripción en la página Claves del recurso del Lector inmersivo en Azure Portal. Puede encontrar el punto de conexión en la página Introducción.

Una vez que tenga la clave de suscripción y el punto de conexión, cree un nuevo archivo denominado _.env_ y pegue el código siguiente, reemplazando `{YOUR_SUBSCRIPTION_KEY}` y `{YOUR_ENDPOINT}` por su clave de suscripción y el punto de conexión, respectivamente.

```text
SUBSCRIPTION_KEY={YOUR_SUBSCRIPTION_KEY}
ENDPOINT={YOUR_ENDPOINT}
```

Asegúrese de no confirmar este archivo en el control de código fuente, ya que contiene secretos que no deben hacerse públicos.

Después, abra el archivo _app.js_ y agregue lo siente en la parte superior. De este modo se carga la clave de suscripción y el punto de conexión como variables de entorno en el nodo.

```javascript
require('dotenv').config();
```

Abra el archivo _routes\index.js_ y la siguiente importación en la parte superior del archivo:

```javascript
var request = require('request');
```

Después, agregue el código siguiente directamente después de esa línea. Este código crea un punto de conexión de API que adquiere un token de acceso con su clave de suscripción y, a continuación, devuelve ese token.

```javascript
router.get('/token', function(req, res, next) {
  request.post({
    headers: {
        'Ocp-Apim-Subscription-Key': process.env.SUBSCRIPTION_KEY,
        'content-type': 'application/x-www-form-urlencoded'
    },
    url: process.env.ENDPOINT
  },
  function(err, resp, token) {
    return res.send(token);
  });
});
```

Este punto de conexión de API debe protegerse detrás de algún tipo de autenticación (por ejemplo, [OAuth](https://oauth.net/2/)); este procedimiento está fuera del ámbito de este tutorial.

## <a name="launch-the-immersive-reader-with-sample-content"></a>Iniciar el Lector inmersivo con contenido de muestra

1. Abra _views\layout.pug_ y agregue el siguiente código bajo la etiqueta `head`, antes de la etiqueta `body`. Estas etiquetas `script` cargan el [DSK de Lector inmersivo](https://github.com/Microsoft/immersive-reader-sdk) y jQuery.

    ```pug
    script(src='https://contentstorage.onenote.office.net/onenoteltir/immersivereadersdk/immersive-reader-sdk.0.0.1.js')
    script(src='https://code.jquery.com/jquery-3.3.1.min.js')
    ```

2. Abra el archivo _views\index.pug_ y reemplace el contenido por el código siguiente. Este código rellena la página con algún contenido de muestra y agrega un botón que inicia el Lector inmersivo.

    ```pug
    extends layout

    block content
      h2(id='title') Geography
      p(id='content') The study of Earth's landforms is called physical geography. Landforms can be mountains and valleys. They can also be glaciers, lakes or rivers.
      div(class='immersive-reader-button' data-button-style='iconAndText' data-locale='en-US' onclick='launchImmersiveReader()')
      script.
        function launchImmersiveReader() {
          // First, get a token using our /token endpoint
          $.ajax('/token', { success: token => {
            // Second, grab the content from the page
            const content = {
              title: document.getElementById('title').innerText,
              chunks: [ {
                content: document.getElementById('content').innerText + '\n\n',
                lang: 'en'
              } ]
            };

            // Third, launch the Immersive Reader
            ImmersiveReader.launchAsync(token, content);
          }});
        }
    ```

3. Nuestra aplicación web ya está lista. Inicie la aplicación; para ello, ejecute:

    ```bash
    npm start
    ```

4. Abra el explorador web y vaya a _http://localhost:3000_ . Debería ver el contenido anterior en la página. Haga clic en el botón **Lector inmersivo** para iniciar el Lector inmersivo con su contenido.

## <a name="specify-the-language-of-your-content"></a>Especificar el idioma del contenido

El Lector inmersivo es compatible con numerosos lenguajes diferentes. Para especificar el idioma del contenido, puede seguir estos pasos.

1. Abra _views\index.pug_ y agregue el código siguiente bajo la etiqueta `p(id=content)` que agregó en el paso anterior. Este código agrega contenido en español a la página.

    ```pug
    p(id='content-spanish') El estudio de las formas terrestres de la Tierra se llama geografía física. Los accidentes geográficos pueden ser montañas y valles. También pueden ser glaciares, lagos o ríos.
    ```

2. En el código JavaScript, agregue lo siguiente encima de la llamada a `ImmersiveReader.launchAsync`. Este código pasa el contenido en español al Lector inmersivo.

    ```pug
    content.chunks.push({
      content: document.getElementById('content-spanish').innerText + '\n\n',
      lang: 'es'
    });
    ```

3. Vaya de nuevo a _http://localhost:3000_ . Debería ver el texto en español en la página y, al hacer clic en **Lector inmersivo**, se mostrará en el Lector inmersivo también.

## <a name="specify-the-language-of-the-immersive-reader-interface"></a>Especificar el idioma de la interfaz del Lector inmersivo

De forma predeterminada, el idioma de la interfaz del Lector inmersivo coincide con la configuración de idioma del explorador. También puede especificar el idioma de la interfaz del Lector inmersivo con el código siguiente.

1. En _views\index.pug_, reemplace la llamada a `ImmersiveReader.launchAsync(token, content)` por el código siguiente.

    ```javascript
    const options = {
        uiLang: 'fr',
    }
    ImmersiveReader.launchAsync(token, content, options);
    ```

2. Vaya a _http://localhost:3000_ . Al iniciar el Lector inmersivo, se mostrará la interfaz en francés.

## <a name="launch-the-immersive-reader-with-math-content"></a>Iniciar el Lector inmersivo con contenido de matemáticas

Puede incluir contenido de matemáticas en el Lector inmersivo mediante [MathML](https://developer.mozilla.org/en-US/docs/Web/MathML).

1. Modifique _views\index.pug_ para incluir el código siguiente encima de la llamada a `ImmersiveReader.launchAsync`:

    ```javascript
    const mathML = '<math xmlns="https://www.w3.org/1998/Math/MathML" display="block"> \
      <munderover> \
        <mo>∫</mo> \
        <mn>0</mn> \
        <mn>1</mn> \
      </munderover> \
      <mrow> \
        <msup> \
          <mi>x</mi> \
          <mn>2</mn> \
        </msup> \
        <mo>ⅆ</mo> \
        <mi>x</mi> \
      </mrow> \
    </math>';

    content.chunks.push({
      content: mathML,
      mimeType: 'application/mathml+xml'
    });
    ```

2. Vaya a _http://localhost:3000_ . Al iniciar el Lector inmersivo y desplazarse hasta la parte inferior, verá la fórmula matemática.

## <a name="next-steps"></a>Pasos siguientes

* Explorar el [SDK del Lector inmersivo](https://github.com/Microsoft/immersive-reader-sdk) y agregar la [Referencia del SDK del Lector inmersivo](./reference.md)
* Ver muestras de código en [GitHub](https://github.com/microsoft/immersive-reader-sdk/samples/advanced-csharp)