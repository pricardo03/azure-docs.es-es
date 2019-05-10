---
title: Kit de desarrollo de Audio de voz dispositivos SDK Roobo inteligente v1 - servicios de voz
titleSuffix: Azure Cognitive Services
description: Requisitos previos e instrucciones para comenzar con el SDK de dispositivos de voz, Roobo inteligente Audio Dev Kit v1.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: erhopf
ms.openlocfilehash: 7c1a13a44d9db8ed029ce798f0bb34944a1a65a7
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2019
ms.locfileid: "65409048"
---
# <a name="device-roobo-smart-audio-dev-kit"></a>Dispositivo: Kit de desarrollo de Audio Roobo inteligente

En este artículo se proporciona información específica de dispositivo para el Kit de Dev Roobo Audio inteligente.

## <a name="set-up-the-development-kit"></a>Configuración del kit de desarrollo

1. El kit de desarrollo tiene dos conectores micro USB. El conector de la izquierda sirve para conectar el kit de desarrollo y aparece resaltado como Power (Conectar) en la siguiente imagen. El de la derecha sirve para controlarlo y aparece marcado como Debug (Depurar) en la imagen.

    ![Conectar el kit de desarrollo](media/speech-devices-sdk/qsg-1.png)

1. Conecte el kit de desarrollo mediante un cable micro USB para conectar el puerto de alimentación a un equipo o adaptador de corriente. Se iluminará un indicador de encendido verde en el panel superior.

1. Para controlar el kit de desarrollo, conecte el puerto de depuración en un equipo mediante el uso de un segundo micro cable USB. Es fundamental usar un cable de alta calidad para garantizar unas comunicaciones de confianza.

1. Oriente el kit de desarrollo para realizar la configuración circular o lineal.

    |Configuración del kit de desarrollo|Orientación|
    |-----------------------------|------------|
    |Circular|Vertical, con los micrófonos orientados hacia el techo|
    |Lineal|En el lateral, con los micrófonos mirando hacia usted (tal como se muestra en la siguiente imagen).|

    ![Orientación lineal del kit de desarrollo](media/speech-devices-sdk/qsg-2.png)

1. Instalar los certificados y establezca los permisos de dispositivo de sonido. Escriba los siguientes comandos en una ventana del símbolo del sistema:

   ```powershell
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
    > Si ve un error `no devices/emulators found` , a continuación, compruebe el cable USB está conectado y es un cable de alta calidad. Puede usar `adb devices` para comprobar que el equipo puede comunicarse con el kit de desarrollo ya que este devolverá una lista de dispositivos.
    >
    > [!TIP]
    > Silencie el micrófono y el altavoz del equipo para asegurarse de que está trabajando con los micrófonos del kit de desarrollo. De esta manera, no activará accidentalmente el dispositivo con el audio del equipo.

1. Si quiere asociar un altavoz al kit de desarrollo, puede conectarlo a la salida de línea de audio. Debe elegir un orador de buena calidad con un enchufe analógica 3,5 mm.

    ![Audio de Vysor](media/speech-devices-sdk/qsg-14.png)

## <a name="development-information"></a>Información de desarrollo

Para obtener más información de desarrollo, consulte el [Guía de desarrollo Roobo](http://dwn.roo.bo/server_upload/ddk/ROOBO%20Dev%20Kit-User%20Guide.pdf).

## <a name="audio"></a>Audio

Roobo proporciona una herramienta que captura todo el audio memoria Flash. Esto puede serle de utilidad para ayudarle a solucionar problemas de audio. Se proporciona una versión de la herramienta para cada configuración del kit de desarrollo. En el [Roobo sitio](https://ddk.roobo.com/), seleccione el dispositivo y, a continuación, seleccione el **Roobo herramientas** vínculo en la parte inferior de la página.

## <a name="next-steps"></a>Pasos siguientes

* [Ejecute la aplicación de ejemplo de Android](speech-devices-sdk-android-quickstart.md)
