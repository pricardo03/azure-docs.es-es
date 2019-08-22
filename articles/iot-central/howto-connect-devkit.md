---
title: Conexión de un dispositivo DevKit a una aplicación de Azure IoT Central | Microsoft Docs
description: Como desarrollador de dispositivos, aprenda a conectar un dispositivo MXChip IoT DevKit a una aplicación de Azure IoT Central.
author: dominicbetts
ms.author: dobett
ms.date: 03/22/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: 81a355cc7c0d1190ee86fac6ed155380e6e5c0d1
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/21/2019
ms.locfileid: "69877525"
---
# <a name="connect-an-mxchip-iot-devkit-device-to-your-azure-iot-central-application"></a>Conexión de un dispositivo MXChip IoT DevKit a una aplicación de Microsoft IoT Central

[!INCLUDE [iot-central-original-pnp](../../includes/iot-central-original-pnp-note.md)]

En este artículo se describe cómo conectar, en tanto que desarrollador de dispositivos, un dispositivo MXChip IoT DevKit (DevKit) a una aplicación de Microsoft Azure IoT Central.

## <a name="before-you-begin"></a>Antes de empezar

Para completar los pasos de este artículo, necesitará los siguientes recursos:

1. Una aplicación de Azure IoT Central creada a partir de la plantilla de aplicación **Ejemplo Devkits**. Para más información, consulte la [guía de inicio rápido para crear una aplicación](quick-deploy-iot-central.md).
1. Un dispositivo DevKit. Para adquirir un dispositivo DevKit, visite [MXChip IoT DevKit](https://microsoft.github.io/azure-iot-developer-kit/).

## <a name="sample-devkits-application"></a>Aplicación de Devkits de ejemplo

Una aplicación creada a partir de la plantilla de aplicación **Ejemplo Devkits** incluye una plantilla de dispositivo **MXChip** que define las siguientes características de dos`psotovp:

- Medidas de telemetría para **Humidity** (Humedad), **Temperature** (Temperatura), **Pressure** (Presión), **Magnetometer** (Magnetómetro) (medido en los ejes X, Y y Z), **Accelerometer** (Acelerómetro) (medido en los ejes X, Y y Z) y **Gyroscope** (Giroscopio) (medido en los ejes X, Y y Z).
- Estado de la medida para **Device State** (Estado del dispositivo).
- Medición de eventos para **Button B Pressed** (Botón B presionado).
- Configuración para **Voltage** (Voltaje), **Current** (Corriente), **Fan Speed** (Velocidad del ventilador) y un botón de alternancia **IR**.
- Propiedades del dispositivo **Die number** (Número de troquel) y **Device Location** (Ubicación del dispositivo), que es una propiedad de ubicación.
- Propiedad de la nube **Manufactured In** (Fabricado en).
- Comandos **Echo** (Eco) y **Countdown** (Cuenta atrás). Cuando un dispositivo real recibe un comando **Echo** (Eco), muestra el valor enviado en la pantalla del dispositivo. Cuando un dispositivo real recibe un comando **Countdown** (Cuenta atrás), el indicador LED pasa por un patrón y el dispositivo envía los valores de cuenta atrás a IoT Central.

Para obtener detalles completos sobre la configuración, consulte [Detalles de la plantilla de dispositivo MXChip](#mxchip-device-template-details).

## <a name="add-a-real-device"></a>Adición de un dispositivo real

### <a name="get-your-device-connection-details"></a>Obtener los detalles de conexión del dispositivo

En la aplicación de Azure IoT Central, agregue un dispositivo real desde la plantilla de dispositivos **MXChip** y tome nota de los detalles de conexión del dispositivo: **id. de ámbito, id. de dispositivo y clave principal**:

1. Agregue un **dispositivo real** desde el Explorador de dispositivos, seleccione **+ Nuevo > Real** para agregar un dispositivo real.

    * Especifique un **id. de dispositivo** en minúsculas o use el **id. de dispositivo** sugerido.
    * Especifique un **nombre de dispositivo** o use el nombre sugerido.

    ![Agregar dispositivo](media/howto-connect-devkit/add-device.png)

1. Para obtener los detalles de conexión del dispositivo, el **id. de ámbito**, **id. de dispositivo** y la **clave principal**, seleccione **Conectar** en la página del dispositivo.

    ![Detalles de conexión](media/howto-connect-devkit/device-connect.png)

1. Tome nota de los detalles de conexión. Al preparar el dispositivo de DevKit en el paso siguiente, se desconectará temporalmente de Internet.

### <a name="prepare-the-devkit-device"></a>Preparación del dispositivo DevKit

Si ha usado anteriormente el dispositivo y desea volver a configurarlo para usar una red Wi-Fi, cadena de conexión o medición de telemetría diferentes, presione los botones **A** y **B** al mismo tiempo. Si eso no funciona, presione el botón **Restablecer** y pruebe otra vez.

#### <a name="to-prepare-the-devkit-device"></a>Para preparar el dispositivo DevKit

1. Descargue el firmware de Azure IoT Central precompilado más reciente para MXChip desde la página [releases](https://aka.ms/iotcentral-docs-MXChip-releases) (versiones) en GitHub.
1. Conecte el dispositivo DevKit en el equipo de desarrollo con un cable USB. En Windows, se abre una ventana del explorador de archivos en una unidad asignada al almacenamiento en el dispositivo DevKit. Por ejemplo, la unidad podría denominarse **AZ3166 (D:)** .
1. Arrastre el archivo **iotCentral.bin** a la ventana de la unidad. Cuando se complete la operación de copia, el dispositivo se reinicia con el nuevo firmware.

1. Cuando se reinicie el dispositivo DevKit, se muestra la siguiente pantalla:

    ```
    Connect HotSpot:
    AZ3166_??????
    go-> 192.168.0.1
    PIN CODE xxxxx
    ```

    > [!NOTE]
    > Si la pantalla muestra cualquier otra cosa, reinicie el dispositivo y presione los botones **A** y **B** del dispositivo al mismo tiempo para reiniciarlo.

1. El dispositivo ya está en modo de punto de acceso. Puede conectarse a este punto de acceso Wi-Fi desde su equipo o dispositivo móvil.

1. En el equipo, teléfono o tableta, conéctese al nombre de red Wi-Fi que se muestra en la pantalla del dispositivo. Cuando se conecta a esta red, no tiene acceso a Internet. Este estado es el esperado y solo se conectará a esta red durante un breve tiempo mientras se configura el dispositivo.

1. Abra el explorador web y vaya a [http://192.168.0.1/start](http://192.168.0.1/start). Aparece la siguiente página web:

    ![Página de configuración del dispositivo](media/howto-connect-devkit/configpage.png)

    En la página web, especifique lo siguiente:
    - El nombre de la red Wi-Fi
    - La contraseña de la red Wi-Fi
    - El código PIN que se muestra en la pantalla del dispositivo
    - Los detalles de conexión, **id. de ámbito**, **id. de dispositivo** y **clave principal** del dispositivo (ya debería haber guardado esta información después de haber realizado los pasos).
    - Seleccione todas las medidas de la telemetría disponibles.

1. Después de elegir **Configure Device** (Configurar dispositivo), verá esta página:

    ![Dispositivo configurado](media/howto-connect-devkit/deviceconfigured.png)

1. Presione el botón **Reset**  (Restablecer) en el dispositivo.

## <a name="view-the-telemetry"></a>Visualización de la telemetría

Cuando se reinicie el dispositivo DevKit, la pantalla del dispositivo muestra:

* El número de mensajes de telemetría enviados.
* El número de errores.
* El número de propiedades deseadas recibidas y el número de propiedades notificadas enviadas.

> [!NOTE]
> Si el dispositivo aparece en un bucle cuando intenta conectarse, compruebe si el dispositivo está **bloqueado** en IoT Central y **desbloquéelo** para que pueda conectarse a la aplicación.

Agite el dispositivo para enviar una propiedad notificada. El dispositivo envía un número aleatorio como propiedad del dispositivo **Die number** (Número de chip).

Puede ver las mediciones de telemetría y los valores de propiedad notificados y configurar los parámetros en Azure IoT Central:

1. Use **Device Explorer** (Explorador de dispositivos) para navegar hasta la página **Measurements** (Medidas) del dispositivo MXChip real que se ha agregado:

    ![Navegación al dispositivo real](media/howto-connect-devkit/realdevicenew.png)

1. En la página **Measurements** (Medidas), puede ver la telemetría proveniente del dispositivo de MXChip:

    ![Visualización de telemetría desde el dispositivo real](media/howto-connect-devkit/devicetelemetrynew.png)

1. En la página **Properties** (Propiedades), puede ver el último número de chip y la ubicación del dispositivo notificados por el dispositivo:

    ![Visualización de las propiedades del dispositivo](media/howto-connect-devkit/devicepropertynew.png)

1. En la página **Settings** (Configuración), puede actualizar la configuración en el dispositivo MXChip:

    ![Visualización de la configuración del dispositivo](media/howto-connect-devkit/devicesettingsnew.png)

1. En la página **Comandos**, puede llamar a los comandos **Echo** (Eco) y **Countdown** (Cuenta atrás):

    ![Comandos de llamada](media/howto-connect-devkit/devicecommands.png)

1. En la página **Dashboard** (Panel), puede ver el mapa de ubicación.

    ![Visualización del panel de dispositivos](media/howto-connect-devkit/devicedashboardnew.png)

## <a name="download-the-source-code"></a>Descarga del código fuente

Si desea explorar y modificar el código del dispositivo, puede descargarlo desde GitHub. Si tiene previsto modificar el código, debe seguir estas instrucciones para [preparar el entorno de desarrollo](https://microsoft.github.io/azure-iot-developer-kit/docs/get-started/#step-5-prepare-the-development-environment) para su sistema operativo de escritorio.

Para descargar el código fuente, ejecute el siguiente comando en el equipo de escritorio:

```cmd/sh
git clone https://github.com/Azure/iot-central-firmware
```

El ejemplo anterior descarga el código fuente en una carpeta denominada `iot-central-firmware`.

> [!NOTE]
> Si **git** no está instalado en el entorno de desarrollo, puede descargarlo desde [https://git-scm.com/download](https://git-scm.com/download).

## <a name="review-the-code"></a>Revisión del código

Use Visual Studio Code para abrir la carpeta `MXCHIP/mxchip_advanced` en la carpeta `iot-central-firmware`:

![Visual Studio Code](media/howto-connect-devkit/vscodeview.png)

Para ver cómo se envían los datos de telemetría a la aplicación de Azure IoT Central, abra el archivo **telemetry.cpp** de la carpeta `src`:

- La función `TelemetryController::buildTelemetryPayload` crea la carga de telemetría de JSON utilizando los datos de los sensores en el dispositivo.

- La función `TelemetryController::sendTelemetryPayload` llamada a `sendTelemetry` en **AzureIOTClient.cpp** para enviar la carga de JSON a IoT Hub que la aplicación de Azure IoT Central usa.

Para ver cómo se notifican los valores de propiedad a la aplicación de Azure IoT Central, abra el archivo **telemetry.cpp** de la carpeta `src`:

- La función `TelemetryController::loop` envía la propiedad notificada **location** cada 30 segundos aproximadamente. Usa la función `sendReportedProperty` del archivo de origen **AzureIOTClient.cpp**.

- La función `TelemetryController::loop` envía la propiedad notificada **dieNumber** cuando el acelerómetro del dispositivo detecta una doble pulsación. Usa la función `sendReportedProperty` del archivo de origen **AzureIOTClient.cpp**.

Para ver cómo el dispositivo responde a los comandos a los que se llaman desde la aplicación de IoT Central, abra el archivo **registeredMethodHandlers.cpp** de la carpeta `src`:

- La función **dmEcho** es el controlador para el comando **echo**. Muestra el campo **displayedValue** en la carga de la pantalla del dispositivo.

- La función **dmCountdown** es el controlador del comando **countdown**. Cambia el color del indicador LED del dispositivo y usa una propiedad notificada para enviar el valor de cuenta atrás a la aplicación de IoT Central. La propiedad notificada tiene el mismo nombre que el comando. La función usa la función `sendReportedProperty` del archivo de origen **AzureIOTClient.cpp**.

El código del archivo de origen **iotHubClient.cpp** usa funciones de los [SDK y las bibliotecas de Microsoft Azure IoT para C](https://github.com/Azure/azure-iot-sdk-c) para interactuar con IoT Hub.

Para obtener información acerca de cómo modificar, compilar y cargar el código de ejemplo en el dispositivo, consulte el archivo **readme.md** en la carpeta `MXCHIP/mxchip_advanced`.

## <a name="mxchip-device-template-details"></a>Detalles de la plantilla de dispositivo MXChip

Una aplicación creada a partir de la plantilla de aplicación Ejemplo Devkits incluye una plantilla de dispositivo MXChip con las siguientes características:

### <a name="measurements"></a>Medidas

#### <a name="telemetry"></a>Telemetría

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

#### <a name="states"></a>States 
| NOMBRE          | Nombre para mostrar   | NORMAL | PRECAUCIÓN | PELIGRO | 
| ------------- | -------------- | ------ | ------- | ------ | 
| DeviceState   | Device State (Estado del dispositivo)   | Verde  | Naranja  | Rojo    | 

#### <a name="events"></a>Eventos 
| NOMBRE             | Nombre para mostrar      | 
| ---------------- | ----------------- | 
| ButtonBPressed   | Button B Pressed (Botón B presionado)  | 

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

### <a name="properties"></a>properties (Propiedades)

| type            | Nombre para mostrar | Nombre del campo | Tipo de datos |
| --------------- | ------------ | ---------- | --------- |
| Propiedad de dispositivo | Die number   | dieNumber  | número    |
| Propiedad de dispositivo | Ubicación del dispositivo   | location  | location    |
| Texto            | Fabricado en     | manufacturedIn   | N/D       |

### <a name="commands"></a>Comandos:

| Nombre para mostrar | Nombre del campo | Tipo de valor devuelto | Nombre para mostrar del campo de entrada | Nombre del campo de entrada | Tipo de campo de entrada |
| ------------ | ---------- | ----------- | ------------------------ | ---------------- | ---------------- |
| Eco         | echo       | text        | valor para mostrar         | displayedValue   | text             |
| Cuenta atrás    | countdown  | número      | Recuento de               | countFrom        | número           |

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a conectar un MXChip IoT DevKit a la aplicación de Azure IoT Central, le sugerimos como siguiente paso aprender a [configurar una plantilla de dispositivo personalizada](howto-set-up-template.md) para su dispositivo de IoT.
