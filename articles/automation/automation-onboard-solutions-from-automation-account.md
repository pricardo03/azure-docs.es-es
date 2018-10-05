---
title: Obtenga información sobre la incorporación de las soluciones Update Management, Change Tracking e Inventory en Azure Automation
description: Obtenga información acerca de cómo incorporar una máquina virtual de Azure con las soluciones Update Management, Change Tracking e Inventory que forman parte de Azure Automation
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 06/06/2018
ms.topic: conceptual
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: 5b906b4a90dbceb62c6f2381d0ffa8bc1bee7ef1
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "47033828"
---
# <a name="onboard-update-management-change-tracking-and-inventory-solutions"></a>Incorporación de las soluciones Update Management, Change Tracking e Inventory

Azure Automation proporciona soluciones para administrar las actualizaciones de seguridad del sistema operativo, el seguimiento de cambios y el inventario de los componentes instalados en los equipos. Hay varias maneras de incorporar máquinas, puede incorporar la solución [desde una máquina virtual](automation-onboard-solutions-from-vm.md), [desde la exploración en varias máquinas](automation-onboard-solutions-from-browse.md), desde su cuenta de Automation o mediante un [runbook](automation-onboard-solutions.md). Este artículo trata la incorporación de estas soluciones desde la cuenta de Automation.

## <a name="log-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure en https://portal.azure.com

## <a name="enable-solutions"></a>Habilitar soluciones

Vaya a la cuenta de Automation y seleccione **Inventory** o **Change Tracking** en **ADMINISTRACIÓN DE CONFIGURACIÓN**.

Para habilitar la solución, elija el área de trabajo de Log Analytics y la cuenta de Automation y haga clic en **Habilitar**. La solución tarda hasta 15 minutos en habilitarse.

![Incorporación de la solución Inventory](media/automation-onboard-solutions-from-automation-account/onboardsolutions.png)

La solución Change Tracking e Inventario permite realizar un [control de cambios](automation-vm-change-tracking.md) e [inventario](automation-vm-inventory.md) en sus máquinas virtuales. En este paso, se habilita la solución en una máquina virtual.

Cuando se complete la notificación de que se ha incorporado la solución de control de cambios e inventario, haga clic en **Administración de actualizaciones** en **Administración de configuración**.

La solución Administración de actualizaciones permite administrar las actualizaciones y las revisiones para las máquinas virtuales Windows de Azure. Puede evaluar rápidamente el estado de las actualizaciones disponibles, programar la instalación de las actualizaciones necesarias y revisar los resultados de la implementación para comprobar si las actualizaciones se aplicaron correctamente en la máquina virtual. Esta acción activa la solución en la máquina virtual.

Seleccione **Update management** en **UPDATE MANAGEMENT**. El área de trabajo de Log Analytics seleccionada es la misma área de trabajo que se usa en el paso anterior. Haga clic en **Habilitar** para incorporar la solución Administración de actualizaciones. La solución tarda hasta 15 minutos en habilitarse.

![Incorporación de la solución de actualización](media/automation-onboard-solutions-from-automation-account/onboardsolutions2.png)

## <a name="scope-configuration"></a>Configuración de ámbito

Cada solución utiliza una configuración de ámbito en el área de trabajo para definir los equipos de destino que obtendrán la solución. La configuración de ámbito es un grupo de una o más búsquedas guardadas que se utiliza para limitar el ámbito de la solución a equipos específicos. Para tener acceso a las configuraciones de ámbito, en la cuenta de Automation, en **Recursos relacionados**, seleccione **Área de trabajo**. A continuación, en el área de trabajo, en **Orígenes de datos del área de trabajo**, seleccione **Configuraciones de ámbito**.

Si el área de trabajo seleccionada no dispone aún de las soluciones Update Management ni Change Tracking, se crearán las siguientes configuraciones de ámbito:

* **MicrosoftDefaultScopeConfig-ChangeTracking**

* **MicrosoftDefaultScopeConfig-Updates**

