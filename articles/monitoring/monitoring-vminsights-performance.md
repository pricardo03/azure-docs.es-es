---
title: Cómo representar el rendimiento en gráficos con Azure Monitor para VM | Microsoft Docs
description: El rendimiento es una característica de Azure Monitor para máquinas virtuales que detecta automáticamente los componentes de la aplicación en sistemas Windows y Linux y asigna la comunicación entre servicios. En este artículo se proporciona información sobre cómo usarla en una variedad de escenarios.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: ''
ms.service: azure-monitor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/17/2018
ms.author: magoedte
ms.openlocfilehash: 06073197254245727cfa41020f060d904a4e50f9
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46957579"
---
# <a name="how-to-chart-performance-with-azure-monitor-for-vms"></a>Cómo representar el rendimiento en gráficos con Azure Monitor para VM
Azure Monitor para VM incluye un conjunto de gráficos de rendimiento que tienen como destino varios indicadores clave de rendimiento (KPI) para ayudarle a determinar el rendimiento de una máquina virtual. Los gráficos muestran el uso de los recursos durante un período de tiempo para que pueda identificar cuellos de botella, anomalías, o cambiar a una perspectiva en la que se muestre cada máquina a fin de ver el uso de los recursos en función de la métrica seleccionada. Aunque hay varios elementos a tener en cuenta cuando se trata del rendimiento, Azure Monitor para máquinas virtuales se centra en el sistema operativo según se manifiesta a través del procesador, la memoria, los adaptadores de red y los discos. El rendimiento complementa la característica de supervisión de mantenimiento y ayuda a exponer los problemas que indican un posible error en el componente del sistema. Además, es compatible con la optimización y los ajustes para lograr la eficiencia, así como con el planeamiento de capacidad.  

## <a name="multi-vm-perspective-from-azure-monitor"></a>Perspectiva de varias VM de Azure Monitor
En Azure monitor, la característica de rendimiento proporciona una vista de todas las VM supervisadas implementadas en grupos de recursos de sus suscripciones o su entorno.  Para obtener acceso desde Azure Monitor, realice lo siguiente. 

1. En Azure Portal, seleccione **Monitor**. 
2. Elija **Máquinas virtuales (versión preliminar)** en la sección **Conclusiones**.
3. Seleccione la pestaña **Rendimiento**.

![Vista de la lista de N principales de rendimiento de conclusiones de la VM](./media/monitoring-vminsights-performance/vminsights-performance-aggview-01.png)

En la pestaña **Top N Charts** (Gráficos de N principales), si tiene más de un área de trabajo de Log Analytics, elija la que esté integrada con la solución en el selector **Área de trabajo** de la parte superior de la página.  En el selector **Grupo**, elija una suscripción, un grupo de recursos o una máquina específica durante un período de tiempo especificado.  De forma predeterminada, los gráficos muestran las últimas 24 horas.  Mediante el selector **Intervalo de tiempo**, puede consultar los intervalos de tiempo históricos de hasta 30 días para mostrar cómo como era el rendimiento en el pasado.   

Los cinco gráficos de uso de la capacidad que se muestran en la página son:

* CPU Utilization % (Porcentaje de uso de la CPU):  muestra las 5 máquinas principales con el mayor uso medio del procesador. 
* Memoria disponible: muestra las 5 máquinas principales con la menor cantidad media de memoria disponible. 
* Logical Disk Space Used % (Porcentaje de espacio usado del disco lógico): muestra las 5 máquinas principales con el mayor porcentaje de espacio en disco medio usado en todos los volúmenes del disco. 
* Bytes Sent Rate (Tasa de bytes enviados): muestra las 5 máquinas principales con la media bytes enviados más alta. 
* Bytes Sent Rate (Tasa de bytes recibidos): muestra las 5 máquinas principales con la media bytes enviados más alta. 

Al hacer clic en la esquina superior derecha de uno de los cinco gráficos, se abrirá la vista **Top N List** (Lista de N principales).  Aquí puede ver el uso de los recursos para esa métrica de rendimiento por cada VM en una vista de lista y la máquina con mayor tendencia.  

![Vista de lista de N principales para una métrica de rendimiento seleccionada](./media/monitoring-vminsights-performance/vminsights-performance-topnlist-01.png)

Al hacer clic en la máquina virtual, el panel **Propiedades** se expande hacia la derecha para mostrar las propiedades del elemento seleccionado, como la información del sistema notificada por el sistema operativo, las propiedades de la VM de Azure, etc. Al hacer clic en una de las opciones de la sección **Vínculos rápidos**, se le redirigirá a esa característica directamente desde la VM seleccionada.  

![Panel Propiedades de la máquina virtual](./media/monitoring-vminsights-performance/vminsights-properties-pane-01.png)

Cambie a la pestaña **Aggregated Charts** (Gráficos agregados) para ver las métricas de rendimiento filtradas por las medidas de los percentiles y las medias.  

