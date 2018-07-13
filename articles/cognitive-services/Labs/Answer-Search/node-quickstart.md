---
title: Inicio rápido de Node para Project Answer Search de Microsoft Cognitive Services | Microsoft Docs
description: Empiece a usar Project Answer Search de Microsoft Cognitive Services en Azure.
services: cognitive-services
author: mikedodaro
ms.service: cognitive-services
ms.technology: project-answer-search
ms.topic: article
ms.date: 04/13/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: 36b2709d39230aae7929164ba4c9306f57043b43
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35381739"
---
# <a name="project-answer-search-node-quickstart"></a>Guía de inicio rápido de Project Answer Search para Node

En el siguiente ejemplo de Node se crea una consulta para obtener información sobre Yosemite National Park.

## <a name="prerequisites"></a>requisitos previos

Obtenga una clave de acceso para la evaluación gratuita de los [Laboratorios de Cognitive Services](https://aka.ms/answersearchsubscription).

Este ejemplo utiliza Node v8.9.4.

## <a name="code-scenario"></a>Escenario de código 

El código siguiente obtiene las respuestas.
Se implementa en los pasos siguientes:
1. Declare las variables para especificar el punto de conexión por host y ruta de acceso.
2. Especifique la dirección URL de la que desea obtener una vista previa y agregue el parámetro de consulta.  
3. Cree una función de controlador para la respuesta.
4. Defina la función de búsqueda que crea la solicitud y agrega el encabezado *Ocp-Apim-Subscription-Key*.
5. Ejecute la función de búsqueda. 

El código completo de esta demostración es el siguiente:

````
'use strict';

let https = require('https');

// Replace the subscriptionKey string value with your valid subscription key.
let subscriptionKey = 'YOUR-SUBSCRIPTION-KEY'; 

let host = 'api.labs.cognitive.microsoft.com';
let path = '/answerSearch/v7.0/search';

let mkt = 'en-us';
let q = 'Yosemite National Park';

let params = '?q=' + encodeURI(q) + '&mkt=en-us';

let response_handler = function (response) {
    let body = '';
    response.on('data', function (d) {
        body += d;
    });
    response.on('end', function () {
        let body_ = JSON.parse(body);
        let body__ = JSON.stringify(body_, null, '  ');
        console.log(body__);
    });
    response.on('error', function (e) {
        console.log('Error: ' + e.message);
    });
};

let Search = function () {
    let request_params = {
        method: 'GET',
        hostname: host,
        path: path + params,
        headers: {
            'Ocp-Apim-Subscription-Key': subscriptionKey,
        }
    };

    let req = https.request(request_params, response_handler);
    req.end();
}

Search();

````

## <a name="next-steps"></a>Pasos siguientes
- [Código de ejemplo de C#](c-sharp-quickstart.md)
- [Inicio rápido de Java](java-quickstart.md)
- [Inicio rápido de Python](python-quickstart.md)