---
title: Planes de servicio y cuotas para Azure Spring Cloud
description: Más información sobre las cuotas y los planes de servicio de Azure Spring Cloud
author: jpconnock
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 9/27/2019
ms.author: jeconnoc
ms.openlocfilehash: 89934ee0ab0c901a904a1a5ac2fb620185571a23
ms.sourcegitcommit: d773b5743cb54b8cbcfa5c5e4d21d5b45a58b081
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/08/2019
ms.locfileid: "72038687"
---
# <a name="quotas-and-service-plans-for-azure-spring-cloud"></a>Cuotas y planes de servicio de Azure Spring Cloud

Todos los servicios de Azure establecen límites y cuotas predeterminados para recursos y características.  Durante el período de versión preliminar, Azure Spring Cloud ofrece solo un plan de servicio.

En este artículo se detallan las cuotas de servicio que se ofrecen durante el período de versión preliminar actual.

## <a name="azure-spring-cloud-service-tiers-and-quotas"></a>Niveles y cuotas de servicio en Azure Spring Cloud

Durante el período de versión preliminar, Azure Spring Cloud ofrece solo un nivel de servicio.

Resource | Importe
------- | -------
vCPU | 4
Memoria | 8 GBytes
Suscripción a Azure Spring Cloud | 1
Instancias del servicio Azure Spring Cloud por región y suscripción | 2
Instancias de aplicación totales por instancia del servicio Azure Spring Cloud | 50
Instancias de aplicación totales por aplicación Spring | 20
Volúmenes persistentes | 10 x 50 GBytes

Cuando alcance una cuota, recibirá un error 400 que indica: "Quota exceeds limit for subscription *your subscription* in region *region where your Azure Spring Cloud service is created*" (La cuota supera el límite de la suscripción <su suscripción> en la región <región en la que se ha creado el servicio de Azure Spring Cloud>).

## <a name="next-steps"></a>Pasos siguientes

Pueden aumentarse ciertas cuotas y límites predeterminados. Si el recurso requiere un aumento, envíenos la solicitud: azure-spring-cloud@service.microsoft.com.
