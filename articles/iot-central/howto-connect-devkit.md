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
ms.openlocfilehash: 130ca6bc946d44d80cddba5486d405bfb15523cb
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66235878"
---
# <a name="connect-an-mxchip-iot-devkit-device-to-your-azure-iot-central-application"></a>Conexión de un dispositivo MXChip IoT DevKit a una aplicación de Microsoft IoT Central

En este artículo se describe cómo conectar, en tanto que desarrollador de dispositivos, un dispositivo MXChip IoT DevKit (DevKit) a una aplicación de Microsoft Azure IoT Central.

## <a name="before-you-begin"></a>Antes de empezar

Para completar los pasos descritos en este artículo, necesita los siguientes recursos:

1. Una aplicación de Azure IoT Central creada a partir de la plantilla de aplicación **Ejemplo Devkits**. Para más información, consulte la [guía de inicio rápido para crear una aplicación](quick-deploy-iot-central.md).
1. Un dispositivo DevKit. Para adquirir un dispositivo DevKit, visite [MXChip IoT DevKit](https://microsoft.github.io/azure-iot-developer-kit/).

## <a name="sample-devkits-application"></a>Aplicación de Devkits de ejemplo

Una aplicación creada a partir la **ejemplo Devkits** plantilla de aplicación incluye un **MXChip** plantilla de dispositivo que define las siguientes características del dispositivo:

- Las medidas de telemetría para **humedad**, **temperatura**, **presión**, **Magnetómetro** (mide a lo largo de X, Y eje Z), **Acelerómetro** (mide a lo largo de X, Y eje Z), y **giroscopio** (mide a lo largo de X, Y eje Z).
- Estado de la medida para **estado del dispositivo**.
- Medición de eventos para **presionado el botón B**.
- Configuración de **voltaje**, **actual**, **velocidad del ventilador**y un **IR** alternar.
- Propiedades del dispositivo **mueren número** y **ubicación del dispositivo**, que es una propiedad de ubicación.
- En la nube de la propiedad **fabricados en**.
- Comandos **Echo** y **cuenta regresiva**. Cuando un dispositivo real recibe un **Echo** comando, muestra el valor enviado en la pantalla del dispositivo. Cuando un dispositivo real recibe un **cuenta regresiva** comando, los ciclos de LED a través de un patrón, y el dispositivo envía los valores de cuenta atrás hasta IoT Central.

Para obtener detalles completos sobre la configuración, consulte [detalles de la plantilla de dispositivo MXChip](#mxchip-device-template-details)

## <a name="add-a-real-device"></a>Adición de un dispositivo real

### <a name="get-your-device-connection-details"></a>Obtener detalles de conexión de dispositivo

En la aplicación de Azure IoT Central, agregue un dispositivo real desde la **MXChip** plantilla de dispositivo y tome nota de los detalles de conexión del dispositivo: **Definir el ámbito de ID, Id. de dispositivo y clave principal**:

1. Agregar un **dispositivo real** en Device Explorer, seleccione **+ nuevo > Real** para agregar un dispositivo real.

    * Escriba una minúscula **Id. de dispositivo**, o usar el sugerido **Id. de dispositivo**.
    * Escriba un **nombre de dispositivo**, o utilice el nombre sugerido

    ![Agregar dispositivo](media/howto-connect-devkit/add-device.png)

1. Para obtener detalles de conexión del dispositivo **Id. de ámbito**, **Id. de dispositivo**, y **clave principal**, seleccione **Connect** en la página del dispositivo.

    ![Detalles de conexión](media/howto-connect-devkit/device-connect.png)

1. Tome nota de los detalles de conexión. Se ha desconectado temporalmente desde internet al preparar el dispositivo de DevKit en el paso siguiente.

### <a name="prepare-the-devkit-device"></a>Preparación del dispositivo DevKit

Si ha usado anteriormente el dispositivo y desea volver a configurarlo para usar una red Wi-Fi diferente, la cadena de conexión o la medición de datos de telemetría, presione tanto el **A** y **B** botones al mismo tiempo. Si no funciona, presione **restablecer** botón e inténtelo de nuevo.

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

1. En el equipo, teléfono o tableta, conéctese al nombre de red Wi-Fi que se muestra en la pantalla del dispositivo. Cuando se conecta a esta red, no tiene acceso a internet. Se espera que este estado, y solo está conectado a esta red durante un breve tiempo mientras configura el dispositivo.

1. Abra el explorador web y vaya a [http://192.168.0.1/start](http://192.168.0.1/start). Aparece la siguiente página web:

    ![Página de configuración del dispositivo](media/howto-connect-devkit/configpage.png)

    En la página web, escriba:
    - El nombre de la red Wi-Fi
    - la contraseña de la red Wi-Fi
    - El código PIN que se muestra en la pantalla del dispositivo
    - Los detalles de conexión **Id. de ámbito**, **Id. de dispositivo**, y **clave principal** del dispositivo (debe ya ha guardado este siguiendo los pasos)
    - Seleccione todas las medidas de la telemetría disponible

1. Después de elegir **Configure Device** (Configurar dispositivo), verá esta página:

    ![Dispositivo configurado](media/howto-connect-devkit/deviceconfigured.png)

1. Presione el botón **Reset**  (Restablecer) en el dispositivo.

## <a name="view-the-telemetry"></a>Visualización de la telemetría

Cuando se reinicie el dispositivo DevKit, la pantalla del dispositivo muestra:

* El número de mensajes de telemetría enviados.
* El número de errores.
* El número de propiedades deseadas recibidas y el número de propiedades notificadas enviadas.

> [!NOTE]
> Si el dispositivo aparece en un bucle cuando intenta conectarse, compruebe si el dispositivo es **bloqueado** en IoT Central, y **desbloquear** el dispositivo para que pueda conectarse a la aplicación.

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

1. En el **comandos** página, puede llamar a la **Echo** y **cuenta regresiva** comandos:

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

Use el código de Visual Studio para abrir el `MXCHIP/mxchip_advanced` carpeta en el `iot-central-firmware` carpeta:

![Visual Studio Code](media/howto-connect-devkit/vscodeview.png)

Para ver cómo se envían los datos de telemetría a la aplicación de Azure IoT Central, abra el **telemetry.cpp** de archivos en el `src` carpeta:

- La función `TelemetryController::buildTelemetryPayload` crea la carga de telemetría de JSON utilizando los datos de los sensores en el dispositivo.

- La función `TelemetryController::sendTelemetryPayload` llamadas `sendTelemetry` en el **AzureIOTClient.cpp** para enviar la carga de JSON al centro de IoT de la aplicación usa Azure IoT Central.

Para ver cómo se notifican los valores de propiedad a la aplicación de Azure IoT Central, abra el **telemetry.cpp** de archivos en el `src` carpeta:

- La función `TelemetryController::loop` envía el **ubicación** propiedad notificada cada 30 segundos aproximadamente. Usa el `sendReportedProperty` funcionando en el **AzureIOTClient.cpp** archivo de código fuente.

- La función `TelemetryController::loop` envía el **dieNumber** propiedad notificada cuando el acelerómetro dispositivo detecta un doble punteo. Usa el `sendReportedProperty` funcionando en el **AzureIOTClient.cpp** archivo de código fuente.

Para ver cómo responde el dispositivo a los comandos que se llama desde la aplicación IoT Central, abra el **registeredMethodHandlers.cpp** de archivos en el `src` carpeta:

- El **dmEcho** función es el controlador para el **echo** comando. Muestra el **displayedValue** presentada en la carga en la pantalla del dispositivo.

- El **dmCountdown** función es el controlador para el **cuenta regresiva** comando. Cambia el color del LED del dispositivo y utiliza una propiedad notificada para enviar el valor de la cuenta atrás a la aplicación IoT Central. La propiedad notificada tiene el mismo nombre que el comando. La función utiliza el `sendReportedProperty` funcionando en el **AzureIOTClient.cpp** archivo de código fuente.

El código en el **AzureIOTClient.cpp** archivo de origen usa funciones desde la [Microsoft SDK y bibliotecas para C](https://github.com/Azure/azure-iot-sdk-c) interactúen con IoT Hub.

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
| NOMBRE          | `Display name`   | NORMAL | PRECAUCIÓN | PELIGRO | 
| ------------- | -------------- | ------ | ------- | ------ | 
| DeviceState   | Device State (Estado del dispositivo)   | Verde  | Naranja  | Rojo    | 

#### <a name="events"></a>Eventos 
| NOMBRE             | `Display name`      | 
| ---------------- | ----------------- | 
| ButtonBPressed   | Button B Pressed (Botón B presionado)  | 

### <a name="settings"></a>Configuración

Valores numéricos

| `Display name` | Nombre del campo | Unidades | Posiciones decimales | Mínima | Máxima | Inicial |
| ------------ | ---------- | ----- | -------------- | ------- | ------- | ------- |
| Voltage (Voltaje)      | setVoltage | Voltios | 0              | 0       | 240     | 0       |
| Current      | setCurrent | Amp  | 0              | 0       | 100     | 0       |
| Fan Speed    | fanSpeed   | RPM   | 0              | 0       | 1000    | 0       |

Cambiar configuración

| `Display name` | Nombre del campo | Texto activado | Texto desactivado | Inicial |
| ------------ | ---------- | ------- | -------- | ------- |
| IR           | activateIR | ACTIVAR      | Apagado      | Off     |

### <a name="properties"></a>Properties (Propiedades)

| Type            | `Display name` | Nombre del campo | Tipo de datos |
| --------------- | ------------ | ---------- | --------- |
| Propiedad de dispositivo | Die number   | dieNumber  | número    |
| Propiedad de dispositivo | Ubicación del dispositivo   | location  | location    |
| Text            | Fabricado en     | manufacturedIn   | N/D       |

### <a name="commands"></a>Comandos:

| `Display name` | Nombre del campo | Tipo de valor devuelto | Nombre para mostrar del campo de entrada | Nombre del campo de entrada | Tipo de campo de entrada |
| ------------ | ---------- | ----------- | ------------------------ | ---------------- | ---------------- |
| Echo         | echo       | text        | valor para mostrar         | displayedValue   | text             |
| Cuenta atrás    | Cuenta atrás  | número      | Recuento de               | countFrom        | número           |

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido cómo conectar un MXChip IoT DevKit a la aplicación de Azure IoT Central, el siguiente paso sugerido es obtener información sobre cómo [configurar una plantilla personalizada del dispositivo](howto-set-up-template.md) para su propio dispositivo de IoT.
