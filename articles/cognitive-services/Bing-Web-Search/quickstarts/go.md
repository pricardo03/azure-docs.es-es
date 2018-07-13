---
title: Inicio rápido de Go para Azure Cognitive Services, Bing Web Search API | Microsoft Docs
description: Empiece a usar rápidamente el lenguaje Go para consultar Bing Web Search API en Microsoft Cognitive Services en Azure.
services: cognitive-services
author: Nhoya
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 03/09/2018
ms.author: rosh
ms.reviewer: nhoyadx@gmail.com, v-gedod
ms.openlocfilehash: 86cb67d46bca40c83c2f175ab7fdf6fbf52cb02f
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35380890"
---
# <a name="call-and-response-your-first-bing-web-search-query-in-go"></a>Llamada y respuesta: la primera consulta de Bing Web Search en Go

Bing Web Search API es similar a Bing.com, y devuelve los resultados de búsqueda pertinentes para la consulta del usuario. Los resultados pueden incluir páginas web, imágenes, vídeos, noticias y entidades, junto con las consultas de búsqueda relacionadas, correcciones ortográficas, zonas horarias, conversión de unidades, traducciones y cálculos. Los resultados se basan en su importancia y en el nivel al que esté suscrito en Bing Search APIs.

Consulte la [referencia de la API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference) para obtener detalles acerca de las API.

## <a name="prerequisites"></a>requisitos previos
Debe tener una [cuenta de Cognitive Services API](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) con **Bing Search APIs**. La [cuenta de evaluación gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) es suficiente para esta guía de inicio rápido. Necesita la clave de acceso que se le proporciona al activar la versión de evaluación gratuita, o puede usar una clave de suscripción de pago desde su panel de Azure.

Instale los [binarios de Go](https://golang.org/dl/).
 
En este tutorial se usan solo bibliotecas **principales**, por lo que no hay dependencias externas.

## <a name="core-libraries"></a>Bibliotecas principales

Use `http` para enviar la solicitud al punto de conexión, `ioutil` para leer la respuesta, `time` y `json` para administrar el json y `fmt` para imprimir la salida.

```
package main

import (
    "fmt"
    "net/http"
    "io/iutil"
    "time"
    "encoding/json"
)
```

## <a name="define-variables"></a>Definición de variables
Establezca el punto de conexión de API y el término de búsqueda.

```
//This is the valid endpoint at the time of the writing
const endpoint = "https://api.cognitive.microsoft.com/bing/v7.0/search"
//API token
token := "YOUR-ACCESS-KEY"
searchTerm := "Microsoft Cognitive Services"
```

## <a name="building-and-sending-the-request"></a>Creación y envío de la solicitud

```
//Declare a new GET request
req, err := http.NewRequest("GET", endpoint, nil)
if err != nil {
    panic(err)
}
//Add the payload to the request
param := req.URL.Query()
param.Add("q", searchTerm)
//Encoding the payload
req.URL.RawQuery = param.Encode()

//Insert the API token in the request header
req.Header.Add("Ocp-Apim-Subscription-Key", token)

//create new client
client := new(http.Client)
//Send the request
resp, err := client.Do(req)
if err != nil {
    panic(err)
}
//Close the response body at the function exit
defer resp.Body.Close() 
```

## <a name="printing-the-answer"></a>Impresión de la respuesta
El resultado de la búsqueda está dentro de la variable `resp`. Imprima el cuerpo de respuesta de la variable.

```
body, err := ioutil.ReadAll(resp.Body)
if err != nil {
    panic(err)
}
//Convert body from byte to string.
fmt.Println(string(body))
```

## <a name="put-everything-together"></a>Todo junto

```
package main
import (
    "fmt"
    "net/http"
    "io/ioutil"
    "time"
    "encoding/json"
)

//The struct that will contain the answer
type BingAnswer struct {
        Type         string `json:"_type"`
        QueryContext struct {
                OriginalQuery string `json:"originalQuery"`
        } `json:"queryContext"`
        WebPages struct {
                WebSearchURL          string `json:"webSearchUrl"`
                TotalEstimatedMatches int    `json:"totalEstimatedMatches"`
                Value                 []struct {
                        ID               string    `json:"id"`
                        Name             string    `json:"name"`
                        URL              string    `json:"url"`
                        IsFamilyFriendly bool      `json:"isFamilyFriendly"`
                        DisplayURL       string    `json:"displayUrl"`
                        Snippet          string    `json:"snippet"`
                        DateLastCrawled  time.Time `json:"dateLastCrawled"`
                        SearchTags       []struct {
                                Name    string `json:"name"`
                                Content string `json:"content"`
                        } `json:"searchTags,omitempty"`
                        About []struct {
                                Name string `json:"name"`
                        } `json:"about,omitempty"`
                } `json:"value"`
        } `json:"webPages"`
        RelatedSearches struct {
                ID    string `json:"id"`
                Value []struct {
                        Text         string `json:"text"`
                        DisplayText  string `json:"displayText"`
                        WebSearchURL string `json:"webSearchUrl"`
                } `json:"value"`
        } `json:"relatedSearches"`
        RankingResponse struct {
                Mainline struct {
                        Items []struct {
                                AnswerType  string `json:"answerType"`
                                ResultIndex int    `json:"resultIndex"`
                                Value       struct {
                                        ID string `json:"id"`
                                } `json:"value"`
                        } `json:"items"`
                } `json:"mainline"`
                Sidebar struct {
                        Items []struct {
                                AnswerType string `json:"answerType"`
                                Value      struct {
                                        ID string `json:"id"`
                                } `json:"value"`
                        } `json:"items"`
                } `json:"sidebar"`
        } `json:"rankingResponse"`
}

func main() {
    const endpoint = "https://api.cognitive.microsoft.com/bing/v7.0/search"
    token := "YOUR-ACCESS-KEY"
    searchTerm := "Microsoft Cognitive Services"
    
    req, err := http.NewRequest("GET", endpoint, nil)
    if err != nil {
        panic(err)
    }
    
    param := req.URL.Query()
    param.Add("q", searchTerm)
    req.URL.RawQuery = param.Encode()

    req.Header.Add("Ocp-Apim-Subscription-Key", token)
    
    client := new(http.Client)
    resp, err := client.Do(req)
    if err != nil {
        panic(err)
    }
    defer resp.Body.Close() 
    body, err := ioutil.ReadAll(resp.Body)
    if err != nil {
        panic(err)
    }
    //creating a new answer struct
    ans := new(BingAnswer)
    err = json.Unmarshal(body, &ans)
    if err != nil {
         fmt.Println(err)
    }
    //Iterate over search results
    for _, result := range ans.WebPages.Value {
         //Printing result name and URL
         fmt.Println(result.Name, "||", result.URL)
    }

}
```

## <a name="next-steps"></a>Pasos siguientes

[Bing Web Search overview](../overview.md) (Introducción a Bing Web Search)  
[Probarlo](https://azure.microsoft.com/services/cognitive-services/bing-web-search-api/)  
[Obtener una clave de acceso para evaluación gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api)
[Referencia de Bing Web Search API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference)

