---
title: Control de las notificaciones de mantenimiento de Virtual Machine Scale Sets en Azure | Microsoft Docs
description: Vea las notificaciones de mantenimiento de Virtual Machine Scale Sets en Azure e inicie el mantenimiento de autoservicio.
services: virtual-machine-scale-sets
documentationcenter: ''
author: shants123
editor: ''
tags: azure-service-management,azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/09/2018
ms.author: shants
ms.openlocfilehash: 4ce984686c2bb320d5d32771d31b81cdec1153af
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2018
ms.locfileid: "38506262"
---
# <a name="handling-planned-maintenance-notifications-for-virtual-machine-scale-sets"></a>Control de las notificaciones de mantenimiento planeado de Virtual Machine Scale Sets

Azure realiza periódicamente actualizaciones para mejorar la confiabilidad, el rendimiento y la seguridad de la infraestructura host de las máquinas virtuales. Las actualizaciones son cambios como la aplicación de revisiones en el entorno de hospedaje o la actualización y la retirada de hardware. La mayoría de estas actualizaciones se realizan sin afectar a las máquinas virtuales hospedadas. Sin embargo, hay casos en los que las actualizaciones tienen un impacto:

- Si el mantenimiento no requiere un reinicio, Azure usa la migración en contexto para pausar la máquina virtual mientras se actualiza el host. Estas operaciones de mantenimiento sin reinicialización se aplican dominio por dominio, si tienen errores, y el progreso se detiene si se recibe alguna señal de estado de advertencia.

- Si el mantenimiento requiere un reinicio, el usuario recibe un aviso de cuándo está programado el mantenimiento. En estos casos, se le asigna también un período de tiempo donde puede iniciar el mantenimiento a la hora que le sea más conveniente.


El mantenimiento planeado que requiere un reinicio se programa en olas. Cada ola tiene un ámbito diferente (regiones).

- Una ola comienza con una notificación a los clientes. De forma predeterminada, la notificación se envía al propietario de la suscripción y a los copropietarios. Con las [alertas del registro de actividad](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) de Azure es posible agregar más destinatarios y opciones de mensajería, como el correo electrónico, los SMS y los webhooks, a las notificaciones.  
- En el momento de la notificación, aparece una *ventana de autoservicio*. En ella, puede ver qué máquinas virtuales se incluyen en esta ola e iniciar de forma proactiva el mantenimiento en función de las necesidades que dicte su programación.
- Después de la ventana de autoservicio, comienza una *ventana de mantenimiento programado*. Mientras está ventana está activa Azure programa el mantenimiento necesario y lo aplica a la máquina virtual. 

El objetivo de tener dos ventanas es proporcionar tiempo suficiente para que pueda iniciar el mantenimiento y reiniciar la máquina virtual sabiendo cuando iniciará Azure el mantenimiento automáticamente.


Puede usar Azure Portal, PowerShell, la API de REST y la CLI para consultar las ventanas de mantenimiento de las máquinas virtuales de Virtual Machine Scale Set e iniciar el mantenimiento de autoservicio.

  
## <a name="should-you-start-maintenance-during-the-self-service-window"></a>¿Debe comenzar el mantenimiento durante la ventana de autoservicio?  

Las directrices siguientes están pensadas para ayudarle a decidir si debe utilizar esta funcionalidad e iniciar el mantenimiento a su ritmo.

> [!NOTE] 
> Es posible que el mantenimiento de autoservicio no esté disponible para todas las máquinas virtuales. Para determinar si su máquina virtual se puede volver a implementar de forma proactiva, busque **Iniciar ahora** en el estado del mantenimiento. El mantenimiento de autoservicio no está disponible para Cloud Services (rol de trabajo o web) y Service Fabric.


El mantenimiento de autoservicio no se recomienda para implementaciones que usen **conjuntos de disponibilidad**, ya que éstas son configuraciones de alta disponibilidad, en las que en un momento dado solo resulta afectado un dominio de actualización. 

