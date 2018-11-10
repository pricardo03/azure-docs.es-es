---
title: Entrada de datos de telemetría y conectividad de dispositivo con Azure Digital Twins | Microsoft Docs
description: Información general de incorporación de un dispositivo con Azure Digital Twins
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/26/2018
ms.author: alinast
ms.openlocfilehash: 7eddea7e0d57b89318232da6f086bbe2f649ee77
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2018
ms.locfileid: "50211934"
---
# <a name="device-connectivity-and-telemetry-ingress"></a>Entrada de datos de telemetría y conectividad de dispositivo

Los datos de telemetría enviados por los dispositivos y sensores son la columna de cualquier solución de IoT. Por lo tanto, representar estos recursos diferentes y administrarlos en el contexto de una ubicación son una de las principales preocupaciones en el desarrollo de aplicaciones de IoT. Azure Digital Twins simplifica el desarrollo de soluciones de IoT al unir dispositivos y sensores con un grafo de inteligencia espacial.

Para empezar, se debe crear un recurso de IoT Hub en la raíz del grafo espacial, lo que permite que todos los dispositivos bajo el espacio raíz envíen mensajes. Una vez que se ha creado la instancia de IoT Hub y los dispositivos con sensores se han registrado en la instancia de Digital Twins, los dispositivos pueden empezar a enviar datos a un servicio de Digital Twins a través del [SDK de dispositivo IoT de Azure](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-sdks#azure-iot-device-sdks).

En el [tutorial para implementar y configurar Digital Twins](tutorial-facilities-setup.md) se puede encontrar una guía paso a paso para incorporar dispositivos. En resumen, los pasos son:

- Implementar una instancia de Azure Digital Twins desde [Azure Portal](https://portal.azure.com)
- Creación espacios en el grafo
- Crear un recurso de IoT Hub y asignarlo a un espacio en el grafo
- Crear dispositivos y sensores en el grafo y asignarlos a los espacios creados en los pasos anteriores
- Crear un buscador de coincidencias para filtrar los mensajes de telemetría en función de las condiciones
- Crear una [función definida por el usuario](concepts-user-defined-functions.md) y asignarla a un espacio en el grafo para el procesamiento personalizado de los mensajes de telemetría
- Asignar un rol para permitir que la función definida por el usuario acceda a los datos del grafo
- Obtener la cadena de conexión del dispositivo de IoT Hub de las API de administración de Digital Twins
- Configurar la cadena de conexión de dispositivo en el dispositivo con el SDK de dispositivo IoT de Azure

A continuación obtendrá información sobre cómo obtener la cadena de conexión del dispositivo de IoT Hub de la API de administración de Digital Twins y cómo usar el formato de mensaje de telemetría de IoT Hub para enviar datos de telemetría basados en sensores. Digital Twins requiere que cada parte de la telemetría recibida se asocie con un sensor dentro del grafo espacial para asegurarse de que los datos se procesen y se enruten en el contexto espacial adecuado.

## <a name="get-the-iot-hub-device-connection-string-from-the-management-api"></a>Obtención de la cadena de conexión del dispositivo de IoT Hub de la API de administración

Haga una llamada GET en la API de dispositivo con un parámetro `includes=ConnectionString` para obtener la cadena de conexión del dispositivo de IoT Hub. Puede filtrar por GUID de dispositivo o id. de hardware para encontrar el dispositivo específico:

```plaintext
https://yourManagementApiUrl/api/v1.0/devices/yourDeviceGuid?includes=ConnectionString
```

```plaintext
https://yourManagementApiUrl/api/v1.0/devices?hardwareIds=yourDeviceHardwareId&includes=ConnectionString
```

| Nombre del atributo personalizado | Reemplazar por |
| --- | --- |
| *suURLDeAPIDeAdministración* | La ruta de dirección URL completa para la API de administración |
| *yourDeviceGuid* | El identificador del dispositivo |
| *yourDeviceHardwareId* | El id. del hardware del dispositivo |

En la carga de respuesta, copie la propiedad *connectionString* del dispositivo, que se usará al llamar al SDK de dispositivo IoT de Azure para enviar datos a Azure Digital Twins.

## <a name="device-to-cloud-message"></a>Mensaje del dispositivo a la nube

Puede personalizar la carga y el formato del mensaje del dispositivo para ajustarse a las necesidades de su solución. Puede usar cualquier contrato de datos que se pueda serializar en una matriz de bytes o secuencia que sea compatible con la [clase de mensaje de cliente de dispositivo IoT de Azure, Message(byte[] byteArray)](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.message.-ctor?view=azure-dotnet#Microsoft_Azure_Devices_Client_Message__ctor_System_Byte___). El mensaje puede estar en un formato binario personalizado de su elección, siempre que descodifique el contrato de datos en una función definida por el usuario correspondiente. El único requisito para un mensaje del dispositivo a la nube es mantener un conjunto de propiedades (ver a continuación) para asegurar que el mensaje se enrute correctamente al motor de procesamiento.

### <a name="telemetry-properties"></a>Propiedades de telemetría

Si bien el contenido de la carga de un **mensaje** pueden ser datos arbitrarios con un tamaño de hasta 256 KB, hay algunos requisitos con respecto a la propiedad [`Message.Properties`](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.message.properties?view=azure-dotnet). Los pasos descritos a continuación reflejan las propiedades obligatorias y opcionales admitidas por el sistema:

| Nombre de propiedad | Valor | Obligatorio | DESCRIPCIÓN |
|---|---|---|---|
| *DigitalTwins-Telemetry* | 1.0 | SÍ | Valor constante que identifica un mensaje en el sistema |
| *DigitalTwins-SensorHardwareId* | `string(72)` | SÍ | Identificador único del sensor que envía el **mensaje**. Este valor debe coincidir con la propiedad **HardwareId** de un objeto para que el sistema lo procese. Por ejemplo: `00FF0643BE88-CO2` |
| *CreationTimeUtc* | `string` | Sin  | Cadena de fecha con formato según [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) que identifica el tiempo de muestreo de la carga. Por ejemplo: `2018-09-20T07:35:00.8587882-07:00` |
| *CorrelationId* | `string` | Sin  | UUID que puede usarse para el seguimiento de eventos en todo el sistema. Por ejemplo: `cec16751-ab27-405d-8fe6-c68e1412ce1f`

### <a name="sending-your-message-to-digital-twins"></a>Envío de mensajes a Digital Twins

Use la llamada [SendEventAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.deviceclient.sendeventasync?view=azure-dotnet) o [SendEventBatchAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.deviceclient.sendeventbatchasync?view=azure-dotnet) de DeviceClient para enviar el mensaje al servicio de Digital Twins.

## <a name="next-steps"></a>Pasos siguientes

Para información sobre el procesamiento de datos y funcionalidades de las funciones definidas por el usuario de Azure Digital Twins, lea [Procesamiento de datos y funciones definidas por el usuario con Azure Digital Twins](concepts-user-defined-functions.md).

