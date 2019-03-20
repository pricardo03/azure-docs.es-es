---
title: Creación de alertas de rendimiento con Azure Monitor para contenedores | Microsoft Docs
description: En este artículo se describe cómo puede crear alertas de Azure personalizadas basadas en consultas de registro para el uso de memoria y de CPU desde Azure Monitor para contenedores.
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
ms.date: 02/26/2019
ms.author: magoedte
ms.openlocfilehash: e6fdb0d57a44578647c1f16dc76c557296f20ddb
ms.sourcegitcommit: 24906eb0a6621dfa470cb052a800c4d4fae02787
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/27/2019
ms.locfileid: "56886788"
---
# <a name="how-to-set-up-alerts-for-performance-problems-in-azure-monitor-for-containers"></a>Cómo configurar alertas para los problemas de rendimiento en Azure Monitor para contenedores
Azure Monitor para contenedores supervisa el rendimiento de las cargas de trabajo de contenedor implementadas en Azure Container Instances o en clústeres de Kubernetes administrados hospedados en Azure Kubernetes Service (AKS). 

Este artículo describe cómo habilitar la generación de alertas para las situaciones siguientes:

* Al uso de CPU y memoria en los nodos del clúster o supera el umbral definido.
* Cuando el uso de CPU o memoria en cualquiera de los contenedores dentro de un controlador supera el umbral definido en comparación con el límite establecido en el recurso correspondiente.

Para generar una alerta cuando el uso de CPU o memoria es alto para un clúster o un controlador, cree una regla de alerta de unidades métricas que se basa en las consultas de registro proporcionadas. Las consultas comparar un valor datetime en el presente mediante el operador ahora y vuelve una hora. Todas las fechas almacenadas por Azure Monitor para contenedores están en formato UTC.

Antes de comenzar, si no está familiarizado con las alertas en Azure Monitor, consulte [Información general sobre las alertas en Microsoft Azure](../platform/alerts-overview.md). Para más información acerca de las alertas mediante consultas de registros, consulte [Alertas de registro en Azure Monitor](../platform/alerts-unified-log.md)

## <a name="resource-utilization-log-search-queries"></a>Consultas de búsqueda de registro de utilización de recursos
Las consultas en esta sección se proporcionan para cada escenario de alertas. Las consultas son necesarias para el paso 7 en el [crear alerta](#create-alert-rule) sección más adelante.  

En la siguiente consulta se calcula el uso promedio de CPU como una utilización promedio de la CPU de los nodos miembros cada minuto.  

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

En la siguiente consulta se calcula el uso promedio de la memoria como una utilización promedio de memoria de los nodos miembros cada minuto.

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
>Consultas siguientes contienen los valores de cadena de marcador de posición para los nombres de clúster y el controlador: < su nombre de clúster > y < your--nombre del controlador >. Reemplace los marcadores de posición con valores específicos de su entorno antes de configurar alertas. 


La siguiente consulta calcula el uso de CPU promedio de todos los contenedores en un controlador como un promedio de uso de CPU de cada instancia del contenedor en un controlador de cada minuto como un porcentaje del límite configurado para un contenedor.

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

La siguiente consulta calcula el uso de memoria promedio de todos los contenedores en un controlador como un promedio de uso de memoria de cada instancia de contenedor en un controlador de cada minuto como un porcentaje del límite configurado para un contenedor.

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

## <a name="create-alert-rule"></a>Crear regla de alertas
Realice los pasos siguientes para crear una alerta de registro en Azure Monitor con una de las reglas de búsqueda de registro proporcionadas anteriormente.  

>[!NOTE]
>El siguiente procedimiento, deberá cambiar a la nueva API de alertas de registros como se describe en [preferencia cambie API para las alertas del registro](../platform/alerts-log-api-switch.md) si va a crear una regla de alerta de utilización de recursos del contenedor. 
>

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Seleccione **Monitor** en el panel de la izquierda de Azure Portal. En la sección **Insights**  (Conclusiones), seleccione **Contenedores**.    
3. En la pestaña **Clústeres supervisados**, seleccione un clúster en la lista, haga clic en el nombre del clúster.
4. En el panel izquierdo, bajo la sección **Supervisión**, seleccione **Registros** para abrir la página de registros de Azure Monitor, que se utiliza para escribir y ejecutar consultas de Azure Log Analytics.
5. En la página **Registros**, haga clic en **+ Nueva regla de alertas**.
6. En la sección **Condición**, haga clic en la condición de registro personalizado predefinida **Cada vez que la búsqueda de registros personalizada sea <logic undefined>**. El tipo de señal de **búsqueda de registros personalizada** se selecciona automáticamente porque se ha iniciado la creación de una regla de alertas directamente desde la página de registros de Azure Monitor.  
7. Pegue uno de los [consultas](#resource-utilization-log-search-queries) proporcionado anteriormente en el **consulta de búsqueda** campo. 

8. Configure la alerta con la siguiente información:

     a. En la lista desplegable **Basado en**, seleccione **Unidades métricas**. Una medida de métricas creará una alerta por cada objeto de la consulta con un valor que supere el umbral especificado.  
    b. En **Condición**, seleccione **Mayor que** y escriba **75** como un **umbral** de línea de base inicial o escriba un valor que cumpla sus criterios.  
    c. En la sección **Desencadenar alerta según**, seleccione **Infracciones consecutivas** y, en la lista desplegable, seleccione **Mayor que** y escriba un valor de **2**.  
    d. Si configura una alerta de CPU de contenedor o el uso de memoria, en **agregado en** seleccione **ContainerName** en la lista desplegable.  
    e. En la sección **Se evaluó basándose en**, modifique el valor de **Período** a 60 minutos. La regla se ejecutará cada cinco minutos y devolverá los registros que se crearon dentro de la última hora desde la hora actual. El hecho de establecer el período de tiempo en una ventana más amplia justifica la posibilidad de latencia en los datos y garantiza que la consulta devuelve datos para evitar un falso negativo en aquellos casos en los que la alerta nunca se activa. 

9. Haga clic en **Listo** para finalizar la regla de alertas.
10. Proporcione un nombre de la alerta en el campo **Nombre de la regla de alertas**. Especifique una **descripción** con los detalles específicos de la alerta y seleccione una gravedad adecuada en las opciones proporcionadas.
11. Para activar inmediatamente la regla de alertas en la creación, acepte el valor predeterminado de **Habilitar regla tras la creación**.
12. En el paso final, seleccione un **grupo de acciones** existente o cree uno, lo que garantiza que se realizan las mismas acciones cada vez que se desencadena una alerta y se puede utilizar para cada regla que defina. Configure en función de cómo las operaciones de TI o DevOps administran los incidentes. 
13. Haga clic en **Crear regla de alertas** para finalizar la regla de alertas. Se iniciará la ejecución de inmediato.

## <a name="next-steps"></a>Pasos siguientes

* Revise algunos de los [ejemplos de consultas de registros](container-insights-analyze.md#search-logs-to-analyze-data) y aprenda acerca de las consultas predefinidas o ejemplos para evaluar y usar o personalizar para otros escenarios de alerta. 
* Para seguir aprendiendo a usar Azure Monitor y supervisar otros aspectos de su clúster de AKS, consulte [Comprender el rendimiento del clúster de AKS con Azure Monitor para contenedores](container-insights-analyze.md)
