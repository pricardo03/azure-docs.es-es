---
title: Consulta de registros desde Azure Monitor para contenedores | Microsoft Docs
description: Azure Monitor para contenedores recopila datos de registro y métricas. En este artículo se describen los registros y se incluyen consultas de ejemplo.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: ''
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/12/2019
ms.author: magoedte
ms.openlocfilehash: d6e65331db53be5ba13a75e6b03b271f1071716d
ms.sourcegitcommit: 6b41522dae07961f141b0a6a5d46fd1a0c43e6b2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/15/2019
ms.locfileid: "67989830"
---
# <a name="how-to-query-logs-from-azure-monitor-for-containers"></a>Cómo consultar registros desde Azure Monitor para contenedores

Azure Monitor para contenedores recopila métricas de rendimiento, datos de inventario e información de estado de mantenimiento de los contenedores y los hosts de contenedor y los reenvía al área de trabajo de Log Analytics en Azure Monitor. Los datos se recopilan cada tres minutos. Estos datos están disponibles para [consulta](../../azure-monitor/log-query/log-query-overview.md) en Azure Monitor. Estos datos se pueden aplicar a escenarios que incluyen la planeación de la migración, el análisis de la capacidad, la detección y la solución de problemas de rendimiento a petición.

## <a name="container-records"></a>Registros de contenedor

En la siguiente tabla se muestran ejemplos de registros recopilados por Azure Monitor para contenedores y los tipos de datos que aparecen en los resultados de la búsqueda de registros.

| Tipo de datos | Tipo de datos en la búsqueda de registros | Fields |
| --- | --- | --- |
| Rendimiento de hosts y contenedores | `Perf` | Computer, ObjectName, CounterName &#40;%Processor Time, Disk Reads MB, Disk Writes MB, Memory Usage MB, Network Receive Bytes, Network Send Bytes, Processor Usage sec, Network&#41;, CounterValue, TimeGenerated, CounterPath, SourceSystem |
| Inventario de contenedor | `ContainerInventory` | TimeGenerated, Computer, container name, ContainerHostname, Image, ImageTag, ContainerState, ExitCode, EnvironmentVar, Command, CreatedTime, StartedTime, FinishedTime, SourceSystem, ContainerID, ImageID |
| Registro de contenedor | `ContainerLog` | TimeGenerated, Computer, image ID, container name, LogEntrySource, LogEntry, SourceSystem, ContainerID |
| Inventario de nodo de contenedor | `ContainerNodeInventory`| TimeGenerated, Computer, ClassName_s, DockerVersion_s, OperatingSystem_s, Volume_s, Network_s, NodeRole_s, OrchestratorType_s, InstanceID_g, SourceSystem|
| Inventario de pods en un clúster de Kubernetes | `KubePodInventory` | TimeGenerated, Computer, ClusterId, ContainerCreationTimeStamp, PodUid, PodCreationTimeStamp, ContainerRestartCount, PodRestartCount, PodStartTime, ContainerStartTime, ServiceName, ControllerKind, ControllerName, ContainerStatus,  ContainerStatusReason, ContainerID, ContainerName, Name, PodLabel, Namespace, PodStatus, ClusterName, PodIp, SourceSystem |
| Inventario de la parte de nodos de un clúster de Kubernetes | `KubeNodeInventory` | TimeGenerated, Computer, ClusterName, ClusterId, LastTransitionTimeReady, Labels, Status, KubeletVersion, KubeProxyVersion, CreationTimeStamp, SourceSystem | 
| Eventos de Kubernetes | `KubeEvents` | TimeGenerated, Computer, ClusterId_s, FirstSeen_t, LastSeen_t, Count_d, ObjectKind_s, Namespace_s, Name_s, Reason_s, Type_s, TimeGenerated_s, SourceComponent_s, ClusterName_s, Message, SourceSystem | 
| Servicios en el clúster de Kubernetes | `KubeServices` | TimeGenerated, ServiceName_s, Namespace_s, SelectorLabels_s, ClusterId_s, ClusterName_s, ClusterIP_s, ServiceType_s, SourceSystem | 
| Métricas de rendimiento de la parte de nodos del clúster de Kubernetes | Perf &#124; donde ObjectName == “K8SNode” | Computer, ObjectName, CounterName &#40;cpuAllocatableBytes, memoryAllocatableBytes, cpuCapacityNanoCores, memoryCapacityBytes, memoryRssBytes, cpuUsageNanoCores, memoryWorkingsetBytes, restartTimeEpoch&#41;, CounterValue, TimeGenerated, CounterPath, SourceSystem | 
| Métricas de rendimiento de la parte de contenedores del clúster de Kubernetes | Perf &#124; donde ObjectName == “K8SContainer” | CounterName &#40; cpuRequestNanoCores, memoryRequestBytes, cpuLimitNanoCores, memoryWorkingSetBytes, restartTimeEpoch, cpuUsageNanoCores, memoryRssBytes&#41;, CounterValue, TimeGenerated, CounterPath, SourceSystem | 
| Métricas personalizadas |`InsightsMetrics` | Computer, Name, Namespace, Origin, SourceSystem, Tags<sup>1</sup>, TimeGenerated, Type, Va, _ResourceId | 