Si el área de trabajo seleccionada ya tiene la solución. La solución no se vuelve a implementar y la configuración de ámbito no se agrega a ella.

## <a name="saved-searches"></a>Búsquedas guardadas

Cuando un equipo se agrega a las soluciones Update Management, Change Tracking o Inventory, se agrega a una de las dos búsquedas guardadas del área de trabajo. Estas búsquedas guardadas son consultas que contienen los equipos de destino de estas soluciones.

Vaya a la cuenta de Automation y seleccione **Búsquedas guardadas** en **General**. Las dos búsquedas guardadas utilizadas por estas soluciones se pueden ver en la tabla siguiente:

|NOMBRE     |Categoría  |Alias  |
|---------|---------|---------|
|MicrosoftDefaultComputerGroup     |  ChangeTracking       | ChangeTracking__MicrosoftDefaultComputerGroup        |
|MicrosoftDefaultComputerGroup     | Actualizaciones        | Updates__MicrosoftDefaultComputerGroup         |

Seleccione cualquier búsqueda guardada para ver la consulta utilizada para rellenar el grupo. La siguiente imagen muestra la consulta y sus resultados:

![Búsquedas guardadas](media/automation-onboard-solutions-from-automation-account/savedsearch.png)

## <a name="onboard-azure-vms"></a>Incorporar máquinas virtuales de Azure

Desde la cuenta de Automation, seleccione **Inventory** o **Change Tracking** en **ADMINISTRACIÓN DE CONFIGURACIÓN** o **Update management** en **UPDATE MANAGEMENT**.

Haga clic en **+ Agregar máquinas virtuales de Azure** y seleccione una o varias máquinas virtuales en la lista. Las máquinas virtuales que no se pueden habilitar se atenúan y no se pueden seleccionar. En la página **Habilitar la administración de actualizaciones**, haga clic en **Habilitar**. Esto agrega las máquinas virtuales seleccionadas a la búsqueda guardada de grupos de equipos de la solución.

![Habilitar máquinas virtuales de Azure](media/automation-onboard-solutions-from-automation-account/enable-azure-vms.png)

## <a name="onboard-a-non-azure-machine"></a>Incorporación de una máquina que no es de Azure

Las máquinas que no estén en Azure se deben agregar manualmente. Desde la cuenta de Automation, seleccione **Inventory** o **Change Tracking** en **ADMINISTRACIÓN DE CONFIGURACIÓN** o **Update management** en **UPDATE MANAGEMENT**.

Haga clic en **Agregar un equipo que no es de Azure**. Esto abre una nueva ventana del explorador con las [instrucciones sobre cómo instalar y configurar Microsoft Monitoring Agent en la máquina](../log-analytics/log-analytics-concept-hybrid.md) para que la máquina pueda empezar a notificar a la solución. Si está incorporando una máquina administrada actualmente por System Center Operations Manager, no es necesario un nuevo agente, se especifica la información del área de trabajo en el agente existente.

## <a name="onboard-machines-in-the-workspace"></a>Incorporación de máquinas en el área de trabajo

Las máquinas instaladas manualmente o las máquinas que ya realicen notificaciones en el área de trabajo deben agregarse a Azure Automation para poder habilitar la solución. Desde la cuenta de Automation, seleccione **Inventory** o **Change Tracking** en **ADMINISTRACIÓN DE CONFIGURACIÓN** o **Update management** en **UPDATE MANAGEMENT**.

Seleccione **Administrar máquinas**. Se abrirá la página **Administrar máquinas**. Esta página permite habilitar la solución en un conjunto seleccionado de máquinas, en todas los máquinas disponibles o habilitar la solución para todas las máquinas actuales y habilitarlo en todas las máquinas futuras.

![Búsquedas guardadas](media/automation-onboard-solutions-from-automation-account/managemachines.png)

### <a name="all-available-machines"></a>Todas las máquinas disponibles

