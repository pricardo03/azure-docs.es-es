---
title: Cómo los registros de consultas de Azure Monitor para contenedores | Microsoft Docs
description: Azure Monitor para contenedores recopila datos de registro y métricas y en este artículo se describe los registros e incluye las consultas de ejemplo.
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
ms.date: 04/17/2019
ms.author: magoedte
ms.openlocfilehash: 66fc55d8c3dbb8487d1e796d5f30b08a94f717f6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60494782"
---
# <a name="how-to-query-logs-from-azure-monitor-for-containers"></a>Cómo consultar los registros de Azure Monitor para contenedores
Supervisión de contenedores de Azure recopila métricas de rendimiento, datos de inventario y la información de estado de mantenimiento de contenedores y los hosts y lo reenvía al área de trabajo de Log Analytics en Azure Monitor. Los datos se recopilan cada tres minutos. Estos datos están disponibles para [consulta](../../azure-monitor/log-query/log-query-overview.md) en Azure Monitor. Estos datos se pueden aplicar a escenarios que incluyen la planeación de la migración, el análisis de la capacidad, la detección y la solución de problemas de rendimiento a petición.

## <a name="container-records"></a>Registros de contenedor

En la siguiente tabla se muestran ejemplos de registros recopilados por Azure Monitor para contenedores y los tipos de datos que aparecen en los resultados de la búsqueda de registros.

| Tipo de datos | Tipo de datos en la búsqueda de registros | Fields |
| --- | --- | --- |
| Rendimiento de hosts y contenedores | `Perf` | Computer, ObjectName, CounterName &#40;%Processor Time, Disk Reads MB, Disk Writes MB, Memory Usage MB, Network Receive Bytes, Network Send Bytes, Processor Usage sec, Network&#41;, CounterValue, TimeGenerated, CounterPath, SourceSystem |
| Inventario de contenedor | `ContainerInventory` | TimeGenerated, Computer, container name, ContainerHostname, Image, ImageTag, ContainerState, ExitCode, EnvironmentVar, Command, CreatedTime, StartedTime, FinishedTime, SourceSystem, ContainerID, ImageID |
| Registro de contenedor | `ContainerLog` | TimeGenerated, Computer, image ID, container name, LogEntrySource, LogEntry, SourceSystem, ContainerID |
| Inventario de nodo de contenedor | `ContainerNodeInventory`| TimeGenerated, Computer, ClassName_s, DockerVersion_s, OperatingSystem_s, Volume_s, Network_s, NodeRole_s, OrchestratorType_s, InstanceID_g, SourceSystem|
| Inventario de pods en un clúster de Kubernetes | `KubePodInventory` | TimeGenerated, equipo, ClusterId, ContainerCreationTimeStamp, PodUid, PodCreationTimeStamp, ContainerRestartCount, PodRestartCount, PodStartTime, ContainerStartTime, ServiceName, ControllerKind, ControllerName, ContainerStatus,  ContainerStatusReason, ContainerID, ContainerName, nombre, PodLabel, Namespace, PodStatus, ClusterName, pod, SourceSystem |
| Inventario de la parte de nodos de un clúster de Kubernetes | `KubeNodeInventory` | TimeGenerated, Computer, ClusterName, ClusterId, LastTransitionTimeReady, Labels, Status, KubeletVersion, KubeProxyVersion, CreationTimeStamp, SourceSystem | 
| Eventos de Kubernetes | `KubeEvents` | TimeGenerated, Computer, ClusterId_s, FirstSeen_t, LastSeen_t, Count_d, ObjectKind_s, Namespace_s, Name_s, Reason_s, Type_s, TimeGenerated_s, SourceComponent_s, ClusterName_s, Message, SourceSystem | 
| Servicios en el clúster de Kubernetes | `KubeServices` | TimeGenerated, ServiceName_s, Namespace_s, SelectorLabels_s, ClusterId_s, ClusterName_s, ClusterIP_s, ServiceType_s, SourceSystem | 
| Métricas de rendimiento de la parte de nodos del clúster de Kubernetes | Perf &#124; donde ObjectName == “K8SNode” | Equipo, ObjectName, CounterName &#40;cpuAllocatableBytes, memoryAllocatableBytes, cpuCapacityNanoCores, memoryCapacityBytes, memoryRssBytes, cpuUsageNanoCores, memoryWorkingsetBytes, restartTimeEpoch&#41;, CounterValue, TimeGenerated, CounterPath, SourceSystem | 
| Métricas de rendimiento de la parte de contenedores del clúster de Kubernetes | Perf &#124; donde ObjectName == “K8SContainer” | CounterName &#40; cpuRequestNanoCores, memoryRequestBytes, cpuLimitNanoCores, memoryWorkingSetBytes, restartTimeEpoch, cpuUsageNanoCores, memoryRssBytes&#41;, CounterValue, TimeGenerated, CounterPath y SourceSystem | 
| InsightsMetrics | Equipo, nombre, Namespace, origen, SourceSystem, etiquetas, TimeGenerated, tipo, valor |

