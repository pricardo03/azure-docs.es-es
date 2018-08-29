---
title: Introducción al SDK de dispositivos de voz
description: Requisitos previos e instrucciones para comenzar a usar el SDK de dispositivos de voz.
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.technology: speech
ms.topic: article
ms.date: 05/18/2018
ms.author: v-jerkin
ms.openlocfilehash: 463a015b7c01dafc5b30de56b95fa0510ffb98e4
ms.sourcegitcommit: fab878ff9aaf4efb3eaff6b7656184b0bafba13b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/22/2018
ms.locfileid: "42424376"
---
# <a name="get-started-with-the-speech-devices-sdk"></a>Introducción al SDK de dispositivos de voz

En este artículo se describe cómo configurar el equipo de desarrollo y el kit de desarrollo de dispositivos de voz para el desarrollo de dispositivos habilitados para voz con el SDK de dispositivos de voz. A continuación, compilará e implementar una aplicación de ejemplo en el dispositivo. 

El código fuente de la aplicación de ejemplo se incluye con el SDK de dispositivos de voz y también está [disponible en GitHub](https://github.com/Azure-Samples/Cognitive-Services-Speech-Devices-SDK).

## <a name="prerequisites"></a>Requisitos previos

Antes de iniciar el desarrollo con el SDK de dispositivos de voz, recopile la información y el software que necesitará.

* Obtenga un kit de desarrollo [de Roobo](http://ddk.roobo.com/). Los kits están disponibles con configuraciones de matriz de micrófono lineal o circular; elija la adecuada para sus necesidades.

    |Configuración del kit de desarrollo|Ubicación del altavoz|
    |-----------------------------|------------|
    |Circular|Cualquier dirección desde el dispositivo|
    |Lineal|Enfrente del dispositivo|

* Obtenga la versión más reciente del SDK de dispositivos de voz, incluida una aplicación de ejemplo de Android, desde el [sitio de descarga](https://shares.datatransfer.microsoft.com/) del SDK de dispositivos de voz. Extraiga el archivo ZIP en una carpeta local (como `C:\SDSDK`).

* Instale [Android Studio](https://developer.android.com/studio/) y [Vysor](http://vysor.io/download/) en su PC.

* Obtenga una [clave de suscripción](get-started.md) del servicio Speech. Puede obtener una prueba gratuita de 30 días u obtener una clave desde el panel de Azure.

* Si desea usar el reconocimiento de intención del servicio Speech, suscríbase al [servicio Language Understanding](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/) (LUIS) y [obtenga una clave de suscripción](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/azureibizasubscription). 

    Puede [crear un modelo simple de LUIS](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/) o utilizar el modelo de ejemplo de LUIS, `LUIS-example.json`, disponible en el [sitio de descarga](https://shares.datatransfer.microsoft.com/) del SDK de dispositivos de voz. Cargue el archivo JSON de su modelo en el [portal de LUIS](https://www.luis.ai/home); para ello, haga clic en **Import new app** (Importar nueva aplicación) y elija el archivo JSON.

## <a name="set-up-the-development-kit"></a>Configuración del kit de desarrollo

1. Encienda el kit de desarrollo mediante un cable mini USB conectado a un equipo o una fuente de alimentación. Debería iluminarse un indicador de potencia verde en el panel superior.

1. Conecte el kit de desarrollo a un equipo mediante un segundo cable mini USB.

    ![conexión del kit de desarrollo](media/speech-devices-sdk/qsg-1.png)

1. Oriente el kit de desarrollo de forma adecuada.

    |Configuración del kit de desarrollo|Orientación|
    |-----------------------------|------------|
    |Circular|Vertical, con los micrófonos orientados hacia el techo|
    |Lineal|Sobre su lateral, con micrófonos de cara a usted (como se muestra a continuación)|

    ![orientación lineal del kit de desarrollo](media/speech-devices-sdk/qsg-2.png)

1. Instale los certificados y el archivo de tabla de palabra de reactivación (palabra clave) y establezca los permisos del dispositivo de sonido. Escriba los siguientes comandos en una ventana de comandos.

    > [!NOTE]
    > Estos comandos usan Android Debug Bridge, `adb.exe`, que forma parte de la instalación de Android Studio. Esta herramienta se puede encontrar en `C:\Users\[user name]\AppData\Local\Android\Sdk\platform-tools`. Puede agregar este directorio a la ruta de acceso para que sea más práctico invocar `adb`. En caso contrario, debe especificar la ruta de acceso completa a la instalación de `adb.exe` en todos los comandos que invoca `adb`.

    ```
    adb push C:\SDSDK\Android-Sample-Release\scripts\roobo_setup.sh /data/ 
    adb shell
    cd /data/ 
    chmod 777 roobo_setup.sh
    ./roobo_setup.sh
    exit
    ```

    > [!TIP]
    > Silencie el micrófono y los altavoces de su PC. De esta manera, puede estar seguro de que está trabajando con los micrófonos del kit de desarrollo, y no activará accidentalmente el dispositivo con audio del PC.
    
1.  Inicie Vysor en el equipo.

    ![Vysor](media/speech-devices-sdk/qsg-3.png)

1.  El dispositivo debe figurar en la lista de "Selección de un dispositivo". Haga clic en el botón **Vista** situado junto a ella. 
 
1.  Para conectarse a la red inalámbrica, haga clic en el icono de carpeta y luego haga clic en **Settings** (Configuración) y **WLAN**.

    ![WLAN de Vysor](media/speech-devices-sdk/qsg-4.png)
 
 > [!NOTE]
 > Si su empresa tiene directivas respecto a la conexión de dispositivos al sistema Wi-Fi, deberá obtener la dirección Mac y ponerse en contacto con el departamento de TI para que le digan cómo conectarse. Para buscar la dirección Mac del kit de desarrollo, haga clic en el icono de carpeta de archivos en el escritorio del kit de desarrollo, en **Settings** (Configuración) busque "Mac address" (dirección Mac), haga clic en **Mac address** (Dirección Mac) en **Advanced WLAN** (WLAN avanzada) y anote la dirección Mac que se encuentra hacia la parte inferior. Además, puede que algunas compañías tengan un tiempo límite en cuanto al tiempo que un dispositivo puede estar conectado a sus sistemas Wi-Fi. Es posible que, tras un determinado número de días, deba ampliar el registro del kit de desarrollo en el sistema Wi-Fi.  
 
 
   ![Carpeta de archivos de Vysor](media/speech-devices-sdk/qsg-10.png)
   
   ![Dirección Mac de Vysor](media/speech-devices-sdk/qsg-11.png)
   
   
 > Si quiere asociar un altavoz al kit de desarrollo, puede conectarlo a la salida de línea de audio. Además, deberá elegir un altavoz de 3,5 mm de buena calidad.
 
   ![Audio de Vysor](media/speech-devices-sdk/qsg-14.png)
 
## <a name="run-a-sample-application"></a>Ejecución de una aplicación de ejemplo

Para ejecutar las pruebas de Roobo y validar la configuración del kit de desarrollo, compile e instale la aplicación de ejemplo.

1.  Inicie Android Studio.

1.  Seleccione abrir un proyecto existente de Android Studio.

    ![Android studio: abrir proyecto existente](media/speech-devices-sdk/qsg-5.png)
 
1.  Vaya a `C:\SDSDK\Android-Sample-Release\example` y, a continuación, haga clic en **Aceptar** para abrir el proyecto de ejemplo.
 
1.  Agregue la clave de suscripción de Speech al código fuente. Si desea probar el reconocimiento de intenciones, agregue también la clave de suscripción y el identificador de aplicación del [servicio Language Understanding](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/). 

    Las claves y la información de la aplicación se quedan en las líneas siguientes del archivo de código fuente `MainActivity.java`.

    ```java
    // Subscription
    private static final String SpeechSubscriptionKey = "[your speech key]";
    private static final String SpeechRegion = "westus";
    private static final String LuisSubscriptionKey = "[your LUIS key]";
    private static final String LuisRegion = "westus2.api.cognitive.microsoft.com";
    private static final String LuisAppId = "[your LUIS app id]"
    ```

1. La palabra de activación predeterminada (palabra clave) es "Equipo".  Si lo desea, puede probar una de las otras palabras de reactivación proporcionadas: "Máquina" y "Asistente". Los archivos de recursos para estas palabras alternativas pueden encontrarse en el SDK de dispositivos de voz en la carpeta "palabra clave". Por ejemplo, `C:\SDSDK\Android-Sample-Release\keyword\Computer` contiene los archivos utilizados para "Equipo".

    También puede [crear una palabra de reactivación personalizada](speech-devices-sdk-create-kws.md).

    Para instalar la palabra de reactivación deseada:
 
    * Cree una carpeta `keyword` en la carpeta \data\ del dispositivo mediante la ejecución de los siguientes comandos en la ventana de comandos.

        ```
        adb shell
        cd /data
        mkdir keyword
        exit
        ```

    * Copie los archivos `kws.table`, `kws_g.fst`, `kws_k.fst` y `words_kw.txt` en la carpeta \data\keyword\ del dispositivo. Ejecute los siguientes comandos en una ventana de comandos. Si ha creado una [palabra de reactivación personalizada](speech-devices-sdk-create-kws.md), el archivo kws.table va a estar en el mismo directorio que los archivos `kws.table`, `kws_g.fst`, `kws_k.fst` y `words_kw.txt`. Use el comando adb push C:\SDSDK\Android-Sample-Release\keyword\[wake_word_name]\kws.table /data/keyword para insertar en su lugar el archivo kws.table en el kit de desarrollo.

        ```
        adb push C:\SDSDK\Android-Sample-Release\keyword\kws.table /data/keyword
        adb push C:\SDSDK\Android-Sample-Release\keyword\Computer\kws_g.fst /data/keyword
        adb push C:\SDSDK\Android-Sample-Release\keyword\Computer\kws_k.fst /data/keyword
        adb push C:\SDSDK\Android-Sample-Release\keyword\Computer\words_kw.txt /data/keyword
        ```
    
    * Haga referencia a estos archivos en la aplicación de ejemplo. Agregue las líneas siguientes a `MainActivity.java`. Asegúrese de que la palabra clave especificada sea la que está utilizando y que la ruta apunte al archivo `kws.table` que insertó en el dispositivo.
        
        ```java
        private static final String Keyword = "Computer";
        private static final String KeywordModel = "/data/keyword/kws.table";
        ```

        > [!NOTE]
        > En su propio código, puede usar el archivo `kws.table` para crear una instancia de modelo de palabra clave y activar el reconocimiento como se indica a continuación.
        >
        > ```java
        > KeywordRecognitionModel km = KeywordRecognitionModel.fromFile(KeywordModel);
        > final Task<?> task = reco.startKeywordRecognitionAsync(km);
        > ```

1.  Actualice las siguientes líneas que contienen la configuración de geometría de matriz del micrófono.

    ```java
    private static final String DeviceGeometry = "Circular6+1";
    private static final String SelectedGeometry = "Circular6+1";
    ```

    |Variable|Significado|Valores disponibles|
    |--------|-------|----------------|
    |`DeviceGeometry`|Configuración del micrófono físico|`Circular6+1` para el kit de desarrollo circular|
    ||| `Linear4` para el kit de desarrollo lineal|
    |`SelectedGeometry`|Configuración de micrófono de software|`Circular6+1` para el kit de desarrollo circular que usa todos los micrófonos|
    |||`Circular3+1` para el kit de desarrollo circular que usa cuatro micrófonos|
    |||`Linear4` para el kit de desarrollo lineal que usa todos los micrófonos|
    |||`Linear2` para el kit de desarrollo lineal que usa dos micrófonos|


1.  Compile la aplicación eligiendo **Ejecutar "aplicación"** en el menú de ejecución. Aparece el cuadro de diálogo para seleccionar el destino de la implementación. Elija el dispositivo y haga clic en **Aceptar** para implementar la aplicación en el dispositivo.

    ![seleccionar destino de implementación](media/speech-devices-sdk/qsg-7.png)
 
1.  Se inicia la aplicación de ejemplo del SDK de dispositivos de voz, con las opciones que se muestran aquí.

    ![aplicación de dispositivo de voz de ejemplo](media/speech-devices-sdk/qsg-8.png)

1. Practique con ella.

## <a name="troubleshooting"></a>solución de problemas

Si recibe errores de certificado cuando se usa el servicio Speech, asegúrese de que el dispositivo tiene la fecha y hora correctas. Vaya a **Settings** (Configuración), haga clic en **Date & time** (Fecha y hora) en System (Sistema) y en **Select time zone** (Seleccionar zona horaria) como la zona horaria actual. Mantenga activada la opción **Automatic date & time** (Fecha y hora automáticas). Cuando vea que la hora del kit desarrollo coincide con la hora del PC, sabrá que el kit de desarrollo está conectado a Internet. 

 ![Carpeta de archivos de Vysor](media/speech-devices-sdk/qsg-12.png)
 
 ![Carpeta de archivos de Vysor](media/speech-devices-sdk/qsg-13.png)

Para obtener información de desarrollo, consulte la [guía de desarrollo](http://dwn.roo.bo/server_upload/ddk/ROOBO%20Dev%20Kit-User%20Guide.pdf) de Roobo.

Roobo proporciona una herramienta que captura todo el audio en memoria flash, lo cual puede ayudar a solucionar problemas de audio. Se proporciona una versión de la herramienta para cada configuración del kit de desarrollo. Elija el dispositivo en [el sitio de Roobo](http://ddk.roobo.com/) y, a continuación, haga clic en el vínculo **ROOBO Tools**(Herramientas de ROOBO) en la parte inferior de la página.
