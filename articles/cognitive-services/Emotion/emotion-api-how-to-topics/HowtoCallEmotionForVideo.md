---
title: Llamada a Emotion API para vídeo | Microsoft Docs
description: Obtenga información acerca de cómo llamar a Emotion API para vídeo en Cognitive Services.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: article
ms.date: 02/06/2017
ms.author: anroth
ms.openlocfilehash: 0875013b2061a84e3e23ae90c1106382672fdca6
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35380722"
---
# <a name="how-to-call-emotion-api-for-video"></a>Llamada a Emotion API para vídeo

> [!IMPORTANT]
> La versión preliminar de Video API terminará el 30 de octubre de 2017. Pruebe la versión preliminar de la nueva [Video Indexer API con una extracción sencilla de detalles en](https://azure.microsoft.com/services/cognitive-services/video-indexer/) vídeos y mejore las experiencias de detección de contenido, como los resultados de la búsqueda, gracias al reconocimiento de texto oral, caras, caracteres y emociones. [Más información](https://docs.microsoft.com/azure/cognitive-services/video-indexer/video-indexer-overview).

En esta guía se muestra cómo llamar a Emotion API para vídeo. Los ejemplos están escritos en C# con la biblioteca cliente de Emotion API para vídeo.

### <a name="Prep">Preparación</a> 
Para poder usar Emotion API para vídeo, necesitará un vídeo que incluya personas; de preferencia, un vídeo donde las personas miren a la cámara.

### <a name="Step1">Paso 1: Autorizar la llamada de API</a> 
Cada llamada a Emotion API para vídeo requiere una clave de suscripción. Esta clave se debe pasar a través de un parámetro de cadena de consulta o se debe especificar en el encabezado de la solicitud. Para pasar la clave de suscripción a través de una cadena de consulta, haga referencia a la dirección URL de la solicitud a continuación para Emotion API para vídeo, por ejemplo:

```
https://westus.api.cognitive.microsoft.com/emotion/v1.0/recognizeInVideo&subscription-key=<Your subscription key>
```

Como alternativa, también puede especificarse la clave de suscripción en el encabezado de la solicitud HTTP:

```
ocp-apim-subscription-key: <Your subscription key>
```

Al usar una biblioteca cliente, la clave de suscripción se pasa a través del constructor de la clase VideoServiceClient. Por ejemplo: 

```
var emotionServiceClient = new emotionServiceClient("Your subscription key");
```
Para obtener una clave de suscripción, consulte [Suscripciones] (https://azure.microsoft.com/try/cognitive-services/). 

### <a name="Step2">Paso 2: Cargar un vídeo en el servicio y comprobar el estado</a>
La manera más sencilla de realizar cualquiera de las llamadas a Emotion API para vídeo es cargar un vídeo directamente. Para esto, se envía una solicitud "POST" con el tipo de contenido application/octet-stream junto con los datos leídos de un archivo de vídeo. El tamaño máximo del vídeo es de 100 MB.

Con la biblioteca cliente, la estabilización por medio de la carga se realiza pasando un objeto de secuencia. Observe el ejemplo siguiente:

```
Operation videoOperation;
using (var fs = new FileStream(@"C:\Videos\Sample.mp4", FileMode.Open))
{
      videoOperation = await videoServiceClient.CreateOperationAsync(fs, OperationType.recognizeInVideo);
}
```

Tenga en cuenta que el método CreateOperationAsync de VideoServiceClient es asincrónico. El método que realiza la llamada debe estar marcado como asincrónico también, para poder usar la cláusula await.
Si el vídeo ya está en la web y tiene una dirección URL pública, se puede tener acceso a Emotion API para vídeo proporcionando la dirección URL. En este ejemplo, el cuerpo de la solicitud será una cadena JSON que contenga la dirección URL.

Mediante la biblioteca de cliente, la estabilización por medio de una dirección URL fácilmente se puede ejecutar con otra sobrecarga del método CreateOperationAsync.


```
var videoUrl = "http://www.example.com/sample.mp4";
Operation videoOperation = await videoServiceClient.CreateOperationAsync(videoUrl, OperationType. recognizeInVideo);

```

Este método de carga será el mismo para todas las lamadas de Emotion API para vídeo. 

Una vez que ha cargado un vídeo, la siguiente operación que querrá realizar es comprobar el estado. Dado que los archivos de vídeo suelen ser más grandes y más diversos que otros archivos, los usuarios pueden prever que el tiempo de procesamiento en este paso será prolongado. El tiempo depende del tamaño y la longitud del archivo.

Mediante la biblioteca cliente, se pueden recuperar el estado y el resultado de la operación con el método GetOperationResultAsync.


```
var operationResult = await videoServiceClient.GetOperationResultAsync(videoOperation);

```
Por lo general, el cliente debe recuperar periódicamente el estado de la operación hasta que el estado se muestre como "Correcto" o "Error".

```
OperationResult operationResult;
while (true)
{
      operationResult = await videoServiceClient.GetOperationResultAsync(videoOperation);
      if (operationResult.Status == OperationStatus.Succeeded || operationResult.Status == OperationStatus.Failed)
      {
           break;
      }

      Task.Delay(30000).Wait();
}

```

Cuando el estado de VideoOperationResult se muestra como "Correcto", el resultado se puede recuperar convirtiendo VideoOperationResult en VideoOperationInfoResult<VideoAggregateRecognitionResult> y accediendo al campo ProcessingResult.

```
var emotionRecognitionJsonString = ((VideoOperationInfoResult<VideoAggregateRecognitionResult>)operationResult).ProcessingResult;
```

### <a name="Step3">Paso 3: Recuperar y entender el reconocimiento y el seguimiento de emociones de la salida JSON</a>

El resultado de salida contiene los metadatos de las caras dentro del archivo especificado en formato JSON.

Como se explica en el paso 2, la salida JSON está disponible en el campo ProcessingResult de OperationResult, cuando su estado se muestra como "Correcto".

El archivo JSON de detección y seguimiento de caras incluye los siguientes atributos:

Atributo | DESCRIPCIÓN
-------------|-------------
Versión | Hace referencia a la versión de Emotion API para vídeo para JSON.
Escala de tiempo | "Tics" por segundo del vídeo.
Offset  |La diferencia horaria de las marcas de tiempo. En la versión 1.0 de Emotion API para vídeos, será siempre 0. En los escenarios admitidos en el futuro, este valor puede cambiar.
Framerate | Fotogramas por segundo del vídeo.
Fragments   | Los metadatos se separan en diferentes segmentos más pequeños, denominados fragmentos. Cada fragmento contiene un inicio, una duración, un número de intervalo y eventos.
Iniciar   | La hora de inicio del primer evento, en tics.
Duration |  La longitud del fragmento, en tics.
Intervalo |  La duración de cada evento dentro del fragmento, en tics.
Eventos  | Una matriz de eventos. La matriz externa representa un intervalo de tiempo. La matriz interna consta de 0 o más eventos que ocurrieron en ese momento en el tiempo.
windowFaceDistribution |    Porcentaje de caras que tienen una determinada emociones durante el evento.
windowMeanScores |  Puntuaciones medias para cada emoción de las caras en la imagen.

El motivo para dar formato al código JSON de esta manera es configurar las API para los escenarios futuros, donde será importante recuperar metadatos rápidamente y administrar una gran secuencia de resultados. Este formato utiliza tanto las técnicas de fragmentación (lo que permite dividir los metadatos en partes basadas en el tiempo, donde puede descargar solo lo que necesita) y segmentación (lo que permite dividir los eventos si se vuelven demasiado grandes). Algunos cálculos sencillos pueden ayudarle a transformar los datos. Por ejemplo, si un evento se inició en 6300 (tics), con una escala de tiempo de 2997 (tics/segundo) y una velocidad de fotogramas de 29,97 (fotogramas/segundo), entonces:

*   Inicio/Escala de tiempo = 2,1 segundos
*   Segundos x (Velocidad de fotogramas/Escala de tiempo) = 63 fotogramas

A continuación se muestra un ejemplo sencillo de extracción de JSON en un formato por fotograma para detección y seguimiento de caras:

```
var emotionRecognitionTrackingResultJsonString = operationResult.ProcessingResult;
var emotionRecognitionTracking = JsonConvert.DeserializeObject<EmotionRecognitionResult>(emotionRecognitionTrackingResultJsonString, settings);
```
Dado que las emociones se suavizan con el tiempo, si alguna vez compila una visualización para superponer los resultados sobre el vídeo original, reste 250 milisegundos de las marcas de tiempo proporcionadas.

### <a name="Summary">Resumen</a>
En esta guía ha aprendido sobre las funcionalidades de Emotion API para vídeo: cómo puede cargar un vídeo, comprobar su estado, recuperar los metadatos de reconocimiento de emociones.

Para obtener más información acerca de los detalles de la API, consulte la guía de referencia de la API "[Referencia de Emotion API para vídeo](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/56f8d40e1984551ec0a0984e)".
