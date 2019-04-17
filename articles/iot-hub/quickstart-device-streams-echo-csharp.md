---
title: Inicio rápido sobre los flujos de dispositivo de Azure IoT Hub (versión preliminar) en C# | Microsoft Docs
description: En este artículo de inicio rápido, va a ejecutar dos aplicaciones C# de ejemplo que se comunican mediante un flujo de dispositivo establecido con IoT Hub.
author: rezasherafat
manager: briz
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: quickstart
ms.custom: mvc
ms.date: 03/14/2019
ms.author: rezas
ms.openlocfilehash: 2853bd5539a40e3b38927f619756fe37a4cec984
ms.sourcegitcommit: 045406e0aa1beb7537c12c0ea1fbf736062708e8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/04/2019
ms.locfileid: "59006865"
---
# <a name="quickstart-communicate-to-device-applications-in-c-via-iot-hub-device-streams-preview"></a>Inicio rápido: Comunicación con aplicaciones de dispositivo en C# mediante flujos de dispositivo de IoT Hub (versión preliminar)

[!INCLUDE [iot-hub-quickstarts-3-selector](../../includes/iot-hub-quickstarts-3-selector.md)]

Microsoft Azure IoT Hub actualmente admite flujos de dispositivos como una [versión preliminar](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

[Los flujos de dispositivo de IoT Hub](./iot-hub-device-streams-overview.md) permiten que las aplicaciones de servicio y de dispositivo se comuniquen de forma segura y apta para el firewall. En este artículo de inicio rápido se trabaja con dos programas de C# que aprovechan los flujos de dispositivo para enviar datos de un lado a otro (eco).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

La versión preliminar de los flujos de dispositivos solo se admite en este momento en instancias de IoT Hub creadas en las siguientes regiones:

  - **Centro de EE. UU.**
  - **EUAP del centro de EE. UU.**

Las dos aplicaciones de ejemplo que se ejecutan en este inicio rápido se escriben con C#. Necesita el SDK de .NET Core 2.1.0 o una versión posterior en el equipo de desarrollo.

Puede descargar el SDK de .NET Core para varias plataformas desde [.NET](https://www.microsoft.com/net/download/all).

Puede verificar la versión actual de C# en el equipo de desarrollo con el comando siguiente:

```
dotnet --version
```

Ejecute el siguiente comando para agregar la extensión IoT de Microsoft Azure para la CLI de Azure a la instancia de Cloud Shell. La extensión IOT agrega comandos específicos de IoT Hub, IoT Edge e IoT Device Provisioning Service (DPS) a la CLI de Azure.

```azurecli-interactive
az extension add --name azure-cli-iot-ext
```

Descargue el proyecto de C# de muestra de https://github.com/Azure-Samples/azure-iot-samples-csharp/archive/master.zip y extraiga el archivo ZIP. Lo necesitará para el dispositivo y para el servicio.

## <a name="create-an-iot-hub"></a>Crear un centro de IoT

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub-device-streams.md)]

## <a name="register-a-device"></a>Registrar un dispositivo

Debe registrar un dispositivo con IoT Hub antes de poder conectarlo. En esta guía de inicio rápido, usará Azure Cloud Shell para registrar un dispositivo simulado.

1. Ejecute los siguientes comandos en Azure Cloud Shell para crear la identidad del dispositivo.

   **YourIoTHubName**: reemplace este marcador de posición por el nombre elegido para el centro de IoT.

   **MyDevice**: nombre que se da al dispositivo registrado. Use MyCDevice tal como se muestra. Si elige otro nombre para el dispositivo, tendrá que usarlo en todo el artículo y actualizar el nombre del dispositivo en las aplicaciones de ejemplo antes de ejecutarlas.

    ```azurecli-interactive
    az iot hub device-identity create --hub-name YourIoTHubName --device-id MyDevice
    ```

2. Ejecute los siguientes comandos en Azure Cloud Shell para obtener la _cadena de conexión del dispositivo_ que acaba de registrar:

   **YourIoTHubName**: reemplace este marcador de posición por el nombre elegido para el centro de IoT.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name YourIoTHubName --device-id MyDevice --output table
    ```

    Anote la cadena de conexión del dispositivo, que se parecerá a la del siguiente ejemplo:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyDevice;SharedAccessKey={YourSharedAccessKey}`

    Usará este valor más adelante en este inicio rápido.

