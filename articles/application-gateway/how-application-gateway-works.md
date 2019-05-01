---
title: Cómo funciona una puerta de enlace de aplicaciones
description: Este artículo proporciona información sobre cómo funciona una puerta de enlace de aplicaciones
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 02/20/2019
ms.author: absha
ms.openlocfilehash: a16421182f533f5aa2ad4bcc2e58e910cc7e8ca6
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2019
ms.locfileid: "64702407"
---
# <a name="how-an-application-gateway-works"></a>Cómo funciona una puerta de enlace de aplicaciones

En este artículo se explica cómo una puerta de enlace de aplicaciones acepta las solicitudes entrantes y los enruta al back-end.

![Cómo una puerta de enlace de aplicaciones acepta una solicitud](./media/how-application-gateway-works/how-application-gateway-works.png)

## <a name="how-an-application-gateway-accepts-a-request"></a>Cómo una puerta de enlace de aplicaciones acepta una solicitud

1. Antes de que un cliente envía una solicitud a una puerta de enlace de aplicaciones, resuelve el nombre de dominio de la puerta de enlace de aplicaciones mediante el uso de un servidor de sistema de nombres de dominio (DNS). Azure controla la entrada DNS porque todas las puertas de enlace de aplicaciones están en el dominio de azure.com.

2. El DNS de Azure, se devuelve la dirección IP para el cliente, que es la dirección IP de front-end de la puerta de enlace de la aplicación.

3. La puerta de enlace de la aplicación acepta el tráfico entrante en una o varias escuchas. Un agente de escucha es una entidad lógica que comprueba las solicitudes de conexión. Se configura con una dirección IP de front-end, el protocolo y el número de puerto para las conexiones de clientes a la puerta de enlace de la aplicación.

4. Si firewall de aplicaciones web (WAF) está en uso, la puerta de enlace de la aplicación comprueba los encabezados de solicitud y el cuerpo, si está presente, según las reglas de WAF. Esta acción se determina si la solicitud es una solicitud válida o una amenaza de seguridad. Si la solicitud es válida, se enruta al back-end. Si la solicitud no es válida, se bloquea como una amenaza de seguridad.

Azure Application Gateway puede usarse como un equilibrador de carga interno de la aplicación o como un equilibrador de carga de aplicación a través de internet. Una puerta de enlace de aplicaciones orientado a internet usa direcciones IP públicas. El nombre DNS de una puerta de enlace de la aplicación a través de internet es que pueda resolverse públicamente en su dirección IP pública. Como resultado, las puertas de enlace de aplicaciones a través de internet pueden enrutar las solicitudes de cliente a internet.

Las puertas de enlace de aplicación interna usan sólo las direcciones IP privadas. El nombre DNS de una puerta de enlace de la aplicación interno es que pueda resolverse públicamente a su dirección IP privada. Por lo tanto, los equilibradores de carga internos solo pueden enrutar las solicitudes de clientes con acceso a una red virtual para la puerta de enlace de aplicaciones.

Puertas de enlace accesible desde internet y aplicaciones internos enrutan las solicitudes a los servidores back-end mediante direcciones IP privadas. Servidores de back-end no tienen direcciones IP públicas para recibir las solicitudes de una instancia interna o una puerta de enlace de la aplicación a través de internet.

## <a name="how-an-application-gateway-routes-a-request"></a>Cómo application gateway enruta una solicitud

Si una solicitud es válida o un WAF no está en uso, la puerta de enlace de aplicación evalúa la regla de enrutamiento de solicitud que está asociado con el agente de escucha. Esta acción determina qué grupo de back-end para enrutar la solicitud.

Las reglas se procesan en el orden en que aparecen en el portal. Según la regla de enrutamiento de solicitud, la puerta de enlace de la aplicación determina si se debe enrutar todas las solicitudes en el agente de escucha a un grupo de back-end específico, enrutar peticiones para los grupos de back-end diferente en función de la ruta de acceso URL o redirigir peticiones al otro puerto o externos sitio.