- Deje que Azure desencadene el mantenimiento. Para el mantenimiento que requiera el reinicio, tenga en cuenta que el mantenimiento se realizará dominio de actualización por dominio de actualización, que los dominios de actualización no reciben necesariamente el mantenimiento de forma secuencial y que hay una pausa de 30 minutos entre los dominios de actualización.
- Si una pérdida temporal de una parte de su capacidad (1/recuento de dominio de actualización) supone un problema, se puede compensar fácilmente mediante la asignación de instancias adicionales durante el período de mantenimiento.
- Para el mantenimiento que no requiere el reinicio, las actualizaciones se aplican en el nivel de dominio de error. 
    
**No** utilice el mantenimiento de autoservicio en los escenarios siguientes: 

- Si apaga las máquinas virtuales con frecuencia, ya sea manualmente, mediante DevTest Labs, con el apagado automático o de forma programada, puede revertir el estado de mantenimiento y, por tanto, provocar mayor tiempo de inactividad.
- En las máquinas virtuales de corta duración que sepa que se van a eliminar antes del final del mantenimiento. 
- En el caso de las cargas de trabajo con un estado grande almacenadas en el disco local (efímero) en el que se desea realizar el mantenimiento tras la actualización. 
- En los casos en los que se cambia el tamaño de una máquina virtual con frecuencia, ya que pudo revertir el estado de mantenimiento. 
- Si ha realizado eventos programados que permiten una conmutación por error proactiva o un apagado ordenado de la carga de trabajo, 15 minutos antes de que comience el apagado de mantenimiento.

**Use** el mantenimiento de autoservicio si tiene planeado ejecutar la máquina virtual de forma ininterrumpida durante la fase de mantenimiento programado y no se produce ninguna de las contraindicaciones anteriores. 

Se recomienda usar el mantenimiento de autoservicio en los siguientes casos:

- Es preciso comunicar una ventana de mantenimiento exacta a la dirección o al cliente final. 
- Es preciso completar el mantenimiento en una fecha determinada. 
- Es preciso controlar la secuencia de mantenimiento, por ejemplo, la aplicación de niveles múltiples para garantizar la recuperación segura.
- Se necesitan más de 30 minutos de recuperación de la máquina virtual entre dos dominios de actualización (UD). Para controlar el tiempo entre los dominios de actualización, no puede desencadenar el mantenimiento en las máquinas virtuales en más de un dominio de actualización (UD) a la vez.

 
## <a name="view-virtual-machine-scale-sets-impacted-by-maintenance-in-the-portal"></a>Visualización de los conjuntos de escalado de máquinas virtuales afectados por el mantenimiento en el portal

Cuando se programa una tanda de mantenimiento planeado, con Azure Portal puede observar la lista de conjuntos de escalado de máquinas virtuales que se ven afectados por la siguiente tanda. 

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Seleccione **Todos los servicios** en el panel de navegación izquierdo y elija **Conjuntos de escalado de máquinas virtuales**.
3. En la página **Conjuntos de escalado de máquinas virtuales**, elija la opción **Editar columnas** en la parte superior para abrir la lista de columnas disponibles.
4. En la **sección Columnas disponibles**, seleccione el elemento **Mantenimiento de autoservicio** y utilice los botones de flecha para pasarlo a la lista **Columnas seleccionadas**. Puede cambiar la lista desplegable en la sección **Columnas disponibles** desde **Todas** a **Propiedades** para que el elemento **Mantenimiento de autoservicio** resulte más fácil de encontrar. Una vez que tenga el elemento **Mantenimiento de autoservicio** en la sección **Columnas seleccionadas**, seleccione el botón **Aplicar** situado en la parte inferior de la página. 

Después de seguir los pasos anteriores, la columna **Mantenimiento de autoservicio** aparecerá en la lista de conjuntos de escalado de máquinas virtuales. Cada conjunto de escalado de máquinas virtuales puede tener uno de los siguientes valores para la columna de mantenimiento de autoservicio:

| Valor | DESCRIPCIÓN |
|-------|-------------|
| Sí | Al menos una máquina virtual en el conjunto de escalado de máquinas virtuales está en una ventana de autoservicio. Puede iniciar el mantenimiento en cualquier momento durante esta ventana de autoservicio. | 
| Sin  | No hay ninguna máquina virtual en una ventana de autoservicio del conjunto de escalado de máquinas virtuales afectado. | 
| - | Los conjuntos de escalado de máquinas virtuales no forman parte de una tanda de mantenimiento planeado.| 