3. También necesita la _cadena de conexión de servicio_ de su centro de IoT para que la aplicación del lado del servicio pueda conectarse al centro de IoT y establecer el flujo de dispositivo. El siguiente comando recupera este valor para el centro de IoT:

   **YourIoTHubName**: reemplace este marcador de posición por el nombre elegido para el centro de IoT.

    ```azurecli-interactive
    az iot hub show-connection-string --policy-name service --name YourIoTHubName
    ```

    Tome nota del valor devuelto, que es similar a este:

   `"HostName={YourIoTHubName}.azure-devices.net;SharedAccessKeyName=service;SharedAccessKey={YourSharedAccessKey}"`

## <a name="communicate-between-device-and-service-via-device-streams"></a>Comunicación entre un dispositivo y un servicio mediante flujos de dispositivo

### <a name="run-the-service-side-application"></a>Ejecución de la aplicación del lado del servicio

Vaya a `iot-hub/Quickstarts/device-streams-echo/service` en la carpeta del proyecto descomprimida. Necesitará tener a mano la siguiente información:

| Nombre de parámetro | Valor del parámetro |
|----------------|-----------------|
| `ServiceConnectionString` | Proporcione la cadena de conexión del servicio de la instancia de IoT Hub. |
| `DeviceId` | Proporcione el identificador del dispositivo que creó anteriormente, por ejemplo, MyDevice. |

Compile y ejecute el código de la siguiente forma:

```
cd ./iot-hub/Quickstarts/device-streams-echo/service/

# Build the application
dotnet build

# Run the application
# In Linux/MacOS
dotnet run "<ServiceConnectionString>" "<MyDevice>"

# In Windows
dotnet run <ServiceConnectionString> <MyDevice>
```

> [!NOTE]
> Si la aplicación no responde a tiempo del lado del dispositivo, el tiempo expira.

### <a name="run-the-device-side-application"></a>Ejecución de la aplicación del lado del dispositivo

Vaya al directorio `iot-hub/Quickstarts/device-streams-echo/device` en la carpeta del proyecto descomprimida. Necesitará tener a mano la siguiente información:

| Nombre de parámetro | Valor del parámetro |
|----------------|-----------------|
| `DeviceConnectionString` | Proporcione la cadena de conexión del dispositivo de la instancia de IoT Hub. |

Compile y ejecute el código de la siguiente forma:

```
cd ./iot-hub/Quickstarts/device-streams-echo/device/

# Build the application
dotnet build

# Run the application
# In Linux/MacOS
dotnet run "<DeviceConnectionString>"

# In Windows
dotnet run <DeviceConnectionString>
```

Al final del último paso, el programa del lado del servicio iniciará un flujo hacia el dispositivo y, una vez que establecido, enviará un búfer de cadena al servicio a través de dicho flujo. En este ejemplo, el programa del lado de servicio simplemente devuelve los mismos datos al dispositivo, lo que demuestra que la comunicación bidireccional entre las dos aplicaciones es correcta. Consulte la figura siguiente.

Salida de la consola en el lado del dispositivo: ![alt text](./media/quickstart-device-streams-echo-csharp/device-console-output.png "Salida de la consola en el lado del dispositivo")

Salida de la consola en el lado del servicio: ![alt text](./media/quickstart-device-streams-echo-csharp/service-console-output.png "Salida de la consola en el lado del servicio")

El tráfico que se envía por el flujo se canaliza mediante IoT Hub en lugar de enviarse directamente. Este escenario proporciona [estas ventajas](./iot-hub-device-streams-overview.md#benefits).

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources-device-streams.md)]

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido, ha configurado un centro de IoT, ha registrado un dispositivo, ha establecido un flujo de dispositivo entre las aplicaciones C# de dispositivo y de servicio, y ha usado el flujo para enviar datos entre las aplicaciones.

Use los siguientes vínculos para más información sobre los flujos de dispositivo:

> [!div class="nextstepaction"]
> [Introducción a los flujos de dispositivo](./iot-hub-device-streams-overview.md)
