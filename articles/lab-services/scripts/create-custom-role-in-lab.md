---
title: 'Script de PowerShell: creación de un rol personalizado en un laboratorio de Azure DevTest Labs | Microsoft Docs'
description: Este script de PowerShell agrega un usuario externo a un laboratorio de Azure DevTest Labs.
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
ms.openlocfilehash: 295f742342fba7d77b556724c8005f3ac4816482
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2018
ms.locfileid: "34636722"
---
# <a name="use-powershell-to-create-a-custom-role-in-a-lab-in-azure-devtest-labs"></a>Usar PowerShell para crear un rol personalizado en un laboratorio de Azure DevTest Labs

Este script de PowerShell de ejemplo crea un rol personalizado para usarlo en un laboratorio de Azure DevTest Labs. 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="prerequisites"></a>requisitos previos
* **Un laboratorio**. Este script requiere que disponga de un laboratorio existente. 

## <a name="sample-script"></a>Script de ejemplo

[!code-powershell[main](../../../powershell_scripts/devtest-lab/create-custom-role-in-lab/create-custom-role-in-lab.ps1 "Add external user to a lab")]

## <a name="script-explanation"></a>Explicación del script

Este script usa los siguientes comandos: 

| Get-Help | Notas |
|---|---|
| [Get-AzureRmProviderOperation](/powershell/module/azurerm.resources/get-azurermprovideroperation) | Obtiene las operaciones de un proveedor de recursos de Azure que son protegibles con RBAC de Azure. |
| [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition) | Enumera todos los roles de RBAC de Azure disponibles para asignación. |
| [New-AzureRmRoleDefinition](/powershell/module/azurerm.resources/new-azurermroledefinition) | Crea un rol personalizado. |

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Azure PowerShell, consulte la [documentación de Azure PowerShell](https://docs.microsoft.com/powershell/).

Encontrará más ejemplos de script de PowerShell de Azure Lab Services en los [ejemplos de PowerShell de Azure Lab Services](../samples-powershell.md).