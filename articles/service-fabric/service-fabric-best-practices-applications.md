---
title: Procedimientos recomendados para el diseño de aplicaciones de Azure Service Fabric | Microsoft Docs
description: Procedimientos recomendados para el desarrollo de aplicaciones de Service Fabric.
services: service-fabric
documentationcenter: .net
author: markfussell
manager: chackdan
editor: ''
ms.assetid: 19ca51e8-69b9-4952-b4b5-4bf04cded217
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/26/2019
ms.author: msfussell
ms.openlocfilehash: 55f043effc7cdb102acea856e89c58f660d0cde5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65237753"
---
# <a name="azure-service-fabric-application-design-best-practices"></a>Procedimientos recomendados para el diseño de aplicaciones de Azure Service Fabric

En este artículo se proporciona una guía de procedimientos recomendados para compilar aplicaciones y servicios en Service Fabric.
 
## <a name="get-familiar-with-service-fabric"></a>Familiarizarse con Service Fabric
* [¿Qué desea saber sobre Service Fabric?](service-fabric-content-roadmap.md)
* Obtenga información sobre los [Escenarios de aplicación de Service Fabric](service-fabric-application-scenarios.md)
* A continuación, comprenda las opciones de modelos de programación con una [introducción a los modelos de programación de Service Fabric](service-fabric-choose-framework.md)



