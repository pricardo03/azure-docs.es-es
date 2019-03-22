---
title: Escenarios de diagnóstico comunes de Azure Service Fabric | Microsoft Docs
description: Vea cómo solucionar problemas de escenarios comunes con Azure Service Fabric
services: service-fabric
documentationcenter: .net
author: srrengar
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/25/2019
ms.author: srrengar
ms.openlocfilehash: 2eb395b4f3d922aa116e01c5de080a54d81e10ff
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "58118653"
---
# <a name="diagnose-common-scenarios-with-service-fabric"></a>Diagnóstico de escenarios comunes con Service Fabric

En este artículo se muestran escenarios comunes que los usuarios se encuentran en el área de supervisión y diagnóstico con Service Fabric. Los escenarios presentados cubren los 3 niveles de Service Fabric: aplicación, clúster e infraestructura. Cada solución usa Application Insights y los registros de Azure Monitor, herramientas de supervisión de Azure para completar cada escenario. Los pasos en cada solución ofrecen una introducción a los usuarios sobre cómo usar Application Insights y Azure Monitor registra en el contexto de Service Fabric.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="prerequisites-and-recommendations"></a>Requisitos previos y recomendaciones

Las soluciones en este artículo utilizan las siguientes herramientas. Se recomienda tenerlas instaladas y configuradas:

* [Application Insights con Service Fabric](service-fabric-tutorial-monitoring-aspnet.md)
* [Habilitar Azure Diagnostics en un clúster](service-fabric-diagnostics-event-aggregation-wad.md)
* [Configurar un área de trabajo de Log Analytics](service-fabric-diagnostics-oms-setup.md)
* [Agente de Log Analytics para realizar el seguimiento de los contadores de rendimiento](service-fabric-diagnostics-oms-agent.md)

## <a name="how-can-i-see-unhandled-exceptions-in-my-application"></a>¿Cómo puedo ver las excepciones no controladas en mi aplicación?

1. Vaya al recurso de Application Insights con el que la aplicación está configurada.
2. Haga clic en *Buscar* en la parte superior izquierda. Después, haga clic en Filtrar en el panel siguiente.

    ![Introducción a AI](media/service-fabric-diagnostics-common-scenarios/ai-search-filter.png)

3. Verá una gran cantidad de tipos de eventos (seguimientos, solicitudes, eventos personalizados). Elija "Excepción" como filtro.

    ![Lista de filtros de AI](media/service-fabric-diagnostics-common-scenarios/ai-filter-list.png)

    Al hacer clic en una excepción de la lista, puede consultar más detalles, incluido el contexto de servicio si usa el SDK de Application Insights para Service Fabric.

    ![Excepción de AI](media/service-fabric-diagnostics-common-scenarios/ai-exception.png)

## <a name="how-do-i-view-which-http-calls-are-used-in-my-services"></a>¿Cómo puedo ver qué llamadas HTTP se usan en mis servicios?

1. En el mismo recurso de Application Insights, puede filtrar por "solicitudes" en lugar de excepciones para ver todas las solicitudes realizadas.
2. Si usa el SDK de Application Insights para Service Fabric, verá una representación visual de los servicios conectados entre sí, y el número de solicitudes correctas e incorrectas. En la izquierda, haga clic en "Mapa de aplicación"

    ![Hoja de Mapa de aplicación de AI](media/service-fabric-diagnostics-common-scenarios/app-map-blade.png) ![AI App Map](media/service-fabric-diagnostics-common-scenarios/app-map-new.png)

    Para más información sobre el mapa de aplicación, consulte la [documentación de Mapa de aplicación](../azure-monitor/app/app-map.md)

## <a name="how-do-i-create-an-alert-when-a-node-goes-down"></a>Cómo puedo crear una alerta cuando un nodo deja de funcionar

1. El clúster de Service Fabric registra los eventos de nodo. Vaya al recurso de solución de Service Fabric Analytics llamado **ServiceFabric(nombreGrupoRecursos)**
2. Haga clic en el gráfico situado en la parte inferior de la hoja "Resumen"

    ![Solución de los registros de Azure Monitor](media/service-fabric-diagnostics-common-scenarios/oms-solution-azure-portal.png)

