---
title: Componentes de la puerta de enlace de aplicaciones de Azure
description: En este artículo se proporciona información sobre los distintos componentes de Application Gateway
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 02/20/2019
ms.author: absha
ms.openlocfilehash: 44c8b331ebb258c39a003c91e0711e6dfb87cb12
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "58076307"
---
# <a name="application-gateway-components"></a>Componentes de la puerta de enlace de aplicaciones

 Application gateway actúa como el único punto de contacto para los clientes. Distribuye el tráfico entrante de aplicación en varios grupos de back-end, como máquinas virtuales de Azure, conjuntos de escalado de máquinas virtuales, servicios de aplicaciones o servidores en entorno local o externo. Para ello, usa varios componentes descritos en este artículo.

![application-gateway-components](./media/application-gateway-components/application-gateway-components.png)

## <a name="frontend-ip-address"></a>Dirección IP del front-end

Una dirección IP de front-end es la dirección IP asociada con la puerta de enlace de la aplicación. Puede configurar la puerta de enlace de aplicaciones para tener una dirección IP pública o una dirección IP privada o ambos. Se admite una única dirección IP pública o una dirección IP privada. La red virtual y la dirección IP pública deben estar en la misma ubicación que la puerta de enlace de la aplicación.

Una dirección de Frontend IP está asociada a un *escucha* después de crearlo. 

### <a name="static-vs-dynamic-public-ip-address"></a>Dirección IP pública dinámica puertos estáticos frente a

La SKU v1 es compatible con direcciones IP internas, pero no admite direcciones IP públicas estáticas. La dirección IP virtual puede cambiar si la puerta de enlace de aplicaciones se detiene y se inicia. El nombre DNS asociado con la puerta de enlace de aplicaciones no cambia durante el ciclo de vida de la puerta de enlace. Por este motivo, se recomienda usar un alias CNAME y hacer que señale a la dirección DNS de la puerta de aplicaciones.

La SKU de puerta de enlace de aplicaciones v2 admite direcciones IP públicas, así como las direcciones IP internas estáticas. 

## <a name="listeners"></a>Agentes de escucha

Para empezar a usar la puerta de enlace de la aplicación, debe agregar uno o varios agentes de escucha. Un agente de escucha es una entidad lógica que comprueba si hay solicitudes de conexión entrantes y acepta las solicitudes si el protocolo, puerto, host y dirección IP asociada con la coincidencia de la solicitud con el protocolo, puerto, host y dirección IP asociada con la configuración de agente de escucha. Puede haber varios agentes de escucha conectados a una puerta de enlace de la aplicación y se pueden usar para el mismo protocolo. Una vez que el agente de escucha detecta las solicitudes entrantes de los clientes, Application Gateway enruta estas solicitudes a los miembros del grupo de back-end, usando las reglas de enrutamiento de solicitud que defina para el agente de escucha que recibió la solicitud entrante.

Los agentes de escucha admiten los siguientes puertos y protocolos:

### <a name="ports"></a>Puertos

Este es el puerto en el que escucha el agente de escucha para la solicitud de cliente. Puede configurar los puertos comprendidos entre 1 y 65502 para SKU V1 y 1 para 65199 para SKU V2.

### <a name="protocols"></a>Protocolos

Application gateway admite los cuatro protocolos siguientes: HTTP, HTTPS, HTTP/2 y WebSocket

