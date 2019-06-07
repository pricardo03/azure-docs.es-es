---
title: Preguntas más frecuentes sobre Azure Application Gateway
description: Encuentre respuestas a preguntas frecuentes sobre Azure Application Gateway.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.workload: infrastructure-services
ms.date: 6/1/2019
ms.author: victorh
ms.openlocfilehash: 25779cd62f70df41a7734d2f8097504923dcb3a3
ms.sourcegitcommit: 087ee51483b7180f9e897431e83f37b08ec890ae
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66430871"
---
# <a name="frequently-asked-questions-about-application-gateway"></a>Preguntas más frecuentes sobre Application Gateway

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Los siguientes son preguntas más frecuentes sobre Azure Application Gateway.

## <a name="general"></a>General

### <a name="what-is-application-gateway"></a>¿Qué es una Puerta de enlace de aplicaciones?

Azure Application Gateway proporciona un controlador de entrega de aplicaciones (ADC) como un servicio. Ofrece diversas funcionalidades de equilibrio de carga de nivel 7 para sus aplicaciones. Este servicio es de alta disponibilidad, escalable y totalmente administrado por Azure.

### <a name="what-features-does-application-gateway-support"></a>¿Qué características admite Application Gateway?

Application Gateway admite el escalado automático, descarga de SSL y SSL de extremo a extremo, un firewall de aplicaciones web (WAF), afinidad de sesión basada en cookies, dirección URL enrutamiento basado en ruta de acceso, hospedaje de múltiples sitios y otras características. Para obtener una lista completa de las características admitidas, consulte [Introducción a Application Gateway](application-gateway-introduction.md).

### <a name="how-do-application-gateway-and-azure-load-balancer-differ"></a>¿Cómo se diferencian los Application Gateway y Azure Load Balancer?

Application Gateway es un equilibrador de carga de nivel 7, lo que significa que solo funciona con el tráfico web (HTTP, HTTPS, HTTP/2 y WebSocket). Admite funcionalidades como la terminación SSL, afinidad de sesión basada en cookies y round robin para el equilibrio de carga de tráfico. Cargar el equilibrador de carga: equilibra el tráfico en el nivel 4 (TCP o UDP).

### <a name="what-protocols-does-application-gateway-support"></a>¿Qué protocolos admite Application Gateway?

Application Gateway admite HTTP, HTTPS, HTTP/2 y WebSocket.

### <a name="how-does-application-gateway-support-http2"></a>¿Admite Application Gateway HTTP/2?

