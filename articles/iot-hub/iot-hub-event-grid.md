---
title: Azure IoT Hub y Event Grid | Microsoft Docs
description: Use Azure Event Grid para desencadenar procesos basados en acciones que se producen en IoT Hub.
author: kgremban
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 02/20/2019
ms.author: kgremban
ms.openlocfilehash: 73a9aebfd0c5338f63927860ce3f6c57b20428a4
ms.sourcegitcommit: 45e4466eac6cfd6a30da9facd8fe6afba64f6f50
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2019
ms.locfileid: "66754772"
---
# <a name="react-to-iot-hub-events-by-using-event-grid-to-trigger-actions"></a>Reacción a eventos de IoT Hub usando Event Grid para desencadenar acciones

Azure IoT Hub se integra con Azure Event Grid para poder enviar notificaciones de eventos a otros servicios y desencadenar procesos de descarga. Configure las aplicaciones empresariales para escuchar eventos de IoT Hub, a fin de poder crear reacciones a eventos críticos de manera confiable, escalable y segura. Por ejemplo, crear una aplicación que actualiza una base de datos, crea un vale de trabajo y ofrece una notificación por correo electrónico cada vez que se registra un nuevo dispositivo de IoT a IoT hub.

[Azure Event Grid](../event-grid/overview.md) es un servicio de enrutamiento de eventos totalmente administrado que utiliza un modelo de publicación-suscripción. Event Grid tiene compatibilidad integrada para servicios de Azure, como [Azure Functions](../azure-functions/functions-overview.md) y [Azure Logic Apps](../logic-apps/logic-apps-what-are-logic-apps.md), y puede proporcionar alertas de eventos a los servicios que no son de Azure mediante webhooks. Para obtener una lista completa de los controladores de eventos que Event Grid admite, vea [una introducción a Azure Event Grid](../event-grid/overview.md).

![Arquitectura de Azure Event Grid](./media/iot-hub-event-grid/event-grid-functional-model.png)

## <a name="regional-availability"></a>Disponibilidad regional

