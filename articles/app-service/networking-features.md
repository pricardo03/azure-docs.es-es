---
title: 'Características de implementación: Azure App Service de red | Microsoft Docs'
description: Cómo usar el servicio de aplicación diversas características de red
author: ccompy
manager: stefsch
editor: ''
services: app-service\web
documentationcenter: ''
ms.assetid: 5c61eed1-1ad1-4191-9f71-906d610ee5b7
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 05/28/2019
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 666430a11fb95871eb601b2a38eb7b97ad16119f
ms.sourcegitcommit: 600d5b140dae979f029c43c033757652cddc2029
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66498955"
---
# <a name="app-service-networking-features"></a>Características de red de App Service

Las aplicaciones en Azure App Service pueden implementarse de varias maneras. De forma predeterminada, App Service aplicaciones alojadas son directamente accesible desde internet y solo pueden llegar a los puntos de conexión de internet hospedada. Sin embargo, muchas aplicaciones de cliente necesitan controlar el tráfico de red entrante y saliente. Hay varias características disponibles en el servicio de aplicación para satisfacer esas necesidades. El desafío consiste en saber qué característica se debe usar para resolver un problema determinado. Este documento está pensado para ayudar a los clientes a determinar qué función se debe utilizar basándose en algunos casos de uso de ejemplo.

Hay dos tipos de implementación principal para Azure App Service. Hay el servicio multiinquilino público, que hospeda los planes de App Service en gratis, compartido, básico, estándar, Premium y Premiumv2 las SKU de precios. Es el único inquilino de App Service Environment, que hospeda los planes de App Service de SKU aislado directamente en Azure Virtual Network (VNet). Las características que usa variará en si está en el servicio multiinquilino o en un entorno ASE. 

## <a name="multi-tenant-app-service-networking-features"></a>Características de red de App Service de varios inquilinos 

Azure App Service es un sistema distribuido. Los roles que controlan las solicitudes entrantes de HTTP/HTTPS se denominan servidores front-end. Los roles que hospedan la carga de trabajo del cliente se llama a los trabajadores. Todos los roles en una implementación de App Service existen en una red de varios inquilinos. Dado que hay muchos clientes diferentes en la misma unidad de escalado de App Service, no se puede conectar la red de App Service directamente a la red. En lugar de conectarse a las redes, necesitamos las características para administrar los diferentes aspectos de la comunicación de la aplicación. Las características que controlan las solicitudes a la aplicación no se puede usar para solucionar problemas al realizar llamadas desde la aplicación. Del mismo modo, las características que resuelven problemas para las llamadas desde la aplicación no se puede usar para solucionar problemas a la aplicación.  

| Características de entrada | Características de salida |
|---------------------|-------------------|
| Direcciones asignadas a las aplicaciones | conexiones híbridas |
| Restricciones de acceso | Necesita integración con redes virtuales de puerta de enlace |
| Puntos de conexión de servicio | Integración con redes virtuales (versión preliminar) |

A menos que se indique lo contrario, todas las características se pueden usar conjuntamente. Puede combinar las características para solucionar diversos problemas.

## <a name="use-case-and-features"></a>Caso de uso y características

Para cualquier caso de uso determinado, puede haber varias maneras de solucionar el problema.  La función adecuada para usar a veces es debido a razones más allá de simplemente el caso de uso. Los siguientes casos de uso de entrada sugieren cómo usar las características de red de App Service para solucionar problemas acerca de cómo controlar el tráfico que va a la aplicación. 
 
