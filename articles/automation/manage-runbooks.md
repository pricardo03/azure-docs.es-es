---
title: Administración de runbooks en Azure Automation
description: En este artículo se describe cómo administrar runbooks en Azure Automation.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 02/14/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 5bb52e0547ed9bc18d67370ffb9db35942212aab
ms.sourcegitcommit: 24906eb0a6621dfa470cb052a800c4d4fae02787
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/27/2019
ms.locfileid: "56887597"
---
# <a name="manage-runbooks-in-azure-automation"></a>Administración de runbooks en Azure Automation

Para agregar un runbook a Azure Automation, puede [crear uno](#create-a-runbook) o importar un runbook existente desde un archivo o desde la [Galería de runbooks](automation-runbook-gallery.md). Este artículo ofrece información sobre cómo crear e importar runbooks de un archivo.  Puede obtener todos los detalles sobre el acceso a los módulos y los runbooks de la comunidad en [Galería de runbooks y módulos para Azure Automation](automation-runbook-gallery.md).

## <a name="create-a-runbook"></a>Crear un runbook

Puede crear un nuevo runbook en Azure Automation mediante uno de los portales de Azure o Windows PowerShell. Cuando se haya creado el runbook, puede editarlo con la información de [Aprendizaje del flujo de trabajo de Windows PowerShell](automation-powershell-workflow.md) y [Creación gráfica en Azure Automation](automation-graphical-authoring-intro.md).

### <a name="create-a-runbook-in-the-azure-portal"></a>Creación de un runbook en Azure Portal

1. En Azure Portal, abra su cuenta de Automation.
2. Desde el centro, seleccione **Runbooks** para abrir la lista de runbooks.
3. Haga clic en el botón **Agregar un Runbook** y, después, en **Crear un runbook nuevo**.
4. Escriba un **Nombre** para el runbook y seleccione su [Tipo](automation-runbook-types.md). El nombre del runbook debe empezar por una letra y puede tener letras, números, caracteres de subrayado y guiones.
5. Haga clic en **Crear** para crear el runbook y abra el editor.

### <a name="create-a-runbook-with-powershell"></a>Creación de un runbook con PowerShell

Puede usar el cmdlet [New-AzureRmAutomationRunbook](/powershell/module/azurerm.automation/new-azurermautomationrunbook) para crear un [runbook de flujo de trabajo de PowerShell](automation-runbook-types.md#powershell-workflow-runbooks) vacío. Use el parámetro **Type** para especificar uno de los cuatro tipos de runbook.

Los comandos de ejemplo siguientes muestran cómo crear un nuevo runbook vacío.

```azurepowershell-interactive
New-AzureRmAutomationRunbook -AutomationAccountName MyAccount `
-Name NewRunbook -ResourceGroupName MyResourceGroup -Type PowerShell
```

## <a name="import-a-runbook"></a>Importación de un runbook

Puede crear un nuevo runbook en Azure Automation mediante la importación de un script de PowerShell o un flujo de trabajo de PowerShell (extensión. ps1), un runbook gráfico exportado (.graphrunbook) o un script de Python 2 (extensión .py).  Necesita especificar el [tipo de runbook](automation-runbook-types.md) que se creará durante la importación teniendo en cuenta las consideraciones siguientes.

* Un archivo `.graphrunbook` solo se puede importar en un [nuevo runbook gráfico](automation-runbook-types.md#graphical-runbooks) y solo se pueden crear runbooks gráficos a partir de un archivo `.graphrunbook`.
* Un archivo `.ps1` que contiene un flujo de trabajo de PowerShell solo se puede importar en un [runbook de flujo de trabajo de PowerShell](automation-runbook-types.md#powershell-workflow-runbooks).  Si el archivo contiene varios flujos de trabajo de PowerShell, se producirá un error en la importación. Debe guardar cada flujo de trabajo en su propio archivo e importar cada uno por separado.
* Un archivo `.ps1` que no contenga un flujo de trabajo se puede importar en un [runbook de PowerShell](automation-runbook-types.md#powershell-runbooks) o en un [runbook de flujo de trabajo de PowerShell](automation-runbook-types.md#powershell-workflow-runbooks).  Si se importa en un runbook de flujo de trabajo de PowerShell, se convertirá en un flujo de trabajo y se incluirán comentarios en el runbook especificando los cambios realizados.

### <a name="to-import-a-runbook-from-a-file-with-the-azure-portal"></a>Para importar un runbook desde un archivo con Azure Portal

Puede usar el siguiente procedimiento para importar un archivo de script en Azure Automation.  

> [!NOTE]
> Tenga en cuenta que solo puede importar un archivo. ps1 en un runbook de flujo de trabajo de PowerShell mediante el portal.

1. En Azure Portal, abra su cuenta de Automation.
2. Desde el centro, seleccione **Runbooks** para abrir la lista de runbooks.
3. Haga clic en el botón **Agregar un runbook** y, después, en **Importar**.
4. Haga clic en **Archivo de runbook** para seleccionar el archivo que se va a importar.
5. Si el campo **Nombre** está habilitado, tiene la opción de cambiarlo.  El nombre del runbook debe empezar por una letra y puede tener letras, números, caracteres de subrayado y guiones.
6. El [tipo de runbook](automation-runbook-types.md) se seleccionará automáticamente, pero puede cambiarlo después de tomar en cuenta las restricciones aplicables. 
7. El runbook nuevo aparece en la lista de runbooks de la cuenta de Automation.
8. Debe [publicar el runbook](#publish-a-runbook) para poder ejecutarlo.

> [!NOTE]
> Después de importar un runbook gráfico o un runbook gráfico de flujo de trabajo de PowerShell, tiene la opción de convertir a otro tipo si así lo desea. No es posible convertir a runbook de texto.

### <a name="to-import-a-runbook-from-a-script-file-with-windows-powershell"></a>Para importar un runbook de un archivo de script con Windows PowerShell

Puede usar el cmdlet [Import-AzureRMAutomationRunbook](https://docs.microsoft.com/powershell/module/azurerm.automation/import-azurermautomationrunbook) para importar un archivo de script como un runbook de flujo de trabajo de PowerShell en borrador. Si ya existe el runbook, la importación dará error, a menos que utilice el parámetro *-Force*.

Los comandos de ejemplo siguientes muestran cómo importar un archivo de script en un runbook.

```azurepowershell-interactive
$automationAccountName =  "AutomationAccount"
$runbookName = "Sample_TestRunbook"
$scriptPath = "C:\Runbooks\Sample_TestRunbook.ps1"
$RGName = "ResourceGroup"

Import-AzureRMAutomationRunbook -Name $runbookName -Path $scriptPath `
-ResourceGroupName $RGName -AutomationAccountName $automationAccountName `
-Type PowerShellWorkflow
```

## <a name="test-a-runbook"></a>Prueba de un runbook

Cuando se prueba un runbook, se ejecuta la [versión de borrador](#publish-a-runbook) y se completan todas las acciones que realiza. No se crea ningún historial de trabajos, pero los flujos de [salida](automation-runbook-output-and-messages.md#output-stream) y los de [error y advertencia](automation-runbook-output-and-messages.md#message-streams) se muestran en el panel de salida de la prueba. Los mensajes del [flujo detallado](automation-runbook-output-and-messages.md#message-streams) solo se muestran en el panel de salida si la [variable $VerbosePreference](automation-runbook-output-and-messages.md#preference-variables) está establecida en Continue.

Aun cuando se ejecuta la versión de borrador, el runbook se ejecuta con normalidad y realiza las acciones correspondientes en los recursos del entorno. Por este motivo, solo debe probar runbooks en recursos no pertenecientes a entornos de producción.

El procedimiento para probar cada [tipo de runbook](automation-runbook-types.md) es el mismo, y no hay diferencias entre realizar las pruebas en el editor de texto o en el editor gráfico de Azure Portal.  

1. Abra la versión de borrador del runbook en el [editor de texto](automation-edit-textual-runbook.md) o el [editor gráfico](automation-graphical-authoring-intro.md).
1. Haga clic en el botón **Probar** para abrir la página Prueba.
1. Si el runbook tiene parámetros, se enumerarán en el panel izquierdo, donde puede proporcionar los valores que se usarán para la prueba.
1. Si quiere ejecutar la prueba en un [Hybrid Runbook Worker](automation-hybrid-runbook-worker.md), cambie los **Parámetros de ejecución** a **Hybrid Worker** y seleccione el nombre del grupo de destino.  De lo contrario, mantenga el valor predeterminado **Azure** para ejecutar la prueba en la nube.
1. Haga clic en el botón **Iniciar** para iniciar la prueba.
1. Si el runbook es de tipo [Flujo de trabajo de PowerShell](automation-runbook-types.md#powershell-workflow-runbooks) o [Gráfico](automation-runbook-types.md#graphical-runbooks), puede detenerlo o suspenderlo durante la prueba con los botones situados bajo el panel de salida. Cuando se suspende el runbook, este completa la actividad que está realizando en ese momento antes de suspenderse. Una vez suspendido, puede detener o reiniciar el runbook.
1. Revise la salida del runbook en el panel de salida.

## <a name="publish-a-runbook"></a>Publicación de un runbook

Cuando cree o importe un runbook nuevo, debe publicarlo para poder ejecutarlo.  Cada runbook de Automation tiene una versión de borrador y una versión publicada. Solo es posible ejecutar la versión publicada y solo es posible editar la versión de borrador. Los cambios realizados en la versión de borrador no afectan la versión publicada. Cuando la versión de borrador deba estar lista para su disponibilidad, puede publicarla, lo que sobrescribirá la versión publicada con la versión de borrador.

### <a name="azure-portal"></a>Azure Portal

1. Abra el runbook en Azure Portal.
2. Haga clic en el botón **Editar** .
3. Haga clic en el botón **Publicar** y, después, haga clic en **Sí** en el mensaje de comprobación.

### <a name="powershell"></a>PowerShell

Puede usar el cmdlet [Publish-AzureRmAutomationRunbook](/powershell/module/azurerm.automation/publish-azurermautomationrunbook) para publicar un runbook con Windows PowerShell. Los comandos de ejemplo siguientes muestran cómo crear un runbook de ejemplo.

```azurepowershell-interactive
$automationAccountName =  "AutomationAccount"
$runbookName = "Sample_TestRunbook"
$RGName = "ResourceGroup"

Publish-AzureRmAutomationRunbook -AutomationAccountName $automationAccountName `
-Name $runbookName -ResourceGroupName $RGName
```

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre las ventajas de la Galería de runbooks y módulos de PowerShell, vea [Galerías de runbooks y módulos para Azure Automation](automation-runbook-gallery.md).
* Para más información acerca de la edición de runbooks de PowerShell y flujo de trabajo de PowerShell, consulte [Edición de runbooks de texto en Azure Automation](automation-edit-textual-runbook.md)
* Para más información sobre la creación de runbooks de gráficos, consulte [Creación gráfica en Azure Automation](automation-graphical-authoring-intro.md)