La integración de Event Grid está disponible en centros de IoT ubicados en las regiones donde se admite Event Grid. Todos los eventos de dispositivo, excepto los eventos de telemetría de dispositivo están disponibles con carácter general. Evento de telemetría de dispositivo se encuentra en versión preliminar pública y está disponible en todas las regiones excepto este de Estados Unidos, oeste de Estados Unidos, Europa occidental, [Azure Government](/azure/azure-government/documentation-government-welcome), [Azure China 21Vianet](/azure/china), y [Azure Germany](https://azure.microsoft.com/global-infrastructure/germany/). Para conocer la lista más reciente de regiones disponibles, vea [Introducción a Azure Event Grid](../event-grid/overview.md).

## <a name="event-types"></a>Tipos de eventos

IoT Hub publica los siguientes tipos de eventos:

| Tipo de evento | DESCRIPCIÓN |
| ---------- | ----------- |
| Microsoft.Devices.DeviceCreated | Se publica cuando se registra un dispositivo en una instancia de IoT Hub. |
| Microsoft.Devices.DeviceDeleted | Se publica cuando se elimina un dispositivo de una instancia de IoT Hub. |
| Microsoft.Devices.DeviceConnected | Se publica cuando se conecta un dispositivo a una instancia de IoT Hub. |
| Microsoft.Devices.DeviceDisconnected | Se publica cuando se desconecta un dispositivo de una instancia de IoT Hub. |
| Microsoft.Devices.DeviceTelemetry | Se publica cuando se envía un mensaje de telemetría de dispositivo a IoT hub |

Use Azure Portal o la CLI de Azure para configurar qué eventos publicar desde cada instancia de IoT Hub. Por ejemplo, pruebe con el tutorial [Envío de notificaciones por correo electrónico sobre eventos de Azure IoT Hub mediante Logic Apps](../event-grid/publish-iot-hub-events-to-logic-apps.md).

## <a name="event-schema"></a>Esquema del evento

Los eventos de IoT Hub contienen toda la información necesaria para responder a cualquier cambio que se produzca en el ciclo de vida del dispositivo. Puede identificar un evento de IoT Hub comprobando que la propiedad eventType empieza por **Microsoft.Devices**. Para más información sobre cómo usar las propiedades de eventos de Event Grid, vea el [Esquema de eventos de Azure Event Grid](../event-grid/event-schema.md).

### <a name="device-connected-schema"></a>Esquema de dispositivo conectado

En el ejemplo siguiente, se muestra el esquema de un evento de dispositivo conectado:

```json
[{  
  "id": "f6bbf8f4-d365-520d-a878-17bf7238abd8",
  "topic": "/SUBSCRIPTIONS/<subscription ID>/RESOURCEGROUPS/<resource group name>/PROVIDERS/MICROSOFT.DEVICES/IOTHUBS/<hub name>",
  "subject": "devices/LogicAppTestDevice",
  "eventType": "Microsoft.Devices.DeviceConnected",
  "eventTime": "2018-06-02T19:17:44.4383997Z",
  "data": {
      "deviceConnectionStateEventInfo": {
        "sequenceNumber":
          "000000000000000001D4132452F67CE200000002000000000000000000000001"
      },
    "hubName": "egtesthub1",
    "deviceId": "LogicAppTestDevice",
    "moduleId" : "DeviceModuleID",
  }, 
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```

### <a name="device-telemetry-schema"></a>Esquema de telemetría del dispositivo

Mensaje de telemetría de dispositivo debe estar en un formato JSON válido con el contentType, establecido en JSON y contentEncoding establecida en UTF-8 en el mensaje [las propiedades del sistema](iot-hub-devguide-routing-query-syntax.md#system-properties). Si no se establece, IoT Hub escribirá los mensajes en formato codificado de base 64.

Pueden enriquecer los eventos de telemetría de dispositivo antes de que se publican en Event Grid, seleccione el punto de conexión como Event Grid. Para obtener más información, consulte [Enriquecimientos de mensaje de información general sobre](iot-hub-message-enrichments-overview.md).

El ejemplo siguiente muestra el esquema de un evento de telemetría de dispositivo:

```json
[{  
  "id": "9af86784-8d40-fe2g-8b2a-bab65e106785",
  "topic": "/SUBSCRIPTIONS/<subscription ID>/RESOURCEGROUPS/<resource group name>/PROVIDERS/MICROSOFT.DEVICES/IOTHUBS/<hub name>",
  "subject": "devices/LogicAppTestDevice",
  "eventType": "Microsoft.Devices.DeviceTelemetry",
  "eventTime": "2019-01-07T20:58:30.48Z",
  "data": {
      "body": {
          "Weather": {
              "Temperature": 900
            },
            "Location": "USA"
        },
        "properties": {
            "Status": "Active"
        },
        "systemProperties": {
          "iothub-content-type": "application/json",
          "iothub-content-encoding": "utf-8",
          "iothub-connection-device-id": "d1",
          "iothub-connection-auth-method": "{\"scope\":\"device\",\"type\":\"sas\",\"issuer\":\"iothub\",\"acceptingIpFilterRule\":null}",
          "iothub-connection-auth-generation-id": "123455432199234570",
          "iothub-enqueuedtime": "2019-01-07T20:58:30.48Z",
          "iothub-message-source": "Telemetry"
        }
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```

### <a name="device-created-schema"></a>Esquema de dispositivo creado

En el ejemplo siguiente, se muestra el esquema de un evento de dispositivo creado:

```json
[{
  "id": "56afc886-767b-d359-d59e-0da7877166b2",
  "topic": "/SUBSCRIPTIONS/<subscription ID>/RESOURCEGROUPS/<resource group name>/PROVIDERS/MICROSOFT.DEVICES/IOTHUBS/<hub name>",
  "subject": "devices/LogicAppTestDevice",
  "eventType": "Microsoft.Devices.DeviceCreated",
  "eventTime": "2018-01-02T19:17:44.4383997Z",
  "data": {
    "twin": {
      "deviceId": "LogicAppTestDevice",
      "etag": "AAAAAAAAAAE=",
      "deviceEtag":"null",
      "status": "enabled",
      "statusUpdateTime": "0001-01-01T00:00:00",
      "connectionState": "Disconnected",
      "lastActivityTime": "0001-01-01T00:00:00",
      "cloudToDeviceMessageCount": 0,
      "authenticationType": "sas",
      "x509Thumbprint": {
        "primaryThumbprint": null,
        "secondaryThumbprint": null
      },
      "version": 2,
      "properties": {
        "desired": {
          "$metadata": {
            "$lastUpdated": "2018-01-02T19:17:44.4383997Z"
          },
          "$version": 1
        },
        "reported": {
          "$metadata": {
            "$lastUpdated": "2018-01-02T19:17:44.4383997Z"
          },
          "$version": 1
        }
      }
    },
    "hubName": "egtesthub1",
    "deviceId": "LogicAppTestDevice"
  },
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```

Para obtener una descripción detallada de cada propiedad, vea [esquema de eventos de Azure Event Grid para IoT Hub](../event-grid/event-schema-iot-hub.md).

## <a name="filter-events"></a>Filtrado de eventos

Suscripciones a eventos de IoT Hub pueden filtrar eventos basándose en el tipo de evento, contenido de los datos y el asunto, que es el nombre del dispositivo.

Event Grid permite [filtrado](../event-grid/event-filtering.md) en contenido de tipos, temas y los datos del evento. Al crear la suscripción de Event Grid, puede elegir para suscribirse a eventos de IoT seleccionados. Los filtros de asunto en Event Grid funcionan según las coincidencias de **Empieza con** (prefijo) y de **Termina con** (sufijo). El filtro utiliza un operador `AND`, por lo que se entregan al suscriptor los eventos con un asunto que coincide con el prefijo y el sufijo.

El asunto de los eventos de IoT Hub utiliza el formato:

```json
devices/{deviceId}
```

Event Grid permite también para filtrar los atributos de cada evento, incluido el contenido de datos. Esto le permite elegir qué eventos se entregan contenido en función del mensaje de telemetría. Consulte [filtrado avanzado](../event-grid/event-filtering.md#advanced-filtering) para ver ejemplos.

Para los eventos que no son de telemetría como DeviceConnected, DeviceDisconnected, DeviceCreated y DeviceDeleted, el filtrado de Event Grid puede usarse al crear la suscripción. Para los eventos de telemetría, además del filtrado de Event Grid, también pueden filtrar los usuarios en los dispositivos gemelos, las propiedades de mensaje y cuerpo a través de la consulta de enrutamiento de mensajes. Creamos un valor predeterminado [ruta](iot-hub-devguide-messages-d2c.md) en IoT Hub, en función de su suscripción de Event Grid para telemetría de dispositivo. Esta ruta solo puede controlar todas las suscripciones de Event Grid. Para filtrar los mensajes antes de envían los datos de telemetría, puede actualizar su [consulta enrutamiento](iot-hub-devguide-routing-query-syntax.md). Tenga en cuenta que esa consulta enrutamiento puede aplicarse al cuerpo del mensaje solo si el cuerpo es JSON.

## <a name="limitations-for-device-connected-and-device-disconnected-events"></a>Limitaciones de los eventos de dispositivo conectado y dispositivo desconectado

Para recibir los eventos de dispositivo conectado y dispositivo desconectado, debe abrir el vínculo D2C o C2D del dispositivo. Si el dispositivo usa el protocolo MQTT, IoT Hub mantendrá el vínculo C2D abierto. De AMQP, puede abrir el vínculo C2D mediante una llamada a la [recibir Async API](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.deviceclient.receiveasync?view=azure-dotnet).

Si está enviando telemetría, el vínculo D2C está abierto. Si la conexión del dispositivo parpadea, lo que significa que el dispositivo se conecta y desconecta con frecuencia, no se le enviará cada estado de conexión único, pero va a publicar el estado de conexión de los cuales se toma una instantánea cada minuto. En el caso de una interrupción de IoT Hub, publicaremos el estado de conexión del dispositivo tan pronto como termine la interrupción. Si el dispositivo se desconecta durante esa interrupción, el evento de dispositivo desconectado se publicará en 10 minutos.

## <a name="tips-for-consuming-events"></a>Sugerencias para consumir eventos

Las aplicaciones que controlan los eventos de IoT Hub deben seguir estas prácticas recomendadas:

* Se pueden configurar varias suscripciones para enrutar eventos al mismo controlador de eventos, por lo que no dé por sentado que los eventos proceden de un origen determinado. Compruebe siempre el tema del mensaje para asegurarse de que se trata de la instancia de IoT Hub que está esperando.

* No suponga que todos los eventos que recibe son los tipos que espera. Compruebe siempre eventType antes de procesar el mensaje.

* Los mensajes pueden llegar desordenados o después de un retraso. Utilice el campo de etag para saber si su información acerca de los objetos está actualizada para el dispositivo que creó o eventos de dispositivo eliminado.

## <a name="next-steps"></a>Pasos siguientes

* [Pruebe el tutorial de eventos de IoT Hub](../event-grid/publish-iot-hub-events-to-logic-apps.md)

* [Aprenda a ordenar eventos conectados y desconectados de dispositivos](iot-hub-how-to-order-connection-state-events.md)

* [Más información sobre Event Grid](../event-grid/overview.md)

* [Compare las diferencias entre el enrutamiento de eventos y mensajes de IoT Hub](iot-hub-event-grid-routing-comparison.md)
