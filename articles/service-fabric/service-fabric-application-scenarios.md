---
title: Escenarios y diseño de aplicaciones | Microsoft Docs
description: Información general de las categorías de las aplicaciones de nube en Service Fabric. Describe el diseño de la aplicación que usa servicios con estado y sin estado.
services: service-fabric
documentationcenter: .net
author: athinanthny
manager: chackdan
editor: ''
ms.assetid: 3a8ca6ea-b8e9-4bc3-9e20-262437d2528e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 4/24/2019
ms.author: atsenthi
ms.openlocfilehash: 91e85f762e05c836fe32f5743cc48afed30ae983
ms.sourcegitcommit: e9936171586b8d04b67457789ae7d530ec8deebe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71327288"
---
# <a name="service-fabric-application-scenarios"></a>Escenarios de aplicación de Service Fabric
Azure Service Fabric ofrece una plataforma confiable y flexible que le permite escribir y ejecutar muchos tipos de aplicaciones y servicios empresariales. Estas aplicaciones y microservicios pueden tener estado o no tenerlo y se equilibran mediante recursos en las máquinas virtuales para maximizar su eficacia. 

La arquitectura única de Service Fabric le permite realizar análisis de datos casi en tiempo real, cálculo en memoria, transacciones paralelas y procesamiento de eventos en sus aplicaciones. Puede escalar o reducir verticalmente sus aplicaciones con facilidad (en realidad hacia dentro o hacia fuera), dependiendo de sus requisitos de recursos variables.

