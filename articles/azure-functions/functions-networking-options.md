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
ms.openlocfilehash: a0bb34f8a43199a5d3a18064bce92ef4bec543af
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67050649"
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
|[Restricciones de IP de entrada](#inbound-ip-restrictions)|✅Sí|✅Sí|✅Sí|✅Sí|
|[Restricciones de IP de salida](#private-site-access)|❌No| ❌No|❌No|✅Sí|
|[Integración de redes virtuales](#virtual-network-integration)|❌No|❌No|✅Sí|✅Sí|
|[Versión preliminar de la integración de red virtual (Azure ExpressRoute y salida de puntos de conexión de servicio)](#preview-version-of-virtual-network-integration)|❌No|✅Sí|✅Sí|✅Sí|
|[conexiones híbridas](#hybrid-connections)|❌No|❌No|✅Sí|✅Sí|
|[Acceso privado a sitios](#private-site-access)|❌No| ✅Sí|✅Sí|✅Sí|

## <a name="inbound-ip-restrictions"></a>Restricciones de IP de entrada

Puede usar las restricciones de IP para definir una lista de direcciones IP ordenadas por prioridad a las que se les permite o deniega el acceso a la aplicación. La lista puede incluir direcciones IPv4 e IPv6. Si hay una o varias entradas, hay una denegación implícita de todo lo que existe al final de la lista. Las restricciones de IP funcionan con todas las opciones de hospedaje de funciones.

> [!NOTE]
> Para usar el editor de Azure Portal, el portal debe poder acceder directamente a la aplicación de funciones en ejecución. Además, el dispositivo que use para acceder al portal debe tener su IP en la lista de permitidos. Con las restricciones de red en su lugar, todavía puede acceder a todas las características de la pestaña **Características de la plataforma**.

Para obtener más información, consulte [Restricciones de acceso estático de Azure App Service](../app-service/app-service-ip-restrictions.md).

## <a name="outbound-ip-restrictions"></a>Restricciones de IP de salida

Las restricciones de IP de salida solo están disponibles para las funciones implementadas en un App Service Environment. Puede configurar las restricciones de salida de la red virtual en la que está implementado el App Service Environment.

## <a name="virtual-network-integration"></a>Integración de la red virtual

La integración de red virtual permite que la aplicación de funciones acceda a recursos dentro de una red virtual. Esta característica está disponible en el plan Premium y en el plan de App Service. Si la aplicación está en un App Service Environment, ya está en una red virtual y no requiere que se use la integración de red virtual para acceder a recursos que están en la misma red virtual.

La integración de red virtual ofrece a su aplicación de funciones acceso a los recursos de su red virtual, pero no concede [acceso privado a sitios](#private-site-access) a su aplicación de funciones desde la red virtual.

Puede usar la integración de red virtual para habilitar el acceso de aplicaciones a bases de datos y servicios web que se ejecutan en su red virtual. Con la integración de red virtual, no tiene que exponer un punto de conexión público para las aplicaciones de la máquina virtual. En su lugar, puede usar las direcciones privadas y enrutables que no son de Internet.

La versión disponible con carácter general de la integración de red virtual depende de una puerta de enlace VPN para conectar las aplicaciones de funciones a una red virtual. Está disponible en las funciones hospedadas en un plan de App Service. Para obtener información sobre cómo configurar esta característica, consulte [Integración de su aplicación con una instancia de Azure Virtual Network](../app-service/web-sites-integrate-with-vnet.md).

### <a name="preview-version-of-virtual-network-integration"></a>Versión preliminar de la integración de red virtual

Hay una nueva versión de la característica Integración de red virtual que está en versión preliminar. No depende de la VPN de punto a sitio. Admite el acceso a los recursos a través de ExpressRoute o puntos de conexión de servicio. Está disponible en el plan Premium y en los planes de App Service escalados hasta PremiumV2.

Estas son algunas características de esta versión:

* No necesita una puerta de enlace para usarla.
* Puede acceder a los recursos a través de conexiones de ExpressRoute sin más configuración que la integración con la red virtual conectada a ExpressRoute.
* Puede consumir los recursos protegidos del punto de conexión de servicio de las funciones en ejecución. Para ello, habilite los puntos de conexión de servicio en la subred usada para la integración de red virtual.
* No puede configurar desencadenadores para usar los recursos protegidos del punto de conexión de servicio. 
* La aplicación de funciones y la red virtual deben estar en la misma región.
* La nueva característica requiere una subred sin usar en la red virtual que ha implementado mediante Azure Resource Manager.
* No se admiten las cargas de trabajo de producción mientras la característica se encuentra en versión preliminar.
* Las tablas de rutas y el emparejamiento global aún no están disponibles con la característica.
* Se usa una dirección para cada posible instancia de una aplicación de funciones. Puesto que no puede cambiar el tamaño de la subred después de la asignación, use una subred que pueda admitir fácilmente su tamaño máximo de escala. Por ejemplo, para admitir un plan Premium que se puede escalar a 80 instancias, se recomienda una subred `/25` que proporciona 126 direcciones de host.

Para obtener más información sobre el uso de la versión preliminar de la integración de red virtual, consulte [Integración de una aplicación de funciones con una red virtual de Azure](functions-create-vnet.md).

## <a name="hybrid-connections"></a>conexiones híbridas

[Conexiones híbridas](../service-bus-relay/relay-hybrid-connections-protocol.md) es una característica de Azure Relay que se puede usar para acceder a recursos de la aplicación en otras redes. Proporciona acceso desde la aplicación a un punto de conexión de la aplicación. No se puede usar para acceder a la aplicación. Conexiones híbridas está disponible para las funciones que se ejecutan en un [plan de App Service](functions-scale.md#app-service-plan) y un [App Service Environment](../app-service/environment/intro.md).

Dado que se usa en Azure Functions, cada conexión híbrida se correlaciona con una combinación única de host y puerto TCP. Esto significa que el punto de conexión de la conexión híbrida puede estar en cualquier sistema operativo y en cualquier aplicación, siempre que se acceda a un puerto de escucha TCP. La característica Conexiones híbridas no sabe lo que es el protocolo de aplicaciones ni sabe a qué se accede. Simplemente ofrece acceso a la red.

Para obtener más información, consulte la [documentación de App Service de Conexiones híbridas](../app-service/app-service-hybrid-connections.md), que es compatible con Functions en un plan de App Service.

## <a name="private-site-access"></a>El acceso privado a sitios

El acceso privado a sitios se refiere a que solo se puede acceder a la aplicación desde una red privada (por ejemplo, desde dentro de una red virtual de Azure). 
* El acceso privado a sitios está disponible en los planes Premium y de App Service si se han configurado **puntos de conexión de servicio**. Para obtener más información, consulte [Puntos de conexión de servicio de red virtual](../virtual-network/virtual-network-service-endpoints-overview.md).
    * Tenga en cuenta que, con los puntos de conexión de servicio, la función todavía tiene acceso saliente completo a Internet, incluso con la integración con red virtual configurada.
* El acceso privado a sitios solo está disponible con un App Service Environment configurado con un equilibrador de carga interno (ILB). Para obtener más información, consulte [Creación y uso de un equilibrador de carga interno con un App Service Environment](../app-service/environment/create-ilb-ase.md).

Hay muchas maneras de acceder a recursos de red virtual de otras opciones de hospedaje. Pero un App Service Environment es la única forma de permitir que los desencadenadores de una función se produzcan a través de una red virtual.

## <a name="next-steps"></a>Pasos siguientes
Para obtener más información sobre las redes y Azure Functions: 

* [Siga el tutorial sobre cómo empezar con la integración de red virtual](./functions-create-vnet.md)
* [Lea las preguntas más frecuentes de las redes de Functions](./functions-networking-faq.md)
* [Obtenga más información sobre la integración de red virtual con App Service o Functions](../app-service/web-sites-integrate-with-vnet.md)
* [Obtenga más información sobre las redes virtuales en Azure](../virtual-network/virtual-networks-overview.md)
* [Habilite más características de redes y control con App Service Environment](../app-service/environment/intro.md)
* [Conéctese a recursos locales individuales mediante Conexiones híbridas sin modificar el firewall](../app-service/app-service-hybrid-connections.md)