| Casos de uso de entrada | Característica |
|---------------------|-------------------|
| Compatibilidad con SSL basada en IP que necesita para la aplicación | direcciones asignadas a las aplicaciones |
| No se comparte, dirección entrante dedicada para la aplicación | direcciones asignadas a las aplicaciones |
| Restringir el acceso a la aplicación desde un conjunto de direcciones bien definidas | Restricciones de acceso |
| Exponer mi aplicación en direcciones IP privadas en mi red virtual | ASE de ILB </br> Puerta de enlace de aplicaciones con puntos de conexión de servicio |
| Restringir el acceso a mi aplicación de recursos en una red virtual | Puntos de conexión de servicio </br> ASE de ILB |
| Exponer mi aplicación en una dirección IP privada en mi red virtual | ASE de ILB </br> dirección IP privada para entrante en una instancia de Application Gateway con puntos de conexión de servicio |
| Proteger mi aplicación con un WAF | Puerta de enlace de aplicaciones + ASE de ILB </br> Puerta de enlace de aplicaciones con puntos de conexión de servicio </br> Azure puerta delantera con restricciones de acceso |
| Equilibrar la carga del tráfico a mis aplicaciones en diferentes regiones | Azure puerta delantera con restricciones de acceso | 
| Equilibrar la carga del tráfico en la misma región | Puerta de enlace de aplicaciones con puntos de conexión de servicio | 

Los siguientes casos de uso de salida sugieren cómo usar las características de red de App Service para resolver las necesidades de acceso de salida de la aplicación. 

| Casos de uso de salida | Característica |
|---------------------|-------------------|
| Acceso a los recursos en una red Virtual de Azure en la misma región | Integración con red virtual </br> ASE |
| Acceso a los recursos en una red Virtual de Azure en una región diferente | Necesita integración con redes virtuales de puerta de enlace </br> Instancia de ASE y el emparejamiento de VNet |
| Obtener acceso a recursos protegidos con los puntos de conexión de servicio | Integración con red virtual </br> ASE |
| Acceso a recursos en una red privada no está conectado a Azure | conexiones híbridas |
| Acceso a los recursos a través de circuitos ExpressRoute | Integración con red virtual (está restringido a direcciones RFC 1918 por ahora) </br> ASE | 


### <a name="default-networking-behavior"></a>Comportamiento predeterminado de red

Las unidades de escalado de Azure App Service admiten a muchos clientes en cada implementación. Los planes gratis y compartido SKU hospedan cargas de trabajo de cliente en los trabajos de varios inquilinos. El Basic y versiones posteriores de los planes host las cargas de trabajo que se dedican a un único plan de App Service (ASP). Si tuviera un plan de App Service estándar, todas las aplicaciones de ese plan se ejecutarán en el mismo trabajo. Si escalar horizontalmente el trabajo, a continuación, todas las aplicaciones de ese ASP se replicará en un trabajo nuevo para cada instancia de la página ASP. Los trabajos que se usan para Premiumv2 son diferentes de los trabajos que se usa para los otros planes. Cada implementación de App Service tiene una dirección IP que se usa para todo el tráfico entrante a las aplicaciones de esa implementación de App Service. Hay sin embargo en cualquier lugar del 4 11 direcciones usadas para realizar llamadas salientes. Estas direcciones se comparten por todas las aplicaciones de esa implementación de App Service. Las direcciones de salida son diferentes en función de los tipos de trabajo diferente. Esto significa que las direcciones usadas por gratis, compartido, básico, estándar y Premium ASP sean diferentes de las direcciones usadas para las llamadas salientes desde las ASP Premiumv2. Si observa en las propiedades de la aplicación, puede ver las direcciones de entrada y salidas utilizados por la aplicación. Si necesita bloquear una dependencia con una ACL de IP, utilice el possibleOutboundAddresses. 

![Propiedades de la aplicación](media/networking-features/app-properties.png)

App Service tiene un número de puntos de conexión que se usan para administrar el servicio.  Dichas direcciones se publican en un documento independiente y también se encuentran en la etiqueta de servicio AppServiceManagement IP. La etiqueta AppServiceManagement solo se usa con un App Service Environment (ASE) donde debe permitir este tráfico. El servicio de aplicación se realiza un seguimiento de direcciones entrantes en la etiqueta de servicio AppService IP. No hay ninguna etiqueta de servicio IP que contiene las direcciones de salida usadas App Service. 

![Diagrama de entrada y salida de App Service](media/networking-features/default-behavior.png)

### <a name="app-assigned-address"></a>Direcciones asignadas a las aplicaciones 

La característica de dirección asignada de la aplicación es una rama de la funcionalidad SSL basada en IP y se accede mediante la configuración de SSL con la aplicación. Esta característica se puede usar para las llamadas basadas en IP SSL, pero también puede utilizarse para proporcionar una dirección que solo tiene a su aplicación. 

![Diagrama de direcciones asignadas a las aplicaciones](media/networking-features/app-assigned-address.png)

Cuando se utiliza una aplicación asignada la dirección, el tráfico sigue pasando por los mismos roles de front-end que controlan todo el tráfico entrante en la unidad de escalado de App Service. La dirección que se asigna a la aplicación, sin embargo, solo se usa la aplicación. Los casos de uso para esta característica son:

* Compatibilidad con SSL basada en IP que necesita para la aplicación
* Establecer una dirección dedicada para la aplicación que no se comparte con nada

Puede aprender a establecer una dirección en la aplicación con el tutorial en [SSL basada en la configuración de IP][appassignedaddress]. 

### <a name="access-restrictions"></a>Restricciones de acceso 

Las restricciones de acceso a funcionalidad le permite filtrar **entrante** solicitudes en función de la dirección IP de origen. La acción de filtrado tiene lugar en los roles de front-end que son de nivel superior de las tiradas de trabajo donde se ejecutan las aplicaciones. Puesto que los roles de front-end son precede a los trabajadores, la funcionalidad de las restricciones de acceso se puede considerar como la protección de nivel de red para sus aplicaciones. La característica le permite crear una lista de permitidos y denegación bloques de direcciones que se evalúan en orden de prioridad. Es similar a la característica de grupo de seguridad de red (NSG) que existe en las redes de Azure.  Puede usar esta característica en un entorno ASE o en el servicio de varios inquilinos. Cuando se usa con un ASE de ILB, puede restringir el acceso de los bloques de direcciones privadas.

![Restricciones de acceso](media/networking-features/access-restrictions.png)

La característica de restricciones de acceso ayuda a escenarios donde desea restringir las direcciones IP que pueden usarse para llegar a la aplicación. Entre el uso casos para esta característica son:

* Restringir el acceso a la aplicación desde un conjunto de direcciones bien definidas 
* Restringir el acceso a la que llega a través de un servicio de equilibrio de carga, como Azure puerta de entrada. Si desea bloquear el tráfico entrante para el acceso de principal de Azure, crear reglas para permitir el tráfico desde 147.243.0.0/16 y 2a01:111:2050:: / 44. 

![Restricciones de acceso con la puerta de entrada](media/networking-features/access-restrictions-afd.png)

Si desea bloquear el acceso a la aplicación para que solo esté accesible desde los recursos de Azure Virtual Network (VNet), necesita una dirección pública en todo lo que es el origen de la red virtual. Si los recursos no tienen una dirección pública, debe usar la característica de puntos de conexión de servicio en su lugar. Obtenga información sobre cómo habilitar esta característica con el tutorial en [configurar restricciones de acceso][iprestrictions].

### <a name="service-endpoints"></a>Puntos de conexión de servicio

Los puntos de conexión de servicio le permite bloquear **entrante** acceso a su aplicación de modo que la dirección de origen debe proceder de un conjunto de subredes que seleccione. Esta característica funciona junto con las restricciones de acceso IP. Los puntos de conexión de servicio se establecen en la misma experiencia de usuario que las restricciones de acceso IP. Puede crear una lista de reglas de acceso permitir o denegar que incluye direcciones públicas, así como las subredes en las redes virtuales. Esta característica es compatible con escenarios como:

![puntos de conexión de servicio](media/networking-features/service-endpoints.png)

* Configuración de Application Gateway con la aplicación para bloquear el tráfico entrante a la aplicación
* Comunicaciones acceso a la aplicación a los recursos de la red virtual. Esto puede incluir máquinas virtuales, las instancias de ase o incluso otras aplicaciones que usan la integración con red virtual 

![puntos de conexión de servicio con application gateway](media/networking-features/service-endpoints-appgw.png)

Puede aprender más sobre cómo configurar los extremos de servicio con la aplicación en el tutorial en [configurar restricciones de acceso del punto de conexión de servicio][serviceendpoints]
 
### <a name="hybrid-connections"></a>conexiones híbridas

Conexiones híbridas de App Service permite que las aplicaciones realizar **saliente** las llamadas a los extremos TCP especificados. El punto de conexión puede ser local, en una red virtual o en cualquier lugar permita el tráfico saliente a Azure en el puerto 443. La característica requiere la instalación de un agente de retransmisión que llama el Administrador de conexiones híbridas (HCM) en Windows Server 2012 o host más reciente. El HCM debe ser capaz de conectarse a Azure Relay en el puerto 443. El HCM puede descargarse desde la interfaz de usuario de aplicación de servicio híbrido conexiones en el portal. 

![Flujo de red de conexiones híbridas](media/networking-features/hybrid-connections.png)

La característica conexiones híbridas de App Service se basa en la funcionalidad conexiones híbridas de Relay de Azure. App Service usa una forma especializada de la característica que solo admite la realización de llamadas salientes desde la aplicación en un host TCP y el puerto. Este host y el puerto solo deben resolver en el host donde está instalado el HCM. Cuando la aplicación en App Service, realiza una búsqueda de DNS en el host y puerto definido en la conexión híbrida, el tráfico se redirige automáticamente para ir a través de la conexión híbrida y espera el Administrador de conexiones híbridas. Para obtener más información sobre las conexiones híbridas, lea la documentación en [conexiones híbridas de App Service][hybridconn]

Esta característica se utiliza normalmente para:

* Acceso a recursos en redes privadas que no están conectadas a Azure con una VPN o ExpressRoute
* Compatibilidad con elevación y desplazamiento de aplicaciones de forma local en App Service sin necesidad de mover las bases de datos auxiliares  
* Proporciona acceso seguro a un único host y puerto de conexión híbrida. La mayoría de las características de red abre el acceso a una red y con las conexiones híbridas solo tiene el único host y el puerto que puede ponerse en contacto.
* Cubre situaciones no cubiertos por otros métodos de conectividad de salida
* El desarrollo se realiza en App Service, donde las aplicaciones pueden aprovechar fácilmente los recursos locales 

Dado que la característica permite el acceso a recursos locales sin un agujero de firewall de entrada, es más popular con los desarrolladores. El otras saliente App Service características de red están muy Azure Virtual red relacionadas con. Conexiones híbridas no tiene una dependencia de pasar por una red virtual y puede usarse para una amplia variedad de necesidades de la red. Es importante tener en cuenta que la característica conexiones híbridas de App Service no ocupa o conocer lo que hace en la parte superior. Es decir que puede usar para tener acceso a una base de datos, un servicio web o un socket TCP arbitrario en un gran sistema. La característica esencialmente la tunelización de paquetes TCP. 

Mientras que las conexiones híbridas es popular para el desarrollo, también se utiliza en numerosas aplicaciones de producción. Es muy útil para tener acceso a un servicio web o una base de datos, pero no es adecuado para situaciones en las que implican un gran número de conexiones va a crear. 

### <a name="gateway-required-vnet-integration"></a>Necesita integración con redes virtuales de puerta de enlace 

La puerta de enlace requiere la característica de integración de red virtual de App Service permite a la aplicación realizar **saliente** solicitudes en una red Virtual de Azure. La característica funciona mediante la conexión en el host de que la aplicación se está ejecutando en una puerta de enlace de red Virtual en la red virtual con una VPN de punto a sitio. Al configurar la característica, la aplicación obtiene una de las direcciones de punto a sitio asignadas a cada instancia. Esta característica le permite tener acceso a recursos clásicos o VNets de Resource Manager en cualquier región. 

![Necesita integración con redes virtuales de puerta de enlace](media/networking-features/gw-vnet-integration.png)

Esta característica soluciona el problema de tener acceso a los recursos de otras redes virtuales e incluso se puede usar para conectarse a través de una red virtual a otras redes virtuales o incluso de forma local. No funciona con ExpressRoute que conecta las redes virtuales pero no con VPN de sitio a sitio redes conectadas. Es normalmente inadecuado usar esta característica desde una aplicación en un App Service Environment (ASE), porque la instancia de ASE ya está en la red virtual. Los casos de uso que soluciona esta característica son:

