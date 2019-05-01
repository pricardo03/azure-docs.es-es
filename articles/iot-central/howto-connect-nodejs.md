---
title: Conexión de una aplicación cliente de Node.js genérica a Azure IoT Central | Microsoft Docs
description: Como desarrollador del dispositivo, cómo conectar un dispositivo de Node.js genérico a una aplicación de Azure IoT Central.
author: dominicbetts
ms.author: dobett
ms.date: 04/05/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: 5497e4956fbdc74eced302867c33a66d07d6a184
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60888965"
---
# <a name="connect-a-generic-client-application-to-your-azure-iot-central-application-nodejs"></a>Conexión de un aplicación cliente de Node.js genérica a una aplicación de Azure IoT Central (Node.js)

En este artículo, se describe el modo de conectar, como desarrollador de dispositivos, una aplicación de Node.js genérica que representa un dispositivo real a la aplicación Microsoft Azure IoT Central.

## <a name="before-you-begin"></a>Antes de empezar

Necesitará lo siguiente para completar los pasos de este artículo:

1. Una aplicación de Azure IoT Central. Para más información, consulte la [guía de inicio rápido para crear una aplicación](quick-deploy-iot-central.md).
1. Una máquina de desarrollo que tenga instalado [Node.js](https://nodejs.org/) versión 4.0.0 o posterior. Puede ejecutar `node --version` en la línea de comandos para comprobar la versión. Node.js está disponible para una amplia variedad de sistemas operativos.

## <a name="create-a-device-template"></a>Creación de una plantilla de dispositivo

En la aplicación de Azure IoT Central, necesita una plantilla de dispositivo con la medidas, las propiedades del dispositivo, configuración y los comandos siguientes:

### <a name="telemetry-measurements"></a>Medidas de telemetría

Agregue la siguiente telemetría en el **medidas** página:

| Display Name (Nombre para mostrar) | Nombre del campo  | Unidades | Min | max | Decimal Places |
| ------------ | ----------- | ----- | --- | --- | -------------- |
| Temperatura  | temperatura | F     | 60  | 110 | 0              |
| Humedad     | humedad    | %     | 0   | 100 | 0              |
| Presión     | presión    | kPa   | 80  | 110 | 0              |

> [!NOTE]
> El tipo de datos de la medida de telemetría es un número de punto flotante.

Escriba los nombres de campo en la plantilla de dispositivo exactamente tal y como aparecen en la tabla. Si los nombres de campo no coinciden con los nombres de propiedad en el código de dispositivo correspondiente, no se puede mostrar los datos de telemetría en la aplicación.

### <a name="state-measurements"></a>Medidas de estado

Agregue el siguiente estado en el **medidas** página:

| Display Name (Nombre para mostrar) | Nombre del campo  | Value 1 | Display Name (Nombre para mostrar) | Value 2 | Display Name (Nombre para mostrar) |
| ------------ | ----------- | --------| ------------ | ------- | ------------ | 
| Fan Mode     | fanmode     | 1       | En ejecución      | 0       | Stopped      |

> [!NOTE]
> El tipo de datos de la medida de tipo Estado es cadena.

Escriba los nombres de campo en la plantilla de dispositivo exactamente tal y como aparecen en la tabla. Si los nombres de campo no coinciden con los nombres de propiedad en el código de dispositivo correspondiente, no se puede mostrar el estado de la aplicación.

### <a name="event-measurements"></a>Medidas de eventos

Agregue el siguiente evento en el **medidas** página:

| Display Name (Nombre para mostrar) | Nombre del campo  | Gravedad |
| ------------ | ----------- | -------- |
| Overheating  | overheat    | Error    |

> [!NOTE]
> El tipo de datos de la medida de tipo Evento es cadena.

### <a name="device-properties"></a>Propiedades del dispositivo

Agregue las siguientes propiedades de dispositivo en el **propiedades** página:

| Display Name (Nombre para mostrar)        | Nombre del campo        | Tipo de datos |
| ------------------- | ----------------- | --------- |
| Número de serie       | serialNumber      | text      |
| Device Manufacturer | manufacturer      | text      |

Escriba los nombres de campo en la plantilla de dispositivo exactamente tal y como se muestran en la tabla. Si los nombres de campo no coinciden con los nombres de propiedad en el código de dispositivo correspondiente, no se puede mostrar las propiedades de la aplicación.

### <a name="settings"></a>Configuración

Agregue el siguiente **número** configuración en el **configuración** página:

| Display Name (Nombre para mostrar)    | Nombre del campo     | Unidades | Decimals | Min | max  | Inicial |
| --------------- | -------------- | ----- | -------- | --- | ---- | ------- |
| Fan Speed       | fanSpeed       | rpm   | 0        | 0   | 3000 | 0       |
| Set Temperature | setTemperature | F     | 0        | 20  | 200  | 80      |

Escriba los nombres de campo en la plantilla de dispositivo exactamente tal y como se muestran en la tabla. Si los nombres de campo no coinciden con los nombres de propiedad en el código de dispositivo correspondiente, el dispositivo no puede recibir el valor de configuración.

### <a name="commands"></a>Comandos:

Agregue el siguiente comando en el **comandos** página:

| Display Name (Nombre para mostrar)    | Nombre del campo     | Tiempo de espera predeterminado | Tipo de datos |
| --------------- | -------------- | --------------- | --------- |
| Cuenta atrás       | Cuenta atrás      | 30              | número    |

Agregue el siguiente campo de entrada para el comando de la cuenta atrás:

| Display Name (Nombre para mostrar)    | Nombre del campo     | Tipo de datos | Valor |
| --------------- | -------------- | --------- | ----- |
| Recuento de      | countFrom      | número    | 10    |

Escriba los nombres de campo tal y como se muestra en las tablas en la plantilla de dispositivo. Si los nombres de campo no coinciden con los nombres de propiedad en el código de dispositivo correspondiente, el dispositivo no puede procesar el comando.

## <a name="add-a-real-device"></a>Adición de un dispositivo real

En la aplicación de Azure IoT Central, agregar un dispositivo real a la plantilla de dispositivo que creó en la sección anterior.

A continuación, siga las instrucciones del tutorial de "Agregar un dispositivo" para [generar una cadena de conexión para el dispositivo real](tutorial-add-device.md#generate-connection-string). Utilice esta cadena de conexión en la sección siguiente:

### <a name="create-a-nodejs-application"></a>Creación de una aplicación Node.js

En los pasos siguientes se muestra cómo crear una aplicación cliente que implementa el dispositivo real que se agregó a la aplicación. Aquí la aplicación Node.js representa el dispositivo real. 

1. Cree una carpeta denominada `connected-air-conditioner-adv` en la máquina. Vaya a esta carpeta en el entorno de línea de comandos.

1. Para inicializar el proyecto Node.js, ejecute los siguientes comandos:

    ```cmd/sh
    npm init
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

1. Cree un archivo denominado **connectedAirConditionerAdv.js** en la carpeta `connected-air-conditioner-adv`.

1. Agregue las siguientes instrucciones `require` al principio del archivo **connectedAirConditionerAdv.js**:

    ```javascript
    "use strict";

    // Use the Azure IoT device SDK for devices that connect to Azure IoT Central.
    var clientFromConnectionString = require('azure-iot-device-mqtt').clientFromConnectionString;
    var Message = require('azure-iot-device').Message;
    var ConnectionString = require('azure-iot-device').ConnectionString;
    ```

1. Agregue las siguientes declaraciones de variable al archivo:

    ```javascript
    var connectionString = '{your device connection string}';
    var targetTemperature = 0;
    var client = clientFromConnectionString(connectionString);
    ```

    Actualizar el marcador de posición `{your device connection string}` con el [cadena de conexión de dispositivo](tutorial-add-device.md#generate-connection-string). En este ejemplo, inicializa `targetTemperature` a cero, podría usar la lectura desde el dispositivo actual o un valor desde el dispositivo gemelo.

1. Para enviar datos de telemetría, el estado y las mediciones de eventos a la aplicación de Azure IoT Central, agregue la siguiente función al archivo:

    ```javascript
    // Send device measurements.
    function sendTelemetry() {
      var temperature = targetTemperature + (Math.random() * 15);
      var humidity = 70 + (Math.random() * 10);
      var pressure = 90 + (Math.random() * 5);
      var fanmode = 0;
      var data = JSON.stringify({
        temperature: temperature,
        humidity: humidity,
        pressure: pressure,
        fanmode: (temperature > 25) ? "1" : "0",
        overheat: (temperature > 35) ? "ER123" : undefined });
      var message = new Message(data);
      client.sendEvent(message, (err, res) => console.log(`Sent message: ${message.getData()}` +
        (err ? `; error: ${err.toString()}` : '') +
        (res ? `; status: ${res.constructor.name}` : '')));
    }
    ```

1. Para enviar las propiedades de dispositivo a la aplicación de Azure IoT Central, agregue la siguiente función al archivo:

    ```javascript
    // Send device reported properties.
    function sendDeviceProperties(twin, properties) {
      twin.properties.reported.update(properties, (err) => console.log(`Sent device properties: ${JSON.stringify(properties)}; ` +
        (err ? `error: ${err.toString()}` : `status: success`)));
    }
    ```

1. Para definir la configuración a la que responde el dispositivo, agregue la siguiente definición:

    ```javascript
    // Add any settings your device supports,
    // mapped to a function that is called when the setting is changed.
    var settings = {
      'fanSpeed': (newValue, callback) => {
          // Simulate it taking 1 second to set the fan speed.
          setTimeout(() => {
            callback(newValue, 'completed');
          }, 1000);
      },
      'setTemperature': (newValue, callback) => {
        // Simulate the temperature setting taking two steps.
        setTimeout(() => {
          targetTemperature = targetTemperature + (newValue - targetTemperature) / 2;
          callback(targetTemperature, 'pending');
          setTimeout(() => {
            targetTemperature = newValue;
            callback(targetTemperature, 'completed');
          }, 5000);
        }, 5000);
      }
    };
    ```

1. Para controlar la configuración actualizada desde la aplicación de Azure IoT Central, agregue lo siguiente al archivo:

    ```javascript
    // Handle settings changes that come from Azure IoT Central via the device twin.
    function handleSettings(twin) {
      twin.on('properties.desired', function (desiredChange) {
        for (let setting in desiredChange) {
          if (settings[setting]) {
            console.log(`Received setting: ${setting}: ${desiredChange[setting].value}`);
            settings[setting](desiredChange[setting].value, (newValue, status, message) => {
              var patch = {
                [setting]: {
                  value: newValue,
                  status: status,
                  desiredVersion: desiredChange.$version,
                  message: message
                }
              }
              twin.properties.reported.update(patch, (err) => console.log(`Sent setting update for ${setting}; ` +
                (err ? `error: ${err.toString()}` : `status: success`)));
            });
          }
        }
      });
    }
    ```

1. Agregue el código siguiente para controlar un comando de cuenta regresiva enviado desde la aplicación IoT Central:

    ```javascript
    // Handle countdown command
    function onCountdown(request, response) {
      console.log('Received call to countdown');

      var countFrom = (typeof(request.payload.countFrom) === 'number' && request.payload.countFrom < 100) ? request.payload.countFrom : 10;

      response.send(200, (err) => {
        if (err) {
          console.error('Unable to send method response: ' + err.toString());
        } else {
          client.getTwin((err, twin) => {
            function doCountdown(){
              if ( countFrom >= 0 ) {
                var patch = {
                  countdown:{
                    value: countFrom
                  }
                };
                sendDeviceProperties(twin, patch);
                countFrom--;
                setTimeout(doCountdown, 2000 );
              }
            }

            doCountdown();
          });
        }
      });
    }
    ```

1. Agregue el código siguiente para completar la conexión a Azure IoT Central y enlazar las funciones en el código de cliente:

    ```javascript
    // Handle device connection to Azure IoT Central.
    var connectCallback = (err) => {
      if (err) {
        console.log(`Device could not connect to Azure IoT Central: ${err.toString()}`);
      } else {
        console.log('Device successfully connected to Azure IoT Central');

        // Create handler for countdown command
        client.onDeviceMethod('countdown', onCountdown);

        // Send telemetry measurements to Azure IoT Central every 1 second.
        setInterval(sendTelemetry, 1000);

        // Get device twin from Azure IoT Central.
        client.getTwin((err, twin) => {
          if (err) {
            console.log(`Error getting device twin: ${err.toString()}`);
          } else {
            // Send device properties once on device start up.
            var properties = {
              serialNumber: '123-ABC',
              manufacturer: 'Contoso'
            };
            sendDeviceProperties(twin, properties);

            // Apply device settings and handle changes to device settings.
            handleSettings(twin);
          }
        });
      }
    };

    // Start the device (connect it to Azure IoT Central).
    client.open(connectCallback);
    ```

## <a name="run-your-nodejs-application"></a>Ejecución de la aplicación de Node.js

En el entorno de línea de comandos, ejecute el comando siguiente:

```cmd/sh
node connectedAirConditionerAdv.js
```

Como operador de la aplicación de Azure IoT Central, en su dispositivo real puede:

* Ver la telemetría de la página **Measurements** (Medidas):

    ![Ver datos de telemetría](media/howto-connect-nodejs/viewtelemetry.png)

* Ver los valores de propiedad del dispositivo enviados desde su dispositivo en la página **Properties** (Propiedades): La propiedad del dispositivo iconos de actualización cuando se conecta el dispositivo:

    ![Visualización de las propiedades del dispositivo](media/howto-connect-nodejs/viewproperties.png)

* Fijar la temperatura del ventilador de velocidad y de destino desde el **configuración** página:

    ![Establecimiento de la velocidad del ventilador](media/howto-connect-nodejs/setfanspeed.png)

* Llame al comando cuenta hacia atrás desde el **comandos** página:

    ![Cuenta regresiva de llamadas (comando)](media/howto-connect-nodejs/callcountdown.png)

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido cómo conectar un cliente de Node.js genérico a una aplicación de Azure IoT Central, el siguiente paso sugerido es obtener información sobre cómo [configurar una plantilla personalizada del dispositivo](howto-set-up-template.md) para su propio dispositivo de IoT.
