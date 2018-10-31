---
title: Problemas conocidos de Azure Monitor para VM | Microsoft Docs
description: Azure Monitor para VM es una solución de Azure que combina la supervisión de rendimiento y de mantenimiento del sistema operativo de la VM de Azure, así como la detección automática de las dependencias y los componentes de aplicación con otros recursos, y asigna la comunicación entre ellos. En este artículo se trata los problemas conocidos.
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
ms.date: 10/15/2018
ms.author: magoedte
ms.openlocfilehash: 6d1f1d1ae07ec32262f655fd6ed7205a70e252f4
ms.sourcegitcommit: f20e43e436bfeafd333da75754cd32d405903b07
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2018
ms.locfileid: "49385098"
---
# <a name="known-issues-with-azure-monitor-for-vms"></a>Problemas conocidos con Azure Monitor para VM

Los siguientes son problemas conocidos de la característica de Mantenimiento de Azure Monitor para VM:

- La característica de mantenimiento se ha incorporado a todas las máquinas virtuales conectadas al área de trabajo de Log Analytics, incluso aunque la incorporación se ha iniciado y completado desde una sola máquina virtual.
- Si ya no existe una máquina virtual porque esta se eliminó, seguirá apareciendo en la vista de lista de máquinas virtuales durante 3 a 7 días. Además, si hace clic en el estado de una máquina virtual que se ha eliminado se iniciará la vista **Diagnóstico de mantenimiento** que, a continuación, pasa a un bucle de carga. Si selecciona el nombre de una máquina virtual eliminada, se abre una hoja con un mensaje que indica que se ha eliminado la máquina virtual.
- El período y la frecuencia de los criterios de mantenimiento no se puede modificar con esta versión. 
- No se pueden deshabilitar los criterios de mantenimiento. 
- Después de la incorporación, los datos pueden tardar en mostrarse en Azure Monitor -> Máquinas virtuales-> Mantenimiento o desde la hoja de recursos de VM -> Insights
- El diagnósticos de mantenimiento experimenta actualizaciones más rápido que cualquier otra vista, por lo que puede experimentar retrasos al cambiar entre vistas.  
- El resumen de alertas que se proporciona en Azure Monitor para el mantenimiento de VM corresponde solo a alertas activadas para problemas de mantenimiento detectados con VM de Azure supervisadas.
- La página de visa **Lista de alertas** en la VM propia y en Azure Monitor muestra alertas cuya condición de supervisión está establecida en "activada" en los últimos 30 días.  No pueden configurarse. Sin embargo, después de hacer clic en el resumen, una vez que se inicia la página de vista **Lista de alertas**, puede cambiar el valor de filtro tanto de la condición de supervisión como del intervalo de tiempo.
- En la página de vista **Lista de alertas**, se recomienda no modificar los filtros **Tipo de recurso**, **Recursos** y **Supervisar servicio**, ya que se han configurado de manera específica para la solución (esta vista de lista muestra algunos campos adicionales en comparación con la vista de lista Azure Monitor -> Alertas).    
- En VM Linux, la vista **Diagnósticos de estado** tiene el nombre de dominio completo de la VM, en lugar del nombre de VM definido por el usuario.
- Apagar las VM actualizará algunos de los criterios de mantenimiento a un estado crítico y otros a un estado correcto con estado neto de la VM en un estado crítico.
- No se puede modificar la gravedad de la alerta de mantenimiento, solo se habilitar o deshabilitar.  Además, algunas gravedades se actualizan según el estado de los criterios de mantenimiento.
- Modificar cualquier configuración de una instancia de criterio de mantenimiento dará lugar a una modificación de la misma configuración en todas las instancias de criterio de mantenimiento del mismo tipo en la VM. Por ejemplo, si el umbral de la instancia de criterio de mantenimiento del espacio libre en el disco correspondiente al disco lógico C: se modifica, este umbral se aplicará a todos los demás discos lógicos detectados y supervisados para la misma VM.   
- Los umbrales de los siguientes criterios de mantenimiento que tienen como destino una máquina virtual Windows no son modificables, puesto que su estado está establecido como **en ejecución** o **disponible**. Cuando se consulta desde [Workload Monitor API](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/workloadmonitor/resource-manager), el estado de mantenimiento muestra el valor *comparisonOperator* de **LessThan** o **GreaterThan** con un valor de *umbral* de **4** para el servicio o la entidad si:
   - Service Health del cliente DNS: El servicio no se está ejecutando 
   - Service Health del cliente DHCP: El servicio no se está ejecutando 
   - Service Health de RPC: El servicio no se está ejecutando 
   - Service Health del firewall de Windows: El servicio no se está ejecutando
   - Service Health del registro de eventos de Windows: El servicio no se está ejecutando 
   - Service Health del servidor: El servicio no se está ejecutando 
   - Service Health del servicio de administración remota de Windows: El servicio no se está ejecutando 
   - Error o daño del sistema de archivos: el disco lógico no está disponible

- Los umbrales de los siguientes criterios de estado de Linux no son modificables, ya que su estado de mantenimiento ya está establecido en **true**.  El estado de mantenimiento muestra *comparisonOperator* con un valor **LessThan** y un valor de *umbral* de **1** cuando Workload Monitoring API realiza una consulta sobre la entidad en función de su contexto:
   - Estado del disco lógico: el disco lógico no está en línea ni está disponible
   - Estado del disco: el disco no está en línea ni está disponible
   - Estado del adaptador de red: el adaptador de red está deshabilitado  

- El criterio de mantenimiento **Uso total de CPU** de Windows muestra un umbral **no igual a 4** en el portal cuando Workload Monitoring API realiza una consulta sobre este y el uso de la CPU es superior al 95 % y la cola del sistema es superior a 15. No se puede modificar este criterio de mantenimiento en esta versión.  
- Los cambios de configuración como, por ejemplo, la actualización de un umbral, tardan unos 30 minutos en surtir efecto incluso aunque el portal o Workload Monitor API se actualicen inmediatamente.  
- Los criterios de mantenimiento en el nivel de procesador individual o procesador lógico no están disponibles en Windows. Solo está disponible el de **Uso total de CPU** para las máquinas virtuales Windows.  
- Las reglas de alertas definidas para cada criterio de estado no están expuestas en Azure Portal. Solo se pueden configurar desde [Workload Monitor API](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/workloadmonitor/resource-manager) para habilitar o deshabilitar una regla de alertas de mantenimiento.  
- Asignar un [grupo de acciones de Azure Monitor](../monitoring-and-diagnostics/monitoring-action-groups.md) para las alertas de estado no es posible desde Azure Portal. Tendrá que usar la API de configuración de notificaciones para configurar un grupo de acciones que se desencadene cada vez que se desencadena una alerta de estado. Actualmente, se pueden asignar grupos de acciones en una máquina virtual, de tal forma que todas las *alertas de estado* desencadenadas en la máquina virtual desencadenen los mismos grupos de acciones. No existe el concepto de un grupo de acciones independiente para cada regla de alertas de mantenimiento, como sucede con las alertas de Azure tradicionales. Además, se admiten solo grupos de acciones configurados para que envíen notificaciones mediante un correo electrónico o SMS cuando se desencadenan alertas de estado. 

## <a name="next-steps"></a>Pasos siguientes
Consulte [Incorporación de Azure Monitor para VM](monitoring-vminsights-onboard.md) para conocer los requisitos y los métodos necesarios para habilitar la supervisión de máquinas virtuales.