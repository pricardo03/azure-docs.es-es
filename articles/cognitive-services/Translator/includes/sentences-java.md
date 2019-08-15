---
author: erhopf
ms.service: cognitive-services
ms.topic: include
ms.date: 08/06/2019
ms.author: erhopf
ms.openlocfilehash: a6c12a2fdc8616dd6f7107d11e8f6c77401811fb
ms.sourcegitcommit: 5d6c8231eba03b78277328619b027d6852d57520
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2019
ms.locfileid: "68968011"
---
## <a name="prerequisites"></a>Requisitos previos

* [JDK 7 o posterior](https://www.oracle.com/technetwork/java/javase/downloads/index.html)
* [Gradle](https://gradle.org/install/)
* Una clave de suscripción de Azure para Translator Text

## <a name="initialize-a-project-with-gradle"></a>Inicialización de un proyecto con Gradle

Comencemos por crear un directorio de trabajo para este proyecto. Desde la línea de comandos (o terminal), ejecute este comando:

```console
mkdir length-sentence-sample
cd length-sentence-sample
```

A continuación, va a inicializar un proyecto de Gradle. Este comando creará archivos de compilación esenciales para Gradle, el más importante, el `build.gradle.kts`, que se utiliza en tiempo de ejecución para crear y configurar la aplicación. Ejecute este comando desde el directorio de trabajo:

```console
gradle init --type basic
```

Cuando se le solicite que elija un **DSL**, seleccione **Kotlin**.

## <a name="configure-the-build-file"></a>Configuración del archivo de compilación

Localice `build.gradle.kts` y ábralo con el IDE o editor de texto favorito. A continuación, se copia en esta configuración de compilación:

```
plugins {
    java
    application
}
application {
    mainClassName = "LengthSentence"
}
repositories {
    mavenCentral()
}
dependencies {
    compile("com.squareup.okhttp:okhttp:2.5.0")
    compile("com.google.code.gson:gson:2.8.5")
}
```

Tenga en cuenta que este ejemplo tiene dependencias en OkHttp para las solicitudes HTTP, y Gson para administrar y analizar JSON. Si desea obtener más información acerca de las configuraciones de compilación, consulte [Creating New Gradle Builds](https://guides.gradle.org/creating-new-gradle-builds/) (Creación de nuevas compilaciones de Gradle).

## <a name="create-a-java-file"></a>Creación de un archivo Java

Vamos a crear una carpeta para la aplicación de ejemplo. En el directorio de trabajo, ejecute:

```console
mkdir -p src/main/java
```

A continuación, en esta carpeta, cree un archivo denominado `LengthSentence.java`.

## <a name="import-required-libraries"></a>Importación de bibliotecas necesarias

Abra `LengthSentence.java` y agregue las siguientes instrucciones de importación:

```java
import java.io.*;
import java.net.*;
import java.util.*;
import com.google.gson.*;
import com.squareup.okhttp.*;
```


## <a name="define-variables"></a>Definición de variables

En primer lugar, deberá crear una clase pública para el proyecto:

```java
public class LengthSentence {
  // All project code goes here...
}
```

Agregue estas líneas a la clase `LengthSentence`. Observe que junto con la `api-version`, puede definir el idioma de entrada. En este ejemplo es el inglés.

```java
String subscriptionKey = "YOUR_SUBSCRIPTION_KEY";
String url = "https://api.cognitive.microsofttranslator.com/breaksentence?api-version=3.0&language=en";
```
Si usa una suscripción a varios servicios de Cognitive Services, también debe incluir `Ocp-Apim-Subscription-Region` en los parámetros de la solicitud. [Más información sobre la autenticación con la suscripción a varios servicios](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication).

## <a name="create-a-client-and-build-a-request"></a>Creación de un cliente y compilación de una solución

Agregue esta línea a la clase `LengthSentence` para crear una instancia de `OkHttpClient`:

```java
// Instantiates the OkHttpClient.
OkHttpClient client = new OkHttpClient();
```

A continuación, vamos a crear la solicitud POST. Si lo desea, cambie el texto. El texto debe escaparse.

```java
// This function performs a POST request.
public String Post() throws IOException {
    MediaType mediaType = MediaType.parse("application/json");
    RequestBody body = RequestBody.create(mediaType,
            "[{\n\t\"Text\": \"How are you? I am fine. What did you do today?\"\n}]");
    Request request = new Request.Builder()
            .url(url).post(body)
            .addHeader("Ocp-Apim-Subscription-Key", subscriptionKey)
            .addHeader("Content-type", "application/json").build();
    Response response = client.newCall(request).execute();
    return response.body().string();
}
```

## <a name="create-a-function-to-parse-the-response"></a>Creación de una función para analizar la respuesta

Esta sencilla función analiza y embellece la respuesta JSON del servicio Translator Text.

```java
// This function prettifies the json response.
public static String prettify(String json_text) {
    JsonParser parser = new JsonParser();
    JsonElement json = parser.parse(json_text);
    Gson gson = new GsonBuilder().setPrettyPrinting().create();
    return gson.toJson(json);
}
```

## <a name="put-it-all-together"></a>Colocación de todo junto

El último paso consiste en realizar una solicitud y obtener una respuesta. Agregue estas líneas al proyecto:

```java
public static void main(String[] args) {
    try {
        LengthSentence lengthSentenceRequest = new LengthSentence();
        String response = lengthSentenceRequest.Post();
        System.out.println(prettify(response));
    } catch (Exception e) {
        System.out.println(e);
    }
}
```

## <a name="run-the-sample-app"></a>Ejecutar la aplicación de ejemplo

Eso es todo, ya está listo para ejecutar la aplicación de ejemplo. Desde la línea de comandos (o sesión de terminal), navegue hasta la raíz del directorio de trabajo y ejecute:

```console
gradle build
```

Cuando la compilación se complete, ejecute lo siguiente:

```console
gradle run
```

## <a name="sample-response"></a>Respuesta de muestra

Se devuelve una respuesta correcta en JSON, tal como se muestra en el siguiente ejemplo:

```json
[
  {
    "detectedLanguage": {
      "language": "en",
      "score": 1.0
    },
    "sentLen": [
      13,
      11,
      22
    ]
  }
]
```

## <a name="next-steps"></a>Pasos siguientes

Eche un vistazo a la referencia de API para comprender todo lo que puede hacer con Translator Text API.

> [!div class="nextstepaction"]
> [Referencia de API](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference)
