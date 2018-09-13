---
title: Entrega y reintento de entrega de Azure Event Grid
description: Describe cómo Azure Event Grid entrega eventos y cómo administra los mensajes no entregados.
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: conceptual
ms.date: 09/05/2018
ms.author: tomfitz
ms.openlocfilehash: 2a9ff23e5182c8cb7c91ad93e368f61f258c84f8
ms.sourcegitcommit: 3d0295a939c07bf9f0b38ebd37ac8461af8d461f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/06/2018
ms.locfileid: "43841599"
---
# <a name="event-grid-message-delivery-and-retry"></a>Entrega y reintento de entrega de mensajes de Event Grid 

En este artículo se describe cómo Azure Event Grid administra los eventos cuando no se confirma la entrega.

Event Grid ofrece entrega duradera. Entrega cada mensaje por lo menos una vez en cada suscripción. Los eventos se envían inmediatamente al webhook registrado de cada suscripción. Si un webhook no acusa recibo de un evento en los 60 segundos siguientes al primer intento, Event Grid reintenta la entrega del evento. 

Actualmente, Event Grid envía cada evento individualmente a los suscriptores. El suscriptor recibe una matriz con un solo evento.

## <a name="message-delivery-status"></a>Estado de entrega de mensajes

Event Grid usa códigos de respuesta HTTP para acusar recibo de eventos. 

### <a name="success-codes"></a>Códigos de éxito

Los siguientes códigos de respuesta HTTP response indican que un evento se ha entregado correctamente en el webhook. Event Grid considera la entrega finalizada.

- 200 OK
- 202 - Aceptado

### <a name="failure-codes"></a>Códigos de error

Los siguientes códigos de respuesta HTTP indican que el intento de entrega de un evento no se ha realizado.

- 400 - Solicitud incorrecta
- 401 No autorizado
- 404 No encontrado
- 408 - Tiempo de espera de solicitud
- 413 Entidad de solicitud demasiado larga
- 414 - URI de solicitud demasiado largo
- 429 Demasiadas solicitudes
- Error de servidor interno 500
- Servicio no disponible 503
- Tiempo de espera de puerta de enlace 504

Si tiene [configurado un punto de conexión de mensajes fallidos](manage-event-delivery.md) y Event Grid recibe un código de respuesta 400 o 413, Event Grid envía de inmediato el evento al punto de conexión de mensajes fallidos. En caso contrario, Event Grid reintenta todos los errores.

## <a name="retry-intervals-and-duration"></a>Intervalos de reintento y duración

Event Grid usa una directiva de reintentos de retroceso exponencial para la entrega de eventos. Si el webhook no responde o devuelve un código de error, Event Grid reintenta la entrega según la siguiente programación:

1. 10 segundos
2. 30 segundos
3. 1 minuto
4. 5 minutos
5. 10 minutos
6. 30 minutos
7. 1 hora

Event Grid agrega una pequeña selección aleatoria a todos los intervalos de reintento. Después de una hora, la entrega del evento se vuelve a intentar cada una hora.

De forma predeterminada, Event Grid expira todos los eventos que no se entregan en 24 horas. Puede [personalizar la directiva de reintentos](manage-event-delivery.md) al crear una suscripción al evento. Proporcione el número máximo de intentos de entrega (el valor predeterminado es 30) y el periodo de vida de los eventos (el valor predeterminado es 1440 minutos).

## <a name="dead-letter-events"></a>Eventos fallidos

Si Event Grid no puede entregar un evento, puede enviar el evento no entregado a una cuenta de almacenamiento. Este proceso se conoce como colas de eventos fallidos. Para ver los eventos sin entregar, puede extraerlos de la ubicación de los eventos fallidos. Para más información, consulte [Directivas de reintentos y eventos fallidos](manage-event-delivery.md).

## <a name="next-steps"></a>Pasos siguientes

* Para ver el estado de las entregas de eventos, consulte [Supervisar la entrega de mensajes de Event Grid](monitor-event-delivery.md).
* Para personalizar las opciones de entrega de eventos, consulte [Administración de la configuración de entrega de Event Grid](manage-event-delivery.md).
* Para obtener una introducción a Event Grid, vea [Acerca de Event Grid](overview.md).
* Para comenzar a usar rápidamente Event Grid, vea [Creación y enrutamiento de eventos personalizados con Azure Event Grid](custom-event-quickstart.md).