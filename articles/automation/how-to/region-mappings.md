---
title: Asignaciones de área de trabajo Azure Automation y Log Analytics
description: En este artículo se describe las correspondencias permitidas entre una cuenta de Automation y un área de trabajo de Log Analytics para admitir la solución
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 05/20/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 8afe8fed8912dd365071f1c4c5560c9f5578dcd8
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66133100"
---
# <a name="workspace-mappings"></a>Asignaciones del área de trabajo

Al habilitar las soluciones como Update Management, Change Tracking e inventario o la Start/Stop VMs During OFF-hours, solo en determinadas regiones se admiten para vincular un área de trabajo de Log Analytics y una cuenta de Automation. Esta asignación se aplica solo a la cuenta de Automation y el área de trabajo de Log Analytics. Los recursos que el área de trabajo de la cuenta de Automation o Log Analytics pueden residir en otras regiones.

## <a name="supported-mappings"></a>Asignaciones admitidas

En la tabla siguiente se muestran las asignaciones admitidas:

|**Región del área de trabajo de Log Analytics**|**Región de Azure Automation**|
|---|---|
|**EE. UU.**||
|EastUS<sup>1</sup>|EastUS2|
|WestUS2|WestUS2|
|WestCentralUS<sup>2</sup>|WestCentralUS<sup>2</sup>|
|**Canadá**||
|CanadaCentral|CanadaCentral|
|**Asia Pacífico**||
|AustraliaSoutheast|AustraliaSoutheast|
|SoutheastAsia|SoutheastAsia|
|CentralIndia|CentralIndia|
|JapanEast|JapanEast|
|**Europa**||
|UKSouth|UKSouth|
|WestEurope|WestEurope|
|**US Gov**||
|USGovVirginia|USGovVirginia|

<sup>1</sup> EastUS asignación de áreas de trabajo de Log Analytics para las cuentas de Automation no es una asignación exacta de una región a otra, pero es la asignación correcta.

<sup>2</sup> debido a restricciones de capacidad la región no están disponible al crear nuevos recursos. Esto incluye las cuentas de Automation y Log Analytics de áreas de trabajo. Sin embargo, recursos vinculados que ya existían en la región deben continuar funcionando.

## <a name="unlink-workspace"></a>Unlink workspace (Desvincular área de trabajo)

Si decide que ya no desea integrar su cuenta de Automation con un área de trabajo de Log Analytics, puede desvincular la cuenta directamente desde el portal de Azure. Antes de continuar, primero deberá quitar la administración de actualizaciones, seguimiento de cambios e inventario o las máquinas virtuales de inicios y paradas durante las soluciones de las horas de trabajo si está usando. Si no se quita, este proceso se impedirá continuar. Revise el artículo de la solución concreta que ha importado para conocer los pasos necesarios para quitarla.

Después de quitar estas soluciones, puede realizar los pasos siguientes para desvincular la cuenta de Automation.

> [!NOTE]
> Algunas soluciones que incluyen versiones anteriores de la solución de supervisión de SQL Azure pueden haber creado recursos de automatización y también puede que tengan que quitarse antes de desvincularse del área de trabajo.

1. En Azure Portal, abra su cuenta de Automation y, en la página de la cuenta de Automation, seleccione **Área de trabajo vinculada** en la sección **Recursos relacionados** de la izquierda.

2. En la página Desvincular área de trabajo, haga clic en **Desvincular área de trabajo**. Recibirá un aviso para comprobar que desea continuar.

3. Aunque Azure Automation trate de desvincular la cuenta del área de trabajo de Log Analytics, puede seguir el progreso en **Notificaciones** en el menú.

Si ha usado la solución de administración de actualizaciones, también puede quitar los siguientes elementos que ya no necesite después de quitar la solución.

* Programaciones de actualizaciones: cada una tendrá nombres que coinciden con las implementaciones de las actualizaciones que ha creado.

* Los grupos de Hybrid Worker que se han creado para la solución: cada uno de ellos se llamará de forma similar a machine1.contoso.com_9ceb8108 - 26 c 9-4051-b6b3-227600d715c8).

Si ha usado la solución de inicio y detención de máquinas virtuales durante las horas de trabajo, también puede quitar los siguientes elementos que ya no necesite después de quitar la solución.

* Programaciones de runbook de inicio y detención de máquinas virtuales
* Runbooks de inicio y detención de máquinas virtuales
* variables

Como alternativa, también puede desvincular el área de trabajo desde su cuenta de Automation desde el área de trabajo de Log Analytics. En el área de trabajo, seleccione **cuenta de Automation** en **recursos relacionados**. En la página cuenta de Automation, seleccione **desvincular la cuenta**.

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre cómo incorporar las siguientes soluciones:

Administración de actualizaciones y seguimiento de cambios e inventario:

* Desde un [máquina virtual](../automation-onboard-solutions-from-vm.md)
* Desde su [cuenta de Automation](../automation-onboard-solutions-from-automation-account.md)
* Cuando [examinar varias máquinas](../automation-onboard-solutions-from-browse.md)
* Desde un [runbook](../automation-onboard-solutions.md)

Inicio y detención de máquinas virtuales durante las horas de trabajo

* [Implementar Start/Stop VMs during OFF-](../automation-solution-vm-management.md)