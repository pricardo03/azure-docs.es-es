---
title: Supervisar el rendimiento del clúster de AKS con Azure Monitor para contenedores | Microsoft Docs
description: En este artículo se describe cómo puede ver y analizar los datos de registro y rendimiento con Azure Monitor para contenedores.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/17/2019
ms.author: magoedte
ms.openlocfilehash: 8fb1d0083796671119de2b4d7feefe738b602fe2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60497375"
---
# <a name="understand-aks-cluster-performance-with-azure-monitor-for-containers"></a>Comprender el rendimiento del clúster de AKS con Azure Monitor para contenedores 
Con Azure Monitor para contenedores, puede usar los gráficos de rendimiento y el estado de mantenimiento para supervisar la carga de trabajo de los clústeres de Azure Kubernetes Service (AKS) desde dos perspectivas: directamente desde un clúster de AKS o en todos los clústeres de AKS de una suscripción desde Azure Monitor. La visualización de Azure Container Instances (ACI) también es posible cuando se supervisa un clúster de AKS concreto.

En este artículo le ayudamos a comprender la experiencia entre las dos perspectivas y el modo de evaluar, investigar y resolver rápidamente los problemas detectados.

Para obtener información acerca de cómo habilitar Azure Monitor para contenedores, consulte el artículo [How to onboard Azure Monitor for containers](container-insights-onboard.md) (Cómo incorporar Azure Monitor para contenedores).

