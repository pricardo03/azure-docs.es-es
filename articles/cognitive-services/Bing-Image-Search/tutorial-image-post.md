---
title: Carga de imágenes de Bing para obtener información detallada | Microsoft Docs
description: Aplicación de consola que utiliza Bing Image Search API para cargar una imagen y obtener información detallada de la imagen.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 12/07/2017
ms.author: v-gedod
ms.openlocfilehash: f0bf32a9951527a072fffe464f6b5f50d0f237a2
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35380063"
---
# <a name="tutorial-bing-image-upload-for-insights"></a>Tutorial: Carga de imágenes de Bing para obtener información detallada

Bing Image Search API proporciona una opción `POST` para cargar una imagen y buscar información relativa a la imagen. Esta aplicación de consola de C# carga una imagen mediante el punto de conexión de Image Search para obtener detalles sobre la imagen.
Los resultados, en pocas palabras, son objetos JSON como el siguiente:

![[Resultados JSON]](media/cognitive-services-bing-images-api/jsonResult.jpg)

Este tutorial explica cómo realizar lo siguiente:

> [!div class="checklist"]
> * Utilizar el punto de conexión `/details` de Image Search en una solicitud `POST`.
> * Especificar encabezados para la solicitud.
> * Utilizar parámetros de dirección URL para especificar los resultados.
> * Cargar los datos de imagen y enviar la solicitud `POST`.
> * Imprimir los resultados JSON en la consola.

## <a name="app-components"></a>Componentes de la aplicación

La aplicación del tutorial incluye tres partes:

> [!div class="checklist"]
> * Configuración del punto de conexión para especificar el punto de conexión de Bing Image Search y los encabezados necesarios
> * Carga de archivos de imagen para la solicitud `POST` al punto de conexión
> * Análisis de los resultados JSON que son los detalles devueltos por la solicitud `POST`

## <a name="scenario-overview"></a>Información general de escenario
Hay [tres puntos de conexión de Image Search](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/image-search-endpoint). El punto de conexión `/details` puede utilizar una solicitud `POST` con datos de imagen en el cuerpo de la solicitud.
```
https://api.cognitive.microsoft.com/bing/v7.0/images/details
```
El parámetro de dirección URL `modules` que sigue a `/details?` especifica qué tipo de detalles contienen los resultados:
* `modules=All`
* `modules=RecognizedEntities` (personas o lugares visibles en la imagen)

Especifique `modules=All` en la solicitud `POST` para obtener el texto JSON que incluye la siguiente lista:
* `bestRepresentativeQuery` - una consulta de Bing que devuelve imágenes similares a la imagen cargada
* `detectedObjects` como un rectángulo de selección o zonas activas en la imagen
* `image` metadatos
* `imageInsightsToken` - token para una solicitud `GET` posterior que obtiene `RecognizedEntities` (personas o lugares visibles en la imagen) 
* `imageTags`
* `pagesIncluding` - páginas web que incluyen la imagen
* `relatedSearches`
* `visuallySimilarImages`

Especifique `modules=RecognizedEntities` en la solicitud `POST` para obtener solo `imageInsightsToken`, que se usa en una solicitud `GET` posterior. Identifica a las personas o los lugares visibles en la imagen.

## <a name="webclient-and-headers-for-the-post-request"></a>WebClient y encabezados para la solicitud POST
Cree un objeto `WebClient` y establezca los encabezados. Todas las solicitudes de Bing Search API necesitan `Ocp-Apim-Subscription-Key`. Una solicitud `POST` para cargar una imagen también debe especificar `ContentType: multipart/form-data`.

```
            WebClient client = new WebClient();
            client.Headers["Ocp-Apim-Subscription-Key"] = accessKey;
            client.Headers["ContentType"] = "multipart/form-data"; 
```

## <a name="upload-the-image-and-get-results"></a>Carga de la imagen y obtención de resultados

La clase `WebClient` incluye el método `UpLoadFile` que da formato a datos para la solicitud `POST`. Da formato a `RequestStream` y a llamadas `HttpWebRequest`, evitando numerosas complejidades.
Llame a `WebClient.UpLoadFile` con el punto de conexión `/details` y el archivo de imagen para cargar. La respuesta son datos binarios que se pueden convertir fácilmente en JSON. 

Utilice el texto JSON para inicializar una instancia de la estructura `SearchResult` (consulte el [código fuente de aplicación](tutorial-image-post-source.md) para el contexto).
```        
         const string uriBase = "https://api.cognitive.microsoft.com/bing/v7.0/images/details";

        // The image to upload. Replace with your file and path.
        const string imageFile = "ansel-adams-tetons-snake-river.jpg";
            
        byte[] resp = client.UploadFile(uriBase + "?modules=All", imageFile);
        var json = System.Text.Encoding.Default.GetString(resp);

        // Create result object for return
        var searchResult = new SearchResult()
        {
            jsonResult = json,
            relevantHeaders = new Dictionary<String, String>()
        };
```

## <a name="print-the-results"></a>Impresión de resultados
El resto del código analiza el resultado JSON y lo imprime en la consola.

```
        /// <summary>
        /// Formats the given JSON string by adding line breaks and indents.
        /// </summary>
        /// <param name="json">The raw JSON string to format.</param>
        /// <returns>The formatted JSON string.</returns>
        static string JsonPrettyPrint(string json)
        {
            if (string.IsNullOrEmpty(json))
                return string.Empty;

            json = json.Replace(Environment.NewLine, "").Replace("\t", "");

            StringBuilder sb = new StringBuilder();
            bool quote = false;
            bool ignore = false;
            char last = ' ';
            int offset = 0;
            int indentLength = 2;

            foreach (char ch in json)
            {
                switch (ch)
                {
                    case '"':
                        if (!ignore) quote = !quote;
                        break;
                    case '\\':
                        if (quote && last != '\\') ignore = true;
                        break;
                }

                if (quote)
                {
                    sb.Append(ch);
                    if (last == '\\' && ignore) ignore = false;
                }
                else
                {
                    switch (ch)
                    {
                        case '{':
                        case '[':
                            sb.Append(ch);
                            sb.Append(Environment.NewLine);
                            sb.Append(new string(' ', ++offset * indentLength));
                            break;
                        case '}':
                        case ']':
                            sb.Append(Environment.NewLine);
                            sb.Append(new string(' ', --offset * indentLength));
                            sb.Append(ch);
                            break;
                        case ',':
                            sb.Append(ch);
                            sb.Append(Environment.NewLine);
                            sb.Append(new string(' ', offset * indentLength));
                            break;
                        case ':':
                            sb.Append(ch);
                            sb.Append(' ');
                            break;
                        default:
                            if (quote || ch != ' ') sb.Append(ch);
                            break;
                    }
                }
                last = ch;
            }

            return sb.ToString().Trim();
        }
```
## <a name="get-request-using-the-imageinsightstoken"></a>Solicitud GET mediante ImageInsightsToken
Para usar el objeto `ImageInsightsToken` devuelto con los resultados de `POST`, cree una solicitud `GET` similar a la siguiente:
```
https://api.cognitive.microsoft.com/bing/v7.0/images/details?InsightsToken="bcid_A2C4BB81AA2C9EF8E049C5933C546449*ccid_osS7gaos*mid_BF7CC4FC4A882A3C3D56E644685BFF7B8BACEAF2
```
Si hay personas o lugares identificables en la imagen, esta solicitud devolverá información sobre ellos.
Las [guías de inicio rápido](https://docs.microsoft.com/azure/cognitive-services/bing-image-search) contienen numerosos ejemplos de código.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Referencia de Bing Image Search API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference)

