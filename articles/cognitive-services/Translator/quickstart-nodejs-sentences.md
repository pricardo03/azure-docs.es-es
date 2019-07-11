---
title: 'Inicio rápido: Obtención de las longitudes de oración con Node.js: Translator Text API'
titleSuffix: Azure Cognitive Services
description: En esta guía de inicio rápido, obtendrá información sobre cómo determinar las longitudes de frase (en caracteres) con Node.js y Translator Text REST API.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: quickstart
ms.date: 06/04/2019
ms.author: swmachan
ms.openlocfilehash: 7a39e52e9145ac6cab298026b4098206e904943c
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2019
ms.locfileid: "67445001"
---
# <a name="quickstart-use-the-translator-text-api-to-determine-sentence-length-with-nodejs"></a>Inicio rápido: Uso de Translator Text API para determinar las longitudes de oración con Node.js

En esta guía de inicio rápido, obtendrá información sobre cómo determinar las longitudes de frase (en caracteres) con Node.js y Translator Text REST API.

En esta guía de inicio rápido, se requiere una [cuenta de Azure Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) con un recurso de Translator Text. Si no tiene una cuenta, puede usar la [evaluación gratuita](https://azure.microsoft.com/try/cognitive-services/) para obtener una clave de suscripción.

## <a name="prerequisites"></a>Requisitos previos

Esta guía de inicio rápido requiere:

* [Node 8.12.x o posterior](https://nodejs.org/en/)
* Una clave de suscripción de Azure para Translator Text

## <a name="create-a-project-and-import-required-modules"></a>Creación de un proyecto e importación de los módulos necesarios

Cree un proyecto con su IDE o editor favorito. A continuación, copie este fragmento de código en un archivo llamado `sentence-length.js`.

```javascript
const request = require('request');
const uuidv4 = require('uuid/v4');
```

> [!NOTE]
> Si no ha usado estos módulos deberá instalarlos antes de ejecutar el programa. Para instalar estos paquetes, ejecute: `npm install request uuidv4`.

Estos módulos son necesarios para construir la solicitud HTTP y crear un identificador único para el encabezado `'X-ClientTraceId'`.

## <a name="set-the-subscription-key"></a>Establecimiento de la clave de suscripción

En este código se intenta leer la clave de suscripción de Translator Text desde la variable de entorno `TRANSLATOR_TEXT_KEY`. Si no está familiarizado con las variables de entorno, puede establecer `subscriptionKey` como una cadena y convertir en comentario la instrucción condicional.

Copie este código en el proyecto:

```javascript
/* Checks to see if the subscription key is available
as an environment variable. If you are setting your subscription key as a
string, then comment these lines out.

If you want to set your subscription key as a string, replace the value for
the Ocp-Apim-Subscription-Key header as a string. */
const subscriptionKey = process.env.TRANSLATOR_TEXT_KEY;
if (!subscriptionKey) {
  throw new Error('Environment variable for your subscription key is not set.')
};
```

## <a name="configure-the-request"></a>Configuración de la solicitud

El método `request()`, disponible mediante el módulo de solicitud, nos permite pasar el método HTTP, la dirección URL, los parámetros de la solicitud, los encabezados y el cuerpo de JSON como un objeto `options`. En este fragmento de código, vamos a configurar la solicitud:

>[!NOTE]
> Para más información sobre los puntos de conexión, las rutas y los parámetros de la solicitud, consulte [Translator Text API 3.0: BreakSentence](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-break-sentence).

```javascript
let options = {
    method: 'POST',
    baseUrl: 'https://api.cognitive.microsofttranslator.com/',
    url: 'breaksentence',
    qs: {
      'api-version': '3.0',
    },
    headers: {
      'Ocp-Apim-Subscription-Key': subscriptionKey,
      'Content-type': 'application/json',
      'X-ClientTraceId': uuidv4().toString()
    },
    body: [{
          'text': 'How are you? I am fine. What did you do today?'
    }],
    json: true,
};
```

La manera más fácil de autenticar una solicitud es pasar la clave de suscripción como un encabezado `Ocp-Apim-Subscription-Key`, que es el que se usa en este ejemplo. O bien, puede intercambiar la clave de suscripción para un token de acceso y pasar este token como un encabezado `Authorization` para validar la solicitud. 

Si usa una suscripción a varios servicios de Cognitive Services, también debe incluir `Ocp-Apim-Subscription-Region` en los encabezados de la solicitud. 

Para más información, consulte [Autenticación](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication).

## <a name="make-the-request-and-print-the-response"></a>Realización de solicitud e impresión de la respuesta

A continuación, vamos a crear una solicitud mediante el método `request()`. Toma el objeto `options` que se creó en la sección anterior como el primer argumento y, a continuación, imprime la respuesta JSON embellecida.

```javascript
request(options, function(err, res, body){
    console.log(JSON.stringify(body, null, 4));
});
```

>[!NOTE]
> En este ejemplo, vamos a definir la solicitud HTTP en el objeto `options`. Sin embargo, el módulo de solicitud también admite métodos de conveniencia como `.post` y `.get`. Para más información, consulte [métodos de conveniencia](https://github.com/request/request#convenience-methods).

## <a name="put-it-all-together"></a>Colocación de todo junto

Eso es todo, ha creado un sencillo programa que llama a Translator Text API y devuelve una respuesta JSON. Ahora es el momento de ejecutar el programa:

```console
node sentence-length.js
```

Si desea comparar su código con el nuestro, el ejemplo completo está disponible en [GitHub](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-NodeJS).

## <a name="sample-response"></a>Respuesta de muestra

```json
[
    {
        "sentLen": [
            13,
            11,
            22
        ]
    }
]
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Si ha codificado de forma rígida la clave de suscripción en el programa, asegúrese de quitarla cuando haya terminado con esta guía de inicio rápido.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Explorar ejemplos de Node.js en GitHub](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-NodeJS)

## <a name="see-also"></a>Otras referencias

Además de para la detección del idioma, aprenda a usar Translator Text API para:

* [Traducir texto](quickstart-nodejs-translate.md)
* [Transliterar texto](quickstart-nodejs-transliterate.md)
* [Identificar el idioma de entrada](quickstart-nodejs-detect.md)
* [Obtener traducciones alternativas](quickstart-nodejs-dictionary.md)
* [Obtener una lista de idiomas admitidos](quickstart-nodejs-languages.md)
