---
title: Tutorial de resultados del área de recorte del SDK de Bing Visual Search | Microsoft Docs
description: Cómo usar el SDK de Bing Visual Search para obtener las direcciones URL de imágenes similares al área de recorte de la imagen cargada.
services: cognitive-services
author: mikedodaro
manager: ronakshah
ms.service: cognitive-services
ms.component: bing-visual-search
ms.topic: article
ms.date: 06/20/2018
ms.author: rosh
ms.openlocfilehash: 9bc3c180f108025f442343d8c5356982a83826a6
ms.sourcegitcommit: 0408c7d1b6dd7ffd376a2241936167cc95cfe10f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/26/2018
ms.locfileid: "36958411"
---
# <a name="tutorial-bing-visual-search-sdk-image-crop-area-and-results"></a>Tutorial: Resultados y área de recorte de imagen del SDK de Bing Visual Search
El SDK de Visual Search incluye una opción para seleccionar un área de una imagen y buscar las imágenes en línea que sean similares al área de recorte de la imagen más grande.  En este ejemplo se especifica un área de recorte que muestra a una persona de una imagen que contiene varias personas.  El código envía el área de recorte y la dirección URL de la imagen de mayor tamaño, y devuelve los resultados que incluyen direcciones URL de Bing Search y direcciones URL de imágenes similares que se encuentran en línea.

## <a name="prerequisites"></a>requisitos previos