Para instrucciones de diseño sobre la compilación de aplicaciones, consulte [Arquitectura de microservicios en Azure Service Fabric](https://docs.microsoft.com/azure/architecture/reference-architectures/microservices/service-fabric) y [Procedimientos recomendados para el diseño de aplicaciones de Azure Service Fabric](service-fabric-best-practices-applications.md).

Considere la opción de usar la plataforma Service Fabric de Azure para los siguientes tipos de aplicaciones:

* **Recopilación de datos, procesamiento e IoT**: Service Fabric se ocupa de sus servicios con estado a gran escala y tiene latencia baja. Puede ayudar a procesar los datos de millones de dispositivos donde se colocan los datos del dispositivo y el cálculo.

    Algunos de los clientes que han compilado servicios de IoT mediante Service Fabric son [Honeywell](https://customers.microsoft.com/story/honeywell-builds-microservices-based-thermostats-on-azure), [PCL Construction](https://customers.microsoft.com/story/pcl-construction-professional-services-azure), [Crestron](https://customers.microsoft.com/story/crestron-partner-professional-services-azure), [BMW](https://customers.microsoft.com/story/bmw-enables-driver-mobility-via-azure-service-fabric/), [ Schneider Electric](https://customers.microsoft.com/story/schneider-electric-powers-engergy-solutions-on-azure-service-fabric) y [Mesh Systems](https://customers.microsoft.com/story/mesh-systems-lights-up-the-market-with-iot-based-azure-solutions).

* **Juegos y aplicaciones interactivas basadas en sesión**: Service Fabric es útil si la aplicación requiere operaciones de lectura y escritura con latencia baja, con el juego en línea o la mensajería instantánea. Service Fabric le permite crear estas aplicaciones con estado interactivas sin tener que crear un almacén o memoria caché independientes. Visite las [soluciones para juegos de Azure](https://azure.microsoft.com/solutions/gaming/) para una guía de diseño sobre el [uso de Service Fabric en los servicios para juegos](https://docs.microsoft.com/gaming/azure/reference-architectures/multiplayer-synchronous-sf).

    Los clientes que han compilado servicios para juegos incluyen [Next Games](https://customers.microsoft.com/story/next-games-media-telecommunications-azure) y [Digamore](https://customers.microsoft.com/story/digamore-entertainment-scores-with-a-new-gaming-platform-based-on-azure-service-fabric/). Los clientes que han compilado sesiones interactivas incluyen [Honeywell con Hololens](https://customers.microsoft.com/story/honeywell-manufacturing-hololens).

* **Procesamiento de análisis y flujos de trabajo de datos**: Las aplicaciones que deben procesar de forma confiable eventos o flujos de datos se benefician de operaciones de lectura y escritura optimizadas en Service Fabric. Service Fabric también admite canalizaciones de procesamiento de aplicaciones donde los resultados deben ser confiables y pasar a la siguiente fase de procesamiento sin pérdidas. Estas canalizaciones incluyen sistemas transaccionales y financieros donde las garantías de procesamiento y coherencia de datos son fundamentales.

    Los clientes que han compilado servicios de flujo de trabajo empresarial incluyen [Zeiss grupo](https://customers.microsoft.com/story/zeiss-group-focuses-on-azure-service-fabric-for-key-integration-platform), [Quorum Business Solutions](https://customers.microsoft.com/en-us/story/quorum-business-solutions-expand-energy-managemant-solutions-using-azure-service-fabric) y [Société General](https://customers.microsoft.com/en-us/story/societe-generale-speeds-real-time-market-quotes-using-azure-service-fabric).

* **Cálculo de datos**: Service Fabric le permite compilar aplicaciones con estado que realizan cálculos de datos intensivos. Service Fabric permite la colocación de los datos y el procesamiento (cálculo) en las aplicaciones. 

   Normalmente, cuando la aplicación requiere acceso a datos, la latencia de red asociada con un nivel de caché o almacenamiento de datos externos limita el tiempo de cálculo. Los servicios de Service Fabric con estado eliminan esa latencia, lo cual optimiza las operaciones de lectura y escritura. 
   
   Por ejemplo, supongamos una aplicación que realiza una selección de recomendaciones en tiempo real para los clientes con un requisito de tiempo de ida y vuelta inferior a 100 milisegundos. Las características de latencia y rendimiento de los servicios de Service Fabric proporcionan una experiencia con capacidad de respuesta al usuario en comparación con el modelo de implementación estándar de tener que recuperar los datos necesarios del almacenamiento remoto. El sistema tiene mayor capacidad de respuesta, ya que el cálculo de la selección de recomendaciones se coloca con los datos y las reglas.

    Los clientes que han compilado servicios de cálculo incluyen [Solidsoft respuesta](https://customers.microsoft.com/story/solidsoft-reply-platform-powers-e-verification-of-pharmaceuticals) y [Infosupport](https://customers.microsoft.com/story/service-fabric-customer-profile-info-support-and-fudura).

* **Servicios de alta disponibilidad**: Service Fabric proporciona conmutación por error rápida mediante la creación de varias réplicas de servicio secundarias. Si un nodo, proceso o servicio individual deja de funcionar por un error de hardware u otro fallo, una de las réplicas secundarias se amplía a una réplica principal con una pérdida mínima de servicio.

* **Servicios escalables**: los servicios individuales pueden tener particiones, permitiendo que el estado se escale horizontalmente en el clúster. También pueden crearse y eliminarse servicios individuales sobre la marcha. Puede escalar horizontalmente servicios de algunas instancias en unos nodos a miles de instancias en gran cantidad de nodos y después reducirlos según sus necesidades. Puede usar Service Fabric para compilar estos servicios y administrar su ciclo de vida completo.

## <a name="application-design-case-studies"></a>Casos prácticos de diseño de aplicaciones
En los sitios de [experiencias de los clientes](https://customers.microsoft.com/search?sq=%22Azure%20Service%20Fabric%22&ff=&p=0&so=story_publish_date%20desc/) y [Microservicios en Azure](https://azure.microsoft.com/solutions/microservice-applications/) se publican estudios de casos que muestran el uso de Service Fabric para diseñar aplicaciones.

## <a name="designing-applications-composed-of-stateless-and-stateful-microservices"></a>Diseño de aplicaciones compuestas por microservicios sin y con estado
La compilación de aplicaciones con roles de trabajo de Azure Cloud Services es un ejemplo de servicio sin estado. Por el contrario, los microservicios con estado mantienen el estado autorizado más allá de la solicitud y su respuesta. Esta funcionalidad proporciona alta disponibilidad y coherencia del estado mediante API sencillas que proporcionan garantías transaccionales respaldadas por la replicación. 

Los servicios con estado de Service Fabric llevan la alta disponibilidad a todos los tipos de aplicaciones, no solo a las bases de datos y otros almacenes de datos. Se trata de una progresión natural. Las aplicaciones ya han pasado de usar bases de datos puramente relacionales de alta disponibilidad a bases de datos NoSQL. Ahora las mismas aplicaciones pueden tener su estado "activo" y sus datos administrados en ellas para mejoras adicionales del rendimiento sin sacrificar la confiabilidad, la coherencia o la disponibilidad.

Al compilar aplicaciones formadas por microservicios, suele tener una combinación de aplicaciones web sin estado (por ejemplo, ASP.NET y Node.js) que llaman a servicios empresariales de nivel intermedio con y sin estado. Las aplicaciones y los servicios se implementan en el mismo clúster de Service Fabric mediante los comandos de implementación de Service Fabric. Cada uno de estos servicios es independiente en cuanto al uso de recursos, la confiabilidad y la escalabilidad. Esta independencia mejora la agilidad y la flexibilidad en el desarrollo y la administración del ciclo de vida.

Los microservicios con estado simplifican los diseños de la aplicación porque acaban con la necesidad de colas y memorias caché adicionales tradicionalmente necesarias para cumplir los requisitos de disponibilidad y latencia de una aplicación puramente sin estado. Dado que los servicios con estado tienen alta disponibilidad y latencia baja, hay menos detalles para administrar en la aplicación. 

En los siguientes diagramas se muestran las diferencias entre diseñar una aplicación sin estado y otra con estado. Al aprovecharse los modelos de programación de [Reliable Services](service-fabric-reliable-services-introduction.md) y [Reliable Actors](service-fabric-reliable-actors-introduction.md), los servicios con estado reducen la complejidad de la aplicación, al mismo tiempo que se obtienen un alto rendimiento y una baja latencia.

Esta es una aplicación de ejemplo que usa servicios sin estado: ![Aplicación que utiliza servicios sin estado][Image1]

Esta es una aplicación de ejemplo que usa servicios con estado: ![Aplicación que utiliza servicios con estado][Image2]

## <a name="next-steps"></a>Pasos siguientes

* Más información sobre [patrones y escenarios](service-fabric-patterns-and-scenarios.md).

* Empiece a crear servicios sin y con estado con los modelos de programación [Reliable Services](service-fabric-reliable-services-quick-start.md) y [Reliable Actors](service-fabric-reliable-actors-get-started.md) de Service Fabric.
* Visite el Centro de arquitectura de Azure para una guía sobre la [compilación de microservicios en Azure](https://docs.microsoft.com/azure/architecture/microservices/).
* Vaya a [Procedimientos recomendados para aplicaciones y clústeres de Azure Service Fabric](service-fabric-best-practices-overview.md) para una guía de diseño de aplicaciones.

* Vea también los siguientes temas:
  * [Quiero información acerca de los microservicios](service-fabric-overview-microservices.md)
  * [Estado de servicio](service-fabric-concepts-state.md)
  * [Disponibilidad de los servicios de Service Fabric](service-fabric-availability-services.md)
  * [Escalación de aplicaciones de Service Fabric](service-fabric-concepts-scalability.md)
  * [Cómo crear particiones de los servicios confiables de Service Fabric](service-fabric-concepts-partitioning.md)

[Image1]: media/service-fabric-application-scenarios/AppwithStatelessServices.png
[Image2]: media/service-fabric-application-scenarios/AppwithStatefulServices.png
