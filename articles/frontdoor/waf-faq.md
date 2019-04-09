---
title: Firewall de aplicaciones web de Azure - preguntas más frecuentes
description: Esta página proporciona respuestas a las preguntas más frecuentes sobre el servicio de puerta de entrada de Azure
services: frontdoor
documentationcenter: ''
author: KumudD
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/02/2019
ms.author: kumud;tyao
ms.openlocfilehash: 05d01851d0a3dc9df6c396e862ce93defd957c70
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59255927"
---
# <a name="frequently-asked-questions-for-azure-web-application-firewall"></a>Preguntas para firewall de aplicaciones web de Azure más frecuentes

En este artículo se responde preguntas comunes sobre la funcionalidad y características de firewall (WAF) de aplicaciones web de Azure. 

## <a name="what-is-azure-waf"></a>¿Qué es Azure WAF?

WAF de Azure es un firewall de aplicaciones web que ayuda a protege sus aplicaciones web frente a amenazas comunes como SQL inyección de código, el scripting entre sitios y otras vulnerabilidades de seguridad de la web. Puede definir una directiva de WAF, que consta de una combinación de reglas personalizadas y administradas para controlar el acceso a sus aplicaciones web.

Para aplicaciones web hospedadas en servicios de Application Gateway o puerta de Azure, se puede aplicar una directiva de WAF de Azure.

## <a name="what-is-waf-for-azure-front-door-service"></a>¿Qué es WAF para el servicio de puerta de entrada de Azure? 

Azure puerta delantera es una red de entrega de contenido y aplicaciones altamente escalable y distribuida globalmente. WAF de Azure, cuando se integra con la puerta delantera, deja de denegación de servicio y los ataques a aplicaciones en el perímetro de red de Azure de destino, cerca de fuentes de ataque antes de que entre la red virtual, ofrece protección sin sacrificar el rendimiento.

## <a name="how-will-i-be-charged-for-azure-waf-for-front-door"></a>¿Cómo se cobrará la de WAF de Azure para la puerta de entrada?
Durante la versión preliminar pública, el uso de WAF en la puerta de entrada son gratuitas. Tenga en cuenta que la puerta delantera cargo adicional. Consulte los precios del servicio de puerta delantera [aquí](https://azure.microsoft.com/pricing/details/frontdoor/).

## <a name="does-azure-waf-support-https"></a>¿Azure WAF admite HTTPS?

Servicio de puerta de entrada ofrece la descarga de SSL. WAF se integra de forma nativa con la puerta delantera y puede inspeccionar una solicitud tras descifrarlo.

## <a name="does-azure-waf-support-ipv6"></a>¿Azure WAF admite IPv6?

Sí. Puede configurar restricciones de IP para IPv4 e IPv6.

## <a name="how-up-to-date-are-the-managed-rule-sets"></a>¿Grado de actualización son los conjuntos de reglas administrado?

Haremos lo posible para mantenerlo con cambiante panorama de amenazas. Una vez que se actualiza una regla nueva, se agrega a predeterminado conjunto de reglas con un nuevo número de versión.

## <a name="what-is-the-propagation-time-if-i-make-a-change-to-my-waf-policy"></a>¿Cuál es el tiempo de propagación si realizo un cambio en mi directiva de WAF?

Implementación de una directiva de WAF globalmente normalmente tarda aproximadamente 5 minutos y a menudo se complete antes.

## <a name="can-waf-policies-be-different-for-different-regions"></a>¿Las directivas de WAF y pueden ser diferentes para diferentes regiones?

Cuando se integra con el servicio de puerta de entrada, WAF es un recurso global. Misma configuración se aplica en todas las ubicaciones de la puerta de entrada.
 
## <a name="how-do-i-limit-access-to-my-back-end-to-be-from-front-door-only"></a>¿Cómo puedo limitar el acceso a mi back-end para ser solo de la puerta delantera?

Puede configurar la lista de Control de acceso de IP en el back-end para permitir solo la puerta delantera saliente intervalos de direcciones IP y denegar el acceso directo desde Internet. Se admiten etiquetas de servicio para que su uso en la red virtual. Además, puede comprobar que el campo de encabezado HTTP X-reenviados-Host es válido para la aplicación web.




## <a name="which-azure-waf-options-should-i-choose"></a>¿Qué opciones de WAF de Azure debería elegir?

Hay dos opciones al aplicar las directivas de WAF en Azure. WAF con Azure puerta de entrada es una solución de seguridad distribuida globalmente de edge. WAF con Application Gateway es una solución regional y dedicada. Se recomienda que elegir una solución basada en los requisitos de seguridad y el rendimiento general. Para obtener más información, consulte [equilibrio de carga con el conjunto de entrega de aplicaciones de Azure](https://docs.microsoft.com/azure/frontdoor/front-door-lb-with-azure-app-delivery-suite).


## <a name="do-you-support-same-waf-features-in-all-integrated-platforms"></a>¿Se admiten las características de WAF mismas en todas las plataformas integradas?

Actualmente, ModSec CRS 2.2.9 y reglas de CRS 3.0 solo se admiten con WAF en Application Gateway. Limitación de velocidad, filtrado geográfico, Azure administrada predeterminada de conjunto de reglas reglas y solo son compatibles con WAF en el acceso de principal de Azure.

## <a name="is-ddos-protection-integrated-with-front-door"></a>¿Se integra protección contra DDoS con puerta delantera? 

Distribuidas globalmente en los bordes de red de Azure, puerta de Azure puede absorber y geográficamente aislar los ataques de gran volumen. Puede crear directivas personalizadas de WAF para bloquear automáticamente y la tasa de reducir los ataques de HTTP que conocen las firmas. Aún más, puede habilitar la protección contra DDoS estándar en la red virtual donde se implementa su back-end. Los clientes de Azure DDoS Protection Standard reciben ventajas adicionales, como acceso a expertos del equipo de respuesta rápida de DDoS para obtener ayuda inmediata, garantía de SLA y protección de costos durante un ataque. 

## <a name="next-steps"></a>Pasos siguientes

- Obtenga información sobre [firewall de aplicaciones web de Azure](waf-overview.md).
- Obtenga más información sobre [Azure puerta](front-door-overview.md).
