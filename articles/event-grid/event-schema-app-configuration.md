---
title: Esquema de eventos de configuración de aplicaciones de Azure Event Grid de Azure
description: Describe las propiedades que se proporcionan para los eventos de configuración de la aplicación de Azure con Azure Event Grid
services: event-grid
author: jimmyca
ms.service: event-grid
ms.topic: reference
ms.date: 05/30/2019
ms.author: jimmyca
ms.openlocfilehash: fe0274f723692eea3cfd25cc0e9e146b35dce2ae
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2019
ms.locfileid: "66735787"
---
# <a name="azure-event-grid-event-schema-for-azure-app-configuration"></a>Esquema de eventos de Azure Event Grid para la configuración de la aplicación de Azure

En este artículo proporciona las propiedades y esquema para los eventos de configuración de aplicación de Azure. Para una introducción a los esquemas de eventos, consulte [Esquema de eventos de Azure Event Grid](event-schema.md).

Para obtener una lista de secuencias de comandos de ejemplo y tutoriales, consulte [origen de eventos de configuración de aplicaciones Azure](event-sources.md#app-configuration).

## <a name="available-event-types"></a>Tipos de eventos disponibles

Configuración de la aplicación de Azure emite los siguientes tipos de evento:

| Tipo de evento | DESCRIPCIÓN |
| ---------- | ----------- |
| Microsoft.AppConfiguration.KeyValueModified | Se genera cuando se crea o sustituye un valor de clave. |
| Microsoft.AppConfiguration.KeyValueDeleted | Se genera cuando se elimina un valor de clave. |

## <a name="example-event"></a>Evento de ejemplo

El ejemplo siguiente muestra el esquema de un evento de modificación de pares clave-valor: 

```json
[{
  "id": "84e17ea4-66db-4b54-8050-df8f7763f87b",
  "topic": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/microsoft.appconfiguration/configurationstores/contoso",
  "subject": "https://contoso.azconfig.io/kv/Foo?label=FizzBuzz",
  "data": {
    "key": "Foo",
    "label": "FizzBuzz",
    "etag": "FnUExLaj2moIi4tJX9AXn9sakm0"
  },
  "eventType": "Microsoft.AppConfiguration.KeyValueModified",
  "eventTime": "2019-05-31T20:05:03Z",
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```

El esquema para un evento eliminado de pares clave-valor es similar: 

```json
[{
  "id": "84e17ea4-66db-4b54-8050-df8f7763f87b",
  "topic": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/microsoft.appconfiguration/configurationstores/contoso",
  "subject": "https://contoso.azconfig.io/kv/Foo?label=FizzBuzz",
  "data": {
    "key": "Foo",
    "label": "FizzBuzz",
    "etag": "FnUExLaj2moIi4tJX9AXn9sakm0"
  },
  "eventType": "Microsoft.AppConfiguration.KeyValueDeleted",
  "eventTime": "2019-05-31T20:05:03Z",
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```
 
## <a name="event-properties"></a>Propiedades de evento

Un evento tiene los siguientes datos de nivel superior:

| Propiedad | Escriba | DESCRIPCIÓN |
| -------- | ---- | ----------- |
| topic | string | Ruta de acceso completa a los recursos del origen del evento. En este campo no se puede escribir. Event Grid proporciona este valor. |
| subject | string | Ruta al asunto del evento definida por el anunciante. |
| eventType | string | Uno de los tipos de eventos registrados para este origen de eventos. |
| eventTime | string | La hora de generación del evento en función de la hora UTC del proveedor. |
| id | string | Identificador único para el evento |
| data | objeto | Datos de eventos de configuración de aplicación. |
| dataVersion | string | Versión del esquema del objeto de datos. El publicador define la versión del esquema. |
| metadataVersion | string | Versión del esquema de los metadatos del evento. Event Grid define el esquema de las propiedades de nivel superior. Event Grid proporciona este valor. |

El objeto data tiene las siguientes propiedades:

| Propiedad | Escriba | DESCRIPCIÓN |
| -------- | ---- | ----------- |
| key | string | La clave de lo pares clave-valor que se ha modificado o eliminado. |
| etiqueta | string | La etiqueta, si existe, de lo pares clave-valor que se ha modificado o eliminado. |
| ETag | string | Para `KeyValueModified` la etag de la nueva clave y valor. Para `KeyValueDeleted` la etag de lo pares clave-valor que se ha eliminado. |
 
## <a name="next-steps"></a>Pasos siguientes

* Para una introducción a Azure Event Grid, consulte [Introducción a Azure Event Grid](overview.md).
* Para más información acerca de la creación de una suscripción de Azure Event Grid, consulte [Esquema de suscripción de Event Grid](subscription-creation-schema.md).
* Para obtener una introducción a cómo trabajar con eventos de configuración de aplicación de Azure, consulte [eventos de configuración de aplicaciones de Azure de ruta - CLI de Azure](../azure-app-configuration/howto-app-configuration-event.md?toc=%2fazure%2fevent-grid%2ftoc.json). 