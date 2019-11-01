---
title: Conexión de un dispositivo DevKit a una aplicación de Azure IoT Central | Microsoft Docs
description: Como desarrollador de dispositivos, aprenda a conectar un dispositivo MXChip IoT DevKit a una aplicación de Azure IoT Central mediante IoT Plug and Play.
author: liydu
ms.author: liydu
ms.date: 08/17/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: jeffya
ms.openlocfilehash: b7d2e1b08653cb8023ef6a5190ab53ecc3d568a6
ms.sourcegitcommit: 4c3d6c2657ae714f4a042f2c078cf1b0ad20b3a4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2019
ms.locfileid: "72941893"
---
# <a name="connect-an-mxchip-iot-devkit-device-to-your-azure-iot-central-application"></a>Conexión de un dispositivo MXChip IoT DevKit a una aplicación de Microsoft IoT Central

En este artículo se muestra cómo conectar un dispositivo MXChip IoT DevKit (DevKit) a una aplicación de Azure IoT Central. El dispositivo usa el modelo de IoT Plug and Play certificado para que el dispositivo DevKit configure su conexión a IoT Central.

En este artículo paso a paso, puede:

- Obtener los detalles de conexión de la aplicación de IoT Central.
- Preparar el dispositivo y conectarlo a la aplicación de IoT Central.
- Ver la telemetría y las propiedades del dispositivo en IoT Central.

## <a name="prerequisites"></a>Requisitos previos

Para completar los pasos de este artículo, necesitará los siguientes recursos:

1. Un [dispositivo DevKit](https://aka.ms/iot-devkit-purchase).
1. Una aplicación de IoT Central creada a partir de la plantilla de **aplicación de versión preliminar**. Puede seguir los pasos descritos en [Create an IoT Plug and Play application](./quick-deploy-iot-central-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) (Creación de una aplicación de Plug and Play).

## <a name="get-device-connection-details"></a>Obtención de los detalles de conexión del dispositivo

Desde la aplicación de Azure IoT Central, seleccione la pestaña **Administración** y, después, **Conexión de dispositivos**. Anote los valores de **ID Scope** (Ámbito de id.) y **Primary key** (Clave principal).

![Detalles de conexión del grupo de dispositivos](media/howto-connect-devkit-pnp/device-group-connection-details.png)

## <a name="prepare-the-device"></a>Preparar el dispositivo

1. Descargue el [firmware de Azure IoT Central Plug and Play precompilado](https://github.com/MXCHIP/IoTDevKit/raw/master/pnp/iotc_devkit/bin/iotc_devkit.bin) más reciente para el dispositivo DevKit desde GitHub.

1. Conecte el dispositivo DevKit en el equipo de desarrollo con un cable USB. En Windows, se abre una ventana del explorador de archivos en una unidad asignada al almacenamiento en el dispositivo DevKit. Por ejemplo, la unidad podría denominarse **AZ3166 (D:)** .

1. Arrastre el archivo **iotc_devkit.bin** a la ventana de la unidad. Cuando se complete la operación de copia, el dispositivo se reinicia con el nuevo firmware.

    > [!NOTE]
    > Si ve errores en la pantalla, como **No hay Wi-Fi**, esto se debe a que el dispositivo DevKit aún no se ha conectado a la red Wi-Fi.

1. En DevKit, mantenga presionado el **botón B**, presione y suelte el botón de **restablecimiento** y después suelte el **botón B**. El dispositivo ya está en modo de punto de acceso. Para confirmarlo, la pantalla muestra "IoT DevKit - AP" y la dirección IP del portal de configuración.

1. En el equipo o tableta, conéctese al nombre de red Wi-Fi que se muestra en la pantalla del dispositivo. La red WiFi comienza con **AZ-** seguido de la dirección MAC. Cuando se conecta a esta red, no tiene acceso a Internet. Este estado es el esperado y solo se conectará a esta red durante un breve tiempo mientras se configura el dispositivo.

1. Abra el explorador web y vaya a [http://192.168.0.1/](http://192.168.0.1/). Aparece la siguiente página web:

    ![Configuración de la interfaz de usuario](media/howto-connect-devkit-pnp/config-ui.png)

    En la página web, especifique lo siguiente:

    - El nombre de la red Wi-Fi (SSID).
    - La contraseña de la red Wi-Fi.
    - Los detalles de la conexión: el **identificador de dispositivo**, que puede elegir usted mismo, y el **ámbito de id.** y la **clave principal de SAS del grupo** que anotó anteriormente.

    > [!NOTE]
    > Actualmente, el dispositivo DevKit de IoT solo puede conectarse a una red Wi-Fi de 2,4 GHz; 5 GHz no se admite debido a las restricciones de hardware.

1. Elija **Configurar dispositivo** y el dispositivo DevKit se reiniciará y ejecutará la aplicación:

    ![Interfaz de usuario de reinicio](media/howto-connect-devkit-pnp/reboot-ui.png)

    En la pantalla DevKit se muestra una confirmación de que la aplicación se está ejecutando:

    ![DevKit en ejecución](media/howto-connect-devkit-pnp/devkit-running.png)

El DevKit registra primero un nuevo dispositivo en la aplicación de IoT Central y, a continuación, inicia el envío de datos.

## <a name="view-the-telemetry"></a>Visualización de la telemetría

En este paso, verá la telemetría en la aplicación de Azure IoT Central.

En la aplicación de IoT Central, seleccione la pestaña **Dispositivos** y seleccione el dispositivo que ha agregado. En la pestaña **Información general**, puede ver los datos de telemetría del dispositivo DevKit:

   ![Información general del dispositivo IoT Central](media/howto-connect-devkit-pnp/mxchip-overview-page.png)

## <a name="review-the-code"></a>Revisión del código

Para revisar el código, modificarlo y compilarlo, vaya a [Ejemplos de código](https://docs.microsoft.com/samples/azure-samples/mxchip-iot-devkit-pnp/sample/).

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a conectar un dispositivo DevKit a la aplicación Azure IoT Central, le sugerimos como siguiente paso aprender a [configurar una plantilla de dispositivo personalizada](./howto-set-up-template-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) para su dispositivo IoT.
