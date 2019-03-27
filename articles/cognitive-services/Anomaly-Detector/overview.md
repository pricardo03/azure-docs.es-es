---
title: ¿Qué es la API del Detector de anomalías? | Microsoft Docs
titleSuffix: Azure Cognitive Services
description: Use algoritmos avanzados de la API Detector de anomalías para identificar anomalías en datos de series temporales.
services: cognitive-services
author: aahill
ms.service: cognitive-services
ms.subservice: anomaly-detection
ms.topic: article
ms.date: 03/26/2019
ms.author: aahi
ms.openlocfilehash: 48dffb8853d3def590e434c1683ecf038e2a194e
ms.sourcegitcommit: fbfe56f6069cba027b749076926317b254df65e5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2019
ms.locfileid: "58473279"
---
# <a name="what-is-the-anomaly-detector-api"></a>¿Qué es la API del Detector de anomalías?

La API del Detector de anomalías permite supervisar y detectar anomalías en datos de series temporales con machine learning. La API del Detector de anomalías se adapta al identificar y aplicar los modelos de algoritmos a los datos, independientemente del sector, el escenario o volumen de datos automáticamente. Usar datos de series temporales, la API determina los límites para la detección de anomalías, los valores esperados, y qué puntos de datos son las anomalías.

![Detectar cambios de patrón en las solicitudes de servicio](./media/anomaly_detection2.png)

Uso el Detector de anomalías no requiere ninguna experiencia previa en el aprendizaje automático y la API de REST le permite integrar fácilmente el servicio en sus aplicaciones y procesos.

## <a name="features"></a>Características

Con el Detector de anomalías, puede detectar automáticamente anomalías a lo largo de los datos de series temporales, o cuando se producen en tiempo real. 

|Característica  |DESCRIPCIÓN  |
|---------|---------|
|Cuando se producen en tiempo real, detectar anomalías. | Detectar anomalías en los datos de streaming mediante el uso de puntos de datos vistos con anterioridad para determinar si la última de ellas es una anomalía. Esta operación genera un modelo con los puntos de datos de envío y determina si el punto de destino es una anomalía. Mediante una llamada a la API con cada nuevo punto de datos que genera, puede supervisar los datos que se crean. |
|Detectar anomalías en todo el conjunto de datos como un lote. | Use la serie temporal para detectar las anomalías que podrían existir a lo largo de los datos. Esta operación genera un modelo con datos de series temporales completo, con cada punto de analizar con el mismo modelo.         |
| Obtener información adicional acerca de los datos. | Obtener datos útiles acerca de los datos y las anomalías observadas, incluidos los valores esperados, los límites de anomalías y las posiciones. |
| Ajustar los límites de detección de anomalías. | La API del Detector de anomalías crea automáticamente límites para la detección de anomalías. Ajustar estos límites para aumentar o disminuir la sensibilidad de la API a las anomalías de datos y que se ajusten mejor a sus datos. |

## <a name="demo"></a>Demostración

Para empezar rápidamente con la API del Detector de anomalías, pruebe una [demostración en línea](https://notebooks.azure.com/AzureAnomalyDetection/projects/anomalydetector) que pueden ejecutarse en el explorador. Esta demostración se ejecuta en un cuaderno de Jupyter notebook hospedado en web y muestra cómo enviar una solicitud de API y visualizar el resultado.

Para ejecutar la demostración, complete los pasos siguientes:

1. Obtenga una clave de suscripción válida de API del Detector de anomalías y un punto de conexión de API. La siguiente sección proporciona instrucciones para registrarse. 
2. Inicie sesión y haga clic en el clon, en la esquina superior derecha.
3. Haga clic en **ejecutarse en proceso gratis**
4. Seleccione uno de los blocs de notas para este ejemplo.
5. Agregue su clave de suscripción de API del Detector de anomalías válida para el `subscription_key` variable. Cambiar el `endpoint` variable para el punto de conexión. Por ejemplo: `https://westus2.api.cognitive.microsoft.com`
1. En la barra de menús superior, haga clic en **celda**, a continuación, **ejecutar todo**.

## <a name="workflow"></a>Flujo de trabajo

La API del Detector de anomalías es un servicio web RESTful, facilitando el proceso llamar desde cualquier lenguaje de programación que pueda realizar solicitudes HTTP y analizar JSON.

[!INCLUDE [cognitive-services-anomaly-detector-data-requirements](../../../includes/cognitive-services-anomaly-detector-data-requirements.md)]

[!INCLUDE [cognitive-services-anomaly-detector-signup-requirements](../../../includes/cognitive-services-anomaly-detector-signup-requirements.md)]

Después de registrarse:

1. Tomar datos de series temporales y convertirlo en un formato JSON válido. Use [procedimientos recomendados](concepts/anomaly-detection-best-practices.md) al preparar los datos para obtener los mejores resultados.
1. Enviar una solicitud a la API del Detector de anomalías con sus datos.
1. Analice el mensaje JSON devuelto para procesar la respuesta de API.

## <a name="next-steps"></a>Pasos siguientes

* [Inicio rápido: Detectar anomalías en datos de series temporales mediante la API de REST del Detector de anomalías](quickstarts/detect-data-anomalies-csharp.md)
* La API del Detector de anomalías [demostración en línea](https://notebooks.azure.com/AzureAnomalyDetection/projects/anomalydetector)
* El Detector de anomalías [referencia de API de REST](https://westus2.dev.cognitive.microsoft.com/docs/services/AnomalyDetector/operations/post-timeseries-entire-detect)