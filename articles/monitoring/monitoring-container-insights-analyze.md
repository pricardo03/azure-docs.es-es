---
title: Supervisar el rendimiento del clúster de AKS con Azure Monitor para contenedores (versión preliminar) | Microsoft Docs
description: En este artículo se describe cómo puede ver y analizar los datos de registro y rendimiento con Azure Monitor para contenedores.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/19/2018
ms.author: magoedte
ms.openlocfilehash: daec3d6e6cd8e4df3fdfe45fbb8ee98966c8a38e
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2018
ms.locfileid: "50214161"
---
# <a name="understand-aks-cluster-performance-with-azure-monitor-for-containers-preview"></a>Comprender el rendimiento del clúster de AKS con Azure Monitor para contenedores (versión preliminar)
El rendimiento de sus clústeres de Azure Kubernetes Service (AKS) puede observarse desde dos perspectivas con Azure Monitor para contenedores, directamente desde un clúster AKS o ver todos los clústeres de AKS en una suscripción de Azure Monitor. 

En este artículo le ayudamos a comprender la experiencia entre las dos perspectivas y cómo evaluar, investigar y resolver rápidamente los problemas detectados.

Para obtener información acerca de cómo habilitar Azure Monitor para contenedores, consulte el artículo [How to onboard Azure Monitor for containers](monitoring-container-insights-onboard.md) (Cómo incorporar Azure Monitor para contenedores).

