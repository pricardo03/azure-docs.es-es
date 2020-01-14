---
title: Conexión de un dispositivo Raspberry Pi a una aplicación de Azure IoT Central (Python) | Microsoft Docs
description: Como desarrollador de dispositivos, aprenderá a conectar Raspberry Pi a su aplicación de Azure IoT Central mediante Python.
author: dominicbetts
ms.author: dobett
ms.date: 08/23/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: timlt
ms.openlocfilehash: 3daa567a916bd0abeb407028c7d06bd1f2bd464b
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2019
ms.locfileid: "75454107"
---
# <a name="connect-a-raspberry-pi-to-your-azure-iot-central-application-python"></a>Conexión de un dispositivo Raspberry Pi a una aplicación de Azure IoT Central (Python)

[!INCLUDE [howto-raspberrypi-selector](../../../includes/iot-central-howto-raspberrypi-selector.md)]

[!INCLUDE [iot-central-original-pnp](../../../includes/iot-central-original-pnp-note.md)]

En este artículo se describe el modo de conectar, como desarrollador de dispositivos, un dispositivo Raspberry Pi a una aplicación de Microsoft Azure IoT Central con el lenguaje de programación Python.

## <a name="before-you-begin"></a>Antes de empezar

Para completar los pasos descritos en este artículo, necesita los siguientes componentes:

