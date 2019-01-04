---
title: Problemas conocidos de Azure Monitor para VM (versión preliminar) | Microsoft Docs
description: En este articulo se tratan problemas conocidos de Azure Monitor para VM, una solución que combina la supervisión del estado y el rendimiento del sistema operativo de máquina virtual de Azure. Azure Monitor para VM detecta automáticamente los componentes de la aplicación y las dependencias con otros recursos, y asigna la comunicación entre ellos.
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
ms.date: 11/07/2018
ms.author: magoedte
ms.openlocfilehash: b2a60ddee58a7a9c37582085132ee8a1df767cb4
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2018
ms.locfileid: "53190363"
---
# <a name="known-issues-with-azure-monitor-for-vms-preview"></a>Problemas conocidos con Azure Monitor para VM (versión preliminar)

En este articulo se tratan problemas conocidos de Azure Monitor para VM, una solución que combina la supervisión del estado y el rendimiento del sistema operativo de máquina virtual de Azure. 

Los siguientes son problemas conocidos de la característica de Mantenimiento:

- La característica de mantenimiento está habilitada para todas las máquinas virtuales que están conectadas al área de trabajo de Log Analytics. Esto es así incluso cuando la acción comienza y termina en una sola máquina virtual.
- Después de deshabilitar la supervisión de una máquina virtual mediante los métodos admitidos e intentar implementarla de nuevo, debería implementarla en la misma área de trabajo. Si usa una nueva área de trabajo e intenta ver el estado de mantenimiento para esa máquina virtual, es posible que muestre comportamientos anómalos.
- Si se quita o se elimina una máquina virtual de Azure, se muestra en la vista de lista de la máquina virtual de tres a siete días. Además, si hace clic en el estado de una máquina virtual que se ha eliminado, se abre la vista **Diagnóstico de mantenimiento** y, a continuación, pasa a un bucle de carga. Si selecciona el nombre de una máquina virtual eliminada, se abre un panel con un mensaje que indica que la máquina virtual se ha eliminado.
- El período y la frecuencia de los criterios de mantenimiento no se pueden modificar con esta versión. 
- No se pueden deshabilitar los criterios de mantenimiento. 
- Después de la implementación, puede tardarse un tiempo en que se muestren los datos en el panel **Azure Monitor** > **Máquinas virtuales** > **Mantenimiento** o el panel **Recurso de máquina virtual** > **Insights**.
- El Diagnóstico de mantenimiento experimenta las actualizaciones más rápido que cualquier otra vista. La información podría retrasarse al cambiar entre vistas. 
- El resumen de alertas que se incluye con Azure Monitor para la máquina virtual muestra solo las alertas que derivan de los problemas de mantenimiento que se detectan con las máquinas virtuales de Azure supervisadas.
- El panel **Lista de alertas** en la única máquina virtual y Azure Monitor muestra alertas cuya condición de supervisión está establecida en *activada* en los últimos 30 días. Las alertas no son configurables. Sin embargo, cuando se abre el panel **Lista de alertas**, puede hacer clic en el resumen para cambiar el valor de filtro tanto de la condición de supervisión como del intervalo de tiempo.
- En el panel **Lista de alertas**, se recomienda no modificar los filtros **Tipo de recurso**, **Recursos** y **Supervisar servicio**. Se han configurado de forma específica para la solución. Esta vista de lista muestra más campos que la de **Azure monitor** > **Alertas**.   
- En las máquinas virtuales Linux, la vista **Diagnósticos de estado** muestra el nombre de dominio completo de la máquina virtual, en lugar del nombre definido por el usuario.
- Al apagar las máquinas virtuales, se actualizan algunos de los criterios de mantenimiento como *críticos* y otras como *Correcto*. Se muestra el estado de la máquina virtual de red como *críticos*.
- La gravedad de alerta del mantenimiento no se puede modificar. Solo puede ser habilitada o deshabilitada. Además, algunas gravedades se actualizan según el estado de los criterios de mantenimiento.
- Si modifica alguna configuración de un criterio de mantenimiento, se modifican todos los criterios de estado del mismo tipo en la máquina virtual. Por ejemplo, si se modifica el umbral del criterio de mantenimiento del espacio libre en el disco correspondiente al disco lógico C:, este umbral se aplicará a todos los demás discos lógicos detectados y supervisados para la misma máquina virtual.  
- Los umbrales de los criterios de mantenimiento que tienen como destino una máquina virtual Windows no son modificables, puesto que su estado está establecido como *en ejecución* o *disponible*. Cuando se consulta el estado de mantenimiento desde la [API Workload Monitor](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/workloadmonitor/resource-manager), muestra el valor *comparisonOperator* de **LessThan** o **GreaterThan** con un valor de *threshold* de **4** para el servicio o la entidad si:
   - Service Health del cliente DNS: el servicio no se está ejecutando. 
   - Service Health del cliente DHCP: el servicio no se está ejecutando. 
   - Service Health de RPC: el servicio no se está ejecutando. 
   - Service Health del firewall de Windows: el servicio no se está ejecutando.
   - Service Health del registro de eventos de Windows: el servicio no se está ejecutando. 
   - Service Health del servidor: el servicio no se está ejecutando. 
   - Service Health del servicio de administración remota de Windows: el servicio no se está ejecutando. 
   - Error o daño del sistema de archivos: el disco lógico no está disponible.

