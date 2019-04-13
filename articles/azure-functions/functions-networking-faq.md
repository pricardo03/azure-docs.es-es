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
ms.openlocfilehash: 126b9ccefedee1f5cefdac8a8666a58e7a4a1fef
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/13/2019
ms.locfileid: "59548657"
---
# <a name="frequently-asked-questions-about-networking-in-azure-functions"></a>Preguntas más frecuentes acerca de las redes en Azure Functions

A continuación es una lista de preguntas más frecuentes de red. Para obtener una introducción más completa, lea el [funciones de documento de las opciones de redes](functions-networking-options.md)

## <a name="how-do-i-set-a-static-ip-in-functions"></a>¿Cómo se puede establecer una dirección IP estática en las funciones?

Implementación de una función en App Service Environment (ASE) es actualmente la única manera de tener una dirección IP entrante y saliente estática para la función. Para obtener más información sobre el uso de un ASE, comience con el artículo aquí: [Creación y uso de un ASE de ILB](../app-service/environment/create-ilb-ase.md).

## <a name="how-do-i-restrict-internet-access-to-my-function"></a>¿Cómo se puede restringir acceso a internet a mi función?

Puede restringir el acceso a internet en un número de formas, enumeradas a continuación.

* [Las restricciones de IP](../app-service/app-service-ip-restrictions.md): restringir el tráfico entrante a la aplicación de función por intervalo de IP.
* Quite todos los desencadenadores HTTP. Para algunas aplicaciones, es suficiente para simplemente evitar desencadenadores HTTP y usar cualquier otro origen de evento para desencadenar la función.

La consideración más importante de este modo es que tener en cuenta que el editor de Azure portal requiere acceso directo a la función de ejecución para utilizar. Cambios de código a través del portal de Azure requerirá que el dispositivo que está usando para examinar el portal para tener su en la lista blanca IP. No obstante, sin embargo, sirven nada en la pestaña de características de plataforma con restricciones de red en su lugar.

## <a name="how-do-i-restrict-my-function-app-to-a-vnet"></a>¿Cómo se restringe mi aplicación de función a una red virtual?

La única forma de restringir totalmente una función de modo que todo el tráfico fluye a través de una red virtual es usar un internamente con equilibrio de carga (ILB) de App Service Environment (ASE). Esta opción implementa el sitio en una infraestructura dedicada en una red virtual y envía todos los desencadenadores y tráfico a través de la red virtual. Para obtener más información sobre el uso de un ASE, comience con el artículo aquí: [Creación y uso de un ASE de ILB](../app-service/environment/create-ilb-ase.md).

## <a name="how-can-i-access-resources-in-a-vnet-from-a-function-app"></a>¿Cómo acceder a recursos en una red virtual desde una aplicación de función?

Puede tener acceso a recursos en una red virtual desde una función de ejecución mediante la integración con red virtual. Para obtener más información, consulte [integración con red virtual](functions-networking-options.md#vnet-integration)

## <a name="how-do-i-access-resources-protected-by-service-endpoints"></a>¿Cómo puedo acceder a recursos protegidos por los extremos de servicio?

Mediante la integración con red virtual nueva (actualmente en versión preliminar), puede tener acceso a los recursos protegidos de una función de la ejecución de punto de conexión de servicio. Para obtener más información, consulte [obtener una vista previa de integración con red virtual](functions-networking-options.md#preview-vnet-integration).

## <a name="how-can-i-trigger-a-function-from-a-resource-in-a-vnet"></a>¿Cómo puedo desencadenar una función de un recurso en una red virtual?

Solo puede desencadenar una función de un recurso en una red virtual mediante la implementación de la aplicación de función en un entorno de App Service. Para obtener más información sobre el uso de un ASE, consulte [creación y uso de un ASE de ILB](../app-service/environment/create-ilb-ase.md).


## <a name="how-can-i-deploy-my-function-app-in-a-vnet"></a>¿Cómo puedo implementar mi aplicación de función en una red virtual?

Implementación en un entorno de App Service es la única forma de crear una aplicación de función que está totalmente dentro de una red virtual para obtener más información sobre el uso de un ASE de ILB, comience con el artículo aquí: [Creación y uso de un ASE de ILB](https://docs.microsoft.com/azure/app-service/environment/create-ilb-ase).

Para escenarios donde solamente necesita unidireccional acceso a recursos de redes virtuales, o menos aislamiento de red completa, consulte el [funciones de información general de redes](functions-networking-options.md).

## <a name="next-steps"></a>Pasos siguientes

Para más información acerca de las redes y funciones: 

* [Siga nuestra red virtual integración tutorial de introducción](./functions-create-vnet.md)
* [Más información sobre las opciones de red de las funciones aquí](./functions-networking-options.md)
* [Más información sobre la integración de la red virtual con App Service y Functions](../app-service/web-sites-integrate-with-vnet.md)
* [Más información sobre las redes virtuales en Azure](../virtual-network/virtual-networks-overview.md)
* [Habilitar más características de red y control con entornos de App Service](../app-service/environment/intro.md)
