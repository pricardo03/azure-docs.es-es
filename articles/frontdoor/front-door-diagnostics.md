---
title: 'Azure Front Door Service: métricas y registro | Microsoft Docs'
description: Este artículo le ayudará a entender las diferentes métricas y registros de acceso que Azure Front Door Service admite.
services: frontdoor
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/18/2018
ms.author: sharadag
ms.openlocfilehash: 643ae03a9350868b172d99b2af7ce23e34fedf47
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46998005"
---
# <a name="monitoring-metrics-for-front-door"></a>Supervisión de métricas para Front Door

Con Azure Front Door Service, puede supervisar las métricas clave para validar la configuración de Front Door, junto con la utilización, el mantenimiento y el rendimiento de dicho servicio.

## <a name="metrics"></a>Métricas

Las métricas son una característica de determinados recursos de Azure en los que puede ver contadores de rendimiento en el portal. Las métricas siguientes están disponibles en Front Door:

| Métrica | Nombre de métrica para mostrar | Unidad | Dimensiones | DESCRIPCIÓN |
| --- | --- | --- | --- | --- |
| RequestCount | Recuento de solicitudes | Recuento | HttpStatus</br>HttpStatusGroup</br>ClientRegion</br>ClientCountry | Número de solicitudes de cliente que atiende Front Door.  |
| RequestSize | Tamaño de la solicitud | Bytes | HttpStatus</br>HttpStatusGroup</br>ClientRegion</br>ClientCountry | Número de bytes enviados como solicitudes de clientes a Front Door. |
| ResponseSize | Tamaño de la respuesta | Bytes | HttpStatus</br>HttpStatusGroup</br>ClientRegion</br>ClientCountry | Número de bytes enviados como respuestas de Front Door a los clientes. |
| TotalLatency | Latencia total | Milisegundos | HttpStatus</br>HttpStatusGroup</br>ClientRegion</br>ClientCountry | Tiempo calculado desde que Front Door recibe la solicitud de cliente hasta que el cliente reconoce el último byte de respuesta de Front Door. |
| BackendRequestCount | Recuento de solicitudes de back-end | Recuento | HttpStatus</br>HttpStatusGroup</br>Back-end | Número de solicitudes enviadas de Front Door a los servidores back-end. |
| BackendRequestLatency | Latencia de las solicitudes de back-end | Milisegundos | Back-end | Tiempo calculado desde que Front Door envía la solicitud al servidor back-end hasta que Front Door recibe el último byte de respuesta del servidor back-end. |
| BackendHealthPercentage | Porcentaje de estado del back-end | Percent | Back-end</br>BackendPool | El porcentaje de sondeos de estado correctos de Front Door a los servidores back-ends. |
| WebApplicationFirewallRequestCount | Recuento de solicitudes del firewall de aplicaciones web | Recuento | PolicyName</br>RuleName</br>. | Número de solicitudes de cliente procesadas por la seguridad del nivel de aplicación de Front Door. |


## <a name="next-steps"></a>Pasos siguientes

- Aprenda a [crear una instancia de Front Door](quickstart-create-front-door.md).
- Más información acerca de cómo [funciona Front Door](front-door-routing-architecture.md).
