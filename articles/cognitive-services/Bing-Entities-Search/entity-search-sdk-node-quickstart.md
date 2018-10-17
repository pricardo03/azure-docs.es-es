---
title: 'Guía de inicio rápido: SDK de Bing Entity Search con Node'
titleSuffix: Azure Cognitive Services
description: Configuración de la aplicación de consola del SDK de Entity Search con Node.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-entity-search
ms.topic: quickstart
ms.date: 02/12/2018
ms.author: v-gedod
ms.openlocfilehash: 1f2a5f6a1473cde40928ada6e30f6bd9b780543d
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2018
ms.locfileid: "48814889"
---
# <a name="quickstart-bing-entity-search-sdk-with-node"></a>Guía de inicio rápido: SDK de Bing Entity Search con Node

El SDK de Bing Entity Search contiene la funcionalidad de la API REST para consultas de entidad y análisis de resultados. 

El [código fuente de los ejemplos del SDK de Bing Entity Search para C#](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/entitySearch.js) está disponible en Git Hub.
## <a name="application-dependencies"></a>Dependencias de aplicaciones

Para configurar una aplicación de consola mediante el SDK de Bing Entity Search, ejecute `npm install azure-cognitiveservices-entitysearch` en el entorno de desarrollo.

## <a name="entity-search-client"></a>Cliente de Entity Search
Obtenga una [clave de acceso de Cognitive Services](https://azure.microsoft.com/try/cognitive-services/) en *Buscar*. Creación de una instancia de `CognitiveServicesCredentials`:
```
const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
```
A continuación, cree una instancia del cliente y busque los resultados:
```
const EntitySearchAPIClient = require('azure-cognitiveservices-entitysearch');

let entitySearchApiClient = new EntitySearchAPIClient(credentials);

entitySearchApiClient.entitiesOperations.search('seahawks').then((result) => {
    console.log(result.queryContext);
    console.log(result.entities.value);
    console.log(result.entities.value[0].description);
}).catch((err) => {
    throw err;
});

```
El código imprime `result.value` elementos en la consola sin analizar ningún texto.  Los resultados, si hubiera por categoría, incluyen:
- _type: 'Thing'
- _type: "ImageObject"

<!-- Removing until we can replace with a sanitized version.
![Entity results](media/entity-search-sdk-node-quickstart-results.png)
-->

## <a name="next-steps"></a>Pasos siguientes

[Ejemplos del SDK de Cognitive Services para Node.js](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples)