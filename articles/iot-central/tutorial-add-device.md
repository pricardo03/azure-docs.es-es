---
title: Adición de un dispositivo real a una aplicación de Azure IoT Central | Microsoft Docs
description: Como operador, puede agregar un dispositivo real a una aplicación de Azure IoT Central.
author: sandeeppujar
ms.author: sandeepu
ms.date: 02/01/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: 8e7eee40bed29117d2873393395a852e4b738533
ms.sourcegitcommit: ad3e63af10cd2b24bf4ebb9cc630b998290af467
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/01/2019
ms.locfileid: "58793488"
---
# <a name="tutorial-add-a-real-device-to-your-azure-iot-central-application"></a>Tutorial: Adición de un dispositivo real a una aplicación de Azure IoT Central

En este tutorial se explica cómo agregar un dispositivo real a la aplicación Microsoft Azure IoT Central.

Este tutorial se compone de dos partes:

1. En primer lugar, como operador, aprenderá a agregar y configurar un dispositivo real en la aplicación de Azure IoT Central. Una vez completada esta parte, recuperará una cadena de conexión para usarla en la segunda parte.
2. A continuación, como desarrollador de dispositivos, aprenderá sobre el código del dispositivo real. Agregará la cadena de conexión de la primera parte al código de ejemplo.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Agregar un nuevo dispositivo real
> * Configuración del dispositivo real
> * Obtener la cadena de conexión para el dispositivo real de la aplicación
> * Comprender cómo se asigna código de cliente a la aplicación
> * Configurar el código de cliente para el dispositivo real

## <a name="prerequisites"></a>Requisitos previos

Antes de empezar, el desarrollador debería realizar al menos el primer tutorial para desarrolladores para crear la aplicación de Azure IoT Central:

* [Definición de un tipo de dispositivo nuevo](tutorial-define-device-type.md) (obligatorio)
* [Configuración de reglas y acciones para el dispositivo](tutorial-configure-rules.md) (opcional)
* [Personalización de la vista del operador](tutorial-customize-operator.md) (opcional)

## <a name="add-a-real-device"></a>Adición de un dispositivo real

Para agregar un dispositivo real a la aplicación, usará la plantilla de dispositivo **Connected Air Conditioner** (Acondicionador de aire conectado) que creó en el tutorial [Definición de un tipo de dispositivo nuevo](tutorial-define-device-type.md).

1. Para agregar un nuevo dispositivo como un operador, seleccione **Device Explorer** (Explorador de dispositivos) en el menú de navegación izquierdo:

   ![Página de Device Explorer (Explorador de dispositivos) que muestra el acondicionador de aire conectado](media/tutorial-add-device/explorer.png)

   En **Device Explorer** (Explorador de dispositivos), se muestra la plantilla de dispositivo **Connected Air Conditioner** (Acondicionador de aire conectado) y un dispositivo simulado. IoT Central crea automáticamente un dispositivo simulado cuando se crea una plantilla de dispositivo.

2. Para empezar a conectar un dispositivo de acondicionador de aire conectado real, seleccione **+** y, a continuación, **Real**:

   ![Primeros pasos para agregar un dispositivo de acondicionador de aire conectado real](media/tutorial-add-device/newreal.png)

3. Escriba el identificador de dispositivo (debe estar en minúsculas) o use el identificador de dispositivo sugerido. También puede escribir el nombre del nuevo dispositivo y elegir **Crear**.

   ![Cambio de nombre del dispositivo](media/tutorial-add-device/rename.png)

## <a name="configure-a-real-device"></a>Configuración de un dispositivo real

El dispositivo real se crea a partir de la plantilla de dispositivo **Connected Air Conditioner** (Acondicionador de aire conectado). Puede usar **Settings** (Configuración) para configurar el dispositivo y establecer valores de propiedad para registrar información sobre el dispositivo.