<sup>1</sup> La propiedad *Tags* representa [varias dimensiones](../platform/data-platform-metrics.md#multi-dimensional-metrics) para la métrica correspondiente. Para información adicional sobre las métricas recopiladas y almacenadas en la tabla `InsightsMetrics` y una descripción de las propiedades de registro, consulte [InsightsMetrics overview](https://github.com/microsoft/OMS-docker/blob/vishwa/june19agentrel/docs/InsightsMetrics.md) (Información general de InsightsMetrics).

>[!NOTE]
>La compatibilidad con Prometheus es una característica que se encuentra en versión preliminar pública en este momento.
>

## <a name="search-logs-to-analyze-data"></a>Búsqueda de registros para analizar datos

Los registros de Azure Monitor pueden ayudarle a buscar tendencias, diagnosticar cuellos de botellas, realizar previsiones o correlacionar datos, que pueden servirle para determinar si la configuración actual del clúster funciona óptimamente. Las búsquedas de registros predefinidas se proporcionan para que comience a usarlas inmediatamente o para que las personalice con el fin de devolver la información de la forma deseada.

Puede realizar un análisis interactivo de los datos en el área de trabajo mediante la selección de la opción **View Kubernetes event logs** (Ver registros de eventos de Kubernetes) o **View container logs** (Ver registros de contenedor) en el panel de vista previa. La página **Búsqueda de registros** aparece a la derecha de la página de Azure Portal en que se encontraba.

![Análisis de los datos en Log Analytics](./media/container-insights-analyze/container-health-log-search-example.png)   

Los registros de contenedor generador que se reenvían al área de trabajo son STDOUT y STDERR. Dado que Azure Monitor supervisa Kubernetes administrado por Azure (AKS), el sistema de Kubernetes no se recopila hoy debido al gran volumen de datos generados. 

### <a name="example-log-search-queries"></a>Ejemplos de consultas de búsqueda de registros

A menudo resulta útil crear consultas que comiencen con un ejemplo o dos y luego modificarlas para ajustarlas a sus requisitos. Para ayudarle a crear consultas más avanzadas, puede experimentar con las siguientes consultas de ejemplo:

| Consultar | DESCRIPCIÓN | 
|-------|-------------|
| ContainerInventory<br> &#124; project Computer, Name, Image, ImageTag, ContainerState, CreatedTime, StartedTime, FinishedTime<br> &#124; render table | Se muestra toda la información del ciclo de vida de un contenedor| 
| KubeEvents_CL<br> &#124; where not(isempty(Namespace_s))<br> &#124; sort by TimeGenerated desc<br> &#124; render table | Eventos de Kubernetes|
| ContainerImageInventory<br> &#124; summarize AggregatedValue = count() by Image, ImageTag, Running | Inventario de imágenes | 
| **Seleccione la opción de visualización del gráfico de líneas**:<br> Perf<br> &#124; where ObjectName == "K8SContainer" and CounterName == "cpuUsageNanoCores" &#124; summarize AvgCPUUsageNanoCores = avg(CounterValue) by bin(TimeGenerated, 30m), InstanceName | CPU de contenedor | 
| **Seleccione la opción de visualización del gráfico de líneas**:<br> Perf<br> &#124; where ObjectName == "K8SContainer" and CounterName == "memoryRssBytes" &#124; summarize AvgUsedRssMemoryBytes = avg(CounterValue) by bin(TimeGenerated, 30m), InstanceName | Memoria de contenedor |

El ejemplo siguiente es una consulta de métricas de Prometheus. Las métricas recopiladas son recuentos y, para determinar el número de errores que se han producido dentro de un período de tiempo específico, tenemos que restar del recuento. El conjunto de datos se particiona mediante *partitionKey*, lo que significa que para cada conjunto único de *Name*, *HostName* y *OperationType*, ejecutamos una subconsulta en ese conjunto que ordena los registros por *TimeGenerated*, un proceso que permite encontrar el *TimeGenerated* anterior y el recuento registrado durante ese tiempo, para determinar un índice.

```
let data = InsightsMetrics 
| where Namespace contains 'prometheus' 
| where Name == 'kubelet_docker_operations' or Name == 'kubelet_docker_operations_errors'    
| extend Tags = todynamic(Tags) 
| extend OperationType = tostring(Tags['operation_type']), HostName = tostring(Tags.hostName) 
| extend partitionKey = strcat(HostName, '/' , Name, '/', OperationType) 
| partition by partitionKey ( 
    order by TimeGenerated asc 
    | extend PrevVal = prev(Val, 1), PrevTimeGenerated = prev(TimeGenerated, 1) 
    | extend Rate = iif(TimeGenerated == PrevTimeGenerated, 0.0, Val - PrevVal) 
    | where isnull(Rate) == false 
) 
| project TimeGenerated, Name, HostName, OperationType, Rate; 
let operationData = data 
| where Name == 'kubelet_docker_operations' 
| project-rename OperationCount = Rate; 
let errorData = data 
| where Name == 'kubelet_docker_operations_errors' 
| project-rename ErrorCount = Rate; 
operationData 
| join kind = inner ( errorData ) on TimeGenerated, HostName, OperationType 
| project-away TimeGenerated1, Name1, HostName1, OperationType1 
| extend SuccessPercentage = iif(OperationCount == 0, 1.0, 1 - (ErrorCount / OperationCount))
```

La salida mostrará resultados similares a los siguientes:

![Registro de los resultados de la consulta del volumen de ingesta de datos](./media/container-insights-log-search/log-query-example-prometheus-metrics.png)

## <a name="next-steps"></a>Pasos siguientes

Azure Monitor para contenedores no incluye un conjunto de alertas predefinido. Consulte [Creación de alertas de rendimiento con Azure Monitor para contenedores](container-insights-alerts.md) para aprender a crear alertas recomendadas en caso de uso elevado de CPU y memoria a fin de permitir sus operaciones de desarrollo o sus procesos y procedimientos operativos. 