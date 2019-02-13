---
title: 'Introducción al SDK de dispositivos de Voz: servicios de Voz'
titleSuffix: Azure Cognitive Services
description: Requisitos previos e instrucciones para comenzar a usar el SDK de dispositivos de voz.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: a9a1446db66e528cfac98222d4bd258efc660c84
ms.sourcegitcommit: 039263ff6271f318b471c4bf3dbc4b72659658ec
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/06/2019
ms.locfileid: "55750382"
---
# <a name="get-started-with-the-speech-devices-sdk"></a>Introducción al SDK de dispositivos de voz

En este artículo se describe cómo configurar el equipo de desarrollo y el kit de desarrollo de dispositivos de voz para el desarrollo de dispositivos habilitados para voz con el SDK de dispositivos de voz. A continuación, compile e implemente una aplicación de ejemplo en el dispositivo.

El código fuente de la aplicación de ejemplo se incluye con el SDK de dispositivos de voz. También está [disponible en GitHub](https://github.com/Azure-Samples/Cognitive-Services-Speech-Devices-SDK).

## <a name="prerequisites"></a>Requisitos previos

Antes de iniciar el proceso de desarrollo con el SDK de dispositivos de voz, recopile la información y el software que necesite:

* Obtenga un [kit de desarrollo de ROOBO](http://ddk.roobo.com/). Los kits están disponibles con configuraciones de matriz de micrófono lineal o circular. Elija la configuración más adecuada a sus necesidades.

    |Configuración del kit de desarrollo|Ubicación del altavoz|
    |-----------------------------|------------|
    |Circular|Cualquier dirección desde el dispositivo|
    |Lineal|Enfrente del dispositivo|

* Obtenga la versión más reciente del SDK de dispositivos de voz, en la cual se incluye una aplicación de ejemplo de Android del [sitio de descarga del SDK de dispositivos de voz](https://shares.datatransfer.microsoft.com/). Extraiga el archivo ZIP en una carpeta local como C:\SDSDK.

* Instale [Android Studio](https://developer.android.com/studio/) y [Vysor](http://vysor.io/download/) en su PC.

* Obtenga una [clave de suscripción del servicio Voz](get-started.md). Puede obtener una prueba gratuita de 30 días u obtener una clave desde el panel de Azure.

* Si quiere usar el reconocimiento de intenciones del servicio Voz, suscríbase al [servicio Language Understanding](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/) (LUIS) y [obtenga una clave de suscripción](https://docs.microsoft.com/azure/cognitive-services/luis/azureibizasubscription).

    También puede [crear un modelo sencillo de LUIS](https://docs.microsoft.com/azure/cognitive-services/luis/) o usar el modelo LUIS de ejemplo, LUIS-example.json. El ejemplo de modelo LUIS está disponible en el [sitio de descarga del SDK de dispositivos de voz](https://shares.datatransfer.microsoft.com/). Para cargar el archivo JSON del modelo en el [portal de LUIS](https://www.luis.ai/home), seleccione **Import new app** (Importar aplicación nueva) y elija el archivo JSON.

## <a name="set-up-the-development-kit"></a>Configuración del kit de desarrollo
    
1. El kit de desarrollo tiene dos conectores micro USB. El conector de la izquierda sirve para conectar el kit de desarrollo y aparece resaltado como Power (Conectar) en la siguiente imagen. El de la derecha sirve para controlarlo y aparece marcado como Debug (Depurar) en la imagen.

    ![Conectar el kit de desarrollo](media/speech-devices-sdk/qsg-1.png)
       
1. Conecte el kit de desarrollo mediante un cable micro USB para conectar el puerto de alimentación a un equipo o adaptador de corriente. Se iluminará un indicador de encendido verde en el panel superior.

1. Para controlar el kit de desarrollo, conecte el puerto de depuración a un equipo mediante un segundo cable micro USB. Es fundamental usar un cable de alta calidad para garantizar unas comunicaciones de confianza.

1. Oriente el kit de desarrollo para realizar la configuración circular o lineal.

    |Configuración del kit de desarrollo|Orientación|
    |-----------------------------|------------|
    |Circular|Vertical, con los micrófonos orientados hacia el techo|
    |Lineal|En el lateral, con los micrófonos mirando hacia usted (tal como se muestra en la siguiente imagen).|

    ![Orientación lineal del kit de desarrollo](media/speech-devices-sdk/qsg-2.png)

1. Instale los certificados y el archivo de tabla de palabra de reactivación (palabra clave) y establezca los permisos del dispositivo de sonido. Escriba los siguientes comandos en una ventana del símbolo del sistema:

   ```
   adb push C:\SDSDK\Android-Sample-Release\scripts\roobo_setup.sh /data/
   adb shell
   cd /data/
   chmod 777 roobo_setup.sh
   ./roobo_setup.sh
   exit
   ```

    > [!NOTE]
    > Estos comandos usan Android Debug Bridge, `adb.exe`, que forma parte de la instalación de Android Studio. Esta herramienta se encuentra en C:\Users\[nombre de usuario] \AppData\Local\Android\Sdk\platform-tools. Puede agregar este directorio a la ruta de acceso para que sea más práctico invocar `adb`. En caso contrario, debe especificar la ruta de acceso completa a la instalación de adb.exe en todos los comandos que invoca `adb`.
    >
    > Si ve un error `no devices/emulators found`, compruebe que el cable USB está conectado y que es un cable de alta calidad. Puede usar `adb devices` para comprobar que el equipo puede comunicarse con el kit de desarrollo ya que este devolverá una lista de dispositivos.

    > [!TIP]
    > Silencie el micrófono y el altavoz del equipo para asegurarse de que está trabajando con los micrófonos del kit de desarrollo. De esta manera, no activará accidentalmente el dispositivo con el audio del equipo.

1.  Inicie Vysor en el equipo.

    ![Vysor](media/speech-devices-sdk/qsg-3.png)

1.  El dispositivo debe figurar en la lista **Choose a device** (Seleccionar un dispositivo). Haga clic en el botón **View** (Vista) situado junto al dispositivo.

1.  Para conectarse a la red inalámbrica, haga clic en el icono de la carpeta y seleccione **Settings** (Configuración) > **WLAN**.

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
    >
    > Si quiere asociar un altavoz al kit de desarrollo, puede conectarlo a la salida de línea de audio. Asimismo, deberá elegir un altavoz de 3,5 mm de buena calidad.
    >
    > ![Audio de Vysor](media/speech-devices-sdk/qsg-14.png)

## <a name="run-a-sample-application"></a>Ejecución de una aplicación de ejemplo

Para ejecutar las pruebas de ROOBO y validar la configuración del kit de desarrollo, compile e instale la aplicación de ejemplo.

1.  Inicie Android Studio.

1.  Seleccione **Abrir un proyecto existente de Android Studio**.

    ![Android Studio: abrir un proyecto existente](media/speech-devices-sdk/qsg-5.png)

1.  Vaya a C:\SDSDK\Android-Sample-Release\example. Seleccione **Aceptar** para abrir el proyecto de ejemplo.

1.  Agregue la clave de suscripción de Speech al código fuente. Si quiere probar el reconocimiento de intenciones, agregue también la clave de suscripción y el identificador de aplicación del [servicio Language Understanding](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/).

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

    También puede [crear una palabra de activación personalizada](speech-devices-sdk-create-kws.md).

    Para instalar la palabra de activación que quiere usar:

    * Cree una carpeta de palabras clave en la carpeta de datos del dispositivo; para ello, ejecute los siguientes comandos en la ventana del símbolo del sistema:

        ```
        adb shell
        cd /data
        mkdir keyword
        exit
        ```

    * Copie los archivos kws.table, kws_k.fst y words_kw.txt a la carpeta \data\keyword del dispositivo. Ejecute los siguientes comandos en la ventana del símbolo del sistema. Si creó una [palabra de activación personalizada](speech-devices-sdk-create-kws.md), el archivo kws.table que se creó en la web estará en el mismo directorio que los archivos kws.table, kws_k.fst y words_kw.txt. Para obtener una palabra de activación personalizada, use el comando `adb push C:\SDSDK\Android-Sample-Release\keyword\[wake_word_name]\kws.table /data/keyword` para insertar el archivo kws.table en el kit de desarrollo:

        ```
        adb push C:\SDSDK\Android-Sample-Release\keyword\kws.table /data/keyword
        adb push C:\SDSDK\Android-Sample-Release\keyword\Computer\kws_k.fst /data/keyword
        adb push C:\SDSDK\Android-Sample-Release\keyword\Computer\words_kw.txt /data/keyword
        ```

    * Haga referencia a estos archivos en la aplicación de ejemplo. Busque las líneas siguientes en MainActivity.java. Asegúrese de que la palabra clave especificada sea la que está utilizando y que la ruta de acceso apunte al archivo `kws.table` que insertó en el dispositivo.

        ```java
        private static final String Keyword = "Computer";
        private static final String KeywordModel = "/data/keyword/kws.table";
        ```

        > [!NOTE]
        > En su propio código, puede usar el archivo kws.table para crear una instancia de modelo de palabra clave y activar el reconocimiento tal como se indica a continuación:
        >
        > ```java
        > KeywordRecognitionModel km = KeywordRecognitionModel.fromFile(KeywordModel);
        > final Task<?> task = reco.startKeywordRecognitionAsync(km);
        > ```

1.  Actualice las siguientes líneas que contienen la configuración de geometría de matriz del micrófono:

    ```java
    private static final String DeviceGeometry = "Circular6+1";
    private static final String SelectedGeometry = "Circular6+1";
    ```
    En la tabla siguiente se describen los valores disponibles:

    |Variable|Significado|Valores disponibles|
    |--------|-------|----------------|
    |`DeviceGeometry`|Configuración del micrófono físico|Para un kit de desarrollo circular: `Circular6+1` |
    |||Para un kit de desarrollo lineal: `Linear4`|
    |`SelectedGeometry`|Configuración de micrófono de software|Para un kit de desarrollo circular que usa todos los micrófonos: `Circular6+1`|
    |||Para un kit de desarrollo circular que usa cuatro micrófonos: `Circular3+1`|
    |||Para un kit de desarrollo lineal que usa todos los micrófonos: `Linear4`|
    |||Para un kit de desarrollo lineal que usa dos micrófonos: `Linear2`|


1.  Para compilar la aplicación, en el menú **Run** (Ejecutar), seleccione **Run 'app'** (Ejecutar "aplicación"). Aparecerá el cuadro de diálogo **Select Deployment Target** (Seleccionar el destino de la implementación).

1. Elija el dispositivo y haga clic en **OK** (Aceptar) para implementar la aplicación en el dispositivo.

    ![Seleccione el cuadro de diálogo para elegir el destino de la implementación.](media/speech-devices-sdk/qsg-7.png)

1.  Se inicia la aplicación de ejemplo del SDK de dispositivos de voz y muestra las siguientes opciones:

    ![Opciones y aplicación de ejemplo del SDK de dispositivos de voz de ejemplo](media/speech-devices-sdk/qsg-8.png)

1. Experimente.

## <a name="troubleshooting"></a>solución de problemas

### <a name="certificate-failures"></a>Errores de certificado

Si recibe errores de certificado cuando use el servicio Voz, asegúrese de que el dispositivo tiene la fecha y hora correctas:

1. Vaya a **Configuración**. En **System** (Sistema), seleccione **Date & time** (Fecha y hora).

    ![En Settings (Configuración), seleccione Date & time (Fecha y hora).](media/speech-devices-sdk/qsg-12.png)

1. Mantenga la opción **Automatic date & time** (Fecha y hora automáticas) seleccionada. En **Select time zone** (Seleccionar zona horaria), seleccione la zona horaria actual.

    ![Seleccione las opciones de fecha y zona horaria](media/speech-devices-sdk/qsg-13.png)

    Cuando vea que la hora del kit desarrollo coincide con la hora del equipo, sabrá que el kit de desarrollo está conectado a Internet.

    Para obtener más información de desarrollo, consulte la [guía de desarrollo de ROOBO](http://dwn.roo.bo/server_upload/ddk/ROOBO%20Dev%20Kit-User%20Guide.pdf).

### <a name="audio"></a>Audio

ROOBO proporciona una herramienta que captura todo el audio en una memoria Flash. Esto puede serle de utilidad para ayudarle a solucionar problemas de audio. Se proporciona una versión de la herramienta para cada configuración del kit de desarrollo. Elija el dispositivo en el [sitio de ROOBO](http://ddk.roobo.com/) y, a continuación, haga clic en el vínculo **ROOBO Tools**(Herramientas de ROOBO) en la parte inferior de la página.