* Una aplicación de Azure IoT Central creada a partir de la plantilla de la **Aplicación heredada**. Para más información, consulte la [guía de inicio rápido para crear una aplicación](quick-deploy-iot-central.md).
* Un dispositivo Raspberry Pi que ejecuta el sistema operativo Raspbian. Raspberry Pi debe poder conectarse a Internet. Para obtener más información, consulte [Configuración del dispositivo Raspberry Pi](https://projects.raspberrypi.org/en/projects/raspberry-pi-setting-up/3).

> [!TIP]
> Para más información sobre cómo configurar y conectarse a un dispositivo Raspberry Pi, consulte [Get started with Raspberry Pi](https://projects.raspberrypi.org/en/pathways/getting-started-with-raspberry-pi) (Introducción a Raspberry Pi).

## <a name="add-a-device-template"></a>Incorporación de una plantilla de dispositivo

En la aplicación de Azure IoT Central, agregue una nueva plantilla de dispositivo **Raspberry Pi** con las siguientes características:

- Datos de telemetría, lo que incluye las siguientes medidas que recopilará el dispositivo:
  - Humedad
  - Temperatura
  - Presión
  - Magnetómetro (X, Y, Z)
  - Acelerómetro (X, Y, Z)
  - Giroscopio (X, Y, Z)
- Configuración
  - Voltage (Voltaje)
  - Current
  - Fan Speed
  - Conmutador de infrarrojos.
- Propiedades
  - Propiedad Die number (Número de chip) del dispositivo
  - Propiedad Location cloud (ubicación en la nube)

1. Seleccione **+Nuevo** desde ![Plantilla de dispositivo](media/howto-connect-raspberry-pi-python/adddevicetemplate.png) en las plantillas de dispositivo
   

2. Seleccione **Raspberry Pi** y cree la plantilla de dispositivo Raspberry Pi ![Agregar plantilla de dispositivo](media/howto-connect-raspberry-pi-python/newdevicetemplate.png)

Para obtener detalles completos sobre la configuración de la plantilla de dispositivo, consulte los [detalles de la plantilla de dispositivo Raspberry Pi](howto-connect-raspberry-pi-python.md#raspberry-pi-device-template-details).

## <a name="add-a-real-device"></a>Adición de un dispositivo real

En su aplicación de Azure IoT Central, agregue un dispositivo real desde la plantilla de dispositivo **Raspberry Pi**. Anote los detalles de conexión del dispositivo (**Id. de ámbito**, **Id. de dispositivo** y **Clave principal**). Para más información, consulte [Add a real device to your Azure IoT Central application](tutorial-add-device.md) (Adición de un dispositivo real a la aplicación de Azure IoT Central).

### <a name="configure-the-raspberry-pi"></a>Configuración del dispositivo Raspberry Pi

En los pasos siguientes se describe cómo descargar y configurar la aplicación de Python de ejemplo desde GitHub. Esta aplicación de ejemplo:

* Envía telemetría y valores de propiedad a Azure IoT Central.
* Responde a los cambios de configuración realizados en Azure IoT Central.

1. Conéctese a un entorno de shell en Raspberry Pi, ya sea desde el escritorio de Raspberry Pi o de forma remota mediante SSH.

1. Ejecute el siguiente comando para instalar el cliente de Python de IoT Central:

    ```bash
    pip install iotc
    ```

1. Descargue el código de ejemplo de Python:

    ```bash
    curl -O https://raw.githubusercontent.com/Azure/iot-central-firmware/master/RaspberryPi/app.py
    ```

1. Edite el archivo `app.py` que ha descargado y reemplace los marcadores de posición `DEVICE_ID`, `SCOPE_ID` y `PRIMARY/SECONDARY device KEY` por los valores de conexión que ha anotado anteriormente. Guarde los cambios.

    > [!TIP]
    > En el dispositivo Raspberry Pi, puede usar los editores de texto **nano** o **vi**.

1. Use el siguiente comando para ejecutar el ejemplo:

    ```bash
    python app.py
    ```

    Raspberry Pi comienza a enviar las medidas de telemetría a Azure IoT Central.

1. En la aplicación de Azure IoT Central, puede ver cómo el código que se ejecuta en Raspberry Pi interactúa con la aplicación:

    * En la página **Measurements** (Medidas) del dispositivo real, puede ver la telemetría enviada desde Raspberry Pi.
    * En la página **Properties** (Propiedades), puede ver la propiedad del dispositivo **Die Number** (Número de chip) notificado.
    * En la página **Configuración**, puede cambiar las opciones de configuración del dispositivo Raspberry Pi, como el voltaje y la velocidad del ventilador. Cuando el dispositivo Raspberry Pi confirma el cambio, el valor se muestra como **sincronizado** en Azure IoT Central.

## <a name="raspberry-pi-device-template-details"></a>Detalles de la plantilla de dispositivo Raspberry PI

Una aplicación creada a partir de la plantilla de aplicación **Ejemplo Devkits** incluye una plantilla de dispositivo **Raspberry Pi** con las siguientes características:

### <a name="telemetry-measurements"></a>Medidas de telemetría

| Nombre del campo     | Unidades  | Mínima | Máxima | Posiciones decimales |
| -------------- | ------ | ------- | ------- | -------------- |
| humedad       | %      | 0       | 100     | 0              |
| temp           | °C     | -40     | 120     | 0              |
| presión       | hPa    | 260     | 1260    | 0              |
| magnetometerX (magnetómetro X)  | mgauss | -1000   | 1000    | 0              |
| magnetometerY (magnetómetro Y)  | mgauss | -1000   | 1000    | 0              |
| magnetometerZ (magnetómetro Z)  | mgauss | -1000   | 1000    | 0              |
| accelerometerX (acelerómetro X) | mg     | -2000   | 2000    | 0              |
| accelerometerY (acelerómetro Y) | mg     | -2000   | 2000    | 0              |
| accelerometerZ (acelerómetro Z) | mg     | -2000   | 2000    | 0              |
| gyroscopeX (giróscopo X)     | mdps   | -2000   | 2000    | 0              |
| gyroscopeY (giróscopo Y)     | mdps   | -2000   | 2000    | 0              |
| gyroscopeZ (giróscopo Z)     | mdps   | -2000   | 2000    | 0              |

### <a name="settings"></a>Configuración

Valores numéricos

| Nombre para mostrar | Nombre del campo | Unidades | Posiciones decimales | Mínima | Máxima | Initial |
| ------------ | ---------- | ----- | -------------- | ------- | ------- | ------- |
| Voltage (Voltaje)      | setVoltage | Voltios | 0              | 0       | 240     | 0       |
| Current      | setCurrent | Amp  | 0              | 0       | 100     | 0       |
| Fan Speed    | fanSpeed   | RPM   | 0              | 0       | 1000    | 0       |

Cambiar configuración

| Nombre para mostrar | Nombre del campo | Texto activado | Texto desactivado | Initial |
| ------------ | ---------- | ------- | -------- | ------- |
| IR           | activateIR | ACTIVAR      | Apagado      | Off     |

### <a name="properties"></a>Propiedades

| Tipo            | Nombre para mostrar | Nombre del campo | Tipo de datos |
| --------------- | ------------ | ---------- | --------- |
| Propiedad de dispositivo | Die number   | dieNumber  | number    |
| Texto            | Location     | ubicación   | N/D       |

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a conectar un dispositivo Raspberry Pi a la aplicación Azure IoT Central, le sugerimos como siguiente paso aprender a [configurar una plantilla de dispositivo personalizada](howto-set-up-template.md) para su dispositivo IoT.