1. En la página **Settings**, tenga en cuenta que el estado del valor **Set Temperature** (Establecer temperatura) es **no update** (sin actualizaciones). Permanece en este estado hasta que se conecta el dispositivo real a la aplicación y se confirma que ha actuado en la configuración.

    ![Configuración que muestra la sincronización](media/tutorial-add-device/settingssyncing.png)

2. En la página **Properties** (Propiedades) del dispositivo nuevo, real, tanto ubicación de servicio como última fecha de servicio son propiedades editables. Los campos de versión de firmware y número de serie están vacíos hasta que el dispositivo se conecte a la aplicación. Estos valores de solo lectura se envían desde el dispositivo y no se puede editar.

    ![Propiedades de dispositivo del dispositivo real](media/tutorial-add-device/setproperties1.png)

3. Puede ver las páginas **Measurements** (Medidas), **Rules** (Reglas) y **Dashboard** (Panel) del dispositivo real.

## <a name="generate-connection-string"></a>Generación de cadena de conexión

Un desarrollador de dispositivos debe insertar la *cadena de conexión* del dispositivo real en el código que se ejecuta en el dispositivo. La cadena de conexión permite al dispositivo conectarse de forma segura a la aplicación. Los pasos siguientes muestran cómo generar la cadena de conexión y preparar el código de Node.js del cliente.

## <a name="prepare-the-client-code"></a>Preparación del código de cliente

