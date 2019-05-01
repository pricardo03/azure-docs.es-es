---
title: Supervisar la entrega de mensajes de Azure Event Grid
description: Describe cómo se supervisa la entrega de mensajes de Azure Event Grid.
services: event-grid
author: spelluru
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 01/22/2019
ms.author: spelluru
ms.openlocfilehash: fdd18b833794c25cb90188ba8bc418d4785492ba
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60824159"
---
# <a name="monitor-event-grid-message-delivery"></a>Supervisar la entrega de mensajes de Event Grid 

En este artículo se describe cómo usar el portal para ver el estado de las entregas de eventos.

Event Grid ofrece entrega duradera. Entrega cada mensaje por lo menos una vez en cada suscripción. Los eventos se envían inmediatamente al webhook registrado de cada suscripción. Si un webhook no acusa recibo de un evento en los 60 segundos siguientes al primer intento, Event Grid reintenta la entrega del evento.

Para obtener información acerca de los reintentos y las entregas de eventos, consulte [Entrega y reintento de entrega de mensajes de Event Grid](delivery-and-retry.md).

## <a name="delivery-metrics"></a>Métricas de entrega

El portal muestra las métricas para el estado de la entrega de mensajes de eventos.

Para los temas, las métricas son:

* **Publicación correcta**: el evento se envió correctamente al tema y se procesó con una respuesta 2xx.
* **Error al publicar**: el evento se envió al tema, pero se rechazó con un código de error.
* **Sin coincidencia**: el evento se publicó correctamente en el tema, pero no coincide con ninguna suscripción a eventos. Se descartó el evento.

Para las suscripciones, las métricas son:

* **Entrega realizada**: el evento se entregó correctamente al punto de conexión de la suscripción y se recibió una respuesta 2xx.
* **Entrega no realizada**: el evento se envió al punto de conexión de la suscripción, pero se recibió una respuesta 4xx o 5xx.
* **Eventos expirados**: no se entregó el evento y se enviaron todos los reintentos. Se descartó el evento.
* **Eventos coincidentes**: la suscripción a eventos coincide con el evento del tema.

## <a name="event-subscription-status"></a>Estado de la suscripción a eventos

Para ver las métricas para una suscripción de eventos, puede buscar por tipo de suscripción o por suscripciones para un recurso concreto.

Para buscar por tipo de suscripción de eventos, seleccione **Todos los servicios**.

![Seleccionar todos los servicios](./media/monitor-event-delivery/all-services.png)

Busque **Event Grid** y seleccione **Suscripciones de Event Grid** entre las opciones disponibles.

![Buscar suscripciones a eventos](./media/monitor-event-delivery/search-and-select.png)

Filtre por el tipo de evento, la suscripción y la ubicación. Seleccione **Métricas** para la suscripción que quiere consultar.

![Filtrar suscripciones a eventos](./media/monitor-event-delivery/filter-events.png)

Consulte las métricas para el tema del evento y la suscripción.

![Ver métricas de eventos](./media/monitor-event-delivery/subscription-metrics.png)

Para buscar las métricas de un recurso concreto, seleccione ese recurso. Después, seleccione **Eventos**.

![Seleccionar eventos para un recurso](./media/monitor-event-delivery/select-events.png)

Puede ver las métricas para las suscripciones para ese recurso.

## <a name="custom-event-status"></a>Estado de evento personalizado

Si ha publicado un tema personalizado, puede consultar sus métricas. Seleccione el grupo de recursos del tema y seleccione el tema.

![Seleccionar tema personalizado](./media/monitor-event-delivery/select-custom-topic.png)

Consulte las métricas para el tema de eventos personalizado.

![Ver métricas de eventos](./media/monitor-event-delivery/custom-topic-metrics.png)

## <a name="next-steps"></a>Pasos siguientes

* Para obtener información acerca de los reintentos y las entregas de eventos, consulte [Entrega y reintento de entrega de mensajes de Event Grid](delivery-and-retry.md).
* Para obtener una introducción a Event Grid, vea [Acerca de Event Grid](overview.md).
* Para comenzar a usar rápidamente Event Grid, vea [Creación y enrutamiento de eventos personalizados con Azure Event Grid](custom-event-quickstart.md).
