---
title: Información general de configuración de Azure Application Gateway
description: Este artículo proporciona información sobre cómo configurar los distintos componentes de Azure Application Gateway
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 03/20/2019
ms.author: absha
ms.openlocfilehash: ca4f9bf00d70f327ff756558e25315762a9a77a8
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2019
ms.locfileid: "58519755"
---
# <a name="application-gateway-configuration-overview"></a>Introducción a la configuración de Application Gateway

Puerta de enlace de aplicación consta de varios componentes que se pueden configurar de maneras diferentes para llevar a cabo diferentes escenarios. Este artículo le guiará a través de cómo cada componente se va a configurar.

![application-gateway-components](./media/configuration-overview/configuration-overview1.png)

La imagen del ejemplo anterior muestra la configuración de una aplicación con 3 agentes de escucha. Dos primeros son agentes de escucha multisitio para `http://acme.com/*` y `http://fabrikam.com/*`, respectivamente. Ambos están escuchando en el puerto 80. El agente de escucha terceros es un agente de escucha básico con terminación SSL de extremo a extremo. 

## <a name="prerequisites"></a>Requisitos previos

### <a name="azure-virtual-network-and-dedicated-subnet"></a>Azure virtual network y subred dedicada

Application gateway es una implementación dedicada en la red virtual. Dentro de la red virtual, una subred dedicada es necesaria para la puerta de enlace de la aplicación. Puede tener varias instancias de una implementación de puerta de enlace de aplicación determinado en esta subred. También puede implementar otras puertas de enlace de aplicaciones en la subred, pero no se puede implementar cualquier otro recurso en la subred de puerta de enlace de aplicaciones.  

> [!NOTE]   
> No se admite la combinación de Application Gateway Standard y Standard_v2 en la misma subred.

#### <a name="size-of-the-subnet"></a>Tamaño de la subred

Application Gateway consume una dirección IP privada por instancia, más otra dirección IP privada si se establece una configuración de dirección IP de front-end privada. Además, Azure reserva cinco direcciones IP: los primeros cuatro y última dirección IP - en cada subred para uso interno. Por ejemplo, si una puerta de enlace de aplicaciones se establece en 15 instancias y ninguna dirección IP de front-end privada, se requerirá al menos 20 direcciones IP en la subred: cinco direcciones IP para uso interno y 15 direcciones IP para las 15 instancias de application gateway. En este caso/27, por lo tanto, se necesita la subred, tamaño o mayor. Si tiene 27 instancias y una configuración de dirección IP del front-end privada IP, será necesarias - 33 direcciones IP 27 direcciones IP para el 27 instancias de application gateway, una dirección IP para la dirección IP privada de front-end y cinco direcciones para uso interno. En este caso un /26, por lo tanto, se necesita la subred, tamaño o mayor.

Se recomienda usar al menos una de/28 tamaño de la subred. Esto le ofrece 11 direcciones utilizables. Si la carga de la aplicación requiere más de 10 instancias, debe considerar un/27 o/26 tamaño de la subred.

#### <a name="network-security-groups-supported-on-the-application-gateway-subnet"></a>Grupos de seguridad de red compatible con la subred de puerta de enlace de aplicaciones

Se admiten grupos de seguridad de red (NSG) en la subred de puerta de enlace de aplicaciones con las restricciones siguientes: 

- Se deben colocar excepciones para el tráfico entrante en los puertos 65503-65534 para la SKU v1 de Application Gateway y los puertos 65200-65535 para la SKU v2. Este intervalo de puertos es necesario para la comunicación de la infraestructura de Azure. Están protegidos (bloqueados) mediante certificados de Azure. Sin los certificados apropiados, las entidades externas, incluidos los clientes de esas puertas de enlace, no podrán iniciar ningún cambio en esos puntos de conexión.

- No puede bloquearse la conectividad saliente de Internet. Reglas de salida de forma predeterminada en el NSG ya permiten la conectividad a internet. Se recomienda no quitar las reglas de salida predeterminado y que no se crean otras reglas de salida que deniegan la conectividad saliente de internet.

- Se debe permitir el tráfico de la etiqueta AzureLoadBalancer.

##### <a name="whitelist-application-gateway-access-to-a-few-source-ips"></a>Acceso de puerta de enlace de aplicación de lista blanca para algunas direcciones IP de origen

Este escenario puede realizarse mediante grupos de seguridad de red en la subred de la puerta de enlace de aplicaciones. Las siguientes restricciones se deben colocar en la subred en el orden de prioridad indicado:

