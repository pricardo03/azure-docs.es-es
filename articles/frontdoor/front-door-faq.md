---
title: 'Azure Front Door Service: preguntas más frecuentes'
description: Esta página proporciona respuestas a las preguntas más frecuentes acerca de Azure Front Door Service.
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
ms.openlocfilehash: 39051014e5e474264a44983fb366bc08f02c31e0
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/03/2020
ms.locfileid: "75639861"
---
# <a name="frequently-asked-questions-for-azure-front-door-service"></a>Preguntas más frecuentes sobre Azure Front Door Service

En este artículo se responden preguntas comunes sobre la funcionalidad y las características de Azure Front Door Service. Si no encuentra una respuesta a su pregunta, póngase en contacto con nosotros mediante los siguientes canales (en orden incremental):

1. La sección Comentarios de este artículo.
2. [UserVoice de Azure Front Door Service](https://feedback.azure.com/forums/217313-networking?category_id=345025).
3. **Soporte técnico de Microsoft:** Para crear una solicitud de soporte técnico, en Azure Portal, vaya a la pestaña **Ayuda**, seleccione el botón **Ayuda y soporte técnico** y elija **Nueva solicitud de soporte técnico**.

## <a name="general"></a>General

### <a name="what-is-azure-front-door-service"></a>¿Qué es Azure Front Door Service?

Azure Front Door Service es una red de entrega de aplicaciones (ADN) como servicio, que ofrece diversas funcionalidades de equilibrio de carga de capa 7 para sus aplicaciones. Proporciona aceleración de sitios dinámicos (DSA), junto con equilibrio de carga global con conmutación por error casi en tiempo real. Es un servicio altamente disponible y escalable que está completamente administrado por Azure.

### <a name="what-features-does-azure-front-door-service-support"></a>¿Qué características admite Azure Front Door Service?

Azure Front Door Service admite la aceleración de sitios dinámicos, la descarga de SSL y SSL de un extremo a otro, firewall de aplicaciones web, afinidad de sesión basada en cookies, enrutamiento basado en ruta de dirección URL, certificados gratuitos y administración de múltiples dominios, entre otras características. Para obtener una lista completa de las características admitidas, consulte [Overview of Azure Front Door Service](front-door-overview.md) (Información general de Azure Front Door Service).

### <a name="what-is-the-difference-between-azure-front-door-service-and-azure-application-gateway"></a>¿Cuál es la diferencia entre Azure Front Door Service y Azure Application Gateway?

Si bien tanto Front Door como Application Gateway son equilibradores de carga de capa 7 (HTTP/HTTPS), la principal diferencia es que Front Door es un servicio global, mientras que Application Gateway es un servicio regional. Front Door puede equilibrar la carga entre las diferentes unidades de escalado/clústeres/unidades de marca entre regiones, y Application Gateway, por su parte, le permite equilibrar la carga entre las máquinas virtuales y contenedores, entre otros, que se encuentran dentro de la unidad de escalado.

### <a name="when-should-we-deploy-an-application-gateway-behind-front-door"></a>¿Cuándo se debe implementar una instancia de Application Gateway detrás de Front Door?

Los principales escenarios en que se debería usar Application Gateway detrás de Front Door son los siguientes:

- Front Door puede realizar el equilibrio de carga basado en rutas solo a nivel global, pero si se desea equilibrar la carga del tráfico incluso a nivel de la red virtual, debería utilizarse Application Gateway.
- Puesto que Front Door no funciona en un nivel de máquina virtual o el contenedor, no puede hacer purga de conexiones. Por contra, Application Gateway sí le permite hacer purga de conexiones. 
- Con una instancia de Application Gateway detrás de AFD, es posible lograr el 100 % de la descarga de SSL y enrutar solo solicitudes HTTP dentro de su red virtual (VNET).
- Tanto Front Door como Application Gateway admiten la afinidad de la sesión. Mientras que Front Door puede dirigir el tráfico posterior desde una sesión de usuario en el mismo clúster o back-end en una región determinada, Application Gateway puede establecer la afinidad del tráfico al mismo servidor en el clúster.  

### <a name="can-we-deploy-azure-load-balancer-behind-front-door"></a>¿Es posible implementar Azure Load Balancer detrás de Front Door?

Azure Front Door Service necesita una VIP pública o un nombre DNS disponible públicamente al que enrutar el tráfico. La implementación de Azure Load Balancer detrás de Front Door es un caso de uso común.

### <a name="what-protocols-does-azure-front-door-service-support"></a>¿Qué protocolos admite Azure Front Door Service?

Azure Front Door Service admite HTTP, HTTPS y HTTP/2.

### <a name="how-does-azure-front-door-service-support-http2"></a>¿Cómo admite HTTP/2 el servicio Azure Front Door Service?

La compatibilidad con el protocolo HTTP/2 está disponible únicamente para los clientes que se conectan a Azure Front Door Service. La comunicación con los back-end en el grupo de back-end se produce a través de HTTP/1.1. La compatibilidad con HTTP/2 está habilitada de forma predeterminada.

### <a name="what-resources-are-supported-today-as-part-of-backend-pool"></a>¿Qué recursos son compatibles actualmente como parte del grupo de back-end?

Los grupos de back-end pueden estar compuestos de instancias de Storage, aplicaciones web, Kubernetes o cualquier otro nombre de host personalizado que tenga conectividad pública. Azure Front Door Service requiere que los back-end se definan a través de una dirección IP pública o un nombre de host DNS que pueda resolverse públicamente. Los miembros de grupos de back-end pueden estar entre zonas, regiones e incluso fuera de Azure, siempre y cuando dispongan de conectividad pública.

### <a name="what-regions-is-the-service-available-in"></a>¿En qué regiones está disponible el servicio?

Azure Front Door Service es un servicio global y no está asociado a ninguna región de Azure específica. La única ubicación que debe especificar al crear una instancia de Front Door es la ubicación del grupo de recursos, que es básicamente dónde se almacenarán los metadatos para el grupo de recursos. El propio recurso de Front Door se crea como un recurso global, y la configuración se implementa globalmente en todos los puntos de presencia. 

### <a name="what-are-the-pop-locations-for-azure-front-door-service"></a>¿Cuáles son las ubicaciones de los puntos de presencia para Azure Front Door Service?

Azure Front Door Service tiene la misma lista de ubicaciones de puntos de presencia que Azure CDN de Microsoft. Para obtener una lista completa de nuestros puntos de presencia, consulte [Azure CDN POP locations from Microsoft](https://docs.microsoft.com/azure/cdn/cdn-pop-locations) (Ubicaciones de puntos de presencia de Azure CDN de Microsoft).

### <a name="is-azure-front-door-service-a-dedicated-deployment-for-my-application-or-is-it-shared-across-customers"></a>¿Azure Front Door Service es una implementación dedicada para mi aplicación o se comparte entre los clientes?

Azure Front Door Service es un servicio multiinquilino distribuido globalmente. Por lo tanto, la infraestructura de Front Door se comparte entre todos sus clientes. Sin embargo, cuando crea un perfil de Front Door, define la configuración específica necesaria para la aplicación y ninguno de los campos realizados a su Front Door afectan a otras configuraciones de Front Door.

### <a name="is-http-https-redirection-supported"></a>¿Se admite el redireccionamiento HTTP->HTTPS?

Sí. De hecho, el servicio Azure Front Door Service admite la redirección de cadenas de host, ruta de acceso y consulta, así como parte de la redirección de URL. Obtenga más información sobre la [redirección de URL](front-door-url-redirect.md). 

### <a name="in-what-order-are-routing-rules-processed"></a>¿En qué orden se procesan las reglas de enrutamiento?

Las rutas de Front Door no están ordenadas y se selecciona una ruta específica en función de la mejor coincidencia. Obtenga más información sobre [cómo hace coincidir Front Door las solicitudes con una regla de enrutamiento](front-door-route-matching.md).

### <a name="how-do-i-lock-down-the-access-to-my-backend-to-only-azure-front-door"></a>¿Cómo puedo hacer que Azure Front Door sea el único que tenga acceso a mi back-end?

Para que la aplicación solo acepte tráfico procedente de su Front Door específico, debe configurar las listas de control de acceso de IP para el back-end y, a continuación, restringir el conjunto de valores aceptados para el encabezado "X-Forwarded-Host" que envía Azure Front Door. Estos pasos se detallan a continuación:

- Configure la creación de listas de control de acceso de IP de los back-end para que acepten tráfico únicamente del espacio de direcciones IP de back-end de Azure Front Door y de los servicios de infraestructura de Azure. Estamos trabajando para la integración con [intervalos de IP y etiquetas de servicio de Azure](https://www.microsoft.com/download/details.aspx?id=56519), pero por ahora puede consultar los intervalos de IP como se muestra a continuación:
 
    - Espacio de IP de back-end **IPv4** de Front Door: `147.243.0.0/16`
    - Espacio de IP de back-end **IPv6** de Front Door: `2a01:111:2050::/44`
    - [Servicios de infraestructura básica](https://docs.microsoft.com/azure/virtual-network/security-overview#azure-platform-considerations) de Azure través de direcciones IP de host virtualizado: `168.63.129.16` y `169.254.169.254`

    > [!WARNING]
    > El espacio de back-end IP de Front Door puede cambiar más adelante; sin embargo, nos aseguraremos de que antes de que eso suceda nos hayamos integrado con los [intervalos de IP y etiquetas de servicio de Azure](https://www.microsoft.com/download/details.aspx?id=56519). Recomendamos que se suscriba a los [intervalos de IP y etiquetas de servicio de Azure](https://www.microsoft.com/download/details.aspx?id=56519) para conocer los cambios o actualizaciones.

-   Filtre los valores para el encabezado de entrada "**X-Forwarded-Host**" enviados por Front Door. Los únicos valores permitidos para el encabezado deben ser todos los hosts de front-end, tal como están definidos en la configuración de Front Door. Para ser más específicos, solo los nombres de host para los que quiere aceptar tráfico en este back-end determinado.
    - Por ejemplo, supongamos que su configuración de Front Door incluye los siguientes hosts de front-end _`contoso.azurefd.net`_ (A), _`www.contoso.com`_ (B), _ (C) y _`notifications.contoso.com`_ (D). Supongamos que tiene dos back-ends X e Y. 
    - El back-end X solo debe recibir tráfico de los nombres de host A y B. El back-end Y puede recibir el tráfico de A, C y D.
    - Por lo tanto, el back-end X solo debe aceptar tráfico que tenga el encabezado "**X-Forwarded-Host**" establecido en _`contoso.azurefd.net`_ o _`www.contoso.com`_ . Por todos los demás, el back-end X debe rechazar el tráfico.
    - De igual modo, el back-end Y solo debe aceptar el tráfico que tenga el encabezado "**X-Forwarded-Host**" establecido en _`contoso.azurefd.net`_ , _`api.contoso.com`_ o _`notifications.contoso.com`_ . Para todos los demás, el back-end Y debe rechazar el tráfico.

### <a name="can-the-anycast-ip-change-over-the-lifetime-of-my-front-door"></a>¿Puede cambiar la dirección IP de difusión por proximidad durante la vigencia de mi instancia de Front Door?

La dirección IP de difusión por proximidad de su instancia de Front Door normalmente no debería cambiar y es posible que permanezca estática durante la vigencia de Front Door. Sin embargo, esto no se puede **garantizar**. No asuma ninguna dependencia directa de la dirección IP.

### <a name="does-azure-front-door-service-support-static-or-dedicated-ips"></a>¿Azure Front Door Service admite direcciones IP estáticas o dedicadas?

No, Azure Front Door Service actualmente no admite direcciones IP de difusión por proximidad de front-end estáticas o dedicadas. 

### <a name="does-azure-front-door-service-support-x-forwarded-for-headers"></a>¿Azure Front Door Service admite encabezados x-forwarded-for?

Sí, Azure Front Door Service admite los encabezados X-Forwarded-For, X-Forwarded-Host y X-Forwarded-Proto. Para X-Forwarded-For, si el encabezado ya estaba presente, Front Door le agrega la dirección IP del socket del cliente. En caso contrario, agrega el encabezado con la dirección IP del socket del cliente como valor. En el caso de For X-Forwarded-Host y X-Forwarded-Proto, el valor se invalida.

Obtenga más información sobre los [encabezados HTTP que admite Front Door](front-door-http-headers-protocol.md).  

### <a name="how-long-does-it-take-to-deploy-an-azure-front-door-service-does-my-front-door-still-work-when-being-updated"></a>¿Cuánto tiempo se tarda en implementar una instancia de Azure Front Door Service? ¿Mi Front Door sigue funcionando mientras se actualiza?

La creación de una instancia de Front Door o cualquier actualización a la que se someta una instancia de Front Door tarda entre 3 y 5 minutos en implementarse globalmente. Esto significa que, en un tiempo de entre 3 y 5 minutos, la configuración de Front Door se implementará en sus puntos de presencia de manera global.

Nota: La personalización de las actualizaciones de certificados SSL tarda unos 30 minutos en implementarse globalmente.

## <a name="configuration"></a>Configuración

### <a name="can-azure-front-door-load-balance-or-route-traffic-within-a-virtual-network"></a>¿Puede Azure Front Door equilibrar la carga o enrutar el tráfico dentro de una red virtual?

Azure Front Door (AFD) requiere una dirección IP pública o un nombre DNS que pueda resolverse públicamente para enrutar el tráfico. Por lo tanto, la respuesta es que ninguna instancia de AFD puede enrutar directamente dentro de una red virtual, pero el uso de Application Gateway o Azure Load Balancer en medio resolverá este escenario.

### <a name="what-are-the-various-timeouts-and-limits-for-azure-front-door-service"></a>¿Cuáles son los distintos tiempos de espera y límites del servicio Azure Front Door Service?

Conozca todos los [tiempos de espera y límites documentados del servicio Azure Front Door Service](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#azure-front-door-service-limits).

## <a name="performance"></a>Rendimiento

### <a name="how-does-azure-front-door-service-support-high-availability-and-scalability"></a>¿Cómo admite Azure Front Door Service la alta disponibilidad y la escalabilidad?

Azure Front Door Service es una plataforma multiinquilino distribuida globalmente con grandes volúmenes de capacidad para satisfacer las necesidades de escalabilidad de la aplicación. Ofrecida desde el perímetro de la red global de Microsoft, Front Door proporciona una funcionalidad de equilibrio de carga global que le permite conmutar por error la aplicación entera o incluso microservicios individuales a través de regiones o nubes diferentes.

## <a name="ssl-configuration"></a>Configuración de SSL

### <a name="what-tls-versions-are-supported-by-azure-front-door-service"></a>¿Qué versiones de TLS son compatibles con Azure Front Door Service?

Todos los perfiles de Front Door creados después de septiembre de 2019 usan TLS 1.2 como valor mínimo predeterminado.

Front Door admite las versiones 1.0, 1.1 y 1.2 de TLS. Aún no se admite TLS 1.3.

### <a name="what-certificates-are-supported-on-azure-front-door-service"></a>¿Qué certificados se admiten en Azure Front Door Service?

Para habilitar el protocolo HTTPS para distribuir de forma segura el contenido en un dominio personalizado de Front Door, puede optar por usar un certificado administrado por un servicio Azure Front Door Service o utilizar su propio certificado.
La opción administrada de Front Door aprovisiona un certificado SSL estándar mediante Digicert y se almacena en el almacén Key Vault de Front Door. Si decide usar su propio certificado, puede incorporar un certificado de una entidad de certificación compatible y puede ser un SSL estándar, un certificado de validación extendido o incluso un certificado comodín. No se admiten certificados autofirmados. Aprenda a [habilitar HTTPS para un dominio personalizado](https://aka.ms/FrontDoorCustomDomainHTTPS).

### <a name="does-front-door-support-autorotation-of-certificates"></a>¿Front Door admite la rotación automática de certificados?

Para la opción de certificados administrados por Front Door, Front Door rota automáticamente los certificados. Si está usando un certificado administrado por Front Door y nota que la fecha de expiración del certificado es inferior a 60 días, cree una incidencia de soporte técnico.
</br>La rotación automática no es compatible con su propio certificado SSL personalizado. Así como lo configuró la primera vez para un dominio personalizado determinado, tendrá que dirigir Front Door a la versión de certificado correcta en su almacén de claves y asegurarse de que la entidad de servicio para Front Door aún tiene acceso al almacén de claves. Esta operación de lanzamiento de certificados actualizados de Front Door es atómica y no afecta la producción siempre que el nombre de sujeto o SAN para el certificado no cambie.

### <a name="what-are-the-current-cipher-suites-supported-by-azure-front-door-service"></a>¿Cuáles son los conjuntos de cifrado actuales que admite Azure Front Door Service?

A continuación se presentan los conjuntos de cifrado actuales que admite Azure Front Door Service:

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
- TLS_DHE_RSA_WITH_AES_128_GCM_SHA256
- TLS_DHE_RSA_WITH_AES_256_GCM_SHA384

### <a name="does-azure-front-door-service-also-support-re-encryption-of-traffic-to-the-backend"></a>¿Admite Azure Front Door Service también volver a cifrar el tráfico dirigido al back-end?

Sí, Azure Front Door Service admite la descarga de SSL y SSL de extremo a extremo, lo cual permite volver a cifrar el tráfico dirigido al back-end. De hecho, puesto que las conexiones con el back-end se producen a través de la dirección IP pública, se recomienda que configure Front Door para que use HTTPS como protocolo de reenvío.

### <a name="can-i-configure-ssl-policy-to-control-ssl-protocol-versions"></a>¿Se puede configurar la directiva SSL para controlar las versiones del protocolo SSL?

Puede configurar una versión mínima de TLS en Azure Front Door a través de la [API de REST de Azure](https://docs.microsoft.com/rest/api/frontdoorservice/frontdoor/frontdoors/createorupdate#minimumtlsversion). Actualmente, puede elegir entre 1.0 y 1.2.

### <a name="can-i-configure-front-door-to-only-support-specific-cipher-suites"></a>¿Puedo configurar Front Door para admitir únicamente determinados conjuntos de cifrado?

No, no se admite la configuración de Front Door para determinados conjuntos de cifrado. 

## <a name="diagnostics-and-logging"></a>Diagnósticos y registro

### <a name="what-types-of-metrics-and-logs-are-available-with-azure-front-door-service"></a>¿Qué tipos de registros y métricas están disponibles con Azure Front Door Service?

Para obtener información sobre los registros y otras funcionalidades de diagnóstico, consulte [Monitoring metrics and logs for Front Door](front-door-diagnostics.md) (Supervisión de métricas y registros para Front Door).

### <a name="what-is-the-retention-policy-on-the-diagnostics-logs"></a>¿Qué es la directiva de retención en los registros de diagnóstico?

Los registros de diagnóstico fluyen hacia la cuenta de almacenamiento de los clientes y estos pueden establecer la directiva de retención según sus preferencias. Los registros de diagnóstico también se pueden enviar a un centro de eventos o a registros de Azure Monitor. Para más información, consulte [Azure Front Door Service Diagnostics](front-door-diagnostics.md) (Diagnósticos de Azure Front Door Service).

### <a name="how-do-i-get-audit-logs-for-azure-front-door-service"></a>¿Cómo puedo obtener registros de auditoría para Azure Front Door Service?

Los registros de auditoría están disponibles para Azure Front Door Service. En el portal, haga clic en **Registro de actividad** en la hoja de menú de su instancia de Front Door para acceder al registro de auditoría. 

### <a name="can-i-set-alerts-with-azure-front-door-service"></a>¿Puedo establecer alertas con Azure Front Door Service?

Sí, Azure Front Door Service admite las alertas. Las alertas se configuran en métricas. 

## <a name="next-steps"></a>Pasos siguientes

- Aprenda a [crear una instancia de Front Door](quickstart-create-front-door.md).
- Más información acerca de cómo [funciona Front Door](front-door-routing-architecture.md).
