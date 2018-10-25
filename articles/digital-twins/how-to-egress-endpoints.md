---
title: Salidas y puntos de conexión en Azure Digital Twins | Microsoft Docs
description: Instrucciones sobre cómo crear puntos de conexión con Azure Digital Twins
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/08/2018
ms.author: alinast
ms.openlocfilehash: c917fab84448684cf29af162ec0781d764605f71
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2018
ms.locfileid: "49323970"
---
# <a name="egress-and-endpoints"></a>Salidas y puntos de conexión

Azure Digital Twins admite el concepto de _puntos de conexión_ donde cada punto de conexión representa un agente de mensaje o evento en la suscripción de Azure del usuario. Los eventos y mensajes pueden enviarse al **centro de eventos**, a **Event Grid** y a los **temas de Service Bus**.

Los eventos se enviarán a los extremos de acuerdo con las preferencias de enrutamiento definidas previamente: el usuario puede especificar qué punto de conexión debe recibir cualquiera de los siguientes eventos:`TopologyOperation`, `UdfCustom`, `SensorChange`, `SpaceChange` o `DeviceMessage`.

Para obtener un conocimiento básico del enrutamiento de eventos y los tipos de eventos, consulte [Routing events and messages](concepts-events-routing.md) (Enrutamiento de eventos y mensajes).

## <a name="event-types-description"></a>Descripción de los tipos de evento

Estos son los formatos de evento para cada uno de los tipos de evento:

- `TopologyOperation`

  Se aplica a los cambios de los gráficos. La propiedad `subject` especifica el tipo de objeto afectado. Los tipos de objetos que podrían desencadenar este evento son: `Device, DeviceBlobMetadata`, `DeviceExtendedProperty`, `ExtendedPropertyKey`, `ExtendedType`, `KeyStore`, `Report`, `RoleDefinition`, `Sensor`, `SensorBlobMetadata`, `SensorExtendedProperty`, `Space`, `SpaceBlobMetadata`, `SpaceExtendedProperty`, `SpaceResource`, `SpaceRoleAssignment`, `System`, `User`, `UserBlobMetadata` y `UserExtendedProperty`.

  Ejemplo:

  ```JSON
  {
    "id": "00000000-0000-0000-0000-000000000000",
    "subject": "ExtendedPropertyKey",
    "data": {
      "SpacesToNotify": [
        "3a16d146-ca39-49ee-b803-17a18a12ba36"
      ],
      "Id": "00000000-0000-0000-0000-000000000000",
      "Type": "ExtendedPropertyKey",
      "AccessType": "Create"
    },
    "eventType": "TopologyOperation",
    "eventTime": "2018-04-17T17:41:54.9400177Z",
    "dataVersion": "1",
    "metadataVersion": "1",
    "topic": "/subscriptions/yourTopicName"
  }
  ```

    | Nombre del atributo personalizado | Reemplace por |
    | --- | --- |
    | `yourTopicName` | El nombre del tema personalizado |

- `UdfCustom`

  Un evento enviado por una función definida por el usuario (UDF). Tenga en cuenta que este evento tiene que enviarse explícitamente desde la misma UDF.

  Ejemplo:

  ```JSON
  {
    "id": "568fd394-380b-46fa-925a-ebb96f658cce",
    "subject": "UdfCustom",
    "data": {
      "TopologyObjectId": "7c799bfc-1bff-4b9e-b15a-669933969d20",
      "ResourceType": "Space",
      "Payload": "\"Room is not available or air quality is poor\"",
      "CorrelationId": "568fd394-380b-46fa-925a-ebb96f658cce"
    },
    "eventType": "UdfCustom",
    "eventTime": "2018-10-02T06:50:15.198Z",
    "dataVersion": "1.0",
    "metadataVersion": "1",
    "topic": "/subscriptions/yourTopicName"
  }
  ```

    | Nombre del atributo personalizado | Reemplace por |
    | --- | --- |
    | `yourTopicName` | El nombre del tema personalizado |

