---
title: Componentes de la puerta de enlace de aplicaciones
description: En este artículo proporciona información sobre los distintos componentes de application gateway
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 02/20/2019
ms.author: absha
ms.openlocfilehash: f5dfa34760bcef23bf54d65b35e3ad8f48cc2ee5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60831847"
---
# <a name="application-gateway-components"></a>Componentes de la puerta de enlace de aplicaciones

 Application gateway actúa como el único punto de contacto para los clientes. Distribuye el tráfico entrante de aplicación en varios grupos de back-end, que incluyen Azure Virtual Machines, conjuntos de escalado de máquinas virtuales, Azure App Service y servidores en entorno local o externo. Para distribuir el tráfico, una puerta de enlace de la aplicación usa varios componentes descritos en este artículo.

![Los componentes utilizados en una puerta de enlace de aplicaciones](./media/application-gateway-components/application-gateway-components.png)

## <a name="frontend-ip-addresses"></a>Direcciones IP de Frontend

Una dirección IP de front-end es la dirección IP asociada con una puerta de enlace de la aplicación. Puede configurar una puerta de enlace de aplicaciones para tener una dirección IP pública, una dirección IP privada o ambos. Application gateway admite uno público o una dirección IP privada. La red virtual y la dirección IP pública deben estar en la misma ubicación que la puerta de enlace de la aplicación. Después de crearlo, una dirección IP de front-end está asociada con un agente de escucha.

### <a name="static-versus-dynamic-public-ip-address"></a>Estático o dirección IP pública dinámica

La SKU de puerta de enlace de aplicaciones de Azure v2 admite ambos internas y estáticas direcciones IP públicas estáticas, aunque la SKU v1 admite solo direcciones IP internas. Si se detiene e inicia una instancia de application gateway, puede cambiar la dirección IP virtual (VIP).

El nombre DNS asociado con una puerta de enlace de la aplicación no cambia durante el ciclo de vida de la puerta de enlace. Como resultado, debe usar un alias CNAME y hacer que apunte a la dirección DNS de la puerta de enlace de la aplicación.

## <a name="listeners"></a>Agentes de escucha

Un agente de escucha es una entidad lógica que comprueba si hay solicitudes de conexión entrantes. Un agente de escucha acepta una solicitud si el protocolo, puerto, host y dirección IP asociada con la solicitud coincide con los mismos elementos asociados con la configuración de agente de escucha.

Antes de usar una puerta de enlace de la aplicación, debe agregar al menos un agente de escucha. Puede haber varios agentes de escucha conectados a una puerta de enlace de aplicaciones, y se pueden usar para el mismo protocolo.

Una vez que un agente de escucha detecta las solicitudes entrantes de clientes, application gateway enruta estas solicitudes a los miembros del grupo de back-end. La puerta de enlace de la aplicación usa las reglas de enrutamiento de solicitud definidas para el agente de escucha que recibió la solicitud entrante.

Los agentes de escucha admiten los siguientes puertos y protocolos.

### <a name="ports"></a>Puertos

Un puerto es donde un agente de escucha realiza escuchas para la solicitud de cliente. Puede configurar los puertos comprendidos entre 1 y 65502 para la SKU v1 y 1 para 65199 para la SKU v2.

### <a name="protocols"></a>Protocolos

Application Gateway admite cuatro protocolos: HTTP, HTTPS, HTTP/2 y WebSocket:

