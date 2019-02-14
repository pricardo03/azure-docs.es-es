---
title: 'Inicio rápido: Traducción de voz, Java (Windows, Linux): servicios de Voz'
titleSuffix: Azure Cognitive Services
description: En este tutorial, creará una sencilla aplicación de Java para capturar la voz del usuario, traducirla a otro idioma y mostrar el texto en la línea de comandos. Esta guía está diseñada para usuarios de Windows y de Linux.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 12/13/2018
ms.author: erhopf
ms.openlocfilehash: 23582e25a7695f4573863b77b83d73408e77ac4e
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2019
ms.locfileid: "56105644"
---
# <a name="quickstart-translate-speech-with-the-speech-sdk-for-java"></a>Inicio rápido: Traducción de voz con Speech SDK para Java

En este inicio rápido, creará una sencilla aplicación de Java que captura la voz del usuario procedente del micrófono del equipo, traduce el contenido de voz y transcribe el texto traducido en la línea de comandos en tiempo real. La aplicación se crea con el paquete Maven del SDK de Voz y el IDE de Java de Eclipse (v4.64) y está diseñada para ejecutarse en Windows de 64 bits o Ubuntu Linux 16.04/18.04 de 64 bits.

Para obtener una lista completa de los idiomas disponibles para la traducción de voz, consulte la [compatibilidad con idiomas](language-support.md).

## <a name="prerequisites"></a>Requisitos previos

Esta guía de inicio rápido requiere:

* Sistema operativo: Windows de 64 bits o Ubuntu Linux 16.04/18.04 de 64 bits
* [IDE de Java de Eclipse](https://www.eclipse.org/downloads/)
* [Java 8](https://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html) o [JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html)
* Una clave de suscripción de Azure para el servicio Voz. [Obtenga una gratis](get-started.md).

Si ejecuta Ubuntu 16.04/18.04, asegúrese de que estén instaladas estas dependencias antes de iniciar Eclipse.

```console
sudo apt-get update
sudo apt-get install build-essential libssl1.0.0 libcurl3 libasound2 wget
```

> [!NOTE]
> Para el SDK de dispositivos de Voz y el dispositivo Roobo, consulte [SDK de dispositivos de voz](speech-devices-sdk.md).

## <a name="create-and-configure-project"></a>Creación y configuración de un proyecto

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-java-create-proj.md)]

## <a name="add-sample-code"></a>Incorporación de código de ejemplo

1. Para agregar una nueva clase vacía al proyecto de Java, seleccione **File (Archivo)**  >  **New (Nuevo)**  >  **Class (Clase)**.

1. En la ventana **New Java Class** (Nueva clase de Java) escriba **speechsdk.quickstart** en el campo **Package** (Paquete) y **Main** en el campo **Name** (Nombre).

   ![Captura de pantalla de la ventana New Java Class (Nueva clase de Java)](media/sdk/qs-java-jre-06-create-main-java.png)

1. Reemplace el código en `Main.java` con el siguiente fragmento de código:

   [!code-java[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/speech-translation/java-jre/src/speechsdk/quickstart/Main.java#code)]

1. Reemplace la cadena `YourSubscriptionKey` por la clave de suscripción.

1. Reemplace la cadena `YourServiceRegion` por la [región](regions.md) asociada con la suscripción (por ejemplo, `westus` para la suscripción de evaluación gratuita).

1. Guarde los cambios en el proyecto.

## <a name="build-and-run-the-app"></a>Compilación y ejecución de la aplicación

Presione F11, o seleccione **Run (Ejecutar)** > **Debug (Depurar)**.

La entrada de voz del micrófono se transcribe en alemán y se registra en la ventana de consola. Presione "Entrar" para detener la captura de voz.

![Captura de pantalla de la salida de la consola después de un reconocimiento correcto](media/sdk/qs-translate-java-jre-output.png)

## <a name="next-steps"></a>Pasos siguientes

Se pueden encontrar ejemplos adicionales como, por ejemplo, leer voz desde un archivo de audio y proporcionar una salida del texto traducido como voz sintetizada, en GitHub.

> [!div class="nextstepaction"]
> [Exploración de ejemplos de Java en GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Otras referencias

- [Inicio rápido: Reconocimiento de voz, Java (Windows, Linux)](quickstart-java-jre.md)
- [Personalización de modelos acústicos](how-to-customize-acoustic-models.md)
- [Personalización de modelos de lenguaje](how-to-customize-language-model.md)
