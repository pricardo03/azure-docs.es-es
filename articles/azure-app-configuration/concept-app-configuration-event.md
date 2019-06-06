---
title: Reacción a eventos de pares clave-valor de configuración de la aplicación de Azure | Microsoft Docs
description: Use Azure Event Grid para suscribirse a eventos de configuración de la aplicación.
services: azure-app-configuration,event-grid
author: jimmyca
ms.author: jimmyca
ms.date: 05/30/2019
ms.topic: article
ms.service: azure-app-configuration
ms.openlocfilehash: 601124aef37d2b285db71130f5c63b3620c7768f
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2019
ms.locfileid: "66735652"
---
# <a name="reacting-to-azure-app-configuration-events"></a>Reacción ante eventos de configuración de la aplicación de Azure

Eventos de configuración de la aplicación de Azure permiten a las aplicaciones reaccionar a los cambios en los valores de clave. Esto se realiza sin necesidad de usar código complejo o servicios de sondeo costosos e ineficaces. En su lugar, se insertan eventos a través de [Azure Event Grid](https://azure.microsoft.com/services/event-grid/) a los suscriptores como [Azure Functions](https://azure.microsoft.com/services/functions/), [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/), o incluso su propio agente de escucha http personalizado y solo paga por lo que utiliza.

Eventos de configuración de la aplicación de Azure se envían a Azure Event Grid que proporciona servicios de entrega confiables a sus aplicaciones a través de enriquecido, vuelva a intentar las directivas y la entrega de mensajes no enviados. Para obtener más información, consulte [entrega de mensajes de Event Grid y vuelva a intentarlo](https://docs.microsoft.com/azure/event-grid/delivery-and-retry).

Escenarios de eventos de configuración de aplicación comunes incluyen actualizar la configuración de la aplicación, desencadenar las implementaciones, o cualquier flujo de trabajo orientados a la configuración. Cuando se realizan pocos cambios en el escenario, pero se requiere una respuesta inmediata, la arquitectura basada en eventos puede ser especialmente eficaz.

Eche un vistazo a [eventos de configuración de aplicaciones de Azure de ruta a un personalizado web endpoint - CLI](./howto-app-configuration-event.md) para obtener un ejemplo rápido. 

![Modelo de Event Grid](./media/event-grid-functional-model.png)

## <a name="available-azure-app-configuration-events"></a>Eventos de configuración de la aplicación de Azure disponibles
Event Grid usa las [suscripciones a eventos](../event-grid/concepts.md#event-subscriptions) para enrutar los mensajes de eventos a los suscriptores. Las suscripciones de eventos de configuración de la aplicación Azure pueden incluir dos tipos de eventos:  

> |Nombre del evento|DESCRIPCIÓN|
> |----------|-----------|
> |`Microsoft.AppConfiguration.KeyValueModified`|Se desencadena cuando se crea o sustituye un valor de clave|
> |`Microsoft.AppConfiguration.KeyValueDeleted`|Se desencadena cuando se elimina un valor de clave|

## <a name="event-schema"></a>Esquema del evento
Eventos de configuración de la aplicación de Azure contienen toda la información que necesaria para responder a los cambios en los datos. Puede identificar un evento de configuración de la aplicación porque la propiedad eventType comienza por "Microsoft.AppConfiguration". Puede encontrar información adicional acerca del uso de las propiedades de los eventos de Event Grid en [Esquema de eventos de Event Grid](../event-grid/event-schema.md).  

> |Propiedad|Escriba|DESCRIPCIÓN|
> |-------------------|------------------------|-----------------------------------------------------------------------|
> |topic|string|Identificador completo de Azure Resource Manager de la configuración de aplicación que emite el evento.|
> |subject|string|El URI de lo pares clave-valor que es el asunto del evento.|
> |eventTime|string|La fecha y hora que se generó el evento, en formato ISO 8601.|
> |eventType|string|"Microsoft.AppConfiguration.KeyValueModified" o "Microsoft.AppConfiguration.KeyValueDeleted".|
> |Id|string|Un identificador único de este evento.|
> |dataVersion|string|Versión del esquema del objeto de datos.|
> |metadataVersion|string|Versión del esquema de propiedades de nivel superior.|
> |data|objeto|Recopilación de datos de evento específico de configuración de la aplicación de Azure|
> |data.key|string|La clave de lo pares clave-valor que se ha modificado o eliminado.|
> |data.label|string|La etiqueta, si existe, de lo pares clave-valor que se ha modificado o eliminado.|
> |data.etag|string|Para `KeyValueModified` la etag de la nueva clave y valor. Para `KeyValueDeleted` la etag de lo pares clave-valor que se ha eliminado.|

Este es un ejemplo de un evento KeyValueModified:
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

Para obtener más información, consulte [esquema de eventos de configuración de aplicaciones Azure](../event-grid/event-schema-app-configuration.md).

## <a name="practices-for-consuming-events"></a>Prácticas para consumir eventos
Las aplicaciones que controlan los eventos de configuración de aplicación deben seguir algunas prácticas recomendadas:
> [!div class="checklist"]
> * Como se pueden configurar varias suscripciones para enrutar eventos al mismo controlador de eventos, es importante no suponer que los eventos son de un origen determinado, sino para comprobar el tema del mensaje para asegurarse de que proviene de la configuración de la aplicación que espera.
> * De igual forma, compruebe que eventType es uno de los que está preparado para procesar y no asuma que todos los eventos que reciba van a ser los tipos que espera.
> * Dado que los mensajes pueden llegar desordenados y con cierto retraso, utilice los campos de etag para saber si la información acerca de los objetos sigue estando actualizada.  Además, utilice los campos del secuenciador para conocer el orden de los eventos en cualquier objeto determinado.
> * Utilice el campo de asunto para tener acceso a lo pares clave-valor que se ha modificado.


## <a name="next-steps"></a>Pasos siguientes

Más información sobre Event Grid y pruebe los eventos de configuración de aplicación de Azure:

- [Una introducción a Azure Event Grid](../event-grid/overview.md)
- [Enrutamiento de eventos de configuración de la aplicación de Azure a un punto de conexión web personalizado](./howto-app-configuration-event.md)