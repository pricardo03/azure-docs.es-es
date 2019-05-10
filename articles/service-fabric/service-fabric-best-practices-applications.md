---
title: Azure Service Fabric application recomendaciones de diseño | Microsoft Docs
description: Prácticas recomendadas para desarrollar aplicaciones de Service Fabric.
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
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/07/2019
ms.locfileid: "65237753"
---
# <a name="azure-service-fabric-application-design-best-practices"></a>Azure Service Fabric application recomendaciones de diseño

Este artículo proporciona instrucciones de prácticas recomendadas para la creación de aplicaciones y servicios en Service Fabric.
 
## <a name="get-familiar-with-service-fabric"></a>Familiarícese con Service Fabric
* [¿Por lo que desea obtener información acerca de Service Fabric?](service-fabric-content-roadmap.md)
* Obtenga información sobre [escenarios de aplicación de Service Fabric](service-fabric-application-scenarios.md)
* A continuación, comprender las opciones de modelo de programación con [información general del modelo de programación de Service Fabric](service-fabric-choose-framework.md)



## <a name="application-design-guidance"></a>Guía de diseño de aplicación
Familiarizarse con la [arquitectura general](https://docs.microsoft.com/azure/architecture/reference-architectures/microservices/service-fabric) de una aplicación de Service Fabric y su [consideraciones de diseño](https://docs.microsoft.com/azure/architecture/reference-architectures/microservices/service-fabric#design-considerations).

### <a name="choose-an-api-gateway"></a>Elegir una puerta de enlace de API
Usar un servicio de puerta de enlace de API que se comunica con servicios de back-end que, a continuación, se pueden escalar horizontalmente. Los servicios de puerta de enlace de API más comunes utilizados son:

- [Azure API Management](https://docs.microsoft.com/azure/service-fabric/service-fabric-api-management-overview), que es [integrado con Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-tutorial-deploy-api-management)
- [Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub/) o [Azure Event Hubs](https://docs.microsoft.com/azure/event-hubs/) utilizando el [ServiceFabricProcessor](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/ServiceFabricProcessor) para leer de particiones del centro de eventos
- [Proxy inverso de Træfik](https://blogs.msdn.microsoft.com/azureservicefabric/2018/04/05/intelligent-routing-on-service-fabric-with-traefik/) utilizando el [proveedor de Azure Service Fabric](https://docs.traefik.io/configuration/backends/servicefabric/)
- [Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/) Nota: no se integra directamente con Service Fabric y Azure API Management es normalmente la opción preferida
- Cree las suyas propias [ASP.NET Core](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-communication-aspnetcore) puerta de enlace de aplicaciones web

### <a name="choose-stateless-services"></a>Elija servicios sin estado
Se recomienda que siempre comiencen a los servicios sin estado de creación mediante [Reliable Services](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-introduction) almacenar el estado en una base de datos de Azure, Azure CosmosDB o almacenamiento de Azure. Tener estado externalizado es el enfoque más familiar para la mayoría de los desarrolladores y permite aprovechar las ventajas de las capacidades de consulta en el almacén.  

### <a name="when-to-choose-stateful-services"></a>Cuándo optar por los servicios con estado
Considere la posibilidad de servicios con estado cuando se tiene un escenario de baja latencia y necesita mantener los datos cerca del proceso. Algunos ejemplos incluyen dispositivos digitales gemelo de IoT, estado del juego, estado de sesión, la caché de datos de una base de datos y los flujos de trabajo para realizar el seguimiento de las llamadas a otros servicios de larga ejecución.

Decida el período de tiempo de retención de datos:

- Datos almacenados en caché - utiliza almacenamiento en caché que la latencia en almacenes externos es un problema. Usar un servicio con estado como sus propios datos de caché o consideración el uso de la [SoCreate service-fabric--caché distribuido con código abierto](https://github.com/SoCreate/service-fabric-distributed-cache). En este escenario, puede perder todos los datos en la memoria caché y no importa.
- Datos enlazados en tiempo - necesita mantener los datos cerca de proceso para la latencia durante un período de tiempo, pero pueden permitirse que los datos se perderá en un *ante desastres* escenario. Por ejemplo, en muchas soluciones de IoT datos tienen que ser Cerrar para compute, por ejemplo calcular la temperatura media durante los últimos días, sin embargo, si estos datos se pierde, los datos específicos apunta registran no es importante. También en este escenario normalmente se preocupa copia de seguridad de los puntos de datos individuales, sólo de los valores calculados de promedio que se escriben en almacenamiento externo periódicamente.  
- Los datos a largo plazo: Reliable collections puede almacenar los datos de forma permanente. Sin embargo, en este caso deberá [prepararse para la recuperación ante desastres](https://docs.microsoft.com/azure/service-fabric/service-fabric-disaster-recovery) incluidos [configuración de directivas de copia de seguridad periódicas](https://docs.microsoft.com/azure/service-fabric/service-fabric-backuprestoreservice-configure-periodic-backup) para los clústeres. Configurar en efecto, ¿qué ocurre si se destruye el clúster en un escenario de desastre, donde había que crear un nuevo clúster y cómo implementar nuevas instancias de aplicación y recuperar la copia de seguridad más reciente.

Ahorrar costos y mejorar la disponibilidad:
- Puede reducir los costos con los servicios con estado, ya que no se incurre en costos de acceso y transacciones de datos desde el almacenamiento remoto y no hace falta utilizar otro servicio, como Azure Redis.
- Usar servicios con estado principalmente para el almacenamiento y no para el proceso es costoso y no se recomienda. Considere la posibilidad de servicios con estado como del proceso con almacenamiento local económico.
- Quitando las dependencias en otros servicios, puede mejorar la disponibilidad del servicio. Tener estado administrado con alta disponibilidad en el clúster, aísla de otros tiempos de inactividad del servicio o problemas de latencia.

## <a name="how-to-properly-work-with-reliable-services"></a>Cómo funciona correctamente con Reliable Services
Servicios fiables permiten crear fácilmente servicios con y sin estado. Leer la [Introducción a Reliable Services](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-introduction)
- Siempre respetan la [token de cancelación](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-lifecycle#stateful-service-primary-swaps) en el `RunAsync()` método para los servicios con y sin estado y el `ChangeRole()` método servicios con estado. Sin esto, Service Fabric no sabe si se puede cerrar el servicio. Por ejemplo, no respeta el token de cancelación puede dar lugar a tiempos de actualización de aplicación mucho más.
-   Apertura y cierre [los agentes de escucha de comunicación](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-communication) de manera oportuna y respetar los tokens de cancelación.
-   Nunca mezcle código de sincronización con código asincrónico. Por ejemplo, no use `.GetAwaiter().GetResult()` en las llamadas asincrónicas; debe ser async *totalmente* a través de la pila de llamadas.

## <a name="how-to-properly-work-with-reliable-actors"></a>Cómo trabajar correctamente con Reliable Actors
Reliable Actors permite crear fácilmente los actores con estado, virtuales. Leer la [Introducción a Reliable Actors](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-introduction)

- Considerar seriamente utilizar pub/sub de mensajería entre los actores para escalar la aplicación. Por ejemplo, el [abierto pub/sub de SoCreate](https://service-fabric-pub-sub.socreate.it/) o [Azure Service Bus](https://docs.microsoft.com/azure/service-bus/).
- Hacer que el estado del actor como [más pormenorizado posible](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-state-management#best-practices).
- Administrar la [ciclo de vida del actor](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-state-management#best-practices). Eliminar actores si no piensa usarlas nunca más. Esto es especialmente cierto cuando se usa el [VolatileState proveedor](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-state-management#state-persistence-and-replication) como el estado se almacena en memoria.
- Debido a sus [simultaneidad en función de a su vez](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-introduction#concurrency) actores son útiles como objetos independientes. No crear gráficos de llamadas al método de actor múltiples y sincrónico (cada uno de los cuales probablemente se convierte en una llamada de red independiente) o con solicitudes de actor circular; Estos modificaremos apreciablemente de escala y rendimiento.
- No mezcle el código de sincronización con código asincrónico; es necesario que sean completamente asincrónicos, para evitar problemas de rendimiento.
- No realice llamadas de larga ejecución en actors, bloqueará otras llamadas para el mismo actor, debido a la simultaneidad basada en turnos.
- Si comunicarse con otros servicios mediante [comunicación remota de Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-communication-remoting) y está creando un `ServiceProxyFactory`, a continuación, cree el generador en el [servicio de actor](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-using) nivel y *no*en el nivel de actor.


## <a name="application-diagnostics"></a>Diagnósticos de aplicaciones
- Sea exhaustivo en agregar [registro de la aplicación](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-generation-app) en las llamadas al servicio. Ayuda de diagnóstico de los escenarios donde los servicios llaman entre sí. Por ejemplo, cuando A -> B -> C -> D la llamada podría generar un error en cualquier lugar; Si no hay suficientes registro, es difícil de diagnosticar. Si los servicios están iniciando sesión demasiado debido a los volúmenes de la llamada, al menos debe registrar errores y advertencias.

## <a name="iot-and-messaging-applications"></a>IoT y aplicaciones de mensajería
Al leer los mensajes de [Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub/) o [Azure Event Hubs](https://docs.microsoft.com/azure/event-hubs/) usar [ServiceFabricProcessor](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/ServiceFabricProcessor) que se integra con Service Fabric Reliable Services para mantener la estado de lectura desde el centro de eventos crea particiones e inserta nuevos mensajes a los servicios a través de la `IEventProcessor::ProcessEventsAsync()` método.


## <a name="design-guidance-on-azure"></a>Guía de diseño en Azure
* Visite el [centro de arquitectura de Azure](https://docs.microsoft.com/azure/architecture/microservices/) para obtener instrucciones sobre diseño [crear microservicios en Azure](https://docs.microsoft.com/azure/architecture/microservices/)

* Visite [comience a usar Azure para juegos](https://docs.microsoft.com/gaming/azure/) para obtener instrucciones sobre diseño [mediante Service Fabric en servicios de juegos](https://docs.microsoft.com/gaming/azure/reference-architectures/multiplayer-synchronous-sf)
