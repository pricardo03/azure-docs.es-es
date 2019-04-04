---
title: Opciones de red de Azure Functions
description: Información general de todas las opciones de red disponibles en Azure Functions
services: functions
author: alexkarcher-msft
manager: jehollan
ms.service: azure-functions
ms.topic: conceptual
ms.date: 1/14/2019
ms.author: alkarche
ms.openlocfilehash: 8bbc44e7af68f005f30fff143741bc4bfe0adcf2
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2019
ms.locfileid: "58896643"
---
# <a name="azure-functions-networking-options"></a>Opciones de red de Azure Functions

Este documento describe el conjunto de características de redes disponibles a través de las opciones de hospedaje de Azure Functions. Todas las siguientes opciones de red proporcionan la posibilidad de tener acceso a recursos sin usar direcciones enrutable de internet o restringir el acceso a internet a una aplicación de función. Todos los modelos de hospedaje tienen diferentes niveles de aislamiento de red disponible y elegir uno correcto le permitirá satisfacer los requisitos de aislamiento de red.

Función de las aplicaciones puede hospedarse de distintas maneras.

* Hay un conjunto de opciones de plan que se ejecutan en infraestructura de varios inquilinos, con distintos niveles de la conectividad de red virtual y opciones de escalado.
    1. El plan de consumo, que se escala dinámicamente en respuesta a la carga y ofrece opciones de aislamiento de red mínimos.
    1. El Plan Premium, que también se escala dinámicamente, al tiempo que ofrece aislamiento de red más completo.
    1. El Plan de App Service, que funciona en una escala fija y ofrece aislamiento de red similar al plan Premium.
* También se pueden ejecutar funciones en App Service Environment (ASE) que implementa la función en la red virtual y ofrece control completo de la red y aislamiento.

## <a name="networking-feature-matrix"></a>Matriz de características de red

