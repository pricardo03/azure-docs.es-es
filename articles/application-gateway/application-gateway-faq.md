---
title: Preguntas frecuentes sobre Azure Application Gateway
description: Encuentre respuestas a las preguntas más frecuentes sobre Azure Application Gateway.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 08/31/2019
ms.author: victorh
ms.openlocfilehash: f2f2e02cdb5698d7569e5be177d54ca4dcb0ae02
ms.sourcegitcommit: cfbea479cc065c6343e10c8b5f09424e9809092e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/08/2020
ms.locfileid: "77086538"
---
# <a name="frequently-asked-questions-about-application-gateway"></a>Preguntas más frecuentes sobre Application Gateway

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Las siguientes preguntas son preguntas frecuentes sobre Azure Application Gateway.

## <a name="general"></a>General

### <a name="what-is-application-gateway"></a>¿Qué es una Puerta de enlace de aplicaciones?

Azure Application Gateway proporciona un controlador de entrega de aplicaciones (ADC) como servicio. Ofrece diversas funcionalidades de equilibrio de carga de nivel 7 para sus aplicaciones. Este servicio es altamente disponible y escalable, y está completamente administrado por Azure.

### <a name="what-features-does-application-gateway-support"></a>¿Qué características admite Application Gateway?

Application Gateway admite escalabilidad automática, descarga de SSL, SSL de un extremo a otro, firewall de aplicaciones web (WAF), afinidad de sesión basada en cookies, enrutamiento basado en ruta de dirección URL, hospedaje de varios sitios y otras características. Para obtener una lista completa de las características admitidas, consulte [Introducción a Application Gateway](application-gateway-introduction.md).

### <a name="how-do-application-gateway-and-azure-load-balancer-differ"></a>¿En qué se diferencian Application Gateway y Azure Load Balancer?

Application Gateway es un equilibrador de carga de nivel 7, lo que significa que solo funciona con tráfico web (HTTP, HTTPS, WebSocket y HTTP/2). Admite funcionalidades como la terminación SSL, la afinidad de sesión basada en cookies y la distribución round robin para el tráfico de equilibrio de carga. Load Balancer equilibra la carga de tráfico en el nivel 4 (TCP o UDP).

### <a name="what-protocols-does-application-gateway-support"></a>¿Qué protocolos admite Application Gateway?

Application Gateway admite HTTP, HTTPS, HTTP/2 y WebSocket.

### <a name="how-does-application-gateway-support-http2"></a>¿Admite Application Gateway HTTP/2?

