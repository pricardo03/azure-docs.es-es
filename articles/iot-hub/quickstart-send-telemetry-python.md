---
title: Inicio rápido para enviar datos de telemetría a Azure IoT Hub (Python) | Microsoft Docs
description: En esta guía de inicio rápido, ejecutará una aplicación Python de ejemplo para enviar datos de telemetría simulados a un centro de IoT y usará una utilidad para leer los datos de telemetría procedentes del centro de IoT.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.devlang: python
ms.topic: quickstart
ms.custom: mvc
ms.date: 02/28/2019
ms.openlocfilehash: a8abd71609d3e063c92541485007a3bde44be954
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67051240"
---
# <a name="quickstart-send-telemetry-from-a-device-to-an-iot-hub-and-read-it-with-a-back-end-application-python"></a>Inicio rápido: Envío de telemetría desde un dispositivo a un centro de IoT y su lectura con una aplicación de back-end (Python)

[!INCLUDE [iot-hub-quickstarts-1-selector](../../includes/iot-hub-quickstarts-1-selector.md)]

IoT Hub es un servicio de Azure que le permite ingerir grandes volúmenes de datos de telemetría desde los dispositivos IoT en la nube para su almacenamiento o procesamiento. En este inicio rápido, enviará datos de telemetría desde una aplicación de dispositivo simulado, por IoT Hub, a una aplicación back-end para su procesamiento.

En este inicio rápido se usa una aplicación Python escrita previamente para enviar los datos de telemetría y una utilidad CLI para leer los datos de telemetría procedentes del centro. Antes de ejecutar estas dos aplicaciones, creará un centro de IoT Hub y registrará un dispositivo con el centro.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

