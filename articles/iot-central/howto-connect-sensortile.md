---
title: Conectar un dispositivo SensorTile.box a una aplicación de Azure IoT Central | Microsoft Docs
description: Como desarrollador de dispositivos, aprenda a conectar un dispositivo SensorTile.box a una aplicación de Azure IoT Central.
author: sarahhubbard
ms.author: sahubbar
ms.date: 04/24/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: sandeep.pujar
ms.openlocfilehash: ce0c5abe6e89094623c07afa2d1c85903e0e7ee7
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/21/2019
ms.locfileid: "69877448"
---
# <a name="connect-sensortilebox-device-to-your-azure-iot-central-application"></a>Conectar un dispositivo SensorTile.box a una aplicación de Azure IoT Central

[!INCLUDE [iot-central-original-pnp](../../includes/iot-central-original-pnp-note.md)]

En este artículo se describe cómo conectar, en tanto que desarrollador de dispositivos, un dispositivo SensorTile.box a una aplicación de Microsoft Azure IoT Central.

## <a name="before-you-begin"></a>Antes de empezar

Para completar los pasos de este artículo, necesitará los siguientes recursos:

* Un dispositivo SensorTile.box. Para obtener más información, consulte [SensorTile.box](https://www.st.com/content/st_com/en/products/evaluation-tools/product-evaluation-tools/mems-motion-sensor-eval-boards/steval-mksbox1v1.html).
* La aplicación ST BLE Sensor instalada en el dispositivo Android, que puede [descargar desde aquí](https://play.google.com/store/apps/details?id=com.st.bluems). Para obtener más información, visite: [ST BLE Sensor](https://www.st.com/stblesensor)
* Una aplicación de Azure IoT Central creada a partir de la plantilla de aplicación **Devkits**. Para más información, consulte la [guía de inicio rápido para crear una aplicación](quick-deploy-iot-central.md).
* Para agregar la plantilla de dispositivo **SensorTile.box** a la aplicación de IoT Central, visite la página **Plantillas de dispositivo**, haga clic en **+ Nueva** y seleccione la plantilla **SensorTile.box**.

### <a name="get-your-device-connection-details"></a>Obtener los detalles de conexión del dispositivo

En su aplicación de Azure IoT Central, agregue un dispositivo real desde la plantilla de dispositivo **SensorTile.box** y anote los detalles de conexión del dispositivo: **Id. de ámbito**, **Id. de dispositivo** y **clave principal**:

1. Agregue un dispositivo desde el Explorador de dispositivos. Seleccione **+ Nuevo > Real** para agregar un dispositivo real.

    * Escriba un **Id. de dispositivo** en minúsculas o use el **Id. de dispositivo** sugerido.
    * Especifique un **nombre de dispositivo** o use el nombre sugerido.

    ![Agregar dispositivo](media/howto-connect-sensortile/real-device.png)

1. Para obtener los detalles de conexión del dispositivo, el **id. de ámbito**, **id. de dispositivo** y la **clave principal**, seleccione **Conectar** en la página del dispositivo.

    ![Detalles de conexión](media/howto-connect-sensortile/connect-device.png)

1. Tome nota de los detalles de conexión. Temporalmente se le desconectará de Internet al preparar el dispositivo DevKit en el siguiente paso.

## <a name="set-up-the-sensortilebox-with-the-mobile-application"></a>Configurar el dispositivo SensorTile.box con la aplicación móvil

En esta sección, obtendrá información sobre cómo insertar el firmware de la aplicación en el dispositivo. A continuación, aprenderá a enviar los datos del dispositivo a IoT Central a través de la aplicación móvil ST BLE Sensor mediante la conectividad de Bluetooth Low Energy (BLE).

1. Abra la aplicación ST BLE Sensor y presione el botón **Crear una nueva aplicación**.

    ![Creación de una aplicación](media/howto-connect-sensortile/create-app.png)

1. Seleccione la aplicación **Barómetro**.
1. Presione el botón Cargar.

    ![Carga del barómetro](media/howto-connect-sensortile/barometer-upload.png)

1. Presione el botón de reproducción asociado con SensorTile.box.
1. Una vez completado el proceso, SensorTile.box transmite la temperatura, la presión y la humedad a través de BLE.

## <a name="connect-the-sensortilebox-to-the-cloud"></a>Conectar SensorTile.box a la nube

En esta sección obtendrá información sobre cómo conectar SensorTile.box a la aplicación móvil y cómo conectar la aplicación móvil a la nube.

1. Mediante el menú izquierdo, seleccione el botón **Registro en la nube**.

    ![Registro en la nube](media/howto-connect-sensortile/cloud-logging.png)

1. Seleccione **Azure IoT Central** como proveedor de nube.
1. Inserte el id. de dispositivo y el id. de ámbito que anotó anteriormente.

    ![Credenciales](media/howto-connect-sensortile/credentials.png)

1. Seleccione el botón de radio **Clave de aplicación**.
1. Haga clic en **Conectar** y seleccione los datos de telemetría que desea cargar.
1. Al cabo de unos segundos, los datos aparecen en el panel de la aplicación IoT Central.

## <a name="sensortilebox-device-template-details"></a>Detalles de la plantilla de dispositivo SensorTile.box

Una aplicación creada a partir de la plantilla de dispositivo SensorTile.box con las siguientes características:

### <a name="telemetry"></a>Telemetría

| Nombre del campo     | Unidades  | Mínima | Máxima | Posiciones decimales |
| -------------- | ------ | ------- | ------- | -------------- |
| humedad       | %      | 30       | 90     | 1              |
| temp           | °C     | 0     | 40     | 1              |
| presión       | mbar    | 900     | 1100    | 2              |
| magnetometerX (magnetómetro X)  | mgauss | -1000   | 1000    | 0              |
| magnetometerY (magnetómetro Y)  | mgauss | -1000   | 1000    | 0              |
| magnetometerZ (magnetómetro Z)  | mgauss | -1000   | 1000    | 0              |
| accelerometerX (acelerómetro X) | mg     | -2000   | 2000    | 0              |
| accelerometerY (acelerómetro Y) | mg     | -2000   | 2000    | 0              |
| accelerometerZ (acelerómetro Z) | mg     | -2000   | 2000    | 0              |
| gyroscopeX (giróscopo X)     | dps   | -3276   | 3276    | 1              |
| gyroscopeY (giróscopo Y)     | dps   | -3276   | 3276    | 1              |
| gyroscopeZ (giróscopo Z)     | dps   | -3276   | 3276    | 1              |
| FFT_X     |    |    |     |               |
| FFT_Y     |    |    |     |               |
| FFT_Z     |    |    |     |               |

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a conectar SensorTile.box con la aplicación Azure IoT Central, le sugerimos como siguiente paso aprender a [configurar una plantilla de dispositivo personalizada](howto-set-up-template.md) para su dispositivo de IoT.
