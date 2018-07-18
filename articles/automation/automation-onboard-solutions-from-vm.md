---
title: Incorporación de las soluciones Update Management, Change Tracking e Inventory desde una máquina virtual de Azure
description: Aprenda a incorporar las soluciones Update Management, Change Tracking e Inventory que forman parte de Azure Automation, desde una máquina virtual de Azure.
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 06/06/2018
ms.topic: conceptual
ms.service: automation
ms.custom: mvc
manager: carmonm
ms.openlocfilehash: f270b2ccea51e83bc6475051b8667bf73d7fd717
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/19/2018
ms.locfileid: "36221519"
---
# <a name="onboard-update-management-change-tracking-and-inventory-solutions-from-an-azure-virtual-machine"></a>Incorporación de las soluciones Update Management, Change Tracking e Inventory desde una máquina virtual de Azure

Azure Automation proporciona soluciones para administrar las actualizaciones de seguridad del sistema operativo, el seguimiento de cambios y el inventario de los componentes instalados en los equipos. Hay varias maneras de incorporar las soluciones. Puede incorporar la solución desde una máquina virtual, [desde la cuenta de Automation](automation-onboard-solutions-from-automation-account.md), [mediante la exploración de varias máquinas](automation-onboard-solutions-from-browse.md) o usando un [runbook](automation-onboard-solutions.md). En este artículo se describe la incorporación de estas soluciones desde una máquina virtual de Azure.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure Portal en https://portal.azure.com.

## <a name="enable-the-solutions"></a>Habilitación de las soluciones

Vaya a una máquina virtual existente. En **Operaciones**, seleccione **Update Management**, **Inventory** o **Change Tracking**.

Para habilitar la solución solo para la máquina virtual, asegúrese de que esté seleccionada la opción **Enable for this VM** (Habilitar para esta máquina virtual). Para incorporar la solución a varias máquinas, seleccione **Enable for VMs in this subscription** (Habilitar para las máquinas virtuales de esta suscripción) y, luego, seleccione **Click to select machines to enable** (Hacer clic para seleccionar las máquinas para habilitar). Para más información sobre la incorporación en varias máquinas a la vez, consulte [Incorporación de las soluciones Update Management, Change Tracking e Inventory](automation-onboard-solutions-from-automation-account.md).

Para habilitar la solución, elija el área de trabajo de Azure Log Analytics y la cuenta de Automation, y seleccione **Habilitar**. La solución tarda hasta 15 minutos en habilitarse.

![Incorporación de la solución Update Management](media/automation-onboard-solutions-from-vm/onboard-solution.png)

Vaya a las otras soluciones y, a continuación, seleccione **Habilitar**. Las listas desplegables de cuentas de Log Analytics y Automation están deshabilitadas porque estas soluciones usan la misma área de trabajo y cuenta de Automation que la solución anteriormente habilitada.

> [!NOTE]
> **Change tracking** y **Inventory** usan la misma solución. Cuando una de estas soluciones está habilitada, también se habilita la otra.

## <a name="scope-configuration"></a>Configuración de ámbito

Cada solución emplea una configuración de ámbito en el área de trabajo para definir los equipos de destino que obtendrán la solución. La configuración de ámbito es un grupo de una o más búsquedas guardadas que se usa para limitar el ámbito de la solución a equipos específicos. Para acceder a las configuraciones de ámbito, en la cuenta de Automation, en **Recursos relacionados**, seleccione **Área de trabajo**. En el área de trabajo, en **Orígenes de datos del área de trabajo**, seleccione **Configuraciones de ámbito**.

Si el área de trabajo seleccionada no dispone aún de las soluciones Update Management ni Change Tracking, se crean las siguientes configuraciones de ámbito:

* **MicrosoftDefaultScopeConfig-ChangeTracking**

* **MicrosoftDefaultScopeConfig-Updates**

Si el área de trabajo seleccionada ya tiene la solución, esta no se vuelve a implementar y no se agrega la configuración de ámbito.

Seleccione los puntos suspensivos (**...**) en cualquiera de las configuraciones y, luego, seleccione **Editar**. En el panel **Editar configuración de ámbito**, seleccione **Seleccionar grupos de equipos**. En el panel **Grupos de equipos** se muestran las búsquedas guardadas que se usan para crear la configuración de ámbito.

## <a name="saved-searches"></a>Búsquedas guardadas

Cuando un equipo se agrega a las soluciones Update Management, Change Tracking o Inventory, se agrega a una de las dos búsquedas guardadas del área de trabajo. Las búsquedas guardadas son consultas que contienen los equipos que son el destino de estas soluciones.

Vaya a su área de trabajo. En **General**, seleccione **Búsquedas guardadas**. Las dos búsquedas guardadas que usan estas soluciones se muestran en la tabla siguiente:

|NOMBRE     |Categoría  |Alias  |
|---------|---------|---------|
|MicrosoftDefaultComputerGroup     |  ChangeTracking       | ChangeTracking__MicrosoftDefaultComputerGroup        |
|MicrosoftDefaultComputerGroup     | Actualizaciones        | Updates__MicrosoftDefaultComputerGroup         |

Seleccione cualquiera de las búsquedas guardadas para ver la consulta que se usa para rellenar el grupo. La siguiente imagen muestra la consulta y sus resultados:

![Búsquedas guardadas](media/automation-onboard-solutions-from-vm/logsearch.png)

## <a name="next-steps"></a>Pasos siguientes

Continúe con los tutoriales de las soluciones para aprender a usarlas:

* [Tutorial: administración de las actualizaciones de la máquina virtual](automation-tutorial-update-management.md)
* [Tutorial: identificación del software en una máquina virtual](automation-tutorial-installed-software.md)
* [Tutorial: solución de problemas de los cambios en una máquina virtual](automation-tutorial-troubleshoot-changes.md)
