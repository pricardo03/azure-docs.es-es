---
title: 'Ejemplos de Azure PowerShell: Conjunto de escalado con redundancia de zona | Microsoft Docs'
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
ms.date: 04/05/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 00dd839c3ef8369490ffdd8840d6d4d8c69c76b5
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/09/2019
ms.locfileid: "55983889"
---
# <a name="create-a-zone-redundant-virtual-machine-scale-set-with-powershell"></a>Creación de un conjunto de escalado de máquinas virtuales con redundancia de zona con PowerShell
Este script crea un conjunto de escalado de máquinas virtuales que ejecutan Windows Server 2016 en varias zonas de disponibilidad. Después de ejecutar el script, puede acceder a la máquina virtual a través de RDP.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [updated-for-az-vm.md](../../../includes/updated-for-az-vm.md)]

## <a name="sample-script"></a>Script de ejemplo

[!code-powershell[main](../../../powershell_scripts/virtual-machine-scale-sets/create-zone-redundant-scale-set/create-zone-redundant-scale-set.ps1 "Create zone-redundant scale set")]

## <a name="clean-up-deployment"></a>Limpieza de la implementación
Ejecute el siguiente comando para quitar el grupo de recursos, el conjunto de escalado y todos los recursos relacionados.

```powershell
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Explicación del script
Este script usa los siguientes comandos para crear la implementación. Cada elemento de la tabla incluye vínculos a la documentación específica del comando.

| Get-Help | Notas |
|---|---|
| [New-AzVmss](/powershell/module/az.compute/new-azvmss) | Crea el conjunto de escalado de máquinas virtuales y todos los recursos auxiliares, como la red virtual, el equilibrador de carga y las reglas NAT. |
| [Get-AzVmss](/powershell/module/az.compute/get-azvmss) | Obtiene información sobre un conjunto de escalado de máquinas virtuales. |
| [Add-AzVmssExtension](/powershell/module/az.compute/add-azvmssextension) | Agrega una extensión de máquina virtual para script personalizado para instalar una aplicación web básica. |
| [Update-AzVmss](/powershell/module/az.compute/update-azvmss) | Actualiza el modelo de conjunto de escalado de máquinas virtuales para aplicar la extensión de máquina virtual. |
| [Get-AzPublicIpAddress](/powershell/module/az.network/get-azpublicipaddress) | Obtiene información sobre la dirección IP pública asignada que usa el equilibrador de carga. |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Quita un grupo de recursos y todos los recursos incluidos en él. |


## <a name="next-steps"></a>Pasos siguientes
Para obtener más información sobre el módulo de Azure PowerShell, consulte la [documentación de Azure PowerShell](/powershell/azure/overview).

Se pueden encontrar otros ejemplos de scripts de PowerShell de conjunto de escalado de máquinas virtuales en la [documentación del conjunto de escalado de máquinas virtuales de Azure](../powershell-samples.md).
