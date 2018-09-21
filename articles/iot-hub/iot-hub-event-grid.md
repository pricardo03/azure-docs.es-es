---
title: Azure IoT Hub y Event Grid | Microsoft Docs
description: Use Azure Event Grid para desencadenar procesos basados en acciones que se producen en IoT Hub.
author: kgremban
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 02/14/2018
ms.author: kgremban
ms.openlocfilehash: 3c12e98137f44ac094adaae282b5d56d30061e60
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2018
ms.locfileid: "44719858"
---
# <a name="react-to-iot-hub-events-by-using-event-grid-to-trigger-actions"></a>Reacción a eventos de IoT Hub usando Event Grid para desencadenar acciones

Azure IoT Hub se integra con Azure Event Grid para poder enviar notificaciones de eventos a otros servicios y desencadenar procesos de descarga. Configure las aplicaciones empresariales para escuchar eventos de IoT Hub, a fin de poder crear reacciones a eventos críticos de manera confiable, escalable y segura. Por ejemplo, compile una aplicación que realice varias acciones, como actualizar una base de datos, crear un vale y entregar una notificación por correo electrónico cada vez que se registre un nuevo dispositivo IoT en IoT Hub. 

[Azure Event Grid][lnk-eg-overview] es un servicio de enrutamiento de eventos totalmente administrado que utiliza un modelo de publicación-suscripción. Event Grid tiene compatibilidad integrada para servicios de Azure, como [Azure Functions](../azure-functions/functions-overview.md) y [Azure Logic Apps](../logic-apps/logic-apps-what-are-logic-apps.md), y puede proporcionar alertas de eventos a los servicios que no son de Azure mediante webhooks. Para obtener una lista completa de los controladores de eventos que Event Grid admite, vea [una introducción a Azure Event Grid][lnk-eg-overview]. 

![Arquitectura de Azure Event Grid](./media/iot-hub-event-grid/event-grid-functional-model.png)

## <a name="regional-availability"></a>Disponibilidad regional

La integración de Event Grid está disponible en centros de IoT ubicados en las regiones donde se admite Event Grid. Para conocer la lista más reciente de regiones disponibles [Introducción a Azure Event Grid][lnk-eg-overview]. 

## <a name="event-types"></a>Tipos de eventos

IoT Hub publica los siguientes tipos de eventos: 

| Tipo de evento | DESCRIPCIÓN |
| ---------- | ----------- |
| Microsoft.Devices.DeviceCreated | Se publica cuando se registra un dispositivo en una instancia de IoT Hub. |
| Microsoft.Devices.DeviceDeleted | Se publica cuando se elimina un dispositivo de una instancia de IoT Hub. |
| Microsoft.Devices.DeviceConnected | Se publica cuando se conecta un dispositivo a una instancia de IoT Hub. |
| Microsoft.Devices.DeviceDisconnected | Se publica cuando se desconecta un dispositivo de una instancia de IoT Hub. |

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

Para obtener una descripción detallada de cada propiedad, vea [Azure Event Grid event schema for IoT Hub](../event-grid/event-schema-iot-hub.md) (Esquema de eventos de Azure Event Grid para IoT Hub).

## <a name="filter-events"></a>Filtrado de eventos

Las suscripciones de eventos de IoT Hub pueden filtrar eventos según los criterios de tipo de evento y nombre de dispositivo. Los filtros de asunto en Event Grid funcionan según las coincidencias de **Empieza con** (prefijo) y de **Termina con** (sufijo). El filtro utiliza un operador `AND`, por lo que se entregan al suscriptor los eventos con un asunto que coincide con el prefijo y el sufijo. 

El asunto de los eventos de IoT Hub utiliza el formato:

```json
devices/{deviceId}
```
## <a name="limitations-for-device-connected-and-device-disconnected-events"></a>Limitaciones de los eventos de dispositivo conectado y dispositivo desconectado

Para recibir los eventos de dispositivo conectado y dispositivo desconectado, debe abrir el vínculo D2C o C2D del dispositivo. Si el dispositivo usa el protocolo MQTT, IoT Hub mantendrá el vínculo C2D abierto. Para AMQP puede abrir el vínculo C2D mediante una llamada a [Receive Async API](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.deviceclient.receiveasync?view=azure-dotnet). Si está enviando telemetría, el vínculo D2C está abierto. Si la conexión del dispositivo es intermitente, es decir, el dispositivo se conecta y desconecta con frecuencia, no se le enviarán todos y cada uno de los estados de conexión sino que se publicará el estado de conexión del que se realice la instantánea cada minuto. En el caso de una interrupción de IoT Hub, publicaremos el estado de conexión del dispositivo tan pronto como termine la interrupción. Si el dispositivo se desconecta durante esa interrupción, el evento de dispositivo desconectado se publicará en 10 minutos.

## <a name="tips-for-consuming-events"></a>Sugerencias para consumir eventos

Las aplicaciones que controlan los eventos de IoT Hub deben seguir estas prácticas recomendadas:

* Se pueden configurar varias suscripciones para enrutar los eventos al mismo controlador de eventos, por lo que es importante no asumir que los eventos proceden de un origen particular. Compruebe siempre el tema del mensaje para asegurarse de que se trata de la instancia de IoT Hub que está esperando. 
* No suponga que todos los eventos que recibe son los tipos que espera. Compruebe siempre eventType antes de procesar el mensaje.
* Los mensajes pueden llegar desordenados o después de un retraso. Utilice el campo de la etiqueta de entidad para saber si la información sobre los objetos está actualizada.

## <a name="next-steps"></a>Pasos siguientes

* [Pruebe el tutorial de eventos de IoT Hub](../event-grid/publish-iot-hub-events-to-logic-apps.md)
* [Aprenda a ordenar eventos conectados y desconectados de dispositivos](../iot-hub/iot-hub-how-to-order-connection-state-events.md)
* [Más información sobre Event Grid][lnk-eg-overview]
* [Compare las diferencias entre el enrutamiento de eventos y mensajes de IoT Hub][lnk-eg-compare]

<!-- Links -->
[lnk-eg-overview]: ../event-grid/overview.md
[lnk-eg-compare]: iot-hub-event-grid-routing-comparison.md