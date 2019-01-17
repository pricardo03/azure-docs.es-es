---
title: Conexión de un dispositivo Raspberry Pi a una aplicación de Azure IoT Central (Python) | Microsoft Docs
description: Como desarrollador de dispositivos, aprenderá a conectar Raspberry Pi a su aplicación de Azure IoT Central mediante Python.
author: dominicbetts
ms.author: dobett
ms.date: 01/23/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: timlt
ms.openlocfilehash: 9f39832b50ed983e7d8a0bfc0a06366870717fa3
ms.sourcegitcommit: d61faf71620a6a55dda014a665155f2a5dcd3fa2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/04/2019
ms.locfileid: "54051992"
---
# <a name="connect-a-raspberry-pi-to-your-azure-iot-central-application-python"></a>Conexión de un dispositivo Raspberry Pi a una aplicación de Azure IoT Central (Python)

[!INCLUDE [howto-raspberrypi-selector](../../includes/iot-central-howto-raspberrypi-selector.md)]

En este artículo se describe el modo de conectar, como desarrollador de dispositivos, un dispositivo Raspberry Pi a una aplicación de Microsoft Azure IoT Central con el lenguaje de programación Python.

## <a name="before-you-begin"></a>Antes de empezar

Para completar los pasos descritos en este artículo, necesita los siguientes componentes:

* Una aplicación de Azure IoT Central creada a partir de la plantilla de aplicación **Ejemplo Devkits**. Para más información, consulte la [guía de inicio rápido para crear una aplicación](quick-deploy-iot-central.md).
* Un dispositivo Raspberry Pi que ejecuta el sistema operativo Raspbian. Necesitará conectar un monitor, un teclado y un mouse (ratón) a su Raspberry Pi para acceder al entorno de interfaz gráfica de usuario. Raspberry Pi debe poder [conectarse a Internet](https://www.raspberrypi.org/learning/software-guide/wifi/).
* Opcionalmente, una placa complementaria [Sense Hat](https://www.raspberrypi.org/products/sense-hat/) para el dispositivo Raspberry Pi. Esta placa recopila datos de telemetría de diversos sensores y los envía a la aplicación de Azure IoT Central. Si no tiene una placa **Sense Hat**, puede usar un emulador en su lugar (disponible como parte de la imagen Raspberry Pi).

## <a name="sample-devkits-application"></a>Aplicación **Ejemplo Devkits**

Una aplicación creada a partir de la plantilla de aplicación **Ejemplo Devkits** incluye una plantilla de dispositivo **Raspberry Pi** con las siguientes características: 

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
- Properties (Propiedades)
    - Propiedad Die number (Número de chip) del dispositivo
    - Propiedad Location cloud (ubicación en la nube)

Para obtener detalles completos sobre la configuración de la plantilla de dispositivo, consulte [Detalles de la plantilla de dispositivo Raspberry PI](howto-connect-raspberry-pi-python.md#raspberry-pi-device-template-details).
    

## <a name="add-a-real-device"></a>Adición de un dispositivo real

En su aplicación de Azure IoT Central, agregue un dispositivo real de la plantilla de dispositivo **Raspberry Pi** y realice un seguimiento de los detalles de conexión del dispositivo (**Id. de ámbito, Id. de dispositivo y clave principal**). Para más información, consulte [Add a real device to your Azure IoT Central application](tutorial-add-device.md) (Adición de un dispositivo real a la aplicación de Azure IoT Central).


### <a name="configure-the-raspberry-pi"></a>Configuración del dispositivo Raspberry Pi

En los pasos siguientes se describe cómo descargar y configurar la aplicación de Python de ejemplo desde GitHub. Esta aplicación de ejemplo:

* Envía telemetría y valores de propiedad a Azure IoT Central.
* Responde a los cambios de configuración realizados en Azure IoT Central.

Para configurar el dispositivo, [siga las instrucciones detalladas de GitHub](https://aka.ms/iotcentral-docs-Raspi-releases).


> [!NOTE]
> Para más información sobre el ejemplo de Python de Raspberry Pi, consulte el archivo [Léame](https://aka.ms/iotcentral-docs-Raspi-releases) en GitHub.


1. Una vez configurado, el dispositivo deberá comenzar a enviar datos a Azure IoT Central en un momento.
1. En la aplicación de Azure IoT Central, puede ver cómo el código que se ejecuta en Raspberry Pi interactúa con la aplicación:

    * En la página **Measurements** (Medidas) del dispositivo real, puede ver la telemetría enviada desde Raspberry Pi. Si usa el **emulador de Sense HAT**, puede modificar los valores de telemetría en la interfaz gráfica de usuario de Raspberry Pi.
    * En la página **Properties** (Propiedades), puede ver el valor de la propiedad **Die Number** (Número de chip) notificado.
    * En la página **Settings** (Configuración), puede cambiar distintos parámetros en Raspberry Pi como el voltaje y la velocidad del ventilador. Cuando Raspberry Pi confirma el cambio, el valor se muestra como **synced** (sincronizado) en Azure IoT Central.


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

| Nombre para mostrar | Nombre del campo | Unidades | Posiciones decimales | Mínima | Máxima | Inicial |
| ------------ | ---------- | ----- | -------------- | ------- | ------- | ------- |
| Voltage (Voltaje)      | setVoltage | Voltios | 0              | 0       | 240     | 0       |
| Current      | setCurrent | Amp  | 0              | 0       | 100     | 0       |
| Fan Speed    | fanSpeed   | RPM   | 0              | 0       | 1000    | 0       |

Cambiar configuración

| Nombre para mostrar | Nombre del campo | Texto activado | Texto desactivado | Inicial |
| ------------ | ---------- | ------- | -------- | ------- |
| IR           | activateIR | ACTIVAR      | Apagado      | Off     |

### <a name="properties"></a>Properties (Propiedades)

| Escriba            | Nombre para mostrar | Nombre del campo | Tipo de datos |
| --------------- | ------------ | ---------- | --------- |
| Propiedad de dispositivo | Die number   | dieNumber  | número    |
| Texto            | Ubicación     | location   | N/D       |

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido cómo conectar un dispositivo Raspberry Pi a la aplicación de Azure IoT Central, estos son los siguientes pasos sugeridos:

* [Connect a generic Node.js client application to Azure IoT Central](howto-connect-nodejs.md) (Conexión una aplicación cliente de Node.js a Azure IoT Central)
