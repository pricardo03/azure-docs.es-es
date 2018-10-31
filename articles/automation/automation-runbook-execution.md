---
title: Ejecución de un runbook en Azure Automation
description: Describe los detalles de cómo se procesa un runbook en Azure Automation.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 10/17/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 2b1a6e2921fdaf9ede1184cfc02c3f61f63c60ac
ms.sourcegitcommit: b4a46897fa52b1e04dd31e30677023a29d9ee0d9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2018
ms.locfileid: "49393771"
---
# <a name="runbook-execution-in-azure-automation"></a>Ejecución de un runbook en Azure Automation

Cuando se inicia un runbook en Azure Automation, se crea un trabajo. Un trabajo es una instancia única de ejecución de un runbook. Un trabajador de Azure Automation está asignado para ejecutar cada trabajo. Aunque los trabajadores los comparten varias cuentas de Azure, los trabajos de diferentes cuentas de Automation están aislados entre sí. No tiene el control sobre qué servicios de trabajo solicitar para el trabajo. Un runbook individual puede tener muchos trabajos que se ejecutan al mismo tiempo. Se puede reutilizar el entorno de ejecución para los trabajos de la misma cuenta de Automation. Al ver la lista de runbooks en Azure Portal, se muesta el estado de todos los trabajos iniciados por cada runbook. Puede ver la lista de trabajos de cada runbook para hacer un seguimiento de su estado. Para obtener una descripción de los distintos estados de trabajo, consulte [Estados del trabajo](#job-statuses).

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-dsr-and-stp-note.md)]