## <a name="search-logs-to-analyze-data"></a>Búsqueda de registros para analizar datos
Registros de Monitor de Azure puede ayudarle a buscar tendencias, diagnosticar cuellos de botella, realizar previsiones o correlacionar datos que pueden ayudarle a determinan si la configuración del clúster actual es un rendimiento óptimo. Las búsquedas de registros predefinidas se proporcionan para que comience a usarlas inmediatamente o para que las personalice con el fin de devolver la información de la forma deseada. 

Puede realizar un análisis interactivo de los datos en el área de trabajo mediante la selección de la opción **View Kubernetes event logs** (Ver registros de eventos de Kubernetes) o **View container logs** (Ver registros de contenedor) en el panel de vista previa. La página **Búsqueda de registros** aparece a la derecha de la página de Azure Portal en que se encontraba.

![Análisis de los datos en Log Analytics](./media/container-insights-analyze/container-health-log-search-example.png)   

La salida de los registros de contenedor que se reenvía a su área de trabajo son STDOUT y STDERR. Dado que Azure Monitor supervisa Kubernetes administrado por Azure (AKS), el sistema de Kubernetes no se recopila hoy debido al gran volumen de datos generados. 

### <a name="example-log-search-queries"></a>Ejemplos de consultas de búsqueda de registros
A menudo resulta útil crear consultas que comiencen con un ejemplo o dos y luego modificarlas para ajustarlas a sus requisitos. Para ayudarle a crear consultas más avanzadas, puede experimentar con las siguientes consultas de ejemplo:

| Consultar | DESCRIPCIÓN | 
|-------|-------------|
| ContainerInventory<br> &#124; project Computer, Name, Image, ImageTag, ContainerState, CreatedTime, StartedTime, FinishedTime<br> &#124; render table | Se muestra toda la información del ciclo de vida de un contenedor| 
| KubeEvents_CL<br> &#124; where not(isempty(Namespace_s))<br> &#124; sort by TimeGenerated desc<br> &#124; render table | Eventos de Kubernetes|
| ContainerImageInventory<br> &#124; summarize AggregatedValue = count() by Image, ImageTag, Running | Inventario de imágenes | 
| **Seleccione la opción de visualización del gráfico de líneas**:<br> Perf<br> &#124; where ObjectName == "K8SContainer" and CounterName == "cpuUsageNanoCores" &#124; summarize AvgCPUUsageNanoCores = avg(CounterValue) by bin(TimeGenerated, 30m), InstanceName | CPU de contenedor | 
| **Seleccione la opción de visualización del gráfico de líneas**:<br> Perf<br> &#124; where ObjectName == "K8SContainer" and CounterName == "memoryRssBytes" &#124; summarize AvgUsedRssMemoryBytes = avg(CounterValue) by bin(TimeGenerated, 30m), InstanceName | Memoria de contenedor |

## <a name="next-steps"></a>Pasos siguientes
Azure Monitor para los contenedores no incluye un conjunto predefinido de alertas. Revise el [crear alertas de rendimiento con Azure Monitor para contenedores](container-insights-alerts.md) para obtener información sobre cómo crear alertas recomendadas para el uso elevado de CPU y memoria admitir su DevOps o procedimientos y procesos operativos. 