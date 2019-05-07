---
title: Conectar un dispositivo SensorTile.box a una aplicación de Azure IoT Central | Microsoft Docs
description: Como desarrollador del dispositivo, obtenga información sobre cómo conectar un dispositivo SensorTile.box a una aplicación de Azure IoT Central.
author: sarahhubbard
ms.author: sahubbar
ms.date: 04/24/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: sandeep.pujar
ms.openlocfilehash: 580a8baa19e8ed4fc3f4449ead9d8aedbc4c039a
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65160911"
---
# <a name="connect-sensortilebox-device-to-your-azure-iot-central-application"></a>Conectar dispositivos SensorTile.box a una aplicación de Azure IoT Central

Este artículo se describe cómo los desarrolladores del dispositivo, conectar un dispositivo SensorTile.box a una aplicación de Microsoft Azure IoT Central.

## <a name="before-you-begin"></a>Antes de empezar

Para completar los pasos descritos en este artículo, necesita los siguientes recursos:

* Un dispositivo SensorTile.box, consulte [SensorTile.box](https://www.st.com/content/st_com/en/products/evaluation-tools/SensorTile.box) para obtener más información.
* La aplicación de ST BLE Sensor instalada en el dispositivo Android, que puede [descargarlo desde aquí] (https://play.google.com/store/apps/details?id=com.st.bluems). Para obtener más información, visite: ([ST BLE Sensor]http://www.st.com/stblesensor)
* Una aplicación de Azure IoT Central creada a partir de la **DevKits** plantilla de aplicación. Para más información, consulte la [guía de inicio rápido para crear una aplicación](quick-deploy-iot-central.md).
* Agregar el **SensorTile.box** plantilla de dispositivo en la aplicación IoT Central visitando el **las plantillas de dispositivo** página, haga clic en **+ nuevo**y seleccionando el **SensorTile** plantilla.

### <a name="get-your-device-connection-details"></a>Obtener detalles de conexión de dispositivo

En la aplicación de Azure IoT Central, agregue un dispositivo real desde la **SensorTile.box** plantilla de dispositivo y tome nota de los detalles de conexión del dispositivo: **Definir el ámbito de ID, Id. de dispositivo y clave principal**:

1. Agregar un dispositivo de Device Explorer. Seleccione **+ nuevo > Real** para agregar un dispositivo real.

    * Escriba una minúscula **Id. de dispositivo**, o usar el sugerido **Id. de dispositivo**.
    * Escriba un **nombre de dispositivo**, o utilice el nombre sugerido

    ![Agregar dispositivo](media/howto-connect-sensortile/real-device.png)

1. Para obtener detalles de conexión del dispositivo **Id. de ámbito**, **Id. de dispositivo**, y **clave principal**, seleccione **Connect** en la página del dispositivo.

    ![Detalles de conexión](media/howto-connect-sensortile/connect-device.png)

1. Tome nota de los detalles de conexión. Se ha desconectado temporalmente desde internet al preparar el dispositivo de DevKit en el paso siguiente.

## <a name="set-up-the-sensortilebox-with-the-mobile-application"></a>Configurar la SensorTile.box con la aplicación móvil

En esta sección, obtendrá información sobre cómo insertar el firmware de la aplicación en el dispositivo y enviar los datos del dispositivo a IoT Central a través de la aplicación móvil ST BLE Sensor por medio de la conectividad de Bluetooth Low Energy (BLE).
1. Abra la aplicación ST BLE Sensor y presione la **crear una nueva aplicación** botón.

    ![Creación de una aplicación](media/howto-connect-sensortile/create-app.png)

1. Seleccione el **barómetro** aplicación.
1. Presione el botón cargar.

    ![Carga barómetro](media/howto-connect-sensortile/barometer-upload.png)

1. Presione el botón play asociado con su SensorTile.box.
1. Una vez completado el proceso, el SensorTile.box transmitirá la temperatura, presión y humedad a través de BLE.

## <a name="connect-the-sensortilebox-to-the-cloud"></a>Conecte el SensorTile.box a la nube

En esta sección obtendrá información sobre cómo conectarse a la SensorTile.box a la aplicación móvil y conecta la aplicación móvil en la nube.
1. Mediante el menú izquierdo, seleccione el **registro en la nube** botón.

    ![Registro en la nube](media/howto-connect-sensortile/cloud-logging.png)

1. Seleccione **Azure IoT Central** como el proveedor de nube.
1. Insertar el Id. de dispositivo y el identificador de ámbito que se han indicado anteriormente.

    ![Credenciales](media/howto-connect-sensortile/credentials.png)

1. Seleccione el **clave de aplicación** botón de radio.
1. Haga clic en **Connect** y seleccione la que desea cargar los datos de telemetría.
1. Después de unos segundos, los datos aparecerán en el panel de la aplicación IoT Central.

## <a name="sensortilebox-device-template-details"></a>Detalles de la plantilla SensorTile.box dispositivo

Una aplicación que se crea a partir de la plantilla de dispositivo SensorTile.box con las siguientes características:

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

Ahora que ha aprendido cómo conectar un SensorTile.box a una aplicación de Azure IoT Central, el siguiente paso sugerido es obtener información sobre cómo [configurar una plantilla personalizada del dispositivo](howto-set-up-template.md) para su propio dispositivo de IoT.