* Acceso a los recursos de direcciones IP privadas de las redes virtuales de Azure 
* Acceso a los recursos locales si hay una VPN de sitio a sitio 
* Acceso a los recursos en redes virtuales emparejadas 

Cuando esta característica está habilitada, la aplicación usará el servidor DNS configurado con la red virtual de destino. Puede leer más sobre esta característica en la documentación en [aplicación de servicio de integración con red virtual][vnetintegrationp2s]. 

### <a name="vnet-integration"></a>Integración con red virtual

La puerta de enlace requiere la característica de integración con red virtual es muy útil, pero todavía no resuelve el acceso a los recursos a través de ExpressRoute. En la parte superior que necesitan alcanzar a través de conexiones de ExpressRoute, es necesario para las aplicaciones poder realizar llamadas a servicios protegidos de punto de conexión de servicio. Para solucionar ambas esas necesidades adicionales, se ha agregado otra funcionalidad de integración con red virtual. La nueva característica de integración con red virtual le permite colocar el back-end de la aplicación en una subred de una VNet de Resource Manager en la misma región. Esta característica no está disponible desde un App Service Environment, que ya está en una red virtual. Esta característica permite lo siguiente:

* Acceso a los recursos VNets de Resource Manager en la misma región
* Obtener acceso a los recursos protegidos con puntos de conexión de servicio 
* Acceso a los recursos que son accesibles a través de conexiones de ExpressRoute o VPN

![Integración con red virtual](media/networking-features/vnet-integration.png)

Esta característica está en versión preliminar y no debe usarse para cargas de trabajo de producción. Para obtener más información sobre esta característica, lea los documentos en [aplicación de servicio de integración con red virtual][vnetintegration].

## <a name="app-service-environment"></a>Entorno de App Service 

App Service Environment (ASE) es una implementación de un solo inquilino de Azure App Service que se ejecuta en la red virtual. La instancia de ASE permite casos de uso, como:

* Acceso a los recursos de la red virtual
* Acceso a los recursos a través de ExpressRoute
* Exponer sus aplicaciones con una dirección de la red virtual privada 
* Acceso a los recursos a través de puntos de conexión de servicio 

Con un ASE, no es necesario utilizar características como la integración con redes virtuales o extremos de servicio porque la instancia de ASE ya está en la red virtual. Si desea tener acceso a recursos como SQL o el almacenamiento a través de extremos de servicio, Habilitar puntos de conexión de servicio en la subred de ASE. Si desea tener acceso a recursos en la red virtual, no es necesario realizar ninguna configuración adicional.  Si desea tener acceso a recursos a través de ExpressRoute, que ya está en la red virtual y no es necesario configurar nada en el ASE o de las aplicaciones dentro de él. 

Dado que las aplicaciones en un ASE de ILB pueden quedar expuestas en una dirección IP privada, puede agregar fácilmente los dispositivos WAF para exponer solo a las aplicaciones que desee a internet y proteger el resto. Se presta para el desarrollo sencillo de aplicaciones de varios niveles. 

Hay algunas cosas que no son posibles desde el servicio de varios inquilinos que provienen de un ASE. Cosas como por ejemplo:

* Exponer las aplicaciones en una dirección IP privada
* Proteger todo el tráfico saliente con los controles de red que no forman parte de la aplicación 
* Hospedar sus aplicaciones en un servicio de inquilino único 
* Escalar verticalmente a muchas más instancias que son posibles en el servicio de varios inquilinos 
* Cargar certificados de cliente de entidad de certificación privados para su uso por las aplicaciones con CA privada protegida de los puntos de conexión 
* Forzar TLS 1.1 en todas las aplicaciones hospedadas en el sistema sin ninguna capacidad de deshabilitar en el nivel de aplicación 
* Proporcione una dirección de salida dedicada para todas las aplicaciones en el ASE que no se comparte con los clientes 

![ASE en una red virtual](media/networking-features/app-service-environment.png)

