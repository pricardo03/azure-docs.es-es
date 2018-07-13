---
title: 'Detección de anomalías de aplicaciones en Java: Microsoft Cognitive Services | Microsoft Docs'
description: Explore una aplicación en Java que usa Anomaly Detection API en Microsoft Cognitive Services. Envíe puntos de datos originales a la API y obtenga el valor esperado y los puntos de anomalías.
services: cognitive-services
author: wenya
manager: bix
ms.service: cognitive-services
ms.technology: anomaly-detection
ms.topic: article
ms.date: 05/01/2018
ms.author: wenya
ms.openlocfilehash: 228d440da358eba1322e2228c54f21e925e36ecd
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2018
ms.locfileid: "38597955"
---
# <a name="anomaly-detection-java-application"></a>Detección de anomalías de aplicaciones en Java

En este artículo se explica cómo usar una aplicación de Java sencilla para invocar Anomaly Detection API.  
El ejemplo envía los datos de serie temporal a Anomaly Detection API con su clave de suscripción y, después, obtiene todos los puntos de anomalías y el valor esperado de cada punto de datos a partir de la API.

## <a name="prerequisites"></a>requisitos previos

### <a name="platform-requirements"></a>Requisitos de la plataforma

Este tutorial se ha desarrollado con [IntelliJ IDEA](https://www.jetbrains.com/idea). También necesita instalar [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/index.html), versión 1.8+, así como una herramienta de compilación actualizada [Maven de Apache](http://maven.apache.org/).

### <a name="subscribe-to-anomaly-detection-and-get-a-subscription-key"></a>Suscripción a Anomaly Detection y cómo obtener una clave de suscripción 

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]
 

## <a name="download-the-tutorial-project"></a>Descargar el proyecto del tutorial

1. Vaya al [repositorio de Java](https://github.com/MicrosoftAnomalyDetection/java-sample) MicrosoftAnomalyDetection.
2. Haga clic en el botón Clonar o Descargar.
3. Haga clic en Descargar ZIP para descargar un archivo ZIP del proyecto del tutorial.

<a name="Step1"></a>
### <a name="open-the-tutorial-project"></a>Abra el proyecto del tutorial

1. Extraiga el archivo ZIP del proyecto del tutorial.
2. En IntelliJ IDEA, haga clic en **Archivo > Abrir**; aparece el cuadro de diálogo Abrir archivo o proyecto.
3. Seleccione la ruta de acceso raíz del proyecto extraído y, después, haga clic en Aceptar.
4. En el panel Proyectos, expanda **SRC > Principal > Java**.
5. Haga doble clic en com.microsoft.cognitiveservice.anomalydetection.Main.java para cargar el archivo en el editor.

<a name="Step2"></a>
### <a name="replace-subscriptionkey-and-uri-region"></a>Sustitución de subscriptionKey y de la región URI

```
// **********************************************
// *** Update or verify the following values. ***
// **********************************************

// Replace the subscriptionKey string value with your valid subscription key.
public static final String subscriptionKey = "<Subscription Key>";

public static final String uriBase = "https://api.labs.cognitive.microsoft.com/anomalyfinder/v1.0/anomalydetection";

```

<a name="Step3"></a>
### <a name="build-and-run-the-tutorial-project"></a>Compilación y ejecución del proyecto del tutorial

1. Abra el menú; para ello, haga clic con el botón derecho en cualquier parte de la pestaña de código fuente de com.microsoft.cognitiveservice.anomalydetection.Main.java. 
2. Seleccione Ejecutar "Main.main()".
3. El resultado de la solicitud de muestra se devolverá y mostrará en el terminal.

### <a name="result-of-the-tutorial-project"></a>Resultado del proyecto del tutorial

[!INCLUDE [diagrams](../includes/diagrams.md)]

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Referencia de API de REST](https://dev.labs.cognitive.microsoft.com/docs/services/anomaly-detection/operations/post-anomalydetection)
