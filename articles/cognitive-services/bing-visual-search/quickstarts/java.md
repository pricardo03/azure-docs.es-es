---
title: 'Inicio rápido: Creación de una consulta de búsqueda visual, Java - Bing Visual Search'
titleSuffix: Azure Cognitive Services
description: Carga de una imagen en Bing Visual Search API y obtención de información detallada sobre la misma.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-visual-search
ms.topic: quickstart
ms.date: 5/16/2018
ms.author: scottwhi
ms.openlocfilehash: f54914b846c6a001a9fb10d938a038e390abf6bf
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2018
ms.locfileid: "50416450"
---
# <a name="quickstart-your-first-bing-visual-search-query-in-java"></a>Inicio rápido: Su primera consulta de Bing Visual Search en Java

Bing Visual Search API devuelve información sobre una imagen proporcionada. Puede proporcionar la imagen utilizando la dirección URL de la misma, un token de conclusiones, o simplemente cargando una imagen. Para obtener más información acerca de estas opciones, consulte [What is Bing Visual Search API?](../overview.md) (¿Qué es Bing Visual Search API?). En este artículo se muestra la forma de cargar una imagen. Cargar una imagen puede serle de utilidad en aquellos momentos en los que use un dispositivo móvil para, por ejemplo, hacer una foto de un lugar interesante y así poder obtener información acerca de él. Asimismo, estos detalles pueden incluir curiosidades sobre ese lugar de interés. 

Si carga una imagen local, a continuación verá los datos del formulario que debe incluir en el cuerpo del elemento POST. Los datos del formulario deben incluir el encabezado Content-Disposition. Asimismo, el parámetro `name` se debe establecer en "imagen" y el parámetro `filename` se puede establecer en cualquier cadena. El contenido del formulario es el binario de la imagen. Recuerde que el tamaño de imagen máximo que puede cargar es de 1 MB. 

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

En este artículo se incluye una aplicación simple de consola que envía una solicitud de Bing Visual Search API y muestra los resultados de búsqueda de JSON. Si bien esta aplicación está escrita en Java, la API es un servicio web RESTful compatible con cualquier lenguaje de programación que pueda realizar solicitudes HTTP y analizar JSON. 


## <a name="prerequisites"></a>Requisitos previos

Necesitará [JDK 7 u 8](https://aka.ms/azure-jdks) para compilar y ejecutar este código. Puede utilizar un IDE de Java si tiene un favorito, pero bastará con un editor de texto.

Para realizar este tutorial de inicio rápido, puede usar una clave de suscripción de [evaluación gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) o una clave de suscripción de pago.

## <a name="running-the-application"></a>Ejecución de la aplicación

A continuación se muestra cómo cargar la imagen mediante MultipartEntityBuilder en Java.

Para ejecutar esta aplicación, siga estos pasos:

1. Descargue o instale la [biblioteca gson](https://github.com/google/gson). También se puede obtener a través de Maven.
2. Cree un nuevo proyecto de Java en su IDE o editor favorito.
3. Agregue el código que se proporciona a un archivo denominado `VisualSearch.java`.
4. Reemplace el valor `subscriptionKey` con la clave de suscripción.
4. Reemplace el valor `imagePath` con la ruta de acceso de la imagen que va a cargar.
5. Ejecute el programa.


```java
package uploadimage;

import java.util.*;
import java.io.*;

/*
 * Gson: https://github.com/google/gson
 * Maven info:
 *     groupId: com.google.code.gson
 *     artifactId: gson
 *     version: 2.8.2
 *
 * Once you have compiled or downloaded gson-2.8.2.jar, assuming you have placed it in the
 * same folder as this file (BingImageSearch.java), you can compile and run this program at
 * the command line as follows.
 *
 * javac BingImageSearch.java -classpath .;gson-2.8.2.jar -encoding UTF-8
 * java -cp .;gson-2.8.2.jar BingImageSearch
 */

import com.google.gson.Gson;
import com.google.gson.GsonBuilder;
import com.google.gson.JsonObject;
import com.google.gson.JsonParser;

// http://hc.apache.org/downloads.cgi (HttpComponents Downloads) HttpClient 4.5.5

import org.apache.http.HttpEntity;
import org.apache.http.HttpResponse;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.entity.ContentType;
import org.apache.http.entity.mime.MultipartEntityBuilder;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClientBuilder;


public class UploadImage2 {

    static String endpoint = "https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch";
    static String subscriptionKey = "<yoursubscriptionkeygoeshere";
    static String imagePath = "<pathtoyourimagetouploadgoeshere>";

    /**
     * @param args the command line arguments
     */
    public static void main(String[] args) {
        
        CloseableHttpClient httpClient = HttpClientBuilder.create().build();
        
        try {
            HttpEntity entity = MultipartEntityBuilder
                .create()
                .addBinaryBody("image", new File(imagePath))
                .build();

            HttpPost httpPost = new HttpPost(endpoint);
            httpPost.setHeader("Ocp-Apim-Subscription-Key", subscriptionKey);
            httpPost.setEntity(entity);
            HttpResponse response = httpClient.execute(httpPost);

            InputStream stream = response.getEntity().getContent();
            String json = new Scanner(stream).useDelimiter("\\A").next();

            System.out.println("\nJSON Response:\n");
            System.out.println(prettify(json));
        }
        catch (IOException e)
        {
            e.printStackTrace(System.out);
            System.exit(1);
        }
        catch (Exception e) {
            e.printStackTrace(System.out);
            System.exit(1);
        }
    }
    
    // pretty-printer for JSON; uses GSON parser to parse and re-serialize
    public static String prettify(String json_text) {
        JsonParser parser = new JsonParser();
        JsonObject json = parser.parse(json_text).getAsJsonObject();
        Gson gson = new GsonBuilder().setPrettyPrinting().create();
        return gson.toJson(json);
    }
    
}
```

## <a name="next-steps"></a>Pasos siguientes

[Obtenga información acerca de una imagen mediante un token de conclusión](../use-insights-token.md)  
[Tutorial sobre la carga de imágenes en Bing Visual Search](../tutorial-visual-search-image-upload.md)
[Tutorial sobre la aplicación de una sola página en Bing Visual Search](../tutorial-bing-visual-search-single-page-app.md)  
[Introducción a Bing Visual Search](../overview.md)  
[Pruébelo](https://aka.ms/bingvisualsearchtryforfree)  
[Obtenga una clave de acceso para evaluación gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-visual-search-api)  
[Referencia a Bing Visual Search API](https://aka.ms/bingvisualsearchreferencedoc)