- Especifique entre HTTP y HTTPS protocolos en la configuración de agente de escucha.
- Compatibilidad con [protocolos WebSockets y HTTP/2](https://docs.microsoft.com/azure/application-gateway/overview#websocket-and-http2-traffic) se proporciona de forma nativa, y [compatibilidad con WebSocket](https://docs.microsoft.com/azure/application-gateway/application-gateway-websocket) está habilitada de forma predeterminada. No hay ninguna opción de configuración que permita al usuario habilitar o deshabilitar la compatibilidad con WebSocket. Utilice WebSockets con agentes de escucha HTTP y HTTPS.
- La compatibilidad con el protocolo HTTP/2 está disponible únicamente para los clientes que se conectan a los agentes de escucha de la puerta de aplicaciones. La comunicación con granjas de servidores back-end es a través de HTTP/1.1. De forma predeterminada, HTTP/2 está deshabilitado. Puede elegir habilitarlo.

Use un agente de escucha HTTPS para la terminación SSL. Un agente de escucha HTTPS evita el trabajo de cifrado y descifrado para la puerta de enlace de la aplicación, por lo que los servidores web no se ve afectados por un sobrecarga. Las aplicaciones, a continuación, son gratis para centrarse en la lógica de negocios.

### <a name="custom-error-pages"></a>Páginas de error personalizadas

Instancia de Application Gateway le permite crear páginas de error personalizado en lugar de mostrar las páginas de error predeterminado. Mediante una página de error personalizada puede usar su propia marca y diseño. Puerta de enlace de aplicación muestra una página de error personalizado cuando una solicitud no puede conectar con el back-end.

Para obtener más información, consulte [páginas de errores personalizados para la puerta de enlace de la aplicación](https://docs.microsoft.com/azure/application-gateway/custom-error).

### <a name="types-of-listeners"></a>Tipos de agentes de escucha

Hay dos tipos de agentes de escucha:

- **Básico**. Este tipo de agente de escucha realiza escuchas en un sitio de dominio único, donde tiene una única asignación de DNS a la dirección IP de la puerta de enlace de la aplicación. Esta configuración de agente de escucha es necesaria cuando se hospeda en un único sitio detrás de una puerta de enlace de la aplicación.

- **Multisitio**. Esta configuración de agente de escucha es necesaria cuando se configura más de una aplicación web en la misma instancia de puerta de enlace de aplicaciones. Permite configurar una topología más eficaz para las implementaciones mediante la adición de hasta 100 sitios Web a una puerta de enlace. Cada sitio web se puede dirigir a su propio grupo de back-end. Por ejemplo, tres subdominios, abc.contoso.com, xyz.contoso.com y pqr.contoso.com, apuntan a la dirección IP de la puerta de enlace de la aplicación. Debe crear tres agentes de escucha multisitio y configurar cada agente de escucha para la configuración de protocolo y puerto respectivo.

    Para obtener más información, consulte [hospedar varios sitios](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-app-overview).

Después de crear un agente de escucha, asociarla a una regla de enrutamiento de solicitud. Esta regla determina cómo se debería enrutar la solicitud recibida en el agente de escucha para el back-end.

Puerta de enlace de aplicación procesa los agentes de escucha en el orden mostrado. Si el agente de escucha coincide con una solicitud entrante, se procesa en primer lugar. Para enrutar el tráfico back-end correcto, configure un agente de escucha multisitio antes de un agente de escucha básico.

## <a name="request-routing-rules"></a>Las reglas de enrutamiento de solicitud

Una regla de enrutamiento de solicitud es un componente clave de una puerta de enlace de aplicaciones, ya que determina cómo enrutar el tráfico en el agente de escucha. La regla enlaza el agente de escucha, el grupo de servidores back-end y la configuración HTTP de back-end.

Cuando un agente de escucha acepta una solicitud, la regla de enrutamiento de solicitud reenvía la solicitud al back-end o lo redirige en otro lugar. Si la solicitud se reenvía al back-end, la regla de enrutamiento de solicitud define qué grupo de servidores back-end para reenviarlo a. Además, la regla de enrutamiento de solicitud determina también si los encabezados de la solicitud se se vuelva a escribir. Un agente de escucha puede asociarse a una regla.

Hay dos tipos de reglas de enrutamiento de solicitud:

- **Básico**. Todas las solicitudes en el agente de escucha asociado (por ejemplo, blog.contoso.com/*) se reenvían al grupo asociado back-end mediante la configuración de HTTP asociada.

- **Ruta de acceso**. Esta regla de enrutamiento le permite enrutar las solicitudes en el agente de escucha asociado a un grupo de back-end específico, según la dirección URL en la solicitud. Si la ruta de acceso de la dirección URL en una solicitud coincide con el patrón de ruta de acceso en una regla basada en ruta de acceso, la regla enruta esa solicitud. El patrón de ruta de acceso se aplica solo a la ruta de acceso de dirección URL, no a sus parámetros de consulta. Si la ruta de acceso de dirección URL en una solicitud de agente de escucha no coincide con ninguna de las reglas basadas en ruta de acceso, enruta la solicitud para el grupo de back-end de forma predeterminada y la configuración de HTTP.

Para obtener más información, consulte [enrutamiento basado en URL](https://docs.microsoft.com/azure/application-gateway/url-route-overview).

### <a name="redirection-support"></a>Compatibilidad con la redirección

La regla de enrutamiento de solicitud, también puede redirigir el tráfico en la puerta de enlace de la aplicación. Se trata de un mecanismo de redirección genérico, por lo que puede redirigir a y desde cualquier puerto se define mediante el uso de reglas.

Puede elegir el destino de la redirección a ser otro agente de escucha (lo que puede ayudar a habilitar HTTP automática al redireccionamiento de HTTPS) o un sitio externo. También puede elegir para tener la redirección temporal o permanente, o para anexar la cadena de ruta de acceso y consulta URI a la dirección URL redirigida.

Para obtener más información, consulte [redirigir el tráfico en application gateway](https://docs.microsoft.com/azure/application-gateway/redirect-overview).

### <a name="rewrite-http-headers"></a>Reescritura de encabezados HTTP

Mediante el uso de las reglas de enrutamiento de solicitud, puede agregar, quitar o actualizar la solicitud HTTP (S) y mover los encabezados de respuesta como los paquetes de solicitud y respuesta entre el cliente y el back-end de los grupos a través de la puerta de enlace de la aplicación.

Los encabezados se pueden establecer valores estáticos o a otros encabezados y las variables de servidor. Esto ayuda a importantes casos de uso, como extraer las direcciones IP de cliente, quitar información confidencial sobre el back-end, agregar más seguridad y así sucesivamente.

Para obtener más información, consulte [encabezados HTTP vuelva a escribir en la puerta de enlace de la aplicación](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers).

## <a name="http-settings"></a>Configuración HTTP

Application gateway enruta el tráfico a los servidores back-end (especificado en la regla de enrutamiento de solicitud que incluyen la configuración de HTTP) con el número de puerto, protocolo y otras opciones que se detallan en este componente.

El puerto y protocolo usado en la configuración de HTTP, determinan si el tráfico entre los servidores de puerta de enlace y el back-end de aplicación está cifrado (proporcionando SSL de extremo a extremo) o sin cifrar.

Este componente también se utiliza para:

- Determinar si una sesión de usuario a mantenerse en el mismo servidor mediante el uso de la [afinidad de sesión basada en cookies](https://docs.microsoft.com/azure/application-gateway/overview#session-affinity).

- Quitar miembros del grupo de back-end correctamente con [drenaje de conexiones](https://docs.microsoft.com/azure/application-gateway/overview#connection-draining).

- Asociar un sondeo personalizado para supervisar el estado de back-end, establecer el intervalo de tiempo de espera de solicitud, reemplace el nombre de host y ruta de acceso en la solicitud y proporciona facilidad de un solo clic para especificar la configuración para el back-end de App Service.

## <a name="backend-pools"></a>Grupos de back-end

Un grupo de back-end enruta la solicitud a los servidores back-end que atender la solicitud. Pueden contener grupos back-end:

- Tarjetas de red
- Conjuntos de escalas de máquina virtual
- Direcciones IP públicas
- Direcciones IP internas
- FQDN
- Back-ends de varios inquilinos (por ejemplo, App Service)

Miembros del grupo de back-end de puerta de enlace de aplicaciones no están atados a un conjunto de disponibilidad. Application gateway puede comunicarse con instancias fuera de la red virtual que se encuentra. Como resultado, los miembros de los grupos de back-end pueden ser a través de clústeres, entre los centros de datos o fuera de Azure, siempre que haya conectividad IP.

Si usa direcciones IP internas como miembros del grupo de back-end, debe usar [emparejamiento de redes virtuales](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) o un [puerta de enlace VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways). Emparejamiento de redes virtuales es tráfico admitido y útil para equilibrio de carga en otras redes virtuales.

Una puerta de enlace de la aplicación también puede comunicarse con a servidores locales cuando están conectados mediante Azure ExpressRoute o túneles VPN si se permite el tráfico.

Puede crear grupos de back-end diferente para distintos tipos de solicitudes. Por ejemplo, cree un grupo de back-end para las solicitudes generales y, a continuación, otro grupo de back-end para las solicitudes a los microservicios de la aplicación.

## <a name="health-probes"></a>Sondeos de estado

De forma predeterminada, application gateway supervisa el estado de todos los recursos de su grupo de back-end y elimina automáticamente las en mal estado. A continuación, supervisa las instancias incorrectas y agregarlos al grupo de back-end correcto cuando estén disponibles y responder a los sondeos de estado.

Aparte del uso de la supervisión del sondeo de estado, también puede personalizar el sondeo de estado para adaptarlo a las necesidades de su aplicación. Los sondeos personalizados permiten un control más granular sobre la supervisión de estado. Cuando se usan sondeos personalizados, puede configurar el intervalo de sondeo, la dirección URL y la ruta de acceso para probar y cuántas respuestas erróneas se aceptan antes de la instancia del grupo de back-end se marque como incorrecta. Se recomienda que configure los sondeos personalizados para supervisar el estado de cada grupo de back-end.

Para obtener más información, consulte [supervisar el estado de la puerta de enlace de la aplicación](https://docs.microsoft.com/azure/application-gateway/application-gateway-probe-overview).

## <a name="next-steps"></a>Pasos siguientes

Creación de una puerta de enlace de aplicaciones:

* [En Azure Portal](quick-create-portal.md)
* [Con Azure PowerShell](quick-create-powershell.md)
* [Mediante el uso de la CLI de Azure](quick-create-cli.md)
