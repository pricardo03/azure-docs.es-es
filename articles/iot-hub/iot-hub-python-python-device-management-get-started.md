---
title: Introducción a la administración de dispositivos de Azure IoT Hub (Python) | Microsoft Docs
description: Describe cómo usar la administración de dispositivos de IoT Hub para iniciar un reinicio del dispositivo remoto. Usará el SDK de Azure IoT para Python con el fin de implementar una aplicación de dispositivo simulado que incluya un método directo y una aplicación de servicio que invoque el método directo.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.devlang: python
ms.topic: conceptual
ms.date: 07/30/2019
ms.author: robinsh
ms.openlocfilehash: daba6601c0da2bd6d1b76897e7ce1ff75da67b5b
ms.sourcegitcommit: fecb6bae3f29633c222f0b2680475f8f7d7a8885
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/30/2019
ms.locfileid: "68667966"
---
# <a name="get-started-with-device-management-python"></a>Introducción a la administración de dispositivos (Python)

[!INCLUDE [iot-hub-selector-dm-getstarted](../../includes/iot-hub-selector-dm-getstarted.md)]

En este tutorial se muestra cómo realizar las siguientes acciones:

* Usar Azure Portal para un IoT Hub y una identidad de dispositivo en este.

* Crear una aplicación de dispositivo simulado que contiene un método directo que reinicia ese dispositivo. Los métodos directos se invocan desde la nube.

* Crear una aplicación de consola de Python que llame al método directo de reinicio en la aplicación de dispositivo simulado mediante su centro de IoT.

Al final de este tutorial tendrá dos aplicaciones de consola de Python:

* **dmpatterns_getstarted_device.js**, que conecta al centro de IoT con la identidad del dispositivo creada anteriormente, recibe un método directo de reinicio, simula un reinicio físico y notifica la hora del último reinicio.

* **dmpatterns_getstarted_service.js**, que llama a un método directo en la aplicación de dispositivo simulado, muestra la respuesta y muestra las propiedades notificadas actualizadas.

[!INCLUDE [iot-hub-include-python-sdk-note](../../includes/iot-hub-include-python-sdk-note.md)]

A continuación se indican las instrucciones de instalación de los requisitos previos.

[!INCLUDE [iot-hub-include-python-installation-notes](../../includes/iot-hub-include-python-installation-notes.md)]

## <a name="create-an-iot-hub"></a>Crear un centro de IoT

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a>Creación de una aplicación de dispositivo simulado

En esta sección:

* Creará una aplicación de consola de Python que responda a un método directo que se llama desde la nube

* Simulará el reinicio de un dispositivo

* Usará las propiedades notificadas para permitir consultas de dispositivo gemelo a fin de identificar los dispositivos y cuándo se reiniciaron por última vez.

1. Con un editor de texto, cree un archivo **dmpatterns_getstarted_device.py**.

2. Agregue las siguientes instrucciones `import` al comienzo del archivo **dmpatterns_getstarted_device.py**.

    ```python
    import random
    import time, datetime
    import sys

    import iothub_client
    from iothub_client import IoTHubClient, IoTHubClientError, IoTHubTransportProvider, IoTHubClientResult, IoTHubError, DeviceMethodReturnValue
    ```

3. Agregue variables, como **CONNECTION_STRING** y la inicialización del cliente.  Reemplace la cadena de conexión por la cadena de conexión del dispositivo.  

    ```python
    CONNECTION_STRING = "{deviceConnectionString}"
    PROTOCOL = IoTHubTransportProvider.MQTT

    CLIENT = IoTHubClient(CONNECTION_STRING, PROTOCOL)

    WAIT_COUNT = 5

    SEND_REPORTED_STATE_CONTEXT = 0
    METHOD_CONTEXT = 0

    SEND_REPORTED_STATE_CALLBACKS = 0
    METHOD_CALLBACKS = 0
    ```

4. Agregue las siguientes devoluciones de llamada a la función para implementar el método directo en el dispositivo.

    ```python
    def send_reported_state_callback(status_code, user_context):
        global SEND_REPORTED_STATE_CALLBACKS

        print ( "Device twins updated." )

    def device_method_callback(method_name, payload, user_context):
        global METHOD_CALLBACKS

        if method_name == "rebootDevice":
            print ( "Rebooting device..." )
            time.sleep(20)

            print ( "Device rebooted." )

            current_time = str(datetime.datetime.now())
            reported_state = "{\"rebootTime\":\"" + current_time + "\"}"
            CLIENT.send_reported_state(reported_state, len(reported_state), send_reported_state_callback, SEND_REPORTED_STATE_CONTEXT)

            print ( "Updating device twins: rebootTime" )

        device_method_return_value = DeviceMethodReturnValue()
        device_method_return_value.response = "{ \"Response\": \"This is the response from the device\" }"
        device_method_return_value.status = 200

        return device_method_return_value
    ```