Especifica explícitamente la elección entre los protocolos HTTP y HTTPS en la configuración del agente de escucha. El [admite los protocolos WebSockets y HTTP/2](https://docs.microsoft.com/azure/application-gateway/overview#websocket-and-http2-traffic) se proporciona de forma nativa. [Compatibilidad con Websocket](https://docs.microsoft.com/azure/application-gateway/application-gateway-websocket) está habilitada de forma predeterminada. No hay ninguna opción de configuración que permita al usuario habilitar o deshabilitar la compatibilidad con WebSocket. Puede usar WebSockets con agentes de escucha HTTP y HTTPS. La compatibilidad con el protocolo HTTP/2 está disponible únicamente para los clientes que se conectan a los agentes de escucha de la puerta de aplicaciones. La comunicación con granjas de servidores back-end es a través de HTTP/1.1. De forma predeterminada, HTTP/2 está deshabilitado. Puede elegir habilitarlo.

Puede usar un agente de escucha HTTPS para la terminación SSL. Un agente de escucha HTTPS descarga el cifrado y descifrado funcionan para la puerta de enlace de la aplicación para que los servidores web no están repartidos por esa sobrecarga. Las aplicaciones, a continuación, son gratis para centrarse en su lógica de negocios.

### <a name="custom-error-pages"></a>Páginas de error personalizadas

Application gateway permite crear páginas de error personalizado en lugar de mostrar las páginas de error predeterminado. Mediante una página de error personalizada puede usar su propia marca y diseño. Application Gateway puede mostrar una página de error personalizada cuando una solicitud no puede conectar con el back-end. Para obtener más información, consulte [páginas de errores personalizados para la puerta de enlace de la aplicación](https://docs.microsoft.com/azure/application-gateway/custom-error).

### <a name="types-of-listeners"></a>Tipos de agentes de escucha

Hay dos tipos de agentes de escucha:

- **Básico**: Este tipo de agente de escucha realiza escuchas en un sitio único dominio donde tiene una única asignación de DNS a la dirección IP de la puerta de enlace de la aplicación. Esta configuración de agente de escucha es necesaria cuando se hospeda un único sitio detrás de una puerta de enlace de la aplicación.
- **Multisitio**: Esta configuración de agente de escucha es necesaria cuando se configura más de una aplicación web en la misma instancia de puerta de enlace de aplicaciones. Esto le permite configurar una topología más eficaz para las implementaciones mediante la adición de hasta 100 sitios Web a una puerta de enlace. Cada sitio web se puede dirigir a su propio grupo de back-end. Por ejemplo:   Para los tres subdominios: abc.contoso.com, xyz.contoso.com y pqr.contoso.com que apunta a la dirección IP de la puerta de enlace de la aplicación. Crear tres agentes de escucha del tipo *multisitio* y configurar cada agente de escucha para el puerto respectivo y configuración del protocolo. Para obtener más información, consulte [hospedaje de varios sitios](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-app-overview).

Después de crear un agente de escucha, asociarla a una regla de enrutamiento de solicitud que determina cómo es la solicitud recibida en el agente de escucha se enrute al back-end.

Los agentes de escucha se procesan en el orden en que aparecen. Por ese motivo, si un agente de escucha coincide con una solicitud entrante lo procesa en primer lugar. Los agentes de escucha de varios sitios deben configurarse antes de un agente de escucha básico para asegurarse de que el tráfico se enrute al back-end correcto.

## <a name="request-routing-rule"></a>Regla de enrutamiento de solicitud

Este es el componente más importante de la puerta de enlace de la aplicación y determina cómo se enruta el tráfico en el agente de escucha asociado con esta regla. La regla enlaza el agente de escucha, el grupo de servidores back-end y la configuración HTTP de back-end. Una vez que un agente de escucha se acepta una solicitud, la regla de enrutamiento de solicitud determina si la solicitud es que se reenvía al back-end o redirige en otro lugar. Si la solicitud se determina al reenviarse al back-end, la regla de enrutamiento de solicitud define a qué grupo de servidores back-end debe reenviarse al. Además, la regla de enrutamiento de solicitud determina también si los encabezados de la solicitud se puede volver a escribir. Un agente de escucha puede asociarse a una única regla.

Puede haber dos tipos de reglas de enrutamiento de solicitud:

- **Básica:** Todas las solicitudes en el agente de escucha asociado (por ejemplo: blog.contoso.com/*) se reenvían al grupo asociado back-end mediante la configuración de HTTP asociada.
- **Ruta de acceso:** Este tipo de regla le permite enrutar las solicitudes en el agente de escucha asociado a un grupo de back-end específico según la dirección URL en la solicitud. Si la ruta de acceso de la dirección URL en una solicitud coincide con el patrón de ruta de acceso en una regla basada en ruta de acceso, la solicitud se enruta mediante esa regla. El patrón de ruta de acceso se aplica solo a la ruta de acceso de la dirección URL, no a sus parámetros de consulta. Si la ruta de acceso de la dirección URL de una solicitud en un agente de escucha no coincide con ninguna de las reglas basadas en ruta de acceso, la solicitud se enruta a la *predeterminado* grupo back-end y el *predeterminado* configuración HTTP. Para obtener más información, consulte [enrutamiento basado en URL](https://docs.microsoft.com/azure/application-gateway/url-route-overview).

### <a name="redirection-support"></a>Compatibilidad con la redirección

La regla de enrutamiento de solicitud, también puede redirigir el tráfico en la puerta de enlace de la aplicación. Se trata de un mecanismo de redireccionamiento genérico, por lo que puede redirigir desde y hacia cualquier puerto que defina mediante reglas. Puede elegir el destino de la redirección a ser otro agente de escucha (lo que puede ayudar a habilitar HTTP automática al redireccionamiento de HTTPS) o un sitio externo, elija la redirección para ser temporal o permanente o elija Anexar la cadena de ruta de acceso y consulta URI a la dirección URL redirigida. Para obtener más información, consulte [redirigir el tráfico en Application Gateway](https://docs.microsoft.com/azure/application-gateway/redirect-overview).

#### <a name="rewrite-http-headers"></a>Reescritura de encabezados HTTP

Con las reglas de enrutamiento de solicitud se puede agregar, quitar o actualizar HTTP (S) de solicitud y encabezados de respuesta mientras la solicitud y mover paquetes de respuesta entre los grupos de back-end y de cliente a través de la puerta de enlace de la aplicación. Los encabezados no solo pueden establecerse a valores estáticos sino también a otros encabezados y las variables de servidor importantes. Esto ayudará a lograr varios casos de uso importantes, como extraer la dirección IP de los clientes, quitar información confidencial sobre el back-end, agregar medidas de seguridad adicionales, etcetera. Para obtener más información, consulte [encabezados HTTP vuelva a escribir en la puerta de enlace de la aplicación](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers).

## <a name="http-settings"></a>Configuración HTTP

Las rutas de puerta de enlace de aplicaciones de tráfico para el back-end de servidores (especificados en la regla de enrutamiento de solicitud a la que se adjunta la configuración de HTTP) con el número de puerto, protocolo y otras opciones especificadas en este componente. El puerto y protocolo usado en la configuración de HTTP es determinar si el tráfico entre los servidores de puerta de enlace y el back-end de aplicación está cifrado, por tanto, llevar a cabo SSL de extremo a extremo o sin cifrar. Este componente también se usa para: determinar si una sesión de usuario a mantenerse en el mismo servidor mediante el uso de la [afinidad de sesión basada en cookies](https://docs.microsoft.com/azure/application-gateway/overview#session-affinity), realizar la eliminación correcta de los miembros del grupo de back-end mediante [conexión Purgando](https://docs.microsoft.com/azure/application-gateway/overview#connection-draining), asociar un sondeo personalizado para supervisar el estado de back-end, establecer el intervalo de tiempo de espera de solicitud, reemplace el nombre de host y ruta de acceso en la solicitud y proporciona facilidad de un solo clic para especificar la configuración de back-end para el back-end de App service. 

## <a name="backend-pool"></a>Grupo back-end

El grupo de back-end se usa para enrutar las solicitudes a los servidores back-end, que atender la solicitud. Los grupos de back-end pueden constar de NIC, conjuntos, pública IP direcciones, interno IP direcciones, FQDN y varios inquilinos de escalado de máquinas virtuales back-end, como Azure App Service. Miembros del grupo de back-end de puerta de enlace de aplicaciones no están asociados a un conjunto de disponibilidad. Instancia de Application Gateway puede comunicarse con instancias fuera de la red virtual que está en, por lo tanto, los miembros de los grupos de back-end pueden ser a través de clústeres, centros de datos, o fuera de Azure, siempre que haya conectividad IP. Si planea usar direcciones IP internas como miembros del grupo de back-end, necesitará [emparejamiento de VNET](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) o [VPN Gateway](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways). Emparejamiento de VNet se admite y es útil para equilibrio de carga de tráfico en otras redes virtuales. La puerta de enlace de la aplicación también puede comunicarse con a servidores locales cuando están conectados mediante ExpressRoute o túneles VPN, siempre y cuando se permite el tráfico.

Puede crear grupos de back-end diferente para distintos tipos de solicitudes. Por ejemplo, cree un grupo de back-end para las solicitudes generales y otro grupo de back-end para las solicitudes a los microservicios de la aplicación.

## <a name="health-probes"></a>Sondeos de estado

Puerta de enlace de aplicaciones de forma predeterminada supervisa el estado de todos los recursos de su grupo de back-end y elimina automáticamente cualquier recurso que se considerará en mal estado del grupo. Continúa supervisando las instancias en mal estado y los agrega al grupo de back-end correcto una vez que estén disponibles y responder a los sondeos de estado. Aparte del uso de la supervisión del sondeo de estado, también puede personalizar el sondeo de estado para adaptarlo a las necesidades de su aplicación. Los sondeos personalizados permiten un control más específico sobre la supervisión de estado. Cuando se usan sondeos personalizados, puede configurar el intervalo de sondeo, la dirección URL y la ruta de acceso para probar y cuántas respuestas erróneas se aceptan antes de la instancia del grupo de back-end se marque como incorrecta. Se recomienda que configure los sondeos personalizados para supervisar el estado de cada grupo de back-end. Para obtener más información, consulte [supervisar el estado de la puerta de enlace de la aplicación](https://docs.microsoft.com/azure/application-gateway/application-gateway-probe-overview).

## <a name="next-steps"></a>Pasos siguientes

Después de conocer los componentes de Application Gateway, hacer lo siguiente:
* [Crear una puerta de enlace de la aplicación en Azure portal](quick-create-portal.md)
* [Crear una puerta de enlace de aplicaciones mediante Azure PowerShell](quick-create-powershell.md)
* [Crear una puerta de enlace de aplicaciones mediante la CLI de Azure](quick-create-cli.md)
