---
title: 'Tutorial: Detección de anomalías con JavaScript'
titlesuffix: Azure Cognitive Services
description: Explore una aplicación web Javascript que usa Anomaly Detection API. Envíe puntos de datos originales a la API y obtenga el valor esperado y los puntos de anomalías.
services: cognitive-services
author: wenya
manager: bix
ms.service: cognitive-services
ms.component: anomaly-detection
ms.topic: tutorial
ms.date: 05/01/2018
ms.author: wenya
ms.openlocfilehash: cd65a5275fac651968197c9000b2d4cd3962693b
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2018
ms.locfileid: "48883710"
---
# <a name="tutorial-anomaly-detection-with-javascript-application"></a>Tutorial: Detección de anomalías con una aplicación JavaScript

[!INCLUDE [PrivatePreviewNote](../../../../../includes/cognitive-services-anomaly-finder-private-preview-note.md)]

Explore una aplicación web que usa Anomaly Detection REST API para detectar anomalías. El ejemplo envía los datos de serie temporal a Anomaly Detection API con su clave de suscripción y, después, obtiene de la API todos los puntos de anomalías y el valor esperado de cada punto de datos.

## <a name="prerequisites"></a>Requisitos previos

### <a name="platform-requirements"></a>Requisitos de la plataforma

Este tutorial se ha desarrollado con un editor de texto simple.

### <a name="subscribe-to-anomaly-detection-and-get-a-subscription-key"></a>Suscripción a Anomaly Detection y obtención de una clave de suscripción 

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]

## <a name="get-and-use-the-example"></a>Obtención y uso del ejemplo

Este tutorial proporciona dos escenarios para la detección de anomalías de datos de serie temporal. Comencemos.

<a name="Step1"></a> 
### <a name="download-the-tutorial-project"></a>Descarga del proyecto del tutorial

Clone el [tutorial de Anomaly Detection de Cognitive Services para JavaScript](https://github.com/MicrosoftAnomalyDetection/javascript-sample) o descargue el archivo .zip y extráigalo en un directorio vacío.

<a name="Step2"></a>
### <a name="run-the-example"></a>Ejecutar el ejemplo

Hay dos escenarios para probar el ejemplo.
1. Coloque la **clave de suscripción** en el campo Subscription Key (Clave de suscripción) en la función detect, en anomalydetection.html.
2. Coloque el punto de conexión de Anomaly Detection API y compruebe que está usando la región correcta en Subscription Region (Región de suscripción).
3. Abra el archivo **anomalydetection.html** en un explorador web.

**Escenario 1: Detección de datos de serie temporal semanales**
1. En el campo Period (Período), especifique el período **7**. 
2. Reemplace los datos de ejemplo por sus puntos de datos de serie temporal semanales (JSON) en el campo Points (Puntos) o utilice directamente los datos de ejemplo.
3. Haga clic en el botón Anomaly Detection y compruebe el resultado en el cuadro de texto Response (Respuesta) de la derecha.

**Escenario 2: Detección de datos de serie temporal sin un período**
1. Deje el período vacío en el campo Period (Período) si no conoce el período de la serie temporal.
2. Utilice los mismos datos de serie temporal que en el escenario 1.
3. Haga clic en el botón Anomaly Detection y compruebe el periodo en el cuadro de texto Response (Respuesta) de la derecha.

<a name="Step3"></a>
### <a name="read-the-result"></a>Lectura del resultado

[!INCLUDE [diagrams](../includes/diagrams.md)]

<a name="Review"></a>
### <a name="review-and-learn"></a>Revisión y comprensión

Ahora obtendrá una aplicación en ejecución. Vamos a revisar cómo se integra la aplicación de ejemplo con la tecnología de Cognitive Services. Esta integración permite que siga creando fácilmente esta aplicación o que cree su propia aplicación con Microsoft Anomaly Detection.
Esta aplicación de ejemplo utiliza el punto de conexión de Anomaly Detection REST API.
Vamos a echar un vistazo a un fragmento de código de anomalydetection.html para ver cómo se usa la API REST en la aplicación de ejemplo.
```JavaScript
function anomalyDetection(url, subscriptionKey, points, period) {
    var obj = new Object();
    obj.Points = JSON.parse(points); // this points are read from text box.
    obj.Period = parseInt(period);//period=7 weekly period
    var tsData = JSON.stringify(obj);
    // Perform the REST API call.
    $.ajax({
        url: url, //Anomaly Detection API endpoint
        // Request headers.
        beforeSend: function (xhrObj) {
            xhrObj.setRequestHeader("Content-Type", "application/json");
            xhrObj.setRequestHeader("Ocp-Apim-Subscription-Key", subscriptionKey); // Replace your subscription key
        },
        type: "POST",
        // Request body.
        data: tsData, // json format
        })
        .done(function (data) {
            // Show formatted JSON on webpage.
            $("#responseTextArea").val(JSON.stringify(data, null, 2));
        })
        .fail(function (jqXHR, textStatus, errorThrown) {
            // Display error message.
            var errorString = (errorThrown === "") ? "Error. " : errorThrown + " (" + jqXHR.status + "): ";
            errorString += (jqXHR.responseText === "") ? "" : jQuery.parseJSON(jqXHR.responseText).message;
            $("#responseTextArea").val(errorString);           
        });
}

```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Referencia de API de REST](https://dev.labs.cognitive.microsoft.com/docs/services/anomaly-detection/operations/post-anomalydetection)