Necesitará [Visual Studio 2017](https://www.visualstudio.com/downloads/) para obtener este código que se ejecuta en Windows. (La edición gratuita Community Edition funcionará).

Debe tener una [cuenta de Cognitive Services API](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) con Bing Search APIs. La [cuenta de evaluación gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) es suficiente para esta guía de inicio rápido. Necesita la clave de acceso que se le proporciona al activar la versión de evaluación gratuita, o puede usar una clave de suscripción de pago desde su panel de Azure.

## <a name="application-dependencies"></a>Dependencias de aplicaciones
Para configurar una aplicación de consola con el SDK de Bing Web Search, vaya a la opción Administrar paquetes NuGet del Explorador de soluciones en Visual Studio. Agregue el paquete Microsoft.Azure.CognitiveServices.Search.VisualSearch.

La instalación del paquete del SDK de NuGet Web Search también instala las dependencias, entre las que se incluyen:

* Microsoft.Rest.ClientRuntime
* Microsoft.Rest.ClientRuntime.Azure
* Newtonsoft.Json

## <a name="image-and-crop-area"></a>Image y área de recorte
La siguiente imagen muestra el equipo de responsables sénior de Microsoft.  Con el SDK de Visual Search, cargamos un área de recorte de la imagen y buscamos otras imágenes y páginas web que incluyan la entidad del área seleccionada en la imagen más grande.  En este caso, la entidad es una persona.

![Equipo de responsables sénior de Microsoft](./media/MS_SrLeaders.jpg)

## <a name="specify-the-crop-area-as-imageinfo-in-visualsearchrequest"></a>Especificación del área de recorte como ImageInfo en VisualSearchRequest
En este ejemplo se utiliza un área de recorte de la imagen anterior que especifica las coordenadas superior izquierda e inferior derecha como un porcentaje de toda la imagen.  El siguiente código crea un objeto `ImageInfo` a partir del área de recorte y carga el objeto `ImageInfo` en una solicitud `VisualSearchRequest`.  El objeto `ImageInfo` también incluye la dirección URL de la imagen en línea.

```
CropArea CropArea = new CropArea(top: (float)0.01, bottom: (float)0.30, left: (float)0.01, right: (float)0.20);
string imageURL = "https://docs.microsoft.com/en-us/azure/cognitive-services/bing-visual-search/media/ms_srleaders.jpg;
ImageInfo imageInfo = new ImageInfo(cropArea: CropArea, url: imageURL);

VisualSearchRequest visualSearchRequest = new VisualSearchRequest(imageInfo: imageInfo);
```
## <a name="search-for-images-similar-to-crop-area"></a>Búsqueda de imágenes similares para al área de recorte
`VisualSearchRequest` contiene la información del área de recorte sobre la imagen y su dirección URL.  El método `VisualSearchMethodAsync` obtiene los resultados.
```
Console.WriteLine("\r\nSending visual search request with knowledgeRequest that contains URL and crop area");
var visualSearchResults = client.Images.VisualSearchMethodAsync(knowledgeRequest: visualSearchRequest).Result; 

```

## <a name="get-the-url-data-from-imagemoduleaction"></a>Obtención de los datos de dirección URL de ImageModuleAction
Los resultados de Visual Search son objetos `ImageTag`.  Cada etiqueta contiene una lista de objetos `ImageAction`.  Cada elemento `ImageAction` contiene un campo `Data` que es una lista de valores que dependen del tipo de acción:

Puede obtener los distintos tipos con el código siguiente:
```
Console.WriteLine("\r\n" + "ActionType: " + i.ActionType + " -> WebSearchUrl: " + i.WebSearchUrl);

```
La aplicación completa devuelve:

* ActionType: PagesIncluding WebSearchURL:
* ActionType: MoreSizes WebSearchURL:
* ActionType: VisualSearch WebSearchURL:
* ActionType: ImageById WebSearchURL: 
* ActionType: RelatedSearches WebSearchURL:
* ActionType: Entity -> WebSearchUrl: https://www.bing.com/cr?IG=E40D0E1A13404994ACB073504BC937A4&CID=03DCF882D7386A442137F49BD6596BEF&rd=1&h=BvvDoRtmZ35Xc_UZE4lZx6_eg7FHgcCkigU1D98NHQo&v=1&r=https%3a%2f%2fwww.bing.com%2fsearch%3fq%3dSatya%2bNadella&p=DevEx,5380.1
* ActionType: TopicResults -> WebSearchUrl: https://www.bing.com/cr?IG=E40D0E1A13404994ACB073504BC937A4&CID=03DCF882D7386A442137F49BD6596BEF&rd=1&h=3QGtxPb3W9LemuHRxAlW4CW7XN4sPkUYCUynxAqI9zQ&v=1&r=https%3a%2f%2fwww.bing.com%2fdiscover%2fnadella%2bsatya&p=DevEx,5382.1
* ActionType: ImageResults -> WebSearchUrl: https://www.bing.com/cr?IG=E40D0E1A13404994ACB073504BC937A4&CID=03DCF882D7386A442137F49BD6596BEF&rd=1&h=l-WNHO89Kkw69AmIGe2MhlUp6MxR6YsJszgOuM5sVLs&v=1&r=https%3a%2f%2fwww.bing.com%2fimages%2fsearch%3fq%3dSatya%2bNadella&p=DevEx,5384.1

Tal y como se muestra en la lista anterior, `Entity` `ActionType` contiene una consulta de Bing Search que devuelve información acerca de una persona, lugar o cosa reconocibles.  Los tipos `TopicResults` y `ImageResults` contienen consultas de imágenes relacionadas. Las direcciones URL de la lista vinculan a los resultados de Bing Search.


## <a name="pagesincluding-actiontype-urls-of-images-found-by-visual-search"></a>Direcciones URL de PagesIncluding ActionType de las imágenes encontradas por Visual Search

Para la obtención de las direcciones URL de imágenes reales se requiere una conversión que lea un elemento `ActionType` como `ImageModuleAction`, que contenga un elemento `Data` con una lista de valores.  Cada valor es la dirección URL de una imagen.  El siguiente código convierte el tipo de acción `PagesIncluding` en `ImageModuleAction` y lee los valores.
```
    if (i.ActionType == "PagesIncluding")
    {
        foreach(ImageObject o in (i as ImageModuleAction).Data.Value)
        {
            Console.WriteLine("ContentURL: " + o.ContentUrl);
        }
    }
```

## <a name="complete-code"></a>Código completo

El código siguiente ejecuta los ejemplos anteriores. Envía una imagen binaria en el cuerpo de la solicitud post, junto con un objeto cropArea y muestra las direcciones URL de Bing Search para cada ActionType. Si ActionType es PagesIncluding, el código obtiene los elementos de ImageObject Data.  Data contiene una lista de valores, que son las direcciones URL de las imágenes en las páginas web.  Copie las direcciones URL de Visual Search resultantes y péguelas en el explorador para mostrar los resultados. Copie los elementos ContentUrl y péguelos en el explorador para mostrar las imágenes.

```
using System;
using System.IO;
using System.Linq;
using Microsoft.Azure.CognitiveServices.Search.VisualSearch;
using Microsoft.Azure.CognitiveServices.Search.VisualSearch.Models;
using Microsoft.Azure.CognitiveServices.Search.ImageSearch;

namespace VisualSearchFeatures
{
    class Program
    {
        static void Main(string[] args)
        {
            String subscriptionKey = "YOUR-ACCESS-KEY";

            VisualSearchImageBinaryWithCropArea(subscriptionKey);

            Console.WriteLine("Any key to quit...");
            Console.ReadKey();

        }

        public static void VisualSearchImageBinaryWithCropArea(string subscriptionKey)
        {
            var client = new VisualSearchAPI(new Microsoft.Azure.CognitiveServices.Search.VisualSearch.ApiKeyServiceClientCredentials(subscriptionKey));

            try
            {
                CropArea CropArea = new CropArea(top: (float)0.01, bottom: (float)0.30, left: (float)0.01, right: (float)0.20);
                
                // The ImageInfo struct specifies the crop area in the image and the URL of the larger image. 
                string imageURL = "https://docs.microsoft.com/en-us/azure/cognitive-services/bing-visual-search/media/ms_srleaders.jpg";
                ImageInfo imageInfo = new ImageInfo(cropArea: CropArea, url: imageURL);
                
                VisualSearchRequest visualSearchRequest = new VisualSearchRequest(imageInfo: imageInfo);

                var visualSearchResults = client.Images.VisualSearchMethodAsync(knowledgeRequest: visualSearchRequest).Result; 

                Console.WriteLine("\r\nVisual search request with image from URL and crop area combined in knowledgeRequest");

                if (visualSearchResults == null)
                {
                    Console.WriteLine("No visual search result data.");
                }
                else
                {
                    // List of tags
                    if (visualSearchResults.Tags.Count > 0)
                    {

                        foreach(ImageTag t in visualSearchResults.Tags)
                        {
                            foreach (ImageAction i in t.Actions)
                            { 
                                Console.WriteLine("\r\n" + "ActionType: " + i.ActionType + " -> WebSearchUrl: " + i.WebSearchUrl);

                                if (i.ActionType == "PagesIncluding")
                                {
                                    foreach(ImageObject o in (i as ImageModuleAction).Data.Value)
                                    {
                                        Console.WriteLine("ContentURL: " + o.ContentUrl);
                                    }
                                }
                            }

                        }

                    }
                    else
                    {
                        Console.WriteLine("Couldn't find image tags!");
                    }
                }
            }

            catch (Exception ex)
            {
                Console.WriteLine("Encountered exception. " + ex.Message);
            }
        }
    }
}

```
## <a name="next-steps"></a>Pasos siguientes
[Respuesta de Visual Search](https://docs.microsoft.com/en-us/azure/cognitive-services/bing-visual-search/overview#the-response)