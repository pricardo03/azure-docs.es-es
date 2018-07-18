---
title: Aprenda a incorporar las soluciones Update Management, Change Tracking e Inventory en varias máquinas virtuales en Azure Automation
description: Obtenga información acerca de cómo incorporar una máquina virtual de Azure con las soluciones Update Management, Change Tracking e Inventory que forman parte de Azure Automation
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 06/06/2018
ms.topic: article
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: 0a624d850b8c3260acb24cb17566090e8ad0043e
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/08/2018
ms.locfileid: "35233944"
---
# <a name="enable-update-management-change-tracking-and-inventory-solutions-on-multiple-vms"></a>Habilitación de las soluciones Update Management, Change Tracking e Inventory en varias máquinas virtuales

Azure Automation proporciona soluciones para administrar las actualizaciones de seguridad del sistema operativo, el seguimiento de cambios y el inventario de los componentes instalados en los equipos. Hay varias formas de incorporar máquinas, puede incorporar la solución desde una máquina virtual, [desde la cuenta de Automation](automation-onboard-solutions-from-vm.md), [al explorar máquinas virtuales](automation-onboard-solutions-from-automation-account.md) o mediante un [runbook](automation-onboard-solutions.md). Este artículo trata la incorporación de estas soluciones al examinar máquinas virtuales en Azure.

## <a name="log-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure en https://portal.azure.com

## <a name="enable-solutions"></a>Habilitar soluciones

En Azure Portal, vaya a **Máquinas virtuales**.

Mediante las casillas, seleccione las máquinas virtuales que desee incorporar con Change Tracking, Inventory o Update Management. La incorporación está disponible para un máximo de tres grupos de recursos a la vez.

![Lista de máquinas virtuales](media/automation-onboard-solutions-from-browse/vmlist.png)
> [!TIP]
> Use los controles de filtro para modificar la lista de máquinas virtuales y, después, haga clic en la casilla superior para seleccionar todas las máquinas virtuales de la lista.

En la barra de comandos, haga clic en **Servicios** y seleccione **Change tracking**, **Inventory** o **Update Management**.

> [!NOTE]
> **Change tracking** e **Inventory** utilizan la misma solución; cuando una está habilitada, la otra también lo está.

La siguiente imagen es de Update Management. Change Tracking e Inventory tienen el mismo diseño y comportamiento.

La lista de máquinas virtuales se filtra para mostrar solo las que se encuentran en la misma suscripción y ubicación. Si las máquinas virtuales están en más de tres grupos de recursos, se seleccionan los tres primeros.

Use los controles de filtro para seleccionar máquinas virtuales de distintas suscripciones, ubicaciones y grupos de recursos.

![Habilitar la solución Update Management](media/automation-onboard-solutions-from-browse/onboardsolutions.png)

Revise las opciones del área de trabajo de Log Analytics y de la cuenta de Automation. De forma predeterminada están seleccionadas un área de trabajo y una cuenta de Automation nuevas. Si tiene una área de trabajo de Log Analytics y una cuenta de Automation existentes que desee usar, haga clic en **cambiar** para seleccionarlas en la página **Configuración**. Cuando termine, haga clic en **Guardar**.

![Seleccionar área de trabajo y cuenta](media/automation-onboard-solutions-from-browse/selectworkspaceandaccount.png)

Anule la selección de las casillas que haya junto a las máquinas virtuales que no desee habilitar. Ya se ha anulado la selección de las máquinas virtuales que no se puedan habilitar.

Haga clic en **Habilitar** para habilitar la solución. La solución tarda hasta 15 minutos en habilitarse.

## <a name="troubleshooting"></a>solución de problemas

Al incorporar varias máquinas, puede haber algunas en las que aparezca **No se puede habilitar**. Hay diferentes motivos por los que algunas máquinas no se pueden habilitar. Las secciones siguientes muestran las posibles razones para que aparezca el estado **No se puede habilitar** en una máquina virtual al intentar incorporarla.

### <a name="vm-reports-to-a-different-workspace-workspacename--change-configuration-to-use-it-for-enabling"></a>La máquina virtual depende de otra área de trabajo: '\<NombreDeAreaDeTrabajo\>'.  Cambie la configuración para usarla para habilitar

**Causa**: este error muestra que la máquina virtual que intenta incorporar depende de otra área de trabajo.

**Solución**: haga clic en **Use as configuration** (Usar como configuración) para cambiar la cuenta de Automation y el área de trabajo de Log Analytics de destino.

### <a name="vm-reports-to-a-workspace-that-is-not-available-in-this-subscription"></a>La máquina virtual depende de un área de trabajo que no está disponible en esta suscripción

**Causa**: el área de trabajo de la que depende la máquina virtual:

* Está en otra suscripción, o bien
* No existe, o bien
* Está en un grupo de recursos al que no tiene permisos de acceso

**Solución**: busque la cuenta de Automation asociada al área de trabajo que la depende la máquina virtual e incorpore la máquina virtual, cambiando la configuración del ámbito.

### <a name="vm-operating-system-version-or-distribution-is-not-supported"></a>La versión o distribución del sistema operativo de la máquina virtual no son compatibles

**Causa:** la solución no es compatible con todas las distribuciones de Linux o todas las versiones de Windows.

**Solución:** consulte la [lista de clientes admitidos](automation-update-management.md#clients) para la solución.

### <a name="classic-vms-cannot-be-enabled"></a>Las máquinas virtuales clásicas no se puede habilitar

**Causa**: no se admiten las máquinas virtuales que usan el modelo de implementación clásica.

**Solución**: migre la máquina virtual al modelo de implementación de Resource Manager. Para aprender a hacerlo, consulte [Migración de los recursos del modelo de implementación clásica](../virtual-machines/windows/migration-classic-resource-manager-overview.md).

### <a name="vm-is-stopped-deallocated"></a>La máquina virtual está detenida. (desasignada)

**Causa**: la máquina virtual no está en estado de **ejecución**.

**Solución**: para incorporar una máquina virtual a una solución debe estar en ejecución. Haga clic en el vínculo insertado **Iniciar VM** para iniciar la máquina virtual sin salir de la página.

## <a name="next-steps"></a>Pasos siguientes

Ahora que está habilitada la solución para las máquinas virtuales, visite el artículo de información general de Update Management para aprender a ver la evaluación de la actualización de las máquinas.

> [!div class="nextstepaction"]
> [Update Management: ver evaluación de actualización](./automation-update-management.md#viewing-update-assessments)

Tutoriales acerca de las soluciones y cómo utilizarlas:

* [Tutorial: administración de las actualizaciones de la máquina virtual](automation-tutorial-update-management.md)

* [Tutorial: identificación del software en una máquina virtual](automation-tutorial-installed-software.md)

* [Tutorial: solución de problemas de los cambios en una máquina virtual](automation-tutorial-troubleshoot-changes.md)