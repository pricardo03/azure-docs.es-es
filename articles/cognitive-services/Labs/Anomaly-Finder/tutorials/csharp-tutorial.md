---
title: 'Detección de anomalías de aplicaciones en C#: Microsoft Cognitive Services | Microsoft Docs'
description: Explore una aplicación en C# que usa Anomaly Detection API en Microsoft Cognitive Services. Envíe puntos de datos originales a la API y obtenga el valor esperado y los puntos de anomalías.
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.technology: anomaly-detection
ms.topic: article
ms.date: 05/01/2018
ms.author: chliang
ms.openlocfilehash: 2e4100fd7d8e85a6b103c31000176aaaeb3d7151
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2018
ms.locfileid: "38317686"
---
# <a name="anomaly-detection-c-application"></a>Detección de anomalías de aplicaciones en C#

Explore una aplicación básica de Windows que utiliza Anomaly Detection API para detectar anomalías a partir de la entrada. El ejemplo envía los datos de serie temporal a Anomaly Detection API con su clave de suscripción y, después, obtiene todos los puntos de anomalías y el valor esperado de cada punto de datos a partir de la API.

## <a name="prerequisites"></a>requisitos previos

### <a name="platform-requirements"></a>Requisitos de la plataforma

El ejemplo se ha desarrollado para .NET Framework mediante [Visual Studio 2017, Community Edition](https://www.visualstudio.com/products/visual-studio-community-vs). 

### <a name="subscribe-to-anomaly-detection-and-get-a-subscription-key"></a>Suscripción a Anomaly Detection y cómo obtener una clave de suscripción 

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]

## <a name="get-and-use-the-example"></a>Obtención y uso del ejemplo

Puede clonar la aplicación de ejemplo de Anomaly Detection en el equipo desde [GitHub](https://github.com/MicrosoftAnomalyDetection/csharp-sample.git). 
<a name="Step1"></a>
### <a name="install-the-example"></a>Instalación del ejemplo

En el escritorio de GitHub, abra Sample\AnomalyDetectionSample.sln.

<a name="Step2"></a>
### <a name="build-the-example"></a>Compilación del ejemplo

Presione Ctrl + Mayús + B, o haga clic en Compilar en el menú de la cinta de opciones y, a continuación, seleccione Compilar solución.

<a name="Step3"></a>
### <a name="run-the-example"></a>Ejecutar el ejemplo

1. Una vez completada la compilación, presione **F5** o haga clic en **Iniciar** en el menú de la cinta de opciones para ejecutar el ejemplo.
2. Busque la ventana de interfaz de usuario de Anomaly Detection en cuyo cuadro de edición de texto se lea "{your_subscription_key}".
3. Reemplace el archivo request.json, que contiene los datos de ejemplo, con sus propios datos; a continuación, haga clic en botón "Enviar". Microsoft recibe los datos cargados y los usa para detectar los puntos de anomalías entre ellos. Los datos actualizados no se conservarán en el servidor de Microsoft. Para volver a detectar el punto de anomalías, necesita volver a cargar los datos.
4. Si los datos están correctos, encontrará el resultado de la detección de anomalías en el campo "Respuesta". Si se produce algún error, su información también se mostrará en el campo Respuesta.

<a name="Review"></a>
### <a name="read-the-result"></a>Lectura del resultado

[!INCLUDE [diagrams](../includes/diagrams.md)]

<a name="Review"></a>
### <a name="review-and-learn"></a>Revisión y comprensión

Ahora que tiene una aplicación en ejecución, vamos a revisar cómo se integra esta aplicación de ejemplo con la tecnología Cognitive Services. Este paso permitirá que sea más fácil seguir compilando sobre esta aplicación o desarrollar su propia aplicación mediante Microsoft Anomaly Detection.

Esta aplicación de ejemplo usa el punto de conexión de la API de RESTful Anomaly Detection.

Cuando revise cómo se usa la API de RESTful en la aplicación de ejemplo, observe el fragmento de código de **AnomalyDetectionClient.cs**. El archivo contiene comentarios de código que indican "KEY SAMPLE CODE STARTS HERE" (EL CÓDIGO DE EJEMPLO PRINCIPAL COMIENZA AQUÍ) y "KEY SAMPLE CODE ENDS HERE" (EL CÓDIGO DE EJEMPLO PRINCIPAL FINALIZA AQUÍ) para ayudarle a encontrar los fragmentos de código reproducidos a continuación.

```csharp
            // ----------------------------------------------------------------------
            // KEY SAMPLE CODE STARTS HERE
            // Set http request header
            // ---------------------------------------------------------------------- 
            client.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
            client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", subscriptionKey);
            // ----------------------------------------------------------------------
            // KEY SAMPLE CODE ENDS HERE 
            // ----------------------------------------------------------------------

```
**Request(...)** Debajo del fragmento de código se muestra cómo usar HttlClient para enviar la clave de suscripción y los puntos de datos al punto de conexión de Anomaly Detection API.

```csharp
    public async Task<string> Request(string baseAddress, string endpoint, string subscriptionKey, string requestData)
    {
        using (HttpClient client = new HttpClient { BaseAddress = new Uri(baseAddress) })
        {
            // ----------------------------------------------------------------------
            // KEY SAMPLE CODE STARTS HERE
            // Set http request header
            // ---------------------------------------------------------------------- 
            client.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
            client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", subscriptionKey);
            // ----------------------------------------------------------------------
            // KEY SAMPLE CODE ENDS HERE 
            // ----------------------------------------------------------------------

            // ----------------------------------------------------------------------
            // KEY SAMPLE CODE STARTS HERE
            // Build the request content
            // ---------------------------------------------------------------------- 
            var content = new StringContent(requestData, Encoding.UTF8, "application/json");
            // ----------------------------------------------------------------------
            // KEY SAMPLE CODE ENDS HERE 
            // ----------------------------------------------------------------------

            // ----------------------------------------------------------------------
            // KEY SAMPLE CODE STARTS HERE
            // Send the request content with POST method.
            // ---------------------------------------------------------------------- 
            var res = await client.PostAsync(endpoint, content);
            // ----------------------------------------------------------------------
            // KEY SAMPLE CODE ENDS HERE 
            // ----------------------------------------------------------------------
            if (res.IsSuccessStatusCode)
            {
                return await res.Content.ReadAsStringAsync();
            }
            else
            {
                return $"ErrorCode: {res.StatusCode}";
            }
        }
    }
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Referencia de API de REST](https://dev.labs.cognitive.microsoft.com/docs/services/anomaly-detection/operations/post-anomalydetection)
