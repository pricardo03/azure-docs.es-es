---
title: Conexión de un dispositivo DevKit a una aplicación de Azure IoT Central | Microsoft Docs
description: Como desarrollador de dispositivos, aprenda a conectar un dispositivo MXChip IoT DevKit a una aplicación de Azure IoT Central mediante IoT Plug and Play.
author: liydu
ms.author: liydu
ms.date: 12/03/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: jeffya
ms.openlocfilehash: 9ba68e90b5ac64c7bdb08af2b567f00c1a076808
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2019
ms.locfileid: "75453929"
---
# <a name="connect-an-mxchip-iot-devkit-device-to-your-azure-iot-central-application-preview-features"></a>Conexión de un dispositivo MXChip IoT DevKit a una aplicación de Azure IoT Central (Características en vista previa (GB))

En este artículo se muestra cómo conectar un dispositivo MXChip IoT DevKit (DevKit) a una aplicación de Azure IoT Central. El dispositivo usa el modelo de IoT Plug and Play certificado para que el dispositivo DevKit configure su conexión a IoT Central.

En este artículo paso a paso, puede:

- Obtener los detalles de conexión de la aplicación de IoT Central.
- Preparar el dispositivo y conectarlo a la aplicación de IoT Central.
- Ver la telemetría y las propiedades del dispositivo en IoT Central.

## <a name="prerequisites"></a>Prerequisites

Para completar los pasos de este artículo, necesitará los siguientes recursos:

- Un [dispositivo DevKit](https://aka.ms/iot-devkit-purchase).
- Una aplicación de IoT Central creada a partir de la plantilla de **aplicación de versión preliminar**. Puede seguir los pasos descritos en [Create an IoT Plug and Play application](./quick-deploy-iot-central.md) (Creación de una aplicación de Plug and Play).

## <a name="get-device-connection-details"></a>Obtención de los detalles de conexión del dispositivo

1. Desde la aplicación de Azure IoT Central, seleccione la pestaña **Plantillas de dispositivo** y, después, **Nueva**. En la sección para **usar una plantilla de dispositivo preconfigurada**, seleccione **MXChip IoT DevKit** en la lista. A continuación, seleccione **Siguiente: Personalizar** y **Crear**.

    ![Plantilla de dispositivo para MXChip IoT DevKit](media/howto-connect-devkit/device-template.png)

1. Seleccione la pestaña **Dispositivos**. En la lista de dispositivos, seleccione **MXChip IoT DevKit** y seleccione **Nuevo** para crear un nuevo dispositivo a partir de la plantilla de dispositivo.

    ![Nuevo dispositivo](media/howto-connect-devkit/new-device.png)

1. En la ventana emergente, escriba el **identificador de dispositivo** como `SampleDevKit` y el **nombre de dispositivo** como `MXChip IoT DevKit - Sample`. Asegúrese de que la opción **Simulado** esté desactivada. Seleccione **Crear**.

    ![Identificador y nombre de dispositivo](media/howto-connect-devkit/device-id-name.png)

1. Haga clic en el dispositivo que acaba de crear y seleccione **Conectar**. Anote el **ámbito del identificador**, el **Identificador de dispositivo** y la **clave principal**.

    ![Información de conexión del dispositivo](media/howto-connect-devkit/device-connection-info.png)

## <a name="prepare-the-device"></a>Preparar el dispositivo

1. Descargue el [firmware de Azure IoT Central Plug and Play precompilado](https://github.com/Azure-Samples/mxchip-iot-devkit-pnp/raw/master/bin/iotc_devkit.bin) más reciente para el dispositivo DevKit desde GitHub.

1. Conecte el dispositivo DevKit en el equipo de desarrollo con un cable USB. En Windows, se abre una ventana del explorador de archivos en una unidad asignada al almacenamiento en el dispositivo DevKit. Por ejemplo, la unidad podría denominarse **AZ3166 (D:)** .

1. Arrastre el archivo **iotc_devkit.bin** a la ventana de la unidad. Cuando se complete la operación de copia, el dispositivo se reinicia con el nuevo firmware.

    > [!NOTE]
    > Si ve errores en la pantalla, como **No hay Wi-Fi**, esto se debe a que el dispositivo DevKit aún no se ha conectado a la red Wi-Fi.

1. En DevKit, mantenga presionado el **botón B**, presione y suelte el botón de **restablecimiento** y después suelte el **botón B**. El dispositivo ya está en modo de punto de acceso. Para confirmarlo, la pantalla muestra "IoT DevKit - AP" y la dirección IP del portal de configuración.

1. En el equipo o tableta, conéctese al nombre de red Wi-Fi que se muestra en la pantalla del dispositivo. La red WiFi comienza con **AZ-** seguido de la dirección MAC. Cuando se conecta a esta red, no tiene acceso a Internet. Este estado es el esperado y solo se conectará a esta red durante un breve tiempo mientras se configura el dispositivo.

1. Abra el explorador web y vaya a [http://192.168.0.1/](http://192.168.0.1/). Aparece la siguiente página web:

    ![Configuración de la interfaz de usuario](media/howto-connect-devkit/config-ui.png)

    En la página web, especifique lo siguiente:

    - El nombre de la red Wi-Fi (SSID).
    - La contraseña de la red Wi-Fi.
    - Los detalles de la conexión: escriba el **identificador de dispositivo**, el **ámbito del identificador** y la **clave principal de SAS** que anotó anteriormente.

    > [!NOTE]
    > Actualmente, el dispositivo DevKit de IoT solo puede conectarse a una red Wi-Fi de 2,4 GHz; 5 GHz no se admite debido a las restricciones de hardware.

1. Elija **Configurar dispositivo** y el dispositivo DevKit se reiniciará y ejecutará la aplicación:

    ![Interfaz de usuario de reinicio](media/howto-connect-devkit/reboot-ui.png)

    En la pantalla DevKit se muestra una confirmación de que la aplicación se está ejecutando:

    ![DevKit en ejecución](media/howto-connect-devkit/devkit-running.png)

El DevKit registra primero un nuevo dispositivo en la aplicación de IoT Central y, a continuación, inicia el envío de datos.

## <a name="view-the-telemetry"></a>Visualización de la telemetría

En este paso, verá la telemetría en la aplicación de Azure IoT Central.

En la aplicación de IoT Central, seleccione la pestaña **Dispositivos** y seleccione el dispositivo que ha agregado. En la pestaña **Información general**, puede ver los datos de telemetría del dispositivo DevKit:

![Información general del dispositivo IoT Central](media/howto-connect-devkit/mxchip-overview-page.png)

## <a name="review-the-code"></a>Revisión del código

Para revisar el código, modificarlo y compilarlo, vaya a [Ejemplos de código](https://docs.microsoft.com/samples/azure-samples/mxchip-iot-devkit-pnp/sample/).

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a conectar un dispositivo DevKit a la aplicación Azure IoT Central, le sugerimos como siguiente paso aprender a [configurar una plantilla de dispositivo personalizada](./howto-set-up-template.md) para su dispositivo IoT.
