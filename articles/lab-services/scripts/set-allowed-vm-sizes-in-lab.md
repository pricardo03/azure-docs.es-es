---
title: 'Script de PowerShell: Establecimiento de tamaños de máquina virtual permitidos en Azure Lab Services | Microsoft Docs'
description: Este script de PowerShell establece los tamaños de máquina virtual permitidos en Azure Lab Services.
services: lab-services
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/11/2018
ms.author: spelluru
ms.openlocfilehash: 0c82e304d3e3d8df1206c7c05883399b74229af7
ms.sourcegitcommit: cdf0e37450044f65c33e07aeb6d115819a2bb822
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57192197"
---
# <a name="use-powershell-to-set-allowed-vm-sizes-in-azure-lab-services"></a>Uso de PowerShell para establecer los tamaños de máquina virtual permitidos en Azure Lab Services

Este script de PowerShell de ejemplo establece los tamaños de máquina virtual en Azure Lab Services.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh-az.md)]

## <a name="prerequisites"></a>Requisitos previos
* **Un laboratorio**. Este script requiere que disponga de un laboratorio existente. 

## <a name="sample-script"></a>Script de ejemplo

[!code-powershell[main](../../../powershell_scripts/devtest-lab/set-allowed-vm-sizes-in-lab/set-allowed-vm-sizes-in-lab.ps1 "Add external user to a lab")]

## <a name="script-explanation"></a>Explicación del script

Este script usa los siguientes comandos: 

| Get-Help | Notas |
|---|---|
| Find-AzResource | Busca recursos en función de los parámetros especificados. |
| [Get-AzResource](/powershell/module/az.resources/get-azresource) | Obtiene recursos. |
| [Set-AzResource](/powershell/module/az.resources/set-azresource) | Modifica un recurso. |
| [New-AzResource](/powershell/module/az.resources/new-azresource) | Crea un recurso. |

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Azure PowerShell, consulte la [documentación de Azure PowerShell](https://docs.microsoft.com/powershell/).

Encontrará más ejemplos de script de PowerShell de Azure Lab Services en los [ejemplos de PowerShell de Azure Lab Services](../samples-powershell.md).