El código de ejemplo de este artículo está escrito en [Node.js](https://nodejs.org/) y muestra el código necesario para:

* Conectarse como dispositivo a la aplicación de Azure IoT Central.
* Enviar datos de telemetría de temperatura como un dispositivo de acondicionador de aire conectado.
* Enviar propiedades de dispositivo a la aplicación de Azure IoT Central.
* Responder a un operador que usa el valor **Set Temperature** (Establecer temperatura).
* Controlar el comando Echo desde la aplicación de Azure IoT Central.

Los artículos enumerados en la sección [Pasos siguientes](#next-steps) incluyen más ejemplos completos y muestran otros lenguajes de programación. Para más información acerca de cómo se conectan los dispositivos a Azure IoT Central, consulte el artículo [Conectividad de dispositivos](concepts-connectivity.md).

Los pasos siguientes muestran cómo preparar el código de ejemplo de [Node.js](https://nodejs.org/):

1. Instale [Node.js](https://nodejs.org/) 4.0.x o una versión posterior en la máquina. Node.js está disponible para una amplia variedad de sistemas operativos.

1. Cree una carpeta denominada `connectedairconditioner` en la máquina.

1. Vaya a la carpeta `connectedairconditioner` que ha creado en el entorno de línea de comandos.

1. Instale el generador de clave de DPS mediante el siguiente comando:

    ```cmd/sh
    npm i -g dps-keygen
    ```

   Obtenga más información sobre la [herramienta de la línea de comandos aquí](https://www.npmjs.com/package/dps-keygen).

1. La cadena de conexión para una instancia de dispositivo de la aplicación se genera a partir de la información de dispositivo proporcionada por IoT Central.

   Vuelva al portal de IoT Central. En la pantalla del dispositivo de la máquina de aire acondicionado real, elija **Connect** (Conectar).

   ![Página del dispositivo con un vínculo de información de conexión](media/tutorial-add-device/connectionlink.png)

1. En la página Device Connection (Conexión del dispositivo), copie y pegue el identificador de ámbito, el de dispositivo y la clave principal en un editor de texto y guárdelos. Estos valores se usan en el siguiente paso.

   ![Detalles de conexión](media/tutorial-add-device/device-connect.png)

1. Vuelva al entorno de la línea de comandos y genere la cadena de conexión mediante la ejecución de lo siguiente:

    ```cmd/sh
    dps-keygen -si:<scope_id> -di:<device_id> -dk:<Primary Key>
    ```

   Copie la salida y guárdela en un nuevo archivo (por ejemplo, connection.txt).

1. Para inicializar el proyecto de Node.js, ejecute el siguiente comando aceptando todos los valores predeterminados:

    ```cmd/sh
    npm init
      ```

1. Para instalar los paquetes necesarios, ejecute el comando siguiente:

    ```cmd/sh
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

1. Con un editor de texto, cree un archivo denominado **ConnectedAirConditioner.js** en la carpeta `connectedairconditioner`.

1. Agregue las siguientes instrucciones `require` al principio del archivo **ConnectedAirConditioner.js**:

    ```javascript
    'use strict';

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

    > [!NOTE]
    > Actualizará el marcador de posición `{your device connection string}` en un paso posterior.

1. Guarde los cambios que ha realizado hasta ahora, pero mantenga abierto el archivo.

## <a name="review-client-code"></a>Revisión de código de cliente

En la sección anterior, creó un esquema de proyecto de Node.js para una aplicación que se conecta a la aplicación de Azure IoT Central. El siguiente paso consiste en agregar código a:

* Conectarse a la aplicación de Azure IoT Central.
* Enviar datos de telemetría a la aplicación de Azure IoT Central.
* Enviar propiedades de dispositivo a la aplicación de Azure IoT Central.
* Recibir la configuración de la aplicación de Azure IoT Central.
* Controlar el comando Echo desde la aplicación de Azure IoT Central.

1. Para enviar datos de telemetría de temperatura a la aplicación de Azure IoT Central, agregue el código siguiente al archivo **ConnectedAirConditioner.js**:

    ```javascript
    // Send device telemetry.
    function sendTelemetry() {
      var temperature = targetTemperature + (Math.random() * 15);
      var data = JSON.stringify({ temperature: temperature });
      var message = new Message(data);
      client.sendEvent(message, (err, res) => console.log(`Sent message: ${message.getData()}` +
        (err ? `; error: ${err.toString()}` : '') +
        (res ? `; status: ${res.constructor.name}` : '')));
    }
    ```

    El nombre del campo en la notación de objetos JavaScript que envíe debe coincidir con el nombre del campo especificado para la telemetría de temperatura de la plantilla de dispositivo. En este ejemplo, el nombre del campo es **temperature**.

1. Para enviar las propiedades del dispositivo, como **firmwareVersion** y **serialNumber**, agregue la siguiente definición:

    ```javascript
    // Send device properties
    function sendDeviceProperties(twin) {
      var properties = {
        firmwareVersion: "9.75",
        serialNumber: "10001"
      };
      twin.properties.reported.update(properties, (errorMessage) => 
      console.log(` * Sent device properties ` + (errorMessage ? `Error: ${errorMessage.toString()}` : `(success)`)));
    }
    ```

1. Para definir la configuración que admite el dispositivo, como **setTemperature**, agregue la siguiente definición:

    ```javascript
    // Add any settings your device supports
    // mapped to a function that is called when the setting is changed.
    var settings = {
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

1. Para controlar la configuración enviada desde Azure IoT Central, agregue la siguiente función que busca y ejecuta el código de dispositivo apropiado:

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

    Esta función:

    * Detecta si Azure IoT Central envía una propiedad deseada.
    * Busca la función adecuada que llamar para controlar el cambio de configuración.
    * Envía una confirmación a la aplicación de Azure IoT Central.

1. Para responder a un comando, como **echo**, desde la aplicación de Azure IoT Central, agregue la siguiente definición:

    ```javascript
    // Respond to the echo command
    function onCommandEcho(request, response) {
      // Display console info
      console.log(' * Echo command received');
      // Respond
      response.send(10, 'Success', function (errorMessage) {});
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
        // Send telemetry measurements to Azure IoT Central every 1 second.
        setInterval(sendTelemetry, 1000);
        // Setup device command callbacks
        client.onDeviceMethod('echo', onCommandEcho);
        // Get device twin from Azure IoT Central.
        client.getTwin((err, twin) => {
          if (err) {
            console.log(`Error getting device twin: ${err.toString()}`);
          } else {
            // Send device properties once on device start up
            sendDeviceProperties(twin);
            // Apply device settings and handle changes to device settings.
            handleSettings(twin);
          }
        });
      }
    };

    client.open(connectCallback);
    ```

1. Guarde los cambios que ha realizado hasta ahora, pero mantenga abierto el archivo.

## <a name="configure-client-code"></a>Configuración de código de cliente

<!-- Add the connection string to the sample code, build, and run -->
Para configurar el código de cliente para que se conecte a la aplicación de Azure IoT Central, debe agregar la cadena de conexión para el dispositivo real que anotó anteriormente en este tutorial.

1. En el archivo **ConnectedAirConditioner.js**, busque la siguiente línea de código:

    ```javascript
    var connectionString = '{your device connection string}';
    ```

1. Reemplace `{your device connection string}` por la cadena de conexión del dispositivo real. La cadena de conexión se ha guardado anteriormente en un editor de texto.

1. Guarde los cambios realizados en el archivo **ConnectedAirConditioner.js**.

1. Para ejecutar el código de ejemplo, escriba el siguiente comando en el entorno de la línea de comandos:

    ```cmd/sh
    node ConnectedAirConditioner.js
    ```

    > [!NOTE]
    > Asegúrese de que se encuentra en la carpeta `connectedairconditioner` al ejecutar este comando.

1. La aplicación imprime la salida en la consola:

   ![Salida de la aplicación cliente](media/tutorial-add-device/output.png)

1. Después de unos 30 segundos, verá los datos de telemetría en la página **Measurements** (Medidas) del dispositivo:

   ![Telemetría ~~real](media/tutorial-add-device/realtelemetry.png)

1. En la página **Settings** (Configuración), puede ver que el valor ahora está sincronizado. Cuando el dispositivo se conectó por primera vez, recibió el valor de configuración y confirmó el cambio:

   ![Valor sincronizado](media/tutorial-add-device/settingsynced.png)

1. En la página **Settings** (Configuración), establezca la temperatura del dispositivo en **95** y seleccione **Update device** (Actualizar dispositivo). La aplicación de ejemplo recibe y procesa este cambio:

   ![Recepción y procesamiento del valor](media/tutorial-add-device/receivesetting.png)

   > [!NOTE]
   > Hay dos mensajes de "actualización de configuración". Uno cuando se envía el estado `pending` y otro cuando se envía el estado `completed`.

1. En la página **Measurements** (Medidas), puede ver que el dispositivo está enviando valores de temperatura más altos:

    ![Los datos de telemetría de temperatura son ahora más altos](media/tutorial-add-device/highertemperature.png)

## <a name="next-steps"></a>Pasos siguientes

En este tutorial aprendió lo siguiente:

> [!div class="nextstepaction"]
> * Agregar un nuevo dispositivo real
> * Configurar el nuevo dispositivo
> * Obtener la cadena de conexión para el dispositivo real de la aplicación
> * Comprender cómo se asigna código de cliente a la aplicación
> * Configurar el código de cliente para el dispositivo real

Ahora que ha conectado un dispositivo real a la aplicación de Azure IoT Central, estos son los siguientes pasos sugeridos:

Como operador, puede obtener información sobre:

* [Administración de dispositivos](howto-manage-devices.md)
* [Uso de conjuntos de dispositivos](howto-use-device-sets.md)
* [Creación de análisis personalizados](howto-use-device-sets.md)

Como desarrollador de dispositivos, puede obtener información sobre:

* [Preparación y conexión de un dispositivo DevKit (C)](howto-connect-devkit.md)
* [Preparación y conexión de un dispositivo Raspberry Pi (Python)](howto-connect-raspberry-pi-python.md)
* [Preparación y conexión de un dispositivo Raspberry Pi (C#)](howto-connect-raspberry-pi-csharp.md)
* [Preparación y conexión de un dispositivo Windows 10 IoT Core (C#)](howto-connect-windowsiotcore.md)
* [Conexión de un cliente de Node.js genérico a una aplicación de Azure IoT Central](howto-connect-nodejs.md)
