---
title: Ejecución de un runbook en Azure Automation
description: Describe los detalles de cómo se procesa un runbook en Azure Automation.
services: automation
ms.subservice: process-automation
ms.date: 04/04/2019
ms.topic: conceptual
ms.openlocfilehash: c97e10d2785b7dc1a438c95dca9be94fcef82f94
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2020
ms.locfileid: "76714847"
---
# <a name="runbook-execution-in-azure-automation"></a>Ejecución de un runbook en Azure Automation

Cuando se inicia un runbook en Azure Automation, se crea un trabajo. Un trabajo es una instancia única de ejecución de un runbook. Un trabajador de Azure Automation está asignado para ejecutar cada trabajo. Aunque los trabajadores los comparten varias cuentas de Azure, los trabajos de diferentes cuentas de Automation están aislados entre sí. No tiene el control sobre qué servicios de trabajo solicitar para el trabajo. Un runbook individual puede tener muchos trabajos que se ejecutan al mismo tiempo. Se puede reutilizar el entorno de ejecución para los trabajos de la misma cuenta de Automation. Cuantos más trabajos se ejecuten al mismo tiempo, más a menudo se podrán enviar al mismo espacio aislado. Los trabajos que se ejecutan en el mismo proceso de espacio aislado pueden afectarse entre sí; un ejemplo es ejecutar el cmdlet `Disconnect-AzureRMAccount`. Ejecutar este cmdlet desconectaría cada trabajo de runbook en el proceso de espacio aislado compartido. Al ver la lista de runbooks en Azure Portal, se muesta el estado de todos los trabajos iniciados por cada runbook. Puede ver la lista de trabajos de cada runbook para hacer un seguimiento de su estado. Los registros de trabajo se almacenan durante 30 días como máximo. Para obtener una descripción de los distintos estados de trabajo, consulte [Estados del trabajo](#job-statuses).

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-dsr-and-stp-note.md)]