Para habilitar la solución para todas las máquinas disponibles, seleccione **Habilitar en todas las máquinas disponibles**. Esto deshabilita el control para agregar máquinas individualmente. Esta tarea agrega todos los nombres de las máquinas que informan al área de trabajo en la consulta de la búsqueda guardada de grupos de equipos.

### <a name="all-available-and-future-machines"></a>Todas las máquinas disponibles y futuras

Para habilitar la solución para todas las máquinas disponibles y futuras, seleccione **Habilitar en todas las máquinas disponibles y futuras**. Esta opción elimina las configuraciones de ámbito y las búsquedas guardadas del área de trabajo. Se abrirá la solución para todas las máquinas de Azure y las que no son de Azure que notifican al área de trabajo.

### <a name="selected-machines"></a>Máquinas seleccionadas

Para habilitar la solución para una o varias máquinas, seleccione **Habilitar en las máquinas seleccionadas** y haga clic en **Agregar** junto a cada máquina que se va a agregar a la solución. Esta tarea agrega los nombres de máquina seleccionados a la consulta de la búsqueda guardada de grupos de equipos de la solución.

## <a name="unlink-workspace"></a>Unlink workspace (Desvincular área de trabajo)

Las siguientes soluciones dependen de un área de trabajo de Log Analytics:

* [Administración de actualizaciones](automation-update-management.md)
* [Seguimiento de cambios](automation-change-tracking.md)
* [Inicio y detención de máquinas virtuales durante las horas de trabajo](automation-solution-vm-management.md)

Si decide que ya no desea integrar su cuenta de Automation con Log Analytics, puede desvincular la cuenta directamente desde Azure Portal.  Antes de continuar, primero deberá quitar las soluciones mencionadas anteriormente; en caso contrario, este proceso no podrá continuar. Revise el artículo de la solución concreta que ha importado para conocer los pasos necesarios para quitarla.

Después de quitar estas soluciones, puede realizar los pasos siguientes para desvincular la cuenta de Automation.

> [!NOTE]
> Algunas soluciones que incluyen versiones anteriores de la solución de supervisión de SQL Azure pueden haber creado recursos de automatización y también puede que tengan que quitarse antes de desvincularse del área de trabajo.

1. En Azure Portal, abra su cuenta de Automation y, en la página de la cuenta de Automation, seleccione **Área de trabajo vinculada** en la sección **Recursos relacionados** de la izquierda.

1. En la página Desvincular área de trabajo, haga clic en **Desvincular área de trabajo**.

   ![Página Desvincular área de trabajo](media/automation-onboard-solutions-from-automation-account/automation-unlink-workspace-blade.png).

   Recibirá un aviso para comprobar que desea continuar.

1. Aunque Azure Automation trate de desvincular la cuenta del área de trabajo de Log Analytics, puede seguir el progreso en **Notificaciones** en el menú.

Si ha usado la solución de administración de actualizaciones, también puede quitar los siguientes elementos que ya no necesite después de quitar la solución.

* Programaciones de actualizaciones: cada una tendrá nombres que coinciden con las implementaciones de las actualizaciones que ha creado.

* Los grupos de Hybrid Worker que se han creado para la solución: cada uno de ellos se llamará de forma similar a machine1.contoso.com_9ceb8108 - 26 c 9-4051-b6b3-227600d715c8).

Si ha usado la solución de inicio y detención de máquinas virtuales durante las horas de trabajo, también puede quitar los siguientes elementos que ya no necesite después de quitar la solución.

* Programaciones de runbook de inicio y detención de máquinas virtuales
* Runbooks de inicio y detención de máquinas virtuales
* variables

## <a name="next-steps"></a>Pasos siguientes

Continúe con los tutoriales de las soluciones para aprender a utilizarlas.

* [Tutorial: administración de las actualizaciones de la máquina virtual](automation-tutorial-update-management.md)

* [Tutorial: identificación del software en una máquina virtual](automation-tutorial-installed-software.md)

* [Tutorial: solución de problemas de los cambios en una máquina virtual](automation-tutorial-troubleshoot-changes.md)