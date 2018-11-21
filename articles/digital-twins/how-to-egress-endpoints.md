---
title: Salidas y puntos de conexión en Azure Digital Twins | Microsoft Docs
description: Instrucciones sobre cómo crear puntos de conexión con Azure Digital Twins
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/26/2018
ms.author: alinast
ms.openlocfilehash: c94d29f16c011a9ff9951d064d7496d3a87f70ef
ms.sourcegitcommit: 542964c196a08b83dd18efe2e0cbfb21a34558aa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2018
ms.locfileid: "51636312"
---
# <a name="egress-and-endpoints"></a>Salidas y puntos de conexión

Azure Digital Twins admite el concepto de **puntos de conexión**. Cada punto de conexión representa un mensaje o un agente de eventos en la suscripción a Azure del usuario. Los eventos y mensajes pueden enviarse a Azure Event Hubs, a Azure Event Grid y a los temas de Azure Service Bus.

Los eventos se envían a los puntos de conexión de acuerdo con las preferencias de enrutamiento predefinidas. El usuario puede especificar qué punto de conexión debe recibir cualquiera de los siguientes eventos: 

- TopologyOperation
- UdfCustom
- SensorChange
- SpaceChange
- DeviceMessage

Para obtener un conocimiento básico del enrutamiento de eventos y los tipos de eventos, consulte [Routing events and messages](concepts-events-routing.md) (Enrutamiento de eventos y mensajes).

## <a name="event-types-description"></a>Descripción de los tipos de evento

Los formatos de eventos para cada uno de los tipos de evento se describen en las secciones siguientes.

### <a name="topologyoperation"></a>TopologyOperation

**TopologyOperation** se aplica a los cambios del grafo. La propiedad **subject** especifica el tipo de objeto afectado. Los siguientes tipos de objetos podrían desencadenar este evento: 

- Dispositivo
- DeviceBlobMetadata
- DeviceExtendedProperty
- ExtendedPropertyKey
- ExtendedType
- KeyStore
- Informe
- RoleDefinition
- Sensor
- SensorBlobMetadata
- SensorExtendedProperty
- Espacio
- SpaceBlobMetadata
- SpaceExtendedProperty
- SpaceResource
- SpaceRoleAssignment
- Sistema
- Usuario
- UserBlobMetadata
- UserExtendedProperty

#### <a name="example"></a>Ejemplo

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
  "topic": "/subscriptions/YOUR_TOPIC_NAME"
}
```

| Valor | Reemplazar por |
| --- | --- |
| YOUR_TOPIC_NAME | El nombre del tema personalizado |

### <a name="udfcustom"></a>UdfCustom

**UdfCustom** es un evento enviado por una función definida por el usuario (UDF). 
  
> [!IMPORTANT]  
> Este evento tiene que enviarse explícitamente desde la misma función.

#### <a name="example"></a>Ejemplo

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
  "topic": "/subscriptions/YOUR_TOPIC_NAME"
}
```

| Valor | Reemplazar por |
| --- | --- |
| YOUR_TOPIC_NAME | El nombre del tema personalizado |

### <a name="sensorchange"></a>SensorChange

**SensorChange** es una actualización del estado de un sensor según los cambios en los datos de telemetría.

#### <a name="example"></a>Ejemplo

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
  "topic": "/subscriptions/YOUR_TOPIC_NAME"
}
```

| Valor | Reemplazar por |
| --- | --- |
| YOUR_TOPIC_NAME | El nombre del tema personalizado |

### <a name="spacechange"></a>SpaceChange

**SpaceChange** es una actualización del estado de un espacio según los cambios en los datos de telemetría.

#### <a name="example"></a>Ejemplo

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
  "topic": "/subscriptions/YOUR_TOPIC_NAME"
}
```

| Valor | Reemplazar por |
| --- | --- |
| YOUR_TOPIC_NAME | El nombre del tema personalizado |

### <a name="devicemessage"></a>DeviceMessage

Al usar **DeviceMessage**, puede especificar una conexión de **EventHub** a la que también se pueden enrutar eventos de telemetría sin procesar desde Azure Digital Twins.

> [!NOTE]
> - **DeviceMessage** se puede combinar solo con **EventHub**. No puede combinar **DeviceMessage** con cualquiera de los otros tipos de eventos.
> - Puede especificar un único punto de conexión de la combinación de tipo **EventHub** o **DeviceMessage**.

## <a name="configure-endpoints"></a>Configuración de extremos

La administración de puntos de conexión se ejerce a través de la API de puntos de conexión. Los ejemplos siguientes muestran cómo configurar los distintos puntos de conexión compatibles. Preste especial atención a la matriz de tipos de evento, ya que define el enrutamiento para el punto de conexión:

```plaintext
POST https://endpoints-demo.azuresmartspaces.net/management/api/v1.0/endpoints
```