En el siguiente diagrama se muestra el ciclo de vida de un trabajo de runbook para [Runbooks gráficos](automation-runbook-types.md#graphical-runbooks) y [Runbooks del flujo de trabajo de PowerShell](automation-runbook-types.md#powershell-workflow-runbooks).

![Estados de trabajo: flujo de trabajo de PowerShell](./media/automation-runbook-execution/job-statuses.png)

En el siguiente diagrama se muestra el ciclo de vida de un trabajo de runbook para [runbooks de PowerShell](automation-runbook-types.md#powershell-runbooks).

![Estados de trabajo: script de PowerShell](./media/automation-runbook-execution/job-statuses-script.png)

Los trabajos tienen acceso a los recursos de Azure mediante una conexión a la suscripción de Azure. Solo tienen acceso a los recursos de su centro de datos si estos recursos están accesibles desde la nube pública.

## <a name="job-statuses"></a>Estados del trabajo

En la tabla siguiente se describen los diferentes estados posibles para un trabajo. PowerShell tiene dos tipos de errores, errores de terminación y de no terminación. Los primeros cambian el estado del runbook a **Failed** (Con error) si se producen. Los segundos permiten que el script continúe después de que sucedan. Un ejemplo de un error de no terminación es utilizar el cmdlet `Get-ChildItem` con una ruta de acceso que no existe. PowerShell se ve que la ruta de acceso no existe, genera un error y pasa a la carpeta siguiente. El error no establecería el estado de runbook en **Failed** (Con error) y se podría marcarse como **Completed** (Completado). Para forzar a un runbook a detenerse ante un error de no terminación, puede usar `-ErrorAction Stop` en el cmdlet.

| Status | Descripción |
|:--- |:--- |
| Completed |El trabajo se completó correctamente. |
| Con error |Para [Runbooks del flujo de trabajo de PowerShell](automation-runbook-types.md), no se pudo compilar el runbook. Para [Runbooks de script de PowerShell](automation-runbook-types.md), no se pudo iniciar el runbook o el trabajo tenía una excepción. |
| Error, esperando recursos |Se produjo un error en el trabajo porque ha alcanzado el límite de [distribución equilibrada](#fair-share) tres veces y se ha iniciado en el mismo punto de control o en el inicio del runbook cada vez. |
| En cola |El trabajo espera que los recursos en un trabajo de Automation estén disponible para que se puede iniciar. |
| Iniciando |El trabajo se ha asignado a un trabajador y el sistema lo está iniciando. |
| Reanudando |El sistema está reanudando el trabajo después de que se suspendió. |
| En ejecución |El trabajo se está ejecutando. |
| En ejecución, esperando recursos |El trabajo se ha descargado porque ha alcanzado el límite de [distribución equilibrada](#fair-share) . Se reanuda en breve desde su último punto de control. |
| Stopped |El trabajo lo detuvo el usuario antes de completarse. |
| Deteniéndose |El sistema está deteniendo el trabajo. |
| Suspended |El trabajo lo ha suspendido el usuario, el sistema o un comando en el runbook. Si un runbook no tiene un punto de control, se inicia desde el principio del runbook. Si lo tiene, puede volver a empezar y reanudarse desde su último punto de comprobación. El sistema solo suspende el runbook cuando se produce una excepción. De forma predeterminada, se establece ErrorActionPreference en **Continuar**, lo que significa que el trabajo sigue ejecutándose en un error. Si se establece esta variable de preferencia en **Detener**, se suspende el trabajo en un error. Solo se aplica a [Runbooks de flujo de trabajo de PowerShell y gráficos](automation-runbook-types.md) . |
| Suspendiendo |El sistema intenta suspender el trabajo a petición del usuario. El runbook debe alcanzar su siguiente punto de control antes de que se pueda suspender. Si ya ha pasado su último punto de control, se completa antes de que se pueda suspender. Solo se aplica a [Runbooks de flujo de trabajo de PowerShell y gráficos](automation-runbook-types.md) . |

## <a name="viewing-job-status-from-the-azure-portal"></a>Visualización de un estado de trabajo desde el portal de Azure

Puede ver un resumen del estado de todos los trabajos del runbook o profundizar en los detalles de un trabajo de runbook específico en Azure Portal. También puede configurar la integración con un área de trabajo de Log Analytics para reenviar los flujos de trabajo y el estado del trabajo del runbook. Para más información sobre la integración con Log Analytics, consulte [Reenvío del estado de un trabajo y de transmisiones de trabajos desde Automation a Log Analytics (OMS)](automation-manage-send-joblogs-log-analytics.md).

### <a name="automation-runbook-jobs-summary"></a>Resumen de trabajos de runbook de Automation

A la derecha de la cuenta de Automation, puede ver un resumen de todos los trabajos de runbook en el icono **Estadísticas de trabajo**.

![Icono Estadísticas de trabajo](./media/automation-runbook-execution/automation-account-job-status-summary.png)

Este icono muestra un contador y una representación gráfica del estado de trabajo de todos los trabajos ejecutados.

Al hacer clic en el icono aparece la página **Trabajos**, que incluye una lista resumida de todos los trabajos ejecutados. Esta página muestra el estado, y las horas de inicio y finalización.

![Página Trabajos de la cuenta de Automation](./media/automation-runbook-execution/automation-account-jobs-status-blade.png)

Para filtrar la lista de trabajos, seleccione **Filtrar trabajos** y filtre por un runbook concreto, el estado de un trabajo o desde la lista desplegable, el intervalo de tiempo en que se busca.

![Filtrar estado del trabajo](./media/automation-runbook-execution/automation-account-jobs-filter.png)

Como alternativa, puede ver los detalles resumidos de los trabajos de un runbook concreto; para ello, selecciónelo en la página **Runbooks** de la cuenta de Automation y, después, seleccione el icono **Trabajos**. Esta acción abre la página **Trabajos**, en la que puede hacer clic en el registro del trabajo para ver sus detalles y salida.

![Página Trabajos de la cuenta de Automation](./media/automation-runbook-execution/automation-runbook-job-summary-blade.png)

### <a name="job-summary"></a>Resumen del trabajo

Puede ver una lista de todos los trabajos que se han creado para un runbook determinado y su estado más reciente. Puede filtrar esta lista por estado del trabajo y el intervalo de fechas del último cambio del trabajo. Para ver su información detallada y la salida, haga clic en el nombre de un trabajo. La vista detallada del trabajo incluye los valores para los parámetros de runbook que se proporcionaron con ese trabajo.

Puede utilizar los pasos siguientes para ver los trabajos de un runbook.

1. En Azure Portal, seleccione **Automation** y después elija el nombre de una cuenta de Automation.
2. En el centro, seleccione **Runbooks** y, después, en la página **Runbooks**, seleccione un runbook de la lista.
3. En la página del runbook seleccionado, haga clic en el icono **Trabajos**.
4. Haga clic en uno de los trabajos de la lista y en la página de detalles del trabajo del runbook podrá ver sus detalles y salida.

## <a name="retrieving-job-status-using-windows-powershell"></a>Recuperación del estado del trabajo con Windows PowerShell

Puede usar [Get-AzureRmAutomationJob](https://docs.microsoft.com/powershell/module/azurerm.automation/get-azurermautomationjob) para recuperar los trabajos creados para un runbook y los detalles de un trabajo determinado. Si inicia un runbook con Windows PowerShell mediante el uso de [Start-AzureRmAutomationRunbook](https://docs.microsoft.com/powershell/module/azurerm.automation/start-azurermautomationrunbook), devuelve el trabajo resultante. Use la salida [Get-AzureRmAutomationJobOutput](https://docs.microsoft.com/powershell/module/azurerm.automation/get-azurermautomationjoboutput) para obtener la salida de un trabajo.

Los comandos de ejemplo siguientes recuperan el último trabajo para un runbook de ejemplo y muestran su estado, los valores proporcionados para los parámetros de runbook y la salida del trabajo.

```azurepowershell-interactive
$job = (Get-AzureRmAutomationJob –AutomationAccountName "MyAutomationAccount" `
–RunbookName "Test-Runbook" -ResourceGroupName "ResourceGroup01" | sort LastModifiedDate –desc)[0]
$job.Status
$job.JobParameters
Get-AzureRmAutomationJobOutput -ResourceGroupName "ResourceGroup01" `
–AutomationAccountName "MyAutomationAcct" -Id $job.JobId –Stream Output
```

El ejemplo siguiente recupera la salida de un trabajo específico y devuelve cada registro. En el caso de que se produjera una excepción para uno de los registros, se escribiría la excepción en lugar del valor. Este comportamiento resulta útil, ya que las excepciones pueden proporcionar información adicional que no se puede registrar normalmente durante la salida.

```azurepowershell-interactive
$output = Get-AzureRmAutomationJobOutput -AutomationAccountName <AutomationAccountName> -Id <jobID> -ResourceGroupName <ResourceGroupName> -Stream "Any"
foreach($item in $output)
{
    $fullRecord = Get-AzureRmAutomationJobOutputRecord -AutomationAccountName <AutomationAccountName> -ResourceGroupName <ResourceGroupName> -JobId <jobID> -Id $item.StreamRecordId
    if ($fullRecord.Type -eq "Error")
    {
        $fullRecord.Value.Exception
    }
    else
    {
    $fullRecord.Value
    }
}
```

## <a name="get-details-from-activity-log"></a>Obtención de detalles del registro de actividad

Detalles como la persona o la cuenta que inició el runbook se pueden recuperar del registro de actividad para la cuenta de Automation. El siguiente ejemplo de PowerShell proporciona el último usuario para ejecutar el runbook en cuestión:

```powershell-interactive
$SubID = "00000000-0000-0000-0000-000000000000"
$rg = "ResourceGroup01"
$AutomationAccount = "MyAutomationAccount"
$RunbookName = "Test-Runbook"
$JobResourceID = "/subscriptions/$subid/resourcegroups/$rg/providers/Microsoft.Automation/automationAccounts/$AutomationAccount/jobs"

Get-AzureRmLog -ResourceId $JobResourceID -MaxRecord 1 | Select Caller
```

## <a name="fair-share"></a>distribución equilibrada

Para compartir recursos entre todos los runbooks en la nube, Azure Automation descargará o detendrá temporalmente todos los trabajos que lleven más de tres horas en ejecución. Los trabajos de [runbooks basados en PowerShell](automation-runbook-types.md#powershell-runbooks) y [runbooks de Python](automation-runbook-types.md#python-runbooks) se detienen y no se reinician, y el estado del trabajo muestra Stopped (Detenido).

Para las tareas de larga ejecución, se recomienda usar un [Hybrid Runbook Worker](automation-hrw-run-runbooks.md#job-behavior). Las instancias de Hybrid Runbook Worker no están limitados por la distribución equilibrada y no tienen una limitación del tiempo que se puede ejecutar un runbook. Los demás [límites](../azure-subscription-service-limits.md#automation-limits) de trabajo se aplican a los espacios aislados de Azure y a Hybrid Runbook Workers. Aunque las instancias de Hybrid Runbook Worker no están restringidos por el límite de distribución equilibrada de 3 horas, los runbooks que se ejecutan en ellos deben desarrollarse para admitir comportamientos de reinicio si se producen problemas inesperados en la infraestructura local.

Otra opción es optimizar el runbook mediante runbooks secundarios. Si el runbook secundario recorre en bucle la misma función en varios recursos, como una operación de base de datos en varias bases de datos, la función se puede mover a un [runbook secundario](automation-child-runbooks.md) y se le puede llamar con el cmdlet [Start-AzureRMAutomationRunbook](/powershell/module/azurerm.automation/start-azurermautomationrunbook). Cada uno de estos runbooks secundarios se ejecutan en paralelo en procesos independientes, lo que reduce el tiempo que el runbook principal tarda en completarse. El cmdlet [Get-AzureRmAutomationJob](/powershell/module/azurerm.automation/Get-AzureRmAutomationJob) de su runbook se puede usar para comprobar el estado del trabajo de cada elemento secundario si hay operaciones que deben realizarse después de que se complete el runbook secundario.

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre los distintos métodos que se pueden usar para iniciar un runbook en Azure Automation, consulte [Inicio de un runbook en Azure Automation](automation-starting-a-runbook.md)