5. Inicie el agente de escucha de método directo y espere.

    ```python
    def iothub_client_init():
        if CLIENT.protocol == IoTHubTransportProvider.MQTT or client.protocol == IoTHubTransportProvider.MQTT_WS:
            CLIENT.set_device_method_callback(device_method_callback, METHOD_CONTEXT)

    def iothub_client_sample_run():
        try:
            iothub_client_init()

            while True:
                print ( "IoTHubClient waiting for commands, press Ctrl-C to exit" )

                status_counter = 0
                while status_counter <= WAIT_COUNT:
                    time.sleep(10)
                    status_counter += 1

        except IoTHubError as iothub_error:
            print ( "Unexpected error %s from IoTHub" % iothub_error )
            return
        except KeyboardInterrupt:
            print ( "IoTHubClient sample stopped" )

    if __name__ == '__main__':
        print ( "Starting the IoT Hub Python sample..." )
        print ( "    Protocol %s" % PROTOCOL )
        print ( "    Connection string=%s" % CONNECTION_STRING )

        iothub_client_sample_run()
    ```

6. Guarde y cierre el archivo **dmpatterns_getstarted_device.py**.

> [!NOTE]
> Por simplificar, este tutorial no implementa ninguna directiva de reintentos. En el código de producción, deberá implementar directivas de reintentos (por ejemplo, retroceso exponencial), tal y como se sugiere en el artículo [Control de errores transitorios](/azure/architecture/best-practices/transient-faults).

## <a name="get-the-iot-hub-connection-string"></a>Obtener la cadena de conexión de IoT Hub

[!INCLUDE [iot-hub-howto-device-management-shared-access-policy-text](../../includes/iot-hub-howto-device-management-shared-access-policy-text.md)]

[!INCLUDE [iot-hub-include-find-service-connection-string](../../includes/iot-hub-include-find-service-connection-string.md)]

## <a name="trigger-a-remote-reboot-on-the-device-using-a-direct-method"></a>Desencadenar un reinicio remoto en el dispositivo con un método directo

En esta sección, creará una aplicación de consola de Python que inicia un reinicio remoto en un dispositivo mediante un método directo. La aplicación usa las consultas gemelas de dispositivo para detectar la hora en que se reinició por última vez el dispositivo.

1. Con un editor de texto, cree un archivo **dmpatterns_getstarted_service.py**.

2. Agregue las siguientes instrucciones `import` al comienzo del archivo **dmpatterns_getstarted_service.py**.

    ```python
    import sys, time
    import iothub_service_client

    from iothub_service_client import IoTHubDeviceMethod, IoTHubError, IoTHubDeviceTwin
    ```

3. Agregue las siguientes declaraciones de variable. Reemplace solo los valores de marcador de posición de _IoTHubConnectionString_ y _deviceId_.

    ```python
    CONNECTION_STRING = "{IoTHubConnectionString}"
    DEVICE_ID = "{deviceId}"

    METHOD_NAME = "rebootDevice"
    METHOD_PAYLOAD = "{\"method_number\":\"42\"}"
    TIMEOUT = 60
    WAIT_COUNT = 10
    ```

4. Agregue la siguiente función para invocar el método de reinicio del dispositivo de destino, luego consulte los dispositivos gemelos y obtenga la hora del último reinicio.

    ```python
    def iothub_devicemethod_sample_run():
        try:
            iothub_twin_method = IoTHubDeviceTwin(CONNECTION_STRING)
            iothub_device_method = IoTHubDeviceMethod(CONNECTION_STRING)

            print ( "" )
            print ( "Invoking device to reboot..." )

            response = iothub_device_method.invoke(DEVICE_ID, METHOD_NAME, METHOD_PAYLOAD, TIMEOUT)

            print ( "" )
            print ( "Successfully invoked the device to reboot." )

            print ( "" )
            print ( response.payload )

            while True:
                print ( "" )
                print ( "IoTHubClient waiting for commands, press Ctrl-C to exit" )

                status_counter = 0
                while status_counter <= WAIT_COUNT:
                    twin_info = iothub_twin_method.get_twin(DEVICE_ID)

                    if twin_info.find("rebootTime") != -1:
                        print ( "Last reboot time: " + twin_info[twin_info.find("rebootTime")+11:twin_info.find("rebootTime")+37])
                    else:
                        print ("Waiting for device to report last reboot time...")

                    time.sleep(5)
                    status_counter += 1

        except IoTHubError as iothub_error:
            print ( "" )
            print ( "Unexpected error {0}".format(iothub_error) )
            return
        except KeyboardInterrupt:
            print ( "" )
            print ( "IoTHubDeviceMethod sample stopped" )

    if __name__ == '__main__':
        print ( "Starting the IoT Hub Service Client DeviceManagement Python sample..." )
        print ( "    Connection string = {0}".format(CONNECTION_STRING) )
        print ( "    Device ID         = {0}".format(DEVICE_ID) )

        iothub_devicemethod_sample_run()
    ```

5. Guarde y cierre el archivo **dmpatterns_getstarted_service.py**.

## <a name="run-the-apps"></a>Ejecución de las aplicaciones

Ya está preparado para ejecutar las aplicaciones.

1. En el símbolo del sistema, ejecute el siguiente comando para iniciar la escucha del método directo de reinicio.

    ```
    python dmpatterns_getstarted_device.py
    ```

2. En otro símbolo del sistema, ejecute el siguiente comando para desencadenar el reinicio remoto y la consulta en el dispositivo gemelo para buscar la hora del último reinicio.

    ```
    python dmpatterns_getstarted_service.py
    ```

3. Verá la respuesta del dispositivo al método directo en la consola.

[!INCLUDE [iot-hub-dm-followup](../../includes/iot-hub-dm-followup.md)]