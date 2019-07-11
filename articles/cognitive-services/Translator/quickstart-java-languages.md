---
title: 'Inicio rápido: Obtención de una lista de idiomas admitidos, Java (Translator Text API)'
titleSuffix: Azure Cognitive Services
description: En esta guía de inicio rápido se obtiene una lista de los idiomas admitidos para la traducción, la transliteración y la búsqueda en el diccionario mediante Translator Text API.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: quickstart
ms.date: 06/04/2019
ms.author: swmachan
ms.openlocfilehash: 1c6db84acc2bca8e649f5f7acb1dcbdfd6d1269d
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2019
ms.locfileid: "67445121"
---
# <a name="quickstart-use-the-translator-text-api-to-get-a-list-of-supported-languages-using-java"></a>Inicio rápido: Uso de Translator Text API para obtener una lista de los idiomas compatibles mediante Java

En esta guía de inicio rápido se obtiene una lista de los idiomas admitidos para la traducción, la transliteración y la búsqueda en el diccionario mediante Translator Text API.

## <a name="prerequisites"></a>Requisitos previos

* [JDK 7 o posterior](https://www.oracle.com/technetwork/java/javase/downloads/index.html)
* [Gradle](https://gradle.org/install/)

## <a name="initialize-a-project-with-gradle"></a>Inicialización de un proyecto con Gradle

Comencemos por crear un directorio de trabajo para este proyecto. Desde la línea de comandos (o terminal), ejecute este comando:

```console
mkdir get-languages-sample
cd get-languages-sample
```

A continuación, va a inicializar un proyecto de Gradle. Este comando creará archivos de compilación esenciales para Gradle, el más importante, el `build.gradle.kts`, que se utiliza en tiempo de ejecución para crear y configurar la aplicación. Ejecute este comando desde el directorio de trabajo:

```console
gradle init --type basic
```

Cuando se le solicite que elija un **DSL**, seleccione **Kotlin**.

## <a name="configure-the-build-file"></a>Configuración del archivo de compilación

Localice `build.gradle.kts` y ábralo con el IDE o editor de texto favorito. A continuación, se copia en esta configuración de compilación:

```java
plugins {
    java
    application
}
application {
    mainClassName = "GetLanguages"
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

A continuación, en esta carpeta, cree un archivo denominado `GetLanguages.java`.

## <a name="import-required-libraries"></a>Importación de bibliotecas necesarias

Abra `GetLanguages.java` y agregue las siguientes instrucciones de importación:

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
public class GetLanguages {
  // All project code goes here...
}
```

Agregue estas líneas a la clase `GetLanguages`:

```java
String url = "https://api.cognitive.microsofttranslator.com/languages?api-version=3.0";
```

Si usa una suscripción a varios servicios de Cognitive Services, también debe incluir `Ocp-Apim-Subscription-Region` en los parámetros de la solicitud. [Más información sobre la autenticación con la suscripción a varios servicios](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication). 

## <a name="create-a-client-and-build-a-request"></a>Creación de un cliente y compilación de una solución

Agregue esta línea a la clase `GetLanguages` para crear una instancia de `OkHttpClient`:

```java
// Instantiates the OkHttpClient.
OkHttpClient client = new OkHttpClient();
```

A continuación, vamos a crear la solicitud `GET`.

```java
// This function performs a GET request.
public String Get() throws IOException {
    Request request = new Request.Builder()
            .url(url).get()
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
        GetLanguages getLanguagesRequest = new GetLanguages();
        String response = getLanguagesRequest.Get();
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

Busque la abreviatura del país o región en esta [lista de idiomas](https://docs.microsoft.com/azure/cognitive-services/translator/language-support).

Se devuelve una respuesta correcta en JSON, tal como se muestra en el siguiente ejemplo:

```json
{
  "translation": {
    "af": {
      "name": "Afrikaans",
      "nativeName": "Afrikaans",
      "dir": "ltr"
    },
    "ar": {
      "name": "Arabic",
      "nativeName": "العربية",
      "dir": "rtl"
    },
...
  },
  "transliteration": {
    "ar": {
      "name": "Arabic",
      "nativeName": "العربية",
      "scripts": [
        {
          "code": "Arab",
          "name": "Arabic",
          "nativeName": "العربية",
          "dir": "rtl",
          "toScripts": [
            {
              "code": "Latn",
              "name": "Latin",
              "nativeName": "اللاتينية",
              "dir": "ltr"
            }
          ]
        },
        {
          "code": "Latn",
          "name": "Latin",
          "nativeName": "اللاتينية",
          "dir": "ltr",
          "toScripts": [
            {
              "code": "Arab",
              "name": "Arabic",
              "nativeName": "العربية",
              "dir": "rtl"
            }
          ]
        }
      ]
    },
...
  },
  "dictionary": {
    "af": {
      "name": "Afrikaans",
      "nativeName": "Afrikaans",
      "dir": "ltr",
      "translations": [
        {
          "name": "English",
          "nativeName": "English",
          "dir": "ltr",
          "code": "en"
        }
      ]
    },
    "ar": {
      "name": "Arabic",
      "nativeName": "العربية",
      "dir": "rtl",
      "translations": [
        {
          "name": "English",
          "nativeName": "English",
          "dir": "ltr",
          "code": "en"
        }
      ]
    },
...
  }
}
```

## <a name="next-steps"></a>Pasos siguientes

Explore el código de ejemplo de esta guía de inicio rápido y otros, incluida la traducción y la transliteración, así como otros proyectos de Translator Text de ejemplo de GitHub.

> [!div class="nextstepaction"]
> [Explorar ejemplos de Java en GitHub](https://aka.ms/TranslatorGitHub?type=&language=java)

## <a name="see-also"></a>Otras referencias

* [Traducir texto](quickstart-java-translate.md)
* [Transliterar texto](quickstart-java-transliterate.md)
* [Identificar el idioma de entrada](quickstart-java-detect.md)
* [Obtener traducciones alternativas](quickstart-java-dictionary.md)
* [Determinar la longitud de las oraciones de una entrada](quickstart-java-sentences.md)
