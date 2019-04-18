---
title: 'Inicio rápido: Lista de texto a voz voces, Node.js - servicios de voz'
titleSuffix: Azure Cognitive Services
description: En este tutorial, obtendrá información sobre cómo obtener la lista completa de las voces neurales y estándares para una región o punto de conexión mediante Node.js. Se devuelve la lista como JSON y disponibilidad de voz varía según la región.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 04/02/2019
ms.author: erhopf
ms.openlocfilehash: 62187ddbe587a81038f8424b079e3c0c313d1ae2
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "58887106"
---
# <a name="quickstart-get-the-list-of-text-to-speech-voices-using-nodejs"></a>Inicio rápido: Obtiene la lista de texto a voz voces mediante Node.js

En este tutorial, obtendrá información sobre cómo obtener la lista completa de las voces neurales y estándares para una región o punto de conexión mediante Node.js. Se devuelve la lista como JSON y disponibilidad de voz varía según la región. Para obtener una lista de regiones admitidas, consulte [regiones](regions.md).

Esta guía, se requiere un [cuenta de Azure Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) con un recurso de servicios de voz. Si no tiene una cuenta, puede usar la [evaluación gratuita](get-started.md) para obtener una clave de suscripción.

## <a name="prerequisites"></a>Requisitos previos

Esta guía de inicio rápido requiere:

* [Node 8.12.x o posterior](https://nodejs.org/en/)
* [Visual Studio](https://visualstudio.microsoft.com/downloads/), [Visual Studio Code](https://code.visualstudio.com/download) o su editor favorito de código
* Una clave de suscripción de Azure para los servicios de voz. [Obtenga una gratis](get-started.md).

## <a name="create-a-project-and-require-dependencies"></a>Creación de un proyecto y requerimiento de dependencias

Cree un proyecto de Node.js mediante su IDE o editor favorito. A continuación, copie este fragmento de código en un archivo llamado `get-voices.js`.

```javascript
// Requires request and request-promise for HTTP requests
// e.g. npm install request request-promise
const rp = require('request-promise');
// Requires fs to write the list of languagesto a file
const fs = require('fs');
```

> [!NOTE]
> Si no ha usado estos módulos deberá instalarlos antes de ejecutar el programa. Para instalar estos paquetes, ejecute: `npm install request request-promise`.

## <a name="get-an-access-token"></a>Obtención de un token de acceso

La API REST Texto a voz requiere un token de acceso para la autenticación. Para obtener un token de acceso, es necesario un intercambio. Esta función intercambia su clave de suscripción de servicios de voz para obtener un token de acceso usa el `issueToken` punto de conexión.

En este ejemplo se da por supuesto que la suscripción de servicios de voz está en la región Oeste de Estados Unidos. Si usa una región diferente, actualice el valor de `uri`. Para obtener una lista completa, consulte [Regiones](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#rest-apis).

Copie este código en el proyecto:

```javascript
// Gets an access token.
function getAccessToken(subscriptionKey) {
    let options = {
        method: 'POST',
        uri: 'https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken',
        headers: {
            'Ocp-Apim-Subscription-Key': subscriptionKey
        }
    }
    return rp(options);
}
```

> [!NOTE]
> Para obtener más información, vea [Autenticación con un token de autenticación](https://docs.microsoft.com/azure/cognitive-services/authentication#authenticate-with-an-authentication-token).

En la siguiente sección, vamos a crear la función para obtener la lista de las voces y guardar la salida JSON al archivo.

## <a name="make-a-request-and-save-the-response"></a>Realización de solicitud y guardado de la respuesta

Aquí va a generar la solicitud y guardar la lista de voces devueltas. En este ejemplo se da por hecho que está utilizando el punto de conexión de Oeste de EE. UU. Si el recurso está registrado en una región diferente, asegúrese de actualizar `uri`. Para obtener más información, consulte [regiones de servicios de voz](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#text-to-speech).

A continuación, agregue los encabezados necesarios para la solicitud. Por último, deberá realizar una solicitud al servicio. Si la solicitud es correcta y se devuelve un código de 200 estado, la respuesta se escribe en el archivo.

```javascript
function textToSpeech(accessToken) {
    let options = {
        method: 'GET',
        baseUrl: 'https://westus.tts.speech.microsoft.com/',
        url: 'cognitiveservices/voices/list',
        headers: {
            'Authorization': 'Bearer ' + accessToken,
            'Content-Type': 'application/json'
        }
    }

    let request = rp(options)
        .on('response', (response) => {
            if (response.statusCode === 200) {
                request.pipe(fs.createWriteStream('voices.json'));
                console.log('\nYour file is ready.\n')
            }
        });
    return request;
}
```

## <a name="put-it-all-together"></a>Colocación de todo junto

Ya casi ha terminado. El último paso es crear una función asincrónica. Esta función se leen su clave de suscripción a una variable de entorno, obtener un token, espere a que finalice la solicitud y luego escribir la respuesta JSON al archivo.

Si está familiarizado con las variables de entorno o si prefiere probar con su suscripción clave codificado de forma rígida como una cadena, reemplace `process.env.SPEECH_SERVICE_KEY` con su clave de suscripción como una cadena.

```javascript
// Use async and await to get the token before attempting
// to convert text to speech.
async function main() {
    // Reads subscription key from env variable.
    // You can replace this with a string containing your subscription key. If
    // you prefer not to read from an env variable.
    // e.g. const subscriptionKey = "your_key_here";
    const subscriptionKey = process.env.SPEECH_SERVICE_KEY;
    if (!subscriptionKey) {
        throw new Error('Environment variable for your subscription key is not set.')
    };
    try {
        const accessToken = await getAccessToken(subscriptionKey);
        await textToSpeech(accessToken);
    } catch (err) {
        console.log(`Something went wrong: ${err}`);
    }
}

main()
```

## <a name="run-the-sample-app"></a>Ejecutar la aplicación de ejemplo

Eso es todo, ya está listo para ejecutar la aplicación de ejemplo. Desde la línea de comandos (o sesión de terminal), vaya al directorio del proyecto y ejecute:

```console
node get-voices.js
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Asegúrese de quitar cualquier información confidencial del código fuente de la aplicación de ejemplo como, por ejemplo, las claves de suscripción.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Explorar ejemplos de Node.js en GitHub](https://github.com/Azure-Samples/Cognitive-Speech-TTS/tree/master/Samples-Http/NodeJS)

## <a name="see-also"></a>Vea también

* [Referencia de Text-to-speech API](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis)
* [Crear fuentes de voz personalizada](how-to-customize-voice-font.md)
* [Grabación de muestras de voz para crear una voz personalizada](record-custom-voice-samples.md)
