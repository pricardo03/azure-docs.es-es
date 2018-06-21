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
ms.date: 05/16/2018
ms.author: srrengar
ms.openlocfilehash: bd7a7e0288ced0219a0600034b273d1acba6b09b
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2018
ms.locfileid: "34660244"
---
# <a name="diagnose-common-scenarios-with-service-fabric"></a>Diagnóstico de escenarios comunes con Service Fabric

En este artículo se muestran escenarios comunes que los usuarios se encuentran en el área de supervisión y diagnóstico con Service Fabric. Los escenarios presentados abarcan las 3 capas de Service Fabric: aplicación, clúster e infraestructura. Cada solución utiliza Application Insights y Log Analytics, las herramientas de supervisión de Azure, para completar cada escenario. Los pasos en cada solución proporcionan a los usuarios una introducción sobre cómo usar Application Insights y Log Analytics en el contexto de Service Fabric.

## <a name="prerequisites-and-recommendations"></a>Requisitos previos y recomendaciones

Las soluciones en este artículo utilizan las siguientes herramientas. Se recomienda tenerlas instaladas y configuradas:

* [Application Insights con Service Fabric](service-fabric-tutorial-monitoring-aspnet.md)
* [Habilitar Azure Diagnostics en un clúster](service-fabric-diagnostics-event-aggregation-wad.md)
* [Configuración de un área de trabajo de OMS Log Analytics](service-fabric-diagnostics-oms-setup.md)
* [Agente de OMS para realizar el seguimiento de los contadores de rendimiento](service-fabric-diagnostics-oms-agent.md)

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

    Para más información sobre el mapa de aplicación, consulte la [documentación de Mapa de aplicación](../application-insights/app-insights-app-map.md)

## <a name="how-do-i-create-an-alert-when-a-node-goes-down"></a>Cómo puedo crear una alerta cuando un nodo deja de funcionar

1. El clúster de Service Fabric registra los eventos de nodo. Vaya al recurso de solución de Service Fabric Analytics llamado **ServiceFabric(nombreGrupoRecursos)**
2. Haga clic en el gráfico situado en la parte inferior de la hoja "Resumen"

    ![Solución OMS](media/service-fabric-diagnostics-common-scenarios/oms-solution-azure-portal.png)

