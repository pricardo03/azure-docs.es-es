---
title: Información general de configuración de Azure Application Gateway
description: En este artículo se describe cómo configurar los componentes de Azure Application Gateway
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 4/30/2019
ms.author: absha
ms.openlocfilehash: 5bfd1f930c190e717e435856f424f0cdf80deb2c
ms.sourcegitcommit: ed66a704d8e2990df8aa160921b9b69d65c1d887
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2019
ms.locfileid: "64946806"
---
# <a name="application-gateway-configuration-overview"></a>Introducción a la configuración de Application Gateway

Azure Application Gateway consta de varios componentes que se pueden configurar de varias maneras para diferentes escenarios. Este artículo muestra cómo configurar cada componente.

![Diagrama de flujo de los componentes de puerta de enlace de aplicaciones](./media/configuration-overview/configuration-overview1.png)

Esta imagen muestra una aplicación que tiene tres agentes de escucha. Los dos primeros son agentes de escucha multisitio para `http://acme.com/*` y `http://fabrikam.com/*`, respectivamente. Ambos escuchan en el puerto 80. El tercero es un agente de escucha básico que tiene la terminación de la capa de Sockets seguros (SSL) de extremo a otro.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Requisitos previos

### <a name="azure-virtual-network-and-dedicated-subnet"></a>Azure virtual network y subred dedicada

Application gateway es una implementación dedicada en la red virtual. Dentro de la red virtual, una subred dedicada es necesaria para la puerta de enlace de la aplicación. Puede tener varias instancias de una implementación de puerta de enlace de aplicación determinada en una subred. También puede implementar otras puertas de enlace de aplicaciones en la subred. Pero no se puede implementar cualquier otro recurso en la subred de puerta de enlace de aplicaciones.

> [!NOTE]
> No se pueden mezclar Standard_v2 y estándar de Azure Application Gateway en la misma subred.

#### <a name="size-of-the-subnet"></a>Tamaño de la subred

Puerta de enlace de aplicación consume 1 dirección IP privada por cada instancia, además de otra dirección IP privada si se configura una dirección de IP de front-end privada.

Además, Azure reserva 5 direcciones IP en cada subred para uso interno: la primera versión 4 y el último direcciones IP. Por ejemplo, considere la posibilidad de 15 instancias de application gateway con ninguna dirección IP de front-end privada. Necesita al menos 20 direcciones IP para esta subred: 5 para uso interno y 15 para las instancias de puerta de enlace de aplicaciones. Por lo tanto, necesita un/27 subred, tamaño o mayor.

Considere la posibilidad de una subred que tiene 27 instancias de application gateway y una dirección IP para una dirección de IP de front-end privada. En este caso, es preciso 33 direcciones IP: 27 para las instancias de puerta de enlace de aplicaciones, 1 para el front-end privada y 5 para uso interno. Por lo tanto, necesita un /26 subred, tamaño o mayor.

Se recomienda que utilice un tamaño de la subred de al menos/28. Este tamaño le ofrece 11 direcciones IP utilizables. Si la carga de la aplicación requiere más de 10 direcciones IP, considere la posibilidad de/27 o/26 tamaño de la subred.

#### <a name="network-security-groups-on-the-application-gateway-subnet"></a>Grupos de seguridad de red en la subred de puerta de enlace de aplicaciones

Se admiten grupos de seguridad de red (NSG) en Application Gateway. Sin embargo, hay varias restricciones:

- Debe incluir las excepciones para el tráfico entrante en los puertos 65503-65534 para la SKU de puerta de enlace de aplicación v1 y los puertos 65200 65535 para el SKU v2. Este intervalo de puertos es necesario para la comunicación de la infraestructura de Azure. Estos puertos están protegidos (bloqueados) mediante certificados de Azure. Las entidades externas, incluidos a los clientes de esas puertas de enlace, no pueden iniciar los cambios en los puntos de conexión sin los certificados apropiados en su lugar.

- No puede bloquearse la conectividad saliente de Internet. Reglas de salida de forma predeterminada en lo NSG permiten la conectividad a internet. Se recomienda que:

  - No quite las reglas de salida predeterminado.
  - No se crean otras reglas de salida que deniegan la conectividad saliente de internet.

- El tráfico de la **AzureLoadBalancer** etiqueta debe estar permitida.

##### <a name="whitelist-application-gateway-access-to-a-few-source-ips"></a>Acceso de puerta de enlace de aplicación de lista blanca para algunas direcciones IP de origen

