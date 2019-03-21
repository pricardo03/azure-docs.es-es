---
title: Reacción ante eventos de Azure Media Services | Microsoft Docs
description: Use Azure Event Grid para suscribirse a los eventos de Media Services.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 03/12/2019
ms.author: juliako
ms.openlocfilehash: cb5d6474a0c830933c712e1008015b5220617c96
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "57850911"
---
# <a name="handling-event-grid-events"></a>Control de eventos de Event Grid

Los eventos de Media Services permiten que las aplicaciones reaccionen a distintos eventos (por ejemplo, el evento de cambio del estado del trabajo) a través de modernas arquitecturas sin servidor. Esto se consigue sin necesidad de código complejo ni de servicios de sondeo costosos e ineficientes. En su lugar, se insertan eventos a través de [Azure Event Grid](https://azure.microsoft.com/services/event-grid/) a los controladores de eventos como [Azure Functions](https://azure.microsoft.com/services/functions/), [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/) o incluso a su propio webhook y solo se paga por lo que se usa. Para información sobre los precios, consulte [Precios de Event Grid](https://azure.microsoft.com/pricing/details/event-grid/).

La disponibilidad de los eventos de Media Services está asociada a la [disponibilidad](../../event-grid/overview.md) de Event Grid. Estarán disponibles en otras regiones cuando lo esté Event Grid.  

## <a name="media-services-events-and-schemas"></a>Los esquemas y los eventos de Media Services

Event Grid usa las [suscripciones a eventos](../../event-grid/concepts.md#event-subscriptions) para enrutar los mensajes de eventos a los suscriptores. Los eventos de Media Services contienen toda la información necesaria para responder a cualquier cambio que se produzca en los datos. Puede identificar un evento de Media Services porque la propiedad eventType comienza por "Microsoft.Media".

Para más información, consulte el artículo sobre los [esquemas de eventos de Media Services](media-services-event-schemas.md).

## <a name="practices-for-consuming-events"></a>Prácticas para consumir eventos

Las aplicaciones que controlan los eventos de Media Services deben seguir algunos procedimientos recomendados:

* Dado que se pueden configurar varias suscripciones para enrutar eventos al mismo controlador de eventos, es importante no asumir que los eventos provienen de un origen determinado, sino comprobar el tema del mensaje para asegurarse de que proviene de la cuenta de almacenamiento esperable.
* De igual forma, compruebe que eventType es uno de los que está preparado para procesar y no asuma que todos los eventos que reciba van a ser los tipos que espera.
* Ignore los campos que no comprenda.  Esta práctica le ayudará a mantenerse resistente a las nuevas características que puedan agregarse en el futuro.
* Use las coincidencias de prefijo y sufijo "subject" para limitar los eventos a un evento determinado.

## <a name="next-steps"></a>Pasos siguientes

[Obtención de eventos de estado del trabajo](job-state-events-cli-how-to.md)