3. Aquí tiene muchos gráficos e iconos que muestran varias métricas. Haga clic en uno de los gráficos y le llevará a la búsqueda de registros. Aquí puede consultar los eventos de clúster o los contadores de rendimiento.
4. Escriba la siguiente consulta. Estos identificadores de evento se encuentran en la [referencia de eventos de nodo](service-fabric-diagnostics-event-generation-operational.md#application-events).

    ```kusto
    ServiceFabricOperationalEvent
    | where EventId >= 25623 or EventId <= 25626
    ```

5. Haga clic en "Nueva regla de alertas" en la parte superior y, ahora, cada vez que llegue un evento basado en esta consulta, recibirá una alerta por el método de comunicación elegido.

    ![OMS: Nueva alerta](media/service-fabric-diagnostics-common-scenarios/oms-create-alert.png)

## <a name="how-can-i-be-alerted-of-application-upgrade-rollbacks"></a>¿Cómo puedo recibir notificaciones de reversiones de actualizaciones de la aplicación?

1. En la misma ventana de búsqueda de registros, escriba la siguiente consulta para buscar reversiones de actualizaciones. Estos identificadores de evento se encuentran en la [referencia de eventos de aplicación](service-fabric-diagnostics-event-generation-operational.md#application-events).

    ```kusto
    ServiceFabricOperationalEvent
    | where EventId == 29623 or EventId == 29624
    ```

2. Haga clic en "Nueva regla de alertas" en la parte superior y, ahora, cada vez que llegue un evento basado en esta consulta, recibirá una alerta.

## <a name="how-do-i-see-container-metrics"></a>¿Cómo puedo ver las métricas de contenedor?

En la misma vista con todos los gráficos, verá algunos iconos del rendimiento de los contenedores. Necesita el agente de OMS y la [solución de supervisión de contenedores](service-fabric-diagnostics-oms-containers.md) para que estos iconos se llenen.

![OMS: métricas de contenedor](media/service-fabric-diagnostics-common-scenarios/containermetrics.png)

>[!NOTE]
>Para instrumentar los datos de telemetría desde **dentro** del contenedor, debe agregar el [paquete NuGet de Application Insights para los contenedores](https://github.com/Microsoft/ApplicationInsights-servicefabric#microsoftapplicationinsightsservicefabric--for-service-fabric-lift-and-shift-scenarios).

## <a name="how-can-i-monitor-performance-counters"></a>¿Cómo puedo supervisar los contadores de rendimiento?

1. Después de agregar el agente de OMS al clúster, debe agregar los contadores de rendimiento que desea supervisar. Vaya a la página del área de trabajo de OMS en el portal; en la página de la solución, la pestaña del área de trabajo está en el menú izquierdo.

    ![OMS: pestaña Área de trabajo](media/service-fabric-diagnostics-common-scenarios/workspacetab.png)

2. Una vez que esté en la página del área de trabajo, haga clic en "Configuración avanzada" en el mismo menú izquierdo.

    ![OMS: Configuración avanzada](media/service-fabric-diagnostics-common-scenarios/advancedsettingsoms.png)

3. Haga clic en Datos > Contadores de rendimiento de Windows (Datos > Contadores de rendimiento de Linux para las máquinas Linux) para comenzar a recopilar los contadores específicos de los nodos mediante el agente de OMS. Estos son ejemplos del formato de los contadores

    * `.NET CLR Memory(<ProcessNameHere>)\\# Total committed Bytes`
    * `Processor(_Total)\\% Processor Time`
    * `Service Fabric Service(*)\\Average milliseconds per request`

    En la guía de inicio rápido, VotingData y VotingWeb son los nombres de proceso utilizados, por lo que el seguimiento de estos contadores tendrá el siguiente aspecto:

    * `.NET CLR Memory(VotingData)\\# Total committed Bytes`
    * `.NET CLR Memory(VotingWeb)\\# Total committed Bytes`

    ![OMS: Contadores de rendimiento](media/service-fabric-diagnostics-common-scenarios/omsperfcounters.png)

4. Esto le permitirá ver cómo la infraestructura está tratando las cargas de trabajo, y establecer las alertas correspondientes en función de la utilización de los recursos. Por ejemplo, puede establecer una alerta si el uso total del procesador es superior al 90 % o inferior al 5 %. El nombre del contador que usaría para esto es "Tiempo de procesador %". Para ello, puede crear una regla de alerta para la consulta siguiente:

    ```kusto
    Perf | where CounterName == "% Processor Time" and InstanceName == "_Total" | where CounterValue >= 90 or CounterValue <= 5.
    ```

## <a name="how-do-i-track-performance-of-my-reliable-services-and-actors"></a>¿Cómo puedo supervisar el rendimiento de Reliable Services y Reliable Actors?

Para supervisar el rendimiento de los actores o servicios de confianza en sus aplicaciones, debe agregar los contadores Actor, Actor Method, Service y Service Method de Service Fabric. Puede agregar estos contadores de un modo similar al escenario anterior; estos son ejemplos de contadores de rendimiento de Reliable Services y Reliable Actors que se pueden agregar a OMS.

* `Service Fabric Service(*)\\Average milliseconds per request`
* `Service Fabric Service Method(*)\\Invocations/Sec`
* `Service Fabric Actor(*)\\Average milliseconds per request`
* `Service Fabric Actor Method(*)\\Invocations/Sec`

Consulte estos vínculos para obtener la lista completa de contadores de rendimiento de [Reliable Services](service-fabric-reliable-serviceremoting-diagnostics.md) y [Reliable Actors](service-fabric-reliable-actors-diagnostics.md).

## <a name="next-steps"></a>Pasos siguientes

* [Definición de alertas en Application Insights](../application-insights/app-insights-alerts.md) para recibir notificaciones sobre los cambios de rendimiento o de uso
* [Detección inteligente en Application Insights](../application-insights/app-insights-proactive-diagnostics.md), donde se realiza un análisis proactivo de la telemetría enviada a AI para avisar de problemas de rendimiento potenciales
* Obtenga más información sobre las [alertas](../log-analytics/log-analytics-alerts.md) de OMS Log Analytics como ayuda para la detección y diagnóstico.
* Si se trata de clústeres locales, OMS ofrece una puerta de enlace (proxy de reenvío HTTP) que puede usarse para enviar datos a OMS. Obtenga más información en [Conexión de equipos sin acceso a Internet a OMS mediante OMS Gateway](../log-analytics/log-analytics-oms-gateway.md).
* Familiarícese con las funciones de [búsqueda de registros y consulta](../log-analytics/log-analytics-log-searches.md) que se ofrecen como parte de Log Analytics
* Para obtener información general más detallada sobre Log Analytics y lo que ofrece, vea [¿Qué es Azure Log Analytics?](../operations-management-suite/operations-management-suite-overview.md)
