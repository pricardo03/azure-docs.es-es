---
title: Crear alertas de rendimiento mediante Azure Monitor para contenedores | Microsoft Docs
description: En este artículo se describe cómo usar a Azure Monitor para los contenedores para crear alertas personalizadas basadas en consultas de registro de uso de memoria y CPU.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/17/2019
ms.author: magoedte
ms.openlocfilehash: bbd7c733c7c089328d2fbe016426fe9de3a6b5ce
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/22/2019
ms.locfileid: "59998658"
---
# <a name="how-to-set-up-alerts-for-performance-problems-in-azure-monitor-for-containers"></a>Cómo configurar alertas para los problemas de rendimiento en Azure Monitor para contenedores
Supervisión de contenedores de Azure supervisa el rendimiento de las cargas de trabajo de contenedor que se implementan en Azure Container Instances o administrar clústeres de Kubernetes que se hospedan en Azure Kubernetes Service (AKS).

Este artículo describe cómo habilitar las alertas de las situaciones siguientes:

* Cuando el uso de CPU o memoria en nodos del clúster supera un umbral definido
* Cuando la utilización de CPU o memoria en cualquier contenedor dentro de un controlador de supera un umbral definido en comparación con un límite que se establece en el recurso correspondiente
* *NotReady* nodo estado de cuenta
*  *No se pudo*, *pendiente*, *desconocido*, *ejecutando*, o *Succeeded* recuentos de fase de pod

Para generar una alerta para el uso de CPU elevado o uso de memoria en los nodos del clúster, use las consultas que se proporcionan para crear una alerta de métrica o una alerta de unidades métricas. Alertas de métricas tienen una menor latencia que las alertas del registro. Pero las alertas de registro proporcionan mayor sofisticación y consultas avanzadas. Consultas comparan un valor datetime en el presente mediante el uso de alertas de registro el *ahora* operador y va realizar una copia de una hora. (Supervisión de contenedores de azure almacena todas las fechas en formato de hora Universal coordinada (UTC)).

Si no está familiarizado con Azure Monitor genera una alerta, consulte [información general sobre las alertas en Microsoft Azure](../platform/alerts-overview.md) antes de empezar. Para obtener más información acerca de las alertas que usan consultas de registros, vea [alertas de registro en Azure Monitor](../platform/alerts-unified-log.md). Para obtener más información acerca de las alertas de métricas, consulte [alertas de métricas de Azure Monitor](../platform/alerts-metric-overview.md).

