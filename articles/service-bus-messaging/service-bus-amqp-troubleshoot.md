---
title: Solucionar errores de AMQP de Service Bus de Azure | Microsoft Docs
description: Proporciona una lista de errores AMQP que puede recibir al usar Azure Service Bus y provocar de esos errores.
services: service-bus-messaging
documentationcenter: na
author: axisc
manager: timlt
editor: spelluru
ms.assetid: ''
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/03/2019
ms.author: aschhab
ms.openlocfilehash: 85d24a9e7c753ec5dba80c4f259dd3fb51d9c14b
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "58910051"
---
# <a name="amqp-errors-in-azure-service-bus"></a>Errores AMQP en Azure Service Bus
Este artículo proporciona algunos de los errores de que recepción al usar AMQP con Azure Service Bus. Son todos los comportamientos estándares del servicio. Para evitarlos, puede realizar llamadas de envío y recepción en el vínculo de conexión o que se volverá a crear automáticamente el vínculo de conexión.

## <a name="link-is-closed"></a>Se cierra el vínculo 
Vea el siguiente error cuando la conexión de AMQP y el vínculo se activa, pero no hay llamadas (por ejemplo, envío o recepción) se realizan mediante el vínculo durante 10 minutos. Por lo tanto, se cierra el vínculo. La conexión está abierta todavía.

```
amqp:link:detach-forced:The link 'G2:7223832:user.tenant0.cud_00000000000-0000-0000-0000-00000000000000' is force detached by the broker due to errors occurred in publisher(link164614). Detach origin: AmqpMessagePublisher.IdleTimerExpired: Idle timeout: 00:10:00. TrackingId:00000000000000000000000000000000000000_G2_B3, SystemTracker:mynamespace:Topic:MyTopic, Timestamp:2/16/2018 11:10:40 PM
```

## <a name="connection-is-closed"></a>Se cierra la conexión
Vea el siguiente error en la conexión de AMQP cuando se han cerrado todos los vínculos de la conexión porque ha habido ninguna actividad (inactivo) y no se ha creado un nuevo vínculo en 5 minutos.

```
Error{condition=amqp:connection:forced, description='The connection was inactive for more than the allowed 300000 milliseconds and is closed by container 'LinkTracker'. TrackingId:00000000000000000000000000000000000_G21, SystemTracker:gateway5, Timestamp:2019-03-06T17:32:00', info=null}
```

## <a name="link-is-not-created"></a>No se crea el vínculo 
Vea este error cuando se crea una nueva conexión de AMQP, pero no se crea un vínculo dentro de 1 minuto de la creación de la conexión de AMQP.

```
Error{condition=amqp:connection:forced, description='The connection was inactive for more than the allowed 60000 milliseconds and is closed by container 'LinkTracker'. TrackingId:0000000000000000000000000000000000000_G21, SystemTracker:gateway5, Timestamp:2019-03-06T18:41:51', info=null}
```

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre AMQP y Service Bus, visite los siguientes vínculos:

* [Información general sobre AMQP para Service Bus]
* [Guía del protocolo AMQP 1.0]
* [AMQP de Service Bus para Windows Server]

[Información general sobre AMQP para Service Bus]: service-bus-amqp-overview.md
[Guía del protocolo AMQP 1.0]: service-bus-amqp-protocol-guide.md
[AMQP de Service Bus para Windows Server]: https://docs.microsoft.com/previous-versions/service-bus-archive/dn282144(v=azure.100)
