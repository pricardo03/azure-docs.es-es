---
title: 'Script de PowerShell: adición de una imagen de Marketplace a un laboratorio de Azure DevTest Labs | Microsoft Docs'
description: Este script de PowerShell agrega una imagen de Marketplace a un laboratorio de Azure DevTest Labs.
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
ms.openlocfilehash: 9a65237b3eba6c9878d73148f0143f20dd60dd79
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2018
ms.locfileid: "34636555"
---
# <a name="use-powershell-to-add-a-marketplace-image-to-a-lab-in-azure-devtest-labs"></a>Usar PowerShell para agregar una imagen de Marketplace a un laboratorio de Azure DevTest Labs

Este script de PowerShell de ejemplo agrega una imagen de Marketplace a un laboratorio de Azure DevTest Labs. 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="prerequisites"></a>requisitos previos
* **Un laboratorio**. Este script requiere que disponga de un laboratorio existente. 

## <a name="sample-script"></a>Script de ejemplo

[!code-powershell[main](../../../powershell_scripts/devtest-lab/add-marketplace-images-to-lab/add-marketplace-images-to-lab.ps1 "Add marketplace images to a lab")]

## <a name="script-explanation"></a>Explicación del script

Este script usa los siguientes comandos: 

| Get-Help | Notas |
|---|---|
| [Find-AzureRmResource](/module/azurerm.resources/find-azurermresource) | Busca recursos en función de los parámetros especificados. |
| [Get-AzureRmResource](/powershell/module/azurerm.resources/get-azurermresource) | Obtiene recursos. |
| [Set-AzureRmResource](/powershell/module/azurerm.resources/set-azurermresource) | Modifica un recurso. |
| [New-AzureRmResource](/powershell/module/azurerm.resources/new-azurermresource) | Crea un recurso. |

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Azure PowerShell, consulte la [documentación de Azure PowerShell](https://docs.microsoft.com/powershell/).

Encontrará más ejemplos de script de PowerShell de Azure Lab Services en los [ejemplos de PowerShell de Azure Lab Services](../samples-powershell.md).