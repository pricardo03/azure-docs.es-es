---
title: 'Tutorial: Detección de anomalías, Python'
titlesuffix: Azure Cognitive Services
description: Explore un cuaderno de Python que usa Anomaly Detection API. Envíe puntos de datos originales a la API y obtenga el valor esperado y los puntos de anomalías.
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.component: anomaly-detection
ms.topic: tutorial
ms.date: 05/01/2018
ms.author: chliang
ms.openlocfilehash: 98f83fbc4db8685526833c3d4f1d15903419ad3f
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2018
ms.locfileid: "48887758"
---
# <a name="tutorial-anomaly-detection-with-python-application"></a>Tutorial: Detección de anomalías con una aplicación Python

[!INCLUDE [PrivatePreviewNote](../../../../../includes/cognitive-services-anomaly-finder-private-preview-note.md)]

En este tutorial se muestra cómo utilizar Anomaly Detection API en Python y cómo visualizar los resultados mediante bibliotecas populares. Use Jupyter para ejecutar el tutorial y pruebe sus propios datos con su clave de suscripción. Para obtener información sobre cómo empezar a trabajar con cuadernos interactivos de Jupyter Notebook, consulte la [documentación de Jupyter](http://jupyter.readthedocs.io/en/latest/index.html). 

## <a name="prerequisites"></a>Requisitos previos

### <a name="subscribe-to-anomaly-detection-and-get-a-subscription-key"></a>Suscripción a Anomaly Detection y obtención de una clave de suscripción 

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]

## <a name="download-the-example-code"></a>Descarga del código de ejemplo

1. Navegue hasta el [cuaderno del tutorial en GitHub](https://github.com/MicrosoftAnomalyDetection/python-sample).
2. Haga clic en el botón verde para clonar o descargar el tutorial. 

## <a name="opening-the-tutorial-notebook-in-jupyter"></a>Apertura del cuaderno del tutorial en Jupyter

1. Abra un símbolo del sistema y vaya a la carpeta python-sample.
2. Ejecute el comando jupyter notebook desde el símbolo del sistema, que iniciará Jupyter.
3. En la ventana de Jupyter, haga clic en <em>Anomaly Detection API Example.ipynb</em> para abrir el cuaderno del tutorial.   

## <a name="running-the-tutorial"></a>Ejecución del tutorial

Para utilizar este cuaderno, necesitará una clave de suscripción para Anomaly Detection API. Visite la página de suscripción para registrarse. En la página "Inicio de sesión", use su cuenta de Microsoft para iniciar sesión y podrá suscribirse y obtener las claves. Después de completar el proceso de suscripción, pegue la clave en la sección de variables del cuaderno (se reproduce a continuación). Funcionará la clave principal o la secundaria. Asegúrese de encerrar la clave entre comillas para que sea una cadena.

```Python

            # Variables
            endpoint = 'https://api.labs.cognitive.microsoft.com/anomalyfinder/v1.0/anomalydetection'
            subscription_key = None #Here you have to paste your primary key

```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Referencia de API de REST](https://dev.labs.cognitive.microsoft.com/docs/services/anomaly-detection/operations/post-anomalydetection)
