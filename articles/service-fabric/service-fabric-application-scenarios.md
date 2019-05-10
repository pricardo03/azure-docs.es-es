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
ms.openlocfilehash: f7ad295eb30d257cd195ae02d5a5b1d85de76bda
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/07/2019
ms.locfileid: "65228505"
---
# <a name="service-fabric-application-scenarios"></a>Escenarios de aplicación de Service Fabric
Azure Service Fabric ofrece una plataforma confiable y flexible que le permite escribir y ejecutar muchos tipos de aplicaciones y servicios empresariales. Estas aplicaciones y microservicios pueden tener estado o no tenerlo, y se equilibran mediante recursos en las máquinas virtuales para maximizar su eficacia. La arquitectura única de Service Fabric le permite realizar análisis de datos casi en tiempo real, cálculo en memoria, transacciones paralelas y procesamiento de eventos en sus aplicaciones. Puede escalar o reducir verticalmente sus aplicaciones con facilidad (en realidad hacia dentro o hacia fuera), dependiendo de sus requisitos de recursos variables.

Para obtener instrucciones de diseño sobre la creación de aplicaciones, leer [arquitectura de microservicios en Azure Service Fabric](https://docs.microsoft.com/azure/architecture/reference-architectures/microservices/service-fabric) y [procedimientos recomendados para diseñar aplicaciones con Service Fabric](service-fabric-best-practices-applications.md)

La plataforma Service Fabric es ideal para los siguientes tipos de aplicaciones:

* **Recopilación de datos, procesamiento e IoT**: Dado que Service Fabric trata a gran escala y baja latencia a través de sus servicios con estado, es ideal para el procesamiento de datos en millones de dispositivos donde se colocan los datos del dispositivo y el cálculo.

    Clientes que han desarrollado los servicios de IoT mediante Service Fabric incluyen [Honeywell](https://customers.microsoft.com/story/honeywell-builds-microservices-based-thermostats-on-azure), [PCL Construction](https://customers.microsoft.com/story/pcl-construction-professional-services-azure), [Crestron](https://customers.microsoft.com/story/crestron-partner-professional-services-azure), [BMW](https://customers.microsoft.com/story/bmw-enables-driver-mobility-via-azure-service-fabric/), [ Schneider Electric](https://customers.microsoft.com/story/schneider-electric-powers-engergy-solutions-on-azure-service-fabric), y [Mesh Systems](https://customers.microsoft.com/story/mesh-systems-lights-up-the-market-with-iot-based-azure-solutions).

* **Aplicaciones interactivas de juegos y basados en sesión**: Service Fabric es ideal si la aplicación requiere una latencia baja lecturas y escrituras, como en los juegos en línea o mensajería instantánea. Service Fabric permite crear estas aplicaciones con estado interactivas sin tener que crear un almacén independiente o la memoria caché. Visite [soluciones de Azure para juegos](https://azure.microsoft.com/solutions/gaming/) para obtener instrucciones sobre diseño [mediante Service Fabric en servicios de juegos](https://docs.microsoft.com/gaming/azure/reference-architectures/multiplayer-synchronous-sf)

    Clientes que han desarrollado los servicios de juegos incluyen [Next Games](https://customers.microsoft.com/story/next-games-media-telecommunications-azure) y [Digamore](https://customers.microsoft.com/story/digamore-entertainment-scores-with-a-new-gaming-platform-based-on-azure-service-fabric/). Clientes que han desarrollado sesiones interactivas incluyen [Honeywell con Hololens](https://customers.microsoft.com/story/honeywell-manufacturing-hololens)

* **Análisis de datos y procesamiento de flujo de trabajo**: Aplicaciones que se deben procesar de forma confiable eventos o secuencias de datos también benefician las optimizado lecturas y escrituras en Service Fabric. Además, Service Fabric admite canalizaciones de procesamiento de aplicación, donde los resultados deben ser confiables y pasar a la siguiente fase de procesamiento sin pérdida. Estos incluyen sistemas transaccionales y financieros, donde las garantías de procesamiento y coherencia de datos son fundamentales.

    Clientes que han desarrollado los servicios de flujo de trabajo de negocio incluyen [Zeiss grupo](https://customers.microsoft.com/story/zeiss-group-focuses-on-azure-service-fabric-for-key-integration-platform) y [Digamore](https://customers.microsoft.com/story/digamore-entertainment-scores-with-a-new-gaming-platform-based-on-azure-service-fabric/)

* **Cálculo de datos**: Service Fabric permite compilar aplicaciones con estado de datos cálculo intensivo. Service Fabric permite la colocación de procesamiento (cálculo) y los datos en las aplicaciones. Normalmente, cuando la aplicación requiere acceso a datos, la latencia de red asociada con un nivel de caché o almacenamiento de datos externos limita el tiempo de cálculo. Con servicios de Service Fabric con estado, esa latencia se elimina, lo que permite más optimizado para las lecturas y escrituras. Por ejemplo, considere una aplicación que realiza una selección de recomendaciones en tiempo real para los clientes, con un requisito de tiempo de ida y vuelta de menos de 100 milisegundos. Las características de latencia y rendimiento de servicios de Service Fabric (donde el cálculo de la selección de la recomendación es comparte ubicación con los datos y reglas) proporciona una capacidad de respuesta al usuario en comparación con el modelo de implementación estándar de tener que recuperar los datos necesarios del almacenamiento remoto.

    Clientes que han desarrollado los servicios de cálculo incluyen [Solidsoft respuesta](https://customers.microsoft.com/story/solidsoft-reply-platform-powers-e-verification-of-pharmaceuticals) y [Infosupport](https://customers.microsoft.com/story/service-fabric-customer-profile-info-support-and-fudura)

* **Servicios de alta disponibilidad**: Service Fabric ofrece conmutación por error rápida mediante la creación de varias réplicas de servicio secundario. Si un nodo, proceso o servicio individual deja de funcionar por un error de hardware u otro fallo, una de las réplicas secundarias se amplía a una réplica principal con una pérdida mínima de servicio.

* **Servicios escalables**: los servicios individuales pueden tener particiones, permitiendo que el estado se escale horizontalmente en el clúster. Además, pueden crearse y eliminarse servicios individuales sobre la marcha. Los servicios pueden ser de forma rápida y fácil escalar horizontalmente de unas pocas instancias de varios nodos a miles de instancias en muchos nodos y, a continuación, reducen horizontalmente en nuevo, según sus necesidades de recursos. Puede usar a Service Fabric para crear estos servicios y administrar sus ciclos de vida completadas.

## <a name="application-design-case-studies"></a>Casos prácticos de diseño de aplicaciones
Un número de casos prácticos que muestra cómo se usa Service Fabric para diseñar aplicaciones se publica en [testimonios de clientes](https://customers.microsoft.com/search?sq=%22Azure%20Service%20Fabric%22&ff=&p=0&so=story_publish_date%20desc/) y [sitio de soluciones de microservicios](https://azure.microsoft.com/solutions/microservice-applications/).

## <a name="design-applications-composed-of-stateless-and-stateful-microservices"></a>Diseño de aplicaciones compuestas por microservicios sin y con estado
La creación de aplicaciones con roles de trabajo del servicio en la nube de Azure es un ejemplo de servicios sin estado. Por el contrario, los microservicios con estado mantienen el estado autorizado más allá de la solicitud y su respuesta. Esta funcionalidad proporciona alta disponibilidad y coherencia del estado a través de API sencillas que proporcionan garantías transaccionales respaldadas por la replicación. Los servicios con estado de Service Fabric democratizan la alta disponibilidad, llevándola a todos los tipos de aplicaciones, no solo a las bases de datos y otros almacenes de datos. Se trata de una progresión natural. Las aplicaciones ya han pasado de usar bases de datos puramente relacionales de alta disponibilidad a bases de datos NoSQL. Ahora las mismas aplicaciones pueden tener su estado "activo" y sus datos administrados en ellas para mejoras adicionales del rendimiento sin sacrificar la confiabilidad, la coherencia o la disponibilidad.

Al crear aplicaciones que constan de microservicios, se suele tener una combinación de aplicaciones web sin estado (ASP.NET, node.js, etc.) que llaman a servicios de nivel intermedio de negocio sin y con estado, todos ellos implementados en el mismo clúster de Service Fabric mediante los comandos de implementación de Service Fabric. Cada uno de estos servicios es independiente con respecto a escala, confiabilidad y el uso de recursos, lo que mejora significativamente la agilidad y flexibilidad en el desarrollo y la administración del ciclo de vida.

Los microservicios con estado simplifican los diseños de la aplicación porque acaban con la necesidad de colas y memorias caché adicionales tradicionalmente necesarias para cumplir los requisitos de disponibilidad y latencia de una aplicación puramente sin estado. Dado que los servicios con estado naturalmente tienen alta disponibilidad y latencia baja, hay menos partes móviles para administrar de la aplicación como un todo. En los siguientes diagramas se muestran las diferencias entre diseñar una aplicación sin estado y otra con estado. Al aprovecharse los modelos de programación de [Reliable Services](service-fabric-reliable-services-introduction.md) y [Reliable Actors](service-fabric-reliable-actors-introduction.md), los servicios con estado reducen la complejidad de la aplicación, al mismo tiempo que se obtienen un alto rendimiento y una baja latencia.

## <a name="an-application-built-using-stateless-services"></a>Una aplicación compilada con servicios sin estado
![Aplicación que hace uso del servicio sin estado][Image1]

## <a name="an-application-built-using-stateful-services"></a>Una aplicación compilada con servicios con estado
![Aplicación que hace uso del servicio sin estado][Image2]

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Pasos siguientes

* Información sobre [casos prácticos de clientes](https://customers.microsoft.com/search?sq=%22Azure%20Service%20Fabric%22&ff=&p=0&so=story_publish_date%20desc)
* Más información sobre [patrones y escenarios](service-fabric-patterns-and-scenarios.md)

* Empiece a crear servicios sin y con estado con los modelos de programación [Reliable Services](service-fabric-reliable-services-quick-start.md) y [Reliable Actors](service-fabric-reliable-actors-get-started.md) de Service Fabric.
* Visite el centro de arquitectura de Azure para obtener instrucciones sobre [crear microservicios en Azure](https://docs.microsoft.com/azure/architecture/microservices/)
* Vaya a [prácticas recomendadas de clúster y una aplicación de Azure Service Fabric](service-fabric-best-practices-overview.md) para obtener instrucciones de diseño de aplicación.

* Vea también los siguientes temas:
  * [Quiero información acerca de los microservicios](service-fabric-overview-microservices.md)
  * [Estado de servicio](service-fabric-concepts-state.md)
  * [Disponibilidad de los servicios de Service Fabric](service-fabric-availability-services.md)
  * [Escalación de aplicaciones de Service Fabric](service-fabric-concepts-scalability.md)
  * [Cómo crear particiones de los servicios confiables de Service Fabric](service-fabric-concepts-partitioning.md)

[Image1]: media/service-fabric-application-scenarios/AppwithStatelessServices.jpg
[Image2]: media/service-fabric-application-scenarios/AppwithStatefulServices.jpg
