---
title: 'Uso de Anomaly Finder API con C#: Microsoft Cognitive Services | Microsoft Docs'
description: Obtendrá información y ejemplos de código que le ayudarán a empezar a usar C# y Anomaly Finder API en Cognitive Services rápidamente.
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.technology: anomaly-detection
ms.topic: article
ms.date: 05/01/2018
ms.author: chliang
ms.openlocfilehash: 867ce4d0262c94de8da0dadeb8de71c28a8295d5
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35381314"
---
# <a name="use-the-anomaly-finder-api-with-c"></a>Uso de Anomaly Finder API con C#

En este artículo se ofrece información y ejemplos de código para ayudarle a comenzar a usar rápidamente Anomaly Finder API con C# para realizar la tarea de obtener resultados de anomalías en los datos de serie temporal.

## <a name="prerequisites"></a>requisitos previos

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]

## <a name="getting-anomaly-points-with-anomaly-finder-api-using-c"></a>Obtener puntos de anomalías con Anomaly Finder API mediante C#

[!INCLUDE [DataContract](../includes/datacontract.md)]

### <a name="example-of-time-series-data-points"></a>Ejemplo de puntos de datos de series temporales

El ejemplo de los puntos de datos de serie temporal es el siguiente.
[!INCLUDE [Request](../includes/request.md)]

### <a name="analyze-data-and-get-anomaly-points-c-example"></a>Ejemplo de análisis de datos y obtención de puntos de anomalías en C#

Los pasos necesarios para usar el ejemplo son los siguientes.

1. Cree una solución de consola en Visual Studio.
2. Reemplace Program.cs con el código siguiente y agregue la referencia a System.Net.Http.
3. Reemplace el valor de `[YOUR_SUBSCRIPTION_KEY]` por su clave de suscripción válida.
4. Reemplace `[REPLACE_WITH_THE_EXAMPLE_OR_YOUR_OWN_DATA_POINTS]` con los puntos de datos.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Text;
using System.Text.RegularExpressions;
using System.Threading.Tasks;

namespace Console
{
    class Program
    {
        // **********************************************
        // *** Update or verify the following values. ***
        // **********************************************

        // Replace the subscriptionKey string value with your valid subscription key.
        const string subscriptionKey = "[YOUR_SUBSCRIPTION_KEY]";

        const string endpoint = "https://api.labs.cognitive.microsoft.com/anomalyfinder/v1.0/anomalydetection";

        // Replace the request data with your real data.
        const string requestData = "[REPLACE_WITH_THE_EXAMPLE_OR_YOUR_OWN_DATA_POINTS]";
        static void Main(string[] args)
        {
            var match = Regex.Match(endpoint, "(?<BaseAddress>https://[^/]+)(?<Url>/*.+)");
            if (match.Success)
            {
                var res = Request(
                    match.Groups["BaseAddress"].Value,
                    match.Groups["Url"].Value,
                    subscriptionKey,
                    requestData).Result;
                System.Console.Write(res);
            }
            else
            {
                System.Console.Write("Incorrect endpoint.");
            }
        }

        /// <summary>
        /// Call API to detect the anomaly points
        /// </summary>
        /// <param name="baseAddress">Base address of the API endpoint.</param>
        /// <param name="endpoint">The endpoint of the API</param>
        /// <param name="subscriptionKey">The subscription key applied  </param>
        /// <param name="requestData">The JSON string for requet data points</param>
        /// <returns>The JSON string for anomaly points and expected values.</returns>
        static async Task<string> Request(string baseAddress, string endpoint, string subscriptionKey, string requestData)
        {
            using (HttpClient client = new HttpClient { BaseAddress = new Uri(baseAddress) })
            {
                client.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
                client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", subscriptionKey);

                var content = new StringContent(requestData, Encoding.UTF8, "application/json");
                var res = await client.PostAsync(endpoint, content);
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
    }
}

```

### <a name="example-response"></a>Respuesta de ejemplo

Se devuelve una respuesta correcta en JSON. A continuación se muestra la respuesta de ejemplo.
[!INCLUDE [Response](../includes/response.md)]

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Aplicación C#](../tutorials/csharp-tutorial.md)
