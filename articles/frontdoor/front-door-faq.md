---
title: 'Servicio de Azure puerta delantera: puerta de preguntas más frecuentes | Microsoft Docs'
description: Esta página proporciona respuestas a las preguntas más frecuentes sobre el servicio de puerta de entrada de Azure
services: frontdoor
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/08/2019
ms.author: sharadag
ms.openlocfilehash: 256435dfd016ebbd86dbbe49f4abbb346fb1cd19
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60736673"
---
# <a name="frequently-asked-questions-for-azure-front-door-service"></a>Preguntas más frecuentes para el servicio de puerta de entrada de Azure

En este artículo se responde preguntas comunes sobre la funcionalidad y las características del servicio de puerta de entrada de Azure. Si no encuentra una respuesta a su pregunta, póngase en contacto con nosotros mediante los siguientes canales (en orden incremental):

1. La sección Comentarios de este artículo.
2. [UserVoice de servicio de Azure puerta delantera](https://feedback.azure.com/forums/217313-networking?category_id=345025).
3. **Soporte técnico de Microsoft:** Para crear una solicitud de soporte técnico, en Azure Portal, vaya a la pestaña **Ayuda**, seleccione el botón **Ayuda y soporte técnico** y elija **Nueva solicitud de soporte técnico**.

## <a name="general"></a>General

### <a name="what-is-azure-front-door-service"></a>¿Qué es Azure Front Door Service?

Servicio de puerta de Azure es una red de entrega de aplicaciones (ADN) como un servicio, que ofrece diversas funcionalidades de equilibrio de carga de nivel 7 para sus aplicaciones. Proporciona la aceleración de sitios dinámicos (DSA), junto con equilibrio de carga global con casi en tiempo real de conmutación por error. Es un servicio altamente disponible y escalable, que es completamente administrado por Azure.

### <a name="what-features-does-azure-front-door-service-support"></a>¿Qué características admite Azure puerta delantera Service?

Servicio de puerta de Azure es compatible con la aceleración de sitios dinámicos (DSA), la descarga de SSL y SSL de extremo a otro, Firewall de aplicaciones Web, afinidad de sesión basada en cookies, enrutamiento basado en ruta de acceso url, gratuita certificados y administración de dominios múltiples y otros. Para obtener una lista completa de las características admitidas, consulte [información general de Azure Service puerta delantera](front-door-overview.md).

### <a name="what-is-the-difference-between-azure-front-door-service-and-azure-application-gateway"></a>¿Cuál es la diferencia entre el servicio de puerta de entrada de Azure y Azure Application Gateway?

Mientras la puerta delantera y Application Gateway son de capa 7 equilibradores de carga (HTTP/HTTPS), la principal diferencia es que la puerta delantera es un servicio global, mientras que Application Gateway es un servicio regional. Mientras la puerta delantera puede equilibrar la carga entre las unidades de unidades/clústeres/marca de escalado diferente entre regiones, Application Gateway le permite equilibrar la carga entre los etc. máquinas virtuales y contenedores que se encuentra dentro de la unidad de escalado.

### <a name="when-should-we-deploy-an-application-gateway-behind-front-door"></a>¿Debemos cuando se implementa una puerta de enlace de aplicaciones detrás de la puerta de entrada?

Los escenarios claves, ¿por qué uno debe usar la puerta de enlace de aplicaciones detrás de la puerta de entrada son:

- Puerta de entrada puede realizar solo en el nivel global, pero si uno desea la carga del tráfico de equilibrio aún dentro de su red virtual (VNET), a continuación, debe usar la puerta de enlace de aplicaciones de equilibrio de carga basada en ruta de acceso.
- Puesto que la puerta de entrada no funciona en un nivel de máquina virtual o el contenedor, por lo que no puede hacer drenaje de conexiones. Sin embargo, Application Gateway le permite hacer drenaje de conexiones. 
- Con una puerta de enlace de aplicaciones detrás de AFD, uno puede lograr el 100% la descarga de SSL y enrutar solicitudes HTTP solo dentro de su red virtual (VNET).
- Puerta delantera y Application Gateway admiten la afinidad de sesión. Mientras la puerta delantera puede dirigir el tráfico posterior desde una sesión de usuario en el mismo clúster o back-end en una región determinada, Application Gateway direct pueden establecer afinidad con el tráfico en el mismo servidor en el clúster.  

### <a name="can-we-deploy-azure-load-balancer-behind-front-door"></a>¿Podemos implementar equilibrador de carga de Azure detrás de la puerta de entrada?

Servicio de puerta de Azure necesita una VIP pública o un nombre DNS disponible públicamente para enrutar el tráfico. Implementar un equilibrador de carga de Azure detrás de la puerta delantera es el caso de uso común.

### <a name="what-protocols-does-azure-front-door-service-support"></a>¿Qué protocolos admite Azure puerta delantera Service?

Servicio de puerta de Azure es compatible con HTTP, HTTPS y HTTP/2.

### <a name="how-does-azure-front-door-service-support-http2"></a>¿Cómo admite HTTP/2 servicio de puerta de entrada de Azure?

Compatibilidad con el protocolo HTTP/2 está disponible para los clientes se conectan al servicio de puerta de entrada de Azure solo. La comunicación con los backends en el grupo de back-end es a través de HTTP/1.1. Compatibilidad con HTTP/2 está habilitado de forma predeterminada.

### <a name="what-resources-are-supported-today-as-part-of-backend-pool"></a>¿Qué recursos son compatibles actualmente como parte del grupo de back-end?

Los grupos de back-end se pueden componer de almacenamiento, aplicaciones Web, las instancias de Kubernetes o cualquier otro nombre de host personalizado que tenga conectividad pública. Servicio de puerta de Azure requiere que el back-ends se definen a través de una dirección IP pública o un nombre de host DNS que pueda resolverse públicamente. Los miembros de grupos de back-end pueden ser en distintas zonas, regiones, o incluso fuera de Azure, siempre tengan conectividad pública.

### <a name="what-regions-is-the-service-available-in"></a>¿En qué regiones está disponible el servicio?

Servicio de puerta de Azure es un servicio global y no está asociado a cualquier región de Azure específica. La única ubicación, que debe especificar al crear una puerta de entrada es la ubicación del grupo de recursos, que es básicamente que especifica donde se almacenarán los metadatos para el grupo de recursos. Se crea el propio recurso de puerta principal como un recurso global y la configuración se implementará globalmente a todos los puntos de presencia (punto de presencia). 

### <a name="what-are-the-pop-locations-for-azure-front-door-service"></a>¿Cuáles son las ubicaciones POP para el servicio de puerta de entrada de Azure?

Servicio de puerta de Azure tiene la misma lista de ubicaciones de POP (punto de presencia) que Azure CDN de Microsoft. Para obtener una lista completa de nuestros puntos de presencia, consulte póngase [ubicaciones POP de Azure CDN de Microsoft](https://docs.microsoft.com/azure/cdn/cdn-pop-locations).

### <a name="is-azure-front-door-service-a-dedicated-deployment-for-my-application-or-is-it-shared-across-customers"></a>¿Es una implementación dedicada para mi aplicación de servicio de puerta de entrada de Azure o se comparte entre los clientes?

Servicio de puerta de Azure es un servicio multiempresa distribuido globalmente. Por lo tanto, la infraestructura para la puerta de entrada se comparte entre todos sus clientes. Sin embargo, definir la configuración específica necesaria para la aplicación mediante la creación de una puerta de entrada y 

### <a name="is-http-https-redirection-supported"></a>¿Se admite el redireccionamiento HTTP->HTTPS?

Puerta de entrada actualmente no admite la redirección de URL.

### <a name="in-what-order-are-routing-rules-processed"></a>¿En qué orden se procesan las reglas de enrutamiento?

No se ordenan las rutas para la puerta de entrada y se selecciona una ruta específica en función de la mejor coincidencia. Obtenga más información sobre [cómo puerta de entrada coincide con las solicitudes a una regla de enrutamiento](front-door-route-matching.md).

### <a name="how-do-i-lock-down-the-access-to-my-backend-to-only-azure-front-door-service"></a>¿Cómo bloquear el acceso a mi back-end al servicio de puerta de entrada de Azure solo?

Puede configurar la creación de ACL de IP para los back-end para que acepte tráfico solo desde el servicio de puerta de entrada de Azure. Puede restringir la aplicación acepte las conexiones entrantes sólo desde el espacio IP de back-end del servicio de puerta de entrada de Azure. Estamos trabajando para la integración con [intervalos de IP de Azure y las etiquetas de servicio](https://www.microsoft.com/download/details.aspx?id=56519) pero por ahora puede consultar los intervalos de IP como sigue:
 
- **IPv4** - `147.243.0.0/16`
- **IPv6** - `2a01:111:2050::/44`

> [!WARNING]
> Nuestro espacio IP de back-end puede cambiar más adelante, sin embargo, se garantizará que antes de que suceda, que se habría integrado con [intervalos de direcciones IP de Azure y las etiquetas de servicio](https://www.microsoft.com/download/details.aspx?id=56519). Se recomienda que se suscribe a [intervalos de direcciones IP de Azure y las etiquetas de servicio](https://www.microsoft.com/download/details.aspx?id=56519) para los cambios o actualizaciones. 

### <a name="can-the-anycast-ip-change-over-the-lifetime-of-my-front-door"></a>¿Se puede cambiar la dirección IP de difusión por proximidad durante la vigencia de mi casa?

La dirección IP de difusión por proximidad de front-end para la puerta de entrada normalmente no debería cambiar y es posible que permanezca estática durante la vigencia de la puerta. Sin embargo, hay **ninguna garantía** para el mismo. Póngase no tienen ninguna dependencia directa en la dirección IP.  

### <a name="does-azure-front-door-service-support-static-or-dedicated-ips"></a>¿Es servicio de puerta de entrada de Azure es compatible con las direcciones IP estática o dedicada?

No, servicio de puerta de entrada de Azure actualmente no admite anycast direcciones IP de front-end dedicado o estático. 

### <a name="does-azure-front-door-service-support-x-forwarded-for-headers"></a>¿Azure puerta delantera Service admite encabezados x-forwarded-for?

Sí, el servicio de puerta de entrada de Azure admite los encabezados X-Forwarded-For, Host reenviados X y X-Forwarded-Proto. A continuación, para X-Forwarded-For if ya estaba presente el encabezado puerta delantera anexa la dirección IP de socket de cliente a ella. En caso contrario, agrega el encabezado con la dirección IP de socket de cliente como el valor. Para Host reenviados X y X-Forwarded-Proto, se invalida el valor.

Obtenga más información sobre la [puerta delantera admiten encabezados HTTP](front-door-http-headers-protocol.md).  

### <a name="how-long-does-it-take-to-deploy-an-azure-front-door-service-does-my-front-door-still-work-when-being-updated"></a>¿Cuánto tarda para implementar un servicio de puerta de entrada de Azure? ¿Mi puerta delantera todavía funciona cuando se actualiza?

Creación de un nuevo puerta delantera o las actualizaciones de una puerta de entrada existente tarda aproximadamente 3 a 5 minutos para la implementación global. En aproximadamente 3 a 5 minutos, que significa la configuración de puerta de entrada se implementarán en todos nuestros puntos de presencia global.

Nota: personalizar las actualizaciones de certificados SSL de unos 30 minutos para que se implementará globalmente.

## <a name="configuration"></a>Configuración

### <a name="can-azure-front-door-load-balance-or-route-traffic-within-a-virtual-network"></a>¿Ruta o equilibrio de carga de la puerta delantera Azure el tráfico dentro de una red virtual?

Azure puerta delantera (AFD) requiere una dirección IP pública o el nombre DNS que pueda resolverse públicamente para enrutar el tráfico. Por lo tanto, la respuesta es no AFD no se puede enrutar directamente dentro de una red virtual, pero uso un equilibrador de carga de Azure o Application Gateway entre resolverá este escenario.

### <a name="what-are-the-various-timeouts-and-limits-for-azure-front-door-service"></a>¿Cuáles son los distintos tiempos de espera y los límites de servicio de puerta de entrada de Azure?

Obtener información sobre todos los documentados [tiempos de espera y los límites de servicio de puerta de entrada de Azure](https://docs.microsoft.com/azure/azure-subscription-service-limits#azure-front-door-service-limits).

## <a name="performance"></a>Rendimiento

### <a name="how-does-azure-front-door-service-support-high-availability-and-scalability"></a>¿Cómo admite Azure puerta delantera Service alta disponibilidad y escalabilidad?

Servicio de puerta de Azure es una plataforma distribuida globalmente de varios inquilinos con grandes volúmenes de capacidad a fin de satisfacer las necesidades de escalabilidad de la aplicación. Entregar desde el borde de la red global de Microsoft, puerta delantera proporciona funcionalidad de equilibrio de carga global que permite la conmutación por error la aplicación entera o microservicios incluso individuales a través de nubes diferentes o regiones.

## <a name="ssl-configuration"></a>Configuración de SSL

### <a name="what-tls-versions-are-supported-by-azure-front-door-service"></a>¿Qué versiones TLS son compatibles con el servicio de puerta de entrada de Azure?

Puerta delantera es compatible con las versiones 1.0, 1.1 y 1.2 de TLS. Aún no se admite TLS 1.3.

### <a name="what-certificates-are-supported-on-azure-front-door-service"></a>¿Qué certificados son compatibles con el servicio de puerta de entrada de Azure?

Para habilitar el protocolo HTTPS para entregar contenido en un dominio personalizado de la puerta de entrada de forma segura, puede elegir usar un certificado que está administrado por servicio de puerta de entrada de Azure o su propio certificado.
La puerta delantera administrados disposiciones de opción de un certificado SSL estándar a través de Digicert y almacenados en la parte delantera de Key Vault de puerta. Si decide usar su propio certificado, a continuación se puede incorporar un certificado de una entidad de certificación compatible y puede ser un SSL estándar, el certificado de validación extendida o incluso un certificado comodín. No se admiten certificados autofirmados. Obtenga información sobre [cómo habilitar HTTPS para un dominio personalizado](https://aka.ms/FrontDoorCustomDomainHTTPS).

### <a name="what-are-the-current-cipher-suites-supported-by-azure-front-door-service"></a>¿Cuáles son los conjuntos de cifrado actuales admitidos por el servicio de puerta de entrada de Azure?

Estos son los conjuntos de cifrado actuales admitidos por el servicio de puerta de entrada de Azure:

- TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256
- TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384
- TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256
- TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384
- TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256
- TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384
- TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256
- TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384
- TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA
- TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA
- TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA
- TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA
- TLS_RSA_WITH_AES_256_GCM_SHA384
- TLS_RSA_WITH_AES_128_GCM_SHA256
- TLS_RSA_WITH_AES_256_CBC_SHA256
- TLS_RSA_WITH_AES_128_CBC_SHA256
- TLS_RSA_WITH_AES_256_CBC_SHA
- TLS_RSA_WITH_AES_128_CBC_SHA

### <a name="does-azure-front-door-service-also-support-re-encryption-of-traffic-to-the-backend"></a>¿Servicio de puerta de entrada de Azure también admite el recifrado del tráfico para el back-end?

Sí, servicio de puerta de entrada de Azure es compatible con la descarga de SSL y SSL de extremo a extremo, que vuelve a cifra el tráfico de back-end. De hecho, puesto que realizará las conexiones con el back-end a través de es la dirección IP pública, se recomienda que configure la puerta de entrada para que use HTTPS como protocolo de reenvío.

### <a name="can-i-configure-ssl-policy-to-control-ssl-protocol-versions"></a>¿Se puede configurar la directiva SSL para controlar las versiones del protocolo SSL?

No, actualmente no admite la puerta delantera para denegar a versiones específicas de TLS no puede establecer las versiones TLS mínima. 

### <a name="can-i-configure-front-door-to-only-support-specific-cipher-suites"></a>¿Puedo configurar la puerta delantera para admitir únicamente conjuntos de cifrado específico?

No, no se admite la configuración de puerta de entrada para conjuntos de cifrado específico. 

## <a name="diagnostics-and-logging"></a>Diagnósticos y registro

### <a name="what-types-of-metrics-and-logs-are-available-with-azure-front-door-service"></a>¿Qué tipos de registros y métricas están disponibles con el servicio de puerta de entrada de Azure?

Para obtener información sobre los registros y otras funcionalidades de diagnóstico, consulte [métricas y registros de supervisión para la puerta delantera](front-door-diagnostics.md).

### <a name="what-is-the-retention-policy-on-the-diagnostics-logs"></a>¿Qué es la directiva de retención en los registros de diagnóstico?

Los registros de diagnóstico fluyen hacia la cuenta de almacenamiento de los clientes y estos pueden establecer la directiva de retención según sus preferencias. Los registros de diagnóstico también se pueden enviar a un centro de eventos o a registros de Azure Monitor. Para obtener más información, consulte [diagnósticos de servicio de Azure puerta delantera](front-door-diagnostics.md).

### <a name="how-do-i-get-audit-logs-for-azure-front-door-service"></a>¿Cómo se puede obtener los registros de auditoría para el servicio de puerta de entrada de Azure?

Los registros de auditoría están disponibles para el servicio de puerta de entrada de Azure. En el portal, haga clic en **registro de actividad** en la hoja del menú de la puerta de entrada para obtener acceso al registro de auditoría. 

### <a name="can-i-set-alerts-with-azure-front-door-service"></a>¿Puedo establecer alertas con el servicio de puerta de entrada de Azure?

Sí, servicio de puerta de entrada de Azure es compatible con las alertas. Las alertas se configuran en métricas. 

## <a name="next-steps"></a>Pasos siguientes

- Aprenda a [crear una instancia de Front Door](quickstart-create-front-door.md).
- Más información acerca de cómo [funciona Front Door](front-door-routing-architecture.md).