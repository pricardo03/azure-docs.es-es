---
title: 'Inicio rápido: Ejecutar el SDK de dispositivos de voz en Android - servicios de voz'
titleSuffix: Azure Cognitive Services
description: Requisitos previos e instrucciones para comenzar con un SDK de dispositivos Android de voz.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: erhopf
ms.openlocfilehash: d5af2bb61eeb986f02a31d45ff9236ecc0c8427e
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65026200"
---
# <a name="quickstart-run-the-speech-devices-sdk-sample-app-on-android"></a>Inicio rápido: Ejecutar la aplicación de ejemplo de SDK de dispositivos de voz en Android

En este tutorial, obtendrá información sobre cómo usar el SDK de dispositivos de voz para Android para compilar un producto compatibles con voz.

Esta guía requiere un [Azure Cognitive Services](get-started.md) cuenta con un recurso de servicios de voz. Si no tiene una cuenta, puede usar la [evaluación gratuita](https://azure.microsoft.com/try/cognitive-services/) para obtener una clave de suscripción.

El código fuente de la aplicación de ejemplo se incluye con el SDK de dispositivos de voz. También está [disponible en GitHub](https://github.com/Azure-Samples/Cognitive-Services-Speech-Devices-SDK).

## <a name="prerequisites"></a>Requisitos previos

Para empezar a usar el SDK de dispositivos de voz, deberá:

* Siga las instrucciones proporcionadas con su [kit de desarrollo](get-speech-devices-sdk.md) para encender el dispositivo.

* Descargue la versión más reciente de la [Speech SDK de dispositivos](https://aka.ms/sdsdk-download)y extraiga el archivo .zip en el directorio de trabajo.
   > [!NOTE]
   > Este archivo incluye la aplicación de ejemplo de Android.

* Para obtener un [clave de suscripción de Azure para servicios de voz](get-started.md)

* Si tiene previsto usar los servicios de voz para identificar las intenciones (o acciones) de grabaciones de voz del usuario, necesitará un [Language Understanding Service (LUIS)](https://docs.microsoft.com/azure/cognitive-services/luis/azureibizasubscription) suscripción. Para obtener más información acerca de LUIS y reconocimiento de intenciones mediante, vea [reconocer las intenciones de voz con LUIS, C# ](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-recognize-intents-from-speech-csharp).

    También puede [crear un modelo sencillo de LUIS](https://docs.microsoft.com/azure/cognitive-services/luis/) o usar el modelo LUIS de ejemplo, LUIS-example.json. El ejemplo de modelo LUIS está disponible en el [sitio de descarga del SDK de dispositivos de voz](https://aka.ms/sdsdk-luis). Para cargar el archivo JSON del modelo en el [portal de LUIS](https://www.luis.ai/home), seleccione **Import new app** (Importar aplicación nueva) y elija el archivo JSON.

* Instale [Android Studio](https://developer.android.com/studio/) y [Vysor](https://vysor.io/download/) en su PC.

## <a name="set-up-the-device"></a>Configurar el dispositivo

1. Inicie Vysor en el equipo.

    ![Vysor](media/speech-devices-sdk/qsg-3.png)

1. El dispositivo debe figurar en la lista **Choose a device** (Seleccionar un dispositivo). Haga clic en el botón **View** (Vista) situado junto al dispositivo.

1. Para conectarse a la red inalámbrica, haga clic en el icono de la carpeta y seleccione **Settings** (Configuración) > **WLAN**.

    ![WLAN de Vysor](media/speech-devices-sdk/qsg-4.png)

    > [!NOTE]
    > Si su empresa tiene directivas respecto a la conexión de dispositivos al sistema Wi-Fi, deberá obtener la dirección MAC y ponerse en contacto con el departamento de TI para que le indiquen cómo conectarse al Wi-Fi de la empresa.
    >
    > Para buscar la dirección MAC del kit de desarrollo, seleccione el icono de la carpeta de archivos en el escritorio del kit de desarrollo.
    >
    >  ![Carpeta de archivos de Vysor](media/speech-devices-sdk/qsg-10.png)
    >
    > Seleccione **Configuración**. Busque "mac address" (dirección MAC) y luego seleccione **Mac address** (Dirección MAC) > **Advanced WLAN** (WLAN avanzada). Anote la dirección MAC que aparece cerca de la parte inferior del cuadro de diálogo.
    >
    > ![Dirección MAC de Vysor](media/speech-devices-sdk/qsg-11.png)
    >
    > Es posible que algunas compañías tengan un tiempo límite en cuanto al tiempo que un dispositivo puede estar conectado a su sistema Wi-Fi. Además, puede que tras un determinado número de días deba ampliar el registro del kit de desarrollo en el sistema Wi-Fi.

## <a name="run-the-sample-application"></a>Ejecutar la aplicación de ejemplo

Para validar la instalación del kit de desarrollo, cree e instale la aplicación de ejemplo:

1. Inicie Android Studio.

1. Seleccione **Abrir un proyecto existente de Android Studio**.

   ![Android Studio: abrir un proyecto existente](media/speech-devices-sdk/qsg-5.png)

1. Vaya a C:\SDSDK\Android-Sample-Release\example. Seleccione **Aceptar** para abrir el proyecto de ejemplo.

1. Agregue la clave de suscripción de Speech al código fuente. Si quiere probar el reconocimiento de intenciones, agregue también la clave de suscripción y el identificador de aplicación del [servicio Language Understanding](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/).

   Las claves y la información de la aplicación aparecerán en las líneas siguientes del archivo de código fuente MainActivity.java:

   ```java
   // Subscription
   private static final String SpeechSubscriptionKey = "[your speech key]";
   private static final String SpeechRegion = "westus";
   private static final String LuisSubscriptionKey = "[your LUIS key]";
   private static final String LuisRegion = "westus2.api.cognitive.microsoft.com";
   private static final String LuisAppId = "[your LUIS app ID]"
   ```

1. La palabra de activación predeterminada (palabra clave) es "Equipo". Además, puede probar una de las otras palabras de activación proporcionadas, como "Máquina" o "Asistente". Los archivos de recursos de estas palabras de activación alternativas pueden encontrarse en el SDK de dispositivos de voz en la carpeta de palabras clave. Por ejemplo, C:\SDSDK\Android-Sample-Release\keyword\Computer contiene los archivos que se usan para la palabra de activación "Equipo".

   > [!TIP]
   > También puede [crear una palabra de activación personalizada](speech-devices-sdk-create-kws.md).

    Para usar una nueva palabra de reactivación, actualice las dos líneas siguientes en `MainActivity.java`y copie el paquete de reactivación palabra a la aplicación. Por ejemplo, para usar la palabra wake 'Machine' desde el kws de paquete de reactivación word-machine.zip:

   * Copie el paquete de reactivación word en la carpeta "C:\SDSDK\Android-Sample-Release\example\app\src\main\assets\".
   * Actualización de la `MainActivity.java` con la palabra clave y el nombre del paquete:

     ```java
     private static final String Keyword = "Machine";
     private static final String KeywordModel = "kws-machine.zip" // set your own keyword package name.
     ```

1. Actualice las siguientes líneas que contienen la configuración de geometría de matriz del micrófono:

   ```java
   private static final String DeviceGeometry = "Circular6+1";
   private static final String SelectedGeometry = "Circular6+1";
   ```

   Esta tabla enumeran los valores admitidos:

   |Variable|Significado|Valores disponibles|
   |--------|-------|----------------|
   |`DeviceGeometry`|Configuración del micrófono físico|Para un kit de desarrollo circular: `Circular6+1` |
   |||Para un kit de desarrollo lineal: `Linear4`|
   |`SelectedGeometry`|Configuración de micrófono de software|Para un kit de desarrollo circular que usa todos los micrófonos: `Circular6+1`|
   |||Para un kit de desarrollo circular que usa cuatro micrófonos: `Circular3+1`|
   |||Para un kit de desarrollo lineal que usa todos los micrófonos: `Linear4`|
   |||Para un kit de desarrollo lineal que usa dos micrófonos: `Linear2`|

1. Para compilar la aplicación, en el menú **Run** (Ejecutar), seleccione **Run 'app'** (Ejecutar "aplicación"). Aparecerá el cuadro de diálogo **Select Deployment Target** (Seleccionar el destino de la implementación).

1. Elija el dispositivo y haga clic en **OK** (Aceptar) para implementar la aplicación en el dispositivo.

    ![Seleccione el cuadro de diálogo para elegir el destino de la implementación.](media/speech-devices-sdk/qsg-7.png)

1. Se inicia la aplicación de ejemplo del SDK de dispositivos de voz y muestra las siguientes opciones:

   ![Opciones y aplicación de ejemplo del SDK de dispositivos de voz de ejemplo](media/speech-devices-sdk/qsg-8.png)

1. Experimente.

## <a name="troubleshooting"></a>solución de problemas

   Si no se puede conectar al dispositivo de voz. Escriba el siguiente comando en una ventana del símbolo del sistema. Devolverá una lista de dispositivos:

   ```powershell
    adb devices
   ```

   > [!NOTE]
   > Este comando usa Android Debug Bridge `adb.exe`, que forma parte de la instalación de Android Studio. Esta herramienta se encuentra en C:\Users\[nombre de usuario] \AppData\Local\Android\Sdk\platform-tools. Puede agregar este directorio a la ruta de acceso para que sea más práctico invocar `adb`. En caso contrario, debe especificar la ruta de acceso completa a la instalación de adb.exe en todos los comandos que invoca `adb`.
   >
   > Si ve un error `no devices/emulators found` , a continuación, compruebe el cable USB está conectado y asegúrese de que se usa un cable de alta calidad.
   >

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Revise las notas de la versión.](devices-sdk-release-notes.md)