|                |[Plan de consumo](functions-scale.md#consumption-plan)|⚠ [Plan Premium](functions-scale.md##premium-plan-public-preview)|[Plan de servicio de aplicación](functions-scale.md#app-service-plan)|[Entorno de App Service](../app-service/environment/intro.md)|
|----------------|-----------|----------------|---------|-----------------------|  
|[**Restricciones de IP de entrada**](#inbound-ip-restrictions)|✅Yes|✅Yes|✅Yes|✅Yes|
|[**Integración con redes virtuales**](#vnet-integration)|❌No|⚠ Sí|✅Yes|✅Yes|
|[**Integración con redes virtuales Preview (Express Route & puntos de conexión de servicio)**](#preview-vnet-integration)|❌No|⚠ Sí|⚠ Sí|✅Yes|
|[**conexiones híbridas**](#hybrid-connections)|❌No|❌No|✅Yes|✅Yes|
|[**Acceso al sitio de privada**](#private-site-access)|❌No| ❌No|❌No|✅Yes|

⚠ Característica de vista previa, no para su uso en producción

## <a name="inbound-ip-restrictions"></a>Restricciones de IP de entrada

Las restricciones de direcciones IP le permiten definir una lista de direcciones IP ordenadas por prioridad que tienen permiso para acceder a la aplicación. La lista de elementos permitidos puede incluir direcciones IPv4 e IPv6. Cuando hay una o varias entradas, hay una denegación implícita de todo lo que existe al final de la lista. La funcionalidad de las restricciones de IP funciona con la función de todas las opciones de hospedaje.

> ! [IMPORTANTE] Para poder usar el editor de Azure portal, el portal debe ser capaz de acceder directamente a la aplicación de función de ejecución y el dispositivo que se usa para acceder al portal debe tener su en la lista blanca IP. Con las restricciones de red en su lugar, es posible tener acceso cualquier característica en el **características de la plataforma** ficha.

[Obtenga más información aquí](https://docs.microsoft.com/azure/app-service/app-service-ip-restrictions)

## <a name="vnet-integration"></a>Integración con redes virtuales

Integración con red virtual permite que la aplicación de función tener acceso a recursos dentro de una red virtual. Integración con redes virtuales está disponible en el plan Premium y el plan de App Service. Si la aplicación está en un entorno de App Service, ya está en una red virtual y no requiere el uso de la característica de integración con red virtual para tener acceso a recursos en la misma red virtual.

Integración con red virtual proporciona el acceso a la aplicación de función a los recursos de la red virtual, pero no concede [acceso privado a sitios](#private-site-access) a la aplicación de función de la red virtual.

La integración con red virtual se usa a menudo para habilitar el acceso de aplicaciones a bases de datos y servicios web que se ejecutan en su red virtual. Con Integración con red virtual, no es necesario exponer un punto de conexión público para las aplicaciones en su máquina virtual, sino que puede usar las direcciones privadas no enrutables sin conexión a Internet en su lugar.

La versión disponible con carácter general de la integración con red virtual se basa en una puerta de enlace VPN para conectarse a aplicaciones de función a una red virtual. Está disponible en las funciones que se hospeda en un plan de app service. Para obtener información sobre cómo configurar esta característica, consulte el [documento de servicio de aplicación de la misma función](../app-service/web-sites-integrate-with-vnet.md#enabling-vnet-integration).

### <a name="preview-vnet-integration"></a>Integración de la red virtual de versión preliminar

Hay una nueva versión de la característica Integración con red virtual que está en versión preliminar. No depende de la VPN de punto a sitio y admite el acceso a los recursos a través de ExpressRoute o de los puntos de conexión de servicio. Esta característica está disponible en el plan Premium y en los planes de App Service, escalados hasta PremiumV2.

La nueva versión de la integración con red virtual, que se encuentra actualmente en versión preliminar, proporciona las siguientes ventajas:

* No es necesaria ninguna puerta de enlace para usar la nueva característica de integración con red virtual.
* Puede acceder a recursos en conexiones de ExpressRoute sin ninguna configuración adicional, aparte de la integración con la red virtual conectada a ExpressRoute.
* Puede consumir los recursos protegidos de ejecutar las funciones de punto de conexión de servicio. Para ello, habilite los puntos de conexión de servicio en la subred usada para la integración con red virtual.
* No se puede configurar desencadenadores para usar el extremo de servicio que protegen recursos mediante la nueva funcionalidad de integración con red virtual. 
* La aplicación de función y la red virtual deben estar en la misma región.
* La nueva característica requiere una subred sin usar en la red virtual de Resource Manager.
* Las cargas de trabajo de producción no se admiten en la nueva versión de la integración con red virtual mientras está en versión preliminar.
* Las tablas de rutas y el emparejamiento global no están aún disponibles con la nueva integración con red virtual.
* Se usa una dirección para cada instancia de aplicación de función posibles. Dado que no se puede cambiar el tamaño de la subred después de la asignación, utilice una subred que puede admitir fácilmente el tamaño de la escala máxima. Por ejemplo, para admitir un plan Premium que se puede escalar hasta 80 instancias, se recomienda un `/25` subred que proporciona 126 direcciones de host.

Para más información sobre el uso de la integración de red virtual de versión preliminar, consulte [integrar una aplicación de función con una red Virtual de Azure](functions-create-vnet.md).

## <a name="hybrid-connections"></a>conexiones híbridas

[Conexiones híbridas](../service-bus-relay/relay-hybrid-connections-protocol.md) es una característica de Azure Relay que se puede usar para tener acceso a recursos de la aplicación en otras redes. Proporciona acceso desde la aplicación a un punto de conexión de la aplicación. No se puede usar para tener acceso a la aplicación. Conexiones híbridas está disponible para las funciones que se ejecutan en un [plan de App Service](functions-scale.md#app-service-plan) y un [App Service Environment](../app-service/environment/intro.md).

Tal como se utiliza en funciones, cada conexión híbrida se correlaciona con una combinación única de host y el puerto TCP. Esto significa que el punto de conexión híbrida puede estar en cualquier sistema operativo y en cualquier aplicación, siempre y cuando se acceda a un puerto de escucha TCP. La característica Conexiones híbridas no sabe lo que es el protocolo de aplicaciones ni sabe a qué se accede. Simplemente ofrece acceso a la red.

Para obtener más información, consulte el [documentación de App Service para las conexiones híbridas](../app-service/app-service-hybrid-connections.md), que es compatible con las funciones y las aplicaciones Web.

## <a name="private-site-access"></a>Acceso al sitio de privada

El acceso al sitio privado se refiere a que la aplicación solo es accesible desde una red privada, como desde dentro de una red virtual de Azure. El acceso privado al sitio solo está disponible con un ASE configurado con un Equilibrador de carga interno (ILB). Para obtener más información sobre el uso de un ASE de ILB, consulte [creación y uso de un ASE de ILB](../app-service/environment/create-ilb-ase.md).

Hay muchas maneras de acceder a recursos de red virtual de otras opciones de hospedaje, pero un ASE es la única manera de permitir que los desencadenadores para una función que se produzca a través de una red virtual.
