---
title: Cómo Azure funciona de la puerta de enlace de aplicaciones
description: En este artículo se proporciona información sobre cómo para el funcionamiento de la puerta de enlace de aplicación
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 02/20/2019
ms.author: absha
ms.openlocfilehash: ef07def377b74fb74d57372f471efcf48fcf7aa2
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "57881102"
---
# <a name="how-application-gateway-works"></a>Cómo funciona Application Gateway

En este artículo se explica cómo la puerta de enlace de aplicaciones acepta las solicitudes entrante y los enruta para el back-end.

![how-application-gateway-works](./media/how-application-gateway-works/how-application-gateway-works.png)

## <a name="how-a-request-is-accepted"></a>¿Cómo se acepta una solicitud

Antes de que un cliente envía una solicitud a la puerta de enlace de aplicaciones, resuelve el nombre de dominio de la aplicación la puerta de enlace con un servidor de sistema de nombres de dominio (DNS). La entrada DNS se controla mediante Azure, ya que las puertas de enlace de aplicaciones están en el dominio de azure.com. Azure DNS devuelve la dirección IP al cliente, que es el *dirección IP de front-end* de Application Gateway. La puerta de enlace de la aplicación acepta el tráfico entrante en uno o varios *los agentes de escucha*. Un agente de escucha es una entidad lógica que comprueba las solicitudes de conexión. Se configura con una dirección IP front-, protocolo y número de puerto para las conexiones de clientes a la puerta de enlace de la aplicación. Si está habilitado el Firewall de aplicaciones Web (WAF), Application Gateway comprueba los encabezados de solicitud y el cuerpo (si existe) contra el *las reglas de WAF* para determinar si la solicitud es una solicitud válida - en cuyo caso se enrutará a la back-end - o una amenaza de seguridad, en el que se bloqueará el caso de la solicitud.  

Instancia de Application gateway puede usarse como un equilibrador de carga interno de la aplicación o un equilibrador de carga de aplicación a través de Internet. Una puerta de enlace de la aplicación a través de Internet tiene direcciones IP públicas. El nombre DNS de una puerta de enlace de la aplicación a través de Internet es que pueda resolverse públicamente en su dirección IP pública. Por lo tanto, puertas de enlace de aplicaciones a través de Internet pueden enrutar las solicitudes de clientes a través de Internet. Las puertas de enlace de aplicaciones internos tienen solo las direcciones IP privadas. El nombre DNS de una puerta de enlace de la aplicación interno es que pueda resolverse públicamente a su dirección IP privada. Por lo tanto, los equilibradores de carga interno solo pueden enrutar las solicitudes de clientes con acceso a la red virtual para la puerta de enlace de la aplicación. Orientado a Internet e internas de Application Gateway enrutan las solicitudes a los servidores back-end mediante direcciones IP privadas. Por lo tanto, los servidores back-end no necesitan direcciones IP públicas para recibir solicitudes de una instancia interna o una puerta de enlace de la aplicación a través de Internet.

## <a name="how-a-request-is-routed"></a>¿Cómo se enruta una solicitud

Si no se encuentra la solicitud sea válido (o si no está habilitado el WAF), el *regla de enrutamiento de solicitud* asociado con el *escucha* se evalúa para determinar el *grupo back-end* a que es la solicitud se enrute. Las reglas se procesan en el orden en que aparecen en el portal. Según el *regla de enrutamiento de solicitud* configuración, la puerta de enlace de aplicaciones decide si desea enrutar todas las solicitudes en el agente de escucha a un grupo de back-end específico o enrutarlos a los grupos de back-end diferente dependiendo de la ruta de acceso de dirección URL o a *redirigir las solicitudes* a otro puerto o el sitio externo

Una vez un *back-end* *grupo* se ha elegido, application gateway envía la solicitud a uno de los servidores back-end configurados en el grupo que está en buen estado (y.y.y.y). El estado del servidor viene determinada por un *sondeo de estado*. Si el grupo de back-end contiene más de un servidor, instancia de application gateway usa el algoritmo round robin para enrutar las solicitudes entre los servidores en buen estado, por lo tanto las solicitudes en los servidores de equilibrio de carga.

Después de que se ha determinado un servidor back-end, la puerta de enlace de aplicación abre una nueva sesión TCP con el servidor back-end basado en la configuración en el *configuración HTTP*. El *configuración HTTP* es un componente que especifica el protocolo, puerto y otras relacionadas con el enrutamiento de la configuración que es necesario para establecer una sesión de nuevo con el servidor back-end. El puerto y protocolo usado en la configuración de HTTP es determinar si el tráfico entre los servidores de puerta de enlace y el back-end de aplicación está cifrado, por tanto, llevar a cabo SSL de extremo a extremo o sin cifrar. Al enviar la solicitud original al servidor back-end, la puerta de enlace de aplicación respeta cualquier configuración personalizada realizada en la configuración de HTTP relacionados con para reemplazar el nombre de host, la ruta de acceso, el protocolo; el nombre de host desde el back-end, etcetera de selección de mantener la afinidad basada en cookies de sesión, purga, de la conexión.

Una instancia interna de Application Gateway tiene solo las direcciones IP privadas. El nombre DNS de una puerta de enlace de aplicaciones interno sea capaz de resolver internamente a su dirección IP privada. Por lo tanto, los equilibradores de carga interno solo pueden enrutar las solicitudes de clientes con acceso a la red virtual para la puerta de enlace de la aplicación.

Tenga en cuenta que enrutar a través de Internet e internas de Application Gateway se solicita a los servidores back-end mediante direcciones IP privadas, si el recurso de grupo de back-end contiene una dirección IP privada, configuración de NIC de VM o una dirección puede resolver internamente y si su grupo back-end es un punto de conexión público, Application Gateway usa su dirección IP pública de front-end para tener acceso al servidor. Si aún no lo ha aprovisionado una dirección IP pública de front-end, uno se asigna para la conectividad saliente externa.

### <a name="modifications-to-the-request"></a>Modificaciones a la solicitud

Application gateway inserta encabezados adicionales 4 para todas las solicitudes antes de reenviar las solicitudes de back-end. Estos encabezados son X-forwarded-for, X-forwarded-proto, X-forwarded-port y original-X-host. El formato de encabezado x-forwarded-for es una lista separada por comas de IP: Port. Los valores válidos para x-forwarded-proto son HTTP o HTTPS. X-forwarded-port especifica el puerto al que llegó la solicitud en la puerta de enlace de aplicaciones. Encabezado X-original-host contiene el encabezado de host original con el que ha llegado la solicitud. Este encabezado es útil en escenarios como la integración con el Sitio web de Azure, donde el encabezado del host entrante se modifica antes de que el tráfico se enrute al back-end. Opcionalmente, si está habilitada la afinidad de sesión, se inserta una cookie de afinidad administrado de la puerta de enlace. 

Además puede configurar application gateway para modificar los encabezados con [encabezados HTTP reescribir](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers) o modificar la ruta de acceso URI con la configuración de invalidación de ruta de acceso. Pero, a menos que se configura para ello, todas las solicitudes entrantes son procesadas por el proxy tal como están en el back-end.


## <a name="next-steps"></a>Pasos siguientes

Después de conocer cómo funciona application gateway, consulte [componentes de la puerta de enlace de aplicaciones](application-gateway-components.md) entender sus componentes con más detalle.
