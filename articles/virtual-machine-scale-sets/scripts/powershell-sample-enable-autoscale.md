---
title: 'Ejemplos de Azure PowerShell: habilitación del escalado automático basado en el host | Microsoft Docs'
description: Ejemplos de Azure PowerShell
services: virtual-machine-scale-sets
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: ee5103434ed1ad7e123b6ce4ed67809ac11fd2b0
ms.sourcegitcommit: b4755b3262c5b7d546e598c0a034a7c0d1e261ec
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2019
ms.locfileid: "54882793"
---
# <a name="automatically-scale-a-virtual-machine-scale-set-with-powershell"></a>Escalado automático de un conjunto de escalado de máquinas virtuales con PowerShell
Este script crea un conjunto de escalado de máquinas virtuales que ejecuta Windows Server 2016 y usa métricas basadas en el host para escalar automáticamente a medida que la carga de CPU cambia.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Script de ejemplo
[!code-powershell[main](../../../powershell_scripts/virtual-machine-scale-sets/auto-scale-host-metrics/auto-scale-host-metrics.ps1 "Automatically scale a virtual machine scale set")]

## <a name="clean-up-deployment"></a>Limpieza de la implementación
Ejecute el siguiente comando para quitar el grupo de recursos, el conjunto de escalado y todos los recursos relacionados.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Explicación del script
Este script usa los siguientes comandos para crear la implementación. Cada elemento de la tabla incluye vínculos a la documentación específica del comando.

| Get-Help | Notas |
|---|---|
| [New-AzureRmVmss](/powershell/module/azurerm.compute/new-azurermvmss) | Crea el conjunto de escalado de máquinas virtuales y todos los recursos auxiliares, como la red virtual, el equilibrador de carga y las reglas NAT. |
| [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss) | Obtiene información sobre un conjunto de escalado de máquinas virtuales. |
| [Add-AzureRmVmssExtension](/powershell/module/azurerm.compute/add-azurermvmssextension) | Agrega una extensión de máquina virtual para script personalizado para instalar una aplicación web básica. |
| [Update-AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss) | Actualiza el modelo de conjunto de escalado de máquinas virtuales para aplicar la extensión de máquina virtual. |
| [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) | Obtiene información sobre la dirección IP pública asignada que usa el equilibrador de carga. |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Quita un grupo de recursos y todos los recursos incluidos en él. |

## <a name="next-steps"></a>Pasos siguientes
Para obtener más información sobre el módulo de Azure PowerShell, consulte la [documentación de Azure PowerShell](/powershell/azure/overview).

Se pueden encontrar otros ejemplos de scripts de PowerShell de conjunto de escalado de máquinas virtuales en la [documentación del conjunto de escalado de máquinas virtuales de Azure](../powershell-samples.md).