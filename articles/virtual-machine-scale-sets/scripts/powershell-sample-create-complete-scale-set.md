---
title: 'Ejemplos de Azure PowerShell: Creación de un conjunto de escalado de máquinas virtuales completo | Microsoft Docs'
description: Ejemplos de Azure PowerShell
services: virtual-machine-scale-sets
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/29/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 5bbdcf8b45af6cf0c56af52941f205267ebdecbd
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2018
ms.locfileid: "34839714"
---
# <a name="create-a-complete-virtual-machine-scale-set-with-powershell"></a>Creación de un conjunto de escalado de máquinas virtuales completo con PowerShell
Este script crea un conjunto de escalado de máquinas virtuales que ejecutan Windows Server 2016. Se configuran y crean recursos individuales, en lugar de usar las [opciones integradas de creación de recursos disponibles aquí en New-AzureRmVmss](powershell-sample-create-simple-scale-set.md). Después de ejecutar el script, puede acceder a las instancias de máquina virtual mediante RDP.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Script de ejemplo
[!code-powershell[main](../../../powershell_scripts/virtual-machine-scale-sets/complete-scale-set/complete-scale-set.ps1 "Create a complete virtual machine scale set")]

## <a name="clean-up-deployment"></a>Limpieza de la implementación
Ejecute el siguiente comando para quitar el grupo de recursos, el conjunto de escalado y todos los recursos relacionados.

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroupName
```

## <a name="script-explanation"></a>Explicación del script
Este script usa los siguientes comandos para crear la implementación. Cada elemento de la tabla incluye vínculos a la documentación específica del comando.

| Get-Help | Notas |
|---|---|
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Crea un grupo de recursos en el que se almacenan todos los recursos. |
| [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | Crea una configuración de subred. Esta configuración se utiliza con el proceso de creación de la red virtual. |
| [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | Crea una red virtual. |
| [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) | Crea una dirección IP pública. |
| [New-AzureRmLoadBalancerFrontendIpConfig](/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig) | Crea una configuración de dirección IP de Front-End para un equilibrador de carga. |
| [New-AzureRmLoadBalancerBackendAddressPoolConfig](/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig) | Crea una configuración de grupo de direcciones de back-end para un equilibrador de carga. |
| [New-AzureRmLoadBalancerInboundNatRuleConfig](/powershell/module/azurerm.network/new-azurermloadbalancerinboundnatruleconfig) | Crea una configuración de regla NAT entrante para un equilibrador de carga. |
| [New-AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer) | Crea un equilibrador de carga. |
| [Add-AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/new-azurermloadbalancerprobeconfig) | Crea una configuración de sondeo para un equilibrador de carga. |
| [Add-AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/new-azurermloadbalancerruleconfig) | Crea una configuración de regla para un equilibrador de carga. |
| [Set-AzureRmLoadBalancer](/powershell/module/AzureRM.Network/Set-AzureRmLoadBalancer) | Actualice el equilibrador de carga con la información que se proporciona. |
| [New-AzureRmVmssIpConfig](/powershell/module/AzureRM.Compute/New-AzureRmVmssIpConfig) | Cree una configuración de IP para las instancias de máquina virtual del conjunto de escalado. Las instancias de máquina virtual están conectadas al grupo de back-end del equilibrador de carga, a un grupo de NAT y a una subred de la red virtual. |
| [New-AzureRmVmssConfig](/powershell/module/AzureRM.Compute/New-AzureRmVmssConfig) | Crea una configuración del conjunto de escalado. Dicha configuración incluye información como el número de instancias de máquina virtual que se crean, la SKU de la máquina virtual (tamaño) y el modo de la directiva de actualización. La configuración se agrega a los cmdlets adicionales y se usa durante la creación del conjunto de escalado. |
| [Set-AzureRmVmssStorageProfile](/powershell/module/AzureRM.Compute/Set-AzureRmVmssStorageProfile) | Defina la imagen que se va a utilizar para las instancias de máquina virtual y agréguela a la configuración del conjunto de escalado. |
| [Set-AzureRmVmssOsProfile](/powershell/module/AzureRM.Compute/Set-AzureRmVmssStorageProfile) | Defina las credenciales del nombre de usuario y la contraseña de administración, así como el prefijo de asignación de nombres de máquina virtual. Agregue estos valores a la configuración del conjunto de escalado. |
| [Add-AzureRmVmssNetworkInterfaceConfiguration](/powershell/module/AzureRM.Compute/Add-AzureRmVmssNetworkInterfaceConfiguration) | Agregue una interfaz de red virtual a las instancias de máquina virtual, en función de la configuración de la IP. Agregue estos valores a la configuración del conjunto de escalado. |
| [New-AzureRmVmss](/powershell/module/AzureRM.Compute/New-AzureRmVmss) | Cree el conjunto de escalado en función de la información proporcionada en la configuración del conjunto de escalado. |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Quita un grupo de recursos y todos los recursos incluidos en él. |

## <a name="next-steps"></a>Pasos siguientes
Para obtener más información sobre el módulo de Azure PowerShell, consulte la [documentación de Azure PowerShell](/powershell/azure/overview).

Se pueden encontrar otros ejemplos de scripts de PowerShell de conjunto de escalado de máquinas virtuales en la [documentación del conjunto de escalado de máquinas virtuales de Azure](../powershell-samples.md).
