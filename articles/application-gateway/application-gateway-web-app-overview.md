---
title: Información general de varios inquilinos back-end, como Azure App service, con Azure Application Gateway
description: En esta página se proporciona una introducción a la compatibilidad de Application Gateway con los servidores back-end multiinquilino.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 03/18/2019
ms.author: victorh
ms.openlocfilehash: 8434340bb7ed95cc36115c05048b2b67682b5796
ms.sourcegitcommit: 12d67f9e4956bb30e7ca55209dd15d51a692d4f6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2019
ms.locfileid: "58224505"
---
# <a name="application-gateway-support-for-multi-tenant-back-ends-such-as-app-service"></a>Compatibilidad con la puerta de enlace de aplicaciones para varios inquilinos back termina como App service

En los diseños de arquitectura de varios inquilinos en los servidores web, varios sitios Web se ejecutan en la misma instancia de servidor web. Los nombres de host se utilizan para diferenciar entre las distintas aplicaciones que se hospedan. De forma predeterminada, la puerta de enlace de aplicaciones no cambia el encabezado del host HTTP de entrada del cliente y lo envía sin alterar al back-end. Esta funciona bien para los miembros del grupo de back-end como interfaces de red, escalado de máquinas virtuales conjuntos, las direcciones IP públicas, las direcciones IP interna y el FQDN como estas no confían en un encabezado de host específico o una extensión SNI para resolver en el punto de conexión correcto. Sin embargo, hay muchos servicios como Azure App service web apps y administración de API de Azure que son multiinquilino por naturaleza y dependen de un encabezado de host específico o una extensión SNI para resolver en el punto de conexión correcto. Normalmente, el nombre DNS de la aplicación, que a su vez, es el nombre DNS asociado con la puerta de enlace de aplicaciones, es diferente del nombre de dominio del servicio back-end. Por lo tanto, el encabezado de host en la solicitud original recibido por la puerta de enlace de la aplicación no es el mismo que el nombre de host del servicio back-end. Por este motivo, a menos que el encabezado de host en la solicitud de la puerta de enlace de aplicaciones back-end, se cambia al nombre de host del servicio back-end, el back-ends de varios inquilinos no son capaces de resolver la solicitud al extremo correcto. 

Application gateway proporciona una funcionalidad que permite a los usuarios reemplazar el encabezado de host HTTP en la solicitud según el nombre de host de back-end. Esta funcionalidad permite la compatibilidad para varios inquilinos back-end, como Azure App service web apps y API management. Esta funcionalidad está disponible para la v1 y v2 estándar y las SKU de WAF. 

![invalidación de host](./media/application-gateway-web-app-overview/host-override.png)

> [!NOTE]
> Esto no es aplicable a Azure de App service environment (ASE) dado que ASE es un recurso dedicado a diferencia de Azure App service, que es un recurso de varios inquilinos.

## <a name="override-host-header-in-the-request"></a>Reemplazar el encabezado de host en la solicitud