Consulte [HTTP/2 support](https://docs.microsoft.com/azure/application-gateway/configuration-overview#http2-support) (Compatibilidad con HTTP/2).

### <a name="what-resources-are-supported-as-part-of-a-backend-pool"></a>¿Qué recursos son compatibles como parte de un grupo de back-end?

Consulte [supported backend resources](https://docs.microsoft.com/azure/application-gateway/application-gateway-components#backend-pools) (recursos de back-end compatibles).

### <a name="in-what-regions-is-application-gateway-available"></a>¿En qué regiones está disponible Application Gateway?

Application Gateway está disponible en todas las regiones de Azure global. También está disponible en [Azure China 21Vianet](https://www.azure.cn/) y [Azure Government](https://azure.microsoft.com/overview/clouds/government/).

### <a name="is-this-deployment-dedicated-for-my-subscription-or-is-it-shared-across-customers"></a>¿Se trata de una implementación dedicada para mi suscripción o compartida entre los clientes?

Application Gateway es una implementación dedicada en su red virtual.

### <a name="does-application-gateway-support-http-to-https-redirection"></a>¿Admite Application Gateway el redireccionamiento de HTTP a HTTPS?

Se admite el redireccionamiento. Consulte [Introducción a la redirección de Application Gateway](application-gateway-redirect-overview.md).

### <a name="in-what-order-are-listeners-processed"></a>¿En qué orden se procesan los agentes de escucha?

Consulte el [orden de procesamiento del agente de escucha](https://docs.microsoft.com/azure/application-gateway/configuration-overview#order-of-processing-listeners).

### <a name="where-do-i-find-the-application-gateway-ip-and-dns"></a>¿Dónde se encuentra la dirección IP y el DNS de Application Gateway?

Si usa una dirección IP pública como punto de conexión, encontrará la información de la dirección IP y del DNS en el recurso de dirección IP pública. O bien la encontrará en el portal, en la página de información general de Application Gateway. Si usa direcciones IP internas, encontrará la información en la página información general.

### <a name="what-are-the-settings-for-keep-alive-timeout-and-tcp-idle-timeout"></a>¿Cuáles son los valores de tiempo de espera de conexión persistente y tiempo de espera de inactividad de TCP?

El *tiempo de espera de conexión persistente* rige cuánto tiempo esperará Application Gateway para que un cliente envíe otra solicitud HTTP en una conexión persistente antes de reutilizarla o cerrarla. El *tiempo de espera de inactividad de TCP* rige cuánto tiempo se mantiene abierta una conexión TCP en caso de que no haya ninguna actividad. 

El *tiempo de espera de conexión persistente* en la SKU de Application Gateway v1 es 120 segundos, mientras que en la SKU v2 es 75 segundos. El *tiempo de espera de inactividad de TCP* es el valor predeterminado de 4 minutos en la IP virtual (VIP) de front-end tanto de la SKU v1 y v2 de Application Gateway. 

### <a name="does-the-ip-or-dns-name-change-over-the-lifetime-of-the-application-gateway"></a>¿Cambia la dirección IP o el nombre DNS durante la vigencia de Application Gateway?

En la SKU de la versión 1 de Application Gateway, la IP virtual puede cambiar si se detiene o inicia la puerta de enlace de aplicaciones. Sin embargo, el nombre DNS asociado a Application Gateway no cambia durante la vigencia de la puerta de enlace. Como el nombre DNS no cambia, se debe usar un alias de CNAME y apuntarlo hacia la dirección DNS de Application Gateway. En la SKU de la versión 2 de Application Gateway, puede establecer la dirección IP como estática, por lo que el nombre de IP y DNS no cambiará durante la vigencia de la puerta de enlace de aplicaciones. 

### <a name="does-application-gateway-support-static-ip"></a>¿Admite Application Gateway direcciones IP estáticas?

Sí, la SKU v2 de Application Gateway es compatible con direcciones IP públicas estáticas. La SKU v1 es compatible con direcciones IP internas estáticas.

### <a name="does-application-gateway-support-multiple-public-ips-on-the-gateway"></a>¿Admite Application Gateway varias direcciones IP públicas en la puerta de enlace?

Solo se admite una dirección IP pública en una instancia de Application Gateway.

### <a name="how-large-should-i-make-my-subnet-for-application-gateway"></a>¿Cómo de grande debe ser la subred para Application Gateway?

Consulte [Application Gateway subnet size considerations](https://docs.microsoft.com/azure/application-gateway/configuration-overview#size-of-the-subnet) (Consideraciones de tamaño de subred de Application Gateway).

### <a name="can-i-deploy-more-than-one-application-gateway-resource-to-a-single-subnet"></a>¿Puedo implementar más de un recurso de Application Gateway para una sola subred?

Sí. Además de tener varias instancias de una determinada implementación de Application Gateway, puede aprovisionar otro recurso de Application Gateway único para una subred existente que contenga un recurso de Application Gateway diferente.

Una sola subred no admite Application Gateway Standard_v2 y Standard conjuntamente.

### <a name="does-application-gateway-support-x-forwarded-for-headers"></a>¿Admite Application Gateway encabezados x-forwarded-for?

Sí. Consulte [Modifications to a request](https://docs.microsoft.com/azure/application-gateway/how-application-gateway-works#modifications-to-the-request) (Modificaciones a una solicitud).

### <a name="how-long-does-it-take-to-deploy-an-application-gateway-will-my-application-gateway-work-while-its-being-updated"></a>¿Cuánto tiempo se tarda en implementar Application Gateway? ¿Mi instancia de Application Gateway funcionará mientras se actualiza?

Las nuevas implementaciones de SKU v1 de Application Gateway pueden tardar hasta 20 minutos en aprovisionarse. Los cambios de tamaño o recuento de instancias no provocan interrupciones y Application Gateway permanece activa durante este tiempo.

La mayoría de las implementaciones que usan la SKU v2 tardan aproximadamente 6 minutos en aprovisionarse. Sin embargo, pueden tardar más tiempo en función del tipo de implementación. Por ejemplo, las implementaciones en varias instancias de Availability Zones con muchas instancias pueden tardar más de 6 minutos. 

### <a name="can-i-use-exchange-server-as-a-backend-with-application-gateway"></a>¿Puedo usar Exchange Server como un back-end con Application Gateway?

No. Application Gateway no admite protocolos de correo electrónico como SMTP, IMAP y POP3. 

## <a name="performance"></a>Rendimiento

### <a name="how-does-application-gateway-support-high-availability-and-scalability"></a>¿Cómo admite Application Gateway la alta disponibilidad y la escalabilidad?

La SKU v1 de Application Gateway admite escenarios de alta disponibilidad cuando se tienen implementadas dos o más instancias. Azure distribuye estas instancias entre dominios de actualización y de errores para asegurarse de que las instancias no produzcan un error todas al mismo tiempo. La SKU v1 admite escalabilidad mediante la incorporación de varias instancias de la misma puerta de enlace para compartir la carga.

La SKU v2 garantiza automáticamente que las nuevas instancias se distribuyan entre dominios de error y dominios de actualización. Si elige la redundancia de zona, las instancias más recientes también se distribuyen entre las zonas de disponibilidad para ofrecer resistencia ante errores de zona.

### <a name="how-do-i-achieve-a-dr-scenario-across-datacenters-by-using-application-gateway"></a>¿Cómo se puede lograr un escenario de recuperación ante desastres a través de centros de datos con Application Gateway?

Use Traffic Manager para distribuir el tráfico a través de varias instancias de Application Gateway en distintos centros de datos.

### <a name="does-application-gateway-support-autoscaling"></a>¿Application Gateway admite la escalabilidad automática?

Sí, la SKU v2 de Application Gateway admite la escalabilidad automática. Para más información, consulte [Autoscaling and Zone-redundant Application Gateway](application-gateway-autoscaling-zone-redundant.md) (Escalabilidad automática y Application Gateway con redundancia de zona).

### <a name="does-manual-or-automatic-scale-up-or-scale-down-cause-downtime"></a>¿El escalado o la reducción vertical manual o automático provocan algún tiempo de inactividad?

No. Las instancias se distribuyen entre varios dominios de actualización y dominios de error.

### <a name="does-application-gateway-support-connection-draining"></a>¿Es compatible Application Gateway con la funcionalidad de drenaje de conexiones?

Sí. Puede configurar el drenaje de conexiones para cambiar los miembros de un grupo de servidores back-end sin interrupciones. Para obtener más información, consulte la [sección de purga de la conexión de Application Gateway](overview.md#connection-draining).

### <a name="can-i-change-instance-size-from-medium-to-large-without-disruption"></a>¿Puedo cambiar el tamaño de la instancia de mediano a grande sin que haya una interrupción?

Sí.

## <a name="configuration"></a>Configuración

### <a name="is-application-gateway-always-deployed-in-a-virtual-network"></a>¿Se implementa Application Gateway siempre en una red virtual?

Sí. Application Gateway se implementa siempre en una subred de red virtual. Esta subred solo puede contener instancias de Application Gateway. Para más información, consulte los [requisitos de subred y red virtual](https://docs.microsoft.com/azure/application-gateway/configuration-overview#azure-virtual-network-and-dedicated-subnet).

### <a name="can-application-gateway-communicate-with-instances-outside-of-its-virtual-network-or-outside-of-its-subscription"></a>¿Puede Application Gateway comunicarse con instancias fuera de su red virtual o de su suscripción?

Siempre que tenga conectividad IP, Application Gateway puede comunicarse con instancias fuera de la red virtual en la que se encuentra. Application Gateway también se puede comunicar con instancias fuera de la red virtual en la que se encuentra. Si planea usar direcciones IP internas como miembros de grupo de back-end, use el [emparejamiento de redes virtuales](../virtual-network/virtual-network-peering-overview.md) o [Azure VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md).

### <a name="can-i-deploy-anything-else-in-the-application-gateway-subnet"></a>¿Puedo implementar algo más en la subred de la puerta de enlace de aplicaciones?

No. Pero se pueden implementar otras instancias de Application Gateway en la subred.

### <a name="are-network-security-groups-supported-on-the-application-gateway-subnet"></a>¿Se admiten grupos de seguridad de red en la subred de Application Gateway?

Consulte [Grupos de seguridad de red en la subred de Application Gateway](https://docs.microsoft.com/azure/application-gateway/configuration-overview#network-security-groups-on-the-application-gateway-subnet).

### <a name="does-the-application-gateway-subnet-support-user-defined-routes"></a>¿Admite la subred de Application Gateway rutas definidas por el usuario?

Consulte [User-defined routes supported in the Application Gateway subnet](https://docs.microsoft.com/azure/application-gateway/configuration-overview#user-defined-routes-supported-on-the-application-gateway-subnet) (Rutas definidas por el usuario en la subred de Application Gateway).

### <a name="what-are-the-limits-on-application-gateway-can-i-increase-these-limits"></a>¿Cuáles son los límites de Application Gateway? ¿Puedo aumentar estos límites?

Consulte [Límites de Application Gateway](../azure-resource-manager/management/azure-subscription-service-limits.md#application-gateway-limits).

### <a name="can-i-simultaneously-use-application-gateway-for-both-external-and-internal-traffic"></a>¿Puedo usar Application Gateway para tráfico externo e interno al mismo tiempo?

Sí. Application Gateway admite una dirección IP interna y una dirección IP externa por cada instancia de Application Gateway.

### <a name="does-application-gateway-support-virtual-network-peering"></a>¿Admite Application Gateway el emparejamiento de redes virtuales?

Sí. El emparejamiento de redes virtuales ayuda a equilibrar la carga del tráfico en otras redes virtuales.

### <a name="can-i-talk-to-on-premises-servers-when-theyre-connected-by-expressroute-or-vpn-tunnels"></a>¿Me puedo comunicar con servidores locales cuando están conectados mediante ExpressRoute o túneles VPN?

Sí, siempre que se permita el tráfico.

### <a name="can-one-backend-pool-serve-many-applications-on-different-ports"></a>¿Puede un grupo de back-end prestar servicio a muchas aplicaciones en puertos diferentes?

Se admite la arquitectura de microservicios. Para realizar sondeos en puertos diferentes, debe establecer varias opciones de configuración de HTTP.

### <a name="do-custom-probes-support-wildcards-or-regex-on-response-data"></a>¿Admiten los sondeos personalizados caracteres comodín o regex en los datos de respuesta?

No. 

### <a name="how-are-routing-rules-processed-in-application-gateway"></a>¿Cómo se procesan las reglas de enrutamiento en Application Gateway?

See [Order of processing rules](https://docs.microsoft.com/azure/application-gateway/configuration-overview#order-of-processing-rules) (Orden de procesamiento de reglas).

### <a name="for-custom-probes-what-does-the-host-field-signify"></a>¿Qué significa el campo Host de los sondeos personalizados?

El campo Host especifica el nombre al que se debe enviar el sondeo cuando se ha configurado la funcionalidad de multisitio en Application Gateway. De lo contrario, use '127.0.0.1'. Este valor es distinto del nombre de host de máquina virtual. Su formato es \<protocolo\>://\<host\>:\<puerto\>\<ruta de acceso\>.

### <a name="can-i-allow-application-gateway-access-to-only-a-few-source-ip-addresses"></a>¿Se puede permitir el acceso de Application Gateway a solo unas cuantas direcciones IP de origen?

Sí. Consulte la [restricción del acceso a determinadas direcciones IP de origen](https://docs.microsoft.com/azure/application-gateway/configuration-overview#allow-application-gateway-access-to-a-few-source-ips).

### <a name="can-i-use-the-same-port-for-both-public-facing-and-private-facing-listeners"></a>¿Puedo usar el mismo puerto para los agentes de escucha públicos y privados?

No.

### <a name="is-there-guidance-available-to-migrate-from-the-v1-sku-to-the-v2-sku"></a>¿Hay instrucciones disponibles para migrar de la SKU v1 a la SKU v2?

Sí. Para más información, consulte [Migración de Azure Application Gateway y Firewall de aplicaciones web de v1 a v2](migrate-v1-v2.md).

### <a name="does-application-gateway-support-ipv6"></a>¿Application Gateway admite IPv6?

Application Gateway V2 no es compatible actualmente con IPv6. Puede funcionar en una red virtual de pila dual solo mediante IPv4, pero la subred de puerta de enlace debe ser solo IPv4. Application Gateway v1 no admite redes virtuales de pila dual. 

## <a name="configuration---ssl"></a>Configuración: SSL

### <a name="what-certificates-does-application-gateway-support"></a>¿Qué certificados admite Application Gateway?

Application Gateway admite certificados autofirmados, certificados de entidad de certificación (CA), certificados de validación extendida (EV) y certificados comodín.

### <a name="what-cipher-suites-does-application-gateway-support"></a>¿Qué conjuntos de cifrado admite Application Gateway?

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

Para obtener información sobre cómo personalizar opciones de SSL, consulte [Configuración de versiones de directivas SSL y conjuntos de cifrado en Application Gateway](application-gateway-configure-ssl-policy-powershell.md).

### <a name="does-application-gateway-support-reencryption-of-traffic-to-the-backend"></a>¿Admite Application Gateway el recifrado del tráfico dirigido al back-end?

Sí. Application Gateway admite la descarga de SSL y SSL de un extremo a otro, lo cual permite volver a cifrar el tráfico dirigido al back-end.

### <a name="can-i-configure-ssl-policy-to-control-ssl-protocol-versions"></a>¿Se puede configurar la directiva SSL para controlar las versiones del protocolo SSL?

Sí. Puede configurar Application Gateway para denegar TLS1.0, TLS1.1 y TLS1.2. De forma predeterminada, SSL 2.0 y 3.0 ya están deshabilitadas y no se pueden configurar.

### <a name="can-i-configure-cipher-suites-and-policy-order"></a>¿Puedo configurar los conjuntos de cifrado y el orden de las directivas?

Sí. En Application Gateway, puede [configurar conjuntos de cifrado](application-gateway-ssl-policy-overview.md). Para definir una directiva personalizada, se debe habilitar al menos uno de los siguientes conjuntos de cifrado. 

* TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 
* TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256
* TLS_DHE_RSA_WITH_AES_128_GCM_SHA256
* TLS_RSA_WITH_AES_128_GCM_SHA256
* TLS_RSA_WITH_AES_256_CBC_SHA256
* TLS_RSA_WITH_AES_128_CBC_SHA256

Application Gateway usa SHA256 para la administración de back-end.

### <a name="how-many-ssl-certificates-does-application-gateway-support"></a>Cuántos certificados SSL admite Application Gateway?

Application Gateway admite hasta 100 certificados SSL.

### <a name="how-many-authentication-certificates-for-backend-reencryption-does-application-gateway-support"></a>¿Cuántos certificados de autenticación de recifrado de back-end admite Application Gateway?

Application Gateway admite hasta 100 certificados de autenticación.

### <a name="does-application-gateway-natively-integrate-with-azure-key-vault"></a>¿Se integra Application Gateway con Azure Key Vault de forma nativa?

Sí, la SKU v2 de Application Gateway admite Key Vault. Para obtener más información, consulte [Terminación SSL con certificados de Key Vault](key-vault-certs.md).

### <a name="how-do-i-configure-https-listeners-for-com-and-net-sites"></a>¿Cómo configuro agentes de escucha HTTPS para sitios .com y .NET? 

Para el enrutamiento (basado en host) basado en varios dominios, puede crear agentes de escucha multisitio, configurar agentes de escucha que usen HTTPS como el protocolo y asociar agentes de escucha a las reglas de enrutamiento. Para más información, consulte [Hosting multiple sites by using Application Gateway ](https://docs.microsoft.com/azure/application-gateway/multiple-site-overview)(Hospedaje de varios sitios mediante Application Gateway).

### <a name="can-i-use-special-characters-in-my-pfx-file-password"></a>¿Se pueden usar caracteres especiales en la contraseña del archivo .pfx?

No, use solo caracteres alfanuméricos en la contraseña del archivo .pfx.

## <a name="configuration---web-application-firewall-waf"></a>Configuración: firewall de aplicaciones web (WAF)

### <a name="does-the-waf-sku-offer-all-the-features-available-in-the-standard-sku"></a>¿Ofrece la SKU de WAF todas las características disponibles en la SKU estándar?

Sí. WAF admite todas las características de la SKU estándar.

### <a name="how-do-i-monitor-waf"></a>¿Cómo se supervisa el WAF?

Supervise WAF a través del registro de diagnóstico. Para más información, consulte [Diagnostic logging and metrics for Application Gateway](application-gateway-diagnostics.md) (Registro de diagnóstico y métricas de Application Gateway).

### <a name="does-detection-mode-block-traffic"></a>¿Bloquea el modo de detección el tráfico?

No. El modo de detección solo registra el tráfico que desencadena una regla de WAF.

### <a name="can-i-customize-waf-rules"></a>¿Se pueden personalizan las reglas de WAF?

Sí. Para más información, consulte [Personalización de reglas de firewall de aplicaciones web mediante Azure Portal](application-gateway-customize-waf-rules-portal.md).

### <a name="what-rules-are-currently-available-for-waf"></a>¿Qué reglas están disponibles actualmente para WAF?

Actualmente, WAF admite CRS [2.2.9](../web-application-firewall/ag/application-gateway-crs-rulegroups-rules.md#owasp229), [3.0](../web-application-firewall/ag/application-gateway-crs-rulegroups-rules.md#owasp30) y [3.1](../web-application-firewall/ag/application-gateway-crs-rulegroups-rules.md#owasp31). Estas reglas proporcionan seguridad de línea base frente a la mayoría de las 10 vulnerabilidades principales que Open Web Application Security Project (OWASP) identifica: 

* Protección contra la inyección de código SQL
* Protección contra ataques de scripts de sitios
* Protección contra ataques web comunes, como la inyección de comandos, el contrabando de solicitudes HTTP, la división de respuestas HTTP y el ataque remoto de inclusión de archivos.
* Protección contra infracciones del protocolo HTTP
* Protección contra anomalías del protocolo HTTP, como la falta de agentes de usuario de host y encabezados de aceptación
* Prevención contra bots, rastreadores y escáneres
* Detección de errores de configuración comunes en aplicaciones (es decir, Apache, IIS, etc.).

Para obtener más información, consulte las [10 vulnerabilidades principales de OWASP](https://www.owasp.org/index.php/Top10#OWASP_Top_10_for_2013).

### <a name="does-waf-support-ddos-protection"></a>¿Admite WAF la protección contra DDoS?

Sí. Puede habilitar la protección contra DDos en la red virtual donde se implementa Application Gateway. Esta configuración garantiza que el servicio Azure DDoS Protection también protege la IP virtual (VIP) de Application Gateway.

### <a name="is-there-guidance-available-to-migrate-from-the-v1-sku-to-the-v2-sku"></a>¿Hay instrucciones disponibles para migrar de la SKU v1 a la SKU v2?

Sí. Para más información, consulte [Migración de Azure Application Gateway y Firewall de aplicaciones web de v1 a v2](migrate-v1-v2.md).

## <a name="configuration---ingress-controller-for-aks"></a>Configuración: controlador de entrada para AKS

### <a name="what-is-an-ingress-controller"></a>¿Qué es un controlador de entrada?

Kubernetes permite la creación de los recursos `deployment` y `service` para exponer un grupo de pods internamente en el clúster. Para exponer el mismo servicio externamente, se define un recurso [`Ingress`](https://kubernetes.io/docs/concepts/services-networking/ingress/) que proporciona equilibrio de carga, terminación SSL y hospedaje virtual basado en nombres.
Para satisfacer este recurso `Ingress`, se requiere un controlador de entrada que realice escuchas de los cambios en los recursos `Ingress` y que configure las directivas del equilibrador de carga.

El controlador de entrada de Application Gateway permite que [Azure Application Gateway](https://azure.microsoft.com/services/application-gateway/) se utilice como entrada para una instancia de [Azure Kubernetes Service](https://azure.microsoft.com/services/kubernetes-service/) llamada "clúster de AKS".

### <a name="can-a-single-ingress-controller-instance-manage-multiple-application-gateways"></a>¿Puede una única instancia del controlador de entrada administrar varias puertas de enlace de aplicaciones?

Actualmente, una instancia del controlador de entrada solo se puede asociar a una puerta de enlace de aplicaciones.

## <a name="diagnostics-and-logging"></a>Diagnósticos y registro

### <a name="what-types-of-logs-does-application-gateway-provide"></a>¿Qué tipos de registros proporciona Application Gateway?

Application Gateway proporciona tres registros: 

* **ApplicationGatewayAccessLog**: el registro de acceso contiene todas las solicitudes enviadas al front-end de Application Gateway. Los datos incluyen la dirección IP del autor de la llamada, la dirección URL solicitada, la latencia de la respuesta, el código de devolución y los bytes de entrada y salida. El registro de acceso se recopila cada 300 segundos y contiene un registro por cada instancia de Application Gateway.
* **ApplicationGatewayPerformanceLog**: el registro de rendimiento captura información de rendimiento para cada instancia de Application Gateway. La información incluye el rendimiento en bytes, el número total de solicitudes atendidas, el recuento de solicitudes con error y el recuento de instancias de back-end correctas e incorrectas.
* **ApplicationGatewayFirewallLog**: para las instancias de Application Gateway que configure con WAF, el registro del firewall contiene las solicitudes que se registran mediante el modo de detección o el modo de prevención.

Para más información, consulte [Mantenimiento del back-end, registro de diagnóstico y métricas de Application Gateway](application-gateway-diagnostics.md).

### <a name="how-do-i-know-if-my-backend-pool-members-are-healthy"></a>¿Cómo se puede saber si mis miembros del grupo de back-end están en buenas condiciones?

Compruebe el estado mediante el cmdlet de PowerShell `Get-AzApplicationGatewayBackendHealth` o el portal. Para más información, consulte [Diagnósticos de Application Gateway](application-gateway-diagnostics.md).

### <a name="whats-the-retention-policy-for-the-diagnostic-logs"></a>¿Qué es la directiva de retención para los registros de diagnóstico?

Los registros de diagnóstico se envían a la cuenta de almacenamiento del cliente. Los clientes pueden establecer la directiva de retención según sus preferencias. Los registros de diagnóstico también se pueden enviar a un centro de eventos o a registros de Azure Monitor. Para más información, consulte [Diagnósticos de Application Gateway](application-gateway-diagnostics.md).

### <a name="how-do-i-get-audit-logs-for-application-gateway"></a>¿Cómo se pueden obtener los registros de auditoría de Application Gateway?

En el portal, en la hoja de menú de una instancia de Application Gateway, haga clic en **Registro de actividad** para acceder al registro de auditoría. 

### <a name="can-i-set-alerts-with-application-gateway"></a>¿Se pueden establecer alertas con Application Gateway?

Sí. En Application Gateway, las alertas se configuran en métricas. Para más información, consulte [Application Gateway metrics](https://docs.microsoft.com/azure/application-gateway/application-gateway-metrics) (Métricas de Application Gateway) y [Receive alert notifications](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) (Recepción de notificaciones de alerta).

### <a name="how-do-i-analyze-traffic-statistics-for-application-gateway"></a>¿Cómo se pueden analizar las estadísticas de tráfico de Application Gateway?

Puede ver y analizar los registros de acceso de varias maneras. Use registros de Azure Monitor, Excel, Power BI, etc.

También puede usar una plantilla de Resource Manager que instala y ejecuta el popular analizador de registros [GoAccess](https://goaccess.io/) para los registros de acceso de Application Gateway. GoAccess proporciona valiosas estadísticas de tráfico HTTP, como visitantes únicos, archivos solicitados, hosts, sistemas operativos, exploradores y códigos de estado HTTP. Para más información, en GitHub, consulte el [archivo Léame en la carpeta de plantillas de Resource Manager](https://aka.ms/appgwgoaccessreadme).

### <a name="what-could-cause-backend-health-to-return-an-unknown-status"></a>¿Qué puede provocar que el estado del back-end devuelva un valor desconocido?

Por lo general, se ve un estado desconocido cuando el acceso al back-end está bloqueado por un grupo de seguridad de red (NSG), un DNS personalizado o un enrutamiento definido por el usuario (UDR) en la subred de Application Gateway. Para más información, consulte [Mantenimiento del back-end, registro de diagnóstico y métricas de Application Gateway](application-gateway-diagnostics.md).

### <a name="is-there-any-case-where-nsg-flow-logs-wont-show-allowed-traffic"></a>¿Hay algún caso en el que los registros de flujo de NSG no muestren el tráfico permitido?

Sí. Si la configuración coincide con el siguiente escenario, no verá el tráfico permitido en los registros de flujo de NSG:
- Ha implementado Application Gateway V2.
- Tiene un NSG en la subred de Application Gateway.
- Ha habilitado los registros de flujo de NSG en ese NSG.

### <a name="how-do-i-use-application-gateway-v2-with-only-private-frontend-ip-address"></a>¿Cómo usar Application Gateway V2 solo con una dirección IP de front-end privada?

Actualmente, Application Gateway V2 no admite solo el modo IP privada. Admite las siguientes combinaciones
* IP privada e IP pública
* Solo IP pública

Pero si desea usar Application Gateway V2 con solo IP privada, puede seguir el siguiente proceso:
1. Cree una Application Gateway con la dirección IP de front-end pública y privada
2. No cree ningún cliente de escucha para la dirección IP pública de front-end. Application Gateway no escuchará ningún tráfico en la dirección IP pública si no se crea ningún cliente de escucha para él.
3. Cree y adjunte un [Grupo de seguridad de red](https://docs.microsoft.com/azure/virtual-network/security-overview) para la subred de Application Gateway con la siguiente configuración en orden de prioridad:
    
    a. Permita el tráfico desde el origen como **etiqueta de servicio de GatewayManager** y el destino como **cualquier** y puerto de destino como **65200-65535**. Este intervalo de puertos es necesario para la comunicación de la infraestructura de Azure. Estos puertos están protegidos (bloqueados) por la autenticación de certificados. Las entidades externas, incluidos los administradores de usuarios de la puerta de enlace, no pueden iniciar cambios en esos puntos de conexión sin los certificados adecuados en su lugar
    
    b. Permita el tráfico desde el origen como **AzureLoadBalancer** etiqueta de servicio y el puerto de destino y destino como **cualquiera**
    
    c. Denegar todo el tráfico entrante desde el origen como **etiqueta del servicio de Internet** y el puerto de destino y destino como **cualquiera**. Asigne a esta regla la *mínima prioridad* en las reglas de entrada
    
    d. Mantenga las reglas predeterminadas y permita la entrada de VirtualNetwork para que el acceso en la dirección IP privada no esté bloqueado
    
    e. No puede bloquearse la conectividad saliente de Internet. De lo contrario, se producirán problemas con el registro, las métricas, etc.

Ejemplo de configuración de NSG para acceso de IP privada solamente: ![Configuración de Application Gateway V2 NSG solo para acceso IP privado](./media/application-gateway-faq/appgw-privip-nsg.png)


## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Application Gateway, consulte [¿Qué es Azure Application Gateway?](overview.md).