3. Aquí tiene muchos gráficos e iconos que muestran varias métricas. Haga clic en uno de los gráficos y le llevará a la búsqueda de registros. Aquí puede consultar los eventos de clúster o los contadores de rendimiento.
4. Escriba la siguiente consulta. Estos identificadores de evento se encuentran en la [referencia de eventos de nodo](service-fabric-diagnostics-event-generation-operational.md#application-events).

    ```kusto
    ServiceFabricOperationalEvent
    | where EventID >= 25622 and EventID <= 25626
    ```

5. Haga clic en "Nueva regla de alertas" en la parte superior y, ahora, cada vez que llegue un evento basado en esta consulta, recibirá una alerta por el método de comunicación elegido.

    ![Nueva alerta de los registros de Azure Monitor](media/service-fabric-diagnostics-common-scenarios/oms-create-alert.png)

## <a name="how-can-i-be-alerted-of-application-upgrade-rollbacks"></a>¿Cómo puedo recibir notificaciones de reversiones de actualizaciones de la aplicación?

1. En la misma ventana de búsqueda de registros, escriba la siguiente consulta para buscar reversiones de actualizaciones. Estos identificadores de evento se encuentran en la [referencia de eventos de aplicación](service-fabric-diagnostics-event-generation-operational.md#application-events).

    ```kusto
    ServiceFabricOperationalEvent
    | where EventID == 29623 or EventID == 29624
    ```

2. Haga clic en "Nueva regla de alertas" en la parte superior y, ahora, cada vez que llegue un evento basado en esta consulta, recibirá una alerta.

## <a name="how-do-i-see-container-metrics"></a>¿Cómo puedo ver las métricas de contenedor?

En la misma vista con todos los gráficos, verá algunos iconos del rendimiento de los contenedores. Necesita el agente de Log Analytics y la [solución de supervisión de contenedores](service-fabric-diagnostics-oms-containers.md) para que estos iconos se llenen.

![Métricas de contenedor de Log Analytics](media/service-fabric-diagnostics-common-scenarios/containermetrics.png)

>[!NOTE]
>Para instrumentar los datos de telemetría desde **dentro** del contenedor, debe agregar el [paquete NuGet de Application Insights para los contenedores](https://github.com/Microsoft/ApplicationInsights-servicefabric#microsoftapplicationinsightsservicefabric--for-service-fabric-lift-and-shift-scenarios).

## <a name="how-can-i-monitor-performance-counters"></a>¿Cómo puedo supervisar los contadores de rendimiento?

1. Una vez agregado el agente de Log Analytics al clúster, debe agregar los contadores de rendimiento concretos cuyo seguimiento quiere realizar. Navegue a la página del área de trabajo de Log Analytics en el portal; en la página de la solución, la pestaña del área de trabajo está en el menú izquierdo.

    ![Pestaña Área de trabajo de Log Analytics](media/service-fabric-diagnostics-common-scenarios/workspacetab.png)

2. Una vez que esté en la página del área de trabajo, haga clic en "Configuración avanzada" en el mismo menú izquierdo.

    ![Configuración avanzada de Log Analytics](media/service-fabric-diagnostics-common-scenarios/advancedsettingsoms.png)

3. Haga clic en Datos > Contadores de rendimiento de Windows (Datos > Contadores de rendimiento de Linux, en el caso de las máquinas Linux) para comenzar a recopilar los contadores específicos de los nodos mediante el agente de Log Analytics. Estos son ejemplos del formato de los contadores

   * `.NET CLR Memory(<ProcessNameHere>)\\# Total committed Bytes`
   * `Processor(_Total)\\% Processor Time`

     En la guía de inicio rápido, VotingData y VotingWeb son los nombres de proceso utilizados, por lo que el seguimiento de estos contadores tendrá el siguiente aspecto:

   * `.NET CLR Memory(VotingData)\\# Total committed Bytes`
   * `.NET CLR Memory(VotingWeb)\\# Total committed Bytes`

     ![Contadores de rendimiento de Log Analytics](media/service-fabric-diagnostics-common-scenarios/omsperfcounters.png)

4. Esto le permitirá ver cómo la infraestructura está tratando las cargas de trabajo, y establecer las alertas correspondientes en función de la utilización de los recursos. Por ejemplo, puede establecer una alerta si el uso total del procesador es superior al 90 % o inferior al 5 %. El nombre del contador que usaría para esto es "Tiempo de procesador %". Para ello, puede crear una regla de alerta para la consulta siguiente:

    ```kusto
    Perf | where CounterName == "% Processor Time" and InstanceName == "_Total" | where CounterValue >= 90 or CounterValue <= 5.
    ```

## <a name="how-do-i-track-performance-of-my-reliable-services-and-actors"></a>¿Cómo puedo supervisar el rendimiento de Reliable Services y Reliable Actors?

Para supervisar el rendimiento de servicios confiables o actores en sus aplicaciones, debe recopilar los contadores de Actor de Service Fabric, método de Actor, servicio y método de servicio. Estos son ejemplos de reliable actor y servicio de los contadores de rendimiento para recopilar

>[!NOTE]
>Contadores de rendimiento de Service Fabric no se pueden recopilar actualmente por el agente de Log Analytics, pero se pueden recopilar mediante [otras soluciones de diagnóstico](service-fabric-diagnostics-partners.md)

* `Service Fabric Service(*)\\Average milliseconds per request`
* `Service Fabric Service Method(*)\\Invocations/Sec`
* `Service Fabric Actor(*)\\Average milliseconds per request`
* `Service Fabric Actor Method(*)\\Invocations/Sec`

Consulte estos vínculos para obtener la lista completa de contadores de rendimiento de [Reliable Services](service-fabric-reliable-serviceremoting-diagnostics.md) y [Reliable Actors](service-fabric-reliable-actors-diagnostics.md).

## <a name="next-steps"></a>Pasos siguientes

* [Definición de alertas en Application Insights](../azure-monitor/app/alerts.md) para recibir notificaciones sobre los cambios de rendimiento o de uso
* [Detección inteligente en Application Insights](../azure-monitor/app/proactive-diagnostics.md), donde se realiza un análisis proactivo de la telemetría enviada a AI para avisar de problemas de rendimiento potenciales
* Más información sobre los registros de Azure Monitor [alertas](../log-analytics/log-analytics-alerts.md) que ayuden en la detección y diagnóstico.
* Para los clústeres locales, los registros de Azure Monitor ofrece una puerta de enlace (Proxy de reenvío HTTP) que puede utilizarse para enviar datos a los registros de Azure Monitor. Obtenga más información en [para conectar equipos sin acceso a Internet a los registros de Azure Monitor con la puerta de enlace de Log Analytics](../azure-monitor/platform/gateway.md)
* Familiarícese con la [búsqueda de registros y realizar consultas](../log-analytics/log-analytics-log-searches.md) características se ofrecen como parte de los registros de Azure Monitor
* ¿Obtenga una introducción más detallada de los registros de Azure Monitor y lo que ofrece, leer [What ' s registros de Azure Monitor?](../operations-management-suite/operations-management-suite-overview.md)