La capacidad de especificar una invalidación del host se define en el [configuración HTTP](https://docs.microsoft.com/azure/application-gateway/configuration-overview#http-settings) y se pueden aplicar a cualquier grupo de back-end durante la creación de reglas. Se admiten las siguientes dos maneras de reemplazar el encabezado de host y la extensión SNI para varios inquilinos back-end:

- La capacidad de establecer el nombre de host en un valor fijo que se escribe explícitamente en la configuración de HTTP. Esta funcionalidad garantiza que el encabezado de host se invalida con este valor para todo el tráfico al grupo de back-end donde se aplica la configuración de HTTP concreta. Al usar SSL de extremo a extremo, este nombre de host invalidado se usa en la extensión SNI. Esta funcionalidad permite escenarios donde un grupo de servidores back-end espera un encabezado de host que es diferente del encabezado de host del cliente de entrada.

- La posibilidad de obtener el nombre de host de la dirección IP o FQDN de los miembros del grupo de servidores back-end. Configuración de HTTP también proporciona una opción para seleccionar dinámicamente el nombre de host de FQDN de un miembro grupo de back-end si ha configurado con la opción para derivar el nombre de host de un miembro del grupo de back-end individual. Al usar SSL de extremo a extremo, este nombre de host se obtiene del FQDN y se usa en la extensión SNI. Esta funcionalidad permite escenarios donde un grupo de servidores back-end puede tener dos o más servicios PaaS multiinquilino, como Azure Web Apps y el encabezado de host de la solicitud para que cada miembro contenga el nombre de host obtenido de su FQDN. Para implementar este escenario, usamos un conmutador en la configuración de HTTP llama [Elegir nombre de host de dirección de back-end](https://docs.microsoft.com/azure/application-gateway/configuration-overview#pick-host-name-from-backend-address) dinámicamente que invalidará el encabezado de host en la solicitud original al mencionado en el grupo de back-end.  Por ejemplo, si el FQDN del grupo de back-end contiene "contoso11.azurewebsites.net" y "contoso22.azurewebsites.net", se invalidarán encabezado de host de la solicitud original que es contoso.com contoso11.azurewebsites.net o contoso22.azurewebsites.net Cuando la solicitud se envía al servidor back-end adecuado. 

  ![escenario de aplicación web](./media/application-gateway-web-app-overview/scenario.png)

Con esta funcionalidad, los clientes especifican las opciones en la configuración de HTTP y los sondeos personalizados para la configuración adecuada. Esta configuración se asocia luego a un agente de escucha y un grupo de servidores back-end mediante una regla.

## <a name="special-considerations"></a>Consideraciones especiales

### <a name="ssl-termination-and-end-to-end-ssl-with-multi-tenant-services"></a>Terminación SSL y SSL de extremo a extremo con servicios multiinquilino

Terminación SSL y el cifrado SSL de extremo a otro es compatible con servicios multiinquilino. Para la terminación SSL en application gateway, sigue siendo necesario agregar al agente de escucha de puerta de enlace de aplicaciones certificado SSL. Sin embargo, en el caso de SSL de extremo a extremo, Azure confianza servicios, como las aplicaciones web de Azure App service no requieren listas de permitidos con el back-ends en la puerta de enlace de la aplicación. Por lo tanto, no hay ninguna necesidad de agregar los certificados de autenticación. 

![SSL de extremo a extremo](./media/application-gateway-web-app-overview/end-to-end-ssl.png)

Tenga en cuenta que en la imagen anterior, no hay ningún requisito para agregar certificados de autenticación cuando se selecciona el servicio de aplicación como back-end.

### <a name="health-probe"></a>Sondeo de mantenimiento

Reemplazar el encabezado de host en el **configuración HTTP** solo afecta a la solicitud y su enrutamiento. no afecta el comportamiento de sondeo de estado. Para que la funcionalidad de extremo a extremo funcione, el sondeo y la configuración de HTTP se deben modificar para reflejar la configuración correcta. Además de proporcionar la capacidad de especificar un encabezado de host en la configuración de sondeo, los sondeos personalizados admiten también la posibilidad de derivar el encabezado de host de la configuración de HTTP. Esta configuración puede especificarse mediante el parámetro `PickHostNameFromBackendHttpSettings` en la configuración de sondeo.

### <a name="redirection-to-app-services-url-scenario"></a>Redirección de escenario de dirección URL del servicio de aplicación

Puede haber escenarios donde el nombre de host en la respuesta del servicio de aplicación puede dirigir el explorador del usuario final en el *. azurewebsites.net nombre de host en lugar del dominio asociado con la puerta de enlace de la aplicación. Este problema puede ocurrir cuando:

- Tener redireccionamiento configurado en el servicio de aplicaciones. Redirección puede ser tan sencilla como agregar una barra oblicua final a la solicitud.
- Tener la autenticación de Azure AD, lo que hace que la redirección.

Para resolver estos casos, consulte [redirección al problema de dirección URL del servicio de aplicación de la solución de problemas](https://docs.microsoft.com/azure/application-gateway/troubleshoot-app-service-redirection-app-service-url).

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre cómo configurar una puerta de enlace de aplicaciones con una aplicación de varios inquilinos como Azure App service web Apps como miembro del grupo de back-end, visite [configurar App Service web apps con Application Gateway](https://docs.microsoft.com/azure/application-gateway/create-web-app)
