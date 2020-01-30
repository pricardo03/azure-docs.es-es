---
title: Opciones de redes de Azure Functions
description: Introducción a todas las opciones de redes disponibles en Azure Functions.
author: alexkarcher-msft
ms.topic: conceptual
ms.date: 4/11/2019
ms.author: alkarche
ms.openlocfilehash: 79c27d252136281249c217f51019e53987922334
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76846457"
---
# <a name="azure-functions-networking-options"></a>Opciones de redes de Azure Functions

En este artículo se describen las características de redes disponibles en las opciones de hospedaje de Azure Functions. Todas las opciones de redes siguientes ofrecen la posibilidad de acceder a recursos sin usar direcciones enrutables de Internet o de restringir el acceso a Internet a una aplicación de funciones.

Los modelos de hospedaje tienen diferentes niveles de aislamiento de red disponibles. Si elige uno correcto, podrá cumplir los requisitos de aislamiento de red que tenga.

Puede hospedar aplicaciones de funciones de dos formas:

* Hay un conjunto de opciones de plan que se ejecutan en una infraestructura multiinquilino con distintos niveles de conectividad de red virtual y opciones de escalado:
    * El [plan de consumo](functions-scale.md#consumption-plan), que se escala dinámicamente como respuesta a la carga y ofrece opciones de aislamiento de red mínimo.
    * El [plan Premium](functions-scale.md#premium-plan), que también se escala dinámicamente, a la vez que ofrece un aislamiento de red más completo.
    * El [plan de App Service](functions-scale.md#app-service-plan), que funciona a una escala fija y ofrece un aislamiento de red similar al del plan Premium.
* Puede ejecutar funciones en un [App Service Environment](../app-service/environment/intro.md). Este método implementa la función en la red virtual y ofrece aislamiento y control de la red completos.

## <a name="matrix-of-networking-features"></a>Matriz de las características de redes

|                |[Plan de consumo](functions-scale.md#consumption-plan)|[Plan Premium](functions-scale.md#premium-plan)|[plan de App Service](functions-scale.md#app-service-plan)|[entorno de App Service](../app-service/environment/intro.md)|
|----------------|-----------|----------------|---------|-----------------------|  
|[Restricciones de IP de entrada y acceso al sitio privado](#inbound-ip-restrictions)|✅Sí|✅Sí|✅Sí|✅Sí|
|[Integración de redes virtuales](#virtual-network-integration)|❌No|✅Sí (regional)|✅Sí (regional y puerta de enlace)|✅Sí|
|[Desencadenadores de red virtual (no HTTP)](#virtual-network-triggers-non-http)|❌No| ✅Sí |✅Sí|✅Sí|
|[Conexiones híbridas](#hybrid-connections) (solo Windows)|❌No|✅Sí|✅Sí|✅Sí|
|[Restricciones de IP de salida](#outbound-ip-restrictions)|❌No| ❌No|❌No|✅Sí|

## <a name="inbound-ip-restrictions"></a>Restricciones de IP de entrada

Puede usar restricciones de IP para definir una lista de direcciones IP ordenadas por prioridad a las que se les permite o deniega el acceso a la aplicación. La lista puede incluir direcciones IPv4 e IPv6. Si hay una o varias entradas, existe un "denegar a todos" implícito al final de la lista. Las restricciones de IP funcionan con todas las opciones de hospedaje de funciones.

> [!NOTE]
> Con las restricciones de red vigentes, solo puede usar el editor del portal desde la red virtual o si ha incluido en la lista de destinatarios seguros la dirección IP del equipo que usa para acceder a Azure Portal. Sin embargo, todavía puede acceder a todas las características de la pestaña **Características de la plataforma** desde cualquier máquina.

Para obtener más información, consulte [Restricciones de acceso estático de Azure App Service](../app-service/app-service-ip-restrictions.md).

## <a name="private-site-access"></a>El acceso privado a sitios

El acceso privado a sitios se refiere a que la aplicación solo sea accesible desde una red privada como una red virtual de Azure.

* El acceso privado a sitios está disponible en los planes [Premium](./functions-premium-plan.md), [Consumo](functions-scale.md#consumption-plan) y [App Service](functions-scale.md#app-service-plan) si se han configurado puntos de conexión de servicio.
    * Los puntos de conexión de servicio se pueden configurar por aplicación en **Características de la plataforma** > **Redes** > **Configurar restricciones de acceso** > **Agregar regla**. Las redes virtuales ahora se pueden seleccionar como un tipo de regla.
    * Para obtener más información, vea [Puntos de conexión de servicio de red virtual](../virtual-network/virtual-network-service-endpoints-overview.md).
    * Tenga en cuenta que, con los puntos de conexión de servicio, la función todavía tiene acceso de salida completo a Internet, incluso cuando está configurada la integración de red virtual.
* El acceso privado a sitios también está disponible en una instancia de App Service Environment configurada con un equilibrador de carga interno (ILB). Para obtener más información, consulte [Creación y uso de un equilibrador de carga interno con un App Service Environment](../app-service/environment/create-ilb-ase.md).

## <a name="virtual-network-integration"></a>Integración de la red virtual

La integración de red virtual permite que la aplicación de funciones acceda a recursos dentro de una red virtual. Esta característica está disponible en el plan Premium y en el plan de App Service. Si la aplicación está en una instancia de App Service Environment, ya está en una red virtual y no exige integración de red virtual para acceder a recursos de la misma red virtual.

Puede usar la integración de red virtual para habilitar el acceso de aplicaciones a bases de datos y servicios web que se ejecutan en su red virtual. Con la integración de red virtual, no tiene que exponer un punto de conexión público para las aplicaciones de la máquina virtual. Puede usar direcciones privadas de Internet no enrutables en su lugar.

Hay dos formas de integración de red virtual:

+ **Integración de red virtual regional (versión preliminar)** : permite la integración con redes virtuales de la misma región. Este tipo de integración requiere una subred en una red virtual de la misma región. Esta característica todavía está en versión preliminar, pero se admite para las aplicaciones de funciones que se ejecutan en Windows, con las advertencias indicadas después de la siguiente tabla de problemas y soluciones.
+ **Integración de red virtual con requisito de puerta de enlace**: permite la integración con redes virtuales de regiones remotas o con redes virtuales clásicas. Este tipo de integración requiere la implementación de una puerta de enlace de red virtual en la red virtual. Se trata de una característica basada en VPN de punto a sitio que solo se admite para las aplicaciones de funciones que se ejecutan en Windows.

Una aplicación solo puede usar un tipo de la característica de integración de red virtual a la vez. Aunque ambos son útiles para muchos escenarios, en la tabla siguiente se indica dónde se debe usar cada uno:

| Problema  | Solución |
|----------|----------|
| Desea tener acceso a una dirección de RFC 1918 (10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16) en la misma región. | Integración de red virtual regional |
| Quiere acceder a los recursos de una red virtual clásica o una red virtual de otra región | Integración de red virtual con requisito de puerta de enlace |
| Quiere acceder a puntos de conexión de RFC 1918 en Azure ExpressRoute | Integración de red virtual regional |
| Desea tener acceso a los recursos en puntos de conexión de servicio. | Integración de red virtual regional |

Ninguna de las características permite el acceso a direcciones que no sean de RFC 1918 en ExpressRoute. Para ello, actualmente hay que usar App Service Environment.

El uso de la integración de red virtual regional no conecta la red virtual a los puntos de conexión locales ni configura puntos de conexión de servicio. Esa es una configuración de red independiente. La integración de red virtual regional simplemente permite a la aplicación realizar llamadas a través de esos tipos de conexiones.

Independientemente de la versión que se use, la integración de red virtual ofrece a la aplicación de funciones acceso a los recursos de la red virtual, pero no le concede acceso privado a sitios desde la red virtual. El acceso privado a sitios se refiere a hacer a la aplicación accesible solamente desde una red privada como una red virtual de Azure. La integración de red virtual solo sirve para realizar llamadas salientes desde la aplicación a la red virtual.

La característica de integración de red virtual:

* requiere un plan de App Service Estándar, Premium o PremiumV2;
* es compatible con TCP y UDP;
* funciona con aplicaciones de App Service y de Functions.

Hay algunos aspectos que la integración de red virtual no admite, como:

* montar una unidad;
* Integración de Active Directory
* NetBIOS.

La integración de red virtual de Azure Functions usa una infraestructura compartida con las aplicaciones web de App Service. Para obtener más información sobre los dos tipos de integración de red virtual, vea:

* [Integración de red virtual regional](../app-service/web-sites-integrate-with-vnet.md#regional-vnet-integration)
* [Integración de red virtual con requisito de puerta de enlace](../app-service/web-sites-integrate-with-vnet.md#gateway-required-vnet-integration)

Para obtener más información sobre el uso de la integración de red virtual, consulte [Integración de una aplicación de funciones con una red virtual de Azure](functions-create-vnet.md).

## <a name="connecting-to-service-endpoint-secured-resources"></a>Conexión a recursos protegidos del punto de conexión de servicio

> [!NOTE]
> De momento, una vez configuradas las restricciones de acceso en el recurso descendente, los nuevos puntos de conexión de servicio pueden tardar hasta 12 horas en estar disponibles para la aplicación de funciones. Durante este tiempo, el recurso dejará de estar disponible para la aplicación.

Con el fin de proporcionar un mayor nivel de seguridad, puede restringir una serie de servicios de Azure a una red virtual mediante puntos de conexión de servicio. Después debe integrar la aplicación de funciones con esa red virtual para acceder al recurso. Esta configuración es compatible con todos los planes que admiten la integración de redes virtuales.

[Más información sobre los puntos de conexión de servicio de red virtual](../virtual-network/virtual-network-service-endpoints-overview.md).

### <a name="restricting-your-storage-account-to-a-virtual-network"></a>Restricción de la cuenta de almacenamiento a una red virtual

Al crear una aplicación de funciones, debe crear una cuenta de Azure Storage de uso general compatible con Blob, Queue y Table Storage, o vincular a una. En la actualidad, no es posible usar ninguna restricción de red virtual en esta cuenta. Si configura un punto de conexión de servicio de red virtual en la cuenta de almacenamiento que usa para la aplicación de funciones, se interrumpe la aplicación.

[Más información sobre los requisitos de la cuenta de almacenamiento](./functions-create-function-app-portal.md#storage-account-requirements).

### <a name="using-key-vault-references"></a>Usar referencias de almacén de claves 

Las referencias de almacén de claves permiten usar secretos de Azure Key Vault en la aplicación Azure Functions sin necesidad de realizar cambios en el código. Azure Key Vault es un servicio que proporciona administración centralizada de los secretos, con control total sobre las directivas de acceso y el historial de auditoría.

Actualmente las [referencias de almacén de claves](../app-service/app-service-key-vault-references.md) no funcionarán si el almacén de claves está protegido con puntos de conexión de servicio. Para conectarse a un almacén de claves mediante la integración de la red virtual, deberá llamar al almacén de claves en el código de la aplicación.

## <a name="virtual-network-triggers-non-http"></a>Desencadenadores de red virtual (no HTTP)

Actualmente, puede usar funciones de desencadenador no HTTP desde una red virtual de una de estas dos formas: 
+ Ejecute la aplicación de funciones en un plan Premium y habilite la compatibilidad con el desencadenador de red virtual.
+ Ejecute una aplicación de funciones en un plan de App Service o App Service Environment.

### <a name="premium-plan-with-virtual-network-triggers"></a>Plan Premium con desencadenadores de red virtual

Cuando se ejecuta en un plan Premium, puede conectar funciones de desencadenador no HTTP a los servicios que se ejecutan en una red virtual. Para ello, debe habilitar la compatibilidad con el desencadenador de red virtual para la aplicación de funciones. La configuración de la **compatibilidad del desencadenador de red virtual** se encuentra en [Azure Portal](https://portal.azure.com) en **Configuración de aplicación de funciones**.

![VNETToggle](media/functions-networking-options/virtual-network-trigger-toggle.png)

También puede habilitar los desencadenadores de red virtual mediante el siguiente comando de la CLI de Azure:

```azurecli-interactive
az resource update -g <resource_group> -n <function_app_name>/config/web --set properties.functionsRuntimeScaleMonitoringEnabled=1 --resource-type Microsoft.Web/sites
```

Los desencadenadores de red virtual se admiten en la versión 2.x y superior del tiempo de ejecución de Functions. Se admiten los siguientes tipos de desencadenador no HTTP.

| Extensión | Versión mínima |
|-----------|---------| 
|[Microsoft.Azure.WebJobs.Extensions.Storage](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Storage/) | 3.0.10 o superior |
|[Microsoft.Azure.WebJobs.Extensions.EventHubs](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.EventHubs)| 4.1.0 o superior|
|[Microsoft.Azure.WebJobs.Extensions.ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.ServiceBus)| 3.2.0 o superior|
|[Microsoft.Azure.WebJobs.Extensions.CosmosDB](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.CosmosDB)| 3.0.5 o superior|
|[Microsoft.Azure.WebJobs.Extensions.DurableTask](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DurableTask)| 2.0.0 o superior|

> [!IMPORTANT]
> Al habilitar la compatibilidad con el desencadenador de red virtual, solo los tipos de desencadenador anteriores se escalan dinámicamente con la aplicación. Puede usar desencadenadores no indicados anteriormente, pero no se escalan más allá de su recuento de instancias activadas previamente. Consulte los [desencadenadores y enlaces](./functions-triggers-bindings.md#supported-bindings) para obtener una lista completa de los desencadenadores.

### <a name="app-service-plan-and-app-service-environment-with-virtual-network-triggers"></a>Plan de App Service y App Service Environment con desencadenadores de red virtual

Cuando la aplicación de funciones se ejecuta en un plan de App Service o App Service Environment, puede usar funciones de desencadenador no HTTP. Para que las funciones se desencadenen correctamente, debe estar conectado a una red virtual con acceso al recurso definido en la conexión del desencadenador. 

Por ejemplo, imagine que quiere configurar Azure Cosmos DB para aceptar el tráfico solo desde una red virtual. En este caso, debe implementar la aplicación de funciones en un plan de App Service que proporcione integración de red virtual con esa red virtual. Esto permite que un recurso de Azure Cosmos DB desencadene una función. 

## <a name="hybrid-connections"></a>conexiones híbridas

[Conexiones híbridas](../service-bus-relay/relay-hybrid-connections-protocol.md) es una característica de Azure Relay que se puede usar para acceder a recursos de la aplicación en otras redes. Proporciona acceso desde la aplicación a un punto de conexión de la aplicación. No se puede usar para acceder a la aplicación. Las conexiones híbridas están disponibles en todos los planes de Functions en Windows, excepto el plan Consumo.

Dado que se usa en Azure Functions, cada conexión híbrida se correlaciona con una combinación única de host y puerto TCP. Esto significa que el punto de conexión de la conexión híbrida puede estar en cualquier sistema operativo y en cualquier aplicación, siempre que se acceda a un puerto de escucha TCP. La característica Conexiones híbridas no sabe lo que es el protocolo de aplicación ni a qué se accede. Simplemente ofrece acceso a la red.

Para obtener más información, vea la [documentación de App Service para Conexiones híbridas](../app-service/app-service-hybrid-connections.md). Estos mismos pasos de configuración sirven para Azure Functions.

>[!IMPORTANT]
> Las conexiones híbridas solo se admiten en los planes de Windows. No se admite Linux

## <a name="outbound-ip-restrictions"></a>Restricciones de IP de salida

Las restricciones de IP de salida solo están disponibles para las funciones implementadas en App Service Environment. Puede configurar las restricciones de salida de la red virtual en la que está implementado el App Service Environment.

Cuando se integra una aplicación de funciones de un plan Premium o un plan de App Service con una red virtual, la aplicación todavía puede realizar llamadas salientes a Internet.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre las redes y Azure Functions:

* [Siga el tutorial sobre cómo empezar con la integración de red virtual](./functions-create-vnet.md)
* [Lea las preguntas más frecuentes de las redes de Functions](./functions-networking-faq.md)
* [Obtenga más información sobre la integración de red virtual con App Service o Functions](../app-service/web-sites-integrate-with-vnet.md)
* [Obtenga más información sobre las redes virtuales en Azure](../virtual-network/virtual-networks-overview.md)
* [Habilite más características de redes y control con App Service Environment](../app-service/environment/intro.md)
* [Conéctese a recursos locales individuales mediante Conexiones híbridas sin modificar el firewall](../app-service/app-service-hybrid-connections.md)
