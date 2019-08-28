---
title: Interacción con un dispositivo IoT Plug and Play (versión preliminar) conectado a una solución de Azure IoT | Microsoft Docs
description: Use Node.js para conectarse e interactuar con un dispositivo IoT Plug and Play (versión preliminar) que esté conectado a su solución de Azure IoT.
author: miagdp
ms.author: miag
ms.date: 08/02/2019
ms.topic: quickstart
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: 246d3eac8f9d8aff6d603ea8686e430ce0f772ea
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/21/2019
ms.locfileid: "69881594"
---
# <a name="quickstart-interact-with-an-iot-plug-and-play-preview-device-thats-connected-to-your-solution"></a>Inicio rápido: Interacción con un dispositivo IoT Plug and Play (versión preliminar) que esté conectado a una solución

IoT Plug and Play (versión preliminar) simplifica IoT, ya que permite interactuar con las funcionalidades de un dispositivo sin tener que conocer la implementación subyacente del dispositivo. En este inicio rápido se muestra cómo usar Node.js para conectarse y controlar un dispositivo IoT Plug and Play que esté conectado a una solución.

## <a name="prerequisites"></a>Requisitos previos

Descargue e instale Node.js desde [nodejs.org](https://nodejs.org).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prepare-an-iot-hub"></a>Preparación de un centro de IoT

También necesitará un centro de IoT de Azure en la suscripción de Azure para completar este inicio rápido. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

Agregue la extensión Microsoft Azure IoT para la CLI de Azure:

```azurecli-interactive
az extension add --name azure-cli-iot-ext
```

Ejecute el siguiente comando la nueva identidad del dispositivo en su centro de IoT. Reemplace **YourIoTHubName** y **YourDevice** por los nombres reales. Si aún no tiene un centro de IoT, siga [estas instrucciones para crearlo](../iot-hub/iot-hub-create-using-cli.md):

```azurecli-interactive
az iot hub device-identity create --hub-name [YourIoTHubName] --device-id [YourDevice]
```

Ejecute los siguientes comandos para obtener la _cadena de conexión del dispositivo_ que acaba de registrar:

```azurecli-interactive
az iot hub device-identity show-connection-string --hub-name [YourIoTHubName] --device-id [YourDevice] --output table
```

Ejecute los comandos siguientes para obtener la _cadena de conexión de su centro de IoT_:

```azurecli-interactive
az iot hub show-connection-string --hub-name [YourIoTHubName] --output table
```

## <a name="connect-your-device"></a>Conexión del dispositivo

En este inicio rápido, se usa un sensor ambiental de ejemplo, que se ha escrito en Node.js, como dispositivo IoT Plug and Play. Las siguientes instrucciones muestran cómo instalar y ejecutar el dispositivo:

1. Clone el repositorio de GitHub:

    ```cmd/sh
    git clone https://github.com/azure-samples/azure-iot-samples-node
    ```

1. En un terminal, vaya a la carpeta raíz del repositorio clonado, vaya a la carpeta **/azure-iot-samples-node/digital-twins/Quickstarts/Device** y, luego, instale todas las dependencias mediante la ejecución del siguiente comando:

    ```cmd/sh
    npm install
    ```

1. Configure la _cadena de conexión del dispositivos_:

    ```cmd/sh
    set DEVICE_CONNECTION_STRING=<your device connection string>
    ```

1. Ejecute el ejemplo con el siguiente comando:

    ```cmd/sh
    node sample_device.js
    ```

1. Verá mensajes que indican que el dispositivo ha enviado datos de telemetría y sus propiedades. El dispositivo ya está listo para recibir comandos y actualizaciones de propiedades. No cierre este terminal, ya que lo necesitará más adelante para confirmar que los ejemplos del servicio también han funcionado.

## <a name="build-the-solution"></a>Compilación de la solución

En este inicio rápido, se usa una solución de IoT de ejemplo creada en Node.js para interactuar con el dispositivo de ejemplo.

1. Abra otro terminal. Vaya a la carpeta del repositorio clonado y luego a la carpeta **/azure-iot-samples-node/digital-twins/Quickstarts/Service**. Instale todas las dependencias, para lo que debe ejecutar el siguiente comando:

    ```cmd/sh
    npm install
    ```

1. Configure la _cadena de conexión del centro_:

    ```cmd/sh
    set IOTHUB_CONNECTION_STRING=<your hub connection string>
    ```

### <a name="read-a-property"></a>Lectura de una propiedad

1. Cuando conecte el dispositivo al terminal verá el siguiente mensaje:

    ```cmd/sh
    reported state property as online
    ```

1. Abra el archivo **get_digital_twin.js**. Reemplace `deviceID` por el identificador de dispositivo y guarde el archivo.

1. Vaya al terminal que abrió para ejecutar el ejemplo de servicio y ejecute el siguiente comando:

    ```cmd/sh
    node get_digital_twin.js
    ```

1. En el resultado, en el componente _environmentalSensor_, verá que se ha notificado el mismo estado:

    ```JSON
    reported state property as online
    ```

### <a name="update-a-writable-property"></a>Actualización de una propiedad editable

1. Abra el archivo **update_digital_twin_property.js**.

1. Al principio del archivo, hay un conjunto de constantes definidas con marcadores de posición en mayúsculas. Reemplace **deviceID** por el identificador de dispositivo real, actualice las constantes con los valores siguientes y guarde el archivo:

    ```javascript
    const componentName = 'environmentalSensor';
    const propertyName = 'brightness';
    const propertyValue = 60;
    ```

1. Vaya al terminal que abrió para ejecutar el ejemplo de servicio y use el siguiente comando para ejecutar el ejemplo:

    ```cmd/sh
    node update_digital_twin_property.js
    ```

1. En el terminal, verá la información del gemelo digital asociada con el dispositivo. Busque el componente _environmentalSensor_ y verá el nuevo valor de brillo 60.

    ```json
        "environmentalSensor": {
        "name": "environmentalSensor",
        "properties": {
          "brightness": {
            "reported": {
              "value": 60,
              "desiredState": {
                "code": 200,
                "version": 14,
                "description": "helpful descriptive text"
              }
            },
            "desired": {
              "value": 60
            }
          },
          "state": {
            "reported": {
              "value": "online"
            }
          }
        }
      }
    ```

1. Vaya al terminal del _dispositivo_ y verá que el dispositivo ha recibido la actualización:

    ```cmd/sh
    Received an update for brightness: 60
    updated the property
    ```
2. Vuelva al terminal del _servicio_ y ejecute de nuevo el comando siguiente para confirmar que la propiedad se ha actualizado.
    
    ```cmd/sh
    node get_digital_twin.js
    ```
3. En el resultado, en el componente environmentalSensor, verá que se ha notificado el valor de brillo actualizado. Note: el dispositivo puede tardar un tiempo en finalizar la actualización. Este paso se puede repetir tantas veces como sea necesario hasta que el dispositivo haya procesado realmente la actualización de la propiedad.
    
    ```json
      "brightness": {
        "reported": {
          "value": 60,
          }
       }
    ```

### <a name="invoke-a-command"></a>Invocación de un comando

1. Abra el archivo **invoke_command.js**.

1. Al principio del archivo, reemplace `deviceID` por el identificador de dispositivo real. Actualice las constantes con los valores siguientes y, después, guarde el archivo:

    ```javascript
    const interfaceInstanceName = 'environmentalSensor';
    const commandName = 'blink';
    ```

1. Vaya al terminal que abrió para ejecutar el ejemplo de servicio. Use el siguiente comando para ejecutar el ejemplo:

    ```cmd/sh
    node invoke_command.js
    ```

1. En el terminal, el resultado correcto es similar al siguiente:

    ```cmd/sh
    invoking command blink on component environmentalSensor for device test...
    {
      "result": "helpful response text",
      "statusCode": 200,
      "requestId": "33e536d3-14f7-4105-88f3-629b9933851c",
      "_response": "helpful response text"
    }
    ```

1. Vaya al terminal del _dispositivo_ y verá que el comando se ha confirmado:

    ```cmd/sh
    received command: blink for component: environmentalSensor
    acknowledgement succeeded.
    ```

## <a name="clean-up-resources"></a>Limpieza de recursos

Si planea leer los artículos siguientes, puede conservar los recursos que usó en este inicio rápido. De lo contrario, puede eliminar los recursos que ha creado para este inicio rápido para evitar cargos adicionales.

Para eliminar el centro y el dispositivo registrado, realice los pasos siguientes en la CLI de Azure:

```azurecli-interactive
az group delete --name <Your group name>
```

Para eliminar solo el dispositivo que registró en IoT Hub, realice los pasos siguientes en la CLI de Azure:

```azurecli-interactive
az iot hub device-identity delete --hub-name [YourIoTHubName] --device-id [YourDevice]
```

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido ha aprendido a conectar un dispositivo IoT Plug and Play a una solución de IoT. Para más información sobre cómo crear una solución que interactúe con dispositivos IoT Plug and Play, consulte:

> [!div class="nextstepaction"]
> [Procedimiento: Conexión e interacción con un dispositivo](howto-develop-solution.md)
