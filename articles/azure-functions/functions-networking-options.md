---
title: Opciones de redes de Azure Functions
description: Información general de todas las opciones de redes disponibles en Azure Functions
services: functions
author: alexkarcher-msft
manager: jeconnoc
ms.service: azure-functions
ms.topic: conceptual
ms.date: 4/11/2019
ms.author: alkarche
ms.openlocfilehash: f4f081001f2573bccc58205ccc7955739b7f5c4c
ms.sourcegitcommit: 6cbf5cc35840a30a6b918cb3630af68f5a2beead
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/05/2019
ms.locfileid: "68779293"
---
# <a name="azure-functions-networking-options"></a>Opciones de redes de Azure Functions

En este artículo se describen las características de redes disponibles en las opciones de hospedaje de Azure Functions. Todas las opciones de redes que se indican a continuación proporcionan la posibilidad de acceder a recursos sin usar direcciones enrutables de Internet o restringir el acceso a Internet a una aplicación de funciones. 

Los modelos de hospedaje tienen diferentes niveles de aislamiento de red disponibles. Si elige uno correcto, podrá cumplir los requisitos de aislamiento de red que tenga.

Puede hospedar aplicaciones de funciones de dos formas:

* Hay un conjunto de opciones de plan que se ejecutan en una infraestructura multiinquilino, con distintos niveles de conectividad de red virtual y opciones de escalado:
    * El [plan de consumo](functions-scale.md#consumption-plan), que se escala dinámicamente como respuesta a la carga y ofrece opciones de aislamiento de red mínimo.
    * El [plan Premium](functions-scale.md#premium-plan), que también se escala dinámicamente, a la vez que ofrece un aislamiento de red más completo.
    * El [plan de Azure App Service](functions-scale.md#app-service-plan), que funciona en una escala fija y ofrece un aislamiento de red similar al del plan Premium.
* Puede ejecutar funciones en un [App Service Environment](../app-service/environment/intro.md). Este método implementa la función en la red virtual y ofrece aislamiento y control de la red completos.

## <a name="matrix-of-networking-features"></a>Matriz de las características de redes

|                |[Plan de consumo](functions-scale.md#consumption-plan)|[Plan Premium (versión preliminar)](functions-scale.md#premium-plan)|[plan de App Service](functions-scale.md#app-service-plan)|[entorno de App Service](../app-service/environment/intro.md)|
|----------------|-----------|----------------|---------|-----------------------|  
|[Restricciones de IP de entrada y acceso al sitio privado](#inbound-ip-restrictions)|✅Sí|✅Sí|✅Sí|✅Sí|
|[Integración de redes virtuales](#virtual-network-integration)|❌No|✅Sí (regional)|✅ Sí (regional y puerta de enlace)|✅Sí|
|[Desencadenadores de red virtual (no HTTP)](#virtual-network-triggers-non-http)|❌No| ❌No|✅Sí|✅Sí|
|[conexiones híbridas](#hybrid-connections)|❌No|❌No|✅Sí|✅Sí|
|[Restricciones de IP de salida](#outbound-ip-restrictions)|❌No| ❌No|❌No|✅Sí|


## <a name="inbound-ip-restrictions"></a>Restricciones de IP de entrada

Puede usar las restricciones de IP para definir una lista de direcciones IP ordenadas por prioridad a las que se les permite o deniega el acceso a la aplicación. La lista puede incluir direcciones IPv4 e IPv6. Si hay una o varias entradas, hay una denegación implícita de todo lo que existe al final de la lista. Las restricciones de IP funcionan con todas las opciones de hospedaje de funciones.

> [!NOTE]
> Con las restricciones de red vigentes, solo puede usar el editor del portal desde la red virtual o cuando haya incluido en la lista de permitidos la dirección IP de la máquina que usa para tener acceso a Azure Portal. Sin embargo, todavía puede acceder a todas las características de la pestaña **Características de la plataforma** desde cualquier máquina.

Para obtener más información, consulte [Restricciones de acceso estático de Azure App Service](../app-service/app-service-ip-restrictions.md).

## <a name="private-site-access"></a>El acceso privado a sitios

El acceso privado a sitios se refiere a que solo se puede acceder a la aplicación desde una red privada (por ejemplo, desde dentro de una red virtual de Azure). 
* El acceso privado a sitios está disponible en los planes [Premium](./functions-premium-plan.md) y de [App Service](functions-scale.md#app-service-plan) si se han configurado **puntos de conexión de servicio**. Para obtener más información, consulte [Puntos de conexión de servicio de red virtual](../virtual-network/virtual-network-service-endpoints-overview.md).
    * Tenga en cuenta que, con los puntos de conexión de servicio, la función todavía tiene acceso saliente completo a Internet, incluso cuando está configurada la integración de red virtual.
* El acceso privado a sitios también está disponible con un App Service Environment configurado con un equilibrador de carga interno (ILB). Para obtener más información, consulte [Creación y uso de un equilibrador de carga interno con un App Service Environment](../app-service/environment/create-ilb-ase.md).

## <a name="virtual-network-integration"></a>Integración de la red virtual

La integración de red virtual permite que la aplicación de funciones acceda a recursos dentro de una red virtual. Esta característica está disponible en el plan Premium y en el plan de App Service. Si la aplicación está en un App Service Environment, ya está en una red virtual y no requiere que se use la integración de red virtual para acceder a recursos que están en la misma red virtual.

Puede usar la integración de red virtual para habilitar el acceso de aplicaciones a bases de datos y servicios web que se ejecutan en su red virtual. Con la integración de red virtual, no tiene que exponer un punto de conexión público para las aplicaciones de la máquina virtual. En su lugar, puede usar las direcciones privadas y enrutables que no son de Internet.

La característica Integración de la red virtual presenta dos formas.

1. La integración de redes virtuales regionales permite la integración con redes virtuales en la misma región. Esta forma de la característica requiere que haya una subred en una red virtual en la misma región. Esta característica está aún en versión preliminar, pero se admite en cargas de trabajo de producción de aplicaciones Windows con las correspondientes salvedades señaladas en cada caso.
2. La integración de red virtual con requisito de puerta de enlace permite la integración con redes virtuales en regiones remotas o con redes virtuales clásicas. Esta versión de la característica requiere la implementación de una puerta de enlace de red virtual en la red virtual. Esta es la característica basada en VPN de punto a sitio y solo es compatible con aplicaciones de Windows.

Una aplicación puede usar solo una forma de la característica Integración con red virtual a la vez. La incógnita, pues, es qué característica usar. Puede usar cada una de ellas para muchas cosas. Estas son las diferencias más patentes entre ambas:

| Problema  | Solución | 
|----------|----------|
| Desea tener acceso a una dirección de RFC 1918 (10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16) en la misma región. | Integración con red virtual regional |
| Quiere tener acceso a una red virtual clásica o a una red virtual en otra región. | Integración con red virtual con requisito de puerta de enlace |
| Desea tener acceso a puntos de conexión de RFC 1918 en ExpressRoute. | Integración con red virtual regional |
| Desea tener acceso a los recursos en puntos de conexión de servicio. | Integración con red virtual regional |

Ninguna de las dos características permitirá el acceso a direcciones que no sean de RFC 1918 en ExpressRoute. Para ello, deberá usar un ASE por ahora.

El uso de Integración con red virtual regional no conecta la red virtual al entorno local ni configura puntos de conexión de servicio. Esa es otra configuración de red aparte. Integración con red virtual regional simplemente permite a la aplicación realizar llamadas a través de esos tipos de conexiones.

Independientemente de la versión que se use, la integración de redes virtuales ofrece a su aplicación de funciones acceso a los recursos de su red virtual, pero no concede a su aplicación de funciones acceso privado a sitios desde la red virtual. El acceso al sitio privado se refiere a que la aplicación solo es accesible desde una red privada, como desde dentro de una red virtual de Azure. Integración con red virtual solo sirve para realizar llamadas salientes desde la aplicación hacia la red virtual. 

La característica Integración con red virtual:

* requiere un plan de App Service Estándar, Premium o PremiumV2;
* es compatible con TCP y UDP;
* funciona con aplicaciones de App Service y de Functions.

Hay algunos aspectos que Integración con red virtual no admite, como:

* montar una unidad;
* la integración de AD; 
* NetBios;

La integración de redes virtuales en Functions usa una infraestructura compartida con las aplicaciones web de App Service. Para obtener más información acerca de los dos tipos de integración de redes virtuales, consulte:
* [Integración con red virtual regional](../app-service/web-sites-integrate-with-vnet.md#regional-vnet-integration)
* [Integración con red virtual con requisito de puerta de enlace](../app-service/web-sites-integrate-with-vnet.md#gateway-required-vnet-integration)

Para obtener más información sobre el uso de la integración de red virtual, consulte [Integración de una aplicación de funciones con una red virtual de Azure](functions-create-vnet.md).

## <a name="virtual-network-triggers-non-http"></a>Desencadenadores de red virtual (no HTTP)

Actualmente, para poder usar desencadenadores de Functions que no sean HTTP desde dentro de una red virtual, debe ejecutar la aplicación de funciones en un plan de App Service o en un App Service Environment.

Como ejemplo, si quiere configurar Azure Cosmos DB para aceptar solo el tráfico de una red virtual, debe implementar la aplicación de funciones en un plan de App Service que permita la integración de la red virtual con esa red virtual. De este modo, podría configurar los desencadenadores de Azure Cosmos DB de ese recurso. En la versión preliminar, la configuración de la integración de red virtual no permitirá que el plan Premium se desencadene en ese recurso de Azure Cosmos DB.

Consulte [esta lista de todos los desencadenares que no son HTTP](./functions-triggers-bindings.md#supported-bindings) para volver a comprobar la compatibilidad.

## <a name="hybrid-connections"></a>conexiones híbridas

[Conexiones híbridas](../service-bus-relay/relay-hybrid-connections-protocol.md) es una característica de Azure Relay que se puede usar para acceder a recursos de la aplicación en otras redes. Proporciona acceso desde la aplicación a un punto de conexión de la aplicación. No se puede usar para acceder a la aplicación. Conexiones híbridas está disponible para las funciones que se ejecutan en un [plan de App Service](functions-scale.md#app-service-plan) y un [App Service Environment](../app-service/environment/intro.md).

Dado que se usa en Azure Functions, cada conexión híbrida se correlaciona con una combinación única de host y puerto TCP. Esto significa que el punto de conexión de la conexión híbrida puede estar en cualquier sistema operativo y en cualquier aplicación, siempre que se acceda a un puerto de escucha TCP. La característica Conexiones híbridas no sabe lo que es el protocolo de aplicaciones ni sabe a qué se accede. Simplemente ofrece acceso a la red.

Para obtener más información, consulte la [documentación de App Service de Conexiones híbridas](../app-service/app-service-hybrid-connections.md), que es compatible con Functions en un plan de App Service.

## <a name="outbound-ip-restrictions"></a>Restricciones de IP de salida

Las restricciones de IP de salida solo están disponibles para las funciones implementadas en un App Service Environment. Puede configurar las restricciones de salida de la red virtual en la que está implementado el App Service Environment.

Cuando se integra una aplicación de funciones en un plan Premium o un plan de App Service con una red virtual, la aplicación todavía puede realizar llamadas salientes a Internet.

## <a name="next-steps"></a>Pasos siguientes
Para obtener más información sobre las redes y Azure Functions: 

* [Siga el tutorial sobre cómo empezar con la integración de red virtual](./functions-create-vnet.md)
* [Lea las preguntas más frecuentes de las redes de Functions](./functions-networking-faq.md)
* [Obtenga más información sobre la integración de red virtual con App Service o Functions](../app-service/web-sites-integrate-with-vnet.md)
* [Obtenga más información sobre las redes virtuales en Azure](../virtual-network/virtual-networks-overview.md)
* [Habilite más características de redes y control con App Service Environment](../app-service/environment/intro.md)
* [Conéctese a recursos locales individuales mediante Conexiones híbridas sin modificar el firewall](../app-service/app-service-hybrid-connections.md)
