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
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65510319"
---
# <a name="connect-a-windows-iot-core-device-to-your-azure-iot-central-application"></a>Conexión de un dispositivo Windows IoT Core a una aplicación de Azure IoT Central

En este artículo se describe cómo conectar, en tanto que desarrollador de dispositivos, un dispositivo Windows IoT Core a una aplicación de Microsoft Azure IoT Central.

## <a name="before-you-begin"></a>Antes de empezar

Necesitará lo siguiente para completar los pasos de este artículo:

- Una aplicación de Azure IoT Central creada a partir de la plantilla de aplicación **Ejemplo Devkits**. Para más información, consulte la [guía de inicio rápido para crear una aplicación](quick-deploy-iot-central.md).

- Un dispositivo en el que se ejecuta el sistema operativo Windows 10 IoT Core. Para obtener más información, vea [Configuración del dispositivo Windows 10 IoT Core](https://docs.microsoft.com/windows/iot-core/tutorials/quickstarter/devicesetup).

- Un equipo de desarrollo que tenga instalado [Node.js](https://nodejs.org/) versión 8.0.0 o posterior. Puede ejecutar `node --version` en la línea de comandos para comprobar la versión. Node.js está disponible para una amplia variedad de sistemas operativos.

## <a name="the-sample-devkits-application"></a>La aplicación Ejemplo Devkits

Una aplicación creada a partir de la plantilla de aplicación **Ejemplo Devkits** incluye una plantilla de dispositivo **Windows IoT Core** con las siguientes características:

- Medidas de telemetría del dispositivo: **Humedad**, **Temperatura** y **Presión**.
- Valor para controlar la **Velocidad del ventilador**.
- Una propiedad de dispositivo **Fecha de anulación** y una propiedad de nube **Ubicación**.

Para obtener todos los detalles sobre la configuración de la plantilla de dispositivo, vea [Detalles de la plantilla de dispositivo de Windows IoT Core](#device-template-details).

## <a name="add-a-real-device"></a>Adición de un dispositivo real

En la aplicación de Azure IoT Central, use la página **Explorador de dispositivos** para agregar un dispositivo real desde la plantilla de dispositivo de **Windows 10 IoT Core**. Anote los detalles de conexión del dispositivo (**Id. de ámbito**, **Id. de dispositivo** y **Clave principal**). Para obtener más información, vea [Obtención de información sobre la conexión](howto-generate-connection-string.md#get-connection-information).

## <a name="prepare-the-device"></a>Preparar el dispositivo

Para que el dispositivo se conecte a IoT Central, necesita una cadena de conexión.

[!INCLUDE [iot-central-howto-connection-string](../../includes/iot-central-howto-connection-string.md)]

Para que el código del dispositivo acceda a la cadena de conexión, guárdelo en un archivo denominado **connection.string.iothub** de la carpeta `C:\Data\Users\DefaultAccount\Documents\` en el dispositivo de Windows 10 IoT Core.

Para copiar el archivo **connection.string.iothub** desde el equipo de escritorio en la carpeta `C:\Data\Users\DefaultAccount\Documents\` del dispositivo, puede usar el [Portal de dispositivos Windows](https://docs.microsoft.com/windows/iot-core/manage-your-device/deviceportal):

1. Use el explorador web para ir al Portal de dispositivos Windows en el dispositivo.
1. Para examinar los archivos del dispositivo, seleccione **Aplicaciones > Explorador de archivos**.
1. Vaya a **Carpetas de usuario\Documentos**. Luego cargue el archivo **connection.string.iothub**:

    ![Carga de cadena de conexión](media/howto-connect-windowsiotcore/device-portal.png)

## <a name="deploy-and-run"></a>Implementación y ejecución

Para implementar y ejecutar la aplicación de ejemplo en el dispositivo, puede usar el [Portal de dispositivos Windows](https://docs.microsoft.com/windows/iot-core/manage-your-device/deviceportal):

1. Use el explorador web para ir al Portal de dispositivos Windows en el dispositivo.
1. Para implementar y ejecutar la aplicación **Cliente de Azure IoT Hub**, seleccione **Aplicaciones > Quick-run samples** (Ejemplos de ejecución rápida). Luego seleccione **Cliente de Azure IoT Hub**.
1. Después seleccione **Implementar y ejecutar**.

    ![Implementación y ejecución](media/howto-connect-windowsiotcore/quick-run.png)

Pasados unos minutos, puede ver la telemetría del dispositivo en la aplicación IoT Central.

El [Portal de dispositivos Windows](https://docs.microsoft.com/windows/iot-core/manage-your-device/deviceportal) incluye herramientas que se pueden usar para solucionar problemas del dispositivo:

- La página **Administrador de aplicaciones** permite controlar las aplicaciones que se ejecutan en el dispositivo.
- Si no tiene un monitor conectado al dispositivo, puede usar la página **Configuración del dispositivo** para realizar capturas de pantalla del dispositivo. Por ejemplo:

    ![Captura de pantalla de la aplicación](media/howto-connect-windowsiotcore/iot-hub-foreground-client.png)

## <a name="download-the-source-code"></a>Descarga del código fuente

Si quiere explorar y modificar el código fuente de la aplicación cliente, puede descargarlo desde el [repositorio de GitHub Windows-iotcore-samples](https://github.com/Microsoft/Windows-iotcore-samples/blob/master/Samples/Azure/IoTHubClients).

## <a name="device-template-details"></a>Detalles de la plantilla de dispositivo

Una aplicación creada a partir de la plantilla de aplicación **Ejemplo Devkits** incluye una plantilla de dispositivo **Windows IoT Core** con las siguientes características:

### <a name="telemetry-measurements"></a>Medidas de telemetría

| Nombre del campo     | Unidades  | Mínima | Máxima | Posiciones decimales |
| -------------- | ------ | ------- | ------- | -------------- |
| humedad       | %      | 0       | 100     | 0              |
| temp           | °C     | -40     | 120     | 0              |
| presión       | hPa    | 260     | 1260    | 0              |

### <a name="settings"></a>Configuración

Valores numéricos

| Nombre para mostrar | Nombre del campo | Unidades | Posiciones decimales | Mínima | Máxima | Inicial |
| ------------ | ---------- | ----- | -------------- | ------- | ------- | ------- |
| Fan Speed    | fanSpeed   | RPM   | 0              | 0       | 1000    | 0       |

### <a name="properties"></a>Properties (Propiedades)

| Type            | Nombre para mostrar | Nombre del campo | Tipo de datos |
| --------------- | ------------ | ---------- | --------- |
| Propiedad de dispositivo | Die number   | dieNumber  | número    |
| Texto            | Ubicación     | location   | N/D       |

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a conectar un dispositivo de Windows IoT Core con la aplicación Azure IoT Central, el siguiente paso sugerido es aprender a [configurar una plantilla de dispositivo personalizada](howto-set-up-template.md) para el dispositivo de IoT.