- Los umbrales de los siguientes criterios de mantenimiento de Linux no son modificables, ya que su estado de mantenimiento ya está establecido en *true*. El estado de mantenimiento muestra *comparisonOperator* con un valor **LessThan** y un valor de *threshold* de **1** cuando la API Workload Monitor realiza una consulta sobre la entidad en función de su contexto:
   - Estado del disco lógico: el disco lógico no está en línea ni está disponible
   - Estado del disco: el disco no está en línea ni está disponible
   - Estado del adaptador de red: el adaptador de red está deshabilitado  

- El criterio de mantenimiento del *uso de CPU total* de Windows muestra el umbral **no es igual que 4** tanto en el portal como en la API Workload Monitor. El umbral se alcanza cuando el *uso de CPU total* es mayor que el 95 por ciento y la longitud de la cola del sistema es mayor que 15. En esta versión, no se puede modificar este criterio de mantenimiento. 
- Los cambios de configuración, como la actualización de un umbral, tardan aproximadamente 30 minutos incluso aunque el portal o en la API Workload Monitor se actualicen inmediatamente. 
- Los criterios de mantenimiento del nivel de procesador lógico y del procesador individual no están disponibles en Windows. Solo el uso de CPU total está disponible para las máquinas virtuales Windows. 
- Las reglas de alertas que se definen para cada criterio de mantenimiento no se muestran en Azure Portal. Solo puede habilitar o deshabilitar una regla de alerta de estado en la [API Workload Monitor](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/workloadmonitor/resource-manager). 
- No puede asignar un [grupo de acciones de Azure Monitor](../../azure-monitor/platform/action-groups.md) para las alertas de estado desde Azure Portal. Solo puede usar la API de configuración de notificaciones para configurar un grupo de acciones que se desencadene cada vez que se desencadena una alerta de estado. Actualmente, puede asignar grupos de acciones en una máquina virtual, de tal forma que todas las *alertas de estado* desencadenadas en la máquina virtual desencadenen los mismos grupos de acciones. A diferencia de las alertas de Azure tradicionales, no existe el concepto de un grupo de acciones independiente para cada regla de alertas de mantenimiento. Además, solo se admiten los grupos de acciones configurados para enviar notificaciones mediante un correo electrónico o SMS cuando se desencadenan alertas de estado. 

## <a name="next-steps"></a>Pasos siguientes
Para conocer los requisitos y los métodos para habilitar la supervisión de máquinas virtuales, consulte [Implementación de Azure Monitor para VM](vminsights-onboard.md).
