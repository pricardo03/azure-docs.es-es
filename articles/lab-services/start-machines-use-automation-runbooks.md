---
title: Iniciar máquinas mediante runbooks de Automation en Azure DevTest Labs | Microsoft Docs
description: Obtenga información sobre cómo iniciar las máquinas virtuales en un laboratorio en Azure DevTest Labs mediante runbooks de Azure Automation.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/01/2019
ms.author: spelluru
ms.openlocfilehash: d80328943ae818b3bad9c0a275b74968ee33d4b7
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2019
ms.locfileid: "58887253"
---
# <a name="start-virtual-machines-in-a-lab-in-order-by-using-azure-automation-runbooks"></a>Iniciar máquinas virtuales en un laboratorio de pedido mediante el uso de runbooks de Azure Automation
El [autostart](devtest-lab-set-lab-policy.md#set-autostart) característica de DevTest Labs le permite configurar las máquinas virtuales para iniciarse automáticamente en un momento determinado. Sin embargo, esta característica no es compatible con máquinas se inicien en un orden específico. Hay varios escenarios donde sería útil este tipo de automatización.  Un escenario es donde un Jumpbox VM dentro de un laboratorio debe iniciarse en primer lugar, antes de las máquinas virtuales, como el Jumpbox se utiliza como el punto de acceso a las otras máquinas virtuales.  Este artículo muestra cómo configurar una cuenta de Azure Automation con un runbook de PowerShell que se ejecuta una secuencia de comandos. La secuencia de comandos utiliza etiquetas en las máquinas virtuales en el laboratorio para permitirle controlar el orden de inicio sin tener que cambiar la secuencia de comandos.

## <a name="setup"></a>Configuración
En este ejemplo, las máquinas virtuales en el laboratorio deben tener la etiqueta **StartupOrder** agregado con el valor apropiado (0,1,2, etcetera.). Designó ninguna máquina que no es necesario iniciar como -1.

## <a name="create-an-azure-automation-account"></a>Creación de una cuenta de Azure Automation
Crear una cuenta de Azure Automation siguiendo las instrucciones de [en este artículo](../automation/automation-create-standalone-account.md). Elija la **cuentas de ejecución** opción al crear la cuenta. Una vez creada la cuenta de automation, abra el **módulos** página y seleccione **actualizar módulos de Azure** en la barra de menús. Los módulos predeterminados son que varias versiones antiguas y sin la actualización de la secuencia de comandos no funcionen.

## <a name="add-a-runbook"></a>Agregar un runbook
Ahora, para agregar un runbook a la cuenta de automation, seleccione **Runbooks** en el menú izquierdo. Seleccione **agregar un runbook** en el menú y siga las instrucciones para [crear un runbook de PowerShell](../automation/automation-first-runbook-textual-powershell.md).

## <a name="powershell-script"></a>Script de PowerShell
El script siguiente toma el nombre de la suscripción, el nombre de laboratorio como parámetros. Es el flujo de la secuencia de comandos obtener todas las máquinas virtuales en el laboratorio y, a continuación, analizar la información de etiqueta para crear una lista de los nombres de máquina virtual y su orden de inicio. El script le guía a través de las máquinas virtuales en orden e inicia a las máquinas virtuales. Si hay varias máquinas virtuales en un número de pedido específico, se inician de forma asincrónica con trabajos de PowerShell. Para esas máquinas virtuales que no tienen una etiqueta, establecer el valor de inicio en el último (10), iniciará últimos, de forma predeterminada.  Si no desea que el laboratorio de la máquina virtual se inicia automáticamente, establezca el valor de etiqueta en 11 y se omitirá.

```powershell
#Requires -Version 3.0
#Requires -Module AzureRM.Resources

param
(
    [Parameter(Mandatory=$false, HelpMessage="Name of the subscription that has the lab")]
    [string] $SubscriptionName,

    [Parameter(Mandatory=$false, HelpMessage="Lab name")]
    [string] $LabName
)

# Connect and add the appropriate subscription
$Conn = Get-AutomationConnection -Name AzureRunAsConnection

Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationID $Conn.ApplicationId -Subscription $SubscriptionName -CertificateThumbprint $Conn.CertificateThumbprint

# Find the lab
$dtLab = Find-AzureRmResource -ResourceType 'Microsoft.DevTestLab/labs' -ResourceNameEquals $LabName

# Get the VMs
$dtlAllVms = New-Object System.Collections.ArrayList
$AllVMs = Get-AzureRmResource -ResourceId "$($dtLab.ResourceId)/virtualmachines" -ApiVersion 2016-05-15

# Get the StartupOrder tag, if missing set to be run last (10)
ForEach ($vm in $AllVMs) {
    if ($vm.Tags) {
        if ($vm.Tags['StartupOrder']) {
            $startupValue = $vm.Tags['StartupOrder']
        } else {
            $startupValue = 10
        }
        } else {
            $startupValue = 10
        }
        $dtlAllVms.Add(@{$vm.Name = $startupValue}) > $null
}

# Setup for the async multiple vm start

# Save profile
$profilePath = Join-Path $env:Temp "profile.json"
If (Test-Path $profilePath){
    Remove-Item $profilePath
}
Save-AzureRmContext -Path $profilePath

# Job to start VMs asynch
$startVMBlock = {
    Param($devTestLab,$vmToStart,$profilePath)
    Import-AzureRmContext -Path ($profilePath)
    Invoke-AzureRmResourceAction `
        -ResourceId "$($devTestLab.ResourceId)/virtualmachines/$vmToStart" `
        -Action Start `
        -Force
    Write-Output "Started: $vmToStart"
}

