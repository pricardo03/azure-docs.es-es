---
title: Introducción a la identidad de módulo y a los módulos gemelos de Azure IoT Hub (Python) | Microsoft Docs
description: Aprenda a crear la identidad de módulo y a actualizar módulos gemelos mediante los SDK de IoT para Python.
author: chrissie926
ms.service: iot-hub
services: iot-hub
ms.devlang: python
ms.topic: conceptual
ms.date: 07/30/2019
ms.author: menchi
ms.openlocfilehash: b54b81a78072664ef47ace917fb3db85a5b1f297
ms.sourcegitcommit: aaa82f3797d548c324f375b5aad5d54cb03c7288
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/29/2019
ms.locfileid: "70147622"
---
# <a name="get-started-with-iot-hub-module-identity-and-module-twin-python"></a>Introducción a la identidad de módulo y a los módulos gemelos de IoT Hub (Python)

[!INCLUDE [iot-hub-selector-module-twin-getstarted](../../includes/iot-hub-selector-module-twin-getstarted.md)]

> [!NOTE]
> [Las identidades de módulo y los módulos gemelos](iot-hub-devguide-module-twins.md) son similares a la identidad de dispositivo y el dispositivo gemelo de Azure IoT Hub, pero ofrecen granularidad más fina. Aunque la identidad de módulo y los dispositivos gemelos de Azure IoT Hub permiten que la aplicación back-end configure un dispositivo y proporcionan visibilidad para las condiciones del dispositivo, la identidad de módulo y los módulos gemelos proporcionan estas funcionalidades para los componentes individuales del dispositivo. En los dispositivos compatibles con varios componentes, como dispositivos con sistema operativo o firmware, permiten la configuración individual y condiciones específicas por componente.
>

Al final de este tutorial, tiene dos aplicaciones de Python:

* **CreateIdentities**, que crea una identidad de dispositivo, una de módulo y la clave de seguridad asociada para conectar el dispositivo y los clientes del módulo.

* **UpdateModuleTwinReportedProperties**, que envía las propiedades notificadas por el módulo gemelo actualizadas a IoT Hub.

[!INCLUDE [iot-hub-include-python-sdk-note](../../includes/iot-hub-include-python-sdk-note.md)]

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [iot-hub-include-python-installation-notes](../../includes/iot-hub-include-python-installation-notes.md)]

## <a name="create-an-iot-hub"></a>Crear un centro de IoT

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="get-the-iot-hub-connection-string"></a>Obtención de la cadena de conexión de IoT Hub

[!INCLUDE [iot-hub-howto-module-twin-shared-access-policy-text](../../includes/iot-hub-howto-module-twin-shared-access-policy-text.md)]

[!INCLUDE [iot-hub-include-find-registryrw-connection-string](../../includes/iot-hub-include-find-registryrw-connection-string.md)]

## <a name="create-a-device-identity-and-a-module-identity-in-iot-hub"></a>Crear una identidad del dispositivo y una identidad de módulo en IoT Hub

En esta sección se crea una aplicación de Python que crea a su vez una identidad de dispositivo y otra de módulo en el registro de identidades de la instancia de IoT Hub. No se puede conectar un dispositivo o un módulo a IoT Hub a menos que tenga una entrada en el registro de identidades. Para más información, consulte la sección "Registro de identidades" de la [guía para desarrolladores de IoT Hub](iot-hub-devguide-identity-registry.md). Cuando se ejecuta esta aplicación de consola, genera una clave y un identificador únicos para el dispositivo y el módulo. El dispositivo y el módulo usan estos valores para identificarse al enviar mensajes del dispositivo a la nube a IoT Hub. Los identificadores distinguen mayúsculas de minúsculas.

Agregue el siguiente código al archivo de Python:

```python
import sys
import iothub_service_client
from iothub_service_client import IoTHubRegistryManager, IoTHubRegistryManagerAuthMethod, IoTHubError

CONNECTION_STRING = "YourConnString"
DEVICE_ID = "myFirstDevice"
MODULE_ID = "myFirstModule"

try:
    # RegistryManager
    iothub_registry_manager = IoTHubRegistryManager(CONNECTION_STRING)

    # CreateDevice
    primary_key = ""
    secondary_key = ""
    auth_method = IoTHubRegistryManagerAuthMethod.SHARED_PRIVATE_KEY
    new_device = iothub_registry_manager.create_device(
        DEVICE_ID, primary_key, secondary_key, auth_method)
    print("new_device <" + DEVICE_ID +
          "> has primary key = " + new_device.primaryKey)

    # CreateModule
    new_module = iothub_registry_manager.create_module(
        DEVICE_ID, primary_key, secondary_key, MODULE_ID, auth_method)
    print("device/new_module <" + DEVICE_ID + "/" + MODULE_ID +
          "> has primary key = " + new_module.primaryKey)

except IoTHubError as iothub_error:
    print("Unexpected error {0}".format(iothub_error))
except KeyboardInterrupt:
    print("IoTHubRegistryManager sample stopped")
```