La aplicación de ejemplo que se ejecuta en este inicio rápido se ha escrito en Python. Actualmente, los SDK de IoT de Microsoft Azure para Python admiten solo versiones específicas de Python en cada plataforma. Para más información, consulte el [archivo Léame del SDK de Python](https://github.com/Azure/azure-iot-sdk-python#important-installation-notes---dealing-with-importerror-issues).

En este inicio rápido se supone que usa una máquina de desarrollo de Windows. En sistemas Windows, solo se admite [Python 3.6](https://www.python.org/downloads/release/python-368/). El instalador de Python que elija debe basarse en la arquitectura del sistema con el que trabaja. Si la arquitectura de la CPU del sistema es de 32 bits, descargue el instalador x86; para arquitecturas de 64 bits, descargue el instalador x86-64. Además, asegúrese de que [Microsoft Visual C++ Redistributable para Visual Studio 2019](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads) esté instalado de acuerdo con su arquitectura (x86 o x64).

Puede descargar Python para otras plataformas en [Python.org](https://www.python.org/downloads/).

Puede verificar la versión actual de Python en el equipo de desarrollo con uno de los comandos siguientes:

```python
python --version
```

```python
python3 --version
```

Ejecute el siguiente comando para agregar la extensión IoT de Microsoft Azure para la CLI de Azure a la instancia de Cloud Shell. La extensión IOT agrega comandos específicos de IoT Hub, IoT Edge e IoT Device Provisioning Service (DPS) a la CLI de Azure.

```azurecli-interactive
az extension add --name azure-cli-iot-ext
```

Descargue el proyecto de muestra de Python desde https://github.com/Azure-Samples/azure-iot-samples-python/archive/master.zip y extraiga el archivo ZIP.

## <a name="create-an-iot-hub"></a>Crear un centro de IoT

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-device"></a>Registrar un dispositivo

Debe registrar un dispositivo con IoT Hub antes de poder conectarlo. En esta guía de inicio rápido, usará Azure Cloud Shell para registrar un dispositivo simulado.

1. Ejecute los siguientes comandos en Azure Cloud Shell para crear la identidad del dispositivo.

    **YourIoTHubName**: reemplace este marcador de posición por el nombre elegido para el centro de IoT.

    **MyPythonDevice**: nombre que se da al dispositivo registrado. Use MyPythonDevice como se muestra. Si elige otro nombre para el dispositivo, tendrá que usarlo en todo el artículo y actualizar el nombre del dispositivo en las aplicaciones de ejemplo antes de ejecutarlas.

    ```azurecli-interactive
    az iot hub device-identity create --hub-name YourIoTHubName --device-id MyPythonDevice
    ```

1. Ejecute los siguientes comandos en Azure Cloud Shell para obtener la _cadena de conexión del dispositivo_ que registró:

    **YourIoTHubName**: reemplace este marcador de posición por el nombre elegido para el centro de IoT.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name YourIoTHubName --device-id MyPythonDevice --output table
    ```

    Anote la cadena de conexión del dispositivo, que se parecerá a esta:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyNodeDevice;SharedAccessKey={YourSharedAccessKey}`

    Usará este valor más adelante en este inicio rápido.

## <a name="send-simulated-telemetry"></a>Envío de datos de telemetría simulados

La aplicación de dispositivo simulado se conecta a un punto de conexión específico del dispositivo en IoT Hub y envía datos de telemetría simulados sobre temperatura y humedad.

1. En una ventana de terminal local, vaya a la carpeta raíz del proyecto de Python de muestra. A continuación, vaya a la carpeta **iot-hub\Quickstarts\simulated-device**.

1. Abra el archivo **SimulatedDevice.py** en el editor de texto de su elección.

    Reemplace el valor de la variable `CONNECTION_STRING` por la cadena de conexión de dispositivo que anotó anteriormente. A continuación, guarde los cambios realizados en el archivo **SimulatedDevice.py**.

1. En la ventana de terminal local, ejecute los comandos siguientes para instalar las bibliotecas necesarias para la aplicación de dispositivo simulado:

    ```cmd/sh
    pip install azure-iothub-device-client
    ```

1. En la ventana de terminal local, ejecute los comandos siguientes para ejecutar la aplicación de dispositivo simulado:

    ```cmd/sh
    python SimulatedDevice.py
    ```

    La siguiente captura de pantalla muestra la salida en la que la aplicación de dispositivo simulado envía datos de telemetría a IoT Hub:

    ![Ejecutar el dispositivo simulado](media/quickstart-send-telemetry-python/SimulatedDevice.png)
    
### <a name="to-avoid-the-import-iothubclient-error"></a>Para evitar el error de importación de iothub_client
La versión actual del SDK de Azure IoT para Python es un contenedor sobre [el SDK de C](https://github.com/azure/azure-iot-sdk-c). Se genera mediante la biblioteca [Boost](https://www.boost.org/). Por ese motivo, incluye varias limitaciones importantes. Consulte más detalles [aquí](https://github.com/Azure/azure-iot-sdk-python#important-installation-notes---dealing-with-importerror-issues).

1. Compruebe que tiene la versión correcta de [Python](https://github.com/Azure/azure-iot-sdk-python#important-installation-notes---dealing-with-importerror-issues). Tenga en cuenta que solo ciertas versiones funcionan correctamente para este ejemplo. 
2. Compruebe que tiene la versión correcta del entorno en tiempo de ejecución de C++ de [Microsoft Visual C++ Redistributable para Visual Studio 2019](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads). (Se recomienda la versión más reciente).
3. Compruebe que tiene instalado el cliente IoTHub: `pip install azure-iothub-device-client`.

## <a name="read-the-telemetry-from-your-hub"></a>Lectura de los datos de telemetría procedentes de su instancia de IoT Hub

La extensión de la CLI de IoT Hub se puede conectar al punto de conexión **Eventos** del lado servicio en su instancia de IoT Hub. La extensión recibe los mensajes del dispositivo a la nube enviados desde el dispositivo simulado. Normalmente, una aplicación back-end de IoT Hub se ejecuta en la nube para recibir y procesar los mensajes del dispositivo a la nube.

Ejecute los siguientes comandos en Azure Cloud Shell y reemplace `YourIoTHubName` por el nombre del centro de IoT:

```azurecli-interactive
az iot hub monitor-events --hub-name YourIoTHubName --device-id MyPythonDevice 
```

En la siguiente captura de pantalla se muestra la salida en la que la extensión recibe los datos de telemetría que el dispositivo simulado ha enviado al centro:

![Ejecutar la aplicación back-end](media/quickstart-send-telemetry-python/ReadDeviceToCloud.png)

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources.md)]

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido, ha configurado IoT Hub, registrado un dispositivo, enviado datos de telemetría simulados al centro con una aplicación Python y leído datos de telemetría desde el centro con una aplicación back-end básica.

Para aprender a controlar el dispositivo simulado desde una aplicación back-end, vaya al siguiente inicio rápido.

> [!div class="nextstepaction"]
> [Inicio rápido: Control de un dispositivo conectado a IoT Hub](quickstart-control-device-python.md)
