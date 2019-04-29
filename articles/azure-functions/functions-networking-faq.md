---
title: Preguntas más frecuentes acerca de las redes en Azure Functions
description: Respuestas a algunas de las preguntas y escenarios de redes con Azure Functions más comunes.
services: functions
author: alexkarcher-msft
manager: jeconnoc
ms.service: azure-functions
ms.topic: troubleshooting
ms.date: 4/11/2019
ms.author: alkarche, glenga
ms.openlocfilehash: 3cf6a0d080e2d8cafcab8e69a614b59a470c7aba
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60637053"
---
# <a name="frequently-asked-questions-about-networking-in-azure-functions"></a>Preguntas más frecuentes acerca de las redes en Azure Functions

En este artículo se enumera las preguntas más frecuentes acerca de las redes en Azure Functions. Para obtener una introducción más completa, consulte [funciones de las opciones de red](functions-networking-options.md).

## <a name="how-do-i-set-a-static-ip-in-functions"></a>¿Cómo se puede establecer una dirección IP estática en las funciones?

Implementación de una función en un entorno de App Service es actualmente la única manera de tener una dirección IP entrante y saliente estática para la función. Para obtener más información sobre el uso de un entorno de App Service, empiece con el artículo [creación y uso de un equilibrador de carga interno con App Service Environment](../app-service/environment/create-ilb-ase.md).

## <a name="how-do-i-restrict-internet-access-to-my-function"></a>¿Cómo se puede restringir acceso a internet a mi función?

Puede restringir el acceso a internet en un par de formas:

* [Las restricciones de IP](../app-service/app-service-ip-restrictions.md): Restringir el tráfico entrante a la aplicación de función por intervalo de IP.
* Eliminación de todos los desencadenadores HTTP. Para algunas aplicaciones, es suficiente para simplemente evitar desencadenadores HTTP y usar cualquier otro origen de evento para desencadenar la función.

Tenga en cuenta que el editor de Azure portal requiere acceso directo a la función de ejecución. Cambios de código a través del portal de Azure requerirá que el dispositivo que está usando para examinar el portal para tener su en la lista blanca IP. Pero puede usar cualquier cosa en la pestaña de características de plataforma con las restricciones de red en su lugar.

## <a name="how-do-i-restrict-my-function-app-to-a-virtual-network"></a>¿Cómo se restringe mi aplicación de función a una red virtual?

La única forma de restringir totalmente una función de modo que todo el tráfico fluye a través de una red virtual es usar un entorno de App Service internamente con equilibrio de carga. Esta opción implementa el sitio en una infraestructura dedicada en una red virtual y envía todos los desencadenadores y tráfico a través de la red virtual. 

Para obtener más información sobre el uso de un entorno de App Service, empiece con el artículo [creación y uso de un equilibrador de carga interno con App Service Environment](../app-service/environment/create-ilb-ase.md).

## <a name="how-can-i-access-resources-in-a-virtual-network-from-a-function-app"></a>¿Cómo acceder a recursos en una red virtual desde una aplicación de función?

Puede tener acceso a recursos en una red virtual desde una función de ejecución mediante la integración de red virtual. Para obtener más información, consulte [integración de red Virtual](functions-networking-options.md#virtual-network-integration).

## <a name="how-do-i-access-resources-protected-by-service-endpoints"></a>¿Cómo puedo acceder a recursos protegidos por los extremos de servicio?

Mediante la integración de red virtual (actualmente en versión preliminar), puede tener acceso a recursos protegidos de punto de conexión de servicio desde una función de ejecución. Para obtener más información, consulte [obtener una vista previa de la integración de red virtual](functions-networking-options.md#preview-version-of-virtual-network-integration).

## <a name="how-can-i-trigger-a-function-from-a-resource-in-a-virtual-network"></a>¿Cómo puedo desencadenar una función de un recurso en una red virtual?

Puede desencadenar una función de un recurso en una red virtual solo mediante la implementación de la aplicación de función en un entorno de App Service. Para obtener más información sobre el uso de un entorno de App Service, consulte [creación y uso de un equilibrador de carga interno con App Service Environment](../app-service/environment/create-ilb-ase.md).


## <a name="how-can-i-deploy-my-function-app-in-a-virtual-network"></a>¿Cómo puedo implementar mi aplicación de función en una red virtual?

Implementación en un entorno de App Service es la única forma de crear una aplicación de función que está totalmente dentro de una red virtual. Para obtener más información sobre el uso de un equilibrador de carga interno con App Service Environment, empiece con el artículo [creación y uso de un equilibrador de carga interno con App Service Environment](https://docs.microsoft.com/azure/app-service/environment/create-ilb-ase).

Para escenarios donde necesita sólo unidireccional acceso a recursos de red virtual, o menos aislamiento de red completa, consulte el [funciones de información general de redes](functions-networking-options.md).

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información acerca de las redes y las funciones: 

* [Siga el tutorial sobre cómo empezar con la integración de red virtual](./functions-create-vnet.md)
* [Más información sobre las opciones de red de Azure Functions](./functions-networking-options.md)
* [Más información sobre la integración de red virtual con App Service y Functions](../app-service/web-sites-integrate-with-vnet.md)
* [Más información sobre redes virtuales en Azure](../virtual-network/virtual-networks-overview.md)
* [Habilitar más características de red y control con entornos de App Service](../app-service/environment/intro.md)