Azure Monitor proporciona una vista de varios clústeres que muestra el estado de mantenimiento de todos los clústeres de AKS supervisados implementados en los grupos de recursos de sus suscripciones, y los clústeres de AKS que no supervisa la solución.  De forma inmediata, puede comprender el estado del clúster y desde aquí puede explorar en profundidad la página de rendimiento del controlador y del nodo, o puede navegar para ver los gráficos de rendimiento del clúster.  En el caso de los clústeres de AKS detectados e identificados como no supervisados, puede optar por habilitar la supervisión de ese clúster en cualquier momento.  

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal
Inicie sesión en el [Azure Portal](https://portal.azure.com). 

## <a name="multi-cluster-view-from-azure-monitor"></a>Vista de varios clústeres desde Azure Monitor 
Para ver el estado de mantenimiento de todos los clústeres de AKS implementados, seleccione **Supervisar** en el panel izquierdo de Azure Portal.  En la sección **Conclusiones**, seleccione **Contenedores (versión preliminar)**.  

![Ejemplo del panel de varios clústeres de Azure Monitor](./media/monitoring-container-insights-analyze/azmon-containers-multiview.png)

En la pestaña **Monitored clusters** (Clústeres supervisados), puede aprender lo siguiente:

1. ¿Cuántos clústeres se encuentran en un estado crítico o incorrecto, frente a cuántos están en buen estado o no notifican ninguno (es decir, tienen un estado desconocido)?
2. ¿Cuántos nodos y pods de usuario y del sistema implementa cada clúster?  

Los estados de mantenimiento definidos son los siguientes: 

1. **Correcto** : no se detectó ningún problema para la VM y funciona según lo necesario. 
2. **Crítico**: se detectan uno o varios problemas críticos, que deben solucionarse para restaurar el estado de funcionamiento normal según lo previsto.
3. **Advertencia**: se detectan uno o varios problemas que deben solucionarse o la condición de estado puede volverse crítica.
4. **Desconocido**: si el servicio no pudo establecer ninguna conexión con el nodo o el pod, el estado cambia a un estado desconocido.

El estado de mantenimiento calcula el estado general del clúster como "*el peor*"de los tres estados con una excepción: si cualquiera de los tres estados es *desconocido*, el estado general del clúster se mostrará como **Desconocido**.  

En la tabla siguiente se proporciona un desglose del cálculo que controla los estados de mantenimiento para un clúster supervisado en la vista de varios clústeres.

| |Status |Disponibilidad |  
|-------|-------|-----------------|  
|**Pod de usuario**| | |  
| |Healthy |100% |  
| |Advertencia |90 - 99 % |  
| |Crítico |<90 % |  
| |Desconocido |Si no se notifica en los últimos 30 minutos |  
|**Pod del sistema**| | |  
| |Healthy |100% |
| |Advertencia |N/D |
| |Crítico |<100 % |
| |Desconocido |Si no se notifica en los últimos 30 minutos |
|**Node** | | |
| |Healthy |>85 % |
| |Advertencia |60 - 84 % |
| |Crítico |<60 % |
| |Desconocido |Si no se notifica en los últimos 30 minutos |

En la lista de clústeres, puede explorar en profundidad la página **Clúster** (para ello, haga clic en el nombre del clúster), la página de rendimiento **Nodos** (para ello, haga clic en la acumulación de nodos en la columna **Nodos** de ese clúster específico), o bien la página de rendimiento **Controladores** (para ello, haga clic en la acumulación de la columna **User pods** [Pods de usuario] o **System pods** [Pods del sistema]).   

## <a name="view-performance-directly-from-an-aks-cluster"></a>Vista del rendimiento directamente desde un clúster de AKS
El acceso a Azure Monitor para contenedores está disponible directamente desde un clúster de AKS mediante la selección de la opción **Conclusiones (versión preliminar)** del panel izquierdo. La vista de información sobre el clúster de AKS está organizada en cuatro perspectivas:

- Clúster
- Nodos 
- Controladores  
- Contenedores

La página predeterminada que se abre al hacer clic en **Conclusiones (versión preliminar)** es **Clúster** e incluye cuatro gráficos de líneas de rendimiento que muestran las métricas clave de rendimiento del clúster. 

![Gráficos de rendimiento de ejemplo de la pestaña Clúster](./media/monitoring-container-insights-analyze/containers-cluster-perfview.png)

El gráfico del rendimiento muestra cuatro métricas de rendimiento:

- **Node CPU Utilization&nbsp;%** (Porcentaje de uso de CPU del nodo): una perspectiva agregada del uso de la CPU para todo el clúster. Puede filtrar los resultados para el intervalo de tiempo seleccionando **Avg (Promedio)**, **Min (Mínimo)**, **Max (Máximo)** , **50th (50)**, **90th (90)** y **95th (95)** en el selector de percentiles situado encima del gráfico, ya sea individualmente o de forma combinada. 
- **Node memory utilization&nbsp;%** (Porcentaje de uso de la memoria del nodo): una perspectiva agregada del uso de la memoria para todo el clúster. Puede filtrar los resultados para el intervalo de tiempo seleccionando **Avg (Promedio)**, **Min (Mínimo)**, **Max (Máximo)** , **50th (50)**, **90th (90)** y **95th (95)** en el selector de percentiles situado encima del gráfico, ya sea individualmente o de forma combinada. 
- **Node count** (Número de nodos): número y estado de los nodos de Kubernetes. Los estados de los nodos del clúster representados son *Todo*, *Listo* y  *No está listo*, y se pueden filtrar de forma individual o combinada en el selector situado encima del gráfico. 
- **Activity pod count** (Número de pods de actividad): recuento y el estado de los pods de Kubernetes. Los estados de los pods representados son *Todo*, *Pendientes*, *En ejecución* y *Desconocidos*, y se pueden filtrar de forma individual o combinada en el selector situado encima del gráfico. 

Cuando cambie a las pestañas **Nodos**, **Controladores** y **Contenedores**, se muestran automáticamente en el lado derecho de la página, en el panel de propiedades.  Muestra las propiedades del elemento seleccionado, incluidas las etiquetas que defina para organizar los objetos de Kubernetes. Haga clic en el vínculo **>>** en el panel, para verlo u ocultarlo.  

![Ejemplo de panel de propiedades de las perspectivas de Kubernetes](./media/monitoring-container-insights-analyze/perspectives-preview-pane-01.png)

A medida que expande los objetos en la jerarquía, el panel de propiedades se actualiza según el objeto seleccionado. En el panel también puede ver los eventos de Kubernetes con búsquedas de registros predefinidas si hace clic en el vínculo **View Kubernetes event logs** (Ver los registros de eventos de Kubernetes) en la parte superior del panel. Para más información acerca de cómo ver los datos de registro de Kubernetes, consulte [Búsquedas de registros para analizar datos](#search-logs-to-analyze-data).

Use la opción **+ Agregar filtro** de la parte superior de la página para filtrar los resultados de la vista por **Servicio**, **Nodo** o **Espacio de nombres** y, después de seleccionar el ámbito de filtro, seleccione uno de los valores que se muestran en el campo **Seleccionar valores**.  Una vez se configura el filtro, se aplica globalmente mientras se visualiza cualquier perspectiva del clúster de AKS.  La fórmula solo admite el signo igual.  Puede agregar filtros adicionales que complementen el primero para restringir aún más los resultados.  Por ejemplo, si especifica un filtro por **Nodo**, el segundo filtro solo le permitiría seleccionar **Servicio** o **Espacio de nombres**.  

![Ejemplo de cómo utilizar el filtro para restringir los resultados](./media/monitoring-container-insights-analyze/add-filter-option-01.png)

La especificación de un filtro en una pestaña se sigue aplicando cuando selecciona otro y se elimina después de hacer clic en el símbolo **x** junto al filtro especificado.   

Al cambiar a la pestaña **Nodos**, la jerarquía de filas sigue el modelo de objetos de Kubernetes a partir de un nodo del clúster. Expanda el nodo y podrá ver uno, o varios, pods que se ejecutan en el nodo. Si hay más de un contenedor agrupado en un pod, se muestran como la última fila de la jerarquía. También puede ver cuántas cargas de trabajo no relacionadas con pods se ejecutan en el host si el host tiene presión de memoria o procesador.

![Ejemplo de jerarquía de nodos de Kubernetes en la vista de rendimiento](./media/monitoring-container-insights-analyze/containers-nodes-view.png)

En un nodo expandido, puede explorar en profundidad desde el pod o contenedor que se ejecuta en el nodo hasta el controlador para ver los datos de rendimiento filtrados por ese controlador o pod. Haga clic en el valor de la columna de **controladores** del nodo específico.   

![Ejemplo de exploración en profundidad del nodo al controlador en la vista de rendimiento](./media/monitoring-container-insights-analyze/drill-down-node-controller.png)

Puede seleccionar los controladores o los contenedores en la parte superior de la página y revisar el estado y el uso de recursos de dichos objetos.  Si prefiere revisar el uso de memoria, en la lista desplegable **Metric** (Métrica), seleccione **Memory RSS** (RSS de memoria) o **Memory working set** (Espacio de trabajo de memoria). **RSS de memoria** solo se admite en Kubernetes versión 1.8, y en las versiones posteriores. En caso contrario, verá los valores de **Min&nbsp;%** (Porcentaje mínimo) como *NaN&nbsp;%* (Porcentaje de NaN), que es un valor de tipo de datos numérico que representa un valor no definido o no representable. 

![Vista de rendimiento de los nodos del contenedor](./media/monitoring-container-insights-analyze/containers-node-metric-dropdown.png)

De forma predeterminada, los datos de rendimiento se basan en las últimas seis horas, pero puede cambiar la ventana con la opción **Intervalo de tiempo** de la parte superior izquierda. También puede filtrar los resultados del intervalo de tiempo seleccionando **Avg (Promedio)**, **Min (Mínimo)**, **Max (Máximo)** , **50th (50)**, **90th (90)** y **95th (95)** en el selector de percentiles. 

![Selección de percentil para el filtrado de datos](./media/monitoring-container-insights-analyze/containers-metric-percentile-filter.png)

En el ejemplo siguiente, observe en el nodo *aks-nodepool-14* que el valor **Contenedores** es 5, que es un resumen del número total de contenedores implementados.

![Ejemplo de resumen de contenedores por nodo](./media/monitoring-container-insights-analyze/containers-nodes-containerstotal.png)

Este resumen puede ayudarle a identificar rápidamente si tiene el equilibrio adecuado de contenedores entre los nodos del clúster. 

La información que se presenta al ver los nodos se describe en la siguiente tabla:

| Columna | DESCRIPCIÓN | 
|--------|-------------|
| NOMBRE | El nombre del host. |
| Status | Vista de Kubernetes del estado del nodo. |
| Avg&nbsp;% (Porcentaje medio), Min&nbsp;% (Porcentaje mínimo), Max&nbsp;% (Porcentaje máximo), 50th&nbsp;% (Porcentaje 50) y 90th&nbsp;% (90) | Porcentaje medio de nodos basado en el percentil durante la duración seleccionada. |
| Avg (Promedio), Min (Mínimo), Max (Máximo), 50th (50), 90th (90) | Valor real de promedio de nodos basado en el percentil para la duración seleccionada. El valor medio se mide desde el límite de CPU/memoria establecido para un nodo; en el caso de los pods y contenedores, es el valor medio notificado por el host. |
| Contenedores | Número de contenedores. |
| Tiempo de actividad | Representa la hora desde un nodo iniciado o que se reinició. |
| Controladores | Solo para los contenedores y pods. Muestra el controlador en que reside. No todos los pods están en un controlador, así que algunos pueden mostrar **N/D**. | 
| Trend Avg&nbsp;%, Min&nbsp;%, Max&nbsp;%, 50th&nbsp;%, 90th&nbsp;% (Porcentaje medio, Porcentaje mínimo, Porcentaje máximo, 50 y 90) de tendencia | Tendencia de gráfico de barras que presenta el porcentaje de métricas de percentil del controlador. |

En el selector, elija **Controllers** (Controladores).

![Selección de la vista de controladores](./media/monitoring-container-insights-analyze/containers-controllers-tab.png)

Aquí puede ver el mantenimiento del rendimiento de los controladores.

![Vista de rendimiento de los controladores <nombre>](./media/monitoring-container-insights-analyze/containers-controllers-view.png)

La jerarquía de filas comienza con un controlador y, al expandir un controlador, se ven uno o varios pods.  Expanda un pod y la última fila mostrará el contenedor agrupado en el pod. Desde un controlador expandido, puede explorar en profundidad hasta el nodo en que se ejecuta para ver los datos de rendimiento filtrados por ese nodo. Haga clic en el valor de la columna de **nodos** del nodo específico.   

![Ejemplo de exploración en profundidad del nodo al controlador en la vista de rendimiento](./media/monitoring-container-insights-analyze/drill-down-controller-node.png)

La información que se muestra al ver los controladores se describe en la siguiente tabla:

| Columna | DESCRIPCIÓN | 
|--------|-------------|
| NOMBRE | El nombre del controlador.|
| Status | El estado del resumen de los contenedores cuando ha terminado de ejecutarse con un estado, como *OK (Correcto)*, *Terminated (Finalizado)*, *Failed (Error)* *Stopped (Detenido)* o *Paused (En pausa)*. Si el contenedor se está ejecutando, pero el estado no se mostró correctamente o el agente no lo seleccionó, y no ha respondido durante más de 30 minutos, el estado es *Unknown* (Desconocido). En la tabla siguiente se proporcionan detalles adicionales del icono de estado.|
| Avg&nbsp;% (Porcentaje medio), Min&nbsp;% (Porcentaje mínimo), Max&nbsp;% (Porcentaje máximo), 50th&nbsp;% (Porcentaje 50) y 90th&nbsp;% (90) | Promedio resumen del porcentaje medio de cada entidad para la métrica y el percentil seleccionados. |
| Avg (Promedio), Min (Mínimo), Max (Máximo), 50th (50), 90th (90)  | Resumen del rendimiento medio de memoria o CPU de millares de núcleos del contenedor para el percentil seleccionado. El valor medio se mide a partir del límite de CPU o memoria establecido para un pod. |
| Contenedores | Número total de contenedores para el controlador o pod. |
| Reinicios | Resumen del número de reinicios de los contenedores. |
| Tiempo de actividad | Representa el tiempo desde que se inició un contenedor. |
| Nodo | Solo para los contenedores y pods. Muestra el controlador en el que reside. | 
| Trend Avg&nbsp;%, Min&nbsp;%, Max&nbsp;%, 50th&nbsp;%, 90th&nbsp;% (Porcentaje medio, Porcentaje mínimo, Porcentaje máximo, 50 y 90) de tendencia| Tendencia de gráfico de barras que representa las métricas de percentil del controlador. |

Los iconos del campo de estado indican el estado en línea de los contenedores:
 
| Icono | Status | 
|--------|-------------|
| ![Icono de estado de ejecución listo](./media/monitoring-container-insights-analyze/containers-ready-icon.png) | En ejecución (Listo)|
| ![Icono de estado de espera o en pausa](./media/monitoring-container-insights-analyze/containers-waiting-icon.png) | En pausa o en espera|
| ![Icono de último estado de ejecución notificado](./media/monitoring-container-insights-analyze/containers-grey-icon.png) | Última notificación: en ejecución (pero no ha respondido en más de 30 minutos)|
| ![Icono de estado correcto](./media/monitoring-container-insights-analyze/containers-green-icon.png) | Se detuvo correctamente o no se pudo detener|

El icono de estado muestra un número en función de lo que proporciona el pod. Muestra los dos peores estados y, al pasar el ratón sobre el estado, muestra un estado de resumen de todos los pods del contenedor. Si no hay un estado listo, el valor de estado muestra **(0)**. 

En el selector, elija **Containers** (Contenedores).

![Selección de la vista de contenedores](./media/monitoring-container-insights-analyze/containers-containers-tab.png)

Aquí puede ver el mantenimiento del rendimiento de los contenedores de Azure Kubernetes.  

![Vista de rendimiento de los controladores <nombre>](./media/monitoring-container-insights-analyze/containers-containers-view.png)

En un contenedor, puede explorar en profundidad un pod o un nodo para ver los datos de rendimiento filtrados por ese objeto. Haga clic en el valor de la columna de **pods** o de **nodos** del contenedor específico.   

![Ejemplo de exploración en profundidad del nodo al controlador en la vista de rendimiento](./media/monitoring-container-insights-analyze/drill-down-controller-node.png)

La información que se muestra al ver los contenedores se describe en la siguiente tabla:

| Columna | DESCRIPCIÓN | 
|--------|-------------|
| NOMBRE | El nombre del controlador.|
| Status | Estado de los contenedores, si lo hay. En la tabla siguiente se proporcionan detalles adicionales del icono de estado.|
| Avg&nbsp;% (Porcentaje medio), Min&nbsp;% (Porcentaje mínimo), Max&nbsp;% (Porcentaje máximo), 50th&nbsp;% (Porcentaje 50) y 90th&nbsp;% (90) | El resumen del porcentaje medio de cada entidad para la métrica y el percentil seleccionados. |
| Avg (Promedio), Min (Mínimo), Max (Máximo), 50th (50), 90th (90)  | El resumen del rendimiento medio de memoria o CPU de millares de núcleos del contenedor para el percentil seleccionado. El valor medio se mide a partir del límite de CPU o memoria establecido para un pod. |
| Pod | Contenedor en el que reside el pod.| 
| Nodo |  Nodo donde reside el contenedor. | 
| Reinicios | Representa el tiempo desde que se inició un contenedor. |
| Tiempo de actividad | Representa el tiempo desde que se inicia o reinicia un contenedor. |
| Trend Avg&nbsp;%, Min&nbsp;%, Max&nbsp;%, 50th&nbsp;%, 90th&nbsp;% (Porcentaje medio, Porcentaje mínimo, Porcentaje máximo, 50 y 90) de tendencia | Una tendencia del gráfico de barras que representa el porcentaje medio de métrica del contenedor. |

Los iconos en el campo de estado indican los estados en línea de los pods, tal como se describen en la tabla siguiente:
 
| Icono | Status |  
|--------|-------------|  
| ![Icono de estado de ejecución listo](./media/monitoring-container-insights-analyze/containers-ready-icon.png) | En ejecución (Listo)|  
| ![Icono de estado de espera o en pausa](./media/monitoring-container-insights-analyze/containers-waiting-icon.png) | En pausa o en espera|  
| ![Icono de último estado de ejecución notificado](./media/monitoring-container-insights-analyze/containers-grey-icon.png) | Última notificación: en ejecución, pero no ha respondido en más de 30 minutos|  
| ![Icono de estado finalizado](./media/monitoring-container-insights-analyze/containers-terminated-icon.png) | Se detuvo correctamente o no se pudo detener|  
| ![Icono de estado de error](./media/monitoring-container-insights-analyze/containers-failed-icon.png) | Estado de error |  

## <a name="container-data-collection-details"></a>Información detallada sobre la recopilación de datos del contenedor
La información del contenedor recopila varias métricas de rendimiento y datos de registro de los hosts de contenedor y los contenedores. Los datos se recopilan cada tres minutos.

### <a name="container-records"></a>Registros de contenedor

En la siguiente tabla se muestran ejemplos de registros recopilados por Azure Monitor para contenedores y los tipos de datos que aparecen en los resultados de la búsqueda de registros.

| Tipo de datos | Tipo de datos en la búsqueda de registros | Fields |
| --- | --- | --- |
| Rendimiento de hosts y contenedores | `Perf` | Computer, ObjectName, CounterName &#40;%Processor Time, Disk Reads MB, Disk Writes MB, Memory Usage MB, Network Receive Bytes, Network Send Bytes, Processor Usage sec, Network&#41;, CounterValue, TimeGenerated, CounterPath, SourceSystem |
| Inventario de contenedor | `ContainerInventory` | TimeGenerated, Computer, container name, ContainerHostname, Image, ImageTag, ContainerState, ExitCode, EnvironmentVar, Command, CreatedTime, StartedTime, FinishedTime, SourceSystem, ContainerID, ImageID |
| Inventario de imágenes de contenedor | `ContainerImageInventory` | TimeGenerated, Computer, Image, ImageTag, ImageSize, VirtualSize, Running, Paused, Stopped, Failed, SourceSystem, ImageID, TotalContainer |
| Registro de contenedor | `ContainerLog` | TimeGenerated, Computer, image ID, container name, LogEntrySource, LogEntry, SourceSystem, ContainerID |
| Registro del servicio de contenedores | `ContainerServiceLog`  | TimeGenerated, Computer, TimeOfCommand, Image, Command, SourceSystem, ContainerID |
| Inventario de nodo de contenedor | `ContainerNodeInventory_CL`| TimeGenerated, Computer, ClassName_s, DockerVersion_s, OperatingSystem_s, Volume_s, Network_s, NodeRole_s, OrchestratorType_s, InstanceID_g, SourceSystem|
| Proceso del contenedor | `ContainerProcess_CL` | TimeGenerated, Computer, Pod_s, Namespace_s, ClassName_s, InstanceID_s, Uid_s, PID_s, PPID_s, C_s, STIME_s, Tty_s, TIME_s, Cmd_s, Id_s, Name_s, SourceSystem |
| Inventario de pods en un clúster de Kubernetes | `KubePodInventory` | TimeGenerated, Computer, ClusterId, ContainerCreationTimeStamp, PodUid, PodCreationTimeStamp, ContainerRestartCount, PodRestartCount, PodStartTime, ContainerStartTime, ServiceName, ControllerKind, ControllerName, ContainerStatus, ContainerID, ContainerName, Name, PodLabel, Namespace, PodStatus, ClusterName, PodIp, SourceSystem |
| Inventario de la parte de nodos de un clúster de Kubernetes | `KubeNodeInventory` | TimeGenerated, Computer, ClusterName, ClusterId, LastTransitionTimeReady, Labels, Status, KubeletVersion, KubeProxyVersion, CreationTimeStamp, SourceSystem | 
| Eventos de Kubernetes | `KubeEvents_CL` | TimeGenerated, Computer, ClusterId_s, FirstSeen_t, LastSeen_t, Count_d, ObjectKind_s, Namespace_s, Name_s, Reason_s, Type_s, TimeGenerated_s, SourceComponent_s, ClusterName_s, Message, SourceSystem | 
| Servicios en el clúster de Kubernetes | `KubeServices_CL` | TimeGenerated, ServiceName_s, Namespace_s, SelectorLabels_s, ClusterId_s, ClusterName_s, ClusterIP_s, ServiceType_s, SourceSystem | 
| Métricas de rendimiento de la parte de nodos del clúster de Kubernetes | Perf &#124; donde ObjectName == “K8SNode” | Computer, ObjectName, CounterName &#40;cpuUsageNanoCores, , memoryWorkingSetBytes, memoryRssBytes, networkRxBytes, networkTxBytes, restartTimeEpoch, networkRxBytesPerSec, networkTxBytesPerSec, cpuAllocatableNanoCores, memoryAllocatableBytes, cpuCapacityNanoCores, memoryCapacityBytes&#41;,CounterValue, TimeGenerated, CounterPath, SourceSystem | 
| Métricas de rendimiento de la parte de contenedores del clúster de Kubernetes | Perf &#124; donde ObjectName == “K8SContainer” | CounterName &#40;cpuUsageNanoCores, memoryWorkingSetBytes, memoryRssBytes, restartTimeEpoch, cpuRequestNanoCores, memoryRequestBytes, cpuLimitNanoCores, memoryLimitBytes&#41;,CounterValue, TimeGenerated, CounterPath, SourceSystem | 

## <a name="search-logs-to-analyze-data"></a>Búsqueda de registros para analizar datos
Log Analytics puede ayudarle a buscar tendencias, diagnosticar cuellos de botellas, realizar previsiones o correlacionar datos, que pueden servirle para determinar si la configuración actual del clúster funciona óptimamente. Las búsquedas de registros predefinidas se proporcionan para que comience a usarlas inmediatamente o para que las personalice con el fin de devolver la información de la forma deseada. 

Puede realizar un análisis interactivo de los datos en el área de trabajo mediante la selección de la opción **View Kubernetes event logs** (Ver registros de eventos de Kubernetes) o **View container logs** (Ver registros de contenedor) en el panel de vista previa. La página **Búsqueda de registros** aparece a la derecha de la página de Azure Portal en que se encontraba.

![Análisis de los datos en Log Analytics](./media/monitoring-container-insights-analyze/container-health-log-search-example.png)   

La salida de los registros del contenedor que se reenvía a Log Analytics es STDOUT y STDERR. Dado que Azure Monitor supervisa Kubernetes administrado por Azure (AKS), el sistema de Kubernetes no se recopila hoy debido al gran volumen de datos generados. 

### <a name="example-log-search-queries"></a>Ejemplos de consultas de búsqueda de registros
A menudo resulta útil crear consultas que comiencen con un ejemplo o dos y luego modificarlas para ajustarlas a sus requisitos. Para ayudarle a crear consultas más avanzadas, puede experimentar con las siguientes consultas de ejemplo:

| Consultar | DESCRIPCIÓN | 
|-------|-------------|
| ContainerInventory<br> &#124; project Computer, Name, Image, ImageTag, ContainerState, CreatedTime, StartedTime, FinishedTime<br> &#124; render table | Se muestra toda la información del ciclo de vida de un contenedor| 
| KubeEvents_CL<br> &#124; where not(isempty(Namespace_s))<br> &#124; sort by TimeGenerated desc<br> &#124; render table | Eventos de Kubernetes|
| ContainerImageInventory<br> &#124; summarize AggregatedValue = count() by Image, ImageTag, Running | Inventario de imágenes | 
| **En Análisis avanzado, seleccione gráficos de líneas**:<br> Perf<br> &#124; where ObjectName == "Container" and CounterName == "% Processor Time"<br> &#124; summarize AvgCPUPercent = avg(CounterValue) by bin(TimeGenerated, 30m), InstanceName | CPU de contenedor | 
| **En Análisis avanzado, seleccione gráficos de líneas**:<br> Perf &#124; where ObjectName == "Container" and CounterName == "Memory Usage MB"<br> &#124; summarize AvgUsedMemory = avg(CounterValue) by bin(TimeGenerated, 30m), InstanceName | Memoria de contenedor |
