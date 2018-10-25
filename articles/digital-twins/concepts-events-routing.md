---
title: Enrutamiento de eventos y mensajes con Azure Digital Twins | Microsoft Docs
description: Información general sobre el enrutamiento de eventos y mensajes a puntos de conexión de servicio con Azure Digital Twins
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/08/2018
ms.author: alinast
ms.openlocfilehash: 9b861f0991b11637c7b27b645d4506e658ea53b3
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2018
ms.locfileid: "49324034"
---
# <a name="routing-events-and-messages"></a>Enrutamiento de eventos y mensajes

A menudo, las soluciones de IoT reúnen varios servicios eficaces como almacenamiento, análisis y mucho más. En este artículo se describe cómo conectar aplicaciones de Azure Digital Twins a los servicios de almacenamiento, IA y análisis de Azure para enriquecerlas con más información y funcionalidades.

## <a name="route-types"></a>Tipos de ruta

Digital Twins ofrece dos formas para integrar eventos de IoT en otros servicios de Azure o en aplicaciones empresariales:

* **Enrutamiento de eventos de Digital Twins**: los eventos Digital Twins de Azure se pueden desencadenar cuando un objeto en el gráfico espacial cambia, cuando se reciben datos de telemetría, o cuando una función definida por el usuario crea una notificación basada en condiciones predefinidas. Los usuarios pueden enviar estos eventos a [instancias de Event Hubs](https://azure.microsoft.com/services/event-hubs/), [temas de Service Bus](https://azure.microsoft.com/services/service-bus/) o [Event Grid](https://azure.microsoft.com/services/event-grid/) para su posterior procesamiento.

* **Enrutamiento de telemetría de dispositivo**: además de los eventos de enrutamientos, Azure Digital Twins también puede enrutar los mensajes de telemetría de dispositivo sin procesar a las instancias de Event Hubs para obtener más información y análisis. Digital Twins no procesa estos tipos de mensajes, y solo se reenvían al **centro de eventos**.

Los usuarios pueden especificar uno o varios puntos de conexión de salida para enviar eventos o para reenviar mensajes. Los eventos y mensajes se enviarán a los puntos de conexión según estas preferencias de enrutamiento predefinidas. En otras palabras, los usuarios pueden especificar un punto de conexión determinado para recibir eventos de operación de gráficos, otro para recibir eventos de telemetría de dispositivo, y así sucesivamente.

![Enrutamiento de eventos de Digital Twins][1]

El enrutamiento a las **instancias de Event Hubs** mantendrá el orden en que se envían los mensajes de telemetría, de modo que lleguen al punto de conexión en el mismo orden en que se recibieron originalmente. **Event Grid** y **Service Bus** no garantizan que los puntos de conexión reciban eventos en el mismo orden en que se produjeron. Sin embargo, el esquema del evento incluye una marca de tiempo que se puede utilizar para identificar el orden después de que los eventos lleguen al punto de conexión.

## <a name="route-implementation"></a>Implementación de rutas

El servicio de Azure Digital Twins actualmente admite los siguientes **EndpointTypes**:

* **EventHub**: es el punto de conexión de la cadena de conexión del centro de eventos.
* **ServiceBus**: es el punto de conexión de la cadena de conexión de Service Bus.
* **EventGrid**: es el punto de conexión de la cadena de conexión de Event Grid.

Azure Digital Twins actualmente admite los siguientes **EventTypes** que se enviarán al punto de conexión elegido:

* **DeviceMessages**: son mensajes de telemetría enviados desde los dispositivos de los usuarios y reenviados por el sistema.
* **TopologyOperation**: son las operaciones que cambian el gráfico o sus metadatos. Por ejemplo, agregar o eliminar una entidad, como un espacio.
* **SpaceChange**: son los cambios en el valor calculado de un espacio como resultado de un mensaje de telemetría de dispositivo.
* **SensorChange**: son los cambios en el valor calculado de un sensor como resultado de un mensaje de telemetría de dispositivo.
* **UdfCustom**: son las notificaciones personalizadas de una función definida por el usuario.

> [!IMPORTANT]
> No todos los **EndpointTypes** admiten todos los **EventTypes**.
> Consulte la tabla siguiente para conocer los **EventTypes** que se permiten para cada **EndpointType**.

|             | DeviceMessages | TopologyOperation | SpaceChange | SensorChange | UdfCustom |
| ----------- | -------------- | ----------------- | ----------- | ------------ | --------- |
| **EventHub**|     X          |         X         |     X       |      X       |   X       |
| **Service Bus**|              |         X         |     X       |      X       |   X       |
| **EventGrid**|               |         X         |     X       |      X       |   X       |

>[!NOTE]
>Para obtener más información sobre cómo crear puntos de conexión y acceder a ejemplos de esquema de los eventos, consulte [Puntos de conexión y salida](how-to-egress-endpoints.md).

## <a name="next-steps"></a>Pasos siguientes

Para obtener información acerca de los límites de la versión preliminar pública, lea los [Límites de la versión preliminar de Azure Digital Twins](concepts-service-limits.md).

Para probar un ejemplo de Azure Digital Twins, lea el [Inicio rápido para buscar salas disponibles](quickstart-view-occupancy-dotnet.md).

<!-- Images -->
[1]: media/concepts/digital-twins-events-routing.png