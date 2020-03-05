---
title: Deshabilitación de la supervisión en Azure Monitor para VM (versión preliminar) | Microsoft Docs
description: En este artículo se describe cómo puede interrumpir la supervisión de las máquinas virtuales en Azure Monitor para VM.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 11/05/2018
ms.openlocfilehash: fb4347e610920380792a17bb620e6d97a7d72505
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/27/2020
ms.locfileid: "77669512"
---
# <a name="disable-monitoring-of-your-vms-in-azure-monitor-for-vms-preview"></a>Deshabilitación de la supervisión de las VM en Azure Monitor para VM (versión preliminar)

Después de habilitar la supervisión de las máquinas virtuales (VM), más adelante puede elegir deshabilitar la supervisión en Azure Monitor para VM. En este artículo se muestra cómo deshabilitar la supervisión de una o más VM.  

Actualmente, Azure Monitor para VM no permite deshabilitar de forma selectiva la supervisión de VM. Puede que el área de trabajo de Log Analytics admita Azure Monitor para VM y otras soluciones. También puede recopilar otros datos de supervisión. Si el área de trabajo de Log Analytics proporciona estos servicios, debe entender el efecto y métodos de la deshabilitación de la supervisión antes de empezar.

Azure Monitor para VM se basa en los siguientes componentes para ofrecer su experiencia:

* Un área de trabajo de Log Analytics, que almacena los datos de supervisión de las máquinas virtuales y otros orígenes.
* Una colección de contadores de rendimiento configurados en el área de trabajo. La colección actualiza la configuración de supervisión en todas las VM conectadas al área de trabajo.
* `InfrastructureInsights` y `ServiceMap`, que son soluciones de supervisión configuradas en el área de trabajo. Estas soluciones actualizan la configuración de supervisión en todas las VM conectadas al área de trabajo.
* `MicrosoftMonitoringAgent` y `DependencyAgent`, que son extensiones de Azure VM. Estas extensiones recopilan y envían datos al área de trabajo.

Cuando se prepare para deshabilitar la supervisión de las VM, tenga en cuenta estas consideraciones:

* Si realizó la evaluación con una sola VM y usó el área de trabajo de Log Analytics predeterminada preseleccionada, puede deshabilitar la supervisión mediante la desinstalación del agente de dependencia de la máquina virtual y la desconexión del agente de Log Analytics de esta área de trabajo. Este enfoque es adecuado si tiene la intención de utilizar la máquina virtual para otros fines y decide posteriormente volver a conectarla a un área de trabajo diferente.
* Si seleccionó un área de trabajo de Log Analytics existente que admite otras soluciones de supervisión y recopilación de datos de otros orígenes, puede quitar los componentes de la solución del área de trabajo sin que el área de trabajo se vea afectada.  

>[!NOTE]
> Después de quitar los componentes de la solución del área de trabajo, puede continuar viendo el estado de mantenimiento de las VM de Azure; específicamente, verá los datos de rendimiento y de mapa cuando vaya a cualquiera de las dos vistas en el portal. Los datos, finalmente, dejarán de aparecer en las vistas **Rendimiento** y **Mapa**. Pero la vista **Estado** seguirá mostrando el estado de mantenimiento para las VM. La opción **Probar ahora** estará disponible en la VM de Azure seleccionada para que pueda volver a habilitar la supervisión en el futuro.  

## <a name="remove-azure-monitor-for-vms-completely"></a>Quitar completamente Azure Monitor para VM

Si aún necesita el área de trabajo de Log Analytics, siga estos pasos para quitar por completo Azure Monitor para VM. Va a quitar las soluciones `InfrastructureInsights` y `ServiceMap` del área de trabajo.  

>[!NOTE]
>Si usó la solución de supervisión de Service Map antes de habilitar Azure Monitor para VM y todavía confía en ella, no la quite como se describe en el último paso del procedimiento siguiente.  
>

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
2. En Azure Portal, seleccione **Todos los servicios**. En la lista de recursos, escriba **Log Analytics**. Cuando comience a escribir, la lista filtrará las sugerencias en función de la entrada. Seleccione **Log Analytics**.
3. En la lista de áreas de trabajo de Log Analytics, seleccione el área de trabajo que eligió al incorporarse a Azure Monitor para VM.
4. A la izquierda, seleccione **Soluciones**.  
5. En la lista de soluciones, seleccione **InfrastructureInsights (nombre de área de trabajo)** . En la página **Introducción** de la solución, seleccione **Eliminar**. Cuando se le solicite confirmación, haga clic en **Sí**.  
6. En la lista de soluciones, seleccione **ServiceMap (nombre de área de trabajo)** . En la página **Introducción** de la solución, seleccione **Eliminar**. Cuando se le solicite confirmación, haga clic en **Sí**.  

Antes de habilitar Azure Monitor para VM, si no [recopilaba contadores de rendimiento](vminsights-enable-overview.md#performance-counters-enabled) para las VM basadas en Windows o Linux en el área de trabajo, [deshabilite esas reglas](../platform/data-sources-performance-counters.md#configuring-performance-counters) para Windows y Linux.

## <a name="disable-monitoring-and-keep-the-workspace"></a>Deshabilitar la supervisión y conservar el área de trabajo  

Si el área de trabajo de Log Analytics aún debe admitir la supervisión de otros orígenes, siga estos pasos para deshabilitar la supervisión en la VM que usó para evaluar Azure Monitor para VM. Para las VM de Azure, quitará la extensión de VM del agente de dependencias y la extensión de VM del agente de Log Analytics para Windows o Linux directamente de la VM. 

>[!NOTE]
>No quite el agente de Log Analytics si: 
>
> * Azure Automation administra la VM para orquestar los procesos o para administrar la configuración o las actualizaciones. 
> * Azure Security Center administra la VM para la seguridad y la detección de amenazas. 
>
> Si quita el agente de Log Analytics, impedirá que esos servicios y soluciones administren de forma proactiva la VM. 

1. Inicie sesión en [Azure Portal](https://portal.azure.com). 
2. En Azure Portal, seleccione **Virtual Machines**. 
3. En la lista, seleccione una máquina virtual. 
4. En el panel izquierdo, seleccione **Extensiones**. En la página **Extensiones**, seleccione **DependencyAgent**.
5. En la página de propiedades de la extensión, seleccione **Desinstalar**.
6. En la página **Extensiones**, seleccione **MicrosoftMonitoringAgent**. En la página de propiedades de la extensión, seleccione **Desinstalar**.  