1. Permitir el tráfico entrante de la IP o intervalo IP de origen.
2. Permitir las solicitudes entrantes de todos los orígenes a los puertos 65503-65534 para la [comunicación del estado del back-end](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics). Este intervalo de puertos es necesario para la comunicación de la infraestructura de Azure. Están protegidos (bloqueados) mediante certificados de Azure. Sin los certificados apropiados, las entidades externas, incluidos los clientes de esas puertas de enlace, no podrán iniciar ningún cambio en esos puntos de conexión.
3. Permitir sondeos entrantes de Azure Load Balancer (con la etiqueta AzureLoadBalancer) y el tráfico de red virtual entrante (con la etiqueta VirtualNetwork) en el [NSG](https://docs.microsoft.com/azure/virtual-network/security-overview).
4. Bloquear todo el tráfico entrante restante con una regla Denegar todo.
5. Permitir el tráfico saliente a Internet para todos los destinos.

#### <a name="user-defined-routes-supported-on-the-application-gateway-subnet"></a>Rutas definidas por el usuario admitidas en la subred de puerta de enlace de aplicaciones

En el caso de SKU v1, se admiten rutas definidas por el usuario (Udr) en la subred de puerta de enlace de aplicaciones, siempre que no alteran la comunicación solicitud-respuesta-to-end. Por ejemplo, puede configurar una UDR en la subred de la puerta de enlace de aplicaciones para que apunte a un dispositivo firewall para la inspección de paquetes, pero debe asegurarse de que el paquete puede llegar a su destino previsto después de la inspección. El no hacerlo podría resultar en un sondeo del estado incorrecto o en un comportamiento de enrutamiento de tráfico. Esto incluye las rutas aprendidas o las rutas 0.0.0.0/0 predeterminadas que propagan las puertas de enlace de VPN o ExpressRoute en la red virtual.

SKU, Udr en la subred de puerta de enlace de la aplicación no se admiten en el caso de v2. Para más información, consulte [Escalabilidad automática y puerta de enlace de aplicaciones con redundancia de zona (versión preliminar pública)](https://docs.microsoft.com/azure/application-gateway/application-gateway-autoscaling-zone-redundant#known-issues-and-limitations).

> [!NOTE]
> Usar las Udr en la subred de puerta de enlace de aplicación hará que el estado de mantenimiento en el [vista de estado de back-end](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics#back-end-health) que se mostrará como **desconocido** y también generará el error de generación de registros de puerta de enlace de aplicaciones y métricas. Se recomienda que no usar Udr en la subred de puerta de enlace de aplicaciones para poder ver el estado de back-end, registros y métricas.

## <a name="frontend-ip"></a>IP de front-end

Puede configurar la puerta de enlace de aplicaciones para tener una dirección IP pública o una dirección IP privada o ambos. Dirección IP pública es necesaria cuando se hospeda un back-end que se necesita tener acceso a los clientes a través de internet a través de una VIP accesible desde Internet. Dirección IP pública no es necesaria para un punto de conexión interno no expuesto a Internet, también conocido como un punto de conexión de (ILB) de equilibrador de carga interno. La configuración de la puerta de enlace con un ILB es útil para aplicaciones de línea de negocio internas no expuestas a Internet. También es útil para los distintos servicios y niveles de una aplicación de niveles múltiples que se encuentran dentro de un límite de seguridad no expuesto a Internet, pero que aún así siguen necesitando distribución de carga round robin, permanencia de sesión o terminación SSL (Capa de sockets seguros).

Se admite una única dirección IP pública o una dirección IP privada. Elija la dirección IP de front-end al crear la puerta de enlace de la aplicación. 

- En el caso de una dirección IP pública, puede elegir crear una nueva dirección IP pública o usar una dirección IP pública existente en la misma ubicación que la puerta de enlace de la aplicación. Si crea una nueva dirección IP pública, el tipo de dirección IP seleccionado (estática o dinámica) no se puede cambiar más adelante. Para obtener más información, consulte [puertos estáticos frente a la dirección IP pública dinámica](https://docs.microsoft.com/azure/application-gateway/application-gateway-components) 

- En el caso de una dirección IP privada, puede elegir especificar una dirección IP privada de la subred en la que se creó la puerta de enlace de la aplicación. Si no especifica explícitamente, se seleccionará automáticamente una dirección IP arbitraria desde la subred. Para obtener más información, consulte [crear una puerta de enlace de la aplicación con un extremo (ILB) de equilibrador de carga interno.](https://docs.microsoft.com/azure/application-gateway/application-gateway-ilb-arm)

Está asociado a un Frontend IP *escucha* que comprueba que las solicitudes entrantes en el Frontend IP.

## <a name="listeners"></a>Agentes de escucha

Un agente de escucha es una entidad lógica que comprueba las solicitudes entrantes de conexión, utilizando el puerto, protocolo, host y dirección IP. Por lo tanto, cuando se configura el agente de escucha que necesita escribir esos valores de puerto, protocolo, host y dirección IP que son los mismos que los valores correspondientes en la solicitud entrante en la puerta de enlace. Cuando se crea una instancia de application gateway mediante Azure portal, también crear un agente de escucha predeterminado eligiendo el protocolo y puerto del agente de escucha. Además, puede elegir si desea habilitar la compatibilidad con HTTP2 en el agente de escucha. Una vez creada la puerta de enlace de la aplicación, puede editar la configuración de este agente de escucha predeterminado (*appGatewayHttpListener*/*appGatewayHttpsListener*) o crear nuevos agentes de escucha.

### <a name="listener-type"></a>Tipo de agente de escucha

Puede elegir entre [agente de escucha básico o multisitio](https://docs.microsoft.com/azure/application-gateway/application-gateway-components#types-of-listeners) al crear un nuevo agente de escucha. 

- Si hospeda un único sitio detrás de una puerta de enlace de aplicaciones, elija el agente de escucha básico. Obtenga información sobre [cómo crear una puerta de enlace de aplicaciones con el agente de escucha básico](https://docs.microsoft.com/azure/application-gateway/quick-create-portal).

- Si va a configurar más de una aplicación web o varios subdominios del mismo dominio primario en la misma instancia de puerta de enlace de aplicaciones, a continuación, elija el agente de escucha multisitio. Para el agente de escucha multisitio, además debe especificar un nombre de host. Esto es porque la instancia de Application Gateway se basa en los encabezados de host HTTP 1.1 para hospedar más de un sitio Web en la misma dirección IP pública y el puerto.

#### <a name="order-of-processing-listeners"></a>Orden de procesamiento de los agentes de escucha

En el caso de las SKU v1, los agentes de escucha se procesan en el orden en que se muestran. Por este motivo, si un agente de escucha coincide con una solicitud entrante, se procesa primero. Por lo tanto, los agentes de escucha multisitio deben configurarse antes de un agente de escucha básico para asegurarse de que el tráfico se enruta al back-end correcto.

En el caso de las SKU de v2, se procesan los agentes de escucha multisitio antes que los agentes de escucha básicos.

### <a name="frontend-ip"></a>IP de front-end

Elija la dirección IP de front-end que se va a asociar a este agente de escucha. El agente de escucha escuchará a la solicitud entrante en esta dirección IP.

### <a name="frontend-port"></a>Puerto front-end

Elija el puerto de front-end. Puede elegir entre los puertos existentes o crear uno nuevo. Puede elegir un valor comprendido entre el [permiten el intervalo de puertos](https://docs.microsoft.com/azure/application-gateway/application-gateway-components#ports). Esto permite no sólo usa puertos conocidos, como 80 y 443, pero cualquiera permitió el puerto personalizado adecuado para su uso. Puede usar un puerto para los agentes de escucha orientados al públicos o privados accesible desde agentes de escucha.

### <a name="protocol"></a>Protocolo

Debe elegir entre los protocolos HTTP y HTTPS. 

- Si elige HTTP, fluirá el tráfico entre la puerta de enlace de cliente y la aplicación sin cifrar.

- Seleccione HTTPS si está interesado en [terminación de la capa de Sockets seguros (SSL)](https://docs.microsoft.com/azure/application-gateway/overview) o [cifrado SSL de extremo a extremo](https://docs.microsoft.com/azure/application-gateway/ssl-overview). Si selecciona HTTPS, se cifrarán el tráfico entre la puerta de enlace de cliente y la aplicación y se terminará la conexión SSL en application gateway.  Si desea que el cifrado SSL de extremo a extremo, además debe elegir el protocolo HTTPS al configurar *configuración HTTP de back-end*. Esto garantizará que el tráfico se vuelve a cifrar cuando su tránsito de puerta de enlace de aplicaciones back-end.

  Para configurar el cifrado SSL de extremo a extremo y terminación de la capa de Sockets seguros (SSL), se requiere un certificado que se agregarán a la escucha con el fin de habilitar la puerta de enlace de aplicaciones derivar una clave simétrica según la especificación del protocolo SSL. A continuación, se usa la clave simétrica para cifrar y descifrar el tráfico enviado a la puerta de enlace. El certificado de la puerta de enlace debe estar en formato de Intercambio de información personal (PFX). Este formato de archivo permite la exportación de la clave privada, lo que es necesario para que la puerta de enlace de aplicaciones pueda realizar el cifrado y descifrado del tráfico. 

#### <a name="supported-certificates"></a>Certificados admitidos

Consulte [certificados compatibles para la terminación SSL](https://docs.microsoft.com/azure/application-gateway/ssl-overview#certificates-supported-for-ssl-termination).

### <a name="additional-protocol-support"></a>Compatibilidad con protocolos adicionales

#### <a name="http2-support"></a>Compatibilidad con HTTP2

La compatibilidad con el protocolo HTTP/2 está disponible únicamente para los clientes que se conectan a los agentes de escucha de la puerta de aplicaciones. La comunicación con granjas de servidores back-end es a través de HTTP/1.1. De forma predeterminada, HTTP/2 está deshabilitado. El siguiente fragmento de código de Azure PowerShell de ejemplo muestra cómo habilitarlo:

```azurepowershell
$gw = Get-AzureRmApplicationGateway -Name test -ResourceGroupName hm

$gw.EnableHttp2 = $true

Set-AzureRmApplicationGateway -ApplicationGateway $gw
```

#### <a name="websocket-support"></a>Compatibilidad con Websocket

Compatibilidad con Websocket está habilitado de forma predeterminada. No hay ninguna opción de configuración que permita al usuario habilitar o deshabilitar la compatibilidad con WebSocket. Puede usar WebSockets con agentes de escucha HTTP y HTTPS. 

### <a name="custom-error-page"></a>Página de Error personalizada

Páginas de errores personalizados se pueden definir en el nivel global, así como el nivel de agente de escucha, sin embargo, crear páginas de errores personalizados de nivel global desde el portal de Azure no se admite actualmente. Puede configurar una página de error personalizado para un error 403 de WAF o una página de mantenimiento 502 en el nivel de agente de escucha. También debe especificar una dirección URL de blob accesible públicamente para el código de estado de error determinado. Para obtener más información, consulte [Crear página de error personalizada](https://docs.microsoft.com/azure/application-gateway/custom-error).

![Códigos de error de Application Gateway](https://docs.microsoft.com/azure/application-gateway/media/custom-error/ag-error-codes.png)

Para configurar una página de error personalizada global, use [Azure PowerShell para la configuración](https://docs.microsoft.com/azure/application-gateway/custom-error#azure-powershell-configuration) 

### <a name="ssl-policy"></a>Directiva SSL

Puede centralizar la administración de certificados SSL y reducir la sobrecarga de cifrado y descifrado de una granja de servidores back-end. Este control centralizado de SSL también le permite especificar una directiva SSL central que se adapte a los requisitos de seguridad de la organización.  Puede elegir entre definidos de forma predeterminada y directiva SSL personalizada. 

Puede configurar la directiva SSL para controlar las versiones del protocolo SSL. Puede configurar la puerta de enlace de aplicaciones para denegar TLS1.0, TLS1.1 y TLS1.2. SSL 2.0 y 3.0 ya están deshabilitados de forma predeterminada y no se pueden configurar. Para obtener más información, consulte [Introducción a la directiva SSL de puerta de enlace de aplicaciones](https://docs.microsoft.com/azure/application-gateway/application-gateway-ssl-policy-overview).

Después de crear un agente de escucha, asocia con una regla de enrutamiento de solicitud que determina cómo se la solicitud recibida en el agente de escucha se enrute al back-end.

## <a name="request-routing-rule"></a>Regla de enrutamiento de solicitud

Al crear la puerta de enlace de aplicaciones mediante Azure portal, crea una regla predeterminada (*rule1*), que enlaza el agente de escucha predeterminado (*appGatewayHttpListener*) con el grupo de back-end predeterminado (*appGatewayBackendPool*) y la configuración de back-end HTTP de forma predeterminada (*appGatewayBackendHttpSettings*). Una vez creada la puerta de enlace de aplicaciones, puede modificar la configuración de esta regla predeterminada o crear nuevas reglas.

### <a name="rule-type"></a>Tipo de regla

Puede elegir entre [regla básica o basada en ruta de acceso](https://docs.microsoft.com/azure/application-gateway/application-gateway-components#request-routing-rule) al crear una nueva regla. 

- Si desea reenviar todas las solicitudes en el agente de escucha asociado (p. ej.: blog.contoso.com/*) a un grupo de back-end único, elija el agente de escucha básico. 
- Elija la escucha de ruta de acceso si desea enrutar las solicitudes con ruta de acceso de dirección URL específica a los grupos de back-end específico. El patrón de ruta de acceso se aplica solo a la ruta de acceso de la dirección URL, no a sus parámetros de consulta.


#### <a name="order-of-processing-rules"></a>Orden de procesamiento de reglas

En el caso de las SKU v1, coincidencia de patrón de la solicitud entrante se procesa en el orden en que se enumeran las rutas de acceso en el mapa de ruta de acceso de dirección URL de la regla de ruta de acceso. Por ese motivo, si una solicitud coincide con el patrón en dos o más rutas de acceso en el mapa de ruta de acceso de dirección URL, a continuación, en primer lugar se buscará la ruta de acceso que se enumera y se reenviará la solicitud al back-end asociado con esa ruta de acceso.

En el caso de las SKU de v2, una coincidencia exacta mantiene prioridad sobre el orden en que se enumeran las rutas de acceso en el mapa de ruta de acceso de dirección URL. Para ese motivo, si una solicitud coincide con el patrón en dos o más rutas de acceso, a continuación, se reenviará la solicitud al back-end asociado con esa ruta de acceso que coincide exactamente con la solicitud. Si la ruta de acceso en la solicitud entrante coincide exactamente cualquier ruta de acceso en el mapa de ruta de acceso de dirección URL, a continuación, coincidencia de patrón de la solicitud entrante se procesa en el orden en que se enumeran las rutas de acceso en el mapa de ruta de acceso de dirección URL de la regla de ruta de acceso.

### <a name="associated-listener"></a>Agente de escucha asociado

Debe asociar un agente de escucha a la regla para que la *regla de enrutamiento de solicitud* asociado con el *escucha* se evalúa para determinar el *grupo back-end* a la que el solicitud es para enrutar.

### <a name="associated-backend-pool"></a>Asociado grupo de back-end

Asocie el grupo de back-end que contiene los destinos de back-end que atenderá las solicitudes recibidas por el agente de escucha. En el caso de una regla básica, un único grupo de back-end se permite porque se reenviará todas las solicitudes en el agente de escucha asociado a este grupo de back-end. En el caso de una regla basada en ruta de acceso, agregue varios grupos de back-end correspondiente a cada ruta de acceso URL. Las solicitudes que coinciden con la ruta de acceso de dirección URL que escribió aquí, se reenviará al grupo de back-end correspondiente. Además, puede agregar un grupo de back-end predeterminado puesto que las solicitudes que no coinciden con cualquier ruta de acceso de dirección URL especificada en esta regla se reenviará a él.

### <a name="associated-backend-http-setting"></a>Configuración de HTTP asociado back-end

Agregue una configuración de HTTP de back-end para cada regla. Las solicitudes se enrutarán desde la puerta de enlace de aplicaciones a los destinos de back-end con el número de puerto, protocolo y otras opciones especificadas en esta configuración. En el caso de una regla básica, solo una configuración de back-end HTTP se permite porque se reenviará todas las solicitudes en el agente de escucha asociado a los destinos de back-end correspondiente mediante esta configuración de HTTP. En el caso de una regla basada en ruta de acceso, agregue varias opciones de configuración de back-end HTTP correspondiente a cada ruta de acceso URL. Las solicitudes que coinciden con la ruta de acceso de dirección URL que escribió aquí, se reenviará a los destinos de back-end correspondiente mediante la configuración de HTTP correspondiente a cada ruta de acceso URL. Además, puede agregar una configuración de HTTP predeterminada ya que las solicitudes que no coinciden con cualquier ruta de acceso de dirección URL especificada en esta regla se reenviará al grupo de back-end predeterminado mediante la configuración de HTTP de forma predeterminada.

### <a name="redirection-setting"></a>Configuración de redirección

Si la redirección se configura para una regla básica, se redirigirán todas las solicitudes en el agente de escucha asociado al destino de redirección, lo que permite la redirección global. Si la redirección se configura para una regla basada en ruta de acceso, las solicitudes solo en un área de sitio específico, por ejemplo un carro de la compra área que se indica mediante/carro / *, será redirigido al destino de redirección, lo que permite la redirección basada en ruta de acceso. 

Para obtener información sobre la funcionalidad de redirección, consulte [Introducción al redireccionamiento](https://docs.microsoft.com/azure/application-gateway/redirect-overview).

- #### <a name="redirection-type"></a>Tipo de redirección

  Elija el tipo de redirección de mínimo de: Other(303) Permanent(301), Temporary(307), Found(302) o vea.

- #### <a name="redirection-target"></a>Destino de la redirección

  Puede elegir entre otro agente de escucha o un sitio externo como destino de la redirección. 

  - ##### <a name="listener"></a>Agente de escucha

    Le ayuda elegir la opción de agente de escucha como el destino de la redirección de redirección de un agente de escucha a otro agente de escucha en la puerta de enlace. Esta configuración es necesaria cuando desea habilitar HTTP al redireccionamiento de HTTPS, es decir, el tráfico de redirección desde el agente de escucha de origen buscando las solicitudes HTTP entrantes para el agente de escucha de destino buscando las solicitudes HTTPS entrantes. También puede elegir la cadena de consulta y la ruta de acceso en la solicitud original que se incluirán en la solicitud que se reenviarán al destino de la redirección.![application-gateway-components](./media/configuration-overview/configure-redirection.png)

    Para obtener más información sobre HTTP a la redirección de HTTPS, consulte [mediante el portal de redirección de HTTP a](https://docs.microsoft.com/azure/application-gateway/redirect-http-to-https-portal), [mediante PowerShell de redirección de HTTP a](https://docs.microsoft.com/azure/application-gateway/redirect-http-to-https-powershell), [mediante la CLI de redirección de HTTP a](https://docs.microsoft.com/azure/application-gateway/redirect-http-to-https-cli)

  - ##### <a name="external-site"></a>Sitio externo

    Elegir sitio externo al que desee redirigir el tráfico en el agente de escucha asociada con la regla, por tanto, se redirige a un sitio externo. Puede elegir la cadena de consulta en la solicitud original que se incluirán en la solicitud que se reenviarán al destino de la redirección. No se puede reenviar la ruta de acceso en la solicitud original al sitio externo.

    Para obtener más información sobre la redirección al sitio externo, consulte [redirigir el tráfico a un sitio externo mediante PowerShell](https://docs.microsoft.com/azure/application-gateway/redirect-external-site-powershell) y [https://docs.microsoft.com/azure/application-gateway/redirect-external-site-cli](https://docs.microsoft.com/azure/application-gateway/redirect-external-site-cli)

#### <a name="rewrite-http-header-setting"></a>Vuelva a escribir la configuración del encabezado HTTP

Esta funcionalidad le permite agregar, quitar o actualizar los encabezados de solicitud y respuesta HTTP mientras la solicitud y los paquetes de respuesta se mueven entre los grupos de cliente y el back-end.    Puede configurar esta funcionalidad solo a través de PowerShell. Soporte técnico CLI y el portal aún no está disponible. Para obtener más información, consulte [encabezados HTTP reescribir](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers) información general y [reescritura del encabezado HTTP configurar](https://docs.microsoft.com/azure/application-gateway/add-http-header-rewrite-rule-powershell#specify-your-http-header-rewrite-rule-configuration).

## <a name="http-settings"></a>Configuración HTTP

Application gateway enruta el tráfico a los servidores de back-end mediante la configuración especificada en este componente. Una vez que cree una configuración de HTTP, deberá asociar uno o más reglas de enrutamiento de solicitud.

### <a name="cookie-based-affinity"></a>Afinidad basada en cookies

Esta característica es útil cuando desea mantener una sesión de usuario en el mismo servidor. Mediante el uso de cookies administradas por la puerta de enlace, Application Gateway puede dirigir el tráfico posterior de una sesión de usuario hasta el mismo servidor para su procesamiento. Esto es importante en aquellos casos en los que se guarda el estado de la sesión de forma local en el servidor para una sesión de usuario. Si la aplicación no puede controlar la afinidad basada en cookies, a continuación, no podrá utilizar esta capacidad. Para usar la afinidad de sesión basada en cookies, debe asegurarse de que los clientes deben admitir cookies. 

### <a name="connection-draining"></a>Purga de la conexión

La purga de conexión ayuda a lograr la correcta eliminación de miembros del grupo de back-end durante las actualizaciones de servicio planeadas. Esta configuración se puede aplicar a todos los miembros de un grupo de back-end durante la creación de reglas. Una vez habilitado, application gateway garantiza que todas las instancias de anular el registro de un grupo de back-end no reciben todas las nuevas solicitudes al tiempo que permite que las solicitudes existentes se completa dentro de un límite de tiempo configurado. Esto se aplica a las dos instancias de back-end que se quitan explícitamente del grupo de back-end mediante una llamada API, así como las instancias de back-end que se notifican como incorrectas según determinan los sondeos de estado.

### <a name="protocol"></a>Protocolo

Application gateway admite los protocolos HTTP y HTTPS para enrutar las solicitudes a los servidores back-end. Si se elige el protocolo HTTP, a continuación, flujos de tráfico sin cifrar a los servidores back-end. En esos casos donde la comunicación sin cifrar en los servidores back-end no es una opción aceptable, debe elegir el protocolo HTTPS. Esta opción junto con la elección de protocolo HTTPS en el agente de escucha permite habilitar [SSL de extremo a extremo](https://docs.microsoft.com/azure/application-gateway/ssl-overview). Que permite transmitir de forma segura información confidencial cifrada back-end. Cada servidor back-end del grupo de back-end con SSL de extremo a extremo habilitado debe configurarse con un certificado para permitir la comunicación segura.

### <a name="port"></a>Port

Éste es el puerto que los servidores back-end están escuchando el tráfico procedente de la puerta de enlace de la aplicación. Puede configurar los puertos comprendidos entre 1 y 65535.

### <a name="request-timeout"></a>Tiempo de espera de solicitud

El número de segundos que la puerta de enlace de la aplicación espera hasta recibir respuesta desde el grupo de back-end antes de devolver un error de "Tiempo de espera agotado para esta conexión".

### <a name="override-backend-path"></a>Reemplazar la ruta de acceso del back-end

Esta configuración le permite configurar una ruta de reenvío personalizado opcional que se usará cuando la solicitud se reenvía al back-end. Esto copiará cualquier parte de la ruta de acceso entrante que coincida con la ruta de acceso personalizada especificada en el **invalidar la ruta de acceso de back-end** campo a la ruta de acceso reenviado. Consulte la tabla siguiente para comprender el funcionamiento de la capacidad.

- Cuando la configuración de HTTP se adjunta a una regla de enrutamiento de solicitud básica:

  | Solicitud original  | Reemplazar la ruta de acceso del back-end | Solicitud que se reenvía al back-end |
  | ----------------- | --------------------- | ---------------------------- |
  | /home/            | /override/            | /override/home/              |
  | /home/secondhome/ | /override/            | /override/home/secondhome/   |

- Cuando la configuración de HTTP se adjunta a una regla de enrutamiento de solicitud basada en ruta de acceso:

  | Solicitud original           | Regla de ruta de acceso       | Reemplazar la ruta de acceso del back-end | Solicitud que se reenvía al back-end |
  | -------------------------- | --------------- | --------------------- | ---------------------------- |
  | /pathrule/home /            | /pathrule*      | /override/            | /override/home/              |
  | /pathrule/home/secondhome/ | /pathrule*      | /override/            | /override/home/secondhome/   |
  | /home/                     | /pathrule*      | /override/            | /override/home/              |
  | /home/secondhome/          | /pathrule*      | /override/            | /override/home/secondhome/   |
  | /pathrule/home /            | / pathrule/home * | /override/            | /override/                   |
  | /pathrule/home/secondhome/ | / pathrule/home * | /override/            | /override/secondhome/        |

### <a name="use-for-app-service"></a>Uso para App Service

Este es un acceso directo de la interfaz de usuario que selecciona las dos configuraciones necesarias para el back-end de App service: permite seleccionar el nombre de host de dirección de back-end y crea un nuevo sondeo personalizado. Se explica el motivo por qué se hace primero en la sección para **Elegir nombre de host de dirección de back-end** configuración. Se crea un sondeo nuevo donde el encabezado de sondeo también se tomó de la dirección del miembro de back-end.

### <a name="use-custom-probe"></a>Usar sondeo personalizado

Esta configuración se utiliza para asociar un [sondeo personalizado](https://docs.microsoft.com/azure/application-gateway/application-gateway-probe-overview#custom-health-probe) con esta configuración de HTTP. Puede asociar solo un sondeo personalizado con una configuración de HTTP. Si explícitamente no asociar un sondeo personalizado, a continuación, [sondeo predeterminado](https://docs.microsoft.com/azure/application-gateway/application-gateway-probe-overview#default-health-probe-settings) se utilizará para supervisar el estado de back-end. Se recomienda que cree un sondeo personalizado para un control más específico sobre la supervisión de estado de los back-end.

> [!NOTE]   
> Sondeo personalizado no iniciará la supervisión del estado del grupo de back-end, a menos que la configuración de HTTP correspondiente se asocia explícitamente con un agente de escucha.

### <a name="pick-host-name-from-backend-address"></a>Seleccionar el nombre de host de la dirección de back-end

Esta capacidad se establece dinámicamente el *host* encabezado en la solicitud para el nombre de host del grupo de back-end con una dirección IP o nombre de dominio completo (FQDN). Esto resulta útil en los escenarios donde el nombre de dominio de back-end es diferente del nombre DNS de la puerta de enlace de aplicaciones y el back-end se basa en un encabezado de host específico o una extensión SNI para resolver en el punto de conexión correcto, como en el caso de los servicios multiinquilino como el back-end. Puesto que App service es un servicio multiempresa con un espacio compartido con una única dirección IP, se puede acceder a un servicio de aplicaciones solo con los nombres de host configurado en la configuración de dominio personalizado. De forma predeterminada, el nombre de dominio personalizado es *example.azurewebsites.net*. Por lo tanto, si desea tener acceso al servicio de aplicación con application gateway, ya sea un nombre de host registrado no explícitamente en App service o con el FQDN de la puerta de enlace de aplicaciones que tienen que invalidar el nombre de host en la solicitud original al nombre de host del servicio de aplicación, por habilitar **Elegir nombre de host de dirección de back-end** configuración.

Si posee un dominio personalizado y ha asignado el nombre DNS personalizado existente a App service, no es necesario habilitar esta configuración.

> [!NOTE]   
> Esta configuración no es necesaria para App Service Environment (ASE) dado que ASE es una implementación dedicada. 

### <a name="host-name-override"></a>Invalidación del nombre de host

Esta funcionalidad reemplaza la *host* encabezado en la solicitud entrante en la puerta de enlace de aplicaciones para el nombre de host que especifique aquí. Por ejemplo, si www\.contoso.com se especifica como el **nombre de Host** establecimiento, la solicitud original https://appgw.eastus.cloudapp.net/path1 cambiará a https://www.contoso.com/path1 cuando la solicitud se reenvía al servidor back-end. 

## <a name="backend-pool"></a>Grupo back-end

Un grupo de back-end se puede apuntar a cuatro tipos de miembros de back-end: un app service, un dirección IP o FQDN, conjunto de escalado de máquina virtual o una máquina virtual específica. Cada grupo de back-end puede apuntar a varios miembros del mismo tipo. No es compatible con la que apunta a los miembros de diferentes tipos en el mismo grupo de back-end. 

Después de crear un grupo de back-end, deberá asociarla a una o varias reglas de enrutamiento de solicitud. También deberá configurar los sondeos de estado para cada grupo de back-end en la puerta de enlace de la aplicación. Cuando se cumple una condición de regla de enrutamiento de solicitud, la puerta de enlace de aplicación reenvía el tráfico a los servidores en buen estado (según lo determina los sondeos de estado) en el grupo de back-end correspondiente.

## <a name="health-probes"></a>Sondeos de estado

Aunque la instancia de application gateway supervisa el estado de todos los recursos en su back-end de forma predeterminada, se recomienda que crear un sondeo personalizado para cada configuración de HTTP de back-end con el fin de tener un control más específico sobre la supervisión de estado. Para obtener información sobre cómo configurar el sondeo de estado personalizado, consulte [configuración de sondeo de estado personalizada](https://docs.microsoft.com/azure/application-gateway/application-gateway-probe-overview#custom-health-probe-settings).

> [!NOTE]   
> Una vez que cree un sondeo de estado personalizado, debe asociarlo a una configuración de HTTP de back-end. Sondeo personalizado no iniciará la supervisión del estado del grupo de back-end, a menos que la configuración de HTTP correspondiente se asocia explícitamente con un agente de escucha.

## <a name="next-steps"></a>Pasos siguientes

Después de conocer los componentes de Application Gateway, hacer lo siguiente:

- [Crear una puerta de enlace de la aplicación en Azure portal](quick-create-portal.md)
- [Crear una instancia de Application Gateway mediante PowerShell](quick-create-powershell.md)
- [Crear una instancia de Application Gateway mediante CLI de Azure](quick-create-cli.md)
