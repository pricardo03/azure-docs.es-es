---
title: Aplicación web de una sola página de Bing Image Search | Microsoft Docs
titleSuffix: Bing Web Search APIs - Cognitive Services
description: Muestra cómo utilizar Bing Image Search API en una aplicación web de una sola página.
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 10/04/2017
ms.author: v-brapel
ms.openlocfilehash: 303d7745167d2ea25fda083ed99881ac4e0a7ec7
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35381850"
---
# <a name="tutorial-visual-search-single-page-web-app"></a>Tutorial: Aplicación web Visual Search de página única

Bing Visual Search API proporciona una experiencia similar a los detalles de imagen que se muestran en Bing.com/images. Visual Search permite especificar una imagen y obtener conclusiones sobre la imagen, como otras imágenes visualmente similares, orígenes de compras, páginas web que incluyen la imagen, etc. 

Este tutorial amplía la aplicación web de página única del tutorial de Bing Image Search (consulte [Aplicación web de una sola página](../Bing-Image-Search/tutorial-bing-image-search-single-page-app.md)). Para obtener el código fuente completo para iniciar este tutorial, vea [Aplicación web de una sola página (código fuente)](../Bing-Image-Search/tutorial-bing-image-search-single-page-app-source.md). Para obtener código fuente final de este tutorial, consulte [Aplicación web Visual Search de página única](tutorial-bing-visual-search-single-page-app-source.md).

Las tareas que se tratan son:

> [!div class="checklist"]
> * Llamar a Bing Visual Search API con un token de conclusiones de imagen
> * Mostrar imágenes similares

## <a name="call-bing-visual-search"></a>Llamar a Bing Visual Search
Edite el tutorial de Bing Image Search y agregue el código siguiente al final del elemento de script en la línea 409. Este código llama a Bing Visual Search API y muestra los resultados.

``` javascript
function handleVisualSearchResponse(){
    if(this.status !== 200){
        console.log(this.responseText);
        return;
    }
    let json = this.responseText;
    let response = JSON.parse(json);
    for (let i =0; i < response.tags.length; i++) {
        let tag = response.tags[i];
        if (tag.displayName === '') {
            for (let j = 0; j < tag.actions.length; j++) {
                let action = tag.actions[j];
                if (action.actionType === 'VisualSearch') {
                    let html = '';
                    for (let k = 0; k < action.data.value.length; k++) {
                        let item = action.data.value[k];
                        let height = 120;
                        let width = Math.max(Math.round(height * item.thumbnail.width / item.thumbnail.height), 120);
                        html += "<img src='"+ item.thumbnailUrl + "&h=" + height + "&w=" + width + "' height=" + height + " width=" + width + "'>";
                    }
                    showDiv("insights", html);
                    window.scrollTo({top: document.getElementById("insights").getBoundingClientRect().top, behavior: "smooth"});
                }
            }
        }
    }
}

function bingVisualSearch(insightsToken){
    let visualSearchBaseURL = 'https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch',
        boundary = 'boundary_ABC123DEF456',
        startBoundary = '--' + boundary,
        endBoundary = '--' + boundary + '--',
        newLine = "\r\n",
        bodyHeader = 'Content-Disposition: form-data; name="knowledgeRequest"' + newLine + newLine;

    let postBody = {
        imageInfo: {
            imageInsightsToken: insightsToken
        }
    }
    let requestBody = startBoundary + newLine;
    requestBody += bodyHeader;
    requestBody += JSON.stringify(postBody) + newLine + newLine;
    requestBody += endBoundary + newLine;       
    
    let request = new XMLHttpRequest();

    try {
        request.open("POST", visualSearchBaseURL);
    } 
    catch (e) {
        renderErrorMessage("Error performing visual search: " + e.message);
    }
    request.setRequestHeader("Ocp-Apim-Subscription-Key", getSubscriptionKey());
    request.setRequestHeader("Content-Type", "multipart/form-data; boundary=" + boundary);
    request.addEventListener("load", handleVisualSearchResponse);
    request.send(requestBody);
}
```

## <a name="capture-insights-token"></a>Capturar el token de conclusiones
Agregue el código siguiente en el objeto `searchItemsRenderer` en la línea 151. Este código agrega un vínculo **buscar similares** que llama a la función `bingVisualSearch` al hacer clic en él. La función recibe imageInsightsToken como un argumento.

``` javascript
html.push("<a href='javascript:bingVisualSearch(\"" + item.imageInsightsToken + "\");'>find similar</a><br>");
```

## <a name="display-similar-images"></a>Mostrar imágenes similares
Agregue el código HTML siguiente en la línea 601. Este código de marcado agrega un elemento que se usa para mostrar los resultados de la llamada a Bing Visual Search API.

``` html
<div id="insights">
    <h3><a href="#" onclick="return toggleDisplay('_insights')">Similar</a></h3>
    <div id="_insights" style="display: none"></div>
</div>
```

Con el nuevo código JavaScript y los elementos HTML implementados, los resultados de la búsqueda se muestran con un vínculo **buscar similares**. Haga clic en el vínculo para rellenar la sección **Similares** con imágenes similares a la que ha seleccionado. Puede que tenga que expandir la sección **Similares** para mostrar las imágenes.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Origen de la aplicación web Visual Search de página única](tutorial-bing-visual-search-single-page-app-source.md)
> [Referencia de Bing Visual Search API](https://aka.ms/bingvisualsearchreferencedoc)