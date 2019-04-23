---
title: Usar herramientas de línea de comandos para iniciar y detener las máquinas virtuales de Azure DevTest Labs | Microsoft Docs
description: Obtenga información sobre cómo usar las herramientas de línea de comandos para iniciar y detener las máquinas virtuales en Azure DevTest Labs.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/25/2019
ms.author: spelluru
ms.openlocfilehash: 51c45fdb0c96e84d3f37f485279aa805361f3818
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59798936"
---
# <a name="use-command-line-tools-to-start-and-stop-azure-devtest-labs-virtual-machines"></a>Usar herramientas de línea de comandos para iniciar y detener las máquinas virtuales de Azure DevTest Labs
Este artículo muestra cómo usar Azure PowerShell o CLI de Azure para iniciar o detener las máquinas virtuales en un laboratorio en Azure DevTest Labs. Puede crear scripts de PowerShell o CLI para automatizar estas operaciones. 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="overview"></a>Información general
Azure DevTest Labs es una manera de crear entornos de desarrollo y pruebas rápidos, sencillos y ligeros. Permite administrar los costos, aprovisionar máquinas virtuales rápidamente y minimizar el desperdicio.  Hay características integradas en el portal de Azure que le permiten configurar las máquinas virtuales en un laboratorio para iniciar y detener a horas específicas automáticamente. 

Sin embargo, en algunos escenarios, puede automatizar el inicio y detención de máquinas virtuales de los scripts de PowerShell o CLI. Ofrece cierta flexibilidad con el inicio y detención de máquinas individuales en cualquier momento, en lugar de en momentos concretos. Estas son algunas de las situaciones en que se ejecuta estas tareas mediante el uso de secuencias de comandos sería útiles.

- Cuando se usa una aplicación de capa 3 como parte de un entorno de prueba, los niveles deben iniciarse en una secuencia. 
- Desactivar una máquina virtual cuando se cumple un criterio personalizada para ahorrar dinero. 
- Usar como una tarea dentro de un flujo de trabajo de CI/CD para comenzar al principio del flujo, usar las máquinas virtuales como equipos de compilación, máquinas o la infraestructura de prueba y luego detener las máquinas virtuales cuando se completa el proceso. Un ejemplo de esto sería el generador de imágenes personalizadas con Azure DevTest Labs.  

## <a name="azure-powershell"></a>Azure PowerShell
El siguiente script de PowerShell inicia una máquina virtual en un laboratorio. [AzResourceAction invocar](/powershell/module/az.resources/invoke-azresourceaction?view=azurermps-6.13.0) es el enfoque principal para esta secuencia de comandos. El **ResourceId** parámetro es el identificador de recurso completo para la máquina virtual en el laboratorio. El **acción** parámetro es donde el **iniciar** o **detener** opciones se establecen según lo que se necesita.

```powershell
# The id of the subscription
$subscriptionId = "111111-11111-11111-1111111"

# The name of the lab
$devTestLabName = "yourlabname"

# The name of the virtual machine to be started
$vMToStart = "vmname"

# The action on the virtual machine (Start or Stop)
$vmAction = "Start"

# Select the Azure subscription
Select-AzSubscription -SubscriptionId $subscriptionId

# Get the lab information
if ($(Get-Module -Name AzureRM).Version.Major -eq 6) {
    $devTestLab = Get-AzResource -ResourceType 'Microsoft.DevTestLab/labs' -Name $devTestLabName
} else {
    $devTestLab = Find-AzResource -ResourceType 'Microsoft.DevTestLab/labs' -ResourceNameEquals $devTestLabName
}

# Start the VM and return a succeeded or failed status
$returnStatus = Invoke-AzResourceAction `
                    -ResourceId "$($devTestLab.ResourceId)/virtualmachines/$vMToStart" `
                    -Action $vmAction `
                    -Force

if ($returnStatus.Status -eq 'Succeeded') {
    Write-Output "##[section] Successfully updated DTL machine: $vMToStart, Action: $vmAction"
}
else {
    Write-Error "##[error]Failed to update DTL machine: $vMToStart, Action: $vmAction"
}
```


## <a name="azure-cli"></a>Azure CLI
El [CLI de Azure](/cli/azure/get-started-with-azure-cli?view=azure-cli-latest) es otra manera de automatizar el inicio y detención de máquinas virtuales de DevTest Labs. CLI de Azure puede ser [instalado](/cli/azure/install-azure-cli?view=azure-cli-latest) en diferentes sistemas operativos. El siguiente script proporciona comandos para iniciar y detener una máquina virtual en un laboratorio. 

```azurecli
# Sign in to Azure
az login 

## Get the name of the resource group that contains the lab
az resource list --resource-type "Microsoft.DevTestLab/labs" --name "yourlabname" --query "[0].resourceGroup" 

## Start the VM
az lab vm start --lab-name yourlabname --name vmname --resource-group labResourceGroupName

## Stop the VM
az lab vm stop --lab-name yourlabname --name vmname --resource-group labResourceGroupName
```


## <a name="next-steps"></a>Pasos siguientes
Consulte el artículo siguiente para usar el portal de Azure para realizar estas operaciones: [Reiniciar una máquina virtual](devtest-lab-restart-vm.md).
