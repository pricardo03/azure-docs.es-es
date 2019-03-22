---
title: Preguntas frecuentes sobre Azure Application Gateway
description: Esta página proporciona respuestas a las preguntas más frecuentes acerca de Azure Application Gateway
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.workload: infrastructure-services
ms.date: 3/20/2019
ms.author: victorh
ms.openlocfilehash: ae55f2abf9815174e7258c2ace949078794c380d
ms.sourcegitcommit: 8a59b051b283a72765e7d9ac9dd0586f37018d30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2019
ms.locfileid: "58286200"
---
# <a name="frequently-asked-questions-for-application-gateway"></a>Preguntas más frecuentes sobre Application Gateway

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="general"></a>General

### <a name="what-is-application-gateway"></a>¿Qué es una Puerta de enlace de aplicaciones?

Azure Application Gateway es un controlador de entrega de aplicaciones (ADC) como servicio que ofrece diversas funcionalidades de equilibrio de carga de capa 7 para sus aplicaciones. Ofrece un servicio altamente disponible y escalable que está completamente administrado por Azure.

### <a name="what-features-does-application-gateway-support"></a>¿Qué características admite Application Gateway?

Application Gateway admite escalabilidad automática, descarga de SSL y SSL de un extremo a otro, firewall de aplicaciones web, afinidad de sesión basada en cookies, enrutamiento basado en ruta de dirección URL, hospedaje de varios sitios y muchas otras más. Para obtener una lista completa de las características admitidas, consulte [Introducción a Application Gateway](application-gateway-introduction.md).

### <a name="what-is-the-difference-between-application-gateway-and-azure-load-balancer"></a>¿Cuál es la diferencia entre Application Gateway y Azure Load Balancer?

Application Gateway es un equilibrador de carga de nivel 7, lo que significa que funciona únicamente con tráfico web (HTTP/HTTPS/WebSocket/HTTP/2). Admite funcionalidades como la terminación SSL, la afinidad de sesión basada en cookies y la distribución round robin para el tráfico de equilibrio de carga. Load Balancer equilibra la carga de tráfico en la capa 4 (TCP/UDP).

### <a name="what-protocols-does-application-gateway-support"></a>¿Qué protocolos admite Application Gateway?

Application Gateway admite HTTP, HTTPS, HTTP/2 y WebSocket.

### <a name="how-does-application-gateway-support-http2"></a>¿Admite Application Gateway HTTP/2?

