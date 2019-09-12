---
title: Comunicación con una aplicación de dispositivo en Node.js mediante flujos de dispositivo de IoT Hub (versión preliminar) | Microsoft Docs
description: En este inicio rápido, ejecutará una aplicación del lado del servicio de Node.js que se comunica con un dispositivo IoT mediante un flujo de dispositivo.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.devlang: nodejs
ms.topic: quickstart
ms.custom: mvc
ms.date: 03/14/2019
ms.author: robinsh
ms.openlocfilehash: e85f2ea849aca9deeb92da7d7b2381d6c2b1b725
ms.sourcegitcommit: b7b0d9f25418b78e1ae562c525e7d7412fcc7ba0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/08/2019
ms.locfileid: "70802445"
---
# <a name="quickstart-communicate-to-a-device-application-in-nodejs-via-iot-hub-device-streams-preview"></a>Inicio rápido: Comunicación con una aplicación de dispositivo en Node.js mediante flujos de dispositivo de IoT Hub (versión preliminar)

[!INCLUDE [iot-hub-quickstarts-3-selector](../../includes/iot-hub-quickstarts-3-selector.md)]

Microsoft Azure IoT Hub actualmente admite flujos de dispositivos como [Característica en vista previa (GB)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

[Los flujos de dispositivo de IoT Hub](./iot-hub-device-streams-overview.md) permiten que las aplicaciones de servicio y de dispositivo se comuniquen de forma segura y apta para el firewall. Durante la versión preliminar pública, el SDK para Node.js solo admite flujos de dispositivo en el lado del servicio. Por consiguiente, este artículo de inicio rápido solo abarca las instrucciones para ejecutar la aplicación del lado del servicio. Hay que ejecutar una aplicación complementaria del lado del dispositivo, disponible en los inicios rápidos siguientes:

* [Comunicación con una aplicación de dispositivo en C mediante flujos de dispositivo de IoT Hub](./quickstart-device-streams-echo-c.md)

* [Comunicación con aplicaciones de dispositivo en C# mediante flujos de dispositivo de IoT Hub](./quickstart-device-streams-echo-csharp.md)

La aplicación Node.js del lado del servidor de este inicio rápido tiene las siguientes funcionalidades:

* Crea un flujo de dispositivo a un dispositivo de IoT.

* Lee la entrada de línea de comandos y la envía a la aplicación del dispositivo, que la devuelve.

El código mostrará el proceso de inicio de un flujo de dispositivos y cómo usarlo para enviar y recibir datos.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

La versión preliminar de los flujos de dispositivos solo se admite en este momento en instancias de IoT Hub creadas en las siguientes regiones:

*  **Centro de EE. UU.**

*  **EUAP de centro de EE. UU.**

Para ejecutar la aplicación del lado del servicio en este inicio rápido se necesita la versión 10.x.x de Node.js o posterior en la máquina de desarrollo.

Puede descargar Node.js para varias plataformas desde [nodejs.org](https://nodejs.org).

Puede verificar la versión actual de Node.js en el equipo de desarrollo con el comando siguiente:

```cmd/sh
node --version
```

Ejecute el siguiente comando para agregar la extensión IoT de Microsoft Azure para la CLI de Azure a la instancia de Cloud Shell. La extensión IOT agrega comandos específicos de IoT Hub, IoT Edge e IoT Device Provisioning Service (DPS) a la CLI de Azure.

```azurecli-interactive
az extension add --name azure-cli-iot-ext
```

Si aún no lo ha hecho, descargue el proyecto de Node.js de muestra desde https://github.com/Azure-Samples/azure-iot-samples-node/archive/streams-preview.zip y extraiga el archivo ZIP.

## <a name="create-an-iot-hub"></a>Crear un centro de IoT

Si ha completado la anterior [Guía de inicio rápido: Envío de telemetría desde un dispositivo a un centro de IoT](quickstart-send-telemetry-node.md) puede omitir este paso.

[!INCLUDE [iot-hub-include-create-hub-device-streams](../../includes/iot-hub-include-create-hub-device-streams.md)]

## <a name="register-a-device"></a>Registrar un dispositivo

Si ha completado la anterior [Guía de inicio rápido: Envío de telemetría desde un dispositivo a un centro de IoT](quickstart-send-telemetry-node.md) puede omitir este paso.

Debe registrar un dispositivo con IoT Hub antes de poder conectarlo. En esta guía de inicio rápido, usará Azure Cloud Shell para registrar un dispositivo simulado.

1. Ejecute los siguientes comandos en Azure Cloud Shell para crear la identidad del dispositivo.

   **YourIoTHubName**: reemplace este marcador de posición por el nombre elegido para el centro de IoT.

   **MyDevice**: nombre que se da al dispositivo registrado. Use MyCDevice tal como se muestra. Si elige otro nombre para el dispositivo, tendrá que usarlo en todo el artículo y actualizar el nombre del dispositivo en las aplicaciones de ejemplo antes de ejecutarlas.

    ```azurecli-interactive
    az iot hub device-identity create --hub-name YourIoTHubName --device-id MyDevice
    ```

2. También necesita una *cadena de conexión de servicio* para permitir que la aplicación back-end se conecte a IoT Hub y recupere los mensajes. El comando siguiente recupera la cadena de conexión del servicio de su instancia de IoT Hub:

    **YourIoTHubName**: reemplace este marcador de posición por el nombre elegido para el centro de IoT.

    ```azurecli-interactive
    az iot hub show-connection-string --policy-name service --name YourIoTHubName
    ```

    Tome nota del valor devuelto, que es similar a este:

   `"HostName={YourIoTHubName}.azure-devices.net;SharedAccessKeyName=service;SharedAccessKey={YourSharedAccessKey}"`

## <a name="communicate-between-device-and-service-via-device-streams"></a>Comunicación entre un dispositivo y un servicio mediante flujos de dispositivo

En esta sección, ejecuta la aplicación del lado del dispositivo y la aplicación del lado del servicio y se comunica entre las dos.

### <a name="run-the-device-side-application"></a>Ejecución de la aplicación del lado del dispositivo

Como ya se ha mencionado, el SDK de IoT Hub para Node.js solo admite flujos de dispositivo en el lado del servicio. Para la aplicación del lado del dispositivo, use los programas de dispositivo complementarios disponibles en uno de estos inicios rápidos:

   * [Comunicación con una aplicación de dispositivo en C mediante flujos de dispositivo de IoT Hub](./quickstart-device-streams-echo-c.md)

   * [Comunicación con aplicaciones de dispositivo en C# mediante flujos de dispositivo de IoT Hub](./quickstart-device-streams-echo-csharp.md)

Asegúrese de que la aplicación del lado del dispositivo se está ejecutando antes de continuar con el paso siguiente.

### <a name="run-the-service-side-application"></a>Ejecución de la aplicación del lado del servicio

Si se da por hecho que la aplicación del lado del dispositivo está en ejecución, realice los siguientes pasos para ejecutar la aplicación del lado del servicio escrito en Node.js:

* Especifique las credenciales del servicio y el identificador del dispositivo como variables de entorno.
 
   ```cmd/sh
   # In Linux
   export IOTHUB_CONNECTION_STRING="<provide_your_service_connection_string>"
   export STREAMING_TARGET_DEVICE="MyDevice"

   # In Windows
   SET IOTHUB_CONNECTION_STRING=<provide_your_service_connection_string>
   SET STREAMING_TARGET_DEVICE=MyDevice
   ```
  
   Cambie `MyDevice` por el identificador que haya elegido para el dispositivo.

* Vaya a `Quickstarts/device-streams-service` en la carpeta del proyecto descomprimida y ejecute el ejemplo que usa el nodo.

   ```cmd/sh
   cd azure-iot-samples-node-streams-preview/iot-hub/Quickstarts/device-streams-service
    
   # Install the preview service SDK, and other dependencies
   npm install azure-iothub@streams-preview
   npm install

   node echo.js
   ```

Al final del último paso, el programa del lado del servicio iniciará un flujo hacia el dispositivo y, una vez que establecido, enviará un búfer de cadena al servicio a través de dicho flujo. En este ejemplo, el programa del lado del servicio simplemente lee `stdin` del terminal y lo envía al dispositivo, que lo devuelve. Aquí se muestra una comunicación bidireccional correcta entre las dos aplicaciones.

![Salida de la consola del servicio](./media/quickstart-device-streams-echo-nodejs/service-console-output.png)

Luego, para terminar el programa vuelva a presionar Entrar.

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [iot-hub-quickstarts-clean-up-resources-device-streams](../../includes/iot-hub-quickstarts-clean-up-resources-device-streams.md)]

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido, ha configurado un centro de IoT, ha registrado un dispositivo, ha establecido un flujo de dispositivo entre las aplicaciones del dispositivo y el lado del servicio, y ha usado el flujo para enviar datos entre las aplicaciones.

Use los siguientes vínculos para más información sobre los flujos de dispositivo:

> [!div class="nextstepaction"]
> [Introducción a los flujos de dispositivo](./iot-hub-device-streams-overview.md) 