- `SensorChange`

  Una actualización del estado de un sensor según los cambios en los datos de telemetría.

  Ejemplo:

  ```JSON
  {
    "id": "60bf5336-2929-45b4-bb4c-b45699dfe95f",
    "subject": "SensorChange",
    "data": {
      "Type": "Classic",
      "DataType": "Motion",
      "Id": "60bf5336-2929-45b4-bb4c-b45699dfe95f",
      "Value": "False",
      "PreviousValue": "True",
      "EventTimestamp": "2018-04-17T17:46:15.4964262Z",
      "MessageType": "sensor",
      "Properties": {
        "ms-client-request-id": "c9e576b7-5eea-4f61-8617-92a57add5179",
        "ms-activity-id": "ct22YwXEGJ5u.605.0"
      }
    },
    "eventType": "SensorChange",
    "eventTime": "2018-04-17T17:46:18.5452993Z",
    "dataVersion": "1",
    "metadataVersion": "1",
    "topic": "/subscriptions/yourTopicName"
  }
  ```

    | Nombre del atributo personalizado | Reemplace por |
    | --- | --- |
    | `yourTopicName` | El nombre del tema personalizado |

- `SpaceChange`

  Una actualización del estado de un espacio según los cambios en los datos de telemetría.

  Ejemplo:

  ```JSON
  {
    "id": "42522e10-b1aa-42ff-a5e7-7181788ffc4b",
    "subject": "SpaceChange",
    "data": {
      "Type": null,
      "DataType": "AvailableAndFresh",
      "Id": "7c799bfc-1bff-4b9e-b15a-669933969d20",
      "Value": "Room is not available or air quality is poor",
      "PreviousValue": null,
      "RawData": null,
      "transactionId": null,
      "EventTimestamp": null,
      "MessageType": null,
      "Properties": null,
      "CorrelationId": "42522e10-b1aa-42ff-a5e7-7181788ffc4b"
    },
    "eventType": "SpaceChange",
    "eventTime": "2018-10-02T06:50:20.128Z",
    "dataVersion": "1.0",
    "metadataVersion": "1",
    "topic": "/subscriptions/yourTopicName"
  }
  ```

    | Nombre del atributo personalizado | Reemplace por |
    | --- | --- |
    | `yourTopicName` | El nombre del tema personalizado |

- `DeviceMessage`

  Le permite especificar una conexión de `EventHub` a la que también se pueden enrutar eventos de telemetría sin procesar desde Azure Digital Twins.

> [!NOTE]
> - `DeviceMessage` solo puede combinarse con `EventHub`; no podrá combinar `DeviceMessage` con ninguno de los otros tipos de eventos.
> - Podrá especificar un único punto de conexión de la combinación de tipo `EventHub`/`DeviceMessage`.

## <a name="configuring-endpoints"></a>Configuración de puntos de conexión

La administración de puntos de conexión se ejerce a través de la API de puntos de conexión. Estos son algunos ejemplos de cómo configurar los distintos puntos de conexión compatibles. Preste especial atención a la matriz de tipos de evento, ya que define el enrutamiento para el punto de conexión:

```plaintext
POST https://endpoints-demo.azuresmartspaces.net/management/api/v1.0/endpoints
```

- Enrute a **Service Bus** los tipos de evento: `SensorChange`, `SpaceChange` y `TopologyOperation`.

  ```JSON
  {
    "type": "ServiceBus",
    "eventTypes": [
      "SensorChange",
      "SpaceChange",
      "TopologyOperation"
    ],
    "connectionString": "Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourPrimaryKey",
    "secondaryConnectionString": "Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourSecondaryKey",
    "path": "yourTopicName"
  }
  ```

    | Nombre del atributo personalizado | Reemplace por |
    | --- | --- |
    | `yourNamespace` | El espacio de nombres del punto de conexión |
    | `yourPrimaryKey` | La cadena de conexión principal utilizada para autenticar |
    | `yourSecondaryKey` | La cadena de conexión principal utilizada para autenticar |
    | `yourTopicName` | El nombre del tema personalizado |

