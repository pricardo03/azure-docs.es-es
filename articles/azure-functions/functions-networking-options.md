---
title: Las opciones de red de Azure Functions
description: Información general de todas las opciones de red disponibles en Azure Functions
services: functions
author: alexkarcher-msft
manager: jeconnoc
ms.service: azure-functions
ms.topic: conceptual
ms.date: 4/11/2019
ms.author: alkarche
ms.openlocfilehash: f13e498859986d5ee697cbd67907fd344147ed0c
ms.sourcegitcommit: 600d5b140dae979f029c43c033757652cddc2029
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66492833"
---
# <a name="azure-functions-networking-options"></a>Las opciones de red de Azure Functions

Este artículo describe las características de redes disponibles a través de las opciones de alojamiento para Azure Functions. Todas las siguientes opciones de red proporcionan la posibilidad de tener acceso a recursos sin usar direcciones enrutable de internet o restringir el acceso a internet a una aplicación de función. 

Los modelos de hospedaje tienen diferentes niveles de aislamiento de red disponible. Elegir uno correcto le ayudará a cumplir los requisitos de aislamiento de red.

Puede hospedar aplicaciones de función en un par de formas:

* Hay un conjunto de opciones de plan que se ejecutan en una infraestructura para varios inquilinos, con distintos niveles de la conectividad de red virtual y opciones de escalado:
    * El [plan de consumo](functions-scale.md#consumption-plan), que se escala dinámicamente en respuesta a la carga y ofrece opciones de aislamiento de red mínimos.
    * El [plan Premium](functions-scale.md#premium-plan-public-preview), que también se escala dinámicamente, al tiempo que ofrece aislamiento de red más completo.
    * Azure [plan de App Service](functions-scale.md#app-service-plan), que funciona en una escala fija y ofrece aislamiento de red similar al plan Premium.
* Puede ejecutar funciones en un [App Service Environment](../app-service/environment/intro.md). Este método implementa la función en la red virtual y ofrece control completo de la red y aislamiento.

## <a name="matrix-of-networking-features"></a>Matriz de características de red

|                |[Plan de consumo](functions-scale.md#consumption-plan)|[Plan Premium (versión preliminar)](functions-scale.md#premium-plan-public-preview)|[plan de App Service](functions-scale.md#app-service-plan)|[entorno de App Service](../app-service/environment/intro.md)|
|----------------|-----------|----------------|---------|-----------------------|  
|[Las restricciones de IP de entrada](#inbound-ip-restrictions)|✅Yes|✅Yes|✅Yes|✅Yes|
|[Restricciones de IP de salida](#private-site-access)|❌No| ❌No|❌No|✅Yes|
|[Integración de redes virtuales](#virtual-network-integration)|❌No|❌No|✅Yes|✅Yes|
|[Obtener una vista previa de integración de red virtual (Azure ExpressRoute y los extremos de servicio saliente)](#preview-version-of-virtual-network-integration)|❌No|✅Yes|✅Yes|✅Yes|
|[conexiones híbridas](#hybrid-connections)|❌No|❌No|✅Yes|✅Yes|
|[acceso privado a sitios](#private-site-access)|❌No| ✅Yes|✅Yes|✅Yes|

## <a name="inbound-ip-restrictions"></a>Las restricciones de IP de entrada

Puede usar las restricciones de IP para definir una lista ordenada por prioridad de las direcciones IP que se permite o deniega el acceso a la aplicación. La lista puede incluir direcciones IPv4 e IPv6. Cuando hay una o varias entradas, implícita "Denegar todo" existe al final de la lista. Las restricciones de IP funcionan con todas las opciones de hospedaje de función.

> [!NOTE]
> Para usar el editor de Azure portal, el portal debe poder tener acceso directamente a la aplicación de función en ejecución. Además, el dispositivo que se usa para acceder al portal debe tener su en la lista blanca IP. Con las restricciones de red en su lugar, todavía puede tener acceso a todas las características en el **características de la plataforma** ficha.

Para obtener más información, consulte [restricciones de acceso estáticos de Azure App Service](../app-service/app-service-ip-restrictions.md).

## <a name="outbound-ip-restrictions"></a>Restricciones de IP saliente

Restricciones de IP saliente solo están disponibles para las funciones implementadas en un entorno de App Service. Puede configurar restricciones de salida para la red virtual donde se implementa el entorno de App Service.

## <a name="virtual-network-integration"></a>Integración de la red virtual

Integración de virtual network permite que la aplicación de función tener acceso a recursos dentro de una red virtual. Esta característica está disponible en el plan Premium y el plan de App Service. Si la aplicación está en un entorno de App Service, ya está en una red virtual y no requiere el uso de la integración de red virtual para tener acceso a recursos en la misma red virtual.

Integración de red virtual proporciona el acceso a la aplicación de función a los recursos de la red virtual, pero no concede [acceso privado a sitios](#private-site-access) a la aplicación de función de la red virtual.

Puede usar la integración de red virtual para permitir el acceso desde aplicaciones de bases de datos y servicios web que se ejecutan en la red virtual. Con la integración de red virtual, no es necesario exponer un punto de conexión público para las aplicaciones en la máquina virtual. Puede usar las direcciones privadas de internet que no son enrutables en su lugar.

La versión disponible con carácter general de la integración de red virtual se basa en una puerta de enlace VPN para conectarse a aplicaciones de función a una red virtual. Está disponible en las funciones hospedadas en un plan de App Service. Para obtener información sobre cómo configurar esta característica, consulte [integrar su aplicación con una red virtual](../app-service/web-sites-integrate-with-vnet.md).

### <a name="preview-version-of-virtual-network-integration"></a>Versión preliminar de integración de red virtual

Una nueva versión de la característica de integración de la red virtual está en versión preliminar. No depende de VPN de punto a sitio. Admite el acceso a los recursos a través de ExpressRoute o puntos de conexión de servicio. Está disponible en el plan Premium y en los planes de App Service, escalados hasta PremiumV2.

Estas son algunas características de esta versión:

* No necesita una puerta de enlace para utilizarlo.
* Puede acceder a los recursos a través de conexiones de ExpressRoute sin más configuración que la integración con la red virtual conectada a ExpressRoute.
* Puede consumir los recursos protegidos de punto de conexión de servicio se ejecuten las funciones. Para ello, habilite los puntos de conexión de servicio en la subred usada para la integración de red virtual.
* No se puede configurar desencadenadores para usar recursos protegidos de punto de conexión de servicio. 
* La aplicación de función y la red virtual deben estar en la misma región.
* La nueva característica requiere una subred sin usar en la red virtual que ha implementado mediante Azure Resource Manager.
* No se admiten las cargas de trabajo de producción mientras la característica está en versión preliminar.
* Las tablas de rutas y el emparejamiento global no están aún disponibles con la característica.
* Se usa una dirección para cada instancia potencial de una aplicación de función. Dado que no se puede cambiar el tamaño de la subred después de la asignación, utilice una subred que puede admitir fácilmente el tamaño de la escala máxima. Por ejemplo, para admitir un plan Premium que se puede escalar hasta 80 instancias, se recomienda un `/25` subred que proporciona 126 direcciones de host.

Para más información sobre el uso de la versión preliminar de integración de red virtual, consulte [integrar una aplicación de función con una red virtual](functions-create-vnet.md).

## <a name="hybrid-connections"></a>conexiones híbridas

[Conexiones híbridas](../service-bus-relay/relay-hybrid-connections-protocol.md) es una característica de Azure Relay que puede usar para tener acceso a recursos de la aplicación en otras redes. Proporciona acceso desde la aplicación a un punto de conexión de la aplicación. No puede usarlo para acceder a la aplicación. Conexiones híbridas está disponible para las funciones que se ejecutan en un [plan de App Service](functions-scale.md#app-service-plan) y un [App Service Environment](../app-service/environment/intro.md).

Tal como se utiliza en Azure Functions, cada conexión híbrida se correlaciona con una combinación única de host y el puerto TCP. Esto significa que el punto de conexión de la conexión híbrida puede estar en cualquier sistema operativo y cualquier aplicación, mientras que está accediendo a un puerto de escucha TCP. La característica conexiones híbridas no sabe ni necesita atención, ¿qué es el protocolo de aplicación, o que está accediendo. Simplemente proporciona acceso a la red.

Para obtener más información, consulte el [documentación de App Service para las conexiones híbridas](../app-service/app-service-hybrid-connections.md), que es compatible con las funciones en un plan de App Service.

## <a name="private-site-access"></a>El acceso privado a sitios

Acceso al sitio privado se refiere a que la aplicación sea accesible solo desde una red privada, como desde dentro de una red virtual de Azure. 
* Acceso privado a sitios está disponible en el Premium y App Service plan cuando **los extremos de servicio** están configurados. Para obtener más información, consulte [puntos de conexión de servicio de red virtual](../virtual-network/virtual-network-service-endpoints-overview.md)
    * Tenga en cuenta que con los puntos de conexión de servicio, la función todavía tiene acceso completo saliente a internet, incluso con la integración con red virtual configurada.
* Acceso privado a sitios está disponible solo con un entorno de App Service configurados con un equilibrador de carga interno (ILB). Para obtener más información, consulte [creación y uso de un equilibrador de carga interno con App Service Environment](../app-service/environment/create-ilb-ase.md).

Hay muchas maneras de obtener acceso a recursos de red virtual de otras opciones de hospedaje. Pero un entorno de App Service es la única manera de permitir que los desencadenadores para una función que se produzca a través de una red virtual.

## <a name="next-steps"></a>Pasos siguientes
Para más información acerca de las funciones de red y Azure: 

* [Siga el tutorial sobre cómo empezar con la integración de red virtual](./functions-create-vnet.md)
* [Leer las funciones de preguntas más frecuentes de red](./functions-networking-faq.md)
* [Más información sobre la integración con la aplicación de servicio o funciones de red virtual](../app-service/web-sites-integrate-with-vnet.md)
* [Más información sobre redes virtuales en Azure](../virtual-network/virtual-networks-overview.md)
* [Habilitar más características de red y control con entornos de App Service](../app-service/environment/intro.md)
* [Conectarse a recursos individuales de forma local sin cambios en el firewall mediante conexiones híbridas](../app-service/app-service-hybrid-connections.md)
