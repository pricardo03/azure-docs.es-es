---
title: 'Inicio rápido: Búsqueda de vídeos mediante el SDK de Java: Bing Video Search'
titleSuffix: Azure Cognitive Services
description: Use este artículo de inicio rápido para enviar solicitudes de búsqueda de vídeo mediante el SDK de Bing Video Search para Java.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-video-search
ms.topic: quickstart
ms.date: 12/09/2019
ms.author: aahi
ms.openlocfilehash: 8222a940909b98fbcce9797fe2d59f0bfb554526
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2019
ms.locfileid: "75448337"
---
# <a name="quickstart-perform-a-video-search-with-the-bing-video-search-sdk-for-java"></a>Inicio rápido: Realización de una búsqueda de vídeo con el SDK de Bing Video Search para Java

Use este artículo de inicio rápido para empezar a buscar vídeos con el SDK de Bing Video Search para Java. Aunque Bing Video Search tiene una API de REST compatible con la mayoría de los lenguajes de programación, el SDK proporciona una forma sencilla de integrar el servicio en sus aplicaciones. El código fuente de este ejemplo está disponible en [GitHub](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples/tree/master/Search/BingVideoSearch) con anotaciones adicionales y características.

## <a name="prerequisites"></a>Prerequisites

* [Kit de desarrollo de Java (JDK)](https://www.oracle.com/technetwork/java/javase/downloads/jdk11-downloads-5066655.html)

* [Biblioteca Gson](https://github.com/google/gson)

[!INCLUDE [cognitive-services-bing-video-search-signup-requirements](../../../../includes/cognitive-services-bing-video-search-signup-requirements.md)]

Instale las dependencias del SDK de Bing Video Search con Maven, Gradle u otro sistema de administración de dependencias. El archivo POM de Maven requiere la declaración siguiente:

```xml
  <dependencies>
    <dependency>
      <groupId>com.microsoft.azure.cognitiveservices</groupId>
      <artifactId>azure-cognitiveservices-videosearch</artifactId>
      <version>0.0.1-beta-SNAPSHOT</version>
    </dependency>
  </dependencies> 
```

## <a name="create-and-initialize-a-project"></a>Creación e inicialización de un proyecto


Cree un proyecto de Java en su IDE o editor favorito e importe las bibliotecas siguientes.

    ```java
    import com.microsoft.azure.cognitiveservices.videosearch.*;
    import com.microsoft.azure.cognitiveservices.videosearch.VideoObject;
    import com.microsoft.rest.credentials.ServiceClientCredentials;
    import okhttp3.Interceptor;
    import okhttp3.OkHttpClient;
    import okhttp3.Request;
    import okhttp3.Response;
    import java.io.IOException;
    import java.util.ArrayList;
    import java.util.List; 
    ```

## <a name="create-a-search-client"></a>Creación de un cliente de búsqueda

1. Implemente el cliente `VideoSearchAPIImpl`, lo que requiere un punto de conexión de API y una instancia de la clase `ServiceClientCredentials`.

    ```java
    public static VideoSearchAPIImpl getClient(final String subscriptionKey) {
        return new VideoSearchAPIImpl("https://api.cognitive.microsoft.com/bing/v7.0/",
                new ServiceClientCredentials() {
                //...
                }
    )};
    ```

    Para implementar `ServiceClientCredentials`, siga estos pasos:

    1. Reemplace la función `applyCredentialsFilter()` por un objeto `OkHttpClient.Builder` como parámetro. 
        
        ```java
        //...
        new ServiceClientCredentials() {
                @Override
                public void applyCredentialsFilter(OkHttpClient.Builder builder) {
                //...
                }
        //...
        ```
    
    2. Dentro de `applyCredentialsFilter()`, llame a `builder.addNetworkInterceptor()`. Cree un nuevo objeto `Interceptor` e invalide su método `intercept()` para tomar un objeto interceptor `Chain`.

        ```java
        //...
        builder.addNetworkInterceptor(
            new Interceptor() {
                @Override
                public Response intercept(Chain chain) throws IOException {
                //...    
                }
            });
        ///...
        ```

    3. Dentro de la función `intercept`, cree variables para su solicitud. Use `Request.Builder()` para compilar su solicitud. Agregue su clave de suscripción al encabezado `Ocp-Apim-Subscription-Key` y devuelva `chain.proceed()` en el objeto de solicitud.
            
        ```java
        //...
        public Response intercept(Chain chain) throws IOException {
            Request request = null;
            Request original = chain.request();
            Request.Builder requestBuilder = original.newBuilder()
                    .addHeader("Ocp-Apim-Subscription-Key", subscriptionKey);
            request = requestBuilder.build();
            return chain.proceed(request);
        }
        //...
        ```

## <a name="send-a-search-request-and-receive-the-response"></a>Envío de una solicitud de búsqueda y recepción de la respuesta 

1. Cree una función denominada `VideoSearch()` que tome su clave de suscripción como una cadena. Cree una instancia del cliente de búsqueda creado anteriormente.
    
    ```java
    public static void VideoSearch(String subscriptionKey){
        VideoSearchAPIImpl client = VideoSDK.getClient(subscriptionKey);
        //...
    }
    ```
2. Dentro de `VideoSearch()`, envíe una solicitud de búsqueda de vídeo con el cliente, con `SwiftKey` como término de búsqueda. Si Video Search API devuelve resultados, obtenga el primer resultado e imprima su identificador, nombre y dirección URL, junto con el número total de vídeos devueltos. 
    
    ```java
    VideosInner videoResults = client.searchs().list("SwiftKey");

    if (videoResults == null){
        System.out.println("Didn't see any video result data..");
    }
    else{
        if (videoResults.value().size() > 0){
            VideoObject firstVideoResult = videoResults.value().get(0);

            System.out.println(String.format("Video result count: %d", videoResults.value().size()));
            System.out.println(String.format("First video id: %s", firstVideoResult.videoId()));
            System.out.println(String.format("First video name: %s", firstVideoResult.name()));
            System.out.println(String.format("First video url: %s", firstVideoResult.contentUrl()));
        }
        else{
            System.out.println("Couldn't find video results!");
        }
    }
    ```

3. Llame al método search desde su método main.

    ```java
    public static void main(String[] args) {
        VideoSDK.VideoSearch("YOUR-SUBSCRIPTION-KEY");
    }
    ```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Creación de una aplicación web de una sola página](../tutorial-bing-video-search-single-page-app.md)

## <a name="see-also"></a>Consulte también 

* [¿Qué es Bing Video Search API?](../overview.md)
* [Ejemplos del SDK de Cognitive Services para .NET](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)