- Enrute a **Event Grid** los tipos de evento: `SensorChange`, `SpaceChange` y `TopologyOperation`

  ```JSON
  {
    "type": "EventGrid",
    "eventTypes": [
      "SensorChange",
      "SpaceChange",
      "TopologyOperation"
    ],
    "connectionString": "yourPrimaryKey",
    "secondaryConnectionString": "yourSecondaryKey",
    "path": "yourTopicName.westus-1.eventgrid.azure.net"
  }
  ```

    | Nombre del atributo personalizado | Reemplace por |
    | --- | --- |
    | `yourPrimaryKey` | La cadena de conexión principal utilizada para autenticar|
    | `yourSecondaryKey` | La cadena de conexión principal utilizada para autenticar |
    | `yourTopicName` | El nombre del tema personalizado |

- Enrute al **centro de eventos** los tipos de evento: `SensorChange`, `SpaceChange` y `TopologyOperation`

  ```JSON
  {
    "type": "EventHub",
    "eventTypes": [
      "SensorChange",
      "SpaceChange",
      "TopologyOperation"
    ],
    "connectionString": "Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourPrimaryKey",
    "secondaryConnectionString": "Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourSecondaryKey",
    "path": "yourEventHubName"
  }
  ```

    | Nombre del atributo personalizado | Reemplace por |
    | --- | --- |
    | `yourNamespace` | El espacio de nombres del punto de conexión |
    | `yourPrimaryKey` | La cadena de conexión principal utilizada para autenticar |
    | `yourSecondaryKey` | La cadena de conexión principal utilizada para autenticar |
    | `yourEventHubName` | El nombre del centro de eventos |

- Enrute al **centro de eventos** los tipos de evento `DeviceMessage`. Tenga en cuenta la inclusión de _EntityPath_ en el parámetro `connectionString`, que es obligatorio.

  ```JSON
  {
    "type": "EventHub",
    "eventTypes": [
      "DeviceMessage"
    ],
    "connectionString": "Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourPrimaryKey;EntityPath=yourEventHubName",
    "secondaryConnectionString": "Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourSecondaryKey;EntityPath=yourEventHubName",
    "path": "yourEventHubName"
  }
  ```

    | Nombre del atributo personalizado | Reemplace por |
    | --- | --- |
    | `yourNamespace` | El espacio de nombres del punto de conexión |
    | `yourPrimaryKey` | La cadena de conexión principal utilizada para autenticar |
    | `yourSecondaryKey` | La cadena de conexión principal utilizada para autenticar |
    | `yourEventHubName` | El nombre del centro de eventos |

> [!NOTE]
> Tras la creación de un nuevo punto de conexión, pueden transcurrir de 5 a 10 minutos para que comience a recibir eventos en el punto de conexión.

## <a name="primary-and-secondary-connection-keys"></a>Claves de conexión principal y secundaria

Cuando una clave de conexión principal deja de estar autorizada, el sistema intenta usar automáticamente la clave de conexión secundaria. De esta forma, se cuenta con una copia de seguridad y esto permite autenticarse correctamente y actualizar la clave principal a través de la API de puntos de conexión.

Si las claves de conexión principal y secundaria no están autorizadas, el sistema especifica un tiempo de espera de interrupción exponencial de hasta 30 minutos. Se cancelarán los eventos cada vez que se desencadene el tiempo de espera de interrupción.

Cada vez que el sistema está en estado de espera de interrupción, actualizar las claves de las conexiones a través de la API de puntos de conexión puede tardar hasta 30 minutos en surtir efecto.

## <a name="unreachable-endpoints"></a>Puntos de conexión inaccesibles

Cuando un punto de conexión se vuelve inaccesible, el sistema especifica un tiempo de espera de interrupción exponencial de hasta 30 minutos. Se cancelarán los eventos cada vez que se desencadene el tiempo de espera de interrupción.

## <a name="next-steps"></a>Pasos siguientes

Para obtener información sobre cómo usar Swagger de Digital Twins, consulte [Swagger de Azure Digital Twins](how-to-use-swagger.md).

Para obtener más información sobre el enrutamiento de eventos y mensajes en Azure Digital Twins, consulte [Routing events and messages](concepts-events-routing.md) (Enrutamiento de eventos y mensajes).
