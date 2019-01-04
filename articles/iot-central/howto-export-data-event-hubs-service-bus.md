---
title: Exportación de datos a Azure Event Hubs y Azure Service Bus | Microsoft Docs
description: Cómo exportar datos desde la aplicación de Azure IoT Central a Azure Event Hubs y Azure Service Bus
services: iot-central
author: viv-liu
ms.author: viviali
ms.date: 12/07/2018
ms.topic: conceptual
ms.service: iot-central
manager: peterpr
ms.openlocfilehash: 14b51f109ca76661ac10c99d42002dda45bc0500
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2018
ms.locfileid: "53318432"
---
# <a name="export-your-data-in-azure-iot-central"></a>Exportación de los datos a Azure IoT Central

*Este tema se aplica a administradores*.

En este artículo se analiza cómo usar la característica de exportación continua de datos de Azure IoT Central para exportar los datos a sus propias instancias de **Azure Event Hubs** y **Azure Service Bus**. Puede exportar **medidas**, **dispositivos** y **plantillas de dispositivo** a su propio destino para analizar las rutas de acceso activas. Esto incluye el desencadenamiento de reglas personalizadas en Azure Stream Analytics, el desencadenamiento de flujos de trabajo personalizados en Azure Logic Apps o la transformación de datos y su paso a través de Azure Functions. 

> [!Note]
> Una vez más, cuando activa la exportación de datos continua, solo obtiene los datos a partir de ese momento. Actualmente, los datos no se pueden recuperar durante un tiempo cuando la exportación de datos continua estaba desactivada. Para conservar más datos históricos, activa la exportación continua de datos al principio.


## <a name="prerequisites"></a>Requisitos previos

- Ser administrador en la aplicación de IoT Central

## <a name="export-to-azure-event-hubs-and-azure-service-bus"></a>Exportación a Azure Event Hubs y Azure Service Bus

Los datos de medidas, dispositivos y plantillas de dispositivo se exportan a su centro de eventos o cola o tema de Service Bus casi en tiempo real. Los datos de medidas exportados contienen la totalidad del mensaje que los dispositivos enviaron a IoT Central, no solo los valores de las propias medidas. Los datos de dispositivo exportados contienen los cambios en las propiedades y la configuración de todos los dispositivos; las plantillas de dispositivo exportadas contienen los cambios en todas las plantillas de dispositivo. Los datos exportados se encuentran en la propiedad "body" y están en formato JSON.

> [!NOTE]
> Al elegir una instancia de Service Bus como destino de exportación, las colas y los temas **no deben tener habilitada la opción de detección de sesiones o de duplicados**. Si cualquiera de estas opciones está habilitada, algunos mensajes no llegarán a la cola o tema.

### <a name="measurements"></a>Medidas

Un nuevo mensaje se exporta rápidamente después de que IoT Central recibe el mensaje de un dispositivo. Cada mensaje exportado en Event Hubs y Service Bus contiene el mensaje completo que el dispositivo ha enviado en la propiedad "body" en formato JSON. 

> [!NOTE]
> Los dispositivos que envían las mediciones se representan mediante identificadores de dispositivo (consulte las secciones siguientes). Para obtener los nombres de los dispositivos, exporte los datos del dispositivo y ponga en correlación cada mensaje mediante el identificador **connectionDeviceId** que coincide con el identificador **deviceId** del mensaje del dispositivo.

El ejemplo siguiente muestra un mensaje acerca de los datos de las medidas que se ha recibido en Event Hubs o en la cola o tema de Service Bus.

```json
{
  "body": {
    "humidity": 29.06963648666288,
    "temp": 8.4503795661685,
    "pressure": 1075.8334910110093,
    "magnetometerX": 408.6966458887116,
    "magnetometerY": -532.8809796603962,
    "magnetometerZ": 174.70653875528205,
    "accelerometerX": 1481.546749013788,
    "accelerometerY": -1139.4316656437406,
    "accelerometerZ": 811.6928695575307,
    "gyroscopeX": 442.19879163299856,
    "gyroscopeY": 123.23710975717177,
    "gyroscopeZ": 708.5397575786151,
    "deviceState": "DANGER"
  },
  "annotations": {
    "iothub-connection-device-id": "<connectionDeviceId>",
    "iothub-connection-auth-method": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\",\"acceptingIpFilterRule\":null}",
    "iothub-connection-auth-generation-id": "<generationId>",
    "iothub-enqueuedtime": 1539381029965,
    "iothub-message-source": "Telemetry",
    "x-opt-sequence-number": 25325,
    "x-opt-offset": "<offset>",
    "x-opt-enqueued-time": 1539381030200
  },
  "sequenceNumber": 25325,
  "enqueuedTimeUtc": "2018-10-12T21:50:30.200Z",
  "offset": "<offset>",
  "properties": {
    "content_type": "application/json",
    "content_encoding": "utf-8"
  }
}
```

### <a name="devices"></a>Dispositivos

