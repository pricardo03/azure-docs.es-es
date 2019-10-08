---
title: 'Inicio rápido: Síntesis de voz en Java (Windows, Linux y macOS): Servicios de voz'
titleSuffix: Azure Cognitive Services
description: En este inicio rápido, aprenderá a crear una aplicación Java sencilla que captura y sintetiza la voz a partir de texto y la reproduce con el altavoz predeterminado.
services: cognitive-services
author: yulin-li
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 09/19/2019
ms.author: yulili
ms.openlocfilehash: 832525ae1441fca85f8df661b4a187c0be8d91dc
ms.sourcegitcommit: 4f3f502447ca8ea9b932b8b7402ce557f21ebe5a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/02/2019
ms.locfileid: "71803701"
---
# <a name="quickstart-synthesize-speech-with-the-speech-sdk-for-java"></a>Inicio rápido: Síntesis de voz con el SDK de Voz para Java

También hay disponibles inicios rápidos para el [reconocimiento de voz](quickstart-java-jre.md), la [traducción de voz a voz](quickstart-translate-speech-java-jre.md) y el [asistente virtual por voz](quickstart-virtual-assistant-java-jre.md).

En este artículo creará una aplicación de consola Java mediante el [SDK de Voz](speech-sdk.md). Sintetice la voz proveniente de texto y reprodúzcala con el altavoz predeterminado del equipo. La aplicación se crea con el paquete Maven de Speech SDK y el IDE de Java de Eclipse (v4.8) en Windows de 64 bits, Linux de 64 bits (Ubuntu 16.04, Ubuntu 18.04, Debian 9) o en macOS 10.13 o posterior. Se ejecuta en un entorno de tiempo de ejecución de Java 8 (JRE) de 64 bits.

> [!NOTE]
> Para el SDK de dispositivos de Voz y el dispositivo Roobo, consulte [SDK de dispositivos de voz](speech-devices-sdk.md).

## <a name="prerequisites"></a>Requisitos previos

Esta guía de inicio rápido requiere:

* Sistema operativo: Windows de 64 bits, Linux de 64 bits (Ubuntu 16.04, Ubuntu 18.04, Debian 9) o macOS 10.13 o posterior
* [IDE de Java de Eclipse](https://www.eclipse.org/downloads/)
* [Java 8](https://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html) o [JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html)
* Una clave de suscripción de Azure para el servicio Voz. [Obtenga una gratis](get-started.md).

Si ejecuta Ubuntu Linux, asegúrese de que estén instaladas estas dependencias antes de iniciar Eclipse.

* En Ubuntu:

  ```sh
  sudo apt-get update
  sudo apt-get install libssl1.0.0 libasound2
  ```

* En Debian 9:

  ```sh
  sudo apt-get update
  sudo apt-get install libssl1.0.2 libasound2
  ```

Si está ejecutando Windows (64 bits), asegúrese de que ha instalado Microsoft Visual C++ Redistributable para su plataforma.
* [Descarga de Microsoft Visual C++ Redistributable para Visual Studio 2019](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads)

## <a name="create-and-configure-project"></a>Creación y configuración de un proyecto

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-java-create-proj.md)]

## <a name="add-sample-code"></a>Incorporación de código de ejemplo

1. Para agregar una nueva clase vacía al proyecto de Java, seleccione **File (Archivo)**  >  **New (Nuevo)**  >  **Class (Clase)** .

1. En la ventana **New Java Class** (Nueva clase de Java) escriba **speechsdk.quickstart** en el campo **Package** (Paquete) y **Main** en el campo **Name** (Nombre).

   ![Captura de pantalla de la ventana New Java Class (Nueva clase de Java)](media/sdk/qs-java-jre-06-create-main-java.png)

1. Reemplace el código en `Main.java` con el siguiente fragmento de código:

   [!code-java[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/text-to-speech/java-jre/src/speechsdk/quickstart/Main.java#code)]

1. Reemplace la cadena `YourSubscriptionKey` por la clave de suscripción.

1. Reemplace la cadena `YourServiceRegion` por la [región](regions.md) asociada con la suscripción (por ejemplo, `westus` para la suscripción de evaluación gratuita).

1. Guarde los cambios en el proyecto.

## <a name="build-and-run-the-app"></a>Compilación y ejecución de la aplicación

Presione F11, o seleccione **Run (Ejecutar)**  > **Debug (Depurar)** .
Escriba un texto cuando se promocione y oirá el audio sintetizado, que se reproduce en el altavoz predeterminado.

## <a name="next-steps"></a>Pasos siguientes

Se pueden encontrar ejemplos adicionales, por ejemplo, cómo leer voz de un archivo de audio, en GitHub.

> [!div class="nextstepaction"]
> [Exploración de ejemplos de Java en GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Otras referencias

- [Inicio rápido: Reconocimiento de voz, Java (Windows, Linux y macOS)](quickstart-java-jre.md)
- [Inicio rápido: Conversión de voz: Java (Windows, Linux y macOS)](quickstart-translate-speech-java-jre.md)
- [Personalizar las fuentes de voz](how-to-customize-voice-font.md)
- [Grabación de ejemplos de voz](record-custom-voice-samples.md)
