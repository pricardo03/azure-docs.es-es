---
title: Obtención de intención con la llamada a REST en Java
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 10/17/2019
ms.author: diberry
ms.openlocfilehash: afa2dc950efe4c03b41afbd6090d9bf29ac5a798
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73499681"
---
## <a name="prerequisites"></a>Requisitos previos

* [JDK SE](https://aka.ms/azure-jdks) (Kit de desarrollo de Java, Standard Edition)
* [Visual Studio Code](https://code.visualstudio.com/) o su IDE favorito.
* Identificador de la aplicación pública: df67dcdb-c37d-46af-88e1-8b97951ca1c2

## <a name="get-luis-key"></a>Obtención de la clave de LUIS

[!INCLUDE [Use authoring key for endpoint](../includes/get-key-quickstart.md)]

## <a name="get-intent-programmatically"></a>Obtención de la intención mediante programación

Use Java para consultar la [API](https://aka.ms/luis-apim-v3-prediction) GET del punto de conexión de predicción y obtener el resultado de la predicción.

1. Cree un subdirectorio llamado `lib` y copie en él las siguientes bibliotecas de Java:

    * [commons-logging-1.2.jar](https://raw.githubusercontent.com/Azure-Samples/cognitive-services-language-understanding/master/documentation-samples/quickstarts/analyze-text/java/lib/commons-logging-1.2.jar)
    * [httpclient-4.5.3.jar](https://raw.githubusercontent.com/Azure-Samples/cognitive-services-language-understanding/master/documentation-samples/quickstarts/analyze-text/java/lib/httpclient-4.5.3.jar)
    * [httpcore-4.4.6.jar](https://raw.githubusercontent.com/Azure-Samples/cognitive-services-language-understanding/master/documentation-samples/quickstarts/analyze-text/java/lib/httpcore-4.4.6.jar)

1. Copie el código siguiente para crear una clase en un archivo llamado `Predict.java`:


    ```java
    import java.io.*;
    import java.net.URI;
    import org.apache.http.HttpEntity;
    import org.apache.http.HttpResponse;
    import org.apache.http.client.HttpClient;
    import org.apache.http.client.methods.HttpGet;
    import org.apache.http.client.utils.URIBuilder;
    import org.apache.http.impl.client.HttpClients;
    import org.apache.http.util.EntityUtils;
    
    public class Predict {
    
        public static void main(String[] args) 
        {
            HttpClient httpclient = HttpClients.createDefault();
    
            try
            {
    
                // The ID of a public sample LUIS app that recognizes intents for turning on and off lights
                String AppId = "df67dcdb-c37d-46af-88e1-8b97951ca1c2";
                
                // Add your endpoint key 
                String Key = "YOUR-KEY";
    
                // Add your endpoint, example is westus.api.cognitive.microsoft.com
                String Endpoint = "YOUR-ENDPOINT";
    
                String Utterance = "turn on all lights";
    
                // Begin endpoint URL string building
                URIBuilder endpointURLbuilder = new URIBuilder("https://" + Endpoint + "/luis/prediction/v3.0/apps/" + AppId + "/slots/production/predict?");
    
                // query string params
                endpointURLbuilder.setParameter("query", Utterance);
                endpointURLbuilder.setParameter("subscription-key", Key);
                endpointURLbuilder.setParameter("show-all-intents", "true");
                endpointURLbuilder.setParameter("verbose", "true");
    
                // create URL from string
                URI endpointURL = endpointURLbuilder.build();
    
                // create HTTP object from URL
                HttpGet request = new HttpGet(endpointURL);
    
                // access LUIS endpoint - analyze text
                HttpResponse response = httpclient.execute(request);
    
                // get response
                HttpEntity entity = response.getEntity();
    
    
                if (entity != null) 
                {
                    System.out.println(EntityUtils.toString(entity));
                }
            }
    
            catch (Exception e)
            {
                System.out.println(e.getMessage());
            }
        }   
    }    
    ```

1. Reemplace los valores siguientes:

    * `YOUR-KEY` por la clave de inicio.
    * `YOUR-ENDPOINT` por el punto de conexión, por ejemplo, `westus2.api.cognitive.microsoft.com`.


1. Compile el programa de Java desde una línea de comandos: 

    ```console
    javac -cp ":lib/*" Predict.java
    ```

1. Ejecute el programa de Java desde la línea de comandos:

    ```console
    java -cp ":lib/*" Predict
    ```

1. Revise la respuesta de predicción en formato JSON:

    ```console
    {'query': 'turn on all lights', 'prediction': {'topIntent': 'HomeAutomation.TurnOn', 'intents': {'HomeAutomation.TurnOn': {'score': 0.5375382}, 'None': {'score': 0.08687421}, 'HomeAutomation.TurnOff': {'score': 0.0207554}}, 'entities': {'HomeAutomation.Operation': ['on'], '$instance': {'HomeAutomation.Operation': [{'type': 'HomeAutomation.Operation', 'text': 'on', 'startIndex': 5, 'length': 2, 'score': 0.724984169, 'modelTypeId': -1, 'modelType': 'Unknown', 'recognitionSources': ['model']}]}}}}
    ```

    Respuesta JSON con formato para mejorar la legibilidad: 

    ```JSON
    {
        "query": "turn on all lights",
        "prediction": {
            "topIntent": "HomeAutomation.TurnOn",
            "intents": {
                "HomeAutomation.TurnOn": {
                    "score": 0.5375382
                },
                "None": {
                    "score": 0.08687421
                },
                "HomeAutomation.TurnOff": {
                    "score": 0.0207554
                }
            },
            "entities": {
                "HomeAutomation.Operation": [
                    "on"
                ],
                "$instance": {
                    "HomeAutomation.Operation": [
                        {
                            "type": "HomeAutomation.Operation",
                            "text": "on",
                            "startIndex": 5,
                            "length": 2,
                            "score": 0.724984169,
                            "modelTypeId": -1,
                            "modelType": "Unknown",
                            "recognitionSources": [
                                "model"
                            ]
                        }
                    ]
                }
            }
        }
    }
    ```

## <a name="luis-keys"></a>Claves de LUIS

[!INCLUDE [Use authoring key for endpoint](../includes/starter-key-explanation.md)]

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando haya terminado con este inicio rápido, elimine el archivo del sistema de archivos. 

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Incorporación de expresiones y entrenamiento con Java](../luis-get-started-java-add-utterance.md)