## <a name="notification-and-alerts-in-the-portal"></a>Notificaciones y alertas en el portal

Azure comunica una programación para el mantenimiento planeado enviando un correo electrónico al grupo de propietario y copropietarios de la suscripción. Puede agregar destinatarios y canales adicionales a esta comunicación creando alertas de registro de actividad de Azure. Para más información, consulte [Supervisión de la actividad de suscripción con el registro de actividad de Azure] (../monitoring-and-diagnostics/monitoring-overview-activity-logs.md)

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. En el menú de la izquierda, seleccione **Monitor**. 
3. En el panel **Monitor: alertas (clásico)**, haga clic en **+ Agregar alerta de registro de actividad**.
4. Rellene la información de la página **Agregar alerta de registro de actividad** y asegúrese de establecer lo siguiente en **Criterios**:
   - **Categoría de eventos**: Azure Service Health
   - **Servicios**: Virtual Machine Scale Sets y Virtual Machines
   - **Tipo**: mantenimiento planeado 
    
Para obtener más información sobre cómo configurar las alertas de registro de actividad, vea [Creación de alertas de registro de actividad](../monitoring-and-diagnostics/monitoring-activity-log-alerts.md)
    
    
## <a name="start-maintenance-on-your-virtual-machine-scale-set-from-the-portal"></a>Inicio del mantenimiento en el conjunto de escalado de máquinas virtuales desde el portal

Al examinar la información general de los conjuntos de escalado de máquinas virtuales, podrá ver más detalles relacionados con el mantenimiento. Si se incluye al menos una máquina virtual en el conjunto de escalado de máquinas virtuales en la tanda de mantenimiento planeado, se agregará una nueva cinta de opciones de notificación en la parte superior de la página. Puede hacer clic en la cinta de opciones de notificación para ir a la página **Mantenimiento** y ver qué instancia de máquina virtual se ve afectada por el mantenimiento planeado. 

A partir de ahí, podrá iniciar el mantenimiento activando la casilla correspondiente a la máquina virtual afectada y, a continuación, haciendo clic en la opción **Iniciar mantenimiento**.

Una vez que comience el mantenimiento, las máquinas virtuales afectadas en el conjunto de escalado de máquinas virtuales se someterá a mantenimiento y no estará disponible temporalmente. Si perdió la ventana de autoservicio, seguirá pudiendo ver la ventana cuando Azure mantenga el conjunto de escalado de máquinas virtuales.
 
## <a name="check-maintenance-status-using-powershell"></a>Comprobación del estado de mantenimiento con PowerShell

Puede usar Azure Powershell para ver cuándo está programado el mantenimiento de las máquinas virtuales de sus conjuntos de escalado. La información de mantenimiento planeado está disponible desde el cmdlet [Get-AzureRmVmss](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmss) cuando se usa el parámetro `-InstanceView`.
 
La información de mantenimiento se devuelve solo si hay mantenimiento planeado. Si no está programado ningún mantenimiento que afecte a la instancia de la máquina virtual, el cmdlet no devuelve ninguna información de mantenimiento. 

```powershell
Get-AzureRmVmss -ResourceGroupName rgName -VMScaleSetName vmssName -InstanceId id -InstanceView
```

Las siguientes propiedades se devuelven en MaintenanceRedeployStatus: 
| Valor | DESCRIPCIÓN   |
|-------|---------------|
| IsCustomerInitiatedMaintenanceAllowed | Indica si puede iniciar el mantenimiento en la máquina virtual en este momento ||
| PreMaintenanceWindowStartTime         | El comienzo de la ventana de autoservicio de mantenimiento en la que puede iniciar el mantenimiento en la máquina virtual ||
| PreMaintenanceWindowEndTime           | El final de la ventana de autoservicio de mantenimiento en la que puede iniciar el mantenimiento en la máquina virtual ||
| MaintenanceWindowStartTime            | El comienzo del mantenimiento programado en que Azure inicia el mantenimiento de la máquina virtual ||
| MaintenanceWindowEndTime              | El final de la ventana de mantenimiento programado en la que Azure inicia el mantenimiento de la máquina virtual ||
| LastOperationResultCode               | El resultado del último intento de iniciar el mantenimiento en la máquina virtual ||



