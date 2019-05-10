---
title: Conexión de un dispositivo Windows IoT Core a una aplicación de Azure IoT Central | Microsoft Docs
description: Como desarrollador de dispositivos, aprenda a conectar un dispositivo MXChip IoT DevKit a una aplicación de Azure IoT Central.
author: miriambrus
ms.author: miriamb
ms.date: 04/05/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: e8d4ab46c598580a3a87f4344202f2700926bf5c
ms.sourcegitcommit: 8fc5f676285020379304e3869f01de0653e39466
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/09/2019
ms.locfileid: "65510319"
---
# <a name="connect-a-windows-iot-core-device-to-your-azure-iot-central-application"></a>Conexión de un dispositivo Windows IoT Core a una aplicación de Azure IoT Central

En este artículo se describe cómo conectar, en tanto que desarrollador de dispositivos, un dispositivo Windows IoT Core a una aplicación de Microsoft Azure IoT Central.

## <a name="before-you-begin"></a>Antes de empezar

Necesitará lo siguiente para completar los pasos de este artículo:

- Una aplicación de Azure IoT Central creada a partir de la plantilla de aplicación **Ejemplo Devkits**. Para más información, consulte la [guía de inicio rápido para crear una aplicación](quick-deploy-iot-central.md).

- Un dispositivo en el que se ejecuta el sistema operativo Windows 10 IoT Core. Para obtener más información, consulte [configurando el dispositivo Windows 10 IoT Core](https://docs.microsoft.com/windows/iot-core/tutorials/quickstarter/devicesetup).

- Una máquina de desarrollo con [Node.js](https://nodejs.org/) versión 8.0.0 o posterior instalado. Puede ejecutar `node --version` en la línea de comandos para comprobar la versión. Node.js está disponible para una amplia variedad de sistemas operativos.

## <a name="the-sample-devkits-application"></a>La aplicación de ejemplo Devkits

Una aplicación creada a partir de la plantilla de aplicación **Ejemplo Devkits** incluye una plantilla de dispositivo **Windows IoT Core** con las siguientes características:

- Medidas de telemetría del dispositivo: **Humedad**, **temperatura**, y **presión**.
- Configuración para controlar **velocidad del ventilador**.
- Una propiedad de dispositivo **mueren número** y una propiedad en la nube **ubicación**.

Para obtener detalles sobre la configuración de la plantilla de dispositivo, consulte [detalles de la plantilla de dispositivo de Windows IoT Core](#device-template-details).

## <a name="add-a-real-device"></a>Adición de un dispositivo real

En la aplicación de Azure IoT Central, use la **Device Explorer** página para agregar un dispositivo real desde la **Windows 10 IoT Core** plantilla de dispositivo. Tome nota del dispositivo de los detalles de conexión (**Id. de ámbito**, **Id. de dispositivo**, y **clave principal**). Para obtener más información, consulte [obtener información de conexión](howto-generate-connection-string.md#get-connection-information).

## <a name="prepare-the-device"></a>Preparar el dispositivo

Para que el dispositivo se conecte a IoT Central, necesita una cadena de conexión.

[!INCLUDE [iot-central-howto-connection-string](../../includes/iot-central-howto-connection-string.md)]

Para que el código de dispositivo tener acceso a la cadena de conexión, guárdelo en un archivo denominado **connection.string.iothub** en la carpeta `C:\Data\Users\DefaultAccount\Documents\` en el dispositivo Windows 10 IoT Core.

Para copiar el **connection.string.iothub** archivo desde el equipo de escritorio a la `C:\Data\Users\DefaultAccount\Documents\` carpeta en el dispositivo, puede usar el [Windows Device Portal](https://docs.microsoft.com/windows/iot-core/manage-your-device/deviceportal):

1. Usar el explorador web para navegar a la de Windows Device Portal en el dispositivo.
1. Para examinar los archivos en el dispositivo, elija **aplicaciones > Explorador de archivos**.
1. Vaya a **Folders\Documents usuario**. A continuación, cargue el **connection.string.iothub** archivo:

    ![Cargar la cadena de conexión](media/howto-connect-windowsiotcore/device-portal.png)

## <a name="deploy-and-run"></a>Implementación y ejecución

Para implementar y ejecutar la aplicación de ejemplo en el dispositivo, puede usar el [Windows Device Portal](https://docs.microsoft.com/windows/iot-core/manage-your-device/deviceportal):

1. Usar el explorador web para navegar a la de Windows Device Portal en el dispositivo.
1. Para implementar y ejecutar el **cliente de Azure IoT Hub** aplicación, elija **aplicaciones > ejemplos de rápida ejecución**. A continuación, elija **cliente de Azure IoT Hub**.
1. A continuación, elija **implementar y ejecutar**.

    ![Implementación y ejecución](media/howto-connect-windowsiotcore/quick-run.png)

Después de unos minutos, puede ver la telemetría del dispositivo en la aplicación IoT Central.

El [Windows Device Portal](https://docs.microsoft.com/windows/iot-core/manage-your-device/deviceportal) incluye herramientas que puede usar para solucionar problemas del dispositivo:

- El **Administrador de aplicaciones** página le permite controlar las aplicaciones que se ejecutan en el dispositivo.
- Si no tiene un monitor conectado al dispositivo, puede usar el **configuración del dispositivo** página para realizar capturas de pantalla del dispositivo. Por ejemplo:

    ![Captura de pantalla de aplicación](media/howto-connect-windowsiotcore/iot-hub-foreground-client.png)

## <a name="download-the-source-code"></a>Descarga del código fuente

Si desea explorar y modificar el código fuente de la aplicación cliente, puede descargarlo desde el [repositorio de GitHub de ejemplos de Windows-iotcore](https://github.com/Microsoft/Windows-iotcore-samples/blob/master/Samples/Azure/IoTHubClients).

## <a name="device-template-details"></a>Detalles de la plantilla de dispositivo

Una aplicación creada a partir de la plantilla de aplicación **Ejemplo Devkits** incluye una plantilla de dispositivo **Windows IoT Core** con las siguientes características:

### <a name="telemetry-measurements"></a>Medidas de telemetría

| Nombre del campo     | Unidades  | Mínimo | Máximo | Posiciones decimales |
| -------------- | ------ | ------- | ------- | -------------- |
| humedad       | %      | 0       | 100     | 0              |
| temp           | °C     | -40     | 120     | 0              |
| presión       | hPa    | 260     | 1260    | 0              |

### <a name="settings"></a>Configuración

Valores numéricos

| `Display name` | Nombre del campo | Unidades | Posiciones decimales | Mínimo | Máximo | Inicial |
| ------------ | ---------- | ----- | -------------- | ------- | ------- | ------- |
| Fan Speed    | fanSpeed   | RPM   | 0              | 0       | 1000    | 0       |

### <a name="properties"></a>Properties (Propiedades)

| Type            | `Display name` | Nombre del campo | Tipo de datos |
| --------------- | ------------ | ---------- | --------- |
| Propiedad de dispositivo | Die number   | dieNumber  | de serie    |
| Text            | Ubicación     | ubicación   | N/D       |

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido cómo conectar un dispositivo Windows IoT Core a una aplicación de Azure IoT Central, el siguiente paso sugerido es obtener información sobre cómo [configurar una plantilla personalizada del dispositivo](howto-set-up-template.md) para su propio dispositivo de IoT.
