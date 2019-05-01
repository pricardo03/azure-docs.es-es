---
title: Supervisión y diagnóstico en aplicaciones de Azure Service Fabric mesh | Microsoft Docs
description: Obtenga información sobre la supervisión y el diagnóstico de aplicaciones en Service Fabric mesh en Azure.
services: service-fabric-mesh
documentationcenter: .net
author: srrengar
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric-mesh
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/19/2019
ms.author: srrengar
ms.custom: mvc, devcenter
ms.openlocfilehash: 36c9a5d75c4a72365638619ab85d451df647feb3
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2019
ms.locfileid: "64939822"
---
# <a name="monitoring-and-diagnostics"></a>Supervisión y diagnóstico
Azure Service Fabric mesh es un servicio totalmente administrado que permite a los desarrolladores implementar aplicaciones de microservicios sin la administración de máquinas virtuales, almacenamiento o redes. La supervisión y el diagnóstico para Service Fabric mesh se clasifican en tres tipos principales de datos de diagnóstico:

- Registros de aplicación: se definen como registros de aplicaciones en contenedores, según cómo haya instrumentado la aplicación (por ejemplo, registros de Docker)
- Eventos de plataforma: eventos de la plataforma mesh pertinentes para la operación de contenedor. Actualmente, se incluyen la activación, desactivación y terminación del contenedor.
- Métricas de contenedor: métricas de rendimiento y uso de recursos para los contenedores (estadísticas de Docker)

En este artículo se describen las opciones de supervisión y diagnóstico para la última versión preliminar disponible.

## <a name="application-logs"></a>Registros de aplicación

Puede ver los registros de Docker de los contenedores implementados, por contenedor. En el modelo de aplicación de Service Fabric mesh, cada contenedor es un paquete de código en la aplicación. Para ver los registros asociados con un paquete de código, use el siguiente comando:

```cli
az mesh code-package-log get --resource-group <nameOfRG> --app-name <nameOfApp> --service-name <nameOfService> --replica-name <nameOfReplica> --code-package-name <nameOfCodePackage>
```

> [!NOTE]
> Puede usar el comando "az mesh service-replica" para obtener el nombre de la réplica. El incremento de los nombres de la réplica son enteros de 0.

Este es el aspecto de los registros del contenedor VotingWeb.Code desde la aplicación de votación:

```cli
az mesh code-package-log get --resource-group <nameOfRG> --application-name SbzVoting --service-name VotingWeb --replica-name 0 --code-package-name VotingWeb.Code
```

## <a name="container-metrics"></a>Métricas de contenedor 

El entorno de malla expone una serie de métricas que indican cómo funcionan los contenedores. Las siguientes métricas están disponibles a través de Azure portal y Azure monitor CLI:

| Métrica | DESCRIPCIÓN | Unidades|
|----|----|----|
| Gráfico | ActualCpu/AllocatedCpu como un porcentaje | % |
| MemoryUtilization | ActualMem/AllocatedMem como un porcentaje | % |
| AllocatedCpu | CPU asignada según la plantilla de Azure Resource Manager | Millicores |
| AllocatedMemory | Memoria asignada según la plantilla de Azure Resource Manager | MB |
| ActualCpu | Uso de CPU | Millicores |
| ActualMemory | Uso de la memoria | MB |
| ContainerStatus | 0 - válido: Se desconoce el estado de contenedor <br> 1 - pendiente: El contenedor se programó para comenzar <br> 2 - inicial: El contenedor está en proceso de inicio <br> 3 - started: El contenedor se ha iniciado correctamente <br> 4 - detener: El contenedor se está deteniendo <br> 5 - detenido: El contenedor se ha detenido correctamente | N/D |
| ApplicationStatus | 0 - desconocido: El estado no es recuperable <br> 1 - listo: La aplicación se ejecuta correctamente <br> 2 - actualización: Hay una actualización en curso <br> 3 - creación: Se está creando la aplicación <br> 4 - eliminar: Se está eliminando la aplicación <br> 5 - Error: No se pudo implementar la aplicación | N/D |
| ServiceStatus | 0 - válido: El servicio no tiene actualmente un estado de mantenimiento <br> 1 - Aceptar: El servicio es correcto  <br> 2 - advertencia: Puede haber algún problema que requiere investigación <br> 3 - error: Hay algún problema que precisa investigación <br> 4 - desconocido: El estado no es recuperable | N/D |
| ServiceReplicaStatus | 0 - válido: La réplica no tiene actualmente un estado de mantenimiento <br> 1 - Aceptar: El servicio es correcto  <br> 2 - advertencia: Puede haber algún problema que requiere investigación <br> 3 - error: Hay algún problema que precisa investigación <br> 4 - desconocido: El estado no es recuperable | N/D | 
| RestartCount | Número de reinicios de contenedor | N/D |

