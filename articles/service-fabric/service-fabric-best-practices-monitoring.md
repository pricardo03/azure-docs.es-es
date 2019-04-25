---
title: Procedimientos recomendados para la supervisión de Azure Service Fabric | Microsoft Docs
description: Procedimientos recomendados para la supervisión de aplicaciones y clústeres de Service Fabric.
services: service-fabric
documentationcenter: .net
author: peterpogorski
manager: chackdan
editor: ''
ms.assetid: 19ca51e8-69b9-4952-b4b5-4bf04cded217
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/23/2019
ms.author: pepogors
ms.openlocfilehash: d90daaf18e5161053e00671b7667d05ec8e5db76
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60533816"
---
# <a name="monitoring-and-diagnostics"></a>Supervisión y diagnóstico

La [supervisión y el diagnóstico](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-overview) son fundamentales para el desarrollo, las pruebas y la implementación de cargas de trabajo en cualquier entorno de la nube. Por ejemplo, puede realizar un seguimiento de cómo se usan las aplicaciones, de las acciones que realiza la plataforma Service Fabric, del uso de recursos con los contadores de rendimiento y del estado general del clúster. Puede utilizar esta información para diagnosticar y corregir cualquier problema, así como para prevenir que se repita en el futuro.

## <a name="application-monitoring"></a>Supervisión de aplicaciones

La supervisión de aplicaciones realiza un seguimiento del uso de las características y componentes de una aplicación. Supervise sus aplicaciones para asegurarse de que detecta los problemas que afectan a sus usuarios. La supervisión de las aplicaciones es responsabilidad de quienes desarrollan la aplicación y sus servicios, ya que es única para la lógica de negocios de la aplicación. Se recomienda configurar la supervisión de aplicaciones con [Application Insights](https://docs.microsoft.com/azure/service-fabric/service-fabric-tutorial-monitoring-aspnet), la herramienta de supervisión de aplicaciones de Azure.

## <a name="cluster-monitoring"></a>Supervisión de clústeres

Uno de los objetivos de Service Fabric es que las aplicaciones sean resistentes a los errores de hardware. Esto se logra gracias a la capacidad que tienen los servicios del sistema de la plataforma de detectar problemas de infraestructura y conmutar por error las cargas de trabajo rápidamente a otros nodos del clúster. Pero, ¿qué ocurre si son los propios servicios del sistema quienes tienen problemas? ¿O, si al intentar implementar o mover una carga de trabajo, se infringen las reglas de ubicación de los servicios? Service Fabric proporciona diagnóstico para estos y otros problemas, lo que le pide tener la certeza de que está informado acerca de cómo interactúa la plataforma de Service Fabric con sus aplicaciones, servicios, contenedores y nodos.

Para los clústeres de Windows, se recomienda que configure la supervisión de clústeres con [agente de Diagnostics](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-aggregation-wad) y [registros de Azure Monitor](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-oms-setup).

Para los clústeres de Linux, los registros de Azure Monitor también es la herramienta recomendada para la supervisión de infraestructura y plataforma de Azure. Los diagnósticos de la plataforma Linux requieren otra configuración, como se indicó en [Eventos de clúster Linux de Service Fabric en Syslog](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-oms-syslog).

## <a name="infrastructure-monitoring"></a>Supervisión de infraestructura

[Registros de Azure Monitor](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-oms-agent) se recomiendan para supervisar eventos de nivel de clúster. Una vez que configure el agente de Log Analytics con su área de trabajo como se describe en el vínculo anterior, podrá recopilar métricas de rendimiento como el uso de la CPU, contadores de rendimiento de .NET como el uso de la CPU a nivel de proceso, contadores de rendimiento de Service Fabric contadores como el número de excepciones de un servicio confiable y métricas de contenedor como el uso de la CPU.  Deberá escribir los registros de contenedor en stdout o stderr para que estén disponibles en los registros de Azure Monitor.

## <a name="watchdogs"></a>Guardianes

Por lo general, un guardián es un servicio independiente que vigila el mantenimiento y la carga de los servicios, hace ping a los puntos de conexión e informa acerca de los eventos de mantenimiento inesperados del clúster. Esto puede ayudar a evitar errores que se podrían pasar pasan por alto si solo se tuviera en cuenta el rendimiento de un servicio. Los guardianes son también un buen lugar para hospedar código que realiza acciones de subsanación que no requieran interacción del usuario, como limpiar los archivos de registro de almacenamiento a determinados intervalos de tiempo. Vea la implementación de un servicio guardián de ejemplo en [Eventos de clúster Linux de Service Fabric en Syslog](https://github.com/Azure-Samples/service-fabric-watchdog-service).

## <a name="next-steps"></a>Pasos siguientes

* Empiece a instrumentar las aplicaciones: [Generación de eventos y registros de nivel de aplicación](service-fabric-diagnostics-event-generation-app.md).
* Consulte los pasos necesarios para configurar Application Insights para una aplicación en [Supervisión y diagnóstico de una aplicación de ASP.NET Core de Service Fabric](service-fabric-tutorial-monitoring-aspnet.md).
* Obtenga más información acerca de la supervisión de la plataforma y los eventos que proporciona Service Fabric: [Generación de eventos y registros de nivel de plataforma](service-fabric-diagnostics-event-generation-infra.md).
* Configurar la integración de registros de Azure Monitor con Service Fabric: [Configurar registros de Azure Monitor para un clúster](service-fabric-diagnostics-oms-setup.md)
* Aprenda a configurar registros de Azure Monitor para supervisar los contenedores: [Supervisión de contenedores Windows en Service Fabric con Log Analytics](service-fabric-tutorial-monitoring-wincontainers.md).
* Vea un ejemplo de problemas en el diagnóstico y sus soluciones con Service Fabric: [diagnóstico de escenarios comunes](service-fabric-diagnostics-common-scenarios.md)
* Más información acerca de las recomendaciones generales de supervisión para los recursos de Azure: [Procedimientos recomendados: supervisión y diagnóstico](https://docs.microsoft.com/azure/architecture/best-practices/monitoring).