---
title: Comunicación con una aplicación de dispositivo en C mediante flujos de dispositivo de Azure IoT Hub (versión preliminar) | Microsoft Docs
description: En este inicio rápido, ejecutará una aplicación del lado del dispositivo de C que se comunica con un dispositivo IoT mediante un flujo de dispositivo.
author: rezasherafat
manager: briz
ms.service: iot-hub
services: iot-hub
ms.devlang: c
ms.topic: quickstart
ms.custom: mvc
ms.date: 03/14/2019
ms.author: rezas
ms.openlocfilehash: 6a2fe71a1086559d90adf5c464323f353be431aa
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2019
ms.locfileid: "66733305"
---
# <a name="quickstart-communicate-to-a-device-application-in-c-via-iot-hub-device-streams-preview"></a>Inicio rápido: Comunicación con una aplicación de dispositivo en C mediante flujos de dispositivo de IoT Hub (versión preliminar)

[!INCLUDE [iot-hub-quickstarts-3-selector](../../includes/iot-hub-quickstarts-3-selector.md)]

Azure IoT Hub admite actualmente flujos de dispositivos como una [característica en vista previa (GB)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

[Los flujos de dispositivo de IoT Hub](iot-hub-device-streams-overview.md) permiten que las aplicaciones de servicio y de dispositivo se comuniquen de forma segura y apta para el firewall. Durante la versión preliminar pública, el SDK para C solo admite flujos de dispositivo en el lado del dispositivo. Por consiguiente, este inicio rápido solo incluye las instrucciones para ejecutar la aplicación del lado del dispositivo. Para ejecutar una aplicación complementaria del lado del servicio, consulte:
 
   * [Comunicación con aplicaciones de dispositivo en C# mediante flujos de dispositivo de IoT Hub](./quickstart-device-streams-echo-csharp.md)
   * [Comunicación con una aplicación de dispositivo en Node.js mediante flujos de dispositivo de IoT Hub](./quickstart-device-streams-echo-nodejs.md)

La aplicación de C del lado del dispositivo de este artículo de inicio rápido tiene las siguientes funcionalidades:

* Establecer un flujo de dispositivo a un dispositivo IoT.
* Recibir datos enviados desde la aplicación del lado del servicio y devolverlos.

El código mostrará el proceso de inicio de un flujo de dispositivo y cómo usarlo para enviar y recibir datos.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

* La versión preliminar de los flujos de dispositivo solo se admite en este momento en instancias de IoT Hub creadas en las siguientes regiones:

  * Centro de EE. UU.
  * EUAP del centro de EE. UU.

* Instale [Visual Studio 2017](https://www.visualstudio.com/vs/) con la carga de trabajo [Desarrollo para el escritorio con C++](https://www.visualstudio.com/vs/support/selecting-workloads-visual-studio-2017/) habilitada.

* Instale la última versión de [Git](https://git-scm.com/download/).

* Ejecute el siguiente comando para agregar la extensión IoT de Azure para la CLI de Azure a la instancia de Cloud Shell. La extensión IOT agrega comandos específicos de IoT Hub, IoT Edge e IoT Device Provisioning Service (DPS) a la CLI de Azure.

   ```azurecli-interactive
   az extension add --name azure-cli-iot-ext
   ```

## <a name="prepare-the-development-environment"></a>Preparación del entorno de desarrollo

En este inicio rápido, va a usar el [SDK de dispositivo IoT de Azure para C](iot-hub-device-sdk-c-intro.md). Va a preparar un entorno de desarrollo utilizado para clonar y compilar el [SDK de IoT de Azure para C](https://github.com/Azure/azure-iot-sdk-c) desde GitHub. El SDK de GitHub incluye el código de ejemplo usado en este inicio rápido.

1. Descargue el [sistema de compilación CMake](https://cmake.org/download/).

    Es importante que los requisitos previos de Visual Studio (Visual Studio y la carga de trabajo *Desarrollo para el escritorio con C++* ) estén instalados en la máquina. Cuando estén dispuestos los requisitos previos y haya comprobado la descarga, puede instalar el sistema de compilación de CMake.

2. Abra un símbolo del sistema o el shell de Bash de Git. Ejecute el siguiente comando para clonar el repositorio de GitHub del [SDK para C de Azure IoT](https://github.com/Azure/azure-iot-sdk-c):

    ```
    git clone https://github.com/Azure/azure-iot-sdk-c.git --recursive -b public-preview
    ```

    Esta operación puede tardar algunos minutos.

3. Cree un subdirectorio *cmake* en el directorio raíz del repositorio de Git, como se muestra en el comando siguiente, y vaya a esa carpeta.

    ```
    cd azure-iot-sdk-c
    mkdir cmake
    cd cmake
    ```

4. Ejecute el siguiente comando en el directorio *cmake* para compilar una versión del SDK específica para su plataforma de cliente de desarrollo.

   * En Linux:

      ```bash
      cmake ..
      make -j
      ```

   * En Windows, ejecute los siguientes comandos en el símbolo del sistema para desarrolladores correspondiente a Visual Studio 2015 o 2017. Se generará una solución de Visual Studio para el dispositivo simulado en el directorio *cmake*.

      ```cmd
      rem For VS2015
      cmake .. -G "Visual Studio 14 2015"

      rem Or for VS2017
      cmake .. -G "Visual Studio 15 2017"

      rem Then build the project
      cmake --build . -- /m /p:Configuration=Release
      ```

## <a name="create-an-iot-hub"></a>Crear un centro de IoT

[!INCLUDE [iot-hub-include-create-hub-device-streams](../../includes/iot-hub-include-create-hub-device-streams.md)]

## <a name="register-a-device"></a>Registrar un dispositivo

Debe registrar un dispositivo con IoT Hub antes de poder conectarlo. En esta sección, usará Azure Cloud Shell con la [extensión de IoT](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot?view=azure-cli-latest) para registrar un dispositivo simulado.

1. Ejecute el siguiente comando en Cloud Shell para crear la identidad del dispositivo:

   > [!NOTE]
   > * Reemplace el marcador de posición *YourIoTHubName* por el nombre que eligió para su centro de IoT.
   > * Use *MyDevice* tal como se muestra. Es el nombre que se da al dispositivo registrado. Si elige otro nombre para el dispositivo, úselo en todo el artículo y actualice el nombre del dispositivo en las aplicaciones de ejemplo antes de ejecutarlas.

    ```azurecli-interactive
    az iot hub device-identity create --hub-name YourIoTHubName --device-id MyDevice
    ```

2. Ejecute los siguientes comandos en Cloud Shell para obtener la *cadena de conexión del dispositivo* que acaba de registrar:

   > [!NOTE]
   > Reemplace el marcador de posición *YourIoTHubName* por el nombre que eligió para su centro de IoT.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name YourIoTHubName --device-id MyDevice --output table
    ```

    Anote la cadena de conexión del dispositivo para usarla más adelante en este inicio rápido. Debe ser similar al ejemplo siguiente:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyDevice;SharedAccessKey={YourSharedAccessKey}`

## <a name="communicate-between-the-device-and-the-service-via-device-streams"></a>Comunicación entre un dispositivo y un servicio mediante flujos de dispositivo

En esta sección, ejecuta la aplicación del lado del dispositivo y la aplicación del lado del servicio y se comunica entre las dos.

### <a name="run-the-device-side-application"></a>Ejecución de la aplicación del lado del dispositivo

Para ejecutar la aplicación del lado del dispositivo, realice las siguientes acciones:

1. Proporcione las credenciales del dispositivo editando el archivo de origen *iothub_client_c2d_streaming_sample.c* en la carpeta *iothub_client/samples/iothub_client_c2d_streaming_sample* y, a continuación, proporcione la cadena de conexión del dispositivo.

   ```C
   /* Paste in your iothub connection string  */
   static const char* connectionString = "[device connection string]";
   ```

2. Compile el código de la forma siguiente:

   ```bash
   # In Linux
   # Go to the sample's folder cmake/iothub_client/samples/iothub_client_c2d_streaming_sample
   make -j
   ```

   ```cmd
   rem In Windows
   rem Go to the cmake folder at the root of repo
   cmake --build . -- /m /p:Configuration=Release
   ```

3. Ejecute el programa compilado:

   ```bash
   # In Linux
   # Go to the sample's folder cmake/iothub_client/samples/iothub_client_c2d_streaming_sample
   ./iothub_client_c2d_streaming_sample
   ```

   ```cmd
   rem In Windows
   rem Go to the sample's release folder cmake\iothub_client\samples\iothub_client_c2d_streaming_sample\Release
   iothub_client_c2d_streaming_sample.exe
   ```

### <a name="run-the-service-side-application"></a>Ejecución de la aplicación del lado del servicio

Como ya se ha mencionado, el SDK de IoT Hub para C solo admite flujos de dispositivos en el lado del dispositivo. Para compilar y ejecutar la aplicación del lado del servicio, siga las instrucciones de uno de los siguientes inicios rápidos:

* [Comunicación con aplicaciones de dispositivo en C# mediante flujos de dispositivo de IoT Hub](./quickstart-device-streams-echo-csharp.md)
* [Comunicación con una aplicación de dispositivo en Node.js mediante flujos de dispositivo de IoT Hub](./quickstart-device-streams-echo-nodejs.md)

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [iot-hub-quickstarts-clean-up-resources-device-streams](../../includes/iot-hub-quickstarts-clean-up-resources-device-streams.md)]

## <a name="next-steps"></a>Pasos siguientes

En este artículo de inicio rápido, ha configurado un centro de IoT, ha registrado un dispositivo, ha establecido un flujo de dispositivos entre una aplicación C del dispositivo y otra aplicación del lado del servicio, y ha usado el flujo para enviar datos entre las aplicaciones.

Para más información sobre los flujos de dispositivo, consulte:

> [!div class="nextstepaction"]
> [Introducción a los flujos de dispositivo](./iot-hub-device-streams-overview.md)
