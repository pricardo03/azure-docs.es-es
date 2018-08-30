---
title: 'Guía de inicio rápido: Reconocimiento de voz en Java en Android con Speech SDK de Cognitive Services'
titleSuffix: Microsoft Cognitive Services
description: Aprenda a reconocer voz en Java para Android con el SDK de Voz de Cognitive Services
services: cognitive-services
author: fmegen
ms.service: cognitive-services
ms.technology: Speech
ms.topic: article
ms.date: 07/16/2018
ms.author: fmegen
ms.openlocfilehash: 9f761fed46f0730a64a984111da1bae1229cc93d
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2018
ms.locfileid: "43127078"
---
# <a name="quickstart-recognize-speech-in-java-on-android-using-the-speech-sdk"></a>Inicio rápido: Reconocimiento de voz en Java para Android mediante el SDK de Voz

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

En este artículo, aprenderá a crear una aplicación Java en Android mediante el SDK de Voz de Cognitive Services para convertir voz en texto.
La aplicación se basa en el paquete de Maven del SDK de Voz de Microsoft Cognitive Services versión 0.6.0 y Android Studio 3.1.

> [!NOTE]
> Para información sobre el SDK de dispositivos de voz y el dispositivo Roobo, visite la página [SDK de dispositivos de voz](speech-devices-sdk.md).

## <a name="prerequisites"></a>Requisitos previos