- Enrute a los tipos de eventos de Service Bus **SensorChange**, **SpaceChange** y **TopologyOperation**:

  ```JSON
  {
    "type": "ServiceBus",
    "eventTypes": [
      "SensorChange",
      "SpaceChange",
      "TopologyOperation"
    ],
    "connectionString": "Endpoint=sb://YOUR_NAMESPACE.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=YOUR_PRIMARY_KEY",
    "secondaryConnectionString": "Endpoint=sb://YOUR_NAMESPACE.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=YOUR_SECONDARY_KEY",
    "path": "YOUR_TOPIC_NAME"
  }
  ```

    | Valor | Reemplazar por |
    | --- | --- |
    | YOUR_NAMESPACE | El espacio de nombres del punto de conexión |
    | YOUR_PRIMARY_KEY | La cadena de conexión principal utilizada para autenticar |
    | YOUR_SECONDARY_KEY | La cadena de conexión principal utilizada para autenticar |
    | YOUR_TOPIC_NAME | El nombre del tema personalizado |

- Enrute a los tipos de eventos de Event Grid **SensorChange**, **SpaceChange** y **TopologyOperation**:

  ```JSON
  {
    "type": "EventGrid",
    "eventTypes": [
      "SensorChange",
      "SpaceChange",
      "TopologyOperation"
    ],
    "connectionString": "YOUR_PRIMARY_KEY",
    "secondaryConnectionString": "YOUR_SECONDARY_KEY",
    "path": "YOUR_TOPIC_NAME.westus-1.eventgrid.azure.net"
  }
  ```

    | Valor | Reemplazar por |
    | --- | --- |
    | YOUR_PRIMARY_KEY | La cadena de conexión principal utilizada para autenticar|
    | YOUR_SECONDARY_KEY | La cadena de conexión principal utilizada para autenticar |
    | YOUR_TOPIC_NAME | El nombre del tema personalizado |

- Enrute a los tipos de eventos de Event Hubs **SensorChange**, **SpaceChange** y **TopologyOperation**:

  ```JSON
  {
    "type": "EventHub",
    "eventTypes": [
      "SensorChange",
      "SpaceChange",
      "TopologyOperation"
    ],
    "connectionString": "Endpoint=sb://YOUR_NAMESPACE.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=YOUR_PRIMARY_KEY",
    "secondaryConnectionString": "Endpoint=sb://YOUR_NAMESPACE.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=YOUR_SECONDARY_KEY",
    "path": "YOUR_EVENT_HUB_NAME"
  }
  ```

    | Valor | Reemplazar por |
    | --- | --- |
    | YOUR_NAMESPACE | El espacio de nombres del punto de conexión |
    | YOUR_PRIMARY_KEY | La cadena de conexión principal utilizada para autenticar |
    | YOUR_SECONDARY_KEY | La cadena de conexión principal utilizada para autenticar |
    | YOUR_EVENT_HUB_NAME | El nombre del centro de eventos |

- Enrute al tipo de evento de Event Hubs **DeviceMessage**. La inclusión de `EntityPath` en **connectionString** es obligatoria:

  ```JSON
  {
    "type": "EventHub",
    "eventTypes": [
      "DeviceMessage"
    ],
    "connectionString": "Endpoint=sb://YOUR_NAMESPACE.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=YOUR_PRIMARY_KEY;EntityPath=YOUR_EVENT_HUB_NAME",
    "secondaryConnectionString": "Endpoint=sb://YOUR_NAMESPACE.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=YOUR_SECONDARY_KEY;EntityPath=YOUR_EVENT_HUB_NAME",
    "path": "YOUR_EVENT_HUB_NAME"
  }
  ```

    | Valor | Reemplazar por |
    | --- | --- |
    | YOUR_NAMESPACE | El espacio de nombres del punto de conexión |
    | YOUR_PRIMARY_KEY | La cadena de conexión principal utilizada para autenticar |
    | YOUR_SECONDARY_KEY | La cadena de conexión principal utilizada para autenticar |
    | YOUR_EVENT_HUB_NAME | El nombre del centro de eventos |

> [!NOTE]  
> Tras la creación de un nuevo punto de conexión, pueden transcurrir de 5 a 10 minutos para que comience a recibir eventos en el punto de conexión.

## <a name="primary-and-secondary-connection-keys"></a>Claves de conexión principal y secundaria

Cuando una clave de conexión principal deja de estar autorizada, el sistema intenta usar automáticamente la clave de conexión secundaria. De esta forma, se cuenta con una copia de seguridad y esto permite autenticarse correctamente y actualizar la clave principal a través de la API de puntos de conexión.

Si las claves de conexión principal y secundaria no están autorizadas, el sistema especifica un tiempo de espera de interrupción exponencial de hasta 30 minutos. Se cancelan los eventos cada vez que se desencadene el tiempo de espera de interrupción.

Cada vez que el sistema está en estado de espera de interrupción, actualizar las claves de las conexiones mediante Endpoints API puede tardar hasta 30 minutos en surtir efecto.

## <a name="unreachable-endpoints"></a>Puntos de conexión inaccesibles

Cuando un punto de conexión se vuelve inaccesible, el sistema especifica un tiempo de espera de interrupción exponencial de hasta 30 minutos. Se cancelan los eventos cada vez que se desencadene el tiempo de espera de interrupción.

## <a name="next-steps"></a>Pasos siguientes

- Aprenda [a usar Azure Digital Twins Swagger](how-to-use-swagger.md).

- Aprenda más sobre cómo [enrutar eventos y mensajes](concepts-events-routing.md) en Azure Digital Twins.