### <a name="start-maintenance-on-your-vm-instance-using-powershell"></a>Inicio del mantenimiento en la instancia de la máquina virtual mediante PowerShell

Puede iniciar el mantenimiento en una máquina virtual si **IsCustomerInitiatedMaintenanceAllowed** se establece en true mediante el cmdlet [Set-AzureRmVmss](/powershell/module/azurerm.compute/set-azurermvmss) con el parámetro `-PerformMaintenance`.

```powershell
Set-AzureRmVmss -ResourceGroupName rgName -VMScaleSetName vmssName -InstanceId id -PerformMaintenance 
```

## <a name="check-maintenance-status-using-cli"></a>Comprobación del estado de mantenimiento mediante la CLI

Se puede ver la información de mantenimiento planeado con [az vmss list-instances](/cli/azure/vmss?view=azure-cli-latest#az-vmss-list-instances).
 
La información de mantenimiento se devuelve solo si hay mantenimiento planeado. Si no hay ningún mantenimiento programado que afecte a la instancia de la máquina virtual, el comando no devuelve ninguna información de mantenimiento. 

```azure-cli
az vmss list-instances -g rgName -n vmssName --expand instanceView
```

Las siguientes propiedades se devuelven en MaintenanceRedeployStatus para cada instancia de una máquina virtual: 
| Valor | DESCRIPCIÓN   |
|-------|---------------|
| IsCustomerInitiatedMaintenanceAllowed | Indica si puede iniciar el mantenimiento en la máquina virtual en este momento ||
| PreMaintenanceWindowStartTime         | El comienzo de la ventana de autoservicio de mantenimiento en la que puede iniciar el mantenimiento en la máquina virtual ||
| PreMaintenanceWindowEndTime           | El final de la ventana de autoservicio de mantenimiento en la que puede iniciar el mantenimiento en la máquina virtual ||
| MaintenanceWindowStartTime            | El comienzo del mantenimiento programado en que Azure inicia el mantenimiento de la máquina virtual ||
| MaintenanceWindowEndTime              | El final de la ventana de mantenimiento programado en la que Azure inicia el mantenimiento de la máquina virtual ||
| LastOperationResultCode               | El resultado del último intento de iniciar el mantenimiento en la máquina virtual ||


### <a name="start-maintenance-on-your-vm-instance-using-cli"></a>Inicio del mantenimiento en la instancia de la máquina virtual mediante la CLI

La siguiente llamada iniciará el mantenimiento en una instancia de máquina virtual si `IsCustomerInitiatedMaintenanceAllowed` está establecido en true.

```azure-cli
az vmss perform-maintenance -g rgName -n vmssName --instance-ids id
```

## <a name="faq"></a>Preguntas más frecuentes


**P: ¿Por qué hay que reiniciar las máquinas virtuales ahora?**

**R:** Aunque la mayoría de las actualizaciones de la plataforma Azure no afectan a la disponibilidad de la máquina virtual, hay casos en los que es imposible evitar reiniciar las máquinas virtuales hospedadas en Azure. Hemos acumulado varios cambios que requieren el reinicio de nuestros servidores, lo que conllevará el reinicio de las máquinas virtuales.

**P: Si sigo las recomendaciones para lograr una alta disponibilidad mediante un conjunto de disponibilidad, ¿estoy seguro?**

**R:** Las máquinas virtuales implementadas en un conjunto de disponibilidad o en conjuntos de escalado de máquinas virtuales tienen la noción de dominios de actualización (UD). Al realizar mantenimiento, Azure respeta la restricción de UD, por lo que no reiniciará máquinas virtuales de diferentes UD (del mismo conjunto de disponibilidad).  Azure también espera al menos 30 minutos antes de pasar al siguiente grupo de máquinas virtuales. 

Para más información acerca de la alta disponibilidad, consulte [Regiones y disponibilidad de máquinas virtuales en Azure](../virtual-machines/windows/regions-and-availability.md).

**P: ¿Cómo recibo notificaciones acerca del mantenimiento planeado?**

**R:** Una oleada de mantenimiento planeado se inicia mediante el establecimiento de una programación en una o varias regiones de Azure. Poco después, se envía una notificación por correo electrónico a los propietarios de las suscripciones (un correo electrónico por suscripción). Mediante Alertas de registro de actividad se pueden configurar más canales y destinatarios de esta notificación. En caso de que implemente una máquina virtual en una región en la que ya se ha programado un mantenimiento planeado, no recibirá la notificación, sino que tendrá que comprobar el estado de mantenimiento de la máquina virtual.

**P: No veo ninguna indicación de mantenimiento planeado en el portal, Powershell ni la CLI. ¿Qué está pasando?**

**R:** La información relacionada con el mantenimiento planeado está disponible durante una tanda de mantenimiento planeado solo para las máquinas virtuales que van a verse afectadas por el mismo. En otras palabras, si no ve datos, es posible que la oleada de mantenimiento se haya completado (o que no haya empezado) o que la máquina virtual ya esté alojada en un servidor actualizado.

**P: ¿Existe alguna una manera de saber exactamente cuándo se verá afectada mi máquina virtual?**

**R:** Al establecer la programación, definimos una ventana de tiempo de varios días. Sin embargo, se desconoce la secuencia exacta de los servidores (y las máquinas virtuales) en esta ventana. Los clientes que deseen conocer el tiempo exacto para sus máquinas virtuales pueden usar [eventos programados](../virtual-machines/windows/scheduled-events.md) y realizar consultas desde la propia máquina virtual y recibir una notificación 15 minutos antes de que una máquina virtual se reinicie.

**P: ¿Cuánto tiempo tardará en reiniciar mi máquina virtual?**

**R:** En función del tamaño de la máquina virtual, el reinicio podría tardar varios minutos durante la ventana de mantenimiento de autoservicio. En los reinicios ejecutados por Azure en la ventana de mantenimiento programada, el reinicio suele tardar unos 25 minutos. Tenga en cuenta que, si usa Cloud Services (roles de trabajo o web), Virtual Machine Scale Sets o conjuntos de disponibilidad, se le proporcionarán treinta minutos entre cada grupo de máquinas virtuales (UD) durante la ventana de mantenimiento programado. 

**P: No veo información de mantenimiento en mis máquinas virtuales. ¿A qué se debe?**

**R:** Hay varios motivos por los que no ve información de mantenimiento en sus máquinas virtuales:
   - Utiliza una suscripción marcada como interna de Microsoft.
   - Las máquinas virtuales no están programadas para su mantenimiento. Es posible que la tanda de mantenimiento haya finalizado, se haya cancelado o se haya modificado para que las máquinas virtuales dejen de verse afectadas por ella.
   - La columna **Mantenimiento** no se ha agregado a la vista de la lista de máquinas virtuales. Aunque hemos agregado esta columna a la vista predeterminada, los clientes que se han configurado para ver las columnas no predeterminadas deben agregar manualmente la columna de **mantenimiento** a la vista de lista de máquinas virtuales.

**P: Está programado que se realice un segundo mantenimiento programado de mi máquina virtual. ¿Por qué?**

**R:** Hay varios casos de uso en los que verá que el mantenimiento de su máquina virtual está programado después de que haya completado el ciclo de mantenimiento-reimplementación:
   - Hemos cancelado la oleada de mantenimiento y la hemos reiniciado con otra carga útil. Es posible que hayamos detectado una carga útil con errores y que necesitemos implementar una carga adicional.
   - El *servicio de una máquina virtual se ha reparado* en otro nodo debido a un error de hardware.
   - Ha seleccionado detener (desasignar) y reiniciar la máquina virtual.
   - El **apagado automático** se ha activado en la máquina virtual.

## <a name="next-steps"></a>Pasos siguientes

Obtenga información acerca de cómo puede registrarse para eventos de mantenimiento desde la máquina virtual con [Eventos programados](../virtual-machines/windows/scheduled-events.md).
