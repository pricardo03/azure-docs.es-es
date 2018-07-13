---
title: Guía de inicio rápido del SDK de Image Search para Node | Microsoft Docs
description: Configuración de la aplicación de consola del SDK de Image Search.
titleSuffix: Azure cognitive services
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 02/12/2018
ms.author: v-gedod
ms.openlocfilehash: e4c8303e39accbb7caec15c0ef47d701971ce632
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35382594"
---
# <a name="image-search-sdk-node-quickstart"></a>Guía de inicio rápido del SDK de Image Search para Node

El SDK de Bing Image Search contiene la funcionalidad de la API REST para consultas de imágenes y análisis de resultados. 

El [código fuente de los ejemplos del SDK de Bing Image Search para Node](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/imageSearch.js) está disponible en Git Hub.

## <a name="application-dependencies"></a>Dependencias de aplicaciones

Para configurar una aplicación de consola mediante el SDK de Bing Image Search, ejecute `npm install azure-cognitiveservices-imagesearch` en el entorno de desarrollo.

## <a name="image-search-client"></a>Cliente de Image Search
Obtenga una [clave de acceso de Cognitive Services](https://azure.microsoft.com/try/cognitive-services/) en *Buscar*. Creación de una instancia de `CognitiveServicesCredentials`:
```
const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
```
Después, cree una instancia del cliente:
```
const ImageSearchAPIClient = require('azure-cognitiveservices-imagesearch');
let client = new ImageSearchAPIClient(credentials);
```
Utilice el cliente para buscar con un texto de consulta, en este caso "El Capitán":
```
client.imagesOperations.search('El Capitan', function (err, result, request, response) {
    if (err) throw err;
    console.log(result.value);
});

```
<!-- Need to sanitize result
The code prints `result.value` items to the console without parsing any text. The results will be:
- _type: 'ImageObjectElementType'

![Imageresults](media/node-sdk-quickstart-image-results.png)
-->

## <a name="next-steps"></a>Pasos siguientes

[Ejemplos del SDK de Cognitive services para Node.js](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples)