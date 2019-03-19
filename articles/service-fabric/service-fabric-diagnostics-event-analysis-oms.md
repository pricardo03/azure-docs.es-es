---
title: Los registros de análisis de eventos de Azure Service Fabric con Azure Monitor | Microsoft Docs
description: Obtenga información sobre cómo visualizar y analizar eventos con los registros de Azure Monitor para la supervisión y diagnóstico de clústeres de Azure Service Fabric.
services: service-fabric
documentationcenter: .net
author: srrengar
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/21/2019
ms.author: srrengar
ms.openlocfilehash: 2f3106b33ab0cbea95efe2ac42c05a8543719190
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2019
ms.locfileid: "57246923"
---
# <a name="event-analysis-and-visualization-with-azure-monitor-logs"></a>Análisis de eventos y la visualización con registros de Azure Monitor
 Registros de Azure Monitor recopila y analiza datos de telemetría de aplicaciones y servicios hospedados en la nube y proporciona las herramientas de análisis para ayudar a maximizar su disponibilidad y rendimiento. En este artículo se describe cómo ejecutar consultas en los registros de Azure Monitor para obtener información y solucionar problemas de lo que sucede en el clúster. Se tratan las siguientes preguntas habituales:

* ¿Cómo se solucionan los eventos de mantenimiento?
* ¿Cómo se puede saber si un nodo deja de funcionar?
* ¿Cómo se puede saber si los servicios de la aplicación se han iniciado o detenido?

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="overview-of-the-log-analytics-workspace"></a>Información general del área de trabajo de Log Analytics

>[!NOTE] 
>Mientras que el almacenamiento de diagnóstico está habilitado de forma predeterminada en el momento de creación del clúster, debe configurar el área de trabajo de Log Analytics para que lea desde el almacenamiento de diagnósticos.

Azure Monitor registra recopila datos de los recursos administrados, incluida una tabla de almacenamiento de Azure o un agente y mantiene en un repositorio central. Estos datos pueden utilizarse posteriormente para análisis, alertas, visualizaciones y tareas ulteriores de exportación. Los registros de Azure Monitor admite eventos, datos de rendimiento o cualquier otro dato personalizado. Desproteger [pasos para configurar la extensión de diagnósticos para agregar eventos](service-fabric-diagnostics-event-aggregation-wad.md) y [pasos para crear un área de trabajo de Log Analytics para leer de los eventos de almacenamiento](service-fabric-diagnostics-oms-setup.md) para asegurarse de que los datos están llegando a Azure Monitor registros.

Después de que recibe los datos en los registros de Azure Monitor, Azure tiene varios *soluciones de supervisión* que son soluciones preempaquetadas o paneles operativos para supervisar los datos entrantes, personalizados para varios escenarios. Puede tratarse de una solución de *Service Fabric Analytics* y una solución de *Containers*, que son las dos opciones más importantes para diagnosticar y supervisar el uso de los clústeres de Service Fabric. En este artículo se describe cómo utilizar la solución de Service Fabric Analytics, que se crea con el área de trabajo.

## <a name="access-the-service-fabric-analytics-solution"></a>Acceso a la solución de Service Fabric Analytics