En este escenario, use los NSG en la subred de puerta de enlace de aplicaciones. Coloque las siguientes restricciones en la subred en este orden de prioridad:

1. Permitir el tráfico entrante desde un origen o intervalo IP.
2. Permitir las solicitudes entrantes de todos los orígenes a los puertos 65503-65534 para [comunicación de mantenimiento del back-end](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics). Este intervalo de puertos es necesario para la comunicación de la infraestructura de Azure. Estos puertos están protegidos (bloqueados) mediante certificados de Azure. Sin los certificados apropiados en su lugar, las entidades externas no pueden iniciar los cambios en esos puntos de conexión.
3. Permitir sondeos entrantes equilibrador de carga de Azure (*AzureLoadBalancer* etiqueta) y el tráfico de red virtual entrante (*VirtualNetwork* etiqueta) en el [grupo de seguridad de red](https://docs.microsoft.com/azure/virtual-network/security-overview).
4. Bloquear el resto del tráfico entrante mediante el uso de una regla denegar todo.
5. Permitir el tráfico saliente a Internet para todos los destinos.

#### <a name="user-defined-routes-supported-on-the-application-gateway-subnet"></a>Rutas definidas por el usuario admitidas en la subred de puerta de enlace de aplicaciones

La SKU v1, rutas definidas por el usuario (Udr) se admiten en la subred de Application Gateway, siempre y cuando no alteran comunicación solicitud-respuesta-to-end. Por ejemplo, puede establecer una UDR en la subred de puerta de enlace de aplicación para que apunte a un dispositivo de firewall para la inspección de paquetes. Pero debe asegurarse de que el paquete puede llegar a su destino después de la inspección. Si no lo hace podría producir un sondeo de estado incorrecto o el comportamiento de enrutamiento de tráfico. Esto incluye rutas aprendidas o las rutas de 0.0.0.0/0 predeterminadas que se propagan las puertas de enlace de ExpressRoute o VPN en la red virtual.

Para la SKU de v2, no se admiten las Udr en la subred de puerta de enlace de aplicaciones. Para obtener más información, consulte [SKU de puerta de enlace de aplicaciones de Azure v2](application-gateway-autoscaling-zone-redundant.md#differences-with-v1-sku).

> [!NOTE]
> Usar las Udr en la subred de puerta de enlace de aplicación hace que el estado de mantenimiento en el [vista mantenimiento del back-end](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics#back-end-health) aparezca como "Desconocido". También hace que la generación de registros de Application Gateway y las métricas de un error. Se recomienda no usar Udr en la subred de puerta de enlace de aplicación para que pueda ver el mantenimiento del back-end, registros y métricas.

## <a name="front-end-ip"></a>IP de front-end

Puede configurar la puerta de enlace de aplicaciones para tener una dirección IP pública, una dirección IP privada o ambos. Una dirección IP pública es necesaria al hospedar un back-end deben tener acceso los clientes a través de internet a través de una IP virtual (VIP) a través de internet. 

Una dirección IP pública no es necesaria para un punto de conexión interno no expuesto a internet. Que se conoce como un *equilibrador de carga interno* punto de conexión (ILB). Una puerta de enlace de la aplicación ILB es útil para aplicaciones de línea de negocio internas que no están expuestas a internet. También es útil para los servicios y niveles de una aplicación de varios niveles dentro de un límite de seguridad que no están expuestos a internet pero que requieren round-robin cargar distribución, permanencia de sesión o terminación SSL.

Se admite solo 1 dirección IP pública o 1 dirección IP privada. Elija la dirección IP front-end cuando se crea la puerta de enlace de la aplicación.

- Para una dirección IP pública, puede crear una nueva dirección IP pública o usar una dirección IP pública existente en la misma ubicación que la puerta de enlace de la aplicación. Si crea una nueva dirección de IP pública, el tipo de dirección IP que seleccionó (dinámico o estático) no se puede cambiar más adelante. Para obtener más información, consulte [estático frente a la dirección IP pública dinámica](https://docs.microsoft.com/azure/application-gateway/application-gateway-components#static-vs-dynamic-public-ip-address).

- Para una dirección IP privada, puede especificar una dirección IP privada de la subred donde se crea la puerta de enlace de la aplicación. Si no especifica ninguna, se selecciona automáticamente una dirección IP arbitraria de la subred. Para obtener más información, consulte [crear una puerta de enlace de aplicaciones con un equilibrador de carga interno](https://docs.microsoft.com/azure/application-gateway/application-gateway-ilb-arm).

Una dirección IP front-end está asociada a un *escucha*, que comprueba que las solicitudes entrantes en la dirección IP front-end.

## <a name="listeners"></a>Agentes de escucha

Un agente de escucha es una entidad lógica que comprueba las solicitudes de conexión entrantes mediante el puerto, protocolo, host y dirección IP. Al configurar el agente de escucha, debe especificar valores para los monitores que coinciden con los valores correspondientes en la solicitud entrante en la puerta de enlace.

Cuando se crea una instancia de application gateway mediante el portal de Azure, también crear un agente de escucha predeterminado eligiendo el protocolo y puerto del agente de escucha. Puede elegir si desea habilitar la compatibilidad con HTTP2 en el agente de escucha. Después de crear la puerta de enlace de la aplicación, puede editar la configuración de ese agente de escucha predeterminado (*appGatewayHttpListener*/*appGatewayHttpsListener*) o crear nuevos agentes de escucha.

### <a name="listener-type"></a>Tipo de agente de escucha

Cuando se crea un nuevo agente de escucha, elegir entre [ *básica* y *multisitio*](https://docs.microsoft.com/azure/application-gateway/application-gateway-components#types-of-listeners).

- Si está hospedando un único sitio detrás de una puerta de enlace de aplicaciones, elija básico. Obtenga información sobre [cómo crear una puerta de enlace de aplicaciones con un agente de escucha básico](https://docs.microsoft.com/azure/application-gateway/quick-create-portal).

- Si va a configurar más de una aplicación web o varios subdominios del mismo dominio primario en la misma instancia de puerta de enlace de aplicaciones, elija el agente de escucha multisitio. Para un agente de escucha multisitio, también debe especificar un nombre de host. Esto es porque la instancia de Application Gateway se basa en los encabezados de host HTTP 1.1 para hospedar más de un sitio Web en la misma dirección IP pública y el puerto.

#### <a name="order-of-processing-listeners"></a>Orden de procesamiento de los agentes de escucha

Para la SKU v1, los agentes de escucha se procesan en el orden en que aparecen. Si un agente de escucha coincide con una solicitud entrante, el agente de escucha procesa en primer lugar esa solicitud. Por lo tanto, configure los agentes de escucha multisitio antes de los agentes de escucha básicos para asegurarse de que el tráfico se enruta al back-end correcto.

Para la SKU de v2, se procesan los agentes de escucha multisitio antes de los agentes de escucha básicos.

### <a name="front-end-ip"></a>IP de front-end

Elija la dirección IP de front-end que va a asociar a este agente de escucha. El agente de escucha escuchará las solicitudes entrantes en esta dirección IP.

### <a name="front-end-port"></a>Puerto front-end

Elija el puerto front-end. Seleccione un puerto existente o cree uno nuevo. Elija un valor comprendido entre el [permiten el intervalo de puertos](https://docs.microsoft.com/azure/application-gateway/application-gateway-components#ports). Puede usar no solo los puertos conocidos, como 80 y 443, pero cualquier puerto permitido personalizado que es adecuado. Un puerto puede usarse para los agentes de escucha público o privado accesible desde los agentes de escucha.

### <a name="protocol"></a>Protocol

Elija HTTP o HTTPS:

- Si elige HTTP, se cifra el tráfico entre el cliente y la puerta de enlace de la aplicación.

- Seleccione HTTPS si desea [terminación SSL](https://docs.microsoft.com/azure/application-gateway/overview#secure-sockets-layer-ssl-terminationl) o [cifrado SSL de extremo a otro](https://docs.microsoft.com/azure/application-gateway/ssl-overview). Se cifra el tráfico entre el cliente y la puerta de enlace de la aplicación. Y termina la conexión SSL en application gateway. Si desea que el cifrado SSL de extremo a otro, debe elegir HTTPS y configurar el **HTTP de back-end** configuración. Esto garantiza que el tráfico se vuelve a cifrar cuando su tránsito de puerta de enlace de aplicaciones para el back-end.

Para configurar la terminación SSL y el cifrado SSL de extremo a otro, debe agregar un certificado para el agente de escucha para habilitar la puerta de enlace de aplicaciones derivar una clave simétrica. Esto viene determinado por la especificación del protocolo SSL. La clave simétrica se utiliza para cifrar y descifrar el tráfico que se envía a la puerta de enlace. El certificado de puerta de enlace debe estar en formato de intercambio de información Personal (PFX). Este formato permite exportar la clave privada que usa la puerta de enlace para cifrar y descifrar el tráfico.

#### <a name="supported-certificates"></a>Certificados admitidos

Consulte [certificados compatibles para la terminación SSL](https://docs.microsoft.com/azure/application-gateway/ssl-overview#certificates-supported-for-ssl-termination).

### <a name="additional-protocol-support"></a>Compatibilidad con protocolos adicionales

#### <a name="http2-support"></a>Compatibilidad con HTTP2

Compatibilidad con el protocolo HTTP/2 está disponible para los clientes que se conectan a sólo escuchas de puerta de enlace de aplicaciones. La comunicación entre grupos de servidores back-end es a través de HTTP/1.1. De forma predeterminada, HTTP/2 está deshabilitado. El siguiente fragmento de código de PowerShell de Azure muestra cómo habilitar esto:

```azurepowershell
$gw = Get-AzApplicationGateway -Name test -ResourceGroupName hm

$gw.EnableHttp2 = $true

Set-AzApplicationGateway -ApplicationGateway $gw
```

#### <a name="websocket-support"></a>Compatibilidad con WebSocket

Compatibilidad con WebSocket está habilitado de forma predeterminada. No hay ningún valor configurable por el usuario para habilitarla o deshabilitarla. Puede usar WebSockets con agentes de escucha HTTP y HTTPS.

### <a name="custom-error-pages"></a>Páginas de error personalizadas

Puede definir el error personalizado en el nivel global o el agente de escucha. Pero crear páginas de errores personalizados de nivel global desde el portal de Azure no se admite actualmente. Puede configurar una página de error personalizada para un error de firewall de aplicación 403 web o una página de 502 mantenimiento en el nivel de agente de escucha. También debe especificar una dirección URL de blob accesible públicamente para el código de estado de error determinado. Para más información, consulte [Create Application Gateway custom error pages](https://docs.microsoft.com/azure/application-gateway/custom-error) (Creación de páginas de error personalizadas de Application Gateway).

![Códigos de error de Application Gateway](https://docs.microsoft.com/azure/application-gateway/media/custom-error/ag-error-codes.png)

Para configurar una página de error personalizada global, consulte [configuración de Azure PowerShell](https://docs.microsoft.com/azure/application-gateway/custom-error#azure-powershell-configuration).

### <a name="ssl-policy"></a>Directiva SSL

Puede centralizar la administración de certificados SSL y reducir la sobrecarga de una granja de servidores back-end de cifrado descifrado. Control centralizado de SSL también le permite especificar una directiva SSL central que se adapte a sus requisitos de seguridad. Puede elegir *predeterminada*, *predefinidos*, o *personalizado* directiva SSL.

Configurar la directiva SSL para el control versiones del protocolo SSL. Puede configurar una puerta de enlace de aplicaciones para denegar TLS1.0, TLS1.1 y TLS1.2. De forma predeterminada, SSL 2.0 y 3.0 están deshabilitados y no son configurables. Para obtener más información, consulte [Introducción a la directiva SSL de puerta de enlace de aplicaciones](https://docs.microsoft.com/azure/application-gateway/application-gateway-ssl-policy-overview).

Después de crear un agente de escucha, asociarla a una regla de enrutamiento de solicitud. Esta regla determina cómo se enrutan las solicitudes que se reciben en el agente de escucha para el back-end.

## <a name="request-routing-rules"></a>Las reglas de enrutamiento de solicitud

Cuando se crea una instancia de application gateway mediante el portal de Azure, crear una regla predeterminada (*rule1*). Esta regla enlaza el agente de escucha predeterminado (*appGatewayHttpListener*) con el grupo de back-end predeterminado (*appGatewayBackendPool*) y la configuración de HTTP de back-end predeterminado ( *appGatewayBackendHttpSettings*). Después de crear la puerta de enlace, puede modificar la configuración de la regla predeterminada o crear nuevas reglas.

### <a name="rule-type"></a>Tipo de regla

Cuando se crea una regla, elegir entre [ *básica* y *basadas en rutas*](https://docs.microsoft.com/azure/application-gateway/application-gateway-components#request-routing-rule).

- Elija básico si desea reenviar todas las solicitudes en el agente de escucha asociado (por ejemplo, *blog<i></i>.contoso.com/\*)* a un único grupo de back-end.
- Elija la ruta de acceso basado en si desea enrutar las solicitudes de determinadas rutas de acceso de dirección URL a los grupos de back-end específicos. El patrón de ruta de acceso se aplica solo a la ruta de acceso de la dirección URL, no a sus parámetros de consulta.

#### <a name="order-of-processing-rules"></a>Orden de procesamiento de reglas

Para la SKU v1, coincidencia de patrones de las solicitudes entrantes se procesa en el orden en que se enumeran las rutas de acceso en el mapa de ruta de acceso de dirección URL de la regla de ruta de acceso. Si una solicitud coincide con el patrón en dos o más rutas de acceso en el mapa de ruta de acceso, la ruta de acceso que aparece en primer lugar se compara. Y la solicitud se reenvía al back-end que está asociado con esa ruta de acceso.

Para la SKU de v2, mayor prioridad que el orden de la ruta de acceso en el mapa de ruta de acceso de dirección URL es una coincidencia exacta. Si una solicitud coincide con el patrón en dos o más rutas de acceso, la solicitud se reenvía al back-end que está asociado con la ruta de acceso que coincida exactamente con la solicitud. Si la ruta de acceso en la solicitud entrante no coincide exactamente con cualquier ruta de acceso en el mapa, coincidencia de patrones de la solicitud se procesa en la lista de pedidos del mapa de ruta de acceso para la regla basada en ruta de acceso.

### <a name="associated-listener"></a>Agente de escucha asociado

Asociar un agente de escucha a la regla para que la *regla de enrutamiento de solicitud* que está asociado con el agente de escucha se evalúa para determinar el grupo de back-end para enrutar la solicitud.

### <a name="associated-back-end-pool"></a>Grupo de back-end asociado

Asociar el grupo de back-end que contiene los destinos de back-end que atender las solicitudes que recibe el agente de escucha a la regla.

 - Una regla básica, se permite un único grupo de back-end. Se reenvían todas las solicitudes en el agente de escucha asociado a ese grupo de back-end.

 - Para una regla basada en ruta de acceso, agregue varios grupos de back-end que corresponden a cada ruta de acceso URL. Las solicitudes que coinciden con la ruta de acceso de dirección URL que se ha especificado se reenvían al grupo de back-end correspondiente. Además, agregue un grupo de back-end de forma predeterminada. Las solicitudes que no coinciden con ninguna ruta de acceso URL en la regla se reenvían a ese grupo.

### <a name="associated-back-end-http-setting"></a>Configuración de HTTP de back-end asociado

Agregue una configuración de HTTP de back-end para cada regla. Las solicitudes se enrutan desde la puerta de enlace de aplicaciones a los destinos de back-end mediante el número de puerto, protocolo y otra información que se especifica en esta configuración.

Una regla básica, se permite solo una configuración de HTTP de back-end. Todas las solicitudes en el agente de escucha asociado se reenvían a los destinos de back-end correspondientes mediante esta configuración de HTTP.

Agregue una configuración de HTTP de back-end para cada regla. Las solicitudes se enrutan desde la puerta de enlace de aplicaciones a los destinos de back-end mediante el número de puerto, protocolo y otra información que se especifica en esta configuración.

Una regla básica, se permite solo una configuración de HTTP de back-end. Todas las solicitudes en el agente de escucha asociado se reenvían a los destinos de back-end correspondientes mediante esta configuración de HTTP.

Para una regla basada en ruta de acceso, agregue varias opciones de configuración de HTTP de back-end que corresponden a cada ruta de acceso URL. Las solicitudes que coinciden con la ruta de acceso de dirección URL en esta configuración se reenvían a los destinos de back-end correspondientes con la configuración de HTTP que corresponden a cada ruta de acceso URL. Además, agregue una configuración de HTTP de forma predeterminada. Las solicitudes que no coinciden con ninguna ruta de acceso URL en esta regla se reenvían al grupo de back-end predeterminado mediante el uso de la configuración de HTTP predeterminada.

### <a name="redirection-setting"></a>Configuración de redirección

Si la redirección se configura para una regla básica, se redirigen todas las solicitudes en el agente de escucha asociado al destino. Se trata de *global* redirección. Si la redirección se configura para una regla basada en ruta de acceso, se redirigen solamente las solicitudes en un área específica del sitio. Un ejemplo es un área de carro de la compra que se indica mediante */cart/\**. Se trata de *basadas en rutas* redirección.

Para obtener más información acerca de las redirecciones, vea [Introducción a la redirección de Application Gateway](https://docs.microsoft.com/azure/application-gateway/redirect-overview).

#### <a name="redirection-type"></a>Tipo de redirección

Elija el tipo de redirección mínimo: *Permanent(301)*, *Temporary(307)*, *Found(302)*, o *consulte other(303)*.

#### <a name="redirection-target"></a>Destino de la redirección

Elija otro agente de escucha o un sitio externo como el destino de la redirección.

##### <a name="listener"></a>Agente de escucha

Elegir el agente de escucha como el destino de la redirección para redirigir el tráfico de un agente de escucha a otro en la puerta de enlace. Esta configuración es necesaria cuando desea habilitar la redirección de HTTP a HTTPS. Redirige el tráfico desde el agente de escucha de origen que comprueba las solicitudes HTTP entrantes para el agente de escucha de destino que se comprueba para las solicitudes HTTPS entrantes. También puede incluir la cadena de consulta y la ruta de acceso de la solicitud original en la solicitud que se reenviará al destino de la redirección.

![Cuadro de diálogo de componentes de puerta de enlace de aplicaciones](./media/configuration-overview/configure-redirection.png)

Para obtener más información acerca de la redirección de HTTP a HTTPS, consulte:
- [Redirección de HTTP a HTTPS mediante el portal de Azure](https://docs.microsoft.com/azure/application-gateway/redirect-http-to-https-portal)
- [Redirección de HTTP a HTTPS mediante PowerShell](https://docs.microsoft.com/azure/application-gateway/redirect-http-to-https-powershell)
- [Redirección de HTTP a HTTPS mediante la CLI de Azure](https://docs.microsoft.com/azure/application-gateway/redirect-http-to-https-cli)

##### <a name="external-site"></a>Sitio externo

Elegir sitio externo al que desee redirigir el tráfico en el agente de escucha que está asociado con esta regla a un sitio externo. Puede incluir la cadena de consulta de la solicitud original en la solicitud que se reenviará al destino de la redirección. No se puede reenviar la ruta de acceso al sitio externo que se encontraba en la solicitud original.

Para obtener más información acerca de la redirección, consulte:
- [Redirigir el tráfico a un sitio externo mediante el uso de PowerShell](https://docs.microsoft.com/azure/application-gateway/redirect-external-site-powershell)
- [Redirigir el tráfico a un sitio externo mediante la CLI](https://docs.microsoft.com/azure/application-gateway/redirect-external-site-cli)

#### <a name="rewrite-the-http-header-setting"></a>Vuelva a escribir el valor del encabezado HTTP

Esta configuración agrega, quita o actualiza los encabezados de solicitud y respuesta HTTP mientras la solicitud y mover paquetes de respuesta entre el cliente y los grupos de back-end. Solo se puede configurar esta funcionalidad a través de PowerShell. Portal de Azure y compatibilidad con la CLI no están aún disponibles. Para más información, consulte:

 - [Vuelva a escribir la información general de los encabezados HTTP](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers)
 - [Configurar la reescritura del encabezado HTTP](https://docs.microsoft.com/azure/application-gateway/add-http-header-rewrite-rule-powershell#specify-your-http-header-rewrite-rule-configuration)

## <a name="http-settings"></a>Configuración HTTP

Application gateway enruta el tráfico a los servidores back-end con la configuración que especifique aquí. Después de crear una configuración de HTTP, debe asociarlo con una o varias reglas de enrutamiento de solicitud.

### <a name="cookie-based-affinity"></a>Afinidad basada en cookies

Esta característica es útil cuando desea mantener una sesión de usuario en el mismo servidor. Las cookies administrado por puerta de enlace permiten la aplicación puerta de enlace directo tráfico posterior desde una sesión de usuario en el mismo servidor para su procesamiento. Esto es importante cuando se guarda el estado de sesión localmente en el servidor para una sesión de usuario. Si la aplicación no puede controlar la afinidad basada en cookies, no se puede usar esta característica. Para ello, asegúrese de que los clientes admiten cookies.

### <a name="connection-draining"></a>Purga de la conexión

Purga de conexión le ayuda a quitar correctamente los miembros del grupo de back-end durante las actualizaciones de servicio planeada. Puede aplicar esta configuración a todos los miembros de un grupo de back-end durante la creación de reglas. Se asegura de que todas las instancias de anular el registro de un grupo de back-end no reciben todas las nuevas solicitudes. Mientras tanto, las solicitudes existentes puedan completarse dentro de un límite de tiempo configurado. Purga de conexión se aplica a instancias de back-end que se quitan explícitamente el grupo de back-end mediante una llamada API. También se aplica a las instancias de back-end que se notifican como *en mal estado* por el estado de los sondeos.

### <a name="protocol"></a>Protocol

Application Gateway admite HTTP y HTTPS para enrutar las solicitudes a los servidores back-end. Si elige HTTP, se cifra el tráfico a los servidores back-end. Si no es aceptable la comunicación sin cifrar, seleccione HTTPS.

Esta configuración se combina con HTTPS en el agente de escucha de es compatible con [SSL de extremo a extremo](https://docs.microsoft.com/azure/application-gateway/ssl-overview). Esto le permite transmitir de forma segura información confidencial cifrada al back-end. Cada servidor de back-end en el grupo de back-end con SSL de extremo a extremo habilitado debe configurarse con un certificado para permitir una comunicación segura.

### <a name="port"></a>Port

Esta configuración especifica el puerto donde escuchan los servidores back-end para el tráfico de la puerta de enlace de aplicaciones. Puede configurar los puertos comprendidos entre 1 y 65535.

### <a name="request-timeout"></a>Tiempo de espera de solicitud

Esta configuración es el número de segundos que la puerta de enlace de la aplicación espera recibir una respuesta desde el grupo de back-end antes de devolver un mensaje de error de "tiempo de espera de conexión".

### <a name="override-back-end-path"></a>Reemplazar la ruta de acceso de back-end

Esta configuración le permite configurar una ruta de reenvío personalizado opcional que se usará cuando la solicitud se reenvía al back-end. Cualquier parte de la ruta de acceso entrante que coincida con la ruta de acceso personalizada en el **invalidar la ruta de acceso de back-end** campo se copia en la ruta de acceso reenviado. En la tabla siguiente se muestra cómo funciona esta característica:

- Cuando la configuración de HTTP se adjunta a una regla básica de enrutamiento de solicitud:

  | Solicitud original  | Reemplazar la ruta de acceso de back-end | Solicitud que se reenvía al back-end |
  | ----------------- | --------------------- | ---------------------------- |
  | /home/            | /override/            | /override/home/              |
  | /home/secondhome/ | /override/            | /override/home/secondhome/   |

- Cuando la configuración de HTTP se adjunta a una regla de enrutamiento de solicitud basada en ruta de acceso:

  | Solicitud original           | Regla de ruta de acceso       | Reemplazar la ruta de acceso de back-end | Solicitud que se reenvía al back-end |
  | -------------------------- | --------------- | --------------------- | ---------------------------- |
  | /pathrule/home /            | /pathrule*      | /override/            | /override/home/              |
  | /pathrule/home/secondhome/ | /pathrule*      | /override/            | /override/home/secondhome/   |
  | /home/                     | /pathrule*      | /override/            | /override/home/              |
  | /home/secondhome/          | /pathrule*      | /override/            | /override/home/secondhome/   |
  | /pathrule/home /            | / pathrule/home * | /override/            | /override/                   |
  | /pathrule/home/secondhome/ | / pathrule/home * | /override/            | /override/secondhome/        |

### <a name="use-for-app-service"></a>Uso de app service

Se trata de un acceso directo de la interfaz de usuario que selecciona las dos configuraciones necesarias para el back-end de Azure App Service. Permite **Elegir nombre de host de direcciones de back-end**, y crea un nuevo sondeo personalizado. (Para obtener más información, consulte el [nombre de host de selección de direcciones de back-end](#pick) configuración de la sección de este artículo.) Se crea un nuevo sondeo y el encabezado de sondeo se tomó de la dirección del miembro de back-end.

### <a name="use-custom-probe"></a>Usar sondeo personalizado

Esta opción asocia un [sondeo personalizado](https://docs.microsoft.com/azure/application-gateway/application-gateway-probe-overview#custom-health-probe) con una configuración de HTTP. Puede asociar solo un sondeo personalizado con una configuración de HTTP. Si no asocia explícitamente un sondeo personalizado, el [sondeo predeterminado](https://docs.microsoft.com/azure/application-gateway/application-gateway-probe-overview#default-health-probe-settings) se usa para supervisar el estado de back-end. Le recomendamos que cree un sondeo personalizado para un mayor control sobre la supervisión de estado de los servidores back-end.

> [!NOTE]
> El sondeo personalizado no supervisa el estado del grupo de back-end a menos que la configuración de HTTP correspondiente se asocia explícitamente con un agente de escucha.

### <a id="pick"/></a>Seleccionar nombre de host de direcciones de back-end

Esta capacidad se establece dinámicamente el *host* encabezado en la solicitud para el nombre de host del grupo de back-end. Usa una dirección IP o FQDN.

Esta característica ayuda cuando el nombre de dominio de back-end es diferente del nombre DNS de la puerta de enlace de aplicaciones y el back-end se basa en un encabezado de host específico o una extensión de indicación de nombre de servidor (SNI) para resolver en el punto de conexión correcto.

Un caso de ejemplo es servicios multiinquilino como el back-end. Un app service es un servicio multiempresa que usa un espacio compartido con una única dirección IP. Por lo tanto, solo se puede acceder a un servicio de aplicaciones a través de los nombres de host que se configuran en la configuración del dominio personalizado.

De forma predeterminada, el nombre de dominio personalizado es *example.azurewebsites.<i> </i>net*. Para obtener acceso a su servicio de aplicaciones mediante el uso de una puerta de enlace de aplicaciones a través de un nombre de host que explícitamente no está registrado en el servicio de aplicación o el FQDN de la aplicación la puerta de enlace, reemplace el nombre de host en la solicitud original al nombre de host del servicio de aplicación. Para ello, habilite el **Elegir nombre de host de dirección de back-end** configuración.

Para un dominio personalizado se asigna cuyo nombre DNS personalizado a app service, no tienes que habilita a esta configuración.

> [!NOTE]
> Esta configuración no es necesaria para App Service Environment para PowerApps, que es una implementación dedicada.

### <a name="host-name-override"></a>Invalidación del nombre de host

Esta funcionalidad reemplaza la *host* encabezado en la solicitud entrante en la puerta de enlace de aplicaciones con el nombre de host que especifique.

Por ejemplo, si *www.contoso<i></i>.com* se especifica en el **nombre de Host** establecimiento, la solicitud original *https:/<i></i>/appgw.eastus.cloudapp.net/path1* cambia a *https:/<i></i>/www.contoso.com/path1* cuando la solicitud se reenvía al servidor back-end.

## <a name="back-end-pool"></a>Grupo de back-end

Puede designar un grupo de back-end para cuatro tipos de miembros de back-end: una máquina virtual específica, un conjunto de escalado de máquinas virtuales, un dirección IP o FQDN o un servicio de aplicaciones. Cada grupo de back-end puede apuntar a varios miembros del mismo tipo. No se admite la que apunta a los miembros de diferentes tipos en el mismo grupo de back-end.

Después de crear un grupo de back-end, debe asociarlo con una o varias reglas de enrutamiento de solicitud. También debe configurar los sondeos de estado para cada grupo de back-end en application gateway. Cuando se cumple una condición de regla de enrutamiento de solicitud, la puerta de enlace de aplicación reenvía el tráfico a los servidores en buen estado (según lo determina los sondeos de estado) en el grupo de back-end correspondiente.

## <a name="health-probes"></a>Sondeos de estado

Application gateway supervisa el estado de todos los recursos en su back-end de forma predeterminada. Pero se recomienda encarecidamente que cree un sondeo personalizado para cada configuración de HTTP de back-end obtener mayor control sobre la supervisión de estado. Para obtener información sobre cómo configurar un sondeo personalizado, consulte [configuración de sondeo de estado personalizada](https://docs.microsoft.com/azure/application-gateway/application-gateway-probe-overview#custom-health-probe-settings).

> [!NOTE]
> Después de crear un sondeo de estado personalizado, debe asociarlo a una configuración de HTTP de back-end. Un sondeo personalizado no supervisará el estado del grupo de back-end, a menos que la configuración de HTTP correspondiente se asocia explícitamente con un agente de escucha.

## <a name="next-steps"></a>Pasos siguientes

Ahora que sabe acerca de los componentes de Application Gateway, puede:

- [Crear una puerta de enlace de la aplicación en Azure portal](quick-create-portal.md)
- [Crear una instancia de application gateway mediante PowerShell](quick-create-powershell.md)
- [Crear una puerta de enlace de aplicaciones mediante la CLI de Azure](quick-create-cli.md)