$current = 0
# Start in order from 0 to 10

While ($current -le 10) {
# Get the VMs in the current stage
    $tobeStarted = $null
    $tobeStarted = $dtlAllVms | Where-Object { $_.Values -eq $current}
    if ($tobeStarted.Count -eq 1) {
        # Run sync – jobs not necessary for a single VM
        $returnStatus = Invoke-AzureRmResourceAction `
                -ResourceId "$($dtLab.ResourceId)/virtualmachines/$($tobeStarted.Keys)" `
                -Action Start `
                -Force
        Write-Output "$($tobeStarted.Keys) status: $($returnStatus.status)"
    } elseif ($tobeStarted.Count -gt 1) {
        # Start multiple VMs async
        $jobs = @()
        Write-Output "Start Jobs start: $(Get-Date)"
        
        # Jobs
        $jobs += Start-Job -ScriptBlock $startVMBlock -ArgumentList $dtLab, $($singlevm.Keys), $profilePath
        Write-Output "Start Jobs end: $(Get-Date)"
    }

    # Get results from all jobs
    if($jobs.Count -ne 0) {
        Write-Output "Receive Jobs start: $(Get-Date)"
        foreach ($job in $jobs){
            $jobResult = Receive-Job -Job $job -Wait | Write-Output
        }
        Remove-Job -Job $jobs -Force
    }
    else
    {
        Write-Output "Information: No jobs available"
    }
}
```

## <a name="create-a-schedule"></a>Crear una programación
Para hacer que esta secuencia de comandos se ejecuten diariamente, [crear una programación](../automation/shared-resources/schedules.md#creating-a-schedule) en la cuenta de automation. Una vez que se creó la programación, [vincularlo al runbook](../automation/shared-resources/schedules.md#linking-a-schedule-to-a-runbook). 

En una situación a gran escala donde hay varias suscripciones con varios laboratorios, almacenar la información de parámetros en un archivo para distintos laboratorios y pase el archivo a la secuencia de comandos en lugar de los parámetros individuales. El script deberá modificarse, pero la ejecución en el núcleo sería el mismo. Aunque este ejemplo utiliza la automatización de Azure para ejecutar el script de PowerShell, existen otras opciones, como el uso de una tarea en una canalización de compilación o versión.

## <a name="next-steps"></a>Pasos siguientes
Consulte el siguiente artículo para obtener más información sobre Azure Automation: [Una introducción a Azure Automation](../automation/automation-intro.md).
