---
title: Configuración de los scripts previos y posteriores a la implementación de Update Management en Azure (versión preliminar)
description: En este artículo se describe cómo configurar y administrar los scripts previos y posteriores a las implementaciones de actualizaciones.
services: automation
ms.service: automation
ms.component: update-management
author: georgewallace
ms.author: gwallace
ms.date: 09/18/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: c906a771a63b3d8320eab1d2d57e8c34916e1d39
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/28/2018
ms.locfileid: "47433199"
---
# <a name="manage-pre-and-post-scripts-preview"></a>Administración de scripts previos y posteriores (versión preliminar)

Los scripts previos y posteriores le permiten ejecutar runbooks de PowerShell en la cuenta de Automation antes (antes de la tarea) y después (después de la tarea) de la implementación de una actualización. Los scripts previos y posteriores se ejecutan en el contexto de Azure y no de forma local.

## <a name="runbook-requirements"></a>Requisitos de runbook

Para que un runbook se utilice como script previo o posterior, el runbook debe importarse a su cuenta de automatización y publicarse. Para más información sobre este proceso, consulte [Publicación de un runbook](automation-creating-importing-runbook.md#publishing-a-runbook).

## <a name="using-a-prepost-script"></a>Uso de script previo o posterior

Para utilizar un script previo o posterior en una implementación de actualización, solo tiene que empezar por crear una de estas implementaciones. Seleccione **Scripts previos + scripts posteriores (versión preliminar)**. Se abrirá la página **Seleccionar scripts previos + scripts posteriores**.  

![Selección de scripts](./media/pre-post-scripts/select-scripts.png)

Seleccione el script que desea usar; en este ejemplo, ha usado el runbook **UpdateManagement TurnOnVms**. Cuando seleccione el runbook, se abre la página **Configurar script**, proporcione los valores de los parámetros y seleccione **Script previo**. Pulse **OK** (Aceptar) cuando haya terminado.

![Configuración de script](./media/pre-post-scripts/configure-script.png)

Repita este proceso para el script **UpdateManagement TurnOffVms**. Pero al elegir el **tipo de script**, elija **Script posterior**.

La sección **Elementos seleccionados** ahora muestra los dos scripts seleccionados y activados como script previo y el otro como script posterior.

![Elementos seleccionados](./media/pre-post-scripts/selected-items.png)

Finalice la configuración de su implementación de actualizaciones.

Cuando se completa la implementación de actualizaciones, puede ir a **Implementaciones de actualizaciones** para ver los resultados. Como se puede ver, se proporciona el script previo y el posterior.

![Resultados de actualización](./media/pre-post-scripts/update-results.png)

Si se hace clic en la ejecución de la implementación de actualizaciones, se proporcionan detalles adicionales para los scripts previos y posteriores. Se proporciona un vínculo al origen del script en el momento de la ejecución.

![Resultados de la ejecución de la implementación](./media/pre-post-scripts/deployment-run.png)

## <a name="passing-parameters"></a>Paso de parámetros

Cuando se configuran los scripts previos y posteriores, se pueden pasar parámetros como si se tratara de programar un runbook. Los parámetros se definen en el momento de la creación de la implementación de actualización. Además de los parámetros estándar de runbook, se proporciona un parámetro adicional. Este parámetro es **SoftwareUpdateConfigurationRunContext**. Este parámetro es una cadena JSON, y si define el parámetro en su script previo o posterior, la implementación de la actualización lo pasará automáticamente. El parámetro contiene información sobre la implementación de la actualización, que es un subconjunto de la información devuelta por [SoftwareUpdateconfigurations API](/rest/api/automation/softwareupdateconfigurations/getbyname#updateconfiguration). En la tabla siguiente se muestran las propiedades que se proporcionan en la variable:

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

Lo siguiente es un ejemplo de una cadena JSON que se pasa al parámetro **SoftwareUpdateConfigurationRunContext**:

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

Un ejemplo completo con todas las propiedades que puede encontrarse en: [Software Update Configurations - Get By Name](/rest/api/automation/softwareupdateconfigurations/getbyname#examples) (Configuraciones de actualizaciones de software: Obtener por nombre).

> [!NOTE]
> Los equipos agregados a una implementación mediante [Grupos dinámicos (versión preliminar)](automation-update-management.md#using-dynamic-groups) no forman parte actualmente del parámetro **SoftwareUpdateConfigurationRunContext**.

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

Todos los ejemplos se basan en la plantilla básica que se define en el ejemplo siguiente. Esta plantilla se puede usar para crear el propio runbook para usarlo con los scripts previos y posteriores. Se incluye la lógica necesaria para autenticar con Azure así como para manejar el parámetro `SoftwareUpdateConfigurationRunContext`.

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
$vmIds = $context.SoftwareUpdateConfigurationSettings.AzureVirtualMachines 
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

Las tareas previas y posteriores se ejecutan en el contexto de Azure y no tienen acceso a máquinas que no son de Azure. Para poder interactuar con las máquinas que no son de Azure, debe tener lo siguiente:

* Una cuenta de ejecución
* Hybrid Runbook Worker instalado en la máquina
* Un runbook que desea ejecutar localmente
* Runbook principal

Para interactuar con máquinas que no son de Azure, se ejecuta un runbook principal en el contexto de Azure. Este runbook llama a un runbook secundario con el cmdlet [Start-AzureRmAutomationRunbook](/powershell/module/azurerm.automation/start-azurermautomationrunbook). Debe especificar el parámetro `-RunOn` y proporcionar el nombre de la instancia de Hybrid Runbook Worker para que el script se ejecute.

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

## <a name="known-issues"></a>Problemas conocidos

* No se pueden pasar objetos o matrices a parámetros al usar los scripts previos y posteriores. Se producirá un error en el runbook.
* Los runbooks que no están publicados se muestran como elementos seleccionables cuando se elige un script previo o posterior. Solo los runbooks que se publican deben elegirse ya que los runbooks no publicados no se pueden llamar y producirán un error.
* Los equipos agregados a una implementación mediante [Grupos dinámicos (versión preliminar)](automation-update-management.md#using-dynamic-groups) no forman parte actualmente del parámetro **SoftwareUpdateConfigurationRunContext** que se pasa a los scripts previos y posteriores.

## <a name="next-steps"></a>Pasos siguientes

Continúe con el tutorial para aprender a administrar actualizaciones de máquinas virtuales Windows.

> [!div class="nextstepaction"]
> [Administración de actualizaciones y revisiones para las máquinas virtuales Windows de Azure](automation-tutorial-update-management.md)