En el [Portal de Azure](https://portal.azure.com), vaya al grupo de recursos en el que creó la solución Service Fabric Analytics.

Seleccione el recurso **ServiceFabric\<nameOfOMSWorkspace\>**.

En `Summary`, verá iconos en forma de grafo para cada una de las soluciones habilitadas, entre ellos uno para Service Fabric. Haga clic en el grafo de **Service Fabric** para ir a la solución Service Fabric Analytics.

![Solución Service Fabric](media/service-fabric-diagnostics-event-analysis-oms/oms_service_fabric_summary.PNG)

La siguiente imagen muestra la página principal de la solución Service Fabric Analytics. Esta página principal proporciona una vista de instantánea de lo que sucede en el clúster.

![Solución Service Fabric](media/service-fabric-diagnostics-event-analysis-oms/oms_service_fabric_solution.PNG)

 Si habilitó el diagnóstico durante la creación del clúster, puede ver eventos de 

* [Eventos de clúster de Service Fabric](service-fabric-diagnostics-event-generation-operational.md)
* [Eventos del modelo de programación de Reliable Actors](service-fabric-reliable-actors-diagnostics.md)
* [Eventos del modelo de programación de Reliable Services](service-fabric-reliable-services-diagnostics.md)

>[!NOTE]
>Además de los eventos de Service Fabric estándar, se pueden recopilar eventos del sistema más detallados mediante la [actualización de la configuración de la extensión de diagnósticos](service-fabric-diagnostics-event-aggregation-wad.md#log-collection-configurations).

## <a name="view-service-fabric-events-including-actions-on-nodes"></a>Ver eventos de tejido de servicio, incluidas las acciones en nodos

En la página de Service Fabric Analytics, haga clic en el grafo de **eventos de Service Fabric**.

![Canal operativo de la solución Service Fabric](media/service-fabric-diagnostics-event-analysis-oms/oms_service_fabric_events_selection.png)

Haga clic en **Lista** para ver los eventos en una lista. Una vez aquí, verá todos los eventos del sistema que se han recopilado. Como referencia, proceden de la **WADServiceFabricSystemEventsTable** en el almacenamiento de Azure cuenta y lo mismo para los eventos de servicios y los actores confiables que ve a continuación provienen de esas tablas respectivas.
    
![Canal operativo de consulta](media/service-fabric-diagnostics-event-analysis-oms/oms_service_fabric_events.png)

También puede hacer clic en la lupa de la izquierda y usar el lenguaje de consulta Kusto para encontrar lo está buscando. Por ejemplo, para buscar todas las acciones realizadas en los nodos del clúster, puede usar la consulta siguiente. Los identificadores de evento que se usan a continuación se encuentran en la [referencia de eventos del canal operativo](service-fabric-diagnostics-event-generation-operational.md).

```kusto
ServiceFabricOperationalEvent
| where EventId < 25627 and EventId > 25619 
```

Puede consultar en muchos más campos, como los nodos específicos (Computer), el servicio del sistema (TaskName).

## <a name="view-service-fabric-reliable-service-and-actor-events"></a>Visualización de eventos de Reliable Services y Reliable Actors

En la página de Service Fabric Analytics, haga clic en el grafo de **Reliable Services**.

![Reliable Services de la solución Service Fabric](media/service-fabric-diagnostics-event-analysis-oms/oms_reliable_services_events_selection.png)

Haga clic en **Lista** para ver los eventos en una lista. Aquí puede ver eventos de Reliable Services. Puede ver eventos diferentes para cuando el servicio runasync se inicia y se completa, lo que ocurre habitualmente en las implementaciones y las actualizaciones. 

![Reliable Services de consulta](media/service-fabric-diagnostics-event-analysis-oms/oms_reliable_service_events.png)

Los eventos de Reliable Actors pueden verse de forma similar. Para configurar más eventos detallados para Reliable Actors, necesita cambiar `scheduledTransferKeywordFilter` en la configuración de la extensión de diagnóstico (se muestra a continuación). Los detalles de los valores de estos están en la [referencia de eventos de Reliable Actors](service-fabric-reliable-actors-diagnostics.md#keywords).

```json
"EtwEventSourceProviderConfiguration": [
                {
                    "provider": "Microsoft-ServiceFabric-Actors",
                    "scheduledTransferKeywordFilter": "1",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricReliableActorEventTable"
                    }
                },
```

El lenguaje de consulta Kusto es eficaz. Otra consulta valiosa que puede ejecutar consiste en averiguar qué nodos están generando la mayoría de los eventos. La consulta de la captura de pantalla siguiente muestra eventos operativos de Service Fabric con el servicio y el nodo específicos.

![Eventos de consulta por nodo](media/service-fabric-diagnostics-event-analysis-oms/oms_kusto_query.png)

## <a name="next-steps"></a>Pasos siguientes

* Para habilitar la supervisión de la infraestructura, es decir, los contadores de rendimiento, vea cómo [agregar el agente de Log Analytics](service-fabric-diagnostics-oms-agent.md). El agente recopila los contadores de rendimiento y los agrega al área de trabajo existente.
* Para los clústeres locales, los registros de Azure Monitor ofrece una puerta de enlace (Proxy de reenvío HTTP) que puede utilizarse para enviar datos a los registros de Azure Monitor. Obtenga más información en [para conectar equipos sin acceso a Internet a los registros de Azure Monitor con la puerta de enlace de Log Analytics](../azure-monitor/platform/gateway.md).
* Configure [alertas automáticas](../log-analytics/log-analytics-alerts.md) para facilitar la detección y el diagnóstico.
* Familiarícese con la [búsqueda de registros y realizar consultas](../log-analytics/log-analytics-log-searches.md) características se ofrecen como parte de los registros de Azure Monitor.
* Obtenga una introducción más detallada de los registros de Azure Monitor y lo que ofrece, leer [What ' s registros de Azure Monitor?](../operations-management-suite/operations-management-suite-overview.md).