Los mensajes que contienen los datos del dispositivo se envían a Event Hubs o a la cola o tema de Service Bus cada pocos minutos. Esto significa que cada pocos minutos, un lote de mensajes llegará con datos acerca de
- Nuevos dispositivos que se han agregado
- Dispositivos con propiedades y valores de configuración modificados

Cada mensaje representa uno o más cambios que se han producido en un dispositivo desde la exportación del último mensaje. La información que se enviará en cada mensaje incluye:
- `id` del dispositivo en IoT Central
- `name` del dispositivo
- `deviceId` del [Device Provisioning Service](https://aka.ms/iotcentraldocsdps)
- Información de la plantilla de dispositivo
- Valores de propiedad
- Valores de configuración

> [!NOTE]
> Los dispositivos que se eliminaron desde el último procesamiento por lotes no se exportan. Actualmente, no hay ningún indicador en los mensajes exportados para los dispositivos eliminados.
>
> La plantilla de dispositivo a la que pertenece cada dispositivo se representa mediante un identificador de plantilla de dispositivo. Para obtener el nombre de la plantilla de dispositivo, asegúrese de exportar también los datos de esta.

El ejemplo siguiente muestra un mensaje acerca de los datos de dispositivo en Event Hubs o en la cola o tema de Service Bus:


```json
{
  "body": {
    "id": "<id>",
    "name": "<deviceName>",
    "simulated": true,
    "deviceId": "<deviceId>",
    "deviceTemplate": {
      "id": "<templateId>",
      "version": "1.0.0"
    },
    "properties": {
      "cloud": {
        "location": "Seattle"
      },
      "device": {
        "dieNumber": 5445.5862873026645
      }
    },
    "settings": {
      "device": {
        "fanSpeed": 0
      }
    }
  },
  "annotations": {
    "iotcentral-message-source": "devices",
    "x-opt-partition-key": "<partitionKey>",
    "x-opt-sequence-number": 39740,
    "x-opt-offset": "<offset>",
    "x-opt-enqueued-time": 1539274959654
  },
  "partitionKey": "<partitionKey>",
  "sequenceNumber": 39740,
  "enqueuedTimeUtc": "2018-10-11T16:22:39.654Z",
  "offset": "<offset>",
}
```

### <a name="device-templates"></a>Plantillas de dispositivo

Los mensajes que contienen los datos de las plantillas del dispositivo se envían a Event Hubs o a la cola o tema de Service Bus cada pocos minutos. Esto significa que cada pocos minutos, un lote de mensajes llegará con datos acerca de
- Nuevas plantillas de dispositivo que se han agregado
- Plantillas de dispositivo con mediciones, propiedades y definiciones de configuración que cambiaron

Cada mensaje representa uno o más cambios que se han producido en una plantilla de dispositivo desde la exportación del último mensaje. La información que se enviará en cada mensaje incluye:
- `id` de la plantilla de dispositivo
- `name` de la plantilla de dispositivo
- `version` de la plantilla de dispositivo
- Tipos de datos de medición y valores mínimos y máximos
- Tipos de datos de propiedad y valores predeterminados
- Tipos de datos de configuración y valores predeterminados

> [!NOTE]
> Las plantillas de dispositivo que se eliminaron desde el último procesamiento por lotes no se exportan. Actualmente, no hay ningún indicador en los mensajes exportados para las plantillas de dispositivo eliminadas.

El ejemplo siguiente muestra un mensaje acerca de los datos de las plantillas de dispositivo en Event Hubs o en la cola o tema de Service Bus:

```json
{
  "body": {
    "id": "<id>",
    "version": "1.0.0",
    "name": "<templateName>",
    "measurements": {
      "telemetry": {
        "humidity": {
          "dataType": "double",
          "name": "humidity"
        },
        "pressure": {
          "dataType": "double",
          "name": "pressure"
        },
        "temp": {
          "dataType": "double",
          "name": "temperature"
        }
      }
    },
    "properties": {
      "cloud": {
        "location": {
          "dataType": "string",
          "name": "Location"
        }
      },
      "device": {
        "dieNumber": {
          "dataType": "double",
          "name": "Die Number"
        }
      }
    },
    "settings": {
      "device": {
        "fanSpeed": {
          "dataType": "double",
          "name": "Fan Speed",
          "initialValue": 0
        }
      }
    }
  },
  "annotations": {
    "iotcentral-message-source": "deviceTemplates",
    "x-opt-partition-key": "<partitionKey>",
    "x-opt-sequence-number": 25315,
    "x-opt-offset": "<offset>",
    "x-opt-enqueued-time": 1539274985085
  },
  "partitionKey": "<partitionKey>",
  "sequenceNumber": 25315,
  "enqueuedTimeUtc": "2018-10-11T16:23:05.085Z",
  "offset": "<offset>",
}
```

## <a name="next-steps"></a>Pasos siguientes

Ahora que sabe cómo exportar los datos a Azure Event Hubs y Azure Service Bus, vaya al paso siguiente:

> [!div class="nextstepaction"]
> [Desencadenar Azure Functions](howto-trigger-azure-functions.md)