En el siguiente diagrama se muestra el ciclo de vida de un trabajo de runbook para [runbooks de PowerShell](automation-runbook-types.md#powershell-runbooks), [runbooks gráficos](automation-runbook-types.md#graphical-runbooks) y [runbooks del flujo de trabajo de PowerShell](automation-runbook-types.md#powershell-workflow-runbooks).

![Estados de trabajo: flujo de trabajo de PowerShell](./media/automation-runbook-execution/job-statuses.png)

Los trabajos tienen acceso a los recursos de Azure mediante una conexión a la suscripción de Azure. Solo tienen acceso a los recursos de su centro de datos si estos recursos están accesibles desde la nube pública.

## <a name="where-to-run-your-runbooks"></a>Dónde ejecutar los runbooks

Los runbooks de Azure Automation puede ejecutarse en un espacio aislado de Azure o una instancia de [Hybrid Runbook Worker](automation-hybrid-runbook-worker.md). Un espacio aislado es un entorno compartido en Azure que puede usarse en varios trabajos. Los trabajos con el mismo espacio aislado están sujetos a las limitaciones de recursos de dicho espacio. La característica Hybrid Runbook Workers puede ejecutar runbooks directamente en el equipo que hospeda el rol y en los recursos del entorno para administrar esos recursos locales. Los runbooks se almacenan y administran en Azure Automation y después se entregan a uno o más equipos asignados. La mayoría de los runbooks se pueden ejecutar fácilmente en los espacios aislados de Azure. Existen escenarios específicos donde se puede recomendar la elección de una instancia de Hybrid Runbook a través de un espacio aislado de Azure para ejecutar el runbook. Consulte la tabla siguiente para ver una lista de algunos escenarios de ejemplo:

|Tarea|Mejor opción|Notas|
|---|---|---|
|Integración con recursos de Azure|Espacio aislado de Azure|Hospedado en Azure, la autenticación es más sencilla. Si usa una instancia de Hybrid Runbook Worker en una máquina virtual de Azure, puede usar [identidades administradas para recursos de Azure](automation-hrw-run-runbooks.md#managed-identities-for-azure-resources).|
|Rendimiento óptimo para administrar recursos de Azure|Espacio aislado de Azure|El script se ejecuta en el mismo entorno, que a su vez tiene menos latencia.|
|Minimizar los costos operativos|Espacio aislado de Azure|No hay sobrecarga de proceso, no hay necesidad de una máquina virtual|
|Script de larga duración|Hybrid Runbook Worker|Los espacios aislados de Azure tienen [limitación en los recursos](../azure-resource-manager/management/azure-subscription-service-limits.md#automation-limits)|
|Interactuar con servicios locales|Hybrid Runbook Worker|Puede tener acceso directo a la máquina host|
|Requerir software de terceros y archivos ejecutables|Hybrid Runbook Worker|Usted administra el sistema operativo y puede instalar software|
|Supervisar un archivo o carpeta con un runbook|Hybrid Runbook Worker|Usar una [tarea de monitor](automation-watchers-tutorial.md) en una instancia de Hybrid Runbook Worker|
|Script que consume muchos recursos|Hybrid Runbook Worker| Los espacios aislados de Azure tienen [limitación en los recursos](../azure-resource-manager/management/azure-subscription-service-limits.md#automation-limits)|
|Uso de módulos con requisitos específicos| Hybrid Runbook Worker|Ejemplos:</br> **WinSCP**: dependencia de winscp.exe </br> **IISAdministration**: necesita IIS para habilitarse|
|Instalar el módulo que requiere el instalador|Hybrid Runbook Worker|Los módulos del espacio aislado deben poderse copiar.|
|Uso de runbooks o módulos que requieren .NET Framework que no sea 4.7.2|Hybrid Runbook Worker|Los espacios aislados de Automation tienen .NET Framework 4.7.2, y no hay ninguna manera de actualizarlo|
|Scripts que requieren elevación|Hybrid Runbook Worker|Los espacios aislados no permiten elevación. Para resolver este problema, use una instancia de Hybrid Runbook Worker; además, puede desactivar UAC y usar `Invoke-Command` al ejecutar el comando que requiere elevación.|
|Scripts que requieren acceso a WMI|Hybrid Runbook Worker|Los trabajos que se ejecutan en espacios aislados en la nube [no tienen acceso a WMI](#device-and-application-characteristics).|

## <a name="runbook-behavior"></a>Comportamiento del runbook

Los runbooks se ejecutan según la lógica que se define dentro de ellos. Si se interrumpe un runbook, se reinicia desde el principio. Este comportamiento requiere que los runbooks se escriban de forma que puedan reiniciarse en caso de producirse problemas transitorios.

Es posible que los trabajos de PowerShell iniciados desde un runbook ejecutado en un espacio aislado de Azure no se puedan ejecutar en modo de lenguaje completo. Para más información sobre los modos de lenguaje de PowerShell, consulte los [modos de lenguaje de PowerShell](/powershell/module/microsoft.powershell.core/about/about_language_modes). Para más información sobre cómo interactuar con los trabajos de Azure Automation, consulte [Recuperar el estado del trabajo mediante PowerShell](#retrieving-job-status-using-powershell).

### <a name="creating-resources"></a>Creación de recursos

Si el script crea los recursos, debe comprobar que el recurso ya exista antes de intentar volver a crearlo. El ejemplo siguiente es de carácter básico:

```powershell
$vmName = "WindowsVM1"
$resourceGroupName = "myResourceGroup"
$myCred = Get-AutomationPSCredential "MyCredential"
$vmExists = Get-AzureRmResource -Name $vmName -ResourceGroupName $resourceGroupName

if(!$vmExists)
    {
    Write-Output "VM $vmName does not exists, creating"
    New-AzureRmVM -Name $vmName -ResourceGroupName $resourceGroupName -Credential $myCred
    }
else
    {
    Write-Output "VM $vmName already exists, skipping"
    }
```

### <a name="time-dependent-scripts"></a>Scripts dependientes del tiempo

Es necesario tener cuidado al crear runbooks. Como se mencionó anteriormente, los runbooks deben crearse de forma que sean sólidos y puedan gestionar errores transitorios que pueden hacer que el runbook se reinicie o produzca error. Si un runbook produce error, se vuelve a intentar. Si un runbook se ejecuta con normalidad dentro de una restricción de tiempo, se debe implementar en él una lógica para comprobar el tiempo de ejecución y garantizar que operaciones como inicio, apagado o escalado horizontal se ejecutan únicamente en momentos específicos.

> [!NOTE]
> La hora local en el proceso de espacio aislado de Azure se establece en la hora UTC. Es necesario tener en cuenta este aspecto en los cálculos de fecha y hora de los runbooks.

### <a name="tracking-progress"></a>Seguimiento del progreso

Es una buena práctica crear runbooks que sean de naturaleza modular. Esto significa estructurar la lógica en el runbook de forma que se pueda reutilizar y reiniciar fácilmente. Una buena manera de garantizar que la lógica de un runbook se ejecuta correctamente si se han producido problemas es realizar el seguimiento del progreso del runbook. Algunas de las posibles maneras de seguir el progreso del runbook es mediante un origen externo, como cuentas de almacenamiento, una base de datos o archivos compartidos. Con el seguimiento externo del estado, puede crear lógica en el runbook para comprobar primero el estado de la última acción que realizó dicho runbook. Luego, en función de los resultados, puede continuar u omitir tareas específicas del runbook.

### <a name="prevent-concurrent-jobs"></a>Evitación de trabajos simultáneos

Algunos runbooks pueden comportarse de forma extraña si se ejecutan en varios trabajos al mismo tiempo. En este caso, es importante implementar lógica para comprobar si un runbook ya tiene un trabajo en ejecución. A continuación se muestra un ejemplo básico de cómo puede realizar este comportamiento:

```powershell
# Authenticate to Azure
$connection = Get-AutomationConnection -Name AzureRunAsConnection
Connect-AzureRmAccount -ServicePrincipal -Tenant $connection.TenantID `
-ApplicationID $connection.ApplicationID -CertificateThumbprint $connection.CertificateThumbprint

$AzureContext = Select-AzureRmSubscription -SubscriptionId $connection.SubscriptionID

# Check for already running or new runbooks
$runbookName = "<RunbookName>"
$rgName = "<ResourceGroupName>"
$aaName = "<AutomationAccountName>"
$jobs = Get-AzureRmAutomationJob -ResourceGroupName $rgName -AutomationAccountName $aaName -RunbookName $runbookName -AzureRmContext $AzureContext

# If then check to see if it is already running
$runningCount = ($jobs | ? {$_.Status -eq "Running"}).count

If (($jobs.status -contains "Running" -And $runningCount -gt 1 ) -Or ($jobs.Status -eq "New")) {
    # Exit code
    Write-Output "Runbook is already running"
    Exit 1
} else {
    # Insert Your code here
}
```

### <a name="working-with-multiple-subscriptions"></a>Trabajo con varias suscripciones

Al crear runbooks que se ocupan de varias suscripciones, el runbook debe usar el cmdlet [Disable-AzureRmContextAutosave](/powershell/module/azurerm.profile/disable-azurermcontextautosave) para garantizar que el contexto de autenticación no se recupera de otro runbook que podría estar en ejecución en el mismo espacio aislado. A continuación, debe usar el parámetro `-AzureRmContext` de sus cmdlets `AzureRM` y pasarle el contexto adecuado.

```powershell
# Ensures you do not inherit an AzureRMContext in your runbook
Disable-AzureRmContextAutosave –Scope Process

$Conn = Get-AutomationConnection -Name AzureRunAsConnection
Connect-AzureRmAccount -ServicePrincipal `
-Tenant $Conn.TenantID `
-ApplicationID $Conn.ApplicationID `
-CertificateThumbprint $Conn.CertificateThumbprint

$context = Get-AzureRmContext

$ChildRunbookName = 'ChildRunbookDemo'
$AutomationAccountName = 'myAutomationAccount'
$ResourceGroupName = 'myResourceGroup'

Start-AzureRmAutomationRunbook `
    -ResourceGroupName $ResourceGroupName `
    -AutomationAccountName $AutomationAccountName `
    -Name $ChildRunbookName `
    -DefaultProfile $context
```

### <a name="handling-exceptions"></a>Control de excepciones

Al crear scripts, es importante poder controlar las excepciones y los posibles errores intermitentes. Estas son algunas formas de controlar excepciones o problemas intermitentes con los runbooks:

#### <a name="erroractionpreference"></a>$ErrorActionPreference

La variable de preferencia [$ErrorActionPreference](/powershell/module/microsoft.powershell.core/about/about_preference_variables#erroractionpreference) determina cómo responde PowerShell a un error de no terminación. Los errores de terminación no se ven afectados por `$ErrorActionPreference`, siempre terminan. Mediante `$ErrorActionPreference`, normalmente un error de no terminación como `PathNotFound` del cmdlet `Get-ChildItem` impedirá que finalice el runbook. En el ejemplo siguiente se muestra el uso de `$ErrorActionPreference`. La línea `Write-Output` final nunca se ejecutará dado que el script se detendrá.

```powershell-interactive
$ErrorActionPreference = 'Stop'
Get-Childitem -path nofile.txt
Write-Output "This message will not show"
```

#### <a name="try-catch-finally"></a>Try Catch Finally

[Try Catch](/powershell/module/microsoft.powershell.core/about/about_try_catch_finally) se usa en los scripts de PowerShell para ayudarlo a controlar los errores de terminación. Con Try Catch, puede detectar excepciones específicas o generales. La instrucción Catch se debe usar para realizar el seguimiento de errores o para intentar controlar el error. En el ejemplo siguiente se intenta descargar un archivo que no existe. Se detecta la excepción `System.Net.WebException` y, si había otra excepción, se devuelve el último valor.

```powershell-interactive
try
{
   $wc = new-object System.Net.WebClient
   $wc.DownloadFile("http://www.contoso.com/MyDoc.doc")
}
catch [System.Net.WebException]
{
    "Unable to download MyDoc.doc from http://www.contoso.com."
}
catch
{
    "An error occurred that could not be resolved."
}
```

#### <a name="throw"></a>Throw

[Throw](/powershell/module/microsoft.powershell.core/about/about_throw) se puede usar para generar un error de terminación. Puede ser útil al definir su propia lógica en un runbook. Si se cumple un determinado criterio que debe detener el script, puede usar `throw` para detenerlo. En el ejemplo siguiente se muestra un parámetro de función necesario usando `throw`.

```powershell-interactive
function Get-ContosoFiles
{
  param ($path = $(throw "The Path parameter is required."))
  Get-ChildItem -Path $path\*.txt -recurse
}
```

### <a name="using-executables-or-calling-processes"></a>Uso de archivos ejecutables o llamada a procesos

Los runbooks que se ejecutan en espacios aislados de Azure no admiten la llamada a procesos (por ejemplo, .exe o subprocess.call). El motivo es que los espacios aislados de Azure son procesos compartidos ejecutados en contenedores, que no pueden tener acceso a todas las API subyacentes. En escenarios donde se requiere software de terceros o la llamada de subprocesos, se recomienda ejecutar el runbook en una instancia de [Hybrid Runbook Worker](automation-hybrid-runbook-worker.md).

### <a name="device-and-application-characteristics"></a>Características de dispositivos y aplicaciones

Los trabajos de runbook que se ejecutan en espacios aislados de Azure no tienen acceso a las características de ningún dispositivo o aplicación. La API más común usada para consultar métricas de rendimiento en Windows es WMI. Algunas de estas métricas comunes son el uso de CPU y memoria. Sin embargo, no importa qué API se use. Los trabajos que se ejecutan en la nube no tienen acceso a la implementación de Microsoft de Web Based Enterprise Management (WBEM), que está integrado en el Modelo de información común (CIM), que son los estándares del sector para definir las características de dispositivos y aplicaciones.

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
| Detenido |El trabajo lo detuvo el usuario antes de completarse. |
| Deteniéndose |El sistema está deteniendo el trabajo. |
| Suspended |El trabajo lo ha suspendido el usuario, el sistema o un comando en el runbook. Si un runbook no tiene un punto de control, se inicia desde el principio del runbook. Si lo tiene, puede volver a empezar y reanudarse desde su último punto de comprobación. El sistema solo suspende el runbook cuando se produce una excepción. De forma predeterminada, se establece ErrorActionPreference en **Continuar**, lo que significa que el trabajo sigue ejecutándose en un error. Si se establece esta variable de preferencia en **Detener**, se suspende el trabajo en un error. Solo se aplica a [Runbooks de flujo de trabajo de PowerShell y gráficos](automation-runbook-types.md) . |
| Suspendiendo |El sistema intenta suspender el trabajo a petición del usuario. El runbook debe alcanzar su siguiente punto de control antes de que se pueda suspender. Si ya ha pasado su último punto de control, se completa antes de que se pueda suspender. Solo se aplica a [Runbooks de flujo de trabajo de PowerShell y gráficos](automation-runbook-types.md) . |

## <a name="viewing-job-status-from-the-azure-portal"></a>Visualización de un estado de trabajo desde el portal de Azure

Puede ver un resumen del estado de todos los trabajos del runbook o profundizar en los detalles de un trabajo de runbook específico en Azure Portal. También puede configurar la integración con el área de trabajo de Log Analytics para reenviar flujos de trabajos y el estado del trabajo del runbook. Para más información sobre la integración con los registros de Azure Monitor, consulte [Reenvío del estado del trabajo y flujos de trabajo de Automation a los registros de Azure Monitor](automation-manage-send-joblogs-log-analytics.md).

### <a name="automation-runbook-jobs-summary"></a>Resumen de trabajos de runbook de Automation

A la derecha de la cuenta de Automation seleccionada, puede ver un resumen de todos los trabajos de runbook en el icono **Estadísticas de trabajo**.

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
4. Haga clic en uno de los trabajos de la lista y en la página de detalles del trabajo del runbook podrá ver sus detalles y la salida.

## <a name="retrieving-job-status-using-powershell"></a>Recuperación del estado del trabajo con PowerShell

Puede usar [Get-AzureRmAutomationJob](https://docs.microsoft.com/powershell/module/azurerm.automation/get-azurermautomationjob) para recuperar los trabajos creados para un runbook y los detalles de un trabajo determinado. Si inicia un runbook con PowerShell mediante [Start-AzureRmAutomationRunbook](https://docs.microsoft.com/powershell/module/azurerm.automation/start-azurermautomationrunbook), se devuelve el trabajo resultante. Use la salida [Get-AzureRmAutomationJobOutput](https://docs.microsoft.com/powershell/module/azurerm.automation/get-azurermautomationjoboutput) para obtener la salida de un trabajo.

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
$AutomationResourceGroupName = "MyResourceGroup"
$AutomationAccountName = "MyAutomationAccount"
$RunbookName = "MyRunbook"
$StartTime = (Get-Date).AddDays(-1)
$JobActivityLogs = Get-AzureRmLog -ResourceGroupName $AutomationResourceGroupName -StartTime $StartTime `
                                | Where-Object {$_.Authorization.Action -eq "Microsoft.Automation/automationAccounts/jobs/write"}

$JobInfo = @{}
foreach ($log in $JobActivityLogs)
{
    # Get job resource
    $JobResource = Get-AzureRmResource -ResourceId $log.ResourceId

    if ($JobInfo[$log.SubmissionTimestamp] -eq $null -and $JobResource.Properties.runbook.name -eq $RunbookName)
    {
        # Get runbook
        $Runbook = Get-AzureRmAutomationJob -ResourceGroupName $AutomationResourceGroupName -AutomationAccountName $AutomationAccountName `
                                            -Id $JobResource.Properties.jobId | ? {$_.RunbookName -eq $RunbookName}

        # Add job information to hash table
        $JobInfo.Add($log.SubmissionTimestamp, @($Runbook.RunbookName,$Log.Caller, $JobResource.Properties.jobId))
    }
}
$JobInfo.GetEnumerator() | sort key -Descending | Select-Object -First 1
```

## <a name="fair-share"></a>distribución equilibrada

Para compartir recursos entre todos los runbooks de la nube, Azure Automation descarga o detiene temporalmente todos los trabajos que lleven más de tres horas en ejecución. Los trabajos de [runbooks basados en PowerShell](automation-runbook-types.md#powershell-runbooks) y [runbooks de Python](automation-runbook-types.md#python-runbooks) se detienen y no se reinician, y el estado del trabajo muestra Stopped (Detenido).

En el caso de tareas de larga duración, se recomienda usar [Hybrid Runbook Worker](automation-hrw-run-runbooks.md#job-behavior). Las instancias de Hybrid Runbook Worker no están limitadas por la distribución equilibrada y no tienen una limitación del tiempo que se puede ejecutar un runbook. Los demás [límites](../azure-resource-manager/management/azure-subscription-service-limits.md#automation-limits) de trabajo se aplican a los espacios aislados de Azure y a Hybrid Runbook Workers. Aunque las instancias de Hybrid Runbook Worker no están restringidas por el límite de distribución equilibrada de tres horas, los runbooks que se ejecutan en ellas deben desarrollarse para admitir comportamientos de reinicio si se producen problemas inesperados en la infraestructura local.

Otra opción es optimizar el runbook mediante runbooks secundarios. Si el runbook secundario recorre en bucle la misma función en varios recursos, como una operación de base de datos en varias bases de datos, la función se puede mover a un [runbook secundario](automation-child-runbooks.md) y se le puede llamar con el cmdlet [Start-AzureRMAutomationRunbook](/powershell/module/azurerm.automation/start-azurermautomationrunbook). Cada uno de estos runbooks secundarios se ejecuta en paralelo en procesos independientes. Este comportamiento reduce la cantidad total de tiempo que tarda en completarse el runbook primario. Puede usar el cmdlet [Get-AzureRmAutomationJob](/powershell/module/azurerm.automation/Get-AzureRmAutomationJob) de su runbook para comprobar el estado del trabajo de cada elemento secundario si hay operaciones que se realizan después de que se complete el runbook secundario.

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre los distintos métodos que se pueden usar para iniciar un runbook en Azure Automation, consulte [Inicio de un runbook en Azure Automation](automation-starting-a-runbook.md)
* Para obtener más información sobre PowerShell, incluidos los módulos de referencia de lenguaje y aprendizaje, consulte la [documentación de PowerShell](https://docs.microsoft.com/powershell/scripting/overview).
