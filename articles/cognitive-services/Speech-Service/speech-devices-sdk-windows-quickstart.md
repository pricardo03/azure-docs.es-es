---
title: 'Inicio rápido: Ejecución de Speech Devices SDK en Windows (Servicios de voz)'
titleSuffix: Azure Cognitive Services
description: Requisitos previos e instrucciones para comenzar a usar Speech Devices SDK de Windows.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/10/2019
ms.author: erhopf
ms.openlocfilehash: ad90a6443cc1c94bcdb730e783b82dfdd4798676
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2019
ms.locfileid: "68553014"
---
# <a name="quickstart-run-the-speech-devices-sdk-sample-app-on-windows"></a>Inicio rápido: Ejecución de la aplicación de ejemplo de Speech Devices SDK en Windows

En este inicio rápido, aprenderá a usar Speech Devices SDK para Windows para crear un producto habilitado para voz o para utilizarlo como un dispositivo de [transcripción de conversaciones](conversation-transcription-service.md). Actualmente, solo se admite [Azure Kinect DK](https://azure.microsoft.com/services/kinect-dk/).

La aplicación se crea con el paquete de Speech SDK y el IDE de Java de Eclipse (v4) en Windows de 64 bits. Se ejecuta en un entorno de tiempo de ejecución de Java 8 (JRE) de 64 bits.

Para esta guía se requiere una cuenta de [Azure Cognitive Services](get-started.md) con un recurso de los servicios de Voz. Si no tiene una cuenta, puede usar la [evaluación gratuita](https://azure.microsoft.com/try/cognitive-services/) para obtener una clave de suscripción.

El código fuente de la [aplicación de ejemplo](https://aka.ms/sdsdk-download-JRE) se incluye con Speech Devices SDK. También está [disponible en GitHub](https://github.com/Azure-Samples/Cognitive-Services-Speech-Devices-SDK).

## <a name="prerequisites"></a>Requisitos previos

Esta guía de inicio rápido requiere:

* Sistema operativo: Windows de 64 bits
* [Azure Kinect DK](https://azure.microsoft.com/services/kinect-dk/)
* [IDE de Java de Eclipse](https://www.eclipse.org/downloads/)
* Solo [Java 8](https://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html) o [JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html).
* [Microsoft Visual C++ Redistributable](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads)
* Una clave de suscripción de Azure para el servicio Voz. [Obtenga una gratis](get-started.md).
* Descargue la versión más reciente de [Speech Devices SDK](https://aka.ms/sdsdk-download-JRE) para Java y extraiga el archivo .zip en el directorio de trabajo.
   > [!NOTE]
   > El archivo JRE-Sample-Release.zip incluye la aplicación de ejemplo de JRE y en este inicio rápido se da por supuesto que ha extraído la aplicación en C:\SDSDK\JRE-Sample-Release.

Transcripción de conversaciones solo está disponible actualmente para "en-US" y "zh-CN", en las regiones "centralus" y "eastasia". Debe tener una clave de voz en una de esas regiones para usar Transcripción de conversaciones.

Si tiene previsto usar intenciones necesitará una suscripción a [Language Understanding Service (LUIS)](https://docs.microsoft.com/azure/cognitive-services/luis/azureibizasubscription). Para más información acerca de LUIS y el reconocimiento de intenciones, consulte [Reconocimiento de las intenciones de voz con LUIS, C#](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-recognize-intents-from-speech-csharp). Hay un [modelo de ejemplo de LUIS](https://aka.ms/sdsdk-luis) disponible para esta aplicación.

## <a name="create-and-configure-the-project"></a>Creación y configuración del proyecto

1. Inicie Eclipse.

1. En el **selector de IDE de Eclipse**, en el campo **Workspace** (Área de trabajo), escriba el nombre de un nuevo directorio de área de trabajo. Luego, seleccione **Launch** (Iniciar).

   ![Captura de pantalla del selector de Eclipse](media/speech-devices-sdk/eclipse-launcher.png)

1. Al cabo de unos segundos, aparece la ventana principal del IDE de Eclipse. Cierre la pantalla de bienvenida si hay alguna.

1. En la barra de menús de Eclipse, cree un nuevo proyecto eligiendo **File (Archivo)**  > **New (Nuevo)**  > **Java Project (Proyecto de Java)** . Si no está disponible seleccione **Project** (Proyecto) y, a continuación, **Java Project** (Proyecto de Java).

1. Se inicia el asistente para **nuevo proyecto de Java**. **Busque** la ubicación del proyecto de ejemplo. Seleccione **Finalizar**.

   ![Captura de pantalla del asistente de nuevo proyecto de Java](media/speech-devices-sdk/eclipse-new-java-project.png)

1. En el **Explorador de paquetes**, haga clic con el botón derecho en el proyecto. Elija **Configure (Configurar)**  > **Convert to Maven Project (Convertir en proyecto de Maven)** en el menú contextual. Seleccione **Finalizar**.

   ![Captura de pantalla del Explorador de paquetes](media/speech-devices-sdk/eclipse-convert-to-maven.png)

1. Copie `kws.table`, `participants.properties` y `Microsoft.CognitiveServices.Speech.extension.pma.dll` en la carpeta del proyecto **target\classes**

## <a name="configure-the-sample-application"></a>Configurar la aplicación de ejemplo

1. Agregue la clave de suscripción de Voz al código fuente. Si quiere probar el reconocimiento de intenciones, agregue también la clave de suscripción y el identificador de aplicación del [servicio Language Understanding](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/).

   Para Voz y LUIS, la información se trasladará a `FunctionsList.java`:

   ```java
    // Subscription
    private static String SpeechSubscriptionKey = "<enter your subscription info here>";
    private static String SpeechRegion = "westus"; // You can change this if your speech region is different.
    private static String LuisSubscriptionKey = "<enter your subscription info here>";
    private static String LuisRegion = "westus2"; // you can change this, if you want to test the intent, and your LUIS region is different.
    private static String LuisAppId = "<enter your LUIS AppId>";
   ```

    Si usa la transcripción de conversaciones, la información de la clave y la región de Voz también se necesitará en `Cts.java`:

   ```java
    private static final String CTSKey = "<Conversation Transcription Service Key>";
    private static final String CTSRegion="<Conversation Transcription Service Region>";// Region may be "centralus" or "eastasia"
    ```

1. La palabra de activación predeterminada (palabra clave) es "Equipo". Además, puede probar una de las otras palabras de activación proporcionadas, como "Máquina" o "Asistente". Los archivos de recursos de estas palabras de activación alternativas pueden encontrarse en el SDK de dispositivos de voz en la carpeta de palabras clave. Por ejemplo, `C:\SDSDK\JRE-Sample-Release\keyword\Computer` contiene los archivos utilizados para la palabra de reactivación "Equipo".

   > [!TIP]
   > También puede [crear una palabra de activación personalizada](speech-devices-sdk-create-kws.md).

    Para usar una nueva palabra de reactivación, actualice las dos líneas siguientes en `FunctionsList.java` y copie el paquete de palabras de reactivación en la aplicación. Por ejemplo, para usar la palabra de reactivación "Máquina" desde el paquete de palabras de reactivación `kws-machine.zip`:

   * Copie el paquete de palabras de reactivación en la carpeta de proyecto **target/classes**.

   * Actualice `FunctionsList.java` con la palabra clave y el nombre del paquete:

     ```java
     private static final String Keyword = "Machine";
     private static final String KeywordModel = "kws-machine.zip" // set your own keyword package name.
     ```

## <a name="run-the-sample-application-from-eclipse"></a>Ejecución de la aplicación de ejemplo de Eclipse

1. En la barra de menús de Eclipse seleccione, **Run (Ejecutar)**  > **Run As (Ejecutar como)**  > **Java Application (Aplicación de Java)** . A continuación, seleccione **FunctionsList** y **OK** (Aceptar).

   ![Captura de pantalla de la aplicación "Select" de Java](media/speech-devices-sdk/eclipse-run-sample.png)

1. Se inicia la aplicación de ejemplo del SDK de dispositivos de voz y muestra las siguientes opciones:

   ![Opciones y aplicación de ejemplo del SDK de dispositivos de voz de ejemplo](media/speech-devices-sdk/java-sample-app-windows.png)

1. Pruebe la nueva demostración de **Transcripción de conversaciones**. Empiece a transcribir con **Sesión** > **Iniciar**. De forma predeterminada, todos los usuarios son invitados. Sin embargo, si dispone de las firmas de voz del participante, estas se pueden colocar en un archivo `participants.properties` en la carpeta del proyecto **target/classes**. Para generar la firma de voz, consulte [Transcripción de conversaciones (SDK)](how-to-use-conversation-transcription-service.md).

   ![Aplicación de demostración de transcripción de conversaciones](media/speech-devices-sdk/cts-sample-app-windows.png)

## <a name="create-and-run-a-standalone-application"></a>Creación y ejecución de una aplicación independiente

1. En el **Explorador de paquetes**, haga clic con el botón derecho en el proyecto. Seleccione **Exportar**. 

1. Aparecerá la ventana **Exportar**. Expanda **Java** y seleccione **Runnable JAR file** (Archivo JAR ejecutable) y, a continuación, seleccione **Siguiente**.

   ![Captura de pantalla de la ventana Exportar](media/speech-devices-sdk/eclipse-export-windows.png) 

1. Aparece la ventana **Runnable JAR File Export** (Exportación de archivo JAR ejecutable). Elija un **Destino de exportación** para la aplicación y, a continuación, seleccione **Finalizar**.
 
   ![Captura de pantalla de la ventana Runnable JAR File Export (Exportación de archivo JAR ejecutable)](media/speech-devices-sdk/eclipse-export-jar-windows.png)

1. Coloque `kws.table`, `participants.properties`, `unimic_runtime.dll`, `pma.dll` y `Microsoft.CognitiveServices.Speech.extension.pma.dll` en la carpeta de destino elegida anteriormente ya que estos archivos son necesarios para la aplicación.

1. Ejecución de la aplicación independiente

     ```powershell
     java -jar SpeechDemo.jar
     ```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Revise las notas de la versión.](devices-sdk-release-notes.md)
