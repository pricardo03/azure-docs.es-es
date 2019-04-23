---
title: Mensajes de nube a dispositivo con IoT Hub de Azure (Python) | Microsoft Docs
description: Cómo enviar mensajes de nube a un dispositivo de una instancia de IoT Hub de Azure mediante los SDK de IoT de Azure para Python. Modifique una aplicación de dispositivo simulado para recibir mensajes de nube a dispositivo y cambie una aplicación de back-end para enviar los mensajes de nube a dispositivo.
author: kgremban
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.devlang: python
ms.topic: conceptual
ms.date: 02/22/2019
ms.author: kgremban
ms.openlocfilehash: 7ac668bdbc3698be3ed2aa50a428cef84e68369a
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59789795"
---
# <a name="send-cloud-to-device-messages-with-iot-hub-python"></a>Envío de mensajes de nube a dispositivo con IoT Hub (Python)

[!INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## <a name="introduction"></a>Introducción
IoT Hub de Azure es un servicio totalmente administrado que permite la comunicación bidireccional confiable y segura entre millones de dispositivos y una solución de back-end. El [empezar a trabajar con IoT Hub](quickstart-send-telemetry-python.md) inicio rápido muestra cómo crear un centro de IoT, aprovisionar una identidad de dispositivo en él y codificar una aplicación de dispositivo simulado que envía mensajes de dispositivo a nube.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Este tutorial se basa en la [Introducción a Iot Hub](quickstart-send-telemetry-python.md). En él se muestra cómo realizar las siguientes acciones:

* Desde el back-end de la nube de la aplicación, envíe mensajes de nube a dispositivo en un único dispositivo a través de IoT Hub.

* Reciba mensajes de nube a dispositivo en un dispositivo.

* Desde el back-end de la nube de la aplicación, solicite confirmación de entrega (*comentarios*) para los mensajes enviados a un dispositivo desde IoT Hub.

Puede encontrar más información sobre los mensajes de nube a dispositivo en el [Guía del desarrollador de IoT Hub](iot-hub-devguide-messaging.md).

Al final de este tutorial tendrá dos aplicaciones de consola de Python:

* **SimulatedDevice.py**, versión modificada de la aplicación creada en [Introducción a IoT Hub](quickstart-send-telemetry-python.md), que se conecta a IoT Hub y recibe mensajes de nube a dispositivo.

* **SendCloudToDeviceMessage.py**, que envía un mensaje de la nube a la aplicación de dispositivo simulado mediante IoT Hub y, luego, recibe su confirmación de entrega.

> [!NOTE]
> El Centro de IoT ofrece compatibilidad con el SDK en muchas plataformas de dispositivos y lenguajes (incluido C, Java y Javascript), mediante los SDK de dispositivos IoT de Azure. Visite el [Centro para desarrolladores de IoT de Azure](https://www.azure.com/develop/iot)para obtener instrucciones paso a paso sobre cómo conectar el dispositivo al código de este tutorial y, en general, al Centro de IoT de Azure.
>

Para completar este tutorial, necesitará lo siguiente:

* [Python 2.x o 3.x](https://www.python.org/downloads/). Asegúrese de usar la instalación de 32 bits o 64 bits en función del programa de instalación. Cuando se le solicite durante la instalación, asegúrese de agregar Python a la variable de entorno específica de la plataforma. Si usa Python 2.x, puede que necesite [instalar o actualizar *pip*, el sistema de administración de paquetes de Python](https://pip.pypa.io/en/stable/installing/).

* Si usa el sistema operativo Windows, puede que necesite el [paquete redistribuible de Visual C++](https://www.microsoft.com/download/confirmation.aspx?id=48145) para permitir el uso de archivos DLL nativos de Python.

* Una cuenta de Azure activa. (En caso de no tener ninguna, puede crear una [cuenta gratuita](https://azure.microsoft.com/pricing/free-trial/) en tan solo unos minutos).

> [!NOTE]
> Los paquetes *pip* de `azure-iothub-service-client` y `azure-iothub-device-client` actualmente solo están disponibles para el sistema operativo Windows. Para Linux o Mac OS, consulte las secciones específicas de Mac OS y Linux en el [preparar el entorno de desarrollo para Python](https://github.com/Azure/azure-iot-sdk-python/blob/master/doc/python-devbox-setup.md) registrar.
>

## <a name="receive-messages-in-the-simulated-device-app"></a>Recepción de mensajes en la aplicación de dispositivo simulado

En esta sección, creará una aplicación de consola de Python para simular el dispositivo y recibir mensajes de la nube al dispositivo desde IoT Hub.

1. Con un editor de texto, cree un archivo **SimulatedDevice.py**.

2. Agregue las siguientes instrucciones y variables `import` al principio del archivo **SimulatedDevice.py**:

   ```python
    import time
    import sys
    import iothub_client
    from iothub_client import IoTHubClient, IoTHubClientError, IoTHubTransportProvider, IoTHubClientResult
    from iothub_client import IoTHubMessage, IoTHubMessageDispositionResult, IoTHubError

    RECEIVE_CONTEXT = 0
    WAIT_COUNT = 10
    RECEIVED_COUNT = 0
    RECEIVE_CALLBACKS = 0
    ```

3. Agregue el código siguiente al archivo **SimulatedDevice.py**. Reemplace el valor de marcador de posición "{deviceConnectionString}" con la cadena de conexión de dispositivo para el dispositivo que creó en el [empezar a trabajar con IoT Hub](quickstart-send-telemetry-python.md) inicio rápido:

    ```python
    # choose AMQP or AMQP_WS as transport protocol
    PROTOCOL = IoTHubTransportProvider.AMQP
    CONNECTION_STRING = "{deviceConnectionString}"
    ```

4. Agregue la función siguiente para imprimir los mensajes recibidos en la consola:

    ```python
    def receive_message_callback(message, counter):
        global RECEIVE_CALLBACKS
        message_buffer = message.get_bytearray()
        size = len(message_buffer)
        print ( "Received Message [%d]:" % counter )
        print ( "    Data: <<<%s>>> & Size=%d" % (message_buffer[:size].decode('utf-8'), size) )
        map_properties = message.properties()
        key_value_pair = map_properties.get_internals()
        print ( "    Properties: %s" % key_value_pair )
        counter += 1
        RECEIVE_CALLBACKS += 1
        print ( "    Total calls received: %d" % RECEIVE_CALLBACKS )
        return IoTHubMessageDispositionResult.ACCEPTED

    def iothub_client_init():
        client = IoTHubClient(CONNECTION_STRING, PROTOCOL)

        client.set_message_callback(receive_message_callback, RECEIVE_CONTEXT)

        return client

    def print_last_message_time(client):
        try:
            last_message = client.get_last_message_receive_time()
            print ( "Last Message: %s" % time.asctime(time.localtime(last_message)) )
            print ( "Actual time : %s" % time.asctime() )
        except IoTHubClientError as iothub_client_error:
            if iothub_client_error.args[0].result == IoTHubClientResult.INDEFINITE_TIME:
                print ( "No message received" )
            else:
                print ( iothub_client_error )
    ```

5. Agregue el código siguiente para inicializar el cliente y espere a recibir el mensaje de la nube al dispositivo:

    ```python
    def iothub_client_init():
        client = IoTHubClient(CONNECTION_STRING, PROTOCOL)

        client.set_message_callback(receive_message_callback, RECEIVE_CONTEXT)

        return client

    def iothub_client_sample_run():
        try:
            client = iothub_client_init()

            while True:
                print ( "IoTHubClient waiting for commands, press Ctrl-C to exit" )

                status_counter = 0
                while status_counter <= WAIT_COUNT:
                    status = client.get_send_status()
                    print ( "Send status: %s" % status )
                    time.sleep(10)
                    status_counter += 1

        except IoTHubError as iothub_error:
            print ( "Unexpected error %s from IoTHub" % iothub_error )
            return
        except KeyboardInterrupt:
            print ( "IoTHubClient sample stopped" )

        print_last_message_time(client)
    ```

6. Agregue la siguiente función main:

    ```python
    if __name__ == '__main__':
        print ( "Starting the IoT Hub Python sample..." )
        print ( "    Protocol %s" % PROTOCOL )
        print ( "    Connection string=%s" % CONNECTION_STRING )

        iothub_client_sample_run()
    ```

7. Guarde y cierre el archivo **SimulatedDevice.py**.

## <a name="send-a-cloud-to-device-message"></a>Envío de mensajes de nube a dispositivo

En esta sección, escribirá una aplicación de consola de Python que envía mensajes de nube a dispositivo a la aplicación del dispositivo simulado. Necesitará el identificador de dispositivo del dispositivo que agregó en el [empezar a trabajar con IoT Hub](quickstart-send-telemetry-python.md) Guía de inicio rápido. También necesita la cadena de conexión para IoT Hub que encontrará en [Azure Portal](https://portal.azure.com).

1. Con un editor de texto, cree un archivo **SendCloudToDeviceMessage.py**.

2. Agregue las siguientes instrucciones y variables `import` al principio del archivo **SendCloudToDeviceMessage.py**:

    ```python
    import random
    import sys
    import iothub_service_client
    from iothub_service_client import IoTHubMessaging, IoTHubMessage, IoTHubError

    OPEN_CONTEXT = 0
    FEEDBACK_CONTEXT = 1
    MESSAGE_COUNT = 1
    AVG_WIND_SPEED = 10.0
    MSG_TXT = "{\"service client sent a message\": %.2f}"
    ```

3. Agregue el código siguiente al archivo **SendCloudToDeviceMessage.py**. Reemplace el valor de marcador de posición "{IoTHubConnectionString}" con la cadena de conexión de IoT Hub para el centro que creó en el [empezar a trabajar con IoT Hub](quickstart-send-telemetry-python.md) Guía de inicio rápido. Reemplace el marcador de posición "{deviceId}" con el identificador de dispositivo del dispositivo que agregó en el [empezar a trabajar con IoT Hub](quickstart-send-telemetry-python.md) inicio rápido:

    ```python
    CONNECTION_STRING = "{IoTHubConnectionString}"
    DEVICE_ID = "{deviceId}"
    ```

4. Agregue la función siguiente para imprimir mensajes de comentarios en la consola:

    ```python
    def open_complete_callback(context):
        print ( 'open_complete_callback called with context: {0}'.format(context) )

    def send_complete_callback(context, messaging_result):
        context = 0
        print ( 'send_complete_callback called with context : {0}'.format(context) )
        print ( 'messagingResult : {0}'.format(messaging_result) )
    ```

5. Agregue el código siguiente para enviar un mensaje a su dispositivo y procesar el mensaje de comentarios cuando el dispositivo reconozca el mensaje de la nube al dispositivo:

    ```python
    def iothub_messaging_sample_run():
        try:
            iothub_messaging = IoTHubMessaging(CONNECTION_STRING)

            iothub_messaging.open(open_complete_callback, OPEN_CONTEXT)

            for i in range(0, MESSAGE_COUNT):
                print ( 'Sending message: {0}'.format(i) )
                msg_txt_formatted = MSG_TXT % (AVG_WIND_SPEED + (random.random() * 4 + 2))
                message = IoTHubMessage(bytearray(msg_txt_formatted, 'utf8'))

                # optional: assign ids
                message.message_id = "message_%d" % i
                message.correlation_id = "correlation_%d" % i
                # optional: assign properties
                prop_map = message.properties()
                prop_text = "PropMsg_%d" % i
                prop_map.add("Property", prop_text)

                iothub_messaging.send_async(DEVICE_ID, message, send_complete_callback, i)

            try:
                # Try Python 2.xx first
                raw_input("Press Enter to continue...\n")
            except:
                pass
                # Use Python 3.xx in the case of exception
                input("Press Enter to continue...\n")

            iothub_messaging.close()

        except IoTHubError as iothub_error:
            print ( "Unexpected error {0}" % iothub_error )
            return
        except KeyboardInterrupt:
            print ( "IoTHubMessaging sample stopped" )
    ```

6. Agregue la siguiente función main:

    ```python
    if __name__ == '__main__':
        print ( "Starting the IoT Hub Service Client Messaging Python sample..." )
        print ( "    Connection string = {0}".format(CONNECTION_STRING) )
        print ( "    Device ID         = {0}".format(DEVICE_ID) )

        iothub_messaging_sample_run()
    ```

7. Guarde y cierre el archivo **SendCloudToDeviceMessage.py**.

## <a name="run-the-applications"></a>Ejecución de las aplicaciones

Ahora está preparado para ejecutar las aplicaciones.

1. Abra un símbolo del sistema e instale el **SDK de dispositivo de Azure IoT Hub para Python**.

    ```
    pip install azure-iothub-device-client
    ```

2. En el símbolo del sistema, ejecute el comando siguiente para escuchar mensajes de nube a dispositivo:

    ```shell
    python SimulatedDevice.py
    ```

    ![Ejecución de una aplicación de dispositivo simulada](./media/iot-hub-python-python-c2d/simulated-device.png)

3. Abra un nuevo símbolo del sistema e instale el **SDK del servicio Azure IoT Hub para Python**.

    ```
    pip install azure-iothub-service-client
    ```

4. En un símbolo del sistema, ejecute el comando siguiente para enviar un mensaje de nube a dispositivo y espere el comentario del mensaje:

    ```shell
    python SendCloudToDeviceMessage.py
    ```

    ![Ejecución de la aplicación para enviar el comando de nube a dispositivo](./media/iot-hub-python-python-c2d/send-command.png)

5. Anote el mensaje que recibirá en el dispositivo.

    ![Mensaje recibido](./media/iot-hub-python-python-c2d/message-received.png)

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, aprendió a enviar y recibir mensajes de nube a dispositivo.

Para ver ejemplos de soluciones de un extremo a otro que usen IoT Hub, vea el [Acelerador de la solución de supervisión remota de Azure IoT](https://azure.microsoft.com/documentation/suites/iot-suite/).

Para obtener más información sobre cómo desarrollar soluciones con IoT Hub, consulte la [Guía para desarrolladores de IoT Hub](iot-hub-devguide.md).