* Clave de suscripción para Speech Service. Consulte [Pruebe Speech Service gratis](get-started.md).
* Un PC (Windows, Linux, Mac) capaz de ejecutar Android Studio.
* La versión 3.1 de [Android Studio](https://developer.android.com/studio/).
* Un dispositivo Android basado en ARM (API 23: Android 6.0 Marshmallow o posterior) [habilitado para el desarrollo](https://developer.android.com/studio/debug/dev-options) con un micrófono que funcione.

## <a name="create-an-android-studio-project"></a>Creación de un nuevo proyecto en Android Studio

Inicie Android Studio y seleccione **Start a new Android Studio project** (Iniciar un nuevo proyecto de Android Studio).

![](media/sdk/qs-java-android-01-start-new-android-studio-project.png)

En el **Asistente para crear un nuevo proyecto** que aparece, realice las siguientes selecciones:

1. En la pantalla **Create Android Project** (Crear proyecto Android), escriba **Quickstart** como **nombre de la aplicación**, **samples.speech.cognitiveservices.microsoft.com** como **dominio de empresa** y elija una ubicación para el proyecto. Deje las casillas sin activar y haga clic en **Next** (Siguiente).

   ![](media/sdk/qs-java-android-02-create-android-project.png)

1. En la pantalla **Target Android Devices** (Dispositivos Android de destino), marque **Phone and Tablet** (Teléfono y tableta) como única opción, elija **API 23: Android 6.0 (Marshmallow)** en la lista desplegable que hay debajo y haga clic en **Next** (Siguiente).

   ![](media/sdk/qs-java-android-03-target-android-devices.png)

1. En la pantalla **Add an Activity to Mobile** (Agregar una actividad al móvil), seleccione **Empty Activity** (Actividad vacía) y haga clic en **Next** (Siguiente).

   ![](media/sdk/qs-java-android-04-add-an-activity-to-mobile.png)

1. En la pantalla **Configure Activity** (Configurar actividad), use **MainActivity** como nombre de la actividad y **activity\_main** como nombre de diseño. Active ambas casillas y haga clic en **Finish** (Finalizar).

   ![](media/sdk/qs-java-android-05-configure-activity.png)

Al cabo de un rato, aparecerá el proyecto de Android Studio recién creado.

## <a name="configure-your-project-for-the-speech-sdk"></a>Configuración del proyecto para el SDK de Voz

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

La versión actual del SDK de Speech de Cognitive Services es `0.6.0`.

El SDK de Voz para Android está empaquetado como una [biblioteca de Android (AAR)](https://developer.android.com/studio/projects/android-library), que incluye las bibliotecas necesarias, así como los permisos necesarios de Android para usarla.
Se hospeda en un repositorio de Maven en https://csspeechstorage.blob.core.windows.net/maven/.

A continuación se describe cómo configurar el proyecto para usar el SDK de Voz.

Abra la ventana de la estructura del proyecto en **File** (Archivo) \> **Project Structure** (Estructura del proyecto).
En la ventana que aparece, realice los cambios siguientes (haga clic en **OK** [Aceptar] solo después de completar todos los pasos):

1. Seleccione **Project** (Proyecto) y edite la configuración **Default Library Repository** (Repositorio de bibliotecas predeterminado); debe anexar una coma y poner entre comillas sencillas la dirección URL del repositorio de Maven `'https://csspeechstorage.blob.core.windows.net/maven/'`:

  ![](media/sdk/qs-java-android-06-add-maven-repository.png)

1. Sin salir de la misma pantalla, seleccione el módulo **App** (Aplicación) en el lado izquierdo y en la parte superior de la pestaña **Dependencies** (Dependencias). A continuación, haga clic en el signo + verde en la esquina superior derecha y seleccione **Library dependency** (Dependencia de biblioteca).

  ![](media/sdk/qs-java-android-07-add-module-dependency.png)

1. En la ventana que aparece, escriba el nombre y la versión de nuestro SDK de Voz para Android, `com.microsoft.cognitiveservices.speech:client-sdk:0.6.0` y, luego, haga clic en **OK** (Aceptar).
   El SDK de Voz se agregará a la lista de dependencias, como se muestra a continuación:

  ![](media/sdk/qs-java-android-08-dependency-added.png)

1. En la parte superior, seleccione la pestaña **Properties** (Propiedades). Seleccione **1.8** para **Source Compability** (Compatibilidad de origen) y **Target Compatibility** (Compatibilidad de destino).

  ![](media/sdk/qs-java-android-09-dependency-added.png)

1. Finalmente, haga clic en **OK** (Aceptar) para cerrar las ventanas **Project Structure** (Estructura de proyecto) y aplicar todas las actualizaciones.

## <a name="create-a-minimal-ui"></a>Creación de una interfaz de usuario mínima

Edite el diseño de la actividad principal, `activity_main.xml`.
De forma predeterminada, aparecerá una barra de título con el nombre de la aplicación y una vista de texto que dice "Hello World!" (Hola mundo).

* Haga clic en la vista de texto. Cambie su atributo ID de la esquina superior derecha por `hello`.

* En la paleta de la esquina superior izquierda de la ventana `activity_main.xml`, arrastre un botón al espacio vacío por encima del texto.

* En los atributos del botón de la derecha, en el valor del atributo `onClick`, escriba `onSpeechButtonClicked`, que será el nombre de nuestro controlador de botón.
  Cambie su atributo ID de la esquina superior derecha por `button`.

* Si quiere deducir las restricciones de diseño que se le aplican, use el icono de varita mágica de la parte superior del diseñador.

  ![](media/sdk/qs-java-android-10-infer-layout-constraints.png)

El texto y la versión gráfica de la interfaz de usuario ahora deben tener este aspecto:

<table>
<tr>
<td valign="top">
![](media/sdk/qs-java-android-11-gui.png)
</td>
<td valign="top">
[!code-xml[](~/samples-cognitive-services-speech-sdk/quickstart/java-android/app/src/main/res/layout/activity_main.xml)]
</td>
</tr>
</table>

## <a name="add-the-sample-code"></a>Incorporación del código de ejemplo

1. Edite el archivo de origen `MainActivity.java` y reemplace su código por el siguiente (debajo de la instrucción del paquete):

   [!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java-android/app/src/main/java/com/microsoft/cognitiveservices/speech/samples/quickstart/MainActivity.java#code)]

   * El método `onCreate` incluye código que solicita permisos para micrófono e Internet, y que inicializa el enlace de plataforma nativa. Configurar los enlaces de plataforma nativa solo es necesario una vez, es decir, se debe realizar al principio durante la inicialización de la aplicación.
   
   * El método `onSpeechButtonClicked` se instaló previamente como controlador de clic de botón. Una presión del botón desencadena el reconocimiento de voz real.

1. Reemplace la cadena `YourSubscriptionKey` por la clave de suscripción.

1. Reemplace la cadena `YourServiceRegion` por la [región](regions.md) asociada a sus suscripción (por ejemplo, `westus` para la suscripción de evaluación gratuita).

## <a name="build-and-run-the-sample"></a>Compilación y ejecución del ejemplo

* Para compilar, presione Ctrl+F9 o seleccione **Build** \> **Make Project** (Compilar > Crear proyecto).

* Conecte el dispositivo Android al equipo de desarrollo. Asegúrese de tener [habilitado el modo de desarrollo y la depuración USB](https://developer.android.com/studio/debug/dev-options).

* Para iniciar la aplicación, presione Mayús+F10, o seleccione **Run** \> **Run 'app'** (Ejecutar > Ejecutar "app").

* En las ventanas de destino de implementación que aparecen, elija el dispositivo Android.

  ![Inicio de la aplicación en depuración](media/sdk/qs-java-android-12-deploy.png)

* La aplicación se iniciará en el dispositivo.
  Una vez que presione el botón, los siguientes 15 segundos se reconocerán y se mostrarán en la interfaz de usuario (podrá ver la respuesta en la ventana logcat de Android Studio):

  ![Interfaz de usuario tras el reconocimiento correcto](media/sdk/qs-java-android-13-gui-on-device.png)

Esta captura de pantalla concluye el inicio rápido de Android. El código de ejemplo completo del proyecto se puede descargar del repositorio de ejemplos.

[!INCLUDE [Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Busque este ejemplo en la carpeta `quickstart/java-android`.

## <a name="next-steps"></a>Pasos siguientes

* [Obtener nuestros ejemplos](speech-sdk.md#get-the-samples)
