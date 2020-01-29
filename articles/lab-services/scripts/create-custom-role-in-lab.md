---
title: 'PowerShell: Creación de un rol personalizado en un laboratorio de Azure DevTest Labs'
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
ms.openlocfilehash: bcad4e02105bff2dc78a4adc3ca614f997794d52
ms.sourcegitcommit: d29e7d0235dc9650ac2b6f2ff78a3625c491bbbf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/17/2020
ms.locfileid: "76166435"
---
# <a name="use-powershell-to-create-a-custom-role-in-a-lab-in-azure-devtest-labs"></a>Usar PowerShell para crear un rol personalizado en un laboratorio de Azure DevTest Labs

Este script de PowerShell de ejemplo crea un rol personalizado para usarlo en un laboratorio de Azure DevTest Labs. 

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh-az.md)]

## <a name="prerequisites"></a>Prerequisites
* **Un laboratorio**. Este script requiere que disponga de un laboratorio existente. 

## <a name="sample-script"></a>Script de ejemplo

[!code-powershell[main](../../../powershell_scripts/devtest-lab/create-custom-role-in-lab/create-custom-role-in-lab.ps1 "Add external user to a lab")]

## <a name="script-explanation"></a>Explicación del script

Este script usa los siguientes comandos: 

| Get-Help | Notas |
|---|---|
| [Get-AzProviderOperation](/powershell/module/az.resources/get-azprovideroperation) | Obtiene las operaciones de un proveedor de recursos de Azure que son protegibles con RBAC de Azure. |
| [Get-AzRoleDefinition](/powershell/module/az.resources/get-azroledefinition) | Enumera todos los roles de RBAC de Azure disponibles para asignación. |
| [New-AzRoleDefinition](/powershell/module/az.resources/new-azroledefinition) | Crea un rol personalizado. |

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Azure PowerShell, consulte la [documentación de Azure PowerShell](https://docs.microsoft.com/powershell/).

Encontrará más ejemplos de script de PowerShell de Azure Lab Services en los [ejemplos de PowerShell de Azure Lab Services](../samples-powershell.md).
