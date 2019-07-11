---
title: 'Inicio rápido: Reconocimiento de voz, Java (Android)'
titleSuffix: Azure Cognitive Services
description: Aprenda a reconocer la voz en Java para Android mediante el SDK de Voz
services: cognitive-services
author: fmegen
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/05/2019
ms.author: wolfma
ms.openlocfilehash: 22970eb58dc244405e522d46fcaa380151076062
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2019
ms.locfileid: "67603095"
---
# <a name="quickstart-recognize-speech-in-java-on-android-by-using-the-speech-sdk"></a>Inicio rápido: Reconocimiento de voz en Java para Android mediante el SDK de Voz

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

En este artículo, aprenderá a desarrollar una aplicación Java en Android mediante el SDK de Voz de Cognitive Services para convertir voz en texto.
La aplicación se basa en el paquete de Maven del SDK de Voz, versión 1.6.0, y en Android Studio 3.3.
El SDK de Voz es compatible actualmente con dispositivos Android que tienen procesadores ARM de 32 o 64 bits o Intel x86 o x64.

> [!NOTE]
> Para el SDK de dispositivos de Voz y el dispositivo Roobo, consulte [SDK de dispositivos de voz](speech-devices-sdk.md).

## <a name="prerequisites"></a>Requisitos previos

Necesita una clave de suscripción de servicios de voz para completar este inicio rápido. Puede obtener una gratis. Para más detalles, consulte [Prueba gratuita de los servicios de voz](get-started.md).

## <a name="create-and-configure-a-project"></a>Creación y configuración de un proyecto

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-java-android-create-proj.md)]

## <a name="create-user-interface"></a>Interfaz de Create user (Crear usuario)

Se creará una interfaz de usuario básica para la aplicación. Edite el diseño de la actividad principal, `activity_main.xml`. Inicialmente, el diseño incluye una barra de título con el nombre de la aplicación y una vista de texto que contiene el texto "Hello World!" (Hola mundo).

* Haga clic en el elemento TextView. Cambie su atributo ID de la esquina superior derecha por `hello`.

* En la paleta de la esquina superior izquierda de la ventana `activity_main.xml`, arrastre un botón al espacio vacío por encima del texto.

* En los atributos del botón de la derecha, en el valor del atributo `onClick`, escriba `onSpeechButtonClicked`. Vamos a escribir un método con este nombre para controlar el evento de botón.  Cambie su atributo ID de la esquina superior derecha por `button`.

* Si quiere deducir las restricciones de diseño que se le aplican, use el icono de varita mágica de la parte superior del diseñador.

  ![Captura de pantalla del icono de varita mágica](media/sdk/qs-java-android-10-infer-layout-constraints.png)

El texto y la representación gráfica de la interfaz de usuario ahora deben tener este aspecto:

![](media/sdk/qs-java-android-11-gui.png)

[!code-xml[](~/samples-cognitive-services-speech-sdk/quickstart/java-android/app/src/main/res/layout/activity_main.xml)]

## <a name="add-sample-code"></a>Incorporación de código de ejemplo

1. Abra el archivo de origen `MainActivity.java`. Reemplace todo el código de este archivo por lo siguiente.

   [!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java-android/app/src/main/java/com/microsoft/cognitiveservices/speech/samples/quickstart/MainActivity.java#code)]

   * El método `onCreate` incluye código que solicita permisos para micrófono e Internet, y que inicializa el enlace de plataforma nativa. Solo hay que configurar los enlaces de plataforma nativa una vez. Debe realizarse al principio durante la inicialización de la aplicación.

   * El método `onSpeechButtonClicked` es, como se indicó anteriormente, el controlador de clic de botón. Al presionar un botón, se desencadena la transcripción de voz a texto.

1. En el mismo archivo, reemplace la cadena `YourSubscriptionKey` por la clave de suscripción.

1. Además, reemplace la cadena `YourServiceRegion` por la [región](regions.md) asociada a su suscripción (por ejemplo, `westus` para la suscripción de evaluación gratuita).

## <a name="build-and-run-the-app"></a>Compilación y ejecución de la aplicación

1. Conecte el dispositivo Android al equipo de desarrollo. Asegúrese de tener [habilitado el modo de desarrollo y la depuración USB](https://developer.android.com/studio/debug/dev-options) en el dispositivo.

1. Para compilar la aplicación, presione Ctrl+F9 o elija **Build (Compilar)**  > **Make Project (Crear proyecto)** desde la barra de menús.

1. Para iniciar la aplicación, presione Mayús+F10 o elija **Run (Ejecutar)**  > **Run 'app' (Ejecutar "aplicación")** .

1. En la ventanas de destino de implementación que aparece, elija el dispositivo Android.

   ![Captura de pantalla de la ventana Select Deployment Target (Seleccionar destino de implementación)](media/sdk/qs-java-android-12-deploy.png)

Presione el botón de la aplicación para empezar una sección de reconocimiento de voz. Los siguientes 15 segundos de la oración en inglés se enviarán al servicio de Voz y se transcribirán. El resultado aparece en la aplicación Android y en la ventana de logcat de Android Studio.

![Captura de pantalla de la aplicación Android](media/sdk/qs-java-android-13-gui-on-device.png)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Exploración de ejemplos de Java en GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Otras referencias

- [Personalización de modelos acústicos](how-to-customize-acoustic-models.md)
- [Personalización de modelos de lenguaje](how-to-customize-language-model.md)
