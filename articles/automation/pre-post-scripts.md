---
title: Configuración de los scripts previos y posteriores a la implementación de Update Management en Azure (versión preliminar)
description: En este artículo se describe cómo configurar y administrar los scripts previos y posteriores a las implementaciones de actualizaciones.
services: automation
ms.service: automation
ms.subservice: update-management
author: georgewallace
ms.author: gwallace
ms.date: 04/04/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 76cd877380090ccad8b2f7b7dbe79957e0eab5bb
ms.sourcegitcommit: b4ad15a9ffcfd07351836ffedf9692a3b5d0ac86
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/05/2019
ms.locfileid: "59056684"
---
# <a name="manage-pre-and-post-scripts-preview"></a>Administración de scripts previos y posteriores (versión preliminar)

Los scripts previos y posteriores le permiten ejecutar runbooks de PowerShell en la cuenta de Automation antes (antes de la tarea) y después (después de la tarea) de la implementación de una actualización. Los scripts previos y posteriores se ejecutan en el contexto de Azure y no de forma local. Los scripts anteriores se ejecutan al principio de la implementación de actualizaciones. Los scripts posteriores se ejecutan al final de la implementación y después de todos reinicios que estén configurados.

## <a name="runbook-requirements"></a>Requisitos de runbook

Para que un runbook se utilice como script previo o posterior, el runbook debe importarse a su cuenta de automatización y publicarse. Para más información sobre este proceso, consulte [Publicación de un runbook](manage-runbooks.md#publish-a-runbook).

## <a name="using-a-prepost-script"></a>Uso de script previo o posterior

Para usar un script previo o posterior en una implementación de actualización, solo tiene que empezar por crear una de estas implementaciones de actualización. Seleccione **Scripts previos + scripts posteriores (versión preliminar)**. Se abrirá la página **Seleccionar scripts previos + scripts posteriores**.  

![Selección de scripts](./media/pre-post-scripts/select-scripts.png)

Seleccione el script que desea usar; en este ejemplo, ha usado el runbook **UpdateManagement TurnOnVms**. Cuando seleccione el runbook, se abre la página **Configurar script**, proporcione los valores de los parámetros y seleccione **Script previo**. Pulse **OK** (Aceptar) cuando haya terminado.

![Configuración de script](./media/pre-post-scripts/configure-script.png)

Repita este proceso para el script **UpdateManagement TurnOffVms**. Pero al elegir el **tipo de script**, elija **Script posterior**.

La sección **Elementos seleccionados** ahora muestra los dos scripts seleccionados y activados como script previo y el otro como script posterior.

![Elementos seleccionados](./media/pre-post-scripts/selected-items.png)

Finalice la configuración de su implementación de actualizaciones.

Cuando se completa la implementación de actualizaciones, puede ir a **Implementaciones de actualizaciones** para ver los resultados. Como se puede ver, se proporciona el estado del script previo y el posterior.

![Resultados de actualización](./media/pre-post-scripts/update-results.png)

Si se hace clic en la ejecución de la implementación de actualizaciones, se proporcionan detalles adicionales acerca de los scripts previos y posteriores. Se proporciona un vínculo al origen del script en el momento de la ejecución.

![Resultados de la ejecución de la implementación](./media/pre-post-scripts/deployment-run.png)

## <a name="passing-parameters"></a>Paso de parámetros

Cuando se configuran los scripts previos y posteriores, se pueden pasar parámetros como cuando se programa un runbook. Los parámetros se definen en el momento de la creación de la implementación de actualización. Los scripts anteriores y posteriores admiten los siguientes tipos:

* [char]
* [byte]
* [int]
* [long]
* [decimal]
* [single]
* [double]
* [DateTime]
* [string]

Si necesita otro tipo de objeto, puede convertirlo a otro tipo mediante su propia lógica en el runbook.

Además de los parámetros estándar de runbook, se proporciona un parámetro adicional. Este parámetro es **SoftwareUpdateConfigurationRunContext**. Este parámetro es una cadena JSON, y si define el parámetro en su script previo o posterior, la implementación de la actualización lo pasará automáticamente. El parámetro contiene información sobre la implementación de la actualización, que es un subconjunto de la información que devuelve [SoftwareUpdateconfigurations API](/rest/api/automation/softwareupdateconfigurations/getbyname#updateconfiguration). En la tabla siguiente se muestran las propiedades que se proporcionan en la variable:

## <a name="stopping-a-deployment"></a>Al detener una implementación

Si desea detener una implementación basada en una secuencia de comandos de Pre debe [throw](automation-runbook-execution.md#throw) una excepción. Si no se produce una excepción, la implementación y el script posterior se ejecutarán. El [runbook de ejemplo](https://gallery.technet.microsoft.com/Update-Management-Run-6949cc44?redir=0) en la galería se muestra cómo hacerlo. El siguiente es un fragmento de código de dicho runbook.

```powershell
#In this case, we want to terminate the patch job if any run fails.
#This logic might not hold for all cases - you might want to allow success as long as at least 1 run succeeds
foreach($summary in $finalStatus)
{
    if ($summary.Type -eq "Error")
    {
        #We must throw in order to fail the patch deployment.  
        throw $summary.Summary
    }
}
```

### <a name="softwareupdateconfigurationruncontext-properties"></a>Propiedades de SoftwareUpdateConfigurationRunContext

|Propiedad  |DESCRIPCIÓN  |
|---------|---------|
|SoftwareUpdateConfigurationName     | El nombre de la configuración de actualizaciones de software        |
|SoftwareUpdateConfigurationRunId     | El identificador único de la ejecución.        |
|SoftwareUpdateConfigurationSettings     | Una colección de propiedades relacionadas con la configuración de actualizaciones de software         |
|SoftwareUpdateConfigurationSettings.operatingSystem     | Sistemas operativos a los que se dirige la implementación de la actualización         |
|SoftwareUpdateConfigurationSettings.duration     | La duración máxima de la implementación de la actualización es de `PT[n]H[n]M[n]S`, según ISO8601, también llamada "ventana de mantenimiento".          |
|SoftwareUpdateConfigurationSettings.Windows     | Una colección de propiedades relacionadas con los equipos de Windows         |
|SoftwareUpdateConfigurationSettings.Windows.excludedKbNumbers     | Una lista de artículos de Knowledge Base que se excluyen de la implementación de actualizaciones        |
|SoftwareUpdateConfigurationSettings.Windows.includedUpdateClassifications     | Clasificaciones de actualizaciones seleccionadas para la implementación de actualizaciones        |
|SoftwareUpdateConfigurationSettings.Windows.rebootSetting     | Reinicio de la configuración para la implementación de actualizaciones        |
|azureVirtualMachines     | Una lista de identificadores de recurso para las máquinas virtuales de Azure en la implementación de actualizaciones        |
|nonAzureComputerNames|Una lista de los nombres de domino completos de los equipos que no son de Azure en la implementación de actualizaciones|

En el ejemplo siguiente se muestra una cadena JSON que se pasa al parámetro **SoftwareUpdateConfigurationRunContext**:

```json
"SoftwareUpdateConfigurationRunContext":{
      "SoftwareUpdateConfigurationName":"sampleConfiguration",
      "SoftwareUpdateConfigurationRunId":"00000000-0000-0000-0000-000000000000",
      "SoftwareUpdateConfigurationSettings":{
         "operatingSystem":"Windows",
         "duration":"PT2H0M",
         "windows":{
            "excludedKbNumbers":[
               "168934",
               "168973"
            ],
            "includedUpdateClassifications":"Critical",
            "rebootSetting":"IfRequired"
         },
         "azureVirtualMachines":[
            "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresources/providers/Microsoft.Compute/virtualMachines/vm-01",
            "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresources/providers/Microsoft.Compute/virtualMachines/vm-02",
            "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresources/providers/Microsoft.Compute/virtualMachines/vm-03"
         ], 
         "nonAzureComputerNames":[
            "box1.contoso.com",
            "box2.contoso.com"
         ]
      }
   }
```

Se puede encontrar un ejemplo completo con todas las propiedades en: [Software Update Configurations - Get By Name](/rest/api/automation/softwareupdateconfigurations/getbyname#examples) (Configuraciones de actualizaciones de software: obtener por el nombre)

> [!NOTE]
> La `SoftwareUpdateConfigurationRunContext` objeto puede contener entradas duplicadas para las máquinas. Esto puede provocar que los scripts anteriores y posteriores ejecutarlo varias veces en el mismo equipo. Para solucionar este comportamiento, use `Sort-Object -Unique` para seleccionar solo los nombres de máquina virtual únicos en la secuencia de comandos.

## <a name="samples"></a>Ejemplos

Se pueden encontrar ejemplos de scripts previos y posteriores la [galería del centro de scripts](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&f%5B0%5D.Value=WindowsAzure&f%5B0%5D.Text=Windows%20Azure&f%5B1%5D.Type=SubCategory&f%5B1%5D.Value=WindowsAzure_automation&f%5B1%5D.Text=Automation&f%5B2%5D.Type=SearchText&f%5B2%5D.Value=update%20management&f%5B3%5D.Type=Tag&f%5B3%5D.Value=Patching&f%5B3%5D.Text=Patching&f%5B4%5D.Type=ProgrammingLanguage&f%5B4%5D.Value=PowerShell&f%5B4%5D.Text=PowerShell), o importarse mediante Azure Portal. Para importarlos mediante el portal, en su la cuenta de Automation, en **Automatización de procesos**, seleccione **Galería de runbooks**. Utilice **Update Management** para el filtro.

![Lista de la galería](./media/pre-post-scripts/runbook-gallery.png)

O puede buscarlos por su nombre de script, tal y como se muestra en la siguiente lista:

* Update Management: activar máquinas virtuales
* Update Management: desactivar máquinas virtuales
* Update Management: ejecución de scripts de forma local
* Update Management: plantilla para scripts previos y posteriores
* Update Management: ejecutar script con el comando de ejecución

> [!IMPORTANT]
> Después de importar los runbooks, debe **publicarlos** antes de que se puedan usar. Para ello, busque el runbook en su cuenta de Automation, seleccione **Editar** y haga clic en **Publicar**.

Todos los ejemplos se basan en la plantilla básica que se define en el ejemplo siguiente. Esta plantilla se puede usar para crear el propio runbook para usarlo con los scripts previos y posteriores. Se incluye la lógica necesaria para realizar una autenticación con Azure así como para manejar el parámetro `SoftwareUpdateConfigurationRunContext`.

```powershell
<# 
.SYNOPSIS 
 Barebones script for Update Management Pre/Post 
 
.DESCRIPTION 
  This script is intended to be run as a part of Update Management Pre/Post scripts.  
  It requires a RunAs account. 
 
.PARAMETER SoftwareUpdateConfigurationRunContext 
  This is a system variable which is automatically passed in by Update Management during a deployment. 
#> 
 
param( 
    [string]$SoftwareUpdateConfigurationRunContext 
) 
#region BoilerplateAuthentication 
#This requires a RunAs account 
$ServicePrincipalConnection = Get-AutomationConnection -Name 'AzureRunAsConnection' 
 
Add-AzureRmAccount ` 
    -ServicePrincipal ` 
    -TenantId $ServicePrincipalConnection.TenantId ` 
    -ApplicationId $ServicePrincipalConnection.ApplicationId ` 
    -CertificateThumbprint $ServicePrincipalConnection.CertificateThumbprint 
 
$AzureContext = Select-AzureRmSubscription -SubscriptionId $ServicePrincipalConnection.SubscriptionID 
#endregion BoilerplateAuthentication 
 
#If you wish to use the run context, it must be converted from JSON 
$context = ConvertFrom-Json  $SoftwareUpdateConfigurationRunContext 
#Access the properties of the SoftwareUpdateConfigurationRunContext 
$vmIds = $context.SoftwareUpdateConfigurationSettings.AzureVirtualMachines | Sort-Object -Unique
$runId = $context.SoftwareUpdateConfigurationRunId 
 
Write-Output $context 
 
#Example: How to create and write to a variable using the pre-script: 
<# 
#Create variable named after this run so it can be retrieved 
New-AzureRmAutomationVariable -ResourceGroupName $ResourceGroup –AutomationAccountName $AutomationAccount –Name $runId -Value "" –Encrypted $false 
#Set value of variable  
Set-AutomationVariable –Name $runId -Value $vmIds 
#> 
 
#Example: How to retrieve information from a variable set during the pre-script 
<# 
$variable = Get-AutomationVariable -Name $runId 
#>      
```

## <a name="interacting-with-non-azure-machines"></a>Interactuación con máquinas que no son de Azure

Las tareas previas y posteriores se ejecutan en el contexto de Azure y no tienen acceso a máquinas que no son de Azure. Para poder interactuar con las máquinas que no son de Azure, debe tener los siguientes elementos:

* Una cuenta de ejecución
* Hybrid Runbook Worker instalado en la máquina
* Un runbook que desea ejecutar localmente
* Runbook principal

Para interactuar con máquinas que no sean de Azure, se ejecuta un runbook principal en el contexto de Azure. Este runbook llama a un runbook secundario con el cmdlet [Start-AzureRmAutomationRunbook](/powershell/module/azurerm.automation/start-azurermautomationrunbook). Debe especificar el parámetro `-RunOn` y proporcionar el nombre de la instancia de Hybrid Runbook Worker para que el script se ejecute.

```powershell
$ServicePrincipalConnection = Get-AutomationConnection -Name 'AzureRunAsConnection'

Add-AzureRmAccount `
    -ServicePrincipal `
    -TenantId $ServicePrincipalConnection.TenantId `
    -ApplicationId $ServicePrincipalConnection.ApplicationId `
    -CertificateThumbprint $ServicePrincipalConnection.CertificateThumbprint

$AzureContext = Select-AzureRmSubscription -SubscriptionId $ServicePrincipalConnection.SubscriptionID

$resourceGroup = "AzureAutomationResourceGroup"
$aaName = "AzureAutomationAccountName"

$output = Start-AzureRmAutomationRunbook -Name "StartService" -ResourceGroupName $resourceGroup  -AutomationAccountName $aaName -RunOn "hybridWorker"

$status = Get-AzureRmAutomationJob -Id $output.jobid -ResourceGroupName $resourceGroup  -AutomationAccountName $aaName
while ($status.status -ne "Completed")
{ 
    Start-Sleep -Seconds 5
    $status = Get-AzureRmAutomationJob -Id $output.jobid -ResourceGroupName $resourceGroup  -AutomationAccountName $aaName
}

$summary = Get-AzureRmAutomationJobOutput -Id $output.jobid -ResourceGroupName $resourceGroup  -AutomationAccountName $aaName

if ($summary.Type -eq "Error")
{
    Write-Error -Message $summary.Summary
}
```

## <a name="abort-patch-deployment"></a>Anular la implementación de revisiones

Si la secuencia de comandos anterior devuelve un error, desea anular la implementación. Para ello, primero debe [throw](/powershell/module/microsoft.powershell.core/about/about_throw) un error en el script para cualquier lógica que supondría un error.

```powershell
if (<My custom error logic>)
{
    #Throw an error to fail the patch deployment.  
    throw "There was an error, abort deployment"
}
```
## <a name="known-issues"></a>Problemas conocidos

* No se pueden pasar objetos o matrices a parámetros al usar los scripts previos y posteriores. Se producirá un error en el runbook.

## <a name="next-steps"></a>Pasos siguientes

Continúe con el tutorial para aprender a administrar actualizaciones de máquinas virtuales Windows.

> [!div class="nextstepaction"]
> [Administración de actualizaciones y revisiones para las máquinas virtuales Windows de Azure](automation-tutorial-update-management.md)