## <a name="application-design-guidance"></a>Guía de diseño de aplicaciones
Familiarícese con la [arquitectura general](https://docs.microsoft.com/azure/architecture/reference-architectures/microservices/service-fabric) de una aplicación de Service Fabric y sus [consideraciones de diseño](https://docs.microsoft.com/azure/architecture/reference-architectures/microservices/service-fabric#design-considerations).

### <a name="choose-an-api-gateway"></a>Elegir una puerta de enlace de API
Use un servicio de puerta de enlace de API que se comunique con servicios de back-end y que, a continuación, se pueda escalar horizontalmente. Los servicios de puerta de enlace de API más comúnmente utilizados son:

- [Azure API Management](https://docs.microsoft.com/azure/service-fabric/service-fabric-api-management-overview), que está [integrada con Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-tutorial-deploy-api-management)
- [Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub/) o [Azure Event Hubs](https://docs.microsoft.com/azure/event-hubs/) con [ServiceFabricProcessor](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/ServiceFabricProcessor) para leer a partir de las particiones de Event Hubs
- [Proxy inverso de Træfik](https://blogs.msdn.microsoft.com/azureservicefabric/2018/04/05/intelligent-routing-on-service-fabric-with-traefik/) con [proveedor de Azure Service Fabric](https://docs.traefik.io/configuration/backends/servicefabric/)
- [Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/) Nota: No está integrado directamente con Service Fabric y Azure API Management normalmente es la opción preferida.
- Compilar sus propias puertas de enlace de aplicaciones web [ASP.NET Core](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-communication-aspnetcore)

### <a name="choose-stateless-services"></a>Elegir servicios sin estado
Se recomienda que siempre comience compilando servicios sin estado mediante el estado de almacenamiento [Reliable Services](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-introduction) en una base de datos de Azure, Azure CosmosDB o Azure Storage. Tener el estado externalizado es el enfoque más conocido para la mayoría de los desarrolladores y le permite aprovechar las ventajas de las capacidades de consulta en el almacén.  

### <a name="when-to-choose-stateful-services"></a>Cuándo elegir servicios con estado
Considere la posibilidad de elegir servicios con estado cuando tenga un escenario de baja latencia y necesite mantener los datos cerca del proceso. Algunos ejemplos incluyen dispositivos gemelos digitales de IoT, estado de juego, estado de sesión, almacenamiento en caché de datos de una base de datos y flujos de trabajo de larga duración para realizar el seguimiento de las llamadas a otros servicios.

Decida el período de tiempo de retención de los datos:

- Datos en caché: usa el almacenamiento en caché cuando la latencia a los almacenes externos es un problema. Use un servicio con estado como caché de datos propia o considere la posibilidad de usar [service-fabric-distributed-cache de SoCreate, de código abierto](https://github.com/SoCreate/service-fabric-distributed-cache). En este escenario, perder todos los datos en la caché no constituye un problema.
- Datos enlazados en tiempo: necesita mantener los datos cerca de proceso debido a la latencia durante un período de tiempo, pero puede permitirse perder dichos datos en un escenario de *desastre*. Por ejemplo, en muchas soluciones de IoT, los datos deben estar cerca del proceso, por ejemplo, calcular la temperatura media durante los últimos días; sin embargo, si estos datos se pierden, los puntos de datos concretos registrados no son tan importantes. Asimismo, en este escenario normalmente no se molesta en hacer copias de seguridad de puntos de datos individuales, solo de los valores medios calculados que se escriben periódicamente en el almacenamiento externo.  
- Datos a largo plazo: las colecciones de confianza pueden almacenar sus datos permanentemente. Sin embargo, en este caso deberá [prepararse para la recuperación ante desastres](https://docs.microsoft.com/azure/service-fabric/service-fabric-disaster-recovery), incluida la [configuración de directivas de copia de seguridad periódicas](https://docs.microsoft.com/azure/service-fabric/service-fabric-backuprestoreservice-configure-periodic-backup) para sus clusters. En efecto, configura qué pasará si su clúster se destruye en un escenario de desastre, en el que necesitaría crear un nuevo clúster, y cómo implementar nuevas instancias de aplicación y recuperar los datos de la copia de seguridad más reciente.

Ahorrar costos y mejorar la disponibilidad:
- Puede reducir los costos con servicios con estado, ya que no se incurre en costos de acceso y transacciones de datos desde el almacén remoto y no es necesario utilizar ningún otro servicio como Azure Redis.
- Usar servicios con estado principalmente para el almacenamiento y no para el proceso es costoso y no se recomienda. Considere la posibilidad de utilizar los servicios con estado como proceso con un almacenamiento local económico.
- Al quitar las dependencias en otros servicios, puede mejorar la disponibilidad del servicio. Tener un estado administrado con alta disponibilidad en el clúster le protege de los tiempos de inactividad o problemas de latencia de otros servicios.

## <a name="how-to-properly-work-with-reliable-services"></a>Cómo trabajar correctamente con Reliable Services
Reliable Services le permite crear fácilmente servicios con estado y sin estado. Lea la [introducción a Reliable Services](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-introduction).
- Respete siempre el [token de cancelación](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-lifecycle#stateful-service-primary-swaps) en el método `RunAsync()` para los servicios con estado y sin estado y el método `ChangeRole()` para los servicios con estado. Sin él, Service Fabric no sabe si se puede cerrar el servicio. Por ejemplo, no respetar el token de cancelación puede dar lugar a tiempos de actualización de aplicaciones mucho más prolongados.
-   Abra y cierre los [clientes de escucha de comunicación](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-communication) de manera oportuna y respete los tokens de cancelación.
-   Nunca mezcle código sincrónico con código asincrónico. Por ejemplo, no use `.GetAwaiter().GetResult()` en las llamadas asincrónicas; debe ser asincrónico *en todo momento* en la pila de llamadas.

## <a name="how-to-properly-work-with-reliable-actors"></a>Cómo trabajar correctamente con Reliable Actors
Reliable Actors le permite crear fácilmente actores con estado virtuales. Lea la [introducción a Reliable Actors](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-introduction).

- Considere seriamente la posibilidad de utilizar la mensajería publicada y enviada entre los actores para escalar la aplicación. Por ejemplo, la [mensajería publicada y enviada de código abierto de SoCreate](https://service-fabric-pub-sub.socreate.it/) o [Azure Service Bus](https://docs.microsoft.com/azure/service-bus/).
- Haga que el estado de los actores sea [lo más pormenorizado posible](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-state-management#best-practices).
- Administre el [ciclo de vida del actor](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-state-management#best-practices). Elimine actores si no piensa usarlos nunca más. Esto es especialmente relevante si usa el [proveedor VolatileState](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-state-management#state-persistence-and-replication), ya que todo el estado se almacena en memoria.
- Debido a su [simultaneidad basada en turnos](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-introduction#concurrency), los actores son más útiles como objetos independientes. No cree gráficos de llamadas al método multiactor sincrónico (cada una de las cuales probablemente se convierte en una llamada de red independiente) ni tenga solicitudes de actor circular, ya que afectarán significativamente al rendimiento y la escala.
- No mezcle código sincrónico con código asincrónico; debe ser asincrónico todo el tiempo para evitar problemas de rendimiento.
- No realice llamadas de larga duración en actores, ya que bloqueará otras llamadas para el mismo actor debido a la simultaneidad basada en turnos.
- Si se está comunicando con otros servicios mediante la [comunicación remota de Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-communication-remoting) y está creando un `ServiceProxyFactory`, cree la fábrica al nivel del [servicio de actores](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-using) y *no* al nivel del actor.


## <a name="application-diagnostics"></a>Diagnósticos de aplicaciones
- Sea exhaustivo al agregar el [registro de aplicaciones](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-generation-app) en las llamadas al servicio. Ayuda a diagnosticar escenarios en los que los servicios se llaman entre sí. Por ejemplo, en A -> B -> C -> D, la llamada podría generar un error en cualquier lugar; si no hay suficientes registros, es difícil de diagnosticar. Si los servicios están realizando demasiados registros debido al volumen de llamadas, al menos asegúrese de registrar los errores y las advertencias.

## <a name="iot-and-messaging-applications"></a>IoT y aplicaciones de mensajería
Al leer los mensajes de [Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub/) o [Azure Event Hubs](https://docs.microsoft.com/azure/event-hubs/), use [ServiceFabricProcessor](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/ServiceFabricProcessor), que se integra con Service Fabric Reliable Services para mantener el estado de lectura desde las particiones de Event Hubs e inserta los nuevos mensajes a los servicios mediante el método `IEventProcessor::ProcessEventsAsync()`.


## <a name="design-guidance-on-azure"></a>Guía de diseño sobre Azure
* Visite el [Centro de arquitectura de Azure](https://docs.microsoft.com/azure/architecture/microservices/) para consultar una guía de diseño sobre [cómo compilar microservicios en Azure](https://docs.microsoft.com/azure/architecture/microservices/).

* Visite [Introducción a Azure para Juegos](https://docs.microsoft.com/gaming/azure/) para consultar una guía de diseño sobre [el uso de Service Fabric en servicios de juegos](https://docs.microsoft.com/gaming/azure/reference-architectures/multiplayer-synchronous-sf).
