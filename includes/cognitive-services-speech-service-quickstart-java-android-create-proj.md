---
author: trrwilson
ms.service: cognitive-services
ms.topic: include
ms.date: 5/23/2019
ms.author: travisw
ms.openlocfilehash: a8118d80e85d562fa4137ed1f1844e6bf9f1793e
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/01/2019
ms.locfileid: "67485022"
---
1. Inicie Android Studio y seleccione **Start a new Android Studio project** (Iniciar un nuevo proyecto de Android Studio) en la ventana Welcome (Bienvenida).

    ![Captura de pantalla de la ventana Welcome (Bienvenida) de Android Studio](../articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-01-start-new-android-studio-project.png)

1. Aparece el asistente **Choose your project** (Elija su proyecto); seleccione **Phone and Tablet** (Teléfono y tableta) y **Empty Activity** (Actividad vacía) en el cuadro de selección de actividad. Seleccione **Next** (Siguiente).

   ![Captura de pantalla del asistente para la elección del proyecto](../articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-02-target-android-devices.png)

1. En la pantalla **Configure your project** (Configure su proyecto), escriba **Inicio rápido** como **nombre**, **samples.speech.cognitiveservices.microsoft.com** como **nombre del paquete** y elija un directorio de proyecto. En **Minimum API level** (Nivel de API mínimo), seleccione **API 23: Android 6.0 (Marshmallow)** , deje el resto de casillas desactivadas y seleccione **Finish** (Finalizar).

   ![Captura de pantalla del asistente para configurar el proyecto](../articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-03-create-android-project.png)

Android Studio tardará unos minutos en preparar el nuevo proyecto de Android. A continuación, configure el proyecto para obtener información del SDK de Voz y para usar Java 8.

[!INCLUDE [License Notice](cognitive-services-speech-service-license-notice.md)]

La versión actual del SDK de Speech de Cognitive Services es `1.6.0`.

El SDK de Voz para Android está empaquetado como una [biblioteca de Android (AAR)](https://developer.android.com/studio/projects/android-library), que incluye las bibliotecas necesarias, así como los permisos necesarios de Android.
Se hospeda en un repositorio de Maven en https:\//csspeechstorage.blob.core.windows.net/maven/.

Configure el proyecto para usar el SDK de Voz. Abra la ventana de la estructura del proyecto eligiendo **File (Archivo)**  > **Project Structure (Estructura del proyecto)** desde la barra de menús de Android Studio. En la ventana Project Structure (Estructura del proyecto), realice los siguientes cambios:

1. En la lista, en el lado izquierdo de la ventana, seleccione **Project** (Proyecto). Edite la configuración **Default Library Repository** (Repositorio de bibliotecas predeterminado); debe anexar una coma y poner entre comillas sencillas la dirección URL del repositorio de Maven. 'https:\//csspeechstorage.blob.core.windows.net/maven/'

   ![Captura de pantalla de la ventana Project Structure (Estructura de proyecto)](../articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-06-add-maven-repository.png)

1. En la misma pantalla, en el lado izquierdo, seleccione **App** (Aplicación). A continuación, seleccione la pestaña **Dependencies** (Dependencias) en la parte superior de la ventana. Seleccione el signo más verde (+) y elija **Library dependency** (Dependencia de biblioteca) en el menú desplegable.

   ![Captura de pantalla de la ventana Project Structure (Estructura de proyecto)](../articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-07-add-module-dependency.png)

1. En la ventana que aparece, escriba el nombre y la versión de nuestro SDK de Voz para Android, `com.microsoft.cognitiveservices.speech:client-sdk:1.6.0`. Después seleccione **Aceptar**.
   El SDK de Voz se agregará a la lista de dependencias, como se muestra a continuación:

   ![Captura de pantalla de la ventana Project Structure (Estructura de proyecto)](../articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-08-dependency-added-1.0.0.png)

1. Haga clic en la pestaña **Properties** (Propiedades). En **Source Compability** (Compatibilidad de origen) y **Target Compatibility** (Compatibilidad de destino), seleccione **1.8**.

   ![](../articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-09-dependency-added.png)

1. Seleccione **OK** (Aceptar) para cerrar la ventana de la estructura del proyecto y aplicar los cambios al proyecto.
