---
title: Creación de un cliente para consumir el servicio web implementado
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo consumir un servicio web generado al implementar un modelo con el modelo de Azure Machine Learning. El servicio web que expone una API REST. Cree clientes para esta API mediante el lenguaje de programación que quiera.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: aashishb
author: aashishb
ms.reviewer: larryfr
ms.date: 12/03/2018
ms.custom: seodec18
ms.openlocfilehash: 03340dc8f3be2465f20756dc9799b9c1e4293521
ms.sourcegitcommit: 79038221c1d2172c0677e25a1e479e04f470c567
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2019
ms.locfileid: "56417132"
---
# <a name="consume-an-azure-machine-learning-model-deployed-as-a-web-service"></a>Consumir un modelo de Azure Machine Learning que está implementado como un servicio web

La implementación de un modelo de Azure Machine Learning como un servicio web crea una API REST. Puede enviar datos a esta API y recibir la predicción que devuelve el modelo. En este documento aprenderá a crear clientes para el servicio web usando C #, Go, Java y Python.

Creará un servicio web cuando se implementa una imagen en una instancia de Azure Container, Azure Kubernetes Service o Project Brainwave (matrices de puertas programables). Creará imágenes a partir de modelos registrados y archivos de puntuación. Recuperará el URI que se usa para obtener acceso a un servicio web gracias al [SDK de Azure Machine Learning](https://aka.ms/aml-sdk). Si la autenticación está habilitada, también puede usar el SDK para obtener las claves de autenticación.

El flujo de trabajo general al crear un cliente que usa un servicio web de Machine Learning es:

1. Usar el SDK para obtener la información de conexión.
1. Determinar el tipo de datos de solicitud que usa el modelo.
1. Crear una aplicación que llame al servicio web.

## <a name="connection-information"></a>Información sobre la conexión

> [!NOTE]
> Use el SDK de Azure Machine Learning para obtener la información del servicio web. Este es un SDK de Python. Puede usar cualquier lenguaje para crear un cliente para el servicio.

La clase [azureml.core.Webservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py) proporciona la información necesaria para crear un cliente. Las siguientes propiedades `Webservice` son útiles para crear una aplicación cliente:

* `auth_enabled`: si la autenticación está habilitada, `True`; de lo contrario, `False`.
* `scoring_uri`: dirección de la API REST.

Existen tres formas de recuperar esta información para los servicios web implementados:

* Cuando implementa un modelo, se devuelve un objeto `Webservice` con información sobre el servicio:

    ```python
    service = Webservice.deploy_from_model(name='myservice',
                                           deployment_config=myconfig,
                                           models=[model],
                                           image_config=image_config,
                                           workspace=ws)
    print(service.scoring_uri)
    ```

* Puede usar `Webservice.list` para recuperar una lista de servicios web implementados para modelos en su área de trabajo. Puede agregar filtros para limitar la lista de información devuelta. Para obtener más información sobre lo que se puede filtrar, consulte la documentación de referencia de [Webservice.list](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.webservice.webservice?view=azure-ml-py).

    ```python
    services = Webservice.list(ws)
    print(services[0].scoring_uri)
    ```

* Si conoce el nombre del servicio implementado, puede crear una nueva instancia de `Webservice` y proporcionar el espacio de trabajo y el nombre del servicio como parámetros. El nuevo objeto contiene información sobre el servicio implementado.

    ```python
    service = Webservice(workspace=ws, name='myservice')
    print(service.scoring_uri)
    ```

### <a name="authentication-key"></a>Clave de autenticación

Al habilitar la autenticación para una implementación, se crean automáticamente las claves de autenticación.

* La autenticación se habilita de manera predeterminada cuando se implementa en Azure Kubernetes Service.
* La autenticación se deshabilita de manera predeterminada cuando se implementa en instancias de Azure Container.

Para controlar la autenticación, use el parámetro `auth_enabled` cuando cree o actualice una implementación.

Si la autenticación está habilitada, puede usar el método `get_keys` para recuperar una clave de autenticación primaria y secundaria:

```python
primary, secondary = service.get_keys()
print(primary)
```

> [!IMPORTANT]
> Si necesita regenerar una clave, use [`service.regen_key`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py).

## <a name="request-data"></a>Datos de la solicitud

La API REST espera que el cuerpo de la solicitud sea un documento JSON con la siguiente estructura:

```json
{
    "data":
        [
            <model-specific-data-structure>
        ]
}
```

> [!IMPORTANT]
> La estructura de los datos debe coincidir con lo que esperan el script y el modelo de puntuación en el servicio. El script de puntuación podría modificar los datos antes de pasarlos al modelo.

Por ejemplo, el modelo en el ejemplo [Train within notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-within-notebook/train-within-notebook.ipynb) espera una matriz de 10 números. El script de puntuación de este ejemplo crea una matriz Numpy a partir de la solicitud y la pasa al modelo. En el siguiente ejemplo se muestran los datos que este servicio espera:

```json
{
    "data": 
        [
            [
                0.0199132141783263, 
                0.0506801187398187, 
                0.104808689473925, 
                0.0700725447072635, 
                -0.0359677812752396, 
                -0.0266789028311707, 
                -0.0249926566315915, 
                -0.00259226199818282, 
                0.00371173823343597, 
                0.0403433716478807
            ]
        ]
}
``` 

El servicio web puede aceptar varios conjuntos de datos en una solicitud. Devuelve un documento JSON que contiene una matriz de respuestas.

### <a name="binary-data"></a>Datos binarios

Si el modelo acepta datos binarios, como una imagen, debe modificar el archivo `score.py` usado para la implementación para aceptar solicitudes HTTP sin procesar. Este es un ejemplo de un archivo `score.py` que acepta datos binarios y devuelve los bytes invertidos para las solicitudes POST. Para las solicitudes GET, devuelve la dirección URL completa en el cuerpo de la respuesta:

```python 
from azureml.contrib.services.aml_request  import AMLRequest, rawhttp
from azureml.contrib.services.aml_response import AMLResponse

def init():
    print("This is init()")

@rawhttp
def run(request):
    print("This is run()")
    print("Request: [{0}]".format(request))
    if request.method == 'GET':
        respBody = str.encode(request.full_path)
        return AMLResponse(respBody, 200)
    elif request.method == 'POST':
        reqBody = request.get_data(False)
        respBody = bytearray(reqBody)
        respBody.reverse()
        respBody = bytes(respBody)
        return AMLResponse(respBody, 200)
    else:
        return AMLResponse("bad request", 500)
```

> [!IMPORTANT]
> El espacio de nombres `azureml.contrib` cambia con frecuencia mientras trabajamos para mejorar el servicio. Por lo tanto, todo el contenido de este espacio de nombres debe considerarse como una versión preliminar, no completamente compatible con Microsoft.
>
> Si necesita probar esto en su entorno de desarrollo local, puede instalar los componentes en el espacio de nombres `contrib`mediante el comando siguiente:
> 
> ```shell
> pip install azureml-contrib-services
> ```

## <a name="call-the-service-c"></a>Llamada al servicio (C#)

En este ejemplo se muestra cómo usar C# para llamar al servicio web creado a partir del ejemplo [Train within notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-within-notebook/train-within-notebook.ipynb):

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Net.Http;
using System.Net.Http.Headers;
using Newtonsoft.Json;

namespace MLWebServiceClient
{
    // The data structure expected by the service
    internal class InputData
    {
        [JsonProperty("data")]
        // The service used by this example expects an array containing
        //   one or more arrays of doubles
        internal double[,] data;
    }
    class Program
    {
        static void Main(string[] args)
        {
            // Set the scoring URI and authentication key
            string scoringUri = "<your web service URI>";
            string authKey = "<your key>";

            // Set the data to be sent to the service.
            // In this case, we are sending two sets of data to be scored.
            InputData payload = new InputData();
            payload.data = new double[,] {
                {
                    0.0199132141783263,
                    0.0506801187398187,
                    0.104808689473925,
                    0.0700725447072635,
                    -0.0359677812752396,
                    -0.0266789028311707,
                    -0.0249926566315915,
                    -0.00259226199818282,
                    0.00371173823343597,
                    0.0403433716478807
                },
                {
                    -0.0127796318808497, 
                    -0.044641636506989, 
                    0.0606183944448076, 
                    0.0528581912385822, 
                    0.0479653430750293, 
                    0.0293746718291555, 
                    -0.0176293810234174, 
                    0.0343088588777263, 
                    0.0702112981933102, 
                    0.00720651632920303
                }
            };

            // Create the HTTP client
            HttpClient client = new HttpClient();
            // Set the auth header. Only needed if the web service requires authentication.
            client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", authKey);

            // Make the request
            try {
                var request = new HttpRequestMessage(HttpMethod.Post, new Uri(scoringUri));
                request.Content = new StringContent(JsonConvert.SerializeObject(payload));
                request.Content.Headers.ContentType = new MediaTypeHeaderValue("application/json");
                var response = client.SendAsync(request).Result;
                // Display the response from the web service
                Console.WriteLine(response.Content.ReadAsStringAsync().Result);
            }
            catch (Exception e)
            {
                Console.Out.WriteLine(e.Message);
            }
        }
    }
}
```

Los resultados devueltos son similares al siguiente documento JSON:

```json
[217.67978776218715, 224.78937091757172]
```

## <a name="call-the-service-go"></a>Llamada al servicio (Go)

En este ejemplo se muestra cómo usar Go para llamar al servicio web creado a partir del ejemplo [Train within notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-within-notebook/train-within-notebook.ipynb):

```go
package main

import (
    "bytes"
    "encoding/json"
    "fmt"
    "io/ioutil"
    "net/http"
)

// Features for this model are an array of decimal values
type Features []float64

// The web service input can accept multiple sets of values for scoring
type InputData struct {
    Data []Features `json:"data",omitempty`
}

// Define some example data
var exampleData = []Features{
    []float64{
        0.0199132141783263, 
        0.0506801187398187, 
        0.104808689473925, 
        0.0700725447072635, 
        -0.0359677812752396, 
        -0.0266789028311707, 
        -0.0249926566315915, 
        -0.00259226199818282, 
        0.00371173823343597, 
        0.0403433716478807,
    },
    []float64{
        -0.0127796318808497, 
        -0.044641636506989, 
        0.0606183944448076, 
        0.0528581912385822, 
        0.0479653430750293, 
        0.0293746718291555, 
        -0.0176293810234174, 
        0.0343088588777263, 
        0.0702112981933102, 
        0.00720651632920303,
    },
}

// Set to the URI for your service
var serviceUri string = "<your web service URI>"
// Set to the authentication key (if any) for your service
var authKey string = "<your key>"

func main() {
    // Create the input data from example data
    jsonData := InputData{
        Data: exampleData,
    }
    // Create JSON from it and create the body for the HTTP request
    jsonValue, _ := json.Marshal(jsonData)
    body := bytes.NewBuffer(jsonValue)

    // Create the HTTP request
    client := &http.Client{}
    request, err := http.NewRequest("POST", serviceUri, body)
    request.Header.Add("Content-Type", "application/json")

    // These next two are only needed if using an authentication key
    bearer := fmt.Sprintf("Bearer %v", authKey)
    request.Header.Add("Authorization", bearer)

    // Send the request to the web service
    resp, err := client.Do(request)
    if err != nil {
        fmt.Println("Failure: ", err)
    }

    // Display the response received
    respBody, _ := ioutil.ReadAll(resp.Body)
    fmt.Println(string(respBody))
}
```

Los resultados devueltos son similares al siguiente documento JSON:

```json
[217.67978776218715, 224.78937091757172]
```

## <a name="call-the-service-java"></a>Llamada al servicio (Java)

En este ejemplo se muestra cómo usar Java para llamar al servicio web creado a partir del ejemplo [Train within notebook ](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-within-notebook/train-within-notebook.ipynb):

```java
import java.io.IOException;
import org.apache.http.client.fluent.*;
import org.apache.http.entity.ContentType;
import org.json.simple.JSONArray;
import org.json.simple.JSONObject;

public class App {
    // Handle making the request
    public static void sendRequest(String data) {
        // Replace with the scoring_uri of your service
        String uri = "<your web service URI>";
        // If using authentication, replace with the auth key
        String key = "<your key>";
        try {
            // Create the request
            Content content = Request.Post(uri)
            .addHeader("Content-Type", "application/json")
            // Only needed if using authentication
            .addHeader("Authorization", "Bearer " + key)
            // Set the JSON data as the body
            .bodyString(data, ContentType.APPLICATION_JSON)
            // Make the request and display the response.
            .execute().returnContent();
            System.out.println(content);
        }
        catch (IOException e) {
            System.out.println(e);
        }
    }
    public static void main(String[] args) {
        // Create the data to send to the service
        JSONObject obj = new JSONObject();
        // In this case, it's an array of arrays
        JSONArray dataItems = new JSONArray();
        // Inner array has 10 elements
        JSONArray item1 = new JSONArray();
        item1.add(0.0199132141783263);
        item1.add(0.0506801187398187);
        item1.add(0.104808689473925);
        item1.add(0.0700725447072635);
        item1.add(-0.0359677812752396);
        item1.add(-0.0266789028311707);
        item1.add(-0.0249926566315915);
        item1.add(-0.00259226199818282);
        item1.add(0.00371173823343597);
        item1.add(0.0403433716478807);
        // Add the first set of data to be scored
        dataItems.add(item1);
        // Create and add the second set
        JSONArray item2 = new JSONArray();
        item2.add(-0.0127796318808497);
        item2.add(-0.044641636506989);
        item2.add(0.0606183944448076);
        item2.add(0.0528581912385822);
        item2.add(0.0479653430750293);
        item2.add(0.0293746718291555);
        item2.add(-0.0176293810234174);
        item2.add(0.0343088588777263);
        item2.add(0.0702112981933102);
        item2.add(0.00720651632920303);
        dataItems.add(item2);
        obj.put("data", dataItems);

        // Make the request using the JSON document string
        sendRequest(obj.toJSONString());
    }
}
```

Los resultados devueltos son similares al siguiente documento JSON:

```json
[217.67978776218715, 224.78937091757172]
```

## <a name="call-the-service-python"></a>Llamada al servicio (Python)

En este ejemplo muestra cómo utilizar Python para llamar al servicio web creado a partir del ejemplo [Train within notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-within-notebook/train-within-notebook.ipynb):

```python
import requests
import json

# URL for the web service
scoring_uri = '<your web service URI>'
# If the service is authenticated, set the key
key = '<your key>'

# Two sets of data to score, so we get two results back
data = {"data": 
            [
                [
                    0.0199132141783263, 
                    0.0506801187398187, 
                    0.104808689473925, 
                    0.0700725447072635, 
                    -0.0359677812752396, 
                    -0.0266789028311707, 
                    -0.0249926566315915, 
                    -0.00259226199818282, 
                    0.00371173823343597, 
                    0.0403433716478807
                ],
                [
                    -0.0127796318808497, 
                    -0.044641636506989, 
                    0.0606183944448076, 
                    0.0528581912385822, 
                    0.0479653430750293, 
                    0.0293746718291555, 
                    -0.0176293810234174, 
                    0.0343088588777263, 
                    0.0702112981933102, 
                    0.00720651632920303]
            ]
        }
# Convert to JSON string
input_data = json.dumps(data)

# Set the content type
headers = { 'Content-Type':'application/json' }
# If authentication is enabled, set the authorization header
headers['Authorization']=f'Bearer {key}'

# Make the request and display the response
resp = requests.post(scoring_uri, input_data, headers = headers)
print(resp.text)

```

Los resultados devueltos son similares al siguiente documento JSON:

```JSON
[217.67978776218715, 224.78937091757172]
```
