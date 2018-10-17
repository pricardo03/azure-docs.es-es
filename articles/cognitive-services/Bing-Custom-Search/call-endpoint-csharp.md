---
title: 'Guía de inicio rápido: Llamada al punto de conexión mediante Node.js en Bing Custom Search'
titlesuffix: Azure Cognitive Services
description: Esta guía de inicio rápido muestra cómo solicitar los resultados de la búsqueda a la instancia de búsqueda personalizada usando Node.js para llamar al punto de conexión de Bing Custom Search.
services: cognitive-services
author: brapel
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: quickstart
ms.date: 05/07/2018
ms.author: v-brapel
ms.openlocfilehash: af77b4c06b61cda4fd18d19ac3578129004c4914
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2018
ms.locfileid: "48816708"
---
# <a name="quickstart-call-bing-custom-search-endpoint-nodejs"></a>Guía de inicio rápido: Llamada a un punto de conexión de Bing Custom Search (Node.js)

Esta guía de inicio rápido muestra cómo solicitar los resultados de la búsqueda a la instancia de búsqueda personalizada usando Node.js para llamar al punto de conexión de Bing Custom Search. 

## <a name="prerequisites"></a>Requisitos previos

Para completar este inicio rápido necesita instalar:

- Una instancia de Custom Search lista para usar. Consulte [Create your first Bing Custom Search instance](quick-start.md) (Creación de la primera instancia de Bing Custom Search).
- [Node.js](https://www.nodejs.org/) instalado.
- Una clave de suscripción. Puede obtener una clave de suscripción cuando active su [evaluación gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search) o puede utilizar una clave de suscripción de pago desde el panel de Azure (consulte [Cuenta de la API de Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account)).    

## <a name="run-the-code"></a>Ejecución del código

Para ejecutar el ejemplo, siga estos pasos:

1. Cree una carpeta para el código.  
  
2. Desde un terminal o símbolo del sistema, vaya a la carpeta que acaba de crear.  
  
3. Instale el módulo de Node **request**:
    <pre>
    npm install request
    </pre>  
    
4. Cree un archivo denominado BingCustomSearch.js en la carpeta que creó y copie el código siguiente en él. Reemplace los valores de **YOUR-SUBSCRIPTION-KEY** y **YOUR-CUSTOM-CONFIG-ID** por su clave de suscripción y su identificador de configuración.  
  
    ``` javascript
    var request = require("request");
    
    var subscriptionKey = 'YOUR-SUBSCRIPTION-KEY';
    var customConfigId = 'YOUR-CUSTOM-CONFIG-ID';
    var searchTerm = 'microsoft';
    
    var options = {
        url: 'https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/search?' + 
          'q=' + searchTerm + 
          '&customconfig=' + customConfigId,
        headers: {
            'Ocp-Apim-Subscription-Key' : subscriptionKey
        }
    }
    
    request(options, function(error, response, body){
        var searchResponse = JSON.parse(body);
        for(var i = 0; i < searchResponse.webPages.value.length; ++i){
            var webPage = searchResponse.webPages.value[i];
            console.log('name: ' + webPage.name);
            console.log('url: ' + webPage.url);
            console.log('displayUrl: ' + webPage.displayUrl);
            console.log('snippet: ' + webPage.snippet);
            console.log('dateLastCrawled: ' + webPage.dateLastCrawled);
            console.log();
        }
    })
    ```  
  
6. Ejecute el código con el comando siguiente:  
  
    ```    
    node BingCustomSearch.js
    ``` 

## <a name="next-steps"></a>Pasos siguientes
- [Configuración de la experiencia de interfaz de usuario hospedada](./hosted-ui.md)
- [Uso de marcadores de decoración para resaltar texto](./hit-highlighting.md)
- [Paginación de páginas web](./page-webpages.md)
