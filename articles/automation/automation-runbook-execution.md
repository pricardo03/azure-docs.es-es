---
title: Ejecución de un runbook en Azure Automation
description: Describe los detalles de cómo se procesa un runbook en Azure Automation.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 04/03/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 9d4661f6c975265ec710b29a8a05cc7ef41b4011
ms.sourcegitcommit: b4ad15a9ffcfd07351836ffedf9692a3b5d0ac86
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/05/2019
ms.locfileid: "59057428"
---
# <a name="runbook-execution-in-azure-automation"></a>Ejecución de un runbook en Azure Automation

Cuando se inicia un runbook en Azure Automation, se crea un trabajo. Un trabajo es una instancia única de ejecución de un runbook. Un trabajador de Azure Automation está asignado para ejecutar cada trabajo. Aunque los trabajadores los comparten varias cuentas de Azure, los trabajos de diferentes cuentas de Automation están aislados entre sí. No tiene el control sobre qué servicios de trabajo solicitar para el trabajo. Un runbook individual puede tener muchos trabajos que se ejecutan al mismo tiempo. Se puede reutilizar el entorno de ejecución para los trabajos de la misma cuenta de Automation. Cuantos más trabajos se ejecuten al mismo tiempo, más a menudo se podrán enviar al mismo espacio aislado. Los trabajos que se ejecutan en el mismo proceso de espacio aislado pueden afectarse entre sí; un ejemplo es ejecutar el cmdlet `Disconnect-AzureRMAccount`. Ejecutar este cmdlet desconectaría cada trabajo de runbook en el proceso de espacio aislado compartido. Al ver la lista de runbooks en Azure Portal, se muesta el estado de todos los trabajos iniciados por cada runbook. Puede ver la lista de trabajos de cada runbook para hacer un seguimiento de su estado. Los registros de trabajo se almacenan durante 30 días como máximo. Para obtener una descripción de los distintos estados de trabajo, consulte [Estados del trabajo](#job-statuses).

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-dsr-and-stp-note.md)]