El ASE proporciona la mejor historia en el hospedaje de aplicación aislada y dedicada, pero conlleva algunos desafíos de administración. Algunos aspectos a tener en cuenta antes de usar una instancia de ASE operativa son:
 
 * Una instancia de ASE se ejecuta dentro de la red virtual, pero tienen dependencias fuera de la red virtual. Se deben permitir esas dependencias. Obtenga más información en [consideraciones de red para App Service Environment][networkinfo]
 * Una instancia de ASE no se escala inmediatamente al igual que el servicio de varios inquilinos. Debe prever las necesidades de escalado en lugar de escalar de forma reactiva. 
 * Una instancia de ASE tienen una mayor por adelantado costo asociado con él. Para obtener el máximo partido de su ASE, se debe previsto poner muchas cargas de trabajo en un ASE en lugar de tener se usa para los esfuerzos de pequeño
 * Las aplicaciones en un entorno ASE no pueden restringir el acceso a algunas aplicaciones en un ASE y otras no.
 * El ASE está en una subred y las reglas de red se aplican a todo el tráfico hacia y desde dicho ASE. Si desea asignar las reglas de tráfico entrante para una sola aplicación, utilice restricciones de acceso. 

## <a name="combining-features"></a>Combinación de características 

Las características que anotó para el servicio multiinquilino se pueden usar conjuntamente para resolver casos de uso más elaborados. Aquí se describen dos de los casos de uso más común, pero son solo ejemplos. Al comprender lo que hacer las distintas características, puede resolver casi todas sus necesidades de arquitectura del sistema.

### <a name="inject-app-into-a-vnet"></a>Insertar la aplicación en una red virtual

Es una solicitud común acerca de cómo poner la aplicación en una red virtual. Poner la aplicación en una red virtual significa que los puntos de conexión entrantes y salientes de una aplicación en una red virtual. El ASE proporciona la mejor solución para solucionar este problema, pero puede aprovechar lo que se necesita con en el servicio de varios inquilinos mediante la combinación de características. Por ejemplo, puede hospedar las aplicaciones solo de intranet con direcciones privadas de entrada y salidas por:

* Creación de una puerta de enlace de aplicaciones con direcciones privadas de entrada y salida
* Proteger el tráfico entrante a la aplicación con los puntos de conexión de servicio 
* Use la nueva integración con red virtual para que el back-end de la aplicación aparezca en la red virtual 

Este estilo de implementación no podría proporcionarle una dirección dedicada para el tráfico saliente a internet u ofrecen la posibilidad de bloquear todo el tráfico saliente desde la aplicación.  Este estilo de implementación le aportara mucho de lo que solo en caso contrario, obtendría con un ASE. 

### <a name="create-multi-tier-applications"></a>Crear aplicaciones de niveles múltiples

Una aplicación de niveles múltiples es una aplicación donde las aplicaciones de back-end de API solo se pueden acceder desde el nivel de front-end. Para crear una aplicación de varios niveles, hacer lo siguiente:

* Usar integración con red virtual para conectar el back-end de la aplicación web front-end con una subred de una red virtual
* Usar puntos de conexión de servicio para proteger el tráfico entrante a la aplicación de API para solo procedentes de la subred usada por la aplicación web front-end

![aplicación de niveles múltiples](media/networking-features/multi-tier-app.png)

Puede tener varias aplicaciones front-end y usar la misma aplicación de API mediante la integración con redes virtuales de las otras aplicaciones front-end y puntos de conexión de servicio de la aplicación de API con sus subredes.  

<!--Links-->
[appassignedaddress]: https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-custom-ssl#bind-your-ssl-certificate
[iprestrictions]: https://docs.microsoft.com/azure/app-service/app-service-ip-restrictions
[serviceendpoints]: https://docs.microsoft.com/azure/app-service/app-service-ip-restrictions
[hybridconn]: https://docs.microsoft.com/azure/app-service/app-service-hybrid-connections
[vnetintegrationp2s]: https://docs.microsoft.com/azure/app-service/web-sites-integrate-with-vnet
[vnetintegration]: https://docs.microsoft.com/azure/app-service/web-sites-integrate-with-vnet
[networkinfo]: https://docs.microsoft.com/azure/app-service/environment/network-info
