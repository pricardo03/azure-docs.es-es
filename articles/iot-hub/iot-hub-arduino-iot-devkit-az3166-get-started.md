---
title: 'IoT DevKit en la nube: conexión de IoT DevKit AZ3166 a Azure IoT Hub | Microsoft Docs'
description: Con este tutorial aprenderá a configurar y conectar IoT DevKit AZ3166 a Azure IoT Hub para que envíe datos a la plataforma en la nube de Azure.
author: wesmc7777
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 12/19/2018
ms.author: wesmc
ms.openlocfilehash: f7eea6e9fdbe69003a93e34f9107d279f2105d7f
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/21/2018
ms.locfileid: "53720346"
---
# <a name="connect-iot-devkit-az3166-to-azure-iot-hub"></a>Conexión de IoT DevKit AZ3166 a Azure IoT Hub

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

Se puede usar [MXChip IoT DevKit](https://microsoft.github.io/azure-iot-developer-kit/) para desarrollar y crear prototipos de soluciones de Internet de las cosas (IoT) que aprovechen los servicios de Microsoft Azure. Incluye una placa compatible con Arduino con periféricos y sensores avanzados, un paquete de placa de código abierto y un [catálogo de proyectos](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/) en aumento.

## <a name="what-you-do"></a>Qué debe hacer

Conecte el DevKit a una instancia de Azure IoT Hub que haya creado. A continuación, recopile los datos de temperatura y humedad de los sensores para enviarlos al centro de IoT.

¿Aún no tiene un DevKit? Pruebe el [simulador de DevKit](https://azure-samples.github.io/iot-devkit-web-simulator/) o [compre un DevKit](https://aka.ms/iot-devkit-purchase).

## <a name="what-you-learn"></a>Conocimientos que adquirirá

* Cómo conectar el IoT DevKit a un punto de acceso inalámbrico y preparar el entorno de desarrollo.
* Cómo crear un centro de IoT y registrar un dispositivo para el MXChip IoT DevKit.
* Cómo recopilar datos del sensor mediante la ejecución de una aplicación de ejemplo en el MXChip IoT DevKit.
* Cómo enviar los datos del sensor al centro de IoT.

## <a name="what-you-need"></a>Lo que necesita

* Una placa MXChip IoT DevKit con un cable microUSB. [Obténgalo ahora](https://aka.ms/iot-devkit-purchase).
* Un equipo con Windows 10 o macOS 10.10 o versiones posteriores.
* Una suscripción de Azure activa. [Active una cuenta de Microsoft Azure de prueba de 30 días gratis](https://azureinfo.microsoft.com/us-freetrial.html).
  
## <a name="prepare-your-hardware"></a>Preparar el hardware

Conecte el siguiente hardware al equipo:

* Placa DevKit
* Cable microUSB

![Requisitos de hardware](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/hardware.jpg)

Para conectar el DevKit al equipo, siga estos pasos:

1. Conecte el extremo USB en el equipo.

2. Conecte el extremo microUSB al DevKit.

3. El LED verde de alimentación confirma la conexión.

   ![Conexiones de hardware](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/connect.jpg)

## <a name="configure-wi-fi"></a>Configuración de Wi-Fi

Los proyectos de IoT se basan en la conectividad a Internet. Siga estas instrucciones para configurar el DevKit para conectarse a la red Wi-Fi.

### <a name="enter-ap-mode"></a>Entrada al modo AP

Mantenga presionado el botón B, presione y suelte el botón de restablecimiento y suelte el botón B. El DevKit pasa al modo AP para la configuración de la red Wi-Fi. La pantalla muestra el identificador de red SSID del DevKit y la dirección IP del portal de configuración.

![Botón de restablecimiento, botón B y SSID](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/wifi-ap.jpg)

![Definición del modo AP](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/set-ap-mode.gif)

### <a name="connect-to-devkit-ap"></a>Conexión a DevKit AP

Ahora, use otro dispositivo con la red Wi-Fi habilitada (un equipo o teléfono móvil) para conectarse al SSID del DevKit (resaltado en la imagen anterior). Deje la contraseña en blanco.

![Información de red y botón Conectar](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/connect-ssid.png)

### <a name="configure-wi-fi-for-the-devkit"></a>Configuración de la red Wi-Fi para el DevKit

Abra la dirección IP que aparece en la pantalla del DevKit en el explorador del equipo o del teléfono móvil, seleccione la red Wi-Fi a la que quiera que se conecte el DevKit y escriba la contraseña. Seleccione **Conectar**.

![Cuadro de contraseña y botón Conectar](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/wifi-portal.png)

Cuando la conexión se realiza correctamente, el DevKit se reinicia en unos segundos. Verá el nombre de la red Wi-Fi y la dirección IP en la pantalla:

![Nombre de la red Wi-Fi y dirección IP](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/wifi-ip.jpg)

> [!NOTE] 
> La dirección IP mostrada en la imagen puede no coincidir con la dirección IP real asignada que se muestre en la pantalla del DevKit. Es normal, ya que la red Wi-Fi usa DHCP para asignar las direcciones IP dinámicamente.

Una vez configurada la red Wi-Fi, las credenciales se conservarán en el dispositivo para esa conexión, aunque se desconecte el dispositivo. Por ejemplo, si configura el DevKit para la red Wi-Fi del hogar y se lo lleva a la oficina, debe volver a configurar el modo AP (comenzando por el paso "Entrada al modo AP") para conectar el DevKit a la red Wi-Fi de la oficina. 

## <a name="start-using-the-devkit"></a>Empezar a usar el DevKit

La aplicación predeterminada que se ejecuta en el DevKit comprueba la versión más reciente del firmware y muestra algunos datos de diagnóstico del sensor.

### <a name="upgrade-to-the-latest-firmware"></a>Actualización al firmware más reciente

> [!NOTE]
> A partir de la versión 1.1, DevKit permite ST-SAFE en el cargador de arranque. Deberá actualizar el firmware si está ejecutando una versión anterior a la 1.1.

Si necesita una actualización de firmware, la pantalla mostrará las versiones de firmware más reciente y la actual. Para la actualización, siga la guía de [Actualización de firmware](https://microsoft.github.io/azure-iot-developer-kit/docs/firmware-upgrading/).

![Presentación de las versiones de firmware más reciente y actual](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/firmware.jpg)

> [!NOTE]
> Esta tarea solo se realiza una vez. Cuando comience el desarrollo en el DevKit y cargue la aplicación, tendrá el firmware más reciente, que viene con la aplicación.

### <a name="test-various-sensors"></a>Prueba de varios sensores

Presione el botón B para probar los sensores. Continúe presionando y soltando el botón B para recorrer en iteración cada sensor.

![Mostrar botón B y sensor](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/sensors.jpg)

## <a name="prepare-the-development-environment"></a>Preparación del entorno de desarrollo

### <a name="install-azure-iot-tools"></a>Instalación de Azure IoT Tools

Se recomienda el paquete de extensión [Azure IoT Workbench](https://aka.ms/azure-iot-tools) para Visual Studio Code para el desarrollo en DevKit. Azure IoT Tools contiene [Azure IoT Device Workbench](https://aka.ms/iot-workbench) para desarrollar y depurar en varios dispositivos de IoT DevKit y [Azure IoT Hub Toolkit](https://aka.ms/iot-toolkit) para administrar e interactuar con Azure IoT Hub.

Puede ver estos vídeos de [Channel 9](https://channel9.msdn.com/) para obtener información general sobre sus funciones:
* [Introducción a la nueva extensión de IoT Workbench para VS Code](https://channel9.msdn.com/Shows/Internet-of-Things-Show/IoT-Workbench-extension-for-VS-Code)
* [Novedades de la extensión de IoT Toolkit para VS Code](https://channel9.msdn.com/Shows/Internet-of-Things-Show/Whats-new-in-the-IoT-Toolkit-extension-for-VS-Code)

Siga estos pasos para preparar el entorno de desarrollo para DevKit:

1. Instale el [IDE de Arduino](https://www.arduino.cc/en/Main/Software). Este IDE proporciona la cadena de herramientas necesaria para compilar y cargar código de Arduino.
    * **Windows**: use la versión de Windows Installer. No instale desde la tienda de aplicaciones.
    * **macOS**: arrastre y coloque el archivo **Arduino.app** extraído en la carpeta `/Applications`.
    * **Ubuntu**: descomprímalo en una carpeta, por ejemplo `$HOME/Downloads/arduino-1.8.8`.

2. Instale [Visual Studio Code](https://code.visualstudio.com/), un editor de código fuente multiplataforma con eficaces herramientas de desarrollo, como la finalización y depuración de código de IntelliSense.

3. Inicie VS Code, busque **Arduino** en el catálogo de extensiones e instálelo. Esta extensión proporciona experiencias mejoradas para el desarrollo en la plataforma Arduino.
    ![Instalación de Arduino](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/install-arduino.png)

4. Busque **Azure IoT Tools** en el catálogo de extensiones e instálelo.
    ![Instalación de Azure IoT Tools](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/install-azure-iot-tools.png)

5. Configure VS Code con la configuración de Arduino.

    En Visual Studio Code, haga clic en **Archivo > Preferencias > Configuración**. A continuación, haga clic en **...** y **abra settings.json**.
    ![Instalación de Azure IoT Tools](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/user-settings-arduino.png)
    
    Agregue las siguientes líneas para configurar Arduino dependiendo de la plataforma: 

    * **Windows**:
      
        ```json
        "arduino.path": "C:\\Program Files (x86)\\Arduino",
        "arduino.additionalUrls": "https://raw.githubusercontent.com/VSChina/azureiotdevkit_tools/master/package_azureboard_index.json"
        ```

    * **macOS**:

        ```json
        "arduino.path": "/Applications",
        "arduino.additionalUrls": "https://raw.githubusercontent.com/VSChina/azureiotdevkit_tools/master/package_azureboard_index.json"
        ```

    * **Ubuntu**:
    
        Reemplace el marcador **{username}** que aparece a continuación por el nombre de usuario.

        ```json
        "arduino.path": "/home/{username}/Downloads/arduino-1.8.8",
        "arduino.additionalUrls": "https://raw.githubusercontent.com/VSChina/azureiotdevkit_tools/master/package_azureboard_index.json"
        ```

6. Haga clic en `F1` para abrir la paleta de comandos y escriba y seleccione **Arduino: Board Manager**. Busque **AZ3166** e instale la última versión.
    ![Instalación del SDK de DevKit](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/install-az3166-sdk.png)

### <a name="install-st-link-drivers"></a>Instalación de los controladores ST-Link

[ST-Link/V2](http://www.st.com/en/development-tools/st-link-v2.html) es la interfaz USB que usa IoT DevKit para comunicarse con la máquina de desarrollo. Siga los pasos específicos del sistema operativo para permitir que la máquina acceda al dispositivo.

* **Windows**: descargue e instale el controlador USB del [sitio web de STMicroelectronics](http://www.st.com/en/development-tools/stsw-link009.html).
* **macOS**: no se necesita ningún controlador para macOS.
* **Ubuntu**: ejecute lo siguiente en el terminal, cierre la sesión e iníciela de nuevo para que el cambio de grupo surta efecto:
    ```bash
    # Copy the default rules. This grants permission to the group 'plugdev'
    sudo cp ~/.arduino15/packages/AZ3166/tools/openocd/0.10.0/linux/contrib/60-openocd.rules /etc/udev/rules.d/
    sudo udevadm control --reload-rules

    # Add yourself to the group 'plugdev'
    # Logout and log back in for the group to take effect
    sudo usermod -a -G plugdev $(whoami)
    ```

Ahora ya ha terminado con la preparación y configuración de su entorno de desarrollo. Vamos a compilar el ejemplo "Hello World" para IoT: envío de datos de telemetría de temperatura a Azure IoT Hub.

## <a name="build-your-first-project"></a>Creación de su primer proyecto

1. Asegúrese de que IoT DevKit **no está conectado** al equipo. Primero, inicie VS Code y luego conecte DevKit al equipo.


1. Haga clic en `F1` para abrir la paleta de comandos, escriba y seleccione **Azure IoT Device Workbench: Open Examples...** (Abrir ejemplos...). A continuación, seleccione **IoT DevKit** como placa.

1. En la página de ejemplos de IoT Workbench, busque **Get Started** (Inicio) y haga clic en **Open Sample** (Abrir ejemplo). A continuación, seleccione la ruta de acceso predeterminada para descargar el código de ejemplo.
    ![Abrir ejemplo](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/open-sample.png)

1. En la nueva ventana de proyecto que se abre, haga clic en `F1` para abrir la paleta de comandos y escriba y seleccione **Azure IoT Device Workbench: Provision Azure Services...** (Aprovisionar servicios de Azure). Siga la guía paso a paso para finalizar el aprovisionamiento de Azure IoT Hub y crear el dispositivo de IoT Hub.
    ![Aprovisionamiento de nube](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/cloud-provision.png)

1. En la barra de estado de la parte inferior derecha, compruebe que se muestra **MXCHIP AZ3166** como placa seleccionada y que se usa el puerto serie con **STMicroelectronics**.
    ![Selección de la placa y el puerto](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/select-com.png)

1. Haga clic en `F1` para abrir la paleta de comandos, escriba y seleccione **Azure IoT Device Workbench: Configure Device Settings...** (Configuración de dispositivo) y luego **Config Device Connection String > Select IoT Hub Device Connection String** (Configuración de la cadena de conexión de dispositivos > Seleccionar cadena de conexión de dispositivos IoT Hub).

1. En DevKit, mantenga presionado el **botón A**, presione y suelte el botón de **reinicio** y, a continuación, suelte el **botón A**. DevKit entra en modo de configuración y guarda la cadena de conexión.
    ![Cadena de conexión](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/connection-string.png)

1. Haga clic en `F1` de nuevo, escriba y seleccione **Azure IoT Device Workbench: Upload Device Code**  (Carga del código de dispositivo). Se inicia la compilación y la carga del código en DevKit.
    ![Carga de Arduino](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/arduino-upload.png)

El DevKit se reinicia y comienza a ejecutar el código.

> [!NOTE]
> Si hay errores o interrupciones, siempre puede volver a ejecutar el comando.

## <a name="test-the-project"></a>Prueba del proyecto

### <a name="view-the-telemetry-sent-to-azure-iot-hub"></a>Visualización de la telemetría enviada a Azure IoT Hub

Haga clic en el icono de interruptor de alimentación de la barra de estado para abrir Serial Monitor: ![Monitor de serie](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/serial-monitor.png)

La aplicación de ejemplo se ejecuta correctamente si ve los siguientes resultados:

* El monitor serie muestra el mensaje enviado a IoT Hub.
* El LED del MXChip IoT DevKit parpadea.

![Salida del monitor serie](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/result-serial-output.png)

### <a name="view-the-telemetry-received-by-azure-iot-hub"></a>Visualización de la telemetría recibida por Azure IoT Hub

Puede usar [Azure IoT Hub Toolkit](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) (conocido anteriormente como Azure IoT Toolkit) para supervisar los mensajes del dispositivo a la nube (D2C) en IoT Hub.

1. En Visual Studio Code, busque **Azure IoT Hub Toolkit** en el catálogo de extensiones e instálelo.

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) y busque la instancia de IoT Hub que creó.
    ![Azure Portal](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/azure-iot-hub-portal.png)

1. En el panel **Directivas de acceso compartido**, haga clic en la directiva **iothubowner** y, luego, anote la cadena de conexión de su centro de IoT.
    ![Cadena de conexión de Azure IoT Hub](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/azure-portal-conn-string.png)

1. En VS Code, haga clic en `F1`, escriba y seleccione **Azure IoT Hub: Set IoT Hub Connection String** (Establecimiento de la cadena de conexión de IoT Hub). Copie la cadena de conexión ahí.
    ![Establecimiento de la cadena de conexión de IoT Hub](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/set-iothub-connection-string.png)

1. Expanda el panel **AZURE IOT HUB DEVICES** (DISPOSITIVOS DE AZURE IOT HUB) de la derecha, haga clic con el botón derecho en el nombre del dispositivo que ha creado y seleccione **Start Monitoring D2C Message** (Iniciar la supervisión de mensajes D2C).
    ![Supervisión de mensajes D2C](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/monitor-d2c.png)

1. En el panel **OUTPUT** (SALIDA), puede ver los mensajes D2C entrantes en IoT Hub.
    ![Mensajes D2C](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/d2c-output.png)

## <a name="problems-and-feedback"></a>Problemas y comentarios

Si tiene problemas, puede buscar una solución en las [preguntas frecuentes de IoT DevKit](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) o ponerse en contacto con nosotros desde [Gitter](https://gitter.im/Microsoft/azure-iot-developer-kit). Se pueden también enviar comentarios al dejar un comentario en esta página.

## <a name="next-steps"></a>Pasos siguientes

Ha conectado correctamente un MXChip IoT DevKit a IoT Hub y ha enviado los datos del sensor capturados a IoT Hub.

[!INCLUDE [iot-hub-get-started-az3166-next-steps](../../includes/iot-hub-get-started-az3166-next-steps.md)]
