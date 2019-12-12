---
title: Edición de runbooks de texto en Azure Automation
description: En este artículo, se proporcionan diferentes procedimientos para trabajar con runbooks de PowerShell y de flujo de trabajo de PowerShell en Azure Automation mediante el editor de texto.
services: automation
ms.service: automation
ms.subservice: process-automation
author: mgoedtel
ms.author: magoedte
ms.date: 08/01/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 5a7eec0a7650f9c8e04a8d1062d32b6feb7d1d99
ms.sourcegitcommit: c38a1f55bed721aea4355a6d9289897a4ac769d2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2019
ms.locfileid: "74850863"
---
# <a name="editing-textual-runbooks-in-azure-automation"></a>Edición de runbooks de texto en Azure Automation

El editor de texto de Azure Automation sirve para editar [runbooks de PowerShell](automation-runbook-types.md#powershell-runbooks) y [runbooks del flujo de trabajo de PowerShell](automation-runbook-types.md#powershell-workflow-runbooks). Además, incluye las características típicas de otros editores de código, como IntelliSense y codificación en colores, junto con otras características especiales que le ayudarán a acceder a recursos habituales de runbooks. En este artículo, se proporcionan pasos detallados para realizar diferentes funciones con este editor.

El editor de texto incluye una característica para insertar código para cmdlets, los recursos y los runbooks secundarios en un runbook. En lugar de escribir en el código personalmente, puede seleccionar entre una lista de recursos disponibles para que se inserte el código adecuado en el runbook.

Cada runbook de Azure Automation tiene dos versiones: una de borrador y otra publicada. Edite la versión de borrador del runbook y después publíquela para que pueda ejecutarse. No se puede editar la versión publicada. Para más información, consulte [Publicación de un Runbook](manage-runbooks.md#publish-a-runbook).

Para trabajar con [runbooks gráficos](automation-runbook-types.md#graphical-runbooks), consulte [Creación gráfica en Azure Automation](automation-graphical-authoring-intro.md).

## <a name="to-edit-a-runbook-with-the-azure-portal"></a>Para editar un runbook con el Portal de Azure

Use el siguiente procedimiento para abrir un runbook para su edición en el editor de texto.

1. En el Portal de Azure, seleccione su cuenta de automatización.
2. En **AUTOMATIZACIÓN DE PROCESOS**, seleccione **Runbooks** para abrir la lista de runbooks.
3. Seleccione el runbook que desea editar y después en el botón **Editar**.
4. Edite el runbook.
5. Haga clic en **Guardar** cuando termine las modificaciones.
6. Haga clic en **Publicar** si desea que la última versión de borrador del runbook se publique.

### <a name="to-insert-a-cmdlet-into-a-runbook"></a>Para insertar un cmdlet en un runbook

1. En el lienzo del editor de texto, coloque el cursor donde desee colocar el cmdlet.
2. Expanda el nodo **Cmdlets** en el control Biblioteca.
3. Expanda el módulo que contiene el cmdlet que desea usar.
4. Haga clic con el botón derecho en el cmdlet para insertarlo y seleccione **Agregar al lienzo**. Si el cmdlet tiene más de un conjunto de parámetros, se agregará el conjunto predeterminado. También puede expandir el cmdlet para seleccionar un conjunto de parámetros diferente.
5. Se inserta el código para el cmdlet con toda su lista de parámetros.
6. Proporcione un valor adecuado en lugar del tipo de datos entre corchetes, <>, para los parámetros necesarios. Elimine cualquier parámetro que no sea necesario.

### <a name="to-insert-code-for-a-child-runbook-into-a-runbook"></a>Para insertar código de runbook secundario en un runbook

1. En el lienzo del editor de texto, coloque el cursor donde desee colocar el código para el [runbook secundario](automation-child-runbooks.md).
2. Expanda el nodo **Runbooks** en el control Biblioteca.
3. Haga clic con el botón derecho en el runbook para insertarlo y seleccione **Agregar al lienzo**.
4. Se inserta el código para el runbook secundario con los marcadores de posición de los parámetros de runbook.
5. Reemplace los marcadores de posición por valores adecuados para cada parámetro.

### <a name="to-insert-an-asset-into-a-runbook"></a>Para insertar un recurso en un runbook

1. En el lienzo del editor de texto, coloque el cursor donde desee colocar el código para el runbook secundario.
2. Expanda el nodo **Recursos** en el control Biblioteca.
3. Expanda el nodo para el tipo de recurso que desee.
4. Haga clic con el botón derecho en el recurso para insertarlo y seleccione **Agregar al lienzo**. Para [recursos variables](automation-variables.md), seleccione **Agregar "Obtener variable" a lienzo** o **Agregar "Establecer variable" a lienzo** en función de si desea obtener o establecer la variable.
5. El código para el recurso se inserta en el runbook.

## <a name="to-edit-an-azure-automation-runbook-using-windows-powershell"></a>Para editar un runbook de Azure Automation mediante Windows PowerShell

Para editar un runbook con Windows PowerShell, use el editor de su elección y guárdelo en un archivo `.ps1`. Puede usar el cmdlet [Export-AzureRmAutomationRunbook](/powershell/module/AzureRM.Automation/Export-AzureRmAutomationRunbook) para recuperar el contenido del runbook y después el cmdlet [Import-AzureRmAutomationRunbook](/powershell/module/AzureRM.Automation/import-azurermautomationrunbook) para reemplazar el borrador del runbook existente por el modificado.

### <a name="to-retrieve-the-contents-of-a-runbook-using-windows-powershell"></a>Para recuperar el contenido de un runbook mediante Windows PowerShell

En los siguientes comandos de ejemplo, se muestra cómo se recupera el script para un runbook y se guarda en un archivo de script. En este ejemplo, se recupera la versión de borrador. También es posible recuperar la versión publicada del runbook, aunque esta no se puede cambiar.

```powershell-interactive
$resourceGroupName = "MyResourceGroup"
$automationAccountName = "MyAutomatonAccount"
$runbookName = "Hello-World"
$scriptFolder = "c:\runbooks"

Export-AzureRmAutomationRunbook -Name $runbookName -AutomationAccountName $automationAccountName -ResourceGroupName $resourceGroupName -OutputFolder $scriptFolder -Slot Draft
```

### <a name="to-change-the-contents-of-a-runbook-using-windows-powershell"></a>Para cambiar el contenido de un runbook mediante Windows PowerShell

En los siguientes comandos de ejemplo, se muestra cómo reemplazar el contenido existente de un runbook por el de un archivo de script. Se trata del mismo procedimiento de ejemplo que en la sección [To import a runbook from a script file with Windows PowerShell](manage-runbooks.md#import-a-runbook) (Importación de un runbook a partir de un archivo de script con Windows PowerShell).

```powershell-interactive
$resourceGroupName = "MyResourceGroup"
$automationAccountName = "MyAutomatonAccount"
$runbookName = "Hello-World"
$scriptFolder = "c:\runbooks"

Import-AzureRmAutomationRunbook -Path "$scriptfolder\Hello-World.ps1" -Name $runbookName -Type PowerShell -AutomationAccountName $automationAccountName -ResourceGroupName $resourceGroupName -Force
Publish-AzureRmAutomationRunbook -Name $runbookName -AutomationAccountName $automationAccountName -ResourceGroupName $resourceGroupName
```

## <a name="related-articles"></a>Artículos relacionados

* [Administración de runbooks en Azure Automation](manage-runbooks.md)
* [Aprendizaje del flujo de trabajo de Windows PowerShell](automation-powershell-workflow.md)
* [Creación gráfica en Azure Automation](automation-graphical-authoring-intro.md)
* [Certificates](automation-certificates.md)
* [Conexiones](automation-connections.md)
* [Credenciales](automation-credentials.md)
* [Programaciones](automation-schedules.md)
* [Variables](automation-variables.md)