Cuando la puerta de enlace de aplicaciones selecciona el grupo de back-end, envía la solicitud a uno de los servidores back-end correcto en el grupo (y.y.y.y). El estado del servidor viene determinada por un sondeo de estado. Si el grupo de back-end contiene varios servidores, la puerta de enlace de la aplicación utiliza un algoritmo round-robin para enrutar las solicitudes entre los servidores en buen estado. Esta carga equilibra las solicitudes en los servidores.

Después de la puerta de enlace de la aplicación determina el servidor back-end, se abrirá una nueva sesión TCP con el servidor back-end basado en la configuración de HTTP. Configuración HTTP Especifique el protocolo, puerto y otras configuraciones relacionadas con el enrutamiento que son necesarios para establecer una nueva sesión con el servidor back-end.

El puerto y protocolo usado en la configuración de HTTP de determinan si está cifrado (por lo tanto, llevar a cabo SSL de extremo a extremo) o sin cifrar el tráfico entre los servidores de puerta de enlace y el back-end de aplicación.

Cuando una puerta de enlace de la aplicación envía la solicitud original al servidor back-end, respeta cualquier realizada en la configuración de HTTP relacionados con para reemplazar el nombre de host, ruta de acceso y el protocolo de configuración personalizada. Esta acción mantiene la afinidad de sesión basada en cookies, la selección de purga, nombre de host de conexión desde el back-end y así sucesivamente.

Una puerta de enlace de aplicaciones internos utiliza sólo las direcciones IP privadas. El nombre DNS de una puerta de enlace de aplicaciones internos sea capaz de resolver en su dirección IP privada. Como resultado, los equilibradores de carga internos solo pueden enrutar las solicitudes de clientes con acceso a la red virtual para la puerta de enlace de la aplicación.

 >[!NOTE]
 >Ambas puertas de enlace de aplicaciones internos y orientados a internet enrutan las solicitudes a servidores back-end mediante el uso de direcciones IP privadas. Esta acción se produce cuando el recurso de grupo de back-end contiene una dirección IP privada, configuración de NIC de VM o una dirección puede resolver internamente. Si el grupo de back-end:
> - **Es un punto de conexión público**, application gateway usa su dirección IP pública de front-end para tener acceso al servidor. Si no hay una dirección IP pública de front-end, uno se asigna para la conectividad saliente externa.
> - **Contiene un FQDN internamente puede resolver o una dirección IP privada**, application gateway enruta la solicitud al servidor back-end mediante el uso de sus direcciones IP privadas de instancia.
> - **Contiene un punto de conexión externo o un FQDN que pueda resolverse externamente y**, application gateway enruta la solicitud al servidor back-end mediante el uso de su dirección IP pública de front-end. La resolución de DNS se basa en una zona DNS privada o el servidor DNS personalizado, si ha configurado o utiliza el valor predeterminado proporcionado por Azure DNS. Si no hay una dirección IP pública de front-end, uno se asigna para la conectividad saliente externa.

### <a name="modifications-to-the-request"></a>Modificaciones a la solicitud

Application gateway inserta cuatro encabezados adicionales a todas las solicitudes antes de reenviar las solicitudes de back-end. Estos encabezados son x-forwarded-for, x-forwarded-proto, x-forwarded-port y original-x-host. El formato de encabezado x-forwarded-for es una lista separada por comas de IP: Port.

Los valores válidos para x-forwarded-proto son HTTP o HTTPS. X-forwarded-port especifica el puerto donde la solicitud alcanza la puerta de enlace de la aplicación. Encabezado X-original-host contiene el encabezado de host original con el que ha llegado la solicitud. Este encabezado es útil en la integración del sitio Web de Azure, donde se modifica el encabezado de host entrante antes de que el tráfico se enruta al back-end. Si está habilitada la afinidad de sesión como una opción, agrega una cookie de afinidad administrado por puerta de enlace.

Puede configurar la puerta de enlace de aplicaciones para modificar los encabezados mediante [encabezados HTTP reescribir](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers) o para modificar la ruta de acceso URI mediante una configuración de invalidación de la ruta de acceso. Sin embargo, a menos que se configura para ello, todas las solicitudes entrantes son procesadas por el proxy para el back-end.

## <a name="next-steps"></a>Pasos siguientes

[Obtenga información acerca de los componentes de puerta de enlace de aplicaciones](application-gateway-components.md)
