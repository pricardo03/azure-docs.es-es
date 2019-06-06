---
title: 'Inicio rápido: Traducción de texto, Java (Translator Text API)'
titleSuffix: Azure Cognitive Services
description: En esta guía de inicio rápido, aprenderá a traducir una cadena de texto de inglés a italiano y alemán con Java y Translator Text REST API.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: quickstart
ms.date: 06/04/2019
ms.author: erhopf
ms.openlocfilehash: fbbe139160a7efd528e63e206aa37ac72b2542df
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66515184"
---
# <a name="quickstart-use-the-translator-text-api-to-translate-a-string-using-java"></a>Inicio rápido: Uso de Translator Text API para traducir una cadena mediante Java

En esta guía de inicio rápido, aprenderá a traducir una cadena de texto de inglés a italiano y alemán con Java y Translator Text REST API.

En esta guía de inicio rápido, se requiere una [cuenta de Azure Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) con un recurso de Translator Text. Si no tiene una cuenta, puede usar la [evaluación gratuita](https://azure.microsoft.com/try/cognitive-services/) para obtener una clave de suscripción.

## <a name="prerequisites"></a>Requisitos previos

* [JDK 7 o posterior](https://www.oracle.com/technetwork/java/javase/downloads/index.html)
* [Gradle](https://gradle.org/install/)
* Una clave de suscripción de Azure para Translator Text

## <a name="initialize-a-project-with-gradle"></a>Inicialización de un proyecto con Gradle

Comencemos por crear un directorio de trabajo para este proyecto. Desde la línea de comandos (o terminal), ejecute este comando:

```console
mkdir translator-sample
cd translator-sample
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
    mainClassName = "Translate"
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

A continuación, en esta carpeta, cree un archivo denominado `Translate.java`.

## <a name="import-required-libraries"></a>Importación de bibliotecas necesarias

Abra `Translate.java` y agregue las siguientes instrucciones de importación:

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
public class Translate {
  // All project code goes here...
}
```

Agregue estas líneas a la clase `Translate`. Observe que junto con la `api-version`, se han anexado dos parámetros adicionales a `url`. Estos parámetros se usan para establecer las salidas de la traducción. En este ejemplo, está establecida en alemán (`de`) e italiano (`it`). Asegúrese de actualizar el valor de la clave de suscripción.

```java
String subscriptionKey = "YOUR_SUBSCRIPTION_KEY";
String url = "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&to=de,it";
```

## <a name="create-a-client-and-build-a-request"></a>Creación de un cliente y compilación de una solución

Agregue esta línea a la clase `Translate` para crear una instancia de `OkHttpClient`:

```java
// Instantiates the OkHttpClient.
OkHttpClient client = new OkHttpClient();
```

A continuación, vamos a crear la solicitud POST. No dude en cambiar el texto para la traducción. El texto debe escaparse.

```java
// This function performs a POST request.
public String Post() throws IOException {
    MediaType mediaType = MediaType.parse("application/json");
    RequestBody body = RequestBody.create(mediaType,
            "[{\n\t\"Text\": \"Welcome to Microsoft Translator. Guess how many languages I speak!\"\n}]");
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
        Translate translateRequest = new Translate();
        String response = translateRequest.Post();
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

```json
[
  {
    "detectedLanguage": {
      "language": "en",
      "score": 1.0
    },
    "translations": [
      {
        "text": "Willkommen bei Microsoft Translator. Erraten Sie, wie viele Sprachen ich spreche!",
        "to": "de"
      },
      {
        "text": "Benvenuti a Microsoft Translator. Indovinate quante lingue parlo!",
        "to": "it"
      }
    ]
  }
]
```

## <a name="next-steps"></a>Pasos siguientes

Explore el código de ejemplo de esta guía de inicio rápido y otros, incluida la transliteración y la identificación del idioma, así como otros proyectos de Translator Text de ejemplo de GitHub.

> [!div class="nextstepaction"]
> [Explorar ejemplos de Java en GitHub](https://aka.ms/TranslatorGitHub?type=&language=java)

## <a name="see-also"></a>Otras referencias

* [Transliterar texto](quickstart-java-transliterate.md)
* [Identificar el idioma de entrada](quickstart-java-detect.md)
* [Obtener traducciones alternativas](quickstart-java-dictionary.md)
* [Obtener una lista de idiomas admitidos](quickstart-java-languages.md)
* [Determinar la longitud de las oraciones de una entrada](quickstart-java-sentences.md)