Consulte [compatibilidad con HTTP/2](https://docs.microsoft.com/azure/application-gateway/configuration-overview#http2-support) para obtener información sobre cómo Application gateway admite el protocolo HTTP/2.

### <a name="what-resources-are-supported-today-as-part-of-backend-pool"></a>¿Qué recursos son compatibles actualmente como parte del grupo de back-end?

Consulte [admite recursos back-end](https://docs.microsoft.com/azure/application-gateway/application-gateway-components#backend-pool) para obtener información sobre los recursos que son compatibles con la puerta de enlace de aplicaciones.

### <a name="what-regions-is-the-service-available-in"></a>¿En qué regiones está disponible el servicio?

Application Gateway está disponible en todas las regiones de Azure global. También está disponible en [Azure China 21Vianet](https://www.azure.cn/) y [Azure Government](https://azure.microsoft.com/overview/clouds/government/)

### <a name="is-this-a-dedicated-deployment-for-my-subscription-or-is-it-shared-across-customers"></a>¿Se trata de una implementación dedicada para mi suscripción o compartida entre los clientes?

Application Gateway es una implementación dedicada en su red virtual.

### <a name="is-http-https-redirection-supported"></a>¿Se admite el redireccionamiento HTTP->HTTPS?

Se admite el redireccionamiento. Consulte [Introducción al redireccionamiento de Application Gateway](application-gateway-redirect-overview.md) para más información.

### <a name="in-what-order-are-listeners-processed"></a>¿En qué orden se procesan los agentes de escucha?

Los agentes de escucha se procesan en el orden en que aparecen. Por este motivo, si un agente de escucha coincide con una solicitud entrante, se procesa primero.  Los agentes de escucha de varios sitios deben configurarse antes de un agente de escucha básico para asegurarse de que el tráfico se enrute al back-end correcto.

### <a name="where-do-i-find-application-gateways-ip-and-dns"></a>¿Dónde se encuentra la dirección IP y el DNS de Application Gateway?

Cuando se usa una dirección IP pública como punto de conexión, esta información se puede encontrar en el recurso de la dirección IP pública o en la página de información general de la puerta de enlace de aplicaciones en el portal. En el caso de direcciones IP internas, esta se puede encontrar en la página de información general.

### <a name="what-is-keep-alive-timeout-and-tcp-idle-timeout-setting-on-application-gateway"></a>¿Qué es el tiempo de espera de mantenimiento y el valor de tiempo de espera de inactividad de TCP en Application Gateway?

Tiempo de espera de Keep-Alive en SKU v1 es 120 s. tiempo de espera Keep-Alive en SKU v2 es tiempo de espera de inactividad de TCP s. 75 valor predeterminado de 4 minutos en el front-end VIP de Application Gateway.

### <a name="does-the-ip-or-dns-name-change-over-the-lifetime-of-the-application-gateway"></a>¿Cambia la dirección IP o el nombre DNS durante la vigencia de Application Gateway?

La dirección IP virtual puede cambiar si la puerta de enlace de aplicaciones se detiene y se inicia. El nombre DNS asociado con la puerta de enlace de aplicaciones no cambia durante el ciclo de vida de la puerta de enlace. Por este motivo, se recomienda usar un alias CNAME y hacer que señale a la dirección DNS de la puerta de aplicaciones.

### <a name="does-application-gateway-support-static-ip"></a>¿Admite Application Gateway direcciones IP estáticas?

Sí, la SKU v2 de Application Gateway es compatible con direcciones IP públicas estáticas. La SKU v1 es compatible con direcciones IP internas estáticas.

### <a name="does-application-gateway-support-multiple-public-ips-on-the-gateway"></a>¿Admite Application Gateway varias direcciones IP públicas en la puerta de enlace?

Solo se admite una dirección IP pública en una puerta de enlace de aplicaciones.

### <a name="how-large-should-i-make-my-subnet-for-application-gateway"></a>¿Cómo de grande debe ser la subred para Application Gateway?

Application Gateway consume una dirección IP privada por instancia, más otra dirección IP privada si se establece una configuración de dirección IP de front-end privada. Además, Azure reserva las cuatro primeras direcciones IP y la última de cada subred para uso interno.
Por ejemplo, si una puerta de enlace de aplicaciones está establecida en tres instancias y ninguna dirección IP de front-end privada, se necesita un tamaño de subred /29 o mayor. En este caso, la puerta de enlace de aplicaciones usa tres direcciones IP. Si tiene tres instancias y una dirección IP para la configuración de dirección IP de front-end privada, se necesitará un tamaño de subred de /28 o mayor dado que hacen falta cuatro direcciones IP.

Como práctica recomendada, utilice al menos una de/28 tamaño de la subred. Esto le ofrece 11 direcciones utilizables. Si la carga de la aplicación requiere más de 10 instancias, debe considerar un/27 o/26 tamaño de la subred.

### <a name="q-can-i-deploy-more-than-one-application-gateway-resource-to-a-single-subnet"></a>P: ¿Puedo implementar más de un recurso de Application Gateway para una sola subred?

Sí, además de tener varias instancias de una determinada implementación de Application Gateway, puede aprovisionar otro recurso de Application Gateway único para una subred existente que contenga un recurso de Application Gateway diferente.

No se admite la combinación de Application Gateway Standard y Standard_v2 en la misma subred. Además, si está habilitado el escalado automático, una subred puede tener solo una puerta de enlace de aplicaciones.

### <a name="does-application-gateway-support-x-forwarded-for-headers"></a>¿Admite Application Gateway encabezados x-forwarded-for?

Sí. Consulte [modificaciones para solicitar](https://docs.microsoft.com/azure/application-gateway/how-application-gateway-works#modifications-to-the-request) para obtener información sobre los encabezados x-forwarded-for admitidos Application Gateway.

### <a name="how-long-does-it-take-to-deploy-an-application-gateway-does-my-application-gateway-still-work-when-being-updated"></a>¿Cuánto tiempo se tarda en implementar Application Gateway? ¿Application Gateway sigue funcionando cuando se actualiza?

Las nuevas implementaciones de SKU v1 de Application Gateway pueden tardar hasta 20 minutos en aprovisionarse. Los cambios de tamaño y recuento de instancias no provocan interrupciones, y la puerta de enlace permanece activa durante este tiempo.

Las implementaciones de SKU V2 pueden tardar unos cinco o seis minutos en aprovisionarse.

### <a name="can-exchange-server-be-used-as-backend-with-application-gateway"></a>¿Servidor de Exchange se puede usar como back-end con Application Gateway?

No, Application Gateway no admite protocolos de correo electrónico como SMTP, IMAP y POP3. 

## <a name="performance"></a>Rendimiento

### <a name="how-does-application-gateway-support-high-availability-and-scalability"></a>¿Cómo admite Application Gateway la alta disponibilidad y la escalabilidad?

La SKU v1 de Application Gateway admite escenarios de alta disponibilidad cuando se tienen implementadas dos o más instancias. Azure distribuye estas instancias entre dominios de actualización y de errores para asegurarse de que todas las instancias no produzcan un error al mismo tiempo. La SKU v1 admite escalabilidad mediante la incorporación de varias instancias de la misma puerta de enlace para compartir la carga.

La SKU v2 garantiza automáticamente que las nuevas instancias se distribuyan entre dominios de error y dominios de actualización. Si se elige la redundancia de zona, las instancias más recientes también se distribuyen entre las zonas de disponibilidad para ofrecer resistencia ante errores de zona.

### <a name="how-do-i-achieve-dr-scenario-across-data-centers-with-application-gateway"></a>¿Cómo se puede lograr el escenario de recuperación ante desastres a través de centros de datos con Application Gateway?

Los clientes pueden usar Traffic Manager para distribuir el tráfico a través de varias instancias de Application Gateway en distintos centros de datos.

### <a name="is-autoscaling-supported"></a>¿Se admite la escalabilidad automática?

Sí, la SKU v2 de Application Gateway admite la escalabilidad automática. Para más información, consulte [Escalabilidad automática y puerta de enlace de aplicaciones con redundancia de zona (versión preliminar pública)](application-gateway-autoscaling-zone-redundant.md).

### <a name="does-manual-scale-updown-cause-downtime"></a>¿Provoca el escalado o reducción vertical algún tiempo de inactividad?

No hay ningún tiempo de inactividad. Las instancias se distribuyen entre varios dominios de actualización y dominios de error.

### <a name="does-application-gateway-support-connection-draining"></a>¿Es compatible Application Gateway con la funcionalidad de drenaje de conexiones?

Sí. Puede configurar el drenaje de conexiones para cambiar los miembros de un grupo de servidores back-end sin interrupciones. De este modo, las conexiones existentes podrán seguir enviándose a su destino anterior hasta que se cierren o hasta que el tiempo de espera configurado se agote. El drenaje de conexiones solo espera a que se completen las conexiones que están en tránsito actualmente. Application Gateway no conoce el estado de sesión de las aplicaciones.

### <a name="can-i-change-instance-size-from-medium-to-large-without-disruption"></a>¿Puedo cambiar el tamaño de la instancia de mediano a grande sin que haya una interrupción?

Sí, Azure distribuye las instancias entre los dominios de actualización y de errores para asegurarse de que todas las instancias no produzcan un error al mismo tiempo. Application Gateway admite el escalado mediante la adición de varias instancias de la misma puerta de enlace para compartir la carga.

## <a name="configuration"></a>Configuración

### <a name="is-application-gateway-always-deployed-in-a-virtual-network"></a>¿Se implementa Application Gateway siempre en una red virtual?

Sí, Application Gateway se implementa siempre en una red virtual. Esta subred solo puede contener instancias de Application Gateway. Consulte [requisitos de red y subred virtual](https://docs.microsoft.com/azure/application-gateway/configuration-overview#azure-virtual-network-and-dedicated-subnet) para conocer las consideraciones de la subred de Application Gateway.

### <a name="can-application-gateway-communicate-with-instances-outside-of-the-virtual-network-it-is-in-or-outside-of-the-subscription-it-is-in"></a>¿Application Gateway puede comunicarse con instancias fuera de la red virtual está en o fuera de la suscripción está en?

Application Gateway puede comunicarse con instancias fuera de la red virtual que está en o fuera de la suscripción esté, siempre que haya conectividad IP. Si planea usar direcciones IP internas como miembros del grupo de back-end, necesitará [emparejamiento de VNET](../virtual-network/virtual-network-peering-overview.md) o [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md).

### <a name="can-i-deploy-anything-else-in-the-application-gateway-subnet"></a>¿Puedo implementar algo más en la subred de la puerta de enlace de aplicaciones?

No, pero se pueden implementar otras puertas de enlace de aplicación en la subred.

### <a name="are-network-security-groups-supported-on-the-application-gateway-subnet"></a>¿Se admiten grupos de seguridad de red en la subred de la puerta de enlace de aplicaciones?

Los grupos de seguridad de red (NSG) se admiten en la subred de la puerta de enlace de aplicaciones con las restricciones siguientes:

* Se deben colocar excepciones para el tráfico entrante en los puertos 65503-65534 para la SKU v1 de Application Gateway y los puertos 65200-65535 para la SKU v2. Este intervalo de puertos es necesario para la comunicación de la infraestructura de Azure. Están protegidos (bloqueados) mediante certificados de Azure. Sin los certificados apropiados, las entidades externas, incluidos los clientes de esas puertas de enlace, no podrán iniciar ningún cambio en esos puntos de conexión.

* No puede bloquearse la conectividad saliente de Internet. Reglas de salida de forma predeterminada en el NSG ya permiten la conectividad a internet. Se recomienda no quitar las reglas de salida predeterminado y que no se crean otras reglas de salida que deniegan la conectividad saliente de internet.

* Se debe permitir el tráfico de la etiqueta AzureLoadBalancer.

### <a name="are-user-defined-routes-supported-on-the-application-gateway-subnet"></a>¿Se admiten las rutas definidas por el usuario en la subred de la puerta de enlace de aplicaciones?

Consulte [restricciones de las rutas definidas por el usuario](https://docs.microsoft.com/azure/application-gateway/configuration-overview#user-defined-routes-supported-on-the-application-gateway-subnet) para obtener información acerca de las rutas definidas por el usuario admitidas en la subred de puerta de enlace de aplicaciones.

### <a name="what-are-the-limits-on-application-gateway-can-i-increase-these-limits"></a>¿Cuáles son los límites de Application Gateway? ¿Puedo aumentar estos límites?

Consulte [Límites de Application Gateway](../azure-subscription-service-limits.md#application-gateway-limits) para ver los límites.

### <a name="can-i-use-application-gateway-for-both-external-and-internal-traffic-simultaneously"></a>¿Puedo usar Application Gateway para tráfico externo e interno al mismo tiempo?

Sí, Application Gateway admite una dirección IP interna y una dirección IP externa por cada puerta de enlace de aplicaciones.

### <a name="is-vnet-peering-supported"></a>¿Se admite el emparejamiento de VNet?

Sí, se admite el emparejamiento de VNet y, además, es beneficioso para el tráfico de equilibrio de carga en otras redes virtuales.

### <a name="can-i-talk-to-on-premises-servers-when-they-are-connected-by-expressroute-or-vpn-tunnels"></a>¿Se puede comunicar con servidores locales cuando están conectados mediante ExpressRoute o túneles VPN?

Sí, siempre que se permita el tráfico.

### <a name="can-i-have-one-backend-pool-serving-many-applications-on-different-ports"></a>¿Se puede tener un grupo de back-end que preste servicio a muchas aplicaciones en puertos diferentes?

Se admite la arquitectura de microservicios. Necesitaría varias opciones de configuración de http configuradas para realizar sondeos en puertos diferentes.

### <a name="do-custom-probes-support-wildcardsregex-on-response-data"></a>¿Admiten los sondeos personalizados caracteres comodín o regex en los datos de respuesta?

Los sondeos personalizados no admiten caracteres comodín o regex en los datos de respuesta.

### <a name="how-are-rules-processed"></a>¿Cómo se procesan las reglas?

Las reglas se procesan en el orden en que están configuradas. Se recomienda que las reglas de varios sitios se configuren antes de las reglas básicas para reducir la posibilidad de que el tráfico se enrute al back-end inadecuado, ya que la regla básica coincidiría con el tráfico basado en el puerto antes de que se evalúe la regla de varios sitios.

### <a name="what-does-the-host-field-for-custom-probes-signify"></a>¿Qué significa el campo Host de los sondeos personalizados?

El campo Host especifica el nombre al que enviar el sondeo. Solo se puede aplicar cuando se ha configurado un entorno multisitio en Application Gateway; de lo contrario hay que usar '127.0.0.1'. Este valor es diferente del nombre de host de máquina virtual y está en formato \<protocolo\>://\<host\>:\<puerto\>\<ruta de acceso\>.

### <a name="can-i-whitelist-application-gateway-access-to-a-few-source-ips"></a>¿Puedo permitir a Application Gateway el acceso a algunas direcciones IP de origen?

Sí. Consulte [restringir el acceso a direcciones IP de origen específica](https://docs.microsoft.com/azure/application-gateway/configuration-overview#whitelist-application-gateway-access-to-a-few-source-ips) para entender cómo asegurarse de que solo en la lista blanca direcciones IP de origen pueden tener acceso a la puerta de enlace de la aplicación.

### <a name="can-the-same-port-be-used-for-both-public-and-private-facing-listeners"></a>¿Se puede usar el mismo puerto para los agentes de escucha públicos y privados?

No, no se admite.

## <a name="configuration---ssl"></a>Configuración - SSL

### <a name="what-certificates-are-supported-on-application-gateway"></a>¿Qué certificados se admiten en Application Gateway?

Se admiten los certificados autofirmados, certificados de CA, certificados EV y certificados comodín.

### <a name="what-are-the-current-cipher-suites-supported-by-application-gateway"></a>¿Cuáles son los conjuntos de cifrado actuales que admite Application Gateway?

Los siguientes son los conjuntos de cifrado actuales que admite Application Gateway. Para aprender a personalizar opciones de SSL, consulte [Configuración de versiones de directivas SSL y conjuntos de cifrado en Application Gateway](application-gateway-configure-ssl-policy-powershell.md).

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

### <a name="does-application-gateway-also-support-re-encryption-of-traffic-to-the-backend"></a>¿Admite Application Gateway también el recifrado del tráfico dirigido al back-end?

Sí, Application Gateway admite la descarga de SSL y SSL de extremo a extremo, lo cual permite volver a cifrar el tráfico dirigido al back-end.

### <a name="can-i-configure-ssl-policy-to-control-ssl-protocol-versions"></a>¿Se puede configurar la directiva SSL para controlar las versiones del protocolo SSL?

Sí, puede configurar Application Gateway para denegar TLS1.0, TLS1.1 y TLS1.2. SSL 2.0 y 3.0 ya están deshabilitados de forma predeterminada y no se pueden configurar.

### <a name="can-i-configure-cipher-suites-and-policy-order"></a>¿Puedo configurar los conjuntos de cifrado y el orden de las directivas?

Sí, se admite la [configuración de conjuntos de cifrado](application-gateway-ssl-policy-overview.md). Al definir una directiva personalizada, se debe habilitar al menos uno de los siguientes conjuntos de cifrado. Application Gateway usa SHA256 para la administración de back-end.

* TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 
* TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256
* TLS_DHE_RSA_WITH_AES_128_GCM_SHA256
* TLS_RSA_WITH_AES_128_GCM_SHA256
* TLS_RSA_WITH_AES_256_CBC_SHA256
* TLS_RSA_WITH_AES_128_CBC_SHA256

### <a name="how-many-ssl-certificates-are-supported"></a>¿Cuántos certificados SSL se admiten?

Se admiten hasta 100 certificados SSL.

### <a name="how-many-authentication-certificates-for-backend-re-encryption-are-supported"></a>¿Cuántos certificados de autenticación de recifrado de back-end se admiten?

Se admiten hasta 10 certificados de autenticación, con 5 como valor predeterminado.

### <a name="does-application-gateway-integrate-with-azure-key-vault-natively"></a>¿Se integra Application Gateway con Azure Key Vault de forma nativa?

No, no se integra con Azure Key Vault.

### <a name="how-to-configure-https-listeners-for-com-and-net-sites"></a>¿Cómo configurar los agentes de escucha HTTPS para sitios .com y. NET? 

Para varios basados en dominio (basado en host) enrutamiento, puede crear agentes de escucha multisitio, seleccione HTTPS como protocolo de configuración de agente de escucha y asociar los agentes de escucha a las reglas de enrutamiento. Para obtener más información, consulte [hospedar varios sitios con Application Gateway](https://docs.microsoft.com/azure/application-gateway/multiple-site-overview). 

## <a name="configuration---web-application-firewall-waf"></a>-Configuración de Firewall de aplicaciones Web (WAF)

### <a name="does-the-waf-sku-offer-all-the-features-available-with-the-standard-sku"></a>¿Ofrece la SKU de WAF todas las características disponibles con la SKU estándar?

Sí, WAF admite todas las características de la SKU estándar.

### <a name="what-is-the-crs-version-application-gateway-supports"></a>¿Qué versión de CRS admite Application Gateway?

Application Gateway admite CRS [2.2.9](application-gateway-crs-rulegroups-rules.md#owasp229) y CRS [3.0](application-gateway-crs-rulegroups-rules.md#owasp30).

### <a name="how-do-i-monitor-waf"></a>¿Cómo se supervisa el WAF?

WAF se supervisa a través del registro de diagnóstico. Puede encontrar más información sobre el registro de diagnóstico en [Métricas y registro de diagnóstico de Application Gateway](application-gateway-diagnostics.md)

### <a name="does-detection-mode-block-traffic"></a>¿Bloquea el modo de detección el tráfico?

No, el modo de detección solo registra el tráfico, que desencadenó una regla de WAF.

### <a name="can-i-customize-waf-rules"></a>¿Se pueden personalizan las reglas de WAF?

Sí, las reglas de WAF son personalizables. Para más información, consulte [Personalización de reglas de firewall de aplicaciones web mediante Azure Portal](application-gateway-customize-waf-rules-portal.md).

### <a name="what-rules-are-currently-available"></a>¿Qué reglas están disponibles actualmente?

WAF actualmente admite CRS [2.2.9](application-gateway-crs-rulegroups-rules.md#owasp229) y [3.0](application-gateway-crs-rulegroups-rules.md#owasp30), que proporcionan una seguridad de línea de base frente a la mayoría de las 10 principales vulnerabilidades identificadas por Open Web Application Security Project (OWASP) y que se describen en [OWASP top 10 Vulnerabilities](https://www.owasp.org/index.php/Top10#OWASP_Top_10_for_2013) (Las 10 principales vulnerabilidades de OWASP).

* Protección contra la inyección de código SQL

* Protección contra scripts entre sitios

* Protección contra ataques web comunes, como inyección de comandos, contrabando de solicitudes HTTP, división de respuestas HTTP y ataque remoto de inclusión de archivos

* Protección contra infracciones del protocolo HTTP

* Protección contra anomalías del protocolo HTTP, como la falta de agentes de usuario de host y encabezados de aceptación

* Prevención contra bots, rastreadores y escáneres

* Detección de errores de configuración comunes en aplicaciones (es decir, Apache, IIS, etc.)

### <a name="does-waf-also-support-ddos-prevention"></a>¿Admite también WAF la prevención DDoS?

Sí. Puede habilitar la protección contra DDos en la red virtual donde se implementa la puerta de enlace de aplicaciones. Así se garantiza que la dirección IP virtual de la puerta de enlace de aplicaciones también está protegida con el servicio Azure DDos Protection.

## <a name="diagnostics-and-logging"></a>Diagnósticos y registro

### <a name="what-types-of-logs-are-available-with-application-gateway"></a>¿Qué tipos de registros están disponibles con Application Gateway?

Hay tres registros disponibles para Application Gateway. Para más información sobre estos registros y otras funcionalidades de diagnóstico, consulte [Mantenimiento del back-end, registro de diagnóstico y métricas de Application Gateway](application-gateway-diagnostics.md).

* **ApplicationGatewayAccessLog**: este registro de acceso contiene todas las solicitudes enviadas al front-end de la puerta de enlace de aplicaciones. Los datos incluyen la dirección IP del autor de la llamada, la dirección URL solicitada, la latencia de la respuesta, el código de devolución y los bytes de entrada y salida. El registro de acceso se recopila cada 300 segundos. Este registro contiene un registro por cada instancia de una puerta de enlace de aplicaciones.
* **ApplicationGatewayPerformanceLog**: este registro de rendimiento captura la información de rendimiento de cada instancia, incluida la cantidad total de solicitudes atendidas, el rendimiento en bytes, la cantidad de solicitudes con error y la cantidad de instancias back-end completadas correcta e incorrectamente.
* **ApplicationGatewayFirewallLog**: este registro de firewall contiene las solicitudes que se registran con el modo de detección o prevención de una puerta de enlace de aplicaciones que está configurada con el firewall de aplicaciones web.

### <a name="how-do-i-know-if-my-backend-pool-members-are-healthy"></a>¿Cómo se puede saber si mis miembros del grupo de back-end están en buenas condiciones?

Puede usar el cmdlet de PowerShell `Get-AzApplicationGatewayBackendHealth` o comprobar el estado a través del portal visitando [Diagnósticos de Application Gateway](application-gateway-diagnostics.md)

### <a name="what-is-the-retention-policy-on-the-diagnostics-logs"></a>¿Qué es la directiva de retención en los registros de diagnóstico?

Los registros de diagnóstico fluyen hacia la cuenta de almacenamiento de los clientes y estos pueden establecer la directiva de retención según sus preferencias. Los registros de diagnóstico también se pueden enviar a un centro de eventos o a registros de Azure Monitor. Consulte [Diagnósticos de Application Gateway](application-gateway-diagnostics.md) para más información.

### <a name="how-do-i-get-audit-logs-for-application-gateway"></a>¿Cómo se pueden obtener los registros de auditoría de Application Gateway?

Los registros de auditoría están disponibles para Application Gateway. En el portal, haga clic en **registro de actividad** en la hoja del menú de una puerta de enlace de aplicaciones para tener acceso al registro de auditoría. 

### <a name="can-i-set-alerts-with-application-gateway"></a>¿Se pueden establecer alertas con Application Gateway?

Sí, Application Gateway admite alertas. Las alertas se configuran en métricas. Para más información sobre las métricas de Application Gateway, consulte [Métricas de Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics#metrics). Para más información sobre las alertas, consulte [Recibir notificaciones de alerta](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).

### <a name="how-do-i-analyze-traffic-statistics-for-application-gateway"></a>¿Cómo se pueden analizar las estadísticas de tráfico de Application Gateway?

Puede ver y analizar los registros de acceso mediante diferentes mecanismos, como registros de Azure Monitor, Excel, Power BI, etc.

También hemos publicado una plantilla de Resource Manager que instala y ejecuta el conocido analizador de registros [GoAccess](https://goaccess.io/) para los registros de acceso de Application Gateway. GoAccess proporciona valiosas estadísticas de tráfico HTTP como visitantes únicos, archivos solicitados, hosts, sistemas operativos, exploradores, códigos de estado HTTP y mucho más. Para obtener más información, consulte el [archivo Léame en la carpeta de plantillas de Resource Manager en GitHub](https://aka.ms/appgwgoaccessreadme).

### <a name="backend-health-returns-unknown-status-what-could-be-causing-this-status"></a>El estado de back-end devuelve un estado desconocido, ¿qué puede haberlo provocado?

La razón más común es que el acceso al servidor está bloqueado por un NSG o DNS personalizado. Consulte [Mantenimiento del back-end, registro de diagnóstico y métricas de Application Gateway](application-gateway-diagnostics.md) para más información.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Application Gateway, consulte [¿Qué es Azure Application Gateway?](overview.md)
