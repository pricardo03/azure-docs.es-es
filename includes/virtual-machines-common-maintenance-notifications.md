---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: shants123
ms.service: virtual-machines
ms.topic: include
ms.date: 07/02/2018
ms.author: shants
ms.custom: include file
ms.openlocfilehash: b3f71c4710bd9711a3209dd55f8e680f63627c1b
ms.sourcegitcommit: f6050791e910c22bd3c749c6d0f09b1ba8fccf0c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2018
ms.locfileid: "50035228"
---
## <a name="view-vms-scheduled-for-maintenance-in-the-portal"></a>Visualización de las máquinas virtuales programadas para mantenimiento en el portal

Una vez que se programa una tanda de mantenimiento planeado, puede observar la lista de máquinas virtuales que se ven afectadas por la siguiente tanda. 

Puede usar Azure Portal y buscar máquinas virtuales con mantenimiento programado.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).

2. En el panel de navegación de la izquierda, haga clic en **Máquinas virtuales**.

3. En el panel Máquinas virtuales, haga clic en el botón **Columnas** para abrir la lista de columnas disponibles.

4. Seleccione y agregue las siguientes columnas:

   **Mantenimiento**: muestra el estado de mantenimiento para la máquina virtual. Estos son los valores posibles:
      
      | Valor | DESCRIPCIÓN |
      |-------|-------------|
      | Empezar ahora | La máquina virtual está en la ventana de mantenimiento de autoservicio, lo que le permite iniciar el mantenimiento usted mismo. Vea a continuación cómo iniciar el mantenimiento en la máquina virtual. | 
      | Programado | La máquina virtual está programada para el mantenimiento sin opción de que inicie el mantenimiento. Puede obtener información de la ventana de mantenimiento mediante la selección de la ventana Mantenimiento: Programado en esta vista o haciendo clic en la máquina virtual. | 
      | Ya se actualizó | La máquina virtual ya se actualizó y no se requiere ninguna otra acción en este momento. | 
      | Inténtelo de nuevo más tarde | Ha intentado iniciar el mantenimiento sin conseguirlo. Podrá utilizar la opción de mantenimiento de autoservicio más adelante. | 
      | Intentar ahora | Puede volver a intentar un mantenimiento automático que se intentó anteriormente sin éxito. | 
      | - | La máquina virtual no forma parte de una tanda de mantenimiento planeado. |
      

   **Ventana Mantenimiento: Autoservicio**: muestra la ventana de tiempo en la que puede iniciar el mantenimiento usted mismo en las máquinas virtuales.
   
   **Ventana Mantenimiento: programado**: muestra la ventana de tiempo en la que Azure reiniciará la máquina virtual con el fin de realizar el mantenimiento. 



## <a name="notification-and-alerts-in-the-portal"></a>Notificaciones y alertas en el portal

Azure comunica una programación para el mantenimiento planeado enviando un correo electrónico al grupo de propietario y copropietarios de la suscripción. Puede agregar destinatarios y canales adicionales a esta comunicación creando alertas de registro de actividad de Azure. Para más información, consulte [Supervise la actividad de suscripción con Azure Activity Log](../articles/monitoring-and-diagnostics/monitoring-overview-activity-logs.md).

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. En el menú de la izquierda, seleccione **Monitor**. 
3. En el panel **Monitor: alertas (clásico)**, haga clic en **+ Agregar alerta de registro de actividad**.
5. Rellene la información de la página **Agregar alerta de registro de actividad** y asegúrese de establecer lo siguiente en **Criterios**:
   - **Categoría de eventos**: Azure Service Health
   - **Servicios**: Virtual Machine Scale Sets y Virtual Machines
   - **Tipo**: mantenimiento planeado 
    
Para más información sobre cómo configurar las alertas de registro de actividad, vea [Creación de alertas de registro de actividad](../articles/monitoring-and-diagnostics/monitoring-activity-log-alerts.md).
    
    
## <a name="start-maintenance-on-your-vm-from-the-portal"></a>Inicio del mantenimiento en la máquina virtual desde el portal

Al examinar los detalles de la máquina virtual, podrá ver más detalles relacionados con el mantenimiento.  
En la parte superior de la vista de detalles de la máquina virtual, se agregará una nueva cinta de opciones de notificación si la máquina virtual se incluye en una tanda de mantenimiento planeado. Además, se agrega una nueva opción para iniciar el mantenimiento siempre que sea posible. 


Haga clic en la notificación de mantenimiento para ver la página de mantenimiento con más detalles en el mantenimiento planeado. Desde allí podrá **iniciar el mantenimiento** en su máquina virtual.

Una vez que comience el mantenimiento, en pocos minutos comenzará en la máquina virtual y el estado de mantenimiento se actualizará para reflejar el resultado.

Si se perdió la ventana de autoservicio, todavía podrá ver la ventana cuando Azure mantenga la máquina virtual. 