## <a name="resource-utilization-log-search-queries"></a>Consultas de búsqueda de registro de utilización de recursos
Las consultas en esta sección admiten cada escenario de alertas. Se usan en el paso 7 de la [crear alerta](#create-an-alert-rule) sección de este artículo.

La siguiente consulta calcula el uso medio de CPU como promedio de uso de CPU de los nodos de miembro cada minuto.  

```kusto
let endDateTime = now();
let startDateTime = ago(1h);
let trendBinSize = 1m;
let capacityCounterName = 'cpuCapacityNanoCores';
let usageCounterName = 'cpuUsageNanoCores';
KubeNodeInventory
| where TimeGenerated < endDateTime
| where TimeGenerated >= startDateTime
// cluster filter would go here if multiple clusters are reporting to the same Log Analytics workspace
| distinct ClusterName, Computer
| join hint.strategy=shuffle (
  Perf
  | where TimeGenerated < endDateTime
  | where TimeGenerated >= startDateTime
  | where ObjectName == 'K8SNode'
  | where CounterName == capacityCounterName
  | summarize LimitValue = max(CounterValue) by Computer, CounterName, bin(TimeGenerated, trendBinSize)
  | project Computer, CapacityStartTime = TimeGenerated, CapacityEndTime = TimeGenerated + trendBinSize, LimitValue
) on Computer
| join kind=inner hint.strategy=shuffle (
  Perf
  | where TimeGenerated < endDateTime + trendBinSize
  | where TimeGenerated >= startDateTime - trendBinSize
  | where ObjectName == 'K8SNode'
  | where CounterName == usageCounterName
  | project Computer, UsageValue = CounterValue, TimeGenerated
) on Computer
| where TimeGenerated >= CapacityStartTime and TimeGenerated < CapacityEndTime
| project ClusterName, Computer, TimeGenerated, UsagePercent = UsageValue * 100.0 / LimitValue
| summarize AggregatedValue = avg(UsagePercent) by bin(TimeGenerated, trendBinSize), ClusterName
```

La siguiente consulta calcula el uso de memoria promedio como un promedio de uso de memoria de los nodos de miembro cada minuto.

```kusto
let endDateTime = now();
let startDateTime = ago(1h);
let trendBinSize = 1m;
let capacityCounterName = 'memoryCapacityBytes';
let usageCounterName = 'memoryRssBytes';
KubeNodeInventory
| where TimeGenerated < endDateTime
| where TimeGenerated >= startDateTime
// cluster filter would go here if multiple clusters are reporting to the same Log Analytics workspace
| distinct ClusterName, Computer
| join hint.strategy=shuffle (
  Perf
  | where TimeGenerated < endDateTime
  | where TimeGenerated >= startDateTime
  | where ObjectName == 'K8SNode'
  | where CounterName == capacityCounterName
  | summarize LimitValue = max(CounterValue) by Computer, CounterName, bin(TimeGenerated, trendBinSize)
  | project Computer, CapacityStartTime = TimeGenerated, CapacityEndTime = TimeGenerated + trendBinSize, LimitValue
) on Computer
| join kind=inner hint.strategy=shuffle (
  Perf
  | where TimeGenerated < endDateTime + trendBinSize
  | where TimeGenerated >= startDateTime - trendBinSize
  | where ObjectName == 'K8SNode'
  | where CounterName == usageCounterName
  | project Computer, UsageValue = CounterValue, TimeGenerated
) on Computer
| where TimeGenerated >= CapacityStartTime and TimeGenerated < CapacityEndTime
| project ClusterName, Computer, TimeGenerated, UsagePercent = UsageValue * 100.0 / LimitValue
| summarize AggregatedValue = avg(UsagePercent) by bin(TimeGenerated, trendBinSize), ClusterName
```
>[!IMPORTANT]
>Las consultas siguientes utilizan los valores de marcador de posición \<el nombre del clúster > y \<el nombre del controlador > para representar el clúster y el controlador. Reemplácelos por valores específicos del entorno al configurar las alertas.

La siguiente consulta calcula el uso de CPU promedio de todos los contenedores en un controlador como un promedio de uso de CPU de cada instancia del contenedor en un controlador de cada minuto. La medida es un porcentaje del límite configurado para un contenedor.

```kusto
let endDateTime = now();
let startDateTime = ago(1h);
let trendBinSize = 1m;
let capacityCounterName = 'cpuLimitNanoCores';
let usageCounterName = 'cpuUsageNanoCores';
let clusterName = '<your-cluster-name>';
let controllerName = '<your-controller-name>';
KubePodInventory
| where TimeGenerated < endDateTime
| where TimeGenerated >= startDateTime
| where ClusterName == clusterName
| where ControllerName == controllerName
| extend InstanceName = strcat(ClusterId, '/', ContainerName),
         ContainerName = strcat(controllerName, '/', tostring(split(ContainerName, '/')[1]))
| distinct Computer, InstanceName, ContainerName
| join hint.strategy=shuffle (
    Perf
    | where TimeGenerated < endDateTime
    | where TimeGenerated >= startDateTime
    | where ObjectName == 'K8SContainer'
    | where CounterName == capacityCounterName
    | summarize LimitValue = max(CounterValue) by Computer, InstanceName, bin(TimeGenerated, trendBinSize)
    | project Computer, InstanceName, LimitStartTime = TimeGenerated, LimitEndTime = TimeGenerated + trendBinSize, LimitValue
) on Computer, InstanceName
| join kind=inner hint.strategy=shuffle (
    Perf
    | where TimeGenerated < endDateTime + trendBinSize
    | where TimeGenerated >= startDateTime - trendBinSize
    | where ObjectName == 'K8SContainer'
    | where CounterName == usageCounterName
    | project Computer, InstanceName, UsageValue = CounterValue, TimeGenerated
) on Computer, InstanceName
| where TimeGenerated >= LimitStartTime and TimeGenerated < LimitEndTime
| project Computer, ContainerName, TimeGenerated, UsagePercent = UsageValue * 100.0 / LimitValue
| summarize AggregatedValue = avg(UsagePercent) by bin(TimeGenerated, trendBinSize) , ContainerName
```

La siguiente consulta calcula el uso de memoria promedio de todos los contenedores en un controlador como un promedio de uso de memoria de cada instancia de contenedor en un controlador de cada minuto. La medida es un porcentaje del límite configurado para un contenedor.

```kusto
let endDateTime = now();
let startDateTime = ago(1h);
let trendBinSize = 1m;
let capacityCounterName = 'memoryLimitBytes';
let usageCounterName = 'memoryRssBytes';
let clusterName = '<your-cluster-name>';
let controllerName = '<your-controller-name>';
KubePodInventory
| where TimeGenerated < endDateTime
| where TimeGenerated >= startDateTime
| where ClusterName == clusterName
| where ControllerName == controllerName
| extend InstanceName = strcat(ClusterId, '/', ContainerName),
         ContainerName = strcat(controllerName, '/', tostring(split(ContainerName, '/')[1]))
| distinct Computer, InstanceName, ContainerName
| join hint.strategy=shuffle (
    Perf
    | where TimeGenerated < endDateTime
    | where TimeGenerated >= startDateTime
    | where ObjectName == 'K8SContainer'
    | where CounterName == capacityCounterName
    | summarize LimitValue = max(CounterValue) by Computer, InstanceName, bin(TimeGenerated, trendBinSize)
    | project Computer, InstanceName, LimitStartTime = TimeGenerated, LimitEndTime = TimeGenerated + trendBinSize, LimitValue
) on Computer, InstanceName
| join kind=inner hint.strategy=shuffle (
    Perf
    | where TimeGenerated < endDateTime + trendBinSize
    | where TimeGenerated >= startDateTime - trendBinSize
    | where ObjectName == 'K8SContainer'
    | where CounterName == usageCounterName
    | project Computer, InstanceName, UsageValue = CounterValue, TimeGenerated
) on Computer, InstanceName
| where TimeGenerated >= LimitStartTime and TimeGenerated < LimitEndTime
| project Computer, ContainerName, TimeGenerated, UsagePercent = UsageValue * 100.0 / LimitValue
| summarize AggregatedValue = avg(UsagePercent) by bin(TimeGenerated, trendBinSize) , ContainerName
```

La siguiente consulta devuelve todos los nodos y recuentos que tengan un estado de *listo* y *NotReady*.

```kusto
let endDateTime = now();
let startDateTime = ago(1h);
let trendBinSize = 1m;
let clusterName = '<your-cluster-name>';
KubeNodeInventory
| where TimeGenerated < endDateTime
| where TimeGenerated >= startDateTime
| distinct ClusterName, Computer, TimeGenerated
| summarize ClusterSnapshotCount = count() by bin(TimeGenerated, trendBinSize), ClusterName, Computer
| join hint.strategy=broadcast kind=inner (
    KubeNodeInventory
    | where TimeGenerated < endDateTime
    | where TimeGenerated >= startDateTime
    | summarize TotalCount = count(), ReadyCount = sumif(1, Status contains ('Ready'))
                by ClusterName, Computer,  bin(TimeGenerated, trendBinSize)
    | extend NotReadyCount = TotalCount - ReadyCount
) on ClusterName, Computer, TimeGenerated
| project   TimeGenerated,
            ClusterName,
            Computer,
            ReadyCount = todouble(ReadyCount) / ClusterSnapshotCount,
            NotReadyCount = todouble(NotReadyCount) / ClusterSnapshotCount
| order by ClusterName asc, Computer asc, TimeGenerated desc
```
La siguiente consulta devuelve recuentos de fase de pod se basa en todas las fases: *No se pudo*, *pendiente*, *desconocido*, *ejecutando*, o *se realizó correctamente*.  

```kusto
let endDateTime = now();
    let startDateTime = ago(1h);
    let trendBinSize = 1m;
    let clusterName = '<your-cluster-name>';
    KubePodInventory
    | where TimeGenerated < endDateTime
    | where TimeGenerated >= startDateTime
    | where ClusterName == clusterName
    | distinct ClusterName, TimeGenerated
    | summarize ClusterSnapshotCount = count() by bin(TimeGenerated, trendBinSize), ClusterName
    | join hint.strategy=broadcast (
        KubePodInventory
        | where TimeGenerated < endDateTime
        | where TimeGenerated >= startDateTime
        | distinct ClusterName, Computer, PodUid, TimeGenerated, PodStatus
        | summarize TotalCount = count(),
                    PendingCount = sumif(1, PodStatus =~ 'Pending'),
                    RunningCount = sumif(1, PodStatus =~ 'Running'),
                    SucceededCount = sumif(1, PodStatus =~ 'Succeeded'),
                    FailedCount = sumif(1, PodStatus =~ 'Failed')
                 by ClusterName, bin(TimeGenerated, trendBinSize)
    ) on ClusterName, TimeGenerated
    | extend UnknownCount = TotalCount - PendingCount - RunningCount - SucceededCount - FailedCount
    | project TimeGenerated,
              TotalCount = todouble(TotalCount) / ClusterSnapshotCount,
              PendingCount = todouble(PendingCount) / ClusterSnapshotCount,
              RunningCount = todouble(RunningCount) / ClusterSnapshotCount,
              SucceededCount = todouble(SucceededCount) / ClusterSnapshotCount,
              FailedCount = todouble(FailedCount) / ClusterSnapshotCount,
              UnknownCount = todouble(UnknownCount) / ClusterSnapshotCount
| summarize AggregatedValue = avg(PendingCount) by bin(TimeGenerated, trendBinSize)
```

>[!NOTE]
>Para generar una alerta en ciertas fases de pod, tales como *pendiente*, *Failed*, o *desconocido*, modifique la última línea de la consulta. Por ejemplo, para generar alertas sobre *FailedCount* usar: <br/>`| summarize AggregatedValue = avg(FailedCount) by bin(TimeGenerated, trendBinSize)`

## <a name="create-an-alert-rule"></a>Crear una regla de alerta
Siga estos pasos para crear una alerta de registro en Azure Monitor con una de las reglas de búsqueda de registro proporcionada anteriormente.  

>[!NOTE]
>El siguiente procedimiento para crear una regla de alerta de utilización de recursos de contenedor, deberá cambiar a un nuevo registro de alertas de API como se describe en [preferencia cambie API para las alertas del registro](../platform/alerts-log-api-switch.md).
>

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Seleccione **Monitor** desde el panel del lado izquierdo. En **Insights**, seleccione **contenedores**.
3. En el **supervisar clústeres** pestaña, seleccione un clúster en la lista.
4. En el panel de la izquierda, bajo **supervisión**, seleccione **registros** para abrir la página de registros de Azure Monitor. Utilice esta página para escribir y ejecutar consultas de Azure Log Analytics.
5. En el **registros** página, seleccione **+ nueva regla de alerta**.
6. En el **condición** sección, seleccione el **es cada vez que el registro búsqueda personalizada \<lógica undefined >** definidas previamente la condición de registro personalizado. El **búsqueda de registros personalizado** porque estamos creando una regla de alerta directamente desde la página de registros de Azure Monitor, se selecciona automáticamente el tipo de señal.  
7. Pegue uno de los [consultas](#resource-utilization-log-search-queries) proporcionado anteriormente en el **consulta de búsqueda** campo.
8. Configure la alerta como sigue:

    1. En la lista desplegable **Basado en**, seleccione **Unidades métricas**. Una métrica medida crea una alerta para cada objeto en la consulta que tiene un valor superior a nuestro umbral especificado.
    1. Para **condición**, seleccione **mayor**y escriba **75** como una línea base inicial **umbral**. O escriba un valor diferente que cumpla sus criterios.
    1. En el **desencadenador alerta basada en** sección, seleccione **infracciones consecutivas**. En la lista desplegable, seleccione **mayor**y escriba **2**.
    1. Para configurar una alerta de CPU de contenedor o el uso de memoria, en **agregado en**, seleccione **ContainerName**. 
    1. En el **evaluado según** sección, establezca el **período** valor **60 minutos**. La regla se ejecuta cada 5 minutos y devolver los registros que se crearon dentro de la última hora a la hora actual. Establecer el período de tiempo a las cuentas de una ventana amplia posible latencia de datos. También garantiza que la consulta devuelve datos para evitar un falso negativo en el que la alerta nunca se activa.

9. Seleccione **realiza** para completar la regla de alerta.
10. Escriba un nombre en el **nombre de regla de alerta** campo. Especifique un **descripción** que proporciona detalles sobre la alerta. Y seleccione un nivel de gravedad adecuado de las opciones proporcionadas.
11. Para activar inmediatamente la regla de alerta, acepte el valor predeterminado para **Habilitar regla tras la creación**.
12. Seleccione una existente **grupo de acciones** o crear un nuevo grupo. Este paso garantiza que las mismas acciones se realizan cada vez que se desencadena una alerta. Configurar en función de cómo el departamento de TI o administra el equipo de operaciones de DevOps incidentes.
13. Seleccione **crear regla de alertas** para completar la regla de alerta. Se iniciará la ejecución de inmediato.

## <a name="next-steps"></a>Pasos siguientes

- Vista [ejemplos de consultas de registro](container-insights-log-search.md#search-logs-to-analyze-data) para ver consultas predefinidas y ejemplos para evaluar o personalizar para las alertas, visualizar o analizar los clústeres.
- Para obtener más información acerca de Azure Monitor y cómo supervisar otros aspectos de su clúster de AKS, consulte [estado de vista de Azure Kubernetes Service](container-insights-analyze.md).
