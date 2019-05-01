---
title: Reaccionar a eventos de Azure Maps mediante Event Grid | Microsoft Docs
description: Aprenda a reaccionar a eventos de Azure Maps mediante Event Grid.
author: walsehgal
ms.author: v-musehg
ms.date: 02/08/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: a70011b934398ac4e7f74bb67013e93bb5e86e4e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60799162"
---
# <a name="react-to-azure-maps-events-by-using-event-grid"></a>Reaccionar a eventos de Azure Maps mediante Event Grid 

Azure Maps se integra con Azure Event Grid para poder enviar notificaciones de eventos a otros servicios y desencadenar procesos de descarga. El propósito de este artículo es ayudarle a configurar las aplicaciones empresariales para escuchar eventos de Azure Maps, a fin de poder crear reacciones a eventos críticos de manera confiable, escalable y segura. Por ejemplo, compile una aplicación que realice varias acciones como, por ejemplo, actualizar una base de datos, crear un vale y enviar una notificación por correo electrónico cada vez que un dispositivo entre en una geovalla.

Azure Event Grid es un servicio de enrutamiento de eventos totalmente administrado que utiliza un modelo de publicación-suscripción. Event Grid tiene compatibilidad integrada para servicios de Azure, como [Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-overview) y [Azure Logic Apps](https://docs.microsoft.com/azure/azure-functions/functions-overview), y puede proporcionar alertas de eventos a los servicios que no son de Azure mediante webhooks. Para obtener una lista completa de los controladores de eventos que Event Grid admite, vea [una introducción a Azure Event Grid](https://docs.microsoft.com/azure/event-grid/overview).


![Modelo funcional de Azure Event Grid](./media/azure-maps-event-grid-integration/azure-event-grid-functional-model.png)


## <a name="azure-maps-events-types"></a>Tipos de eventos de Azure Maps

Event Grid usa las [suscripciones a eventos](https://docs.microsoft.com/azure/event-grid/concepts#event-subscriptions) para enrutar los mensajes de eventos a los suscriptores. Una cuenta de Azure Maps emite los siguientes tipos de eventos: 

| Tipo de evento | DESCRIPCIÓN |
| ---------- | ----------- |
| Microsoft.Maps.GeofenceEntered | Se genera cuando se mueven las coordenadas recibidas desde fuera de una determinada geovalla a dentro |
| Microsoft.Maps.GeofenceExited | Se genera cuando se mueven las coordenadas recibidas desde dentro de una determinada geovalla a fuera |
| Microsoft.Maps.GeofenceResult | Se genera cada vez que una consulta de geovalla devuelve un resultado, independientemente del estado |

## <a name="event-schema"></a>Esquema del evento

En el ejemplo siguiente, se muestra el esquema de un evento GeofenceResult

```JSON
{   
   "id":"451675de-a67d-4929-876c-5c2bf0b2c000", 
   "topic":"/subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.Maps/accounts/{accountName}", 
   "subject":"/spatial/geofence/udid/{udid}/id/{eventId}", 
   "data":{   
      "geometries":[   
         {   
            "deviceId":"device_1", 
            "udId":"1a13b444-4acf-32ab-ce4e-9ca4af20b169", 
            "geometryId":"1", 
            "distance":999.0, 
            "nearestLat":47.609833, 
            "nearestLon":-122.148274 
         }, 
         {   
            "deviceId":"device_1", 
            "udId":"1a13b444-4acf-32ab-ce4e-9ca4af20b169", 
            "geometryId":"2", 
            "distance":999.0, 
            "nearestLat":47.621954, 
            "nearestLon":-122.131841 
         } 
      ], 
      "expiredGeofenceGeometryId":[   
      ], 
      "invalidPeriodGeofenceGeometryId":[   
      ] 
   }, 
   "eventType":"Microsoft.Maps.GeofenceResult", 
   "eventTime":"2018-11-08T00:52:08.0954283Z", 
   "metadataVersion":"1", 
   "dataVersion":"1.0" 
}
```

## <a name="tips-for-consuming-events"></a>Sugerencias para consumir eventos

Las aplicaciones que controlan los eventos de geovallas de Azure Maps deben seguir algunos procedimientos recomendados:

* Se pueden configurar varias suscripciones para enrutar eventos al mismo controlador de eventos. Es importante no asumir que los eventos proceden de un origen determinado. Compruebe siempre el tema del mensaje para asegurarse de que procede del origen que está esperando.
* Los mensajes pueden llegar desordenados o después de un retraso. Utilice el campo `X-Correlation-id` del encabezado de la respuesta para saber si la información sobre los objetos está actualizada.
* Cuando se llama a Get y POST Geofence API con el parámetro de modo establecido en `EnterAndExit`, se genera un evento de entrada o de salida para cada geometría de la geovalla en la que se ha cambiado el estado desde la anterior llamada a Geofence API.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre cómo usar las geovallas para controlar operaciones en un sitio de construcción, consulte:

> [!div class="nextstepaction"] 
> [Configuración de una geovalla con Azure Maps](tutorial-geofence.md)