Azure Monitor proporciona una vista de varios clústeres que muestra el estado de mantenimiento de todos los clústeres de AKS supervisados e implementados en los grupos de recursos de sus suscripciones.  Muestra los clústeres de AKS detectados que la solución no supervisa. De forma inmediata, puede comprender el estado del clúster y desde aquí puede explorar en profundidad la página de rendimiento del controlador y del nodo, o explorar para ver los gráficos de rendimiento del clúster.  En el caso de los clústeres de AKS detectados e identificados como no supervisados, puede optar por habilitar la supervisión de ese clúster en cualquier momento.  

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal
Inicie sesión en el [Azure Portal](https://portal.azure.com). 

## <a name="multi-cluster-view-from-azure-monitor"></a>Vista de varios clústeres desde Azure Monitor 
Para ver el estado de mantenimiento de todos los clústeres de AKS implementados, seleccione **Supervisar** en el panel izquierdo de Azure Portal.  En la sección **Insights**  (Conclusiones), seleccione **Contenedores**.  

![Ejemplo del panel de varios clústeres de Azure Monitor](./media/container-insights-analyze/azmon-containers-multiview-1018.png)

En la pestaña **Monitored clusters** (Clústeres supervisados), puede aprender lo siguiente:

1. ¿Cuántos clústeres se encuentran en un estado crítico o incorrecto, frente a cuántos están en buen estado o no notifican ninguno (es decir, tienen un estado desconocido)?
2. ¿Están todas mis implementaciones de [Azure Kubernetes Engine (motor de AKS)](https://github.com/Azure/aks-engine)en buen estado?
3. ¿Cuántos nodos, usuario y los pods de sistema se implementan por clúster?
4. ¿Cuánto espacio en disco disponible y existe un problema de capacidad?

Los estados de mantenimiento incluidos son: 

* **Correcto** : no se detectó ningún problema para la VM y funciona según lo necesario. 
* **Crítico**: se detectan uno o varios problemas críticos, que deben solucionarse para restaurar el estado de funcionamiento normal según lo previsto.
* **Advertencia**: se detectan uno o varios problemas que deben solucionarse o la condición de estado puede volverse crítica.
* **Desconocido**: si el servicio no pudo establecer ninguna conexión con el nodo o el pod, el estado cambia a un estado desconocido.
* **No se encuentra**: ya sea el área de trabajo, el grupo de recursos o la suscripción que contiene el área de trabajo para esta solución, han sido eliminados.
* **No autorizado**: el usuario no tiene los permisos necesarios para leer los datos del área de trabajo.
* **Error**: al intentar leer los datos del área de trabajo.
* **Mis configurados**: Azure Monitor para contenedores no se configuró correctamente en el área de trabajo especificada.
* **No hay datos**: no se han notificado los datos en el área de trabajo en los últimos 30 minutos.

Estado de mantenimiento calcula el estado general del clúster como *lo peor* los tres estados con una excepción: si cualquiera de los tres estados es *desconocido*, mostrará el estado general del clúster **desconocido**.  

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

En la lista de clústeres, puede explorar en profundidad la página **Clúster** (para ello, haga clic en el nombre del clúster), la página de rendimiento **Nodos** (para ello, haga clic en la acumulación de nodos en la columna **Nodos** de ese clúster específico) o bien la página de rendimiento **Controladores** (para ello, haga clic en la acumulación de la columna **User pods** [Pods de usuario] o **System pods** [Pods del sistema]).   

## <a name="view-performance-directly-from-an-aks-cluster"></a>Vista del rendimiento directamente desde un clúster de AKS
El acceso a Azure Monitor para contenedores está disponible directamente desde un clúster de AKS mediante la selección de la opción **Insights** (Conclusiones) del panel izquierdo. La vista de información sobre el clúster de AKS está organizada en cuatro perspectivas:

- Clúster
- Nodos 
- Controladores  
- Contenedores

La página predeterminada que se abre al hacer clic en **Insights** (Conclusiones) es **Clúster** e incluye cuatro gráficos de líneas de rendimiento que muestran las métricas clave de rendimiento del clúster. 

![Gráficos de rendimiento de ejemplo de la pestaña Clúster](./media/container-insights-analyze/containers-cluster-perfview.png)

El gráfico del rendimiento muestra cuatro métricas de rendimiento:

- **Node CPU Utilization&nbsp;%** (Uso de la CPU del nodo): Una perspectiva agregada del uso de la CPU para todo el clúster. Puede filtrar los resultados para el intervalo de tiempo seleccionando **Avg (Promedio)**, **Min (Mínimo)**, **Max (Máximo)** , **50th (50)**, **90th (90)** y **95th (95)** en el selector de percentiles situado encima del gráfico, ya sea individualmente o de forma combinada. 
- **Node memory utilization&nbsp;%** (Utilización de memoria del nodo): Una perspectiva agregada del uso de la memoria para todo el clúster. Puede filtrar los resultados para el intervalo de tiempo seleccionando **Avg (Promedio)**, **Min (Mínimo)**, **Max (Máximo)** , **50th (50)**, **90th (90)** y **95th (95)** en el selector de percentiles situado encima del gráfico, ya sea individualmente o de forma combinada. 
- **Node count** (Número de nodos): Número y estado de los nodos de Kubernetes. Los estados de los nodos del clúster representados son *Todo*, *Listo* y  *No está listo*, y se pueden filtrar de forma individual o combinada en el selector situado encima del gráfico. 
- **Activity pod count** (Número de pods de actividad): Número y estado de los pods de Kubernetes. Los estados de los pods representados son *Todo*, *Pendientes*, *En ejecución* y *Desconocidos*, y se pueden filtrar de forma individual o combinada en el selector situado encima del gráfico. 

Puede usar las teclas de dirección izquierda y derecha para desplazarse por cada punto de datos en el gráfico y las teclas de dirección arriba y abajo para desplazarse por las líneas de percentil.

Azure Monitor para los contenedores también es compatible con Azure Monitor [Explorador de métricas](../platform/metrics-getting-started.md), donde puede crear sus propios gráficos de trazado, poner en correlación e investigar las tendencias y anclar en paneles. Desde el Explorador de métricas, también puede usar los criterios que ha establecido para visualizar las métricas como la base de un [métrica según la regla de alerta](../platform/alerts-metric.md).  

## <a name="view-container-metrics-in-metrics-explorer"></a>Visualización de las métricas de contenedor en el Explorador de métricas
En el Explorador de métricas, puede ver el nodo agregado y pod métricas de utilización de Azure Monitor para contenedores. En la tabla siguiente se resume los detalles que le ayudarán a aprender a usar los gráficos de métricas para visualizar las métricas de contenedor.

|Espacio de nombres | Métrica |
|----------|--------|
| insights.container/nodes | |
| | cpuUsageMillicores |
| | cpuUsagePercentage |
| | memoryRssBytes |
| | memoryRssPercentage |
| | memoryWorkingSetBytes |
| | memoryWorkingSetPercentage |
| | nodesCount |
| insights.container/pods | |
| | PodCount |

Puede aplicar [dividir](../platform/metrics-charts.md#apply-splitting-to-a-chart) de una métrica para verlo por dimensión y visualizar diferentes segmentos de la comparar entre sí. Para un nodo, también puede segmentar el gráfico por la *host* dimensión, y desde un pod se puede segmentar por las dimensiones siguientes:

* Controller
* Kubernetes namespace
* Nodo
* Fase

## <a name="analyze-nodes-controllers-and-container-health"></a>Analizar los nodos, los controladores y mantenimiento de contenedores

Cuando cambie a las pestañas **Nodos**, **Controladores** y **Contenedores**, se muestran automáticamente en el lado derecho de la página, en el panel de propiedades.  Muestra las propiedades de los elementos seleccionados, incluidas las etiquetas definen para organizar objetos de Kubernetes. Haga clic en el vínculo **>>** en el panel, para verlo u ocultarlo.  

![Ejemplo de panel de propiedades de las perspectivas de Kubernetes](./media/container-insights-analyze/perspectives-preview-pane-01.png)

A medida que expande los objetos en la jerarquía, el panel de propiedades se actualiza según el objeto seleccionado. En el panel, también puede ver los eventos de Kubernetes con búsquedas de registros predefinidas si hace clic en el vínculo **View Kubernetes event logs** (Ver los registros de eventos de Kubernetes) en la parte superior del panel. Para más información acerca de cómo ver los datos de registro de Kubernetes, consulte [Búsquedas de registros para analizar datos](container-insights-log-search.md). Al revisar los contenedores en la vista **Contenedores**, puede ver los registros de contenedores en tiempo real. Para más información sobre esta característica y la configuración necesaria para conceder y controlar el acceso, consulte [Vista de los registros de contenedor en tiempo real con Azure Monitor para contenedores (versión preliminar)](container-insights-live-logs.md). 

Use la **+ Agregar filtro** opción desde la parte superior de la página para filtrar los resultados de la vista por **servicio**, **nodo**, **Namespace**, o  **Grupo de nodos** y después de seleccionar el ámbito de filtro, a continuación, seleccione uno de los valores mostrados en la **seleccionar valores** campo.  Una vez se configura el filtro, se aplica globalmente mientras se visualiza cualquier perspectiva del clúster de AKS.  La fórmula solo admite el signo igual.  Puede agregar filtros adicionales que complementen el primero para restringir aún más los resultados.  Por ejemplo, si especifica un filtro por **Nodo**, el segundo filtro solo le permitiría seleccionar **Servicio** o **Espacio de nombres**.  

![Ejemplo de cómo utilizar el filtro para restringir los resultados](./media/container-insights-analyze/add-filter-option-01.png)

La especificación de un filtro en una pestaña se sigue aplicando cuando selecciona otro y se elimina después de hacer clic en el símbolo **x** junto al filtro especificado.   

Al cambiar a la pestaña **Nodos**, la jerarquía de filas sigue el modelo de objetos de Kubernetes a partir de un nodo del clúster. Expanda el nodo y podrá ver uno, o varios, pods que se ejecutan en el nodo. Si hay más de un contenedor agrupado en un pod, se muestran como la última fila de la jerarquía. También puede ver cuántas cargas de trabajo no relacionadas con pods se ejecutan en el host si el host tiene presión de memoria o procesador.

![Ejemplo de jerarquía de nodos de Kubernetes en la vista de rendimiento](./media/container-insights-analyze/containers-nodes-view.png)

Los nodos virtuales de Azure Container Instances que se ejecutan en el sistema operativo Linux se muestran tras el último nodo del clúster de AKS en la lista.  Al expandir un nodo virtual de ACI, puede ver uno o varios pods y contenedores de ACI que se ejecutan en el nodo.  No se recopilan ni notifican métricas para los nodos, solo para los pods.

![Ejemplo de jerarquía de nodos con Azure Container Instances enumerados](./media/container-insights-analyze/nodes-view-aci.png)

En un nodo expandido, puede explorar en profundidad desde el pod o contenedor que se ejecuta en el nodo hasta el controlador para ver los datos de rendimiento filtrados por ese controlador. Haga clic en el valor de la columna de **controladores** del nodo específico.   
![Ejemplo de exploración en profundidad del nodo al controlador en la vista de rendimiento](./media/container-insights-analyze/drill-down-node-controller.png)

Puede seleccionar los controladores o los contenedores en la parte superior de la página y revisar el estado y el uso de recursos de dichos objetos.  Si prefiere revisar el uso de memoria, en la lista desplegable **Metric** (Métrica), seleccione **Memory RSS** (RSS de memoria) o **Memory working set** (Espacio de trabajo de memoria). **RSS de memoria** solo se admite en Kubernetes versión 1.8, y en las versiones posteriores. En caso contrario, verá los valores de **Min&nbsp;%** (Porcentaje mínimo) como *NaN&nbsp;%* (Porcentaje de NaN), que es un valor de tipo de datos numérico que representa un valor no definido o no representable. 

![Vista de rendimiento de los nodos del contenedor](./media/container-insights-analyze/containers-node-metric-dropdown.png)

De forma predeterminada, los datos de rendimiento se basan en las últimas seis horas, pero puede cambiar la ventana con la opción **Intervalo de tiempo** de la parte superior izquierda. También puede filtrar los resultados del intervalo de tiempo seleccionando **Avg (Promedio)**, **Min (Mínimo)**, **Max (Máximo)** , **50th (50)**, **90th (90)** y **95th (95)** en el selector de percentiles. 

![Selección de percentil para el filtrado de datos](./media/container-insights-analyze/containers-metric-percentile-filter.png)

Cuando mueve el mouse sobre el gráfico de barras en la columna **Tendencia**, cada barra muestra el uso de CPU o de memoria, según la métrica seleccionada, dentro de un período de muestreo de 15 minutos. Después de seleccionar el gráfico de tendencias mediante un teclado, puede usar las teclas Alt + RePág o Alt + AvPág para desplazarse por cada barra individualmente y obtener los mismos detalles que obtendría con un evento mouseover.

![Gráfico de barras al mantener el mouse de tendencia a través de ejemplo](./media/container-insights-analyze/containers-metric-trend-bar-01.png)    

En el ejemplo siguiente, observe en el primer nodo de la lista en *aks-nodepool-1* que el valor de **Contenedores** es 9, que es un resumen del número total de contenedores implementados.

![Ejemplo de acumulación de contenedores por nodo](./media/container-insights-analyze/containers-nodes-containerstotal.png)

Este resumen puede ayudarle a identificar rápidamente si tiene el equilibrio adecuado de contenedores entre los nodos del clúster. 

La información que se presenta al ver los nodos se describe en la siguiente tabla:

| Columna | DESCRIPCIÓN | 
|--------|-------------|
| Name | El nombre del host. |
| Status | Vista de Kubernetes del estado del nodo. |
| Avg&nbsp;% (Porcentaje medio), Min&nbsp;% (Porcentaje mínimo), Max&nbsp;% (Porcentaje máximo), 50th&nbsp;% (Porcentaje 50) y 90th&nbsp;% (90) | Porcentaje medio de nodos basado en el percentil durante la duración seleccionada. |
| Avg (Promedio), Min (Mínimo), Max (Máximo), 50th (50), 90th (90) | Valor real de promedio de nodos basado en el percentil para la duración seleccionada. El valor medio se mide desde el límite de CPU/memoria establecido para un nodo; en el caso de los pods y contenedores, es el valor medio notificado por el host. |
| Contenedores | Número de contenedores. |
| Tiempo de actividad | Representa la hora desde un nodo iniciado o que se reinició. |
| Controladores | Solo para los contenedores y pods. Muestra el controlador en que reside. No todos los pods están en un controlador, así que algunos pueden mostrar **N/D**. | 
| Trend Avg&nbsp;%, Min&nbsp;%, Max&nbsp;%, 50th&nbsp;%, 90th&nbsp;% (Porcentaje medio, Porcentaje mínimo, Porcentaje máximo, 50 y 90) de tendencia | Tendencia de gráfico de barras que presenta el porcentaje de métricas de percentil promedio del controlador. |

En el selector, elija **Controllers** (Controladores).

![Selección de la vista de controladores](./media/container-insights-analyze/containers-controllers-tab.png)

Aquí puede ver el estado de rendimiento de los controladores y de los controladores del nodo virtual de ACI o los pods de nodo virtual no conectados a un controlador.

![Vista de rendimiento de los controladores <nombre>](./media/container-insights-analyze/containers-controllers-view.png)

La jerarquía de filas comienza con un controlador y, al expandir un controlador, se ven uno o varios pods.  Expanda un pod y la última fila mostrará el contenedor agrupado en el pod. Desde un controlador expandido, puede explorar en profundidad hasta el nodo en que se ejecuta para ver los datos de rendimiento filtrados por ese nodo. Los pods de ACI no conectados a un controlador se enumeran los últimos en la lista.

![Ejemplo de jerarquía de controladores con pods de Azure Container Instances enumerados](./media/container-insights-analyze/controllers-view-aci.png)

Haga clic en el valor de la columna de **nodos** del nodo específico.   

![Ejemplo de exploración en profundidad del nodo al controlador en la vista de rendimiento](./media/container-insights-analyze/drill-down-controller-node.png)

La información que se muestra al ver los controladores se describe en la siguiente tabla:

| Columna | DESCRIPCIÓN | 
|--------|-------------|
| Name | El nombre del controlador.|
| Status | Estado de la acumulación de los contenedores cuando ha terminado de ejecutarse con un estado, como *OK (Correcto)*, *Terminated (Finalizado)*, *Failed (Error)* *Stopped (Detenido)* o *Paused (En pausa)*. Si el contenedor se está ejecutando, pero el estado no se mostró correctamente o el agente no lo seleccionó, y no ha respondido durante más de 30 minutos, el estado es *Unknown* (Desconocido). En la tabla siguiente se proporcionan detalles adicionales del icono de estado.|
| Avg&nbsp;% (Porcentaje medio), Min&nbsp;% (Porcentaje mínimo), Max&nbsp;% (Porcentaje máximo), 50th&nbsp;% (Porcentaje 50) y 90th&nbsp;% (90) | Resumen medio del porcentaje promedio de cada entidad para la métrica seleccionada y percentil. |
| Avg (Promedio), Min (Mínimo), Max (Máximo), 50th (50), 90th (90)  | Acumulación del rendimiento medio de memoria o CPU de millares de núcleos del contenedor para el percentil seleccionado. El valor medio se mide a partir del límite de CPU o memoria establecido para un pod. |
| Contenedores | Número total de contenedores para el controlador o pod. |
| Reinicios | Resumen del número de reinicios desde los contenedores. |
| Tiempo de actividad | Representa el tiempo desde que se inició un contenedor. |
| Nodo | Solo para los contenedores y pods. Muestra el controlador en el que reside. | 
| Trend Avg&nbsp;%, Min&nbsp;%, Max&nbsp;%, 50th&nbsp;%, 90th&nbsp;% (Porcentaje medio, Porcentaje mínimo, Porcentaje máximo, 50 y 90) de tendencia| Tendencia de gráfico de barras que representa las métricas de percentil promedio del controlador. |

Los iconos del campo de estado indican el estado en línea de los contenedores:
 
| Icono | Status | 
|--------|-------------|
| ![Icono de estado de ejecución listo](./media/container-insights-analyze/containers-ready-icon.png) | En ejecución (Listo)|
| ![Icono de estado de espera o en pausa](./media/container-insights-analyze/containers-waiting-icon.png) | En pausa o en espera|
| ![Icono de último estado de ejecución notificado](./media/container-insights-analyze/containers-grey-icon.png) | Última notificación: en ejecución (pero no ha respondido en más de 30 minutos)|
| ![Icono de estado correcto](./media/container-insights-analyze/containers-green-icon.png) | Se detuvo correctamente o no se pudo detener|

El icono de estado muestra un número en función de lo que proporciona el pod. Muestra los dos peores estados y, al pasar el ratón sobre el estado, muestra un estado de resumen de todos los pods del contenedor. Si no hay un estado listo, el valor de estado muestra **(0)**. 

En el selector, elija **Containers** (Contenedores).

![Selección de la vista de contenedores](./media/container-insights-analyze/containers-containers-tab.png)

Aquí puede ver el estado del rendimiento de los contenedores de Azure Container Instances y Azure Kubernetes.  

![Vista de rendimiento de los controladores <nombre>](./media/container-insights-analyze/containers-containers-view.png)

En un contenedor, puede explorar en profundidad un pod o un nodo para ver los datos de rendimiento filtrados por ese objeto. Haga clic en el valor de la columna de **pods** o de **nodos** del contenedor específico.   

![Ejemplo de exploración en profundidad del nodo al controlador en la vista de rendimiento](./media/container-insights-analyze/drill-down-controller-node.png)

La información que se muestra al ver los contenedores se describe en la siguiente tabla:

| Columna | DESCRIPCIÓN | 
|--------|-------------|
| Name | El nombre del controlador.|
| Status | Estado de los contenedores, si lo hay. En la tabla siguiente se proporcionan detalles adicionales del icono de estado.|
| Avg&nbsp;% (Porcentaje medio), Min&nbsp;% (Porcentaje mínimo), Max&nbsp;% (Porcentaje máximo), 50th&nbsp;% (Porcentaje 50) y 90th&nbsp;% (90) | El resumen del porcentaje medio de cada entidad para la métrica y el percentil seleccionados. |
| Avg (Promedio), Min (Mínimo), Max (Máximo), 50th (50), 90th (90)  | Resumen del rendimiento medio de memoria o CPU de millares de núcleos del contenedor para el percentil seleccionado. El valor medio se mide a partir del límite de CPU o memoria establecido para un pod. |
| Pod | Contenedor en el que reside el pod.| 
| Nodo |  Nodo donde reside el contenedor. | 
| Reinicios | Representa el tiempo desde que se inició un contenedor. |
| Tiempo de actividad | Representa el tiempo desde que se inicia o reinicia un contenedor. |
| Trend Avg&nbsp;%, Min&nbsp;%, Max&nbsp;%, 50th&nbsp;%, 90th&nbsp;% (Porcentaje medio, Porcentaje mínimo, Porcentaje máximo, 50 y 90) de tendencia | Tendencia de gráfico de barras que representa el porcentaje de métricas de percentil promedio del contenedor. |

Los iconos en el campo de estado indican los estados en línea de los pods, tal como se describen en la tabla siguiente:
 
| Icono | Status |  
|--------|-------------|  
| ![Icono de estado de ejecución listo](./media/container-insights-analyze/containers-ready-icon.png) | En ejecución (Listo)|  
| ![Icono de estado de espera o en pausa](./media/container-insights-analyze/containers-waiting-icon.png) | En pausa o en espera|  
| ![Icono de último estado de ejecución notificado](./media/container-insights-analyze/containers-grey-icon.png) | Última notificación: en ejecución, pero no ha respondido en más de 30 minutos|  
| ![Icono de estado finalizado](./media/container-insights-analyze/containers-terminated-icon.png) | Se detuvo correctamente o no se pudo detener|  
| ![Icono de estado de error](./media/container-insights-analyze/containers-failed-icon.png) | Estado de error |  

## <a name="next-steps"></a>Pasos siguientes
- Revise el [crear alertas de rendimiento con Azure Monitor para contenedores](container-insights-alerts.md) para aprender a crear alertas de uso elevado de CPU y memoria admitir su DevOps o procedimientos y procesos operativos. 
- Vista [ejemplos de consultas de registro](container-insights-log-search.md#search-logs-to-analyze-data) para ver consultas predefinidas y ejemplos para evaluar o personalizar para las alertas, visualizar o analizar los clústeres.
