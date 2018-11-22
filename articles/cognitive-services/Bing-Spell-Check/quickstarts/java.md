---
title: 'Guía de inicio rápido: Bing Spell Check API con Java'
titlesuffix: Azure Cognitive Services
description: Obtenga información y ejemplos de código que le ayuden a empezar a usar rápidamente Bing Spell Check API.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-spell-check
ms.topic: quickstart
ms.date: 09/14/2017
ms.author: aahi
ms.openlocfilehash: 988bf8aeea2bd3b172803dac19584b1d3019b178
ms.sourcegitcommit: ebf2f2fab4441c3065559201faf8b0a81d575743
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2018
ms.locfileid: "52161055"
---
# <a name="quickstart-for-bing-spell-check-api-with-java"></a>Guía de inicio rápido para Bing Spell Check API con Java 

En este artículo se muestra cómo usar [Bing Spell Check API](https://azure.microsoft.com/services/cognitive-services/spell-check/) con Java. Spell Check API devuelve una lista de palabras que no reconoce, junto con los reemplazos sugeridos. Normalmente, enviaría texto a esta API y, luego, realizaría los reemplazos sugeridos en el texto o los mostraría al usuario de la aplicación para que pueda decidir si realizarlos no. En este artículo se muestra cómo enviar una solicitud que contiene el texto "Hollo, wrld!". Los reemplazos sugeridos serán "Hello" y "world".

## <a name="prerequisites"></a>Requisitos previos

Necesitará [JDK 7 u 8](https://aka.ms/azure-jdks) para compilar y ejecutar este código. Puede utilizar un IDE de Java si tiene un favorito, pero bastará con un editor de texto.

Debe tener una [cuenta de la API de Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) con **Bing Spell Check API v7**. La [cuenta de evaluación gratuita](https://azure.microsoft.com/try/cognitive-services/#lang) es suficiente para esta guía de inicio rápido. Necesita la clave de acceso que se le proporciona al activar la versión de evaluación gratuita, o puede usar una clave de suscripción de pago desde su panel de Azure.

## <a name="get-spell-check-results"></a>Obtención de resultados de Spell Check

1. Cree un nuevo proyecto de Java en su IDE favorito.
2. Agregue el código que se proporciona a continuación.
3. Reemplace el valor `subscriptionKey` por una clave de acceso válida para la suscripción.
4. Ejecute el programa.

```java
import java.io.*;
import java.net.*;
import javax.net.ssl.HttpsURLConnection;

public class HelloWorld {

    static String host = "https://api.cognitive.microsoft.com";
    static String path = "/bing/v7.0/spellcheck";

    // NOTE: Replace this example key with a valid subscription key.
    static String key = "ENTER KEY HERE";

    static String mkt = "en-US";
    static String mode = "proof";
    static String text = "Hollo, wrld!";

    public static void check () throws Exception {
        String params = "?mkt=" + mkt + "&mode=" + mode;
        URL url = new URL(host + path + params);
        HttpsURLConnection connection = (HttpsURLConnection) url.openConnection();
        connection.setRequestMethod("POST");
        connection.setRequestProperty("Content-Type", "application/x-www-form-urlencoded");
        connection.setRequestProperty("Content-Length", "" + text.length() + 5);
        connection.setRequestProperty("Ocp-Apim-Subscription-Key", key);
        connection.setDoOutput(true);

        DataOutputStream wr = new DataOutputStream(connection.getOutputStream());
        wr.writeBytes("text=" + text);
        wr.flush();
        wr.close();

        BufferedReader in = new BufferedReader(
        new InputStreamReader(connection.getInputStream()));
        String line;
        while ((line = in.readLine()) != null) {
            System.out.println(line);
        }
        in.close();
    }

    public static void main(String[] args) {
        try {
            check ();
        }
        catch (Exception e) {
            System.out.println (e);
        }
    }
}
```

**Respuesta**

Se devuelve una respuesta correcta en JSON, como se muestra en el siguiente ejemplo: 

```json
{
   "_type": "SpellCheck",
   "flaggedTokens": [
      {
         "offset": 0,
         "token": "Hollo",
         "type": "UnknownToken",
         "suggestions": [
            {
               "suggestion": "Hello",
               "score": 0.9115257530801
            },
            {
               "suggestion": "Hollow",
               "score": 0.858039839213461
            },
            {
               "suggestion": "Hallo",
               "score": 0.597385084464481
            }
         ]
      },
      {
         "offset": 7,
         "token": "wrld",
         "type": "UnknownToken",
         "suggestions": [
            {
               "suggestion": "world",
               "score": 0.9115257530801
            }
         ]
      }
   ]
}
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Tutorial de Bing Spell Check](../tutorials/spellcheck.md)

## <a name="see-also"></a>Otras referencias

- [Introducción a Bing Spell Check](../proof-text.md)
- [Referencia de Bing Spell Check API v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v7-reference)
