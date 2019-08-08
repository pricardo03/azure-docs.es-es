---
title: 'Inicio rápido: Project Answer Search, Node'
description: Introducción al uso de Project Answer Search con Node.
services: cognitive-services
author: mikedodaro
manager: nitinme
ms.service: cognitive-services
ms.subservice: answer-search
ms.topic: quickstart
ms.date: 04/13/2018
ms.author: rosh
ROBOTS: NOINDEX
ms.openlocfilehash: ace83b9bdc8003e2e38d5b0a188582bc0cc2147c
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/01/2019
ms.locfileid: "68707093"
---
# <a name="quickstart-project-answer-search-with-node"></a>Inicio rápido: Project Answer Search con Node

En el siguiente ejemplo de Node se crea una consulta para obtener información sobre Yosemite National Park.

## <a name="prerequisites"></a>Requisitos previos

Obtenga una clave de acceso para la evaluación gratuita de los [Laboratorios de Cognitive Services](https://labs.cognitive.microsoft.com/en-us/project-answer-search).

Este ejemplo utiliza Node v8.9.4.

## <a name="code-scenario"></a>Escenario de código 

El código siguiente obtiene las respuestas.
Se implementa en los pasos siguientes:
1. Declare las variables para especificar el punto de conexión por host y ruta de acceso.
2. Especifique la dirección URL de la que desea obtener una vista previa y agregue el parámetro de consulta.  
3. Cree una función de controlador para la respuesta.
4. Defina la función de búsqueda que crea la solicitud y agrega el encabezado *Ocp-Apim-Subscription-Key*.
5. Ejecute la función Search. 

Este es el código completo de esta demostración:

```
'use strict';

let https = require('https');

// Replace the subscriptionKey string value with your valid subscription key.
let subscriptionKey = 'YOUR-SUBSCRIPTION-KEY'; 

let host = 'api.labs.cognitive.microsoft.com';
let path = '/answerSearch/v7.0/search';

let mkt = 'en-us';
let q = 'Yosemite National Park';

let params = '?q=' + encodeURI(q) + '&mkt= + mkt;

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

```

## <a name="next-steps"></a>Pasos siguientes
- [Código de ejemplo de C#](c-sharp-quickstart.md)
- [Inicio rápido de Java](java-quickstart.md)
- [Inicio rápido de Python](python-quickstart.md)