![Vista agregada del rendimiento de las conclusiones de la VM](./media/monitoring-vminsights-performance/vminsights-performance-aggview-02.png)

Se proporcionan los siguientes gráficos de uso de la capacidad:

* CPU Utilization % (Porcentaje de uso de la CPU): el valor predeterminado muestra la media y el percentil 95 superior. 
* Memoria disponible: el valor predeterminado muestra la media y los percentiles 5 y 10 superiores. 
* Logical Disk Space Used % (Porcentaje de espacio usado del disco lógico): el valor predeterminado muestra la media y el percentil 95. 
* Bytes Sent Rate (Tasa de bytes enviados): el valor predeterminado muestra la media de bytes enviados. 
* Bytes Sent Rate (Tasa de bytes recibidos): el valor predeterminado muestra la media de bytes recibidos.

También puede cambiar la granularidad de los gráficos en el intervalo de tiempo. Para ello, seleccione **Avg** (Promedio), **Min** (Mínimo), **Max** (Máximo), **50th** (50), **90th** (90) y **95th** (95) en el selector de percentiles.   

Para ver el uso de recursos por máquina virtual individual en una vista de lista y ver qué máquina tiene tendencia a una mayor utilización, seleccione la pestaña **Top N List** (Lista de N principales).  En la página **Top N lista** (Lista de N principales), se muestran las 20 máquinas principales ordenadas por el mayor uso según el percentil 95 para la métrica *CPU Utilization %* (Porcentaje de uso de la CPU).  Para ver más máquinas, seleccione **Cargar más**. De este modo, los resultados se ampliarán y se mostrarán las 500 máquinas principales. 

>[!NOTE]
>La lista no puede mostrar más de 500 máquinas a la vez.  
>

![Ejemplo de la página de lista de N superiores](./media/monitoring-vminsights-performance/vminsights-performance-topnlist-01.png)

Para filtrar los resultados en una máquina virtual específica de la lista, escriba el nombre de su equipo en el cuadro de texto **Buscar por nombre**.  

Si prefiere ver el uso desde otra métrica de rendimiento, en la lista desplegable **Métrica**, seleccione **Memoria disponible**, **Logical Disk Space Used %** (Logical Disk Space Used %), **Network Received Byte/s** (Bytes recibidos por la red) o **Network Sent Byte/s** (Bytes enviados por la red) y la lista se actualizará para mostrar el uso con el ámbito de esa métrica.  

Al seleccionar una máquina virtual de la lista, se abre el panel **Propiedades** del lado derecho de la página y, desde aquí, puede seleccionar **Performance detail** (Detalles de rendimiento).  Se abrirá la página **Virtual Machine Detail** (Detalles de la máquina virtual) con el ámbito en esa máquina virtual, similar a la experiencia de acceso al rendimiento de las conclusiones de la VM directamente desde la VM de Azure.  

## <a name="view-performance-directly-from-an-azure-vm"></a>Ver el rendimiento directamente desde una VM de Azure
Para obtener acceso directamente desde una máquina virtual, realice lo siguiente.

1. En Azure Portal, seleccione **Virtual Machines**. 
2. En la lista, elija una máquina virtual y, en la sección **Monitor**, elija **Conclusiones (versión preliminar)**.  
3. Seleccione la pestaña **Rendimiento**. 

Esta página no solo incluye los gráficos de uso de rendimiento, sino también una tabla para cada disco lógico detectado, su capacidad, la utilización y la media total de cada medida.  

Se proporcionan los siguientes gráficos de uso de la capacidad:

* CPU Utilization % (Porcentaje de uso de la CPU): el valor predeterminado muestra la media y el percentil 95 superior. 
* Memoria disponible: el valor predeterminado muestra la media y los percentiles 5 y 10 superiores. 
* Logical Disk Space Used % (Porcentaje de espacio usado del disco lógico): el valor predeterminado muestra la media y el percentil 95. 
* Logical Disk IOPS (IOPS del disco lógico): el valor predeterminado muestra la media y el percentil 95.
* Logical Disk MB/s (MB/s del disco lógico): el valor predeterminado muestra la media y el percentil 95.
* Max Logical Disk Used % (Porcentaje de uso máximo del disco lógico): el valor predeterminado muestra la media y el percentil 95.
* Bytes Sent Rate (Tasa de bytes enviados): el valor predeterminado muestra la media de bytes enviados. 
* Bytes Sent Rate (Tasa de bytes recibidos): el valor predeterminado muestra la media de bytes recibidos.

![Vista de rendimiento de las conclusiones de la VM directamente desde la VM](./media/monitoring-vminsights-performance/vminsights-performance-directvm-01.png)

## <a name="next-steps"></a>Pasos siguientes
Para obtener información sobre cómo usar la característica de mantenimiento, consulte el artículo sobre cómo [ver el mantenimiento de Azure Monitor para VM](monitoring-vminsights-health.md), o bien, para ver las dependencias de aplicaciones detectadas, consulte el artículo para [ver la asignación de Azure Monitor para VM](monitoring-vminsights-maps.md). 