Consulte [compatibilidad con HTTP/2](https://docs.microsoft.com/azure/application-gateway/configuration-overview#http2-support).

### <a name="what-resources-are-supported-as-part-of-a-backend-pool"></a>¿Qué recursos se admiten como parte de un grupo de back-end?

Consulte [admite recursos back-end](https://docs.microsoft.com/azure/application-gateway/application-gateway-components#backend-pool).

### <a name="in-what-regions-is-application-gateway-available"></a>¿En qué regiones está disponible Application Gateway?

Application Gateway está disponible en todas las regiones de Azure global. También está disponible en [Azure China 21Vianet](https://www.azure.cn/) y [Azure Government](https://azure.microsoft.com/overview/clouds/government/).

### <a name="is-this-deployment-dedicated-for-my-subscription-or-is-it-shared-across-customers"></a>¿Es esta implementación dedicada para mi suscripción o se comparte entre los clientes?

Application Gateway es una implementación dedicada en su red virtual.

### <a name="does-application-gateway-support-http-to-https-redirection"></a>¿Admite Application Gateway redirección HTTP a HTTPS?

Se admite el redireccionamiento. Consulte [Introducción a la redirección de Application Gateway](application-gateway-redirect-overview.md).

### <a name="in-what-order-are-listeners-processed"></a>¿En qué orden se procesan los agentes de escucha?

Consulte la [orden de procesamiento del agente de escucha](https://docs.microsoft.com/azure/application-gateway/configuration-overview#order-of-processing-listeners).

### <a name="where-do-i-find-the-application-gateway-ip-and-dns"></a>¿Dónde se puede encontrar la IP de puerta de enlace de aplicaciones y DNS?

Si usa una dirección IP pública como punto de conexión, encontrará la información de dirección IP y DNS en el recurso de dirección IP pública. O búsquelo en el portal, en la página de información general de la puerta de enlace de la aplicación. Si usa direcciones IP internas, localice la información en la página información general.

### <a name="what-are-the-settings-for-keep-alive-timeout-and-tcp-idle-timeout"></a>¿Cuáles son los valores de tiempo de espera de Keep-Alive y tiempo de espera de inactividad de TCP?

 En la SKU de puerta de enlace de aplicación v1, el tiempo de espera de mantenimiento es de 120 segundos. El tiempo de espera de mantenimiento para la SKU v2 es 75 segundos. El tiempo de espera de inactividad de TCP es el valor predeterminado de 4 minutos a la IP virtual de front-end (VIP) de Application Gateway.

### <a name="does-the-ip-or-dns-name-change-over-the-lifetime-of-the-application-gateway"></a>¿Se cambia el nombre DNS o IP durante la vigencia de la puerta de enlace de aplicaciones?

La dirección VIP puede cambiar si se puede detener e iniciar la puerta de enlace de la aplicación. Pero no cambia el nombre DNS asociado con la puerta de enlace de aplicaciones durante la vigencia de la puerta de enlace. Ya que no cambia el nombre DNS, debe usar un alias CNAME y hacer que apunte a la dirección DNS de la puerta de enlace de aplicaciones.

### <a name="does-application-gateway-support-static-ip"></a>¿Admite Application Gateway direcciones IP estáticas?

Sí, la SKU de puerta de enlace de aplicaciones v2 admite direcciones IP públicas estáticas. La SKU v1 es compatible con direcciones IP internas estáticas.

### <a name="does-application-gateway-support-multiple-public-ips-on-the-gateway"></a>¿Admite Application Gateway varias direcciones IP públicas en la puerta de enlace?

Application gateway admite una única dirección IP pública.

### <a name="how-large-should-i-make-my-subnet-for-application-gateway"></a>¿Cómo de grande debe ser la subred para Application Gateway?

Consulte [consideraciones de tamaño de subred de Application Gateway](https://docs.microsoft.com/azure/application-gateway/configuration-overview#size-of-the-subnet).

### <a name="can-i-deploy-more-than-one-application-gateway-resource-to-a-single-subnet"></a>¿Puedo implementar más de un recurso de Application Gateway para una sola subred?

Sí. Además de varias instancias de una determinada implementación de Application Gateway, puede aprovisionar otro recurso de puerta de enlace de aplicación único a una subred existente que contiene un recurso de puerta de enlace de aplicación diferente.

Una sola subred no puede admitir Standard_v2 y puerta de enlace de aplicaciones estándar juntos.

### <a name="does-application-gateway-support-x-forwarded-for-headers"></a>¿Admite Application Gateway encabezados x-forwarded-for?

Sí. Consulte [modificaciones a una solicitud](https://docs.microsoft.com/azure/application-gateway/how-application-gateway-works#modifications-to-the-request).

### <a name="how-long-does-it-take-to-deploy-an-application-gateway-will-my-application-gateway-work-while-its-being-updated"></a>¿Cuánto tarda en implementar una puerta de enlace de aplicaciones? ¿Funcionará mi puerta de enlace de la aplicación mientras se actualiza?

Las nuevas implementaciones de SKU v1 de Application Gateway pueden tardar hasta 20 minutos en aprovisionarse. Los cambios realizados en el tamaño de la instancia o el recuento no son perjudiciales, y la puerta de enlace permanece activa durante este tiempo.

Las implementaciones que usan la SKU v2 pueden tardar hasta 6 minutos en aprovisionarse.

### <a name="can-i-use-exchange-server-as-a-backend-with-application-gateway"></a>¿Puedo usar Exchange Server como un back-end con Application Gateway?

No. Application Gateway no admite protocolos de correo electrónico como SMTP, IMAP y POP3. 

## <a name="performance"></a>Rendimiento

### <a name="how-does-application-gateway-support-high-availability-and-scalability"></a>¿Cómo admite Application Gateway la alta disponibilidad y la escalabilidad?

La SKU de v1 Application Gateway admite escenarios de alta disponibilidad cuando se ha implementado dos o más instancias. Azure distribuye estas instancias entre dominios de error y actualización para asegurarse de que las instancias no todos los errores al mismo tiempo. La SKU v1 admite escalabilidad mediante la incorporación de varias instancias de la misma puerta de enlace para compartir la carga.

La SKU v2 garantiza automáticamente que las nuevas instancias se distribuyan entre dominios de error y dominios de actualización. Si elige la redundancia de zona, las instancias más recientes también se distribuyen entre las zonas de disponibilidad para ofrecer resistencia ante errores zonal.

### <a name="how-do-i-achieve-a-dr-scenario-across-datacenters-by-using-application-gateway"></a>¿Cómo alcanzar un escenario de recuperación ante desastres en centros de datos mediante el uso de Application Gateway?

Use Traffic Manager para distribuir el tráfico entre varias puertas de enlace de aplicaciones en distintos centros de datos.

### <a name="does-application-gateway-support-autoscaling"></a>¿Admite Application Gateway el escalado automático?

Sí, la SKU v2 de Application Gateway admite la escalabilidad automática. Para obtener más información, consulte [el escalado automático y la puerta de enlace de aplicaciones con redundancia de zona](application-gateway-autoscaling-zone-redundant.md).

### <a name="does-manual-scale-up-or-scale-down-cause-downtime"></a>¿No escala manual de o reducir verticalmente el tiempo de inactividad de la causa?

No. Las instancias se distribuyen entre varios dominios de actualización y dominios de error.

### <a name="does-application-gateway-support-connection-draining"></a>¿Es compatible Application Gateway con la funcionalidad de drenaje de conexiones?

Sí. Puede configurar purga para cambiar a los miembros dentro de un grupo de back-end sin interrupciones de la conexión. Esta configuración permite que continúe enviar las conexiones existentes a su destino anterior hasta que esa conexión se cierra o expira un tiempo de espera configurable. Purga de la conexión espera sólo conexiones en curso actuales finalice. Instancia de Application Gateway no tiene en cuenta el estado de sesión de la aplicación.

### <a name="can-i-change-instance-size-from-medium-to-large-without-disruption"></a>¿Puedo cambiar el tamaño de la instancia de mediano a grande sin que haya una interrupción?

Sí. Azure distribuye las instancias entre dominios de error y actualización para garantizar que las instancias no todo al mismo tiempo. Application Gateway admite el escalado mediante la adición de varias instancias de la misma puerta de enlace para compartir la carga.

## <a name="configuration"></a>Configuración

### <a name="is-application-gateway-always-deployed-in-a-virtual-network"></a>¿Se implementa Application Gateway siempre en una red virtual?

Sí. Puerta de enlace de aplicación siempre se implementa en una subred de red virtual. Esta subred puede contener solo puertas de enlace de aplicaciones. Para obtener más información, consulte [requisitos de red y subred virtual](https://docs.microsoft.com/azure/application-gateway/configuration-overview#azure-virtual-network-and-dedicated-subnet).

### <a name="can-application-gateway-communicate-with-instances-outside-of-its-virtual-network-or-outside-of-its-subscription"></a>¿Application Gateway puede comunicarse con instancias fuera de su red virtual o fuera de su suscripción?

Siempre que tenga conectividad IP, Application Gateway puede comunicarse con instancias fuera de la red virtual que se encuentra. Puerta de enlace de aplicación también puede comunicarse con instancias fuera de la suscripción en. Si planea usar direcciones IP internas como miembros del grupo de back-end, use [emparejamiento de redes virtuales](../virtual-network/virtual-network-peering-overview.md) o [Azure VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md).

### <a name="can-i-deploy-anything-else-in-the-application-gateway-subnet"></a>¿Puedo implementar algo más en la subred de la puerta de enlace de aplicaciones?

No. Pero se pueden implementar otras puertas de enlace de aplicaciones en la subred.

### <a name="are-network-security-groups-supported-on-the-application-gateway-subnet"></a>¿Se admiten grupos de seguridad de red en la subred de puerta de enlace de aplicaciones?

Consulte [grupos de seguridad de la subred de puerta de enlace de aplicaciones de red](https://docs.microsoft.com/azure/application-gateway/configuration-overview#network-security-groups-on-the-application-gateway-subnet).

### <a name="does-the-application-gateway-subnet-support-user-defined-routes"></a>¿Es la subred de puerta de enlace de la aplicación es compatible con las rutas definidas por el usuario?

Consulte [rutas definidas por el usuario de la subred de Application Gateway admitidas](https://docs.microsoft.com/azure/application-gateway/configuration-overview#user-defined-routes-supported-on-the-application-gateway-subnet).

### <a name="what-are-the-limits-on-application-gateway-can-i-increase-these-limits"></a>¿Cuáles son los límites de Application Gateway? ¿Puedo aumentar estos límites?

Consulte [límites de Application Gateway](../azure-subscription-service-limits.md#application-gateway-limits).

### <a name="can-i-simultaneously-use-application-gateway-for-both-external-and-internal-traffic"></a>¿Puedo usar simultáneamente Application Gateway para tráfico interno y externo?

Sí. Application Gateway admite una dirección IP interna y una dirección IP externa por cada instancia de application gateway.

### <a name="does-application-gateway-support-virtual-network-peering"></a>¿Admite Application Gateway emparejamiento de redes virtuales?

Sí. Emparejamiento de redes virtuales le ayuda a tráfico de equilibrio de carga en otras redes virtuales.

### <a name="can-i-talk-to-on-premises-servers-when-theyre-connected-by-expressroute-or-vpn-tunnels"></a>¿Se puede comunicar con servidores locales cuando están conectados mediante ExpressRoute o túneles VPN?

Sí, siempre que se permita el tráfico.

### <a name="can-one-backend-pool-serve-many-applications-on-different-ports"></a>¿Puede actuar un grupo de back-end muchas aplicaciones en puertos diferentes?

Se admite la arquitectura de Microservicios. Para realizar sondeos en puertos diferentes, deberá configurar varias opciones de configuración de HTTP.

### <a name="do-custom-probes-support-wildcards-or-regex-on-response-data"></a>¿Admiten los sondeos personalizados caracteres comodín o regex en los datos de respuesta?

No. 

### <a name="how-are-routing-rules-processed-in-application-gateway"></a>¿Cómo se procesan las reglas de enrutamiento en Application Gateway?

Consulte [orden de las reglas de procesamiento](https://docs.microsoft.com/azure/application-gateway/configuration-overview#order-of-processing-rules).

### <a name="for-custom-probes-what-does-the-host-field-signify"></a>Para los sondeos personalizados, ¿qué el campo Host indica?

El campo Host especifica el nombre para enviar el sondeo para que haya configurado multisitio en Application Gateway. En caso contrario, usar '127.0.0.1'. Este valor es diferente del nombre del host de máquina virtual. Su formato es \<protocolo\>://\<host\>:\<puerto\>\<ruta de acceso\>.

### <a name="can-i-allow-application-gateway-access-to-only-a-few-source-ip-addresses"></a>¿Permitir que Application Gateway el acceso únicamente a unos origen direcciones IP?

Sí. Consulte [restringir el acceso a direcciones IP de origen específica](https://docs.microsoft.com/azure/application-gateway/configuration-overview#allow-application-gateway-access-to-a-few-source-ips).

### <a name="can-i-use-the-same-port-for-both-public-facing-and-private-facing-listeners"></a>¿Puedo usar el mismo puerto para acceso público y privado accesible desde los agentes de escucha?

No.

### <a name="is-there-guidance-available-to-migrate-from-the-v1-sku-to-the-v2-sku"></a>¿Hay instrucciones disponibles para migrar de la SKU de v1 a la SKU v2?

Sí. Para obtener información detallada, consulte [migrar Azure Application Gateway y Firewall de aplicaciones Web de v1 a v2](migrate-v1-v2.md).


## <a name="configuration---ssl"></a>Configuración - SSL

### <a name="what-certificates-does-application-gateway-support"></a>¿Qué certificados admite Application Gateway?

Application Gateway admite los certificados autofirmados, certificados de la entidad de certificación (CA), los certificados de validación extendida (EV) y los certificados comodín.

### <a name="what-cipher-suites-does-application-gateway-support"></a>¿Los conjuntos de cifrado Application Gateway admite?

Application Gateway admite los siguientes conjuntos de cifrado. 

- TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384
- TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256
- TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384
- TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256
- TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA
- TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA
- TLS_DHE_RSA_WITH_AES_256_GCM_SHA384
- TLS_DHE_RSA_WITH_AES_128_GCM_SHA256
- TLS_DHE_RSA_WITH_AES_256_CBC_SHA
- TLS_DHE_RSA_WITH_AES_128_CBC_SHA
- TLS_RSA_WITH_AES_256_GCM_SHA384
- TLS_RSA_WITH_AES_128_GCM_SHA256
- TLS_RSA_WITH_AES_256_CBC_SHA256
- TLS_RSA_WITH_AES_128_CBC_SHA256
- TLS_RSA_WITH_AES_256_CBC_SHA
- TLS_RSA_WITH_AES_128_CBC_SHA
- TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384
- TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256
- TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384
- TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256
- TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA
- TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA
- TLS_DHE_DSS_WITH_AES_256_CBC_SHA256
- TLS_DHE_DSS_WITH_AES_128_CBC_SHA256
- TLS_DHE_DSS_WITH_AES_256_CBC_SHA
- TLS_DHE_DSS_WITH_AES_128_CBC_SHA
- TLS_RSA_WITH_3DES_EDE_CBC_SHA
- TLS_DHE_DSS_WITH_3DES_EDE_CBC_SHA

Para obtener información sobre cómo personalizar las opciones de SSL, consulte [versiones de directivas de configuración de SSL y conjuntos de cifrado en Application Gateway](application-gateway-configure-ssl-policy-powershell.md).

### <a name="does-application-gateway-support-reencryption-of-traffic-to-the-backend"></a>¿Admite Application Gateway nuevo cifrado de tráfico para el back-end?

Sí. Application Gateway admite la descarga SSL y SSL de extremo a otro, que cifrar de nuevo tráfico back-end.

### <a name="can-i-configure-ssl-policy-to-control-ssl-protocol-versions"></a>¿Puedo configurar la directiva SSL para el control versiones del protocolo SSL?

Sí. Puede configurar la puerta de enlace de aplicaciones para denegar TLS1.0, TLS1.1 y TLS1.2. De forma predeterminada, SSL 2.0 y 3.0 ya están deshabilitados y no son configurables.

### <a name="can-i-configure-cipher-suites-and-policy-order"></a>¿Puedo configurar los conjuntos de cifrado y el orden de las directivas?

Sí. En la puerta de enlace de aplicaciones, puede [configurar conjuntos de cifrado](application-gateway-ssl-policy-overview.md). Para definir una directiva personalizada, habilite al menos uno de los siguientes conjuntos de cifrado. 

* TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 
* TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256
* TLS_DHE_RSA_WITH_AES_128_GCM_SHA256
* TLS_RSA_WITH_AES_128_GCM_SHA256
* TLS_RSA_WITH_AES_256_CBC_SHA256
* TLS_RSA_WITH_AES_128_CBC_SHA256

Application Gateway usa SHA256 a para la administración de back-end.

### <a name="how-many-ssl-certificates-does-application-gateway-support"></a>¿Cuántos certificados SSL admite Application Gateway?

Application Gateway admite hasta 100 certificados SSL.

### <a name="how-many-authentication-certificates-for-backend-reencryption-does-application-gateway-support"></a>¿Cuántos certificados de autenticación para el back-end de nuevo cifrado se admite Application Gateway?

Application Gateway admite hasta 10 certificados de autenticación. El valor predeterminado es 5.

### <a name="does-application-gateway-natively-integrate-with-azure-key-vault"></a>¿Application Gateway de forma nativa se integra con Azure Key Vault?

Sí, la SKU de puerta de enlace de aplicaciones v2 es compatible con Key Vault. Para obtener más información, consulte [terminación SSL con certificados de Key Vault](key-vault-certs.md).

### <a name="how-do-i-configure-https-listeners-for-com-and-net-sites"></a>¿Cómo se puede configurar los agentes de escucha HTTPS para sitios .com y. NET? 

Para varios basados en dominio (basado en host) enrutamiento, puede crear agentes de escucha multisitios, configurar los agentes de escucha que usan HTTPS como el protocolo y asociar los agentes de escucha a las reglas de enrutamiento. Para obtener más información, consulte [hospedar varios sitios mediante el uso de Application Gateway](https://docs.microsoft.com/azure/application-gateway/multiple-site-overview). 

## <a name="configuration---web-application-firewall-waf"></a>-Configuración de firewall de aplicaciones web (WAF)

### <a name="does-the-waf-sku-offer-all-the-features-available-in-the-standard-sku"></a>¿Ofrece la SKU de WAF todas las características disponibles en la SKU estándar?

Sí. WAF admite todas las características de la SKU estándar.

### <a name="which-crs-versions-does-application-gateway-support"></a>¿Qué versiones CRS admite Application Gateway?

Application Gateway admite CRS [2.2.9](application-gateway-crs-rulegroups-rules.md#owasp229) y CRS [3.0](application-gateway-crs-rulegroups-rules.md#owasp30).

### <a name="how-do-i-monitor-waf"></a>¿Cómo se supervisa el WAF?

WAF de monitor a través del registro de diagnóstico. Para obtener más información, consulte [registro de diagnóstico y métricas de Application Gateway](application-gateway-diagnostics.md).

### <a name="does-detection-mode-block-traffic"></a>¿Bloquea el modo de detección el tráfico?

No. Modo de detección solo registra el tráfico que se desencadena una regla de WAF.

### <a name="can-i-customize-waf-rules"></a>¿Se pueden personalizan las reglas de WAF?

Sí. Para obtener más información, consulte [reglas y grupos de reglas de WAF personalizar](application-gateway-customize-waf-rules-portal.md).

### <a name="what-rules-are-currently-available-for-waf"></a>¿Qué reglas están disponibles actualmente para WAF?

WAF actualmente admite CRS [2.2.9](application-gateway-crs-rulegroups-rules.md#owasp229) y [3.0](application-gateway-crs-rulegroups-rules.md#owasp30). Estas reglas proporcionan seguridad de la línea de base frente a la mayoría de las vulnerabilidades de las 10 identifica Open Web Application Security Project (OWASP): 

* Protección contra la inyección de código SQL
* Protección de scripting entre sitios
* Protección contra ataques web comunes, como inyección de comandos, contrabando de división de respuestas HTTP de solicitudes HTTP y ataque inclusión de archivos remoto
* Protección contra infracciones del protocolo HTTP
* Protección contra anomalías del protocolo HTTP, como la falta de agentes de usuario de host y encabezados de aceptación
* Prevención contra bots, rastreadores y escáneres
* Detección de errores comunes (es decir, Apache, IIS etc.)

Para obtener más información, consulte [vulnerabilidades OWASP top 10](https://www.owasp.org/index.php/Top10#OWASP_Top_10_for_2013).

### <a name="does-waf-support-ddos-protection"></a>¿Admite WAF la protección contra DDoS?

Sí. Puede habilitar la protección contra DDoS en la red virtual donde se implementa la puerta de enlace de la aplicación. Esta configuración garantiza que el servicio Azure DDoS Protection protege también la IP virtual (VIP) puerta de enlace de aplicaciones.

### <a name="is-there-guidance-available-to-migrate-from-the-v1-sku-to-the-v2-sku"></a>¿Hay instrucciones disponibles para migrar de la SKU de v1 a la SKU v2?

Sí. Para obtener información detallada, consulte [migrar Azure Application Gateway y Firewall de aplicaciones Web de v1 a v2](migrate-v1-v2.md).

## <a name="diagnostics-and-logging"></a>Diagnósticos y registro

### <a name="what-types-of-logs-does-application-gateway-provide"></a>¿Qué tipos de registros proporciona Application Gateway?

Application Gateway proporciona tres registros: 

* **ApplicationGatewayAccessLog**: El registro de acceso contiene todas las solicitudes enviadas al front-end de puerta de enlace de aplicaciones. Los datos incluyen el llamador IP, dirección URL solicitada, latencia de la respuesta, código de retorno y bytes de entrada y salida. El registro de acceso se recopila cada 300 segundos. Contiene un registro por cada instancia de application gateway.
* **ApplicationGatewayPerformanceLog**: El registro de rendimiento captura información de rendimiento para cada puerta de enlace de aplicaciones. La información incluye la capacidad de proceso en bytes, el número total de solicitudes atendidas, el recuento de solicitudes con error y el recuento de instancias de back-end positiva y negativa.
* **ApplicationGatewayFirewallLog**: Para las puertas de enlace de aplicaciones que se configuración con WAF, el registro de firewall contiene las solicitudes que se registran con el modo de detección o modo de prevención.

Para obtener más información, consulte [back-end de estado, los registros de diagnóstico y métricas de Application Gateway](application-gateway-diagnostics.md).

### <a name="how-do-i-know-if-my-backend-pool-members-are-healthy"></a>¿Cómo se puede saber si mis miembros del grupo de back-end están en buenas condiciones?

Comprobar el estado mediante el cmdlet de PowerShell `Get-AzApplicationGatewayBackendHealth` o el portal. Para más información, consulte [Diagnósticos de Application Gateway](application-gateway-diagnostics.md).

### <a name="whats-the-retention-policy-for-the-diagnostic-logs"></a>¿Qué es la directiva de retención para los registros de diagnóstico?

Flujo de registros de diagnóstico para la cuenta de almacenamiento del cliente. Los clientes pueden establecer la directiva de retención según sus preferencias. También se pueden enviar los registros de diagnóstico en un centro de eventos o registros de Azure Monitor. Para más información, consulte [Diagnósticos de Application Gateway](application-gateway-diagnostics.md).

### <a name="how-do-i-get-audit-logs-for-application-gateway"></a>¿Cómo se pueden obtener los registros de auditoría de Application Gateway?

En el portal, en la hoja del menú de una puerta de enlace de aplicaciones, seleccione **registro de actividad** para tener acceso al registro de auditoría. 

### <a name="can-i-set-alerts-with-application-gateway"></a>¿Se pueden establecer alertas con Application Gateway?

Sí. En Application Gateway, se configuran las alertas en métricas. Para obtener más información, consulte [métricas de Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics#metrics) y [recibir notificaciones de alerta](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).

### <a name="how-do-i-analyze-traffic-statistics-for-application-gateway"></a>¿Cómo se pueden analizar las estadísticas de tráfico de Application Gateway?

Puede ver y analizar los registros de acceso de varias maneras. Use los registros de Azure Monitor, Excel, Power BI y así sucesivamente.

También puede usar una plantilla de Resource Manager que se instala y ejecuta el popular [GoAccess](https://goaccess.io/) iniciar el analizador de registros de acceso de Application Gateway. GoAccess proporciona las estadísticas de tráfico HTTP valiosas como visitantes únicos, los archivos solicitados, hosts, sistemas operativos, exploradores y códigos de estado HTTP. Para obtener más información, en GitHub, consulte el [archivo Léame en la carpeta de plantillas de Resource Manager](https://aka.ms/appgwgoaccessreadme).

### <a name="what-could-cause-backend-health-to-return-an-unknown-status"></a>¿Lo que podría provocar el mantenimiento de back-end devolver un estado desconocido?

Por lo general, verá un estado desconocido cuando acceso para el back-end está bloqueado por un grupo de seguridad de red (NSG), DNS personalizado o enrutamiento definido por el usuario (UDR) en la subred de puerta de enlace de aplicaciones. Para obtener más información, consulte [back-end de estado, registro de diagnóstico y métricas de Application Gateway](application-gateway-diagnostics.md).

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información acerca de Application Gateway, consulte [¿qué es Azure Application Gateway?](overview.md).
