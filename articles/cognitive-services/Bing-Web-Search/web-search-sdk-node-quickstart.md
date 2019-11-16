---
title: 'Inicio rápido: Uso del SDK de Bing Web Search para Node.js'
titleSuffix: Azure Cognitive Services
description: El SDK de Bing Web Search facilita la integración de Bing Web Search en la aplicación de Node.js. En esta guía de inicio rápido, aprenderá a crear una instancia de un cliente, enviar una solicitud e imprimir la respuesta.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: quickstart
ms.date: 03/12/2019
ms.author: aahi
ms.openlocfilehash: 79f0630c65aa438ac5d831b03842cc74b609cd3b
ms.sourcegitcommit: 2d3740e2670ff193f3e031c1e22dcd9e072d3ad9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/16/2019
ms.locfileid: "74132742"
---
# <a name="quickstart-use-the-bing-web-search-sdk-for-nodejs"></a>Inicio rápido: Uso del SDK de Bing Web Search para Node.js

El SDK de Bing Web Search facilita la integración de Bing Web Search en la aplicación de Node.js. En esta guía de inicio rápido, aprenderá a crear una instancia de un cliente, enviar una solicitud e imprimir la respuesta.

¿Desea ver el código ahora mismo? Hay [ejemplos del SDK de Bing Web Search para Node.js](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples) disponibles en GitHub.

[!INCLUDE [bing-web-search-quickstart-signup](../../../includes/bing-web-search-quickstart-signup.md)]

Consulte también [Precios de Cognitive Services - Bing Search API](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).

## <a name="prerequisites"></a>Requisitos previos
Estas son algunas cosas que necesitará antes de ejecutar esta guía de inicio rápido:

* [Node.js 6](https://nodejs.org/en/download/) o posterior
* Una clave de suscripción  

## <a name="set-up-your-development-environment"></a>Configurado su entorno de desarrollo

Empecemos por la configuración del entorno de desarrollo para nuestro proyecto de Node.js.

1. Cree un directorio para su proyecto:

    ```console
    mkdir YOUR_PROJECT
    ```

1. Cree un archivo de paquete:

    ```console
    cd YOUR_PROJECT
    npm init
    ```

1. Ahora, vamos a instalar algunos módulos de Azure y agregarlos a `package.json`:

    ```console
    npm install --save azure-cognitiveservices-websearch
    npm install --save ms-rest-azure
    ```

## <a name="create-a-project-and-declare-required-modules"></a>Creación de un proyecto y declaración de los módulos necesarios

En el mismo directorio que su `package.json`, cree un nuevo proyecto de Node.js con su editor o IDE favorito. Por ejemplo: `sample.js`.

A continuación, copie este código en el proyecto. Se cargan los módulos instalados en la sección anterior.

```javascript
const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
const WebSearchAPIClient = require('azure-cognitiveservices-websearch');
```

## <a name="instantiate-the-client"></a>Creación de una instancia del cliente

Este código crea una instancia de un cliente mediante el módulo `azure-cognitiveservices-websearch`. Asegúrese de escribir una clave de suscripción válida para la cuenta de Azure antes de continuar.

```javascript
let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
let webSearchApiClient = new WebSearchAPIClient(credentials);
```

## <a name="make-a-request-and-print-the-results"></a>Presentación de solicitud e impresión de resultados

Utilice el cliente para enviar una consulta de búsqueda a Bing Web Search. Si la respuesta incluye resultados para cualquiera de los elementos de la matriz `properties`, se imprime `result.value` en la consola.

```javascript
webSearchApiClient.web.search('seahawks').then((result) => {
    let properties = ["images", "webPages", "news", "videos"];
    for (let i = 0; i < properties.length; i++) {
        if (result[properties[i]]) {
            console.log(result[properties[i]].value);
        } else {
            console.log(`No ${properties[i]} data`);
        }
    }
}).catch((err) => {
    throw err;
})
```

## <a name="run-the-program"></a>Ejecución del programa

¡El último paso es ejecutar el programa!

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando haya terminado con este proyecto, asegúrese de quitar la clave de suscripción desde el código del programa.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Ejemplos del SDK de Cognitive Services para Node.js](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples)

## <a name="see-also"></a>Otras referencias

* [Referencia del SDK de Nodo de Azure](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-websearch/)