Esta aplicación crea una identidad del dispositivo con el identificador **myFirstDevice** y una de módulo con el identificador **myFirstModule** en el dispositivo **myFirstDevice**. (Si el identificador de módulo ya existe en el Registro de identidad, el código simplemente recupera la información existente del módulo). A continuación, la aplicación muestra la clave principal de esa identidad. Esta clave se usará en la aplicación de módulo simulado para conectarse a IoT Hub.

> [!NOTE]
> El registro de identidades de IoT Hub solo almacena identidades de dispositivos y módulos para permitir el acceso seguro a IoT Hub. El registro de identidades almacena los identificadores y las claves de dispositivo para usarlas como credenciales de seguridad. El registro de identidades también almacena una marca de habilitado o deshabilitado de cada dispositivo que se puede usar para deshabilitar el acceso a dicho dispositivo. Si la aplicación necesita almacenar otros metadatos específicos del dispositivo, debe usar un almacén específico de la aplicación. No hay marcas de habilitado/deshabilitado para las identidades de módulo. Consulte la [guía de desarrolladores de IoT Hub](iot-hub-devguide-identity-registry.md) para obtener más información.
>

## <a name="update-the-module-twin-using-python-device-sdk"></a>Actualizar el módulo gemelo con el SDK del dispositivo de Python

En esta sección se crea una aplicación de Python en el dispositivo simulado que actualiza las propiedades notificadas del módulo gemelo.

1. **Obtenga la cadena de conexión del módulo**: para ello, inicie sesión en [Azure Portal](https://portal.azure.com/). Vaya a la instancia de IoT Hub y haga clic en Dispositivos IoT. Busque myFirstDevice, ábralo y verá que myFirstModule se creó correctamente. Copie la cadena de conexión del módulo, la necesitará en el paso siguiente.

   ![Detalles del módulo en Azure Portal](./media/iot-hub-python-python-module-twin-getstarted/module-detail.png)

2. **Cree la aplicación UpdateModuleTwinReportedProperties**.

   Agregue las siguientes instrucciones `using` al principio del archivo **Program.cs** :

    ```python
    import sys
    import iothub_service_client
    from iothub_service_client import IoTHubRegistryManager, IoTHubRegistryManagerAuthMethod, IoTHubDeviceTwin, IoTHubError

    CONNECTION_STRING = "FILL IN CONNECTION STRING"
    DEVICE_ID = "MyFirstDevice"
    MODULE_ID = "MyFirstModule"

    UPDATE_JSON = "{\"properties\":{\"desired\":{\"telemetryInterval\":122}}}"

    try:
        iothub_twin = IoTHubDeviceTwin(CONNECTION_STRING)

        twin_info = iothub_twin.get_twin(DEVICE_ID, MODULE_ID)
        print ( "" )
        print ( "Twin before update    :" )
        print ( "{0}".format(twin_info) )

        twin_info_updated = iothub_twin.update_twin(DEVICE_ID, MODULE_ID, UPDATE_JSON)
        print ( "" )
        print ( "Twin after update     :" )
        print ( "{0}".format(twin_info_updated) )

    except IoTHubError as iothub_error:
        print ( "Unexpected error {0}".format(iothub_error) )
    except KeyboardInterrupt:
        print ( "IoTHubRegistryManager sample stopped" )
    ```

Este ejemplo de código muestra cómo recuperar el módulo gemelo y actualizar las propiedades notificadas con el protocolo AMQP.

## <a name="get-updates-on-the-device-side"></a>Obtener actualizaciones en el lado de dispositivo

Además del código anterior, puede agregar el siguiente bloque de código para obtener el mensaje de actualización gemela en el dispositivo.

```python
import random
import time
import sys
import iothub_client
from iothub_client import IoTHubModuleClient, IoTHubClientError, IoTHubTransportProvider, IoTHubClientResult

PROTOCOL = IoTHubTransportProvider.AMQP
CONNECTION_STRING = ""


def module_twin_callback(update_state, payload, user_context):
    print("")
    print("Twin callback called with:")
    print("updateStatus: %s" % update_state)
    print("context: %s" % user_context)
    print("payload: %s" % payload)


try:
    module_client = IoTHubModuleClient(CONNECTION_STRING, PROTOCOL)
    module_client.set_module_twin_callback(module_twin_callback, 1234)

    print("Waiting for incoming twin messages.  Hit Control-C to exit.")
    while True:

        time.sleep(1000000)

except IoTHubError as iothub_error:
    print("Unexpected error {0}".format(iothub_error))
except KeyboardInterrupt:
    print("module client sample stopped")
```

## <a name="next-steps"></a>Pasos siguientes

Para continuar la introducción a IoT Hub y explorar otros escenarios de IoT, consulte:

* [Introducción a la administración de dispositivos](iot-hub-node-node-device-management-get-started.md)

* [Introducción a IoT Edge](../iot-edge/tutorial-simulate-device-linux.md)