> [!NOTE]
> Los valores ServiceStatus y ServiceReplicaStatus son los mismos que los [HealthState](https://docs.microsoft.com/dotnet/api/system.fabric.health.healthstate?view=azure-dotnet) en Service Fabric. 

Cada métrica está disponible en distintas dimensiones para que pueda ver agregados en distintos niveles. La lista actual de las dimensiones son los siguientes:

* ApplicationName
* ServiceName
* ServiceReplicaName
* CodePackageName

> [!NOTE]
> La dimensión CodePackageName no está disponible para las aplicaciones de Linux. 

Cada dimensión corresponde a los diferentes componentes de la [el modelo de aplicación de Service Fabric](service-fabric-mesh-service-fabric-resources.md#applications-and-services)

### <a name="azure-monitor-cli"></a>CLI de Azure Monitor

Una lista completa de comandos está disponible en el [documentos con la CLI de Azure Monitor](https://docs.microsoft.com/cli/azure/monitor/metrics?view=azure-cli-latest#az-monitor-metrics-list) pero hemos incluido algunos ejemplos útiles siguiente 

En cada ejemplo, el identificador de recurso sigue este patrón

`"/subscriptions/<your sub ID>/resourcegroups/<your RG>/providers/Microsoft.ServiceFabricMesh/applications/<your App name>"`


* Uso de CPU de los contenedores en una aplicación

```cli
    az monitor metrics list --resource <resourceId> --metric "CpuUtilization"
```
* Uso de memoria para cada réplica de servicio
```cli
    az monitor metrics list --resource <resourceId> --metric "MemoryUtilization" --dimension "ServiceReplicaName"
``` 

* Se reinicia para cada contenedor en una ventana de 1 hora 
```cli
    az monitor metrics list --resource <resourceId> --metric "RestartCount" --start-time 2019-02-01T00:00:00Z --end-time 2019-02-01T01:00:00Z
``` 

* Uso de CPU promedio entre servicios con nombre "VotingWeb" en una ventana de 1 hora
```cli
    az monitor metrics list --resource <resourceId> --metric "CpuUtilization" --start-time 2019-02-01T00:00:00Z --end-time 2019-02-01T01:00:00Z --aggregation "Average" --filter "ServiceName eq 'VotingWeb'"
``` 

### <a name="metrics-explorer"></a>Explorador de métricas

Explorador de métricas es una hoja en el portal en el que puede visualizar las métricas para la aplicación de malla. Esta hoja es accesible en la página de aplicación en el portal y la hoja Azure monitor, el último de los cuales puede usar para ver las métricas para todos los recursos de Azure que admiten a Azure Monitor. 

![Explorador de métricas](./media/service-fabric-mesh-monitoring-diagnostics/metricsexplorer.png)


<!--
### Container Insights

In addition to the metrics explorer, we also have a dashboard available out of the box that shows sample metrics over time under the Insights blade in the application's page in the portal. 

![Container Insights](./media/service-fabric-mesh-monitoring-diagnostics/containerinsights.png)
-->

## <a name="next-steps"></a>Pasos siguientes
* Para obtener más información sobre Service Fabric Mesh, consulte la [información general de Service Fabric Mesh](service-fabric-mesh-overview.md).
* Para obtener más información sobre los comandos de las métricas de Azure Monitor, consulte el [documentos con la CLI de Azure Monitor](https://docs.microsoft.com/cli/azure/monitor/metrics?view=azure-cli-latest#az-monitor-metrics-list).
