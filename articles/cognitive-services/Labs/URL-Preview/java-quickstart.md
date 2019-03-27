---
title: 'Inicio rápido: Project URL Preview, Java'
titlesuffix: Azure Cognitive Services
description: Ejemplo de script para empezar a usar Project URL Preview con Java.
services: cognitive-services
author: mikedodaro
manager: nitinme
ms.service: cognitive-services
ms.subservice: url-preview
ms.topic: quickstart
ms.date: 04/24/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: f4acab2cb4522048f4a78b12ca41161695a31e18
ms.sourcegitcommit: aa3be9ed0b92a0ac5a29c83095a7b20dd0693463
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2019
ms.locfileid: "58259608"
---
# <a name="quickstart-url-preview-with-java"></a>Inicio rápido: URL Preview con Java

En el ejemplo de Java siguiente se crea una vista previa de la dirección URL del sitio web de SwiftKey: https://swiftkey.com/en.

## <a name="prerequisites"></a>Requisitos previos

Obtenga una clave de acceso para la evaluación gratuita de los [Laboratorios de Cognitive Services](https://aka.ms/answersearchsubscription).

## <a name="request"></a>Solicitud 

El siguiente código crea una instancia de `WebRequest`, establece el encabezado de la clave de acceso y agrega una cadena de consulta para "https:\//swiftkey.com/en".  Después envía la solicitud y asigna la respuesta a una cadena que contiene el texto JSON.

```
    // construct URL of search request (endpoint + query string)

    static String host = "https://api.labs.cognitive.microsoft.com";
    static String path = "/urlpreview/v7.0/search";

    static String searchTerm = "https://swiftkey.com/en";

    URL url = new URL(host + path + "?q=" +  URLEncoder.encode(searchQuery, "UTF-8") + &mkt=en-us");
    HttpsURLConnection connection = (HttpsURLConnection)url.openConnection();
    connection.setRequestProperty("Ocp-Apim-Subscription-Key", subscriptionKey);

    // receive JSON body
    InputStream stream = connection.getInputStream();
    String response = new Scanner(stream).useDelimiter("\\A").next();

    // construct result object for return
    SearchResults results = new SearchResults(new HashMap<String, String>(), response);
```

## <a name="complete-code"></a>Código completo

Bing Answer Search API devuelve resultados desde el motor de búsqueda de Bing.
1. Descargue o instale la biblioteca gson.
2. Cree un proyecto de Java en su IDE o editor favorito.
3. Agregue el código que se le ha proporcionado a continuación.
4. Reemplace el valor subscriptionKey por una clave de acceso válida para la suscripción.
5. Ejecute el programa.

```
package UrlPreviewpkg;

import java.net.*;
import java.util.*;
import java.io.*;
import javax.net.ssl.HttpsURLConnection;

import com.google.gson.Gson;
import com.google.gson.GsonBuilder;
import com.google.gson.JsonObject;
import com.google.gson.JsonParser;

/*
 * Gson: https://github.com/google/gson
 * Maven info:
 *     groupId: com.google.code.gson
 *     artifactId: gson
 *     version: 2.8.1
 *
 * When you have compiled or downloaded gson-2.8.1.jar and have placed it in the
 * same folder as this file (BingImageSearch.java), you can compile and run this program at
 * the command line as follows.
 *
 * javac UrlPreviewcls.java -classpath .;gson-2.8.1.jar -encoding UTF-8
 * java -cp .;gson-2.8.1.jar UrlPreview
 */

public class UrlPreview 
{
    // Replace the subscriptionKey string value with your valid subscription key.
    static String subscriptionKey = "YOUR-ACCESS-KEY";

    static String host = "https://api.labs.cognitive.microsoft.com";
    static String path = "/urlpreview/v7.0/search";

    static String searchTerm = "https://swiftkey.com/en";

    public static SearchResults SearchImages (String searchQuery) throws Exception {
        // construct URL of search request (endpoint + query string)
        URL url = new URL(host + path + "?q=" +  URLEncoder.encode(searchQuery, "UTF-8"));
        HttpsURLConnection connection = (HttpsURLConnection)url.openConnection();
        connection.setRequestProperty("Ocp-Apim-Subscription-Key", subscriptionKey);

        // receive JSON body
        InputStream stream = connection.getInputStream();
        String response = new Scanner(stream).useDelimiter("\\A").next();

        // construct result object for return
        SearchResults results = new SearchResults(new HashMap<String, String>(), response);

        // extract Bing-related HTTP headers
        Map<String, List<String>> headers = connection.getHeaderFields();
        for (String header : headers.keySet()) {
            if (header == null) continue;      // may have null key
            if (header.startsWith("BingAPIs-") || header.startsWith("X-MSEdge-")) {
                results.relevantHeaders.put(header, headers.get(header).get(0));
            }
        }

        stream.close();
        return results;
    }

    // pretty-printer for JSON; uses GSON parser to parse and re-serialize
    public static String prettify(String json_text) {
        JsonParser parser = new JsonParser();
        JsonObject json = parser.parse(json_text).getAsJsonObject();
        Gson gson = new GsonBuilder().setPrettyPrinting().create();
        return gson.toJson(json);
    }

    public static void main (String[] args) {
        if (subscriptionKey.length() != 32) {
            System.out.println("Invalid Bing Search API subscription key!");
            System.out.println("Please paste yours into the source code.");
            System.exit(1);
        }

        try {
            System.out.println("Searching the Web for: " + searchTerm);

            SearchResults result = SearchImages(searchTerm);

            System.out.println("\nRelevant HTTP Headers:\n");
            for (String header : result.relevantHeaders.keySet())
                System.out.println(header + ": " + result.relevantHeaders.get(header));

            System.out.println("\nJSON Response:\n");
            System.out.println(prettify(result.jsonResponse));
        }
        catch (Exception e) {
            e.printStackTrace(System.out);
            System.exit(1);
        }
    }
}
//Container class for search results encapsulates relevant headers and JSON data
class SearchResults{
 HashMap<String, String> relevantHeaders;
 String jsonResponse;
 SearchResults(HashMap<String, String> headers, String json) {
     relevantHeaders = headers;
     jsonResponse = json;
 }
}

```

## <a name="next-steps"></a>Pasos siguientes
- [Inicio rápido de C#](csharp.md)
- [Inicio rápido de JavaScript](javascript.md)
- [Inicio rápido de Node](node-quickstart.md)
- [Inicio rápido de Python](python-quickstart.md)