El siguiente diagrama muestra el ciclo de vida de un trabajo de runbook para [runbooks de PowerShell](automation-runbook-types.md#powershell-runbooks), [runbooks gráficos](automation-runbook-types.md#graphical-runbooks) y [runbooks de flujo de trabajo de PowerShell](automation-runbook-types.md#powershell-workflow-runbooks).

![Estados de trabajo: flujo de trabajo de PowerShell](./media/automation-runbook-execution/job-statuses.png)

Los trabajos tienen acceso a los recursos de Azure mediante una conexión a la suscripción de Azure. Solo tienen acceso a los recursos de su centro de datos si estos recursos están accesibles desde la nube pública.

## <a name="where-to-run-your-runbooks"></a>Dónde ejecutar los runbooks

Los runbooks de Azure Automation puede ejecutarse en un espacio aislado de Azure o una instancia de [Hybrid Runbook Worker](automation-hybrid-runbook-worker.md). Un espacio aislado es un entorno compartido en Azure que puede usarse en varios trabajos. Los trabajos con el mismo espacio aislado están sujetos a las limitaciones de recursos de dicho espacio. Hybrid Runbook Worker puede ejecutar runbooks directamente en el equipo que hospeda el rol y en los recursos del entorno para administrar esos recursos locales. Los runbooks se almacenan y administran en Azure Automation y después se entregan a uno o más equipos asignados. En los espacios aislados de Azure, puede ejecutar fácilmente la mayoría de los runbooks. Existen escenarios específicos donde se puede recomendar la elección de una instancia de Hybrid Runbook a través de un espacio aislado de Azure para ejecutar el runbook. Consulte la tabla siguiente para ver una lista de algunos escenarios de ejemplo:

|Tarea|Mejor opción|Notas|
|---|---|---|
|Integración con recursos de Azure|Espacio aislado de Azure|Hospedado en Azure, la autenticación es más sencilla. Si usa una instancia de Hybrid Runbook Worker en una máquina virtual de Azure, puede usar [identidades administradas para recursos de Azure](automation-hrw-run-runbooks.md#managed-identities-for-azure-resources).|
|Rendimiento óptimo para administrar recursos de Azure|Espacio aislado de Azure|Secuencia de comandos se ejecuta en el mismo entorno, que a su vez tiene la menor latencia|
|Minimizar los costos operativos|Espacio aislado de Azure|No hay sobrecarga de proceso, no hay necesidad de una máquina virtual|
|Script de larga duración|Hybrid Runbook Worker|Los espacios aislados de Azure tienen [limitación en los recursos](../azure-subscription-service-limits.md#automation-limits)|
|Interactuar con servicios locales|Hybrid Runbook Worker|Puede tener acceso directo a la máquina host|
|Requerir software de terceros y archivos ejecutables|Hybrid Runbook Worker|Usted administra el sistema operativo y puede instalar software|
|Supervisar un archivo o carpeta con un runbook|Hybrid Runbook Worker|Usar una [tarea de monitor](automation-watchers-tutorial.md) en una instancia de Hybrid Runbook Worker|
|Script que consume muchos recursos|Hybrid Runbook Worker| Los espacios aislados de Azure tienen [limitación en los recursos](../azure-subscription-service-limits.md#automation-limits)|
|Uso de módulos con requisitos específicos| Hybrid Runbook Worker|A continuación, se indican algunos ejemplos:</br> **WinSCP**: dependencia de winscp.exe </br> **IISAdministration**: necesita IIS para habilitarse|
|Instalar el módulo que requiere el instalador|Hybrid Runbook Worker|Los módulos de recinto de seguridad deben ser que se puede copiar|
|Uso de runbooks o módulos que requieren .NET Framework que no sea 4.7.2|Hybrid Runbook Worker|Los espacios aislados de Automation tienen .NET Framework 4.7.2, y no hay ninguna manera de actualizarlo|
|Secuencias de comandos que requieren la elevación|Hybrid Runbook Worker|Los espacios aislados no permitir la elevación. Para resolver este problema, utilice un Hybrid Runbook Worker y puede desactivar UAC y usar `Invoke-Command` cuando ejecute el comando que requiere la elevación|
|Secuencias de comandos que requieren acceso a WMI|Hybrid Runbook Worker|Trabajos que se ejecutan en espacios aislados de la nube [no tiene acceso a WMI](#device-and-application-characteristics)|

## <a name="runbook-behavior"></a>Comportamiento del runbook

Los runbooks se ejecutan según la lógica que se define dentro de ellos. Si se interrumpe un runbook, se reinicia desde el principio. Este comportamiento requiere que los runbooks se escriban de forma que puedan reiniciarse en caso de producirse problemas transitorios.

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

### <a name="time-dependant-scripts"></a>Scripts dependientes del tiempo

Es necesario tener cuidado al crear runbooks. Como se mencionó anteriormente, los runbooks deben crearse de forma que sean sólidos y puedan gestionar errores transitorios que pueden hacer que el runbook se reinicie o produzca error. Si se produce un error en un runbook, se vuelve a intentar. Si un runbook se ejecuta con normalidad dentro de una restricción de tiempo, se ejecutan la lógica para comprobar el tiempo de ejecución debe implementarse en el runbook para garantizar las operaciones como inicio, apagar o escalar horizontalmente solo durante horas específicas.

### <a name="tracking-progress"></a>Seguimiento del progreso

Es una buena práctica crear runbooks que sean de naturaleza modular. Esto significa estructurar la lógica en el runbook de forma que se pueda reutilizar y reiniciar fácilmente. Seguimiento del progreso en un runbook es una buena forma de asegurarse de que la lógica en un runbook se ejecutará correctamente si hubo problemas. Algunas de las posibles maneras de seguir el progreso del runbook es mediante un origen externo, como cuentas de almacenamiento, una base de datos o archivos compartidos. Mediante el seguimiento del estado externamente, puede crear lógica en su runbook para comprobar primero el estado de la última acción realizado por el runbook. A continuación, basado en los resultados, o bien omitir o continuar tareas específicas en el runbook.

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

Al crear runbooks que tratar con varias suscripciones, el runbook debe usar el [Disable-AzureRmContextAutosave](/powershell/module/azurerm.profile/disable-azurermcontextautosave) cmdlet para asegurarse de que el contexto de autenticación no se recupera desde otro runbook que puede ser que se ejecuta en el mismo espacio aislado. A continuación, deberá usar el `-AzureRmContext` parámetro en su `AzureRM` cmdlets y pasarle el contexto adecuado.

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

Al crear secuencias de comandos, es importante ser capaz de controlar las excepciones y los posibles errores intermitentes. Éstas son algunas formas de controlar las excepciones o problemas intermitentes con sus runbooks:

#### <a name="erroractionpreference"></a>$ErrorActionPreference

El [$ErrorActionPreference](/powershell/module/microsoft.powershell.core/about/about_preference_variables#erroractionpreference) variable de preferencia determina cómo PowerShell responde a un error de no terminación. Los errores de terminación no se ven afectados por `$ErrorActionPreference`, siempre terminan. Mediante el uso de `$ErrorActionPreference`, como un error de no terminación normalmente `PathNotFound` desde el `Get-ChildItem` cmdlet detendrá el runbook antes de finalización. El ejemplo siguiente muestra utilizando `$ErrorActionPreference`. El último `Write-Output` línea nunca se ejecutará como se detendrá la secuencia de comandos.

```powershell-interactive
$ErrorActionPreference = 'Stop'
Get-Childitem -path nofile.txt
Write-Output "This message will not show"
```

#### <a name="try-catch-finally"></a>Try Catch Finally

[Try Catch](/powershell/module/microsoft.powershell.core/about/about_try_catch_finally) se utiliza en los scripts de PowerShell para ayudarle a controlar los errores de terminación. Mediante el uso de Try Catch, puede detectar excepciones específicas o excepciones generales. La instrucción Catch debe utilizada para realizar el seguimiento de errores o intentar controlar el error. El ejemplo siguiente se intenta descargar un archivo que no existe. Detecta el `System.Net.WebException` excepción, si había otra excepción, se devuelve el último valor.

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

[Producir](/powershell/module/microsoft.powershell.core/about/about_throw) puede usarse para generar un error de terminación. Esto puede ser útil al definir su propia lógica en un runbook. Si se cumple determinados criterios que debe detenerse la secuencia de comandos, puede usar `throw` para detener la secuencia de comandos. El ejemplo siguiente se muestra un parámetro de función necesario mediante el uso de machine `throw`.

```powershell-interactive
function Get-ContosoFiles
{
  param ($path = $(throw "The Path parameter is required."))
  Get-ChildItem -Path $path\*.txt -recurse
}
```

### <a name="using-executables-or-calling-processes"></a>Uso de archivos ejecutables o llamada a procesos

Los runbooks que se ejecutan en espacios aislados de Azure no admiten los procesos que realiza la llamada (por ejemplo, .exe o subprocess.call). Esto es porque los espacios aislados de Azure son procesos compartidos que se ejecutan en contenedores, que no se pueden tener acceso a todas las API subyacentes. En escenarios donde se requiere software de terceros o la llamada de subprocesos, se recomienda ejecutar el runbook en una instancia de [Hybrid Runbook Worker](automation-hybrid-runbook-worker.md).

### <a name="device-and-application-characteristics"></a>Características de dispositivos y aplicaciones

Los trabajos de runbook que se ejecutan en espacios aislados de Azure no tiene acceso a las características de dispositivo o aplicación. La API más comunes que se usa para consultar métricas de rendimiento en Windows es WMI. Algunas de estas métricas comunes son el uso de CPU y memoria. Sin embargo, no importa lo que se usa la API. Trabajos que se ejecutan en la nube no tiene acceso la implementación de Microsoft de Web basada en Enterprise Management (WBEM), que se basa en el modelo de información común (CIM), que son los estándares del sector para definir características del dispositivo y la aplicación.

## <a name="job-statuses"></a> Estados del trabajo

En la tabla siguiente se describen los diferentes estados posibles para un trabajo. PowerShell tiene dos tipos de errores, errores de terminación y de no terminación. Los primeros cambian el estado del runbook a **Failed** (Con error) si se producen. Los segundos permiten que el script continúe después de que sucedan. Un ejemplo de un error de no terminación es utilizar el cmdlet `Get-ChildItem` con una ruta de acceso que no existe. PowerShell se ve que la ruta de acceso no existe, genera un error y pasa a la carpeta siguiente. El error no establecería el estado de runbook en **Failed** (Con error) y se podría marcarse como **Completed** (Completado). Para forzar a un runbook a detenerse ante un error de no terminación, puede usar `-ErrorAction Stop` en el cmdlet.

| Status | DESCRIPCIÓN |
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

Puede ver un resumen del estado de todos los trabajos del runbook o profundizar en los detalles de un trabajo de runbook específico en Azure Portal. También puede configurar la integración con el área de trabajo de Log Analytics para reenviar flujos de trabajos y el estado del trabajo del runbook. Para obtener más información sobre la integración con los registros de Azure Monitor, consulte [reenvío del estado del trabajo y flujos de trabajo de Automation a los registros de Azure Monitor](automation-manage-send-joblogs-log-analytics.md).

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

## <a name="retrieving-job-status-using-powershell"></a>Recuperar el estado del trabajo mediante PowerShell

Puede usar [Get-AzureRmAutomationJob](https://docs.microsoft.com/powershell/module/azurerm.automation/get-azurermautomationjob) para recuperar los trabajos creados para un runbook y los detalles de un trabajo determinado. Si inicia un runbook con el uso de PowerShell [Start-AzureRmAutomationRunbook](https://docs.microsoft.com/powershell/module/azurerm.automation/start-azurermautomationrunbook), a continuación, devuelve el trabajo resultante. Use la salida [Get-AzureRmAutomationJobOutput](https://docs.microsoft.com/powershell/module/azurerm.automation/get-azurermautomationjoboutput) para obtener la salida de un trabajo.

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

Para compartir recursos entre todos los runbooks en la nube, Azure Automation descargará temporalmente o detiene cualquier trabajo que se ha ejecutado durante más de tres horas. Los trabajos de [runbooks basados en PowerShell](automation-runbook-types.md#powershell-runbooks) y [runbooks de Python](automation-runbook-types.md#python-runbooks) se detienen y no se reinician, y el estado del trabajo muestra Stopped (Detenido).

En el caso de tareas de larga duración, se recomienda usar [Hybrid Runbook Worker](automation-hrw-run-runbooks.md#job-behavior). Las instancias de Hybrid Runbook Worker no están limitadas por la distribución equilibrada y no tienen una limitación del tiempo que se puede ejecutar un runbook. Los demás [límites](../azure-subscription-service-limits.md#automation-limits) de trabajo se aplican a los espacios aislados de Azure y a Hybrid Runbook Workers. Mientras que Hybrid Runbook Worker no están limitado por el límite de reparto de 3 horas, se deben desarrollar runbooks que se ejecutan en ellos para admitir los comportamientos de reinicio de problemas de infraestructura local inesperado.

Otra opción es optimizar el runbook mediante runbooks secundarios. Si el runbook secundario recorre en bucle la misma función en varios recursos, como una operación de base de datos en varias bases de datos, la función se puede mover a un [runbook secundario](automation-child-runbooks.md) y se le puede llamar con el cmdlet [Start-AzureRMAutomationRunbook](/powershell/module/azurerm.automation/start-azurermautomationrunbook). Cada uno de estos runbooks secundarios se ejecuta en paralelo en procesos independientes. Este comportamiento reduce la cantidad total de tiempo que tarda en completarse el runbook primario. Puede usar el [Get-AzureRmAutomationJob](/powershell/module/azurerm.automation/Get-AzureRmAutomationJob) cmdlet en su runbook para comprobar el estado del trabajo para cada elemento secundario si hay operaciones que llevan a cabo después de que se complete el runbook secundario.

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre los distintos métodos que se pueden usar para iniciar un runbook en Azure Automation, consulte [Inicio de un runbook en Azure Automation](automation-starting-a-runbook.md)

