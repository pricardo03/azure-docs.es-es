---
title: Asignación de un acceso de identidad administrada a un recurso de Azure mediante PowerShell
description: Instrucciones paso a paso para asignar una identidad administrada en un recurso y acceso a otro recurso, mediante PowerShell.
services: active-directory
documentationcenter: ''
author: daveba
manager: daveba
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/06/2018
ms.author: daveba
ms.openlocfilehash: 72e05af92e88dc04f470d8be9a65347672777556
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2019
ms.locfileid: "54427668"
---
# <a name="assign-a-managed-identity-access-to-a-resource-using-powershell"></a>Asignación de un acceso de identidad administrada a un recurso mediante PowerShell

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Una vez que haya configurado un recurso de Azure con una identidad administrada, puede dar acceso de identidad administrada a otro recurso, al igual que cualquier entidad de seguridad. En este ejemplo se muestra cómo otorgar acceso de identidad administrada a una máquina virtual de Azure para una cuenta de almacenamiento de Azure, mediante PowerShell.

## <a name="prerequisites"></a>Requisitos previos

- Si no está familiarizado con las identidades administradas de los recursos de Azure, consulte la [sección de introducción](overview.md). **No olvide revisar la [diferencia entre una identidad administrada asignada por el sistema y una identidad administrada asignada por el usuario](overview.md#how-does-it-work)**.
- Si aún no tiene una cuenta de Azure, [regístrese para una cuenta gratuita](https://azure.microsoft.com/free/) antes de continuar.
- Instale [la versión más reciente de Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM) si aún no lo ha hecho.

## <a name="use-rbac-to-assign-a-managed-identity-access-to-another-resource"></a>Uso de RBAC para asignar el acceso de una identidad administrada a otro recurso

Después de habilitar la identidad administrada en un recurso de Azure, [como una máquina virtual de Azure](qs-configure-powershell-windows-vm.md):

1. Inicie sesión en Azure mediante el cmdlet `Connect-AzureRmAccount`. Use una cuenta asociada a la suscripción de Azure en la que haya configurado la identidad administrada:

   ```powershell
   Connect-AzureRmAccount
   ```
2. En este ejemplo, vamos a dar a una máquina virtual de Azure acceso a una cuenta de almacenamiento. En primer lugar, se usa [Get- AzureRMVM](/powershell/module/azurerm.compute/get-azurermvm) para obtener la entidad de servicio para la máquina virtual denominada `myVM`, que se creó cuando se habilitó la identidad administrada. A continuación, usamos [New-AzureRmRoleAssignment](/powershell/module/AzureRM.Resources/New-AzureRmRoleAssignment) para conceder a la máquina virtual acceso de **lectura** a una cuenta de almacenamiento denominada `myStorageAcct`:

    ```powershell
    $spID = (Get-AzureRMVM -ResourceGroupName myRG -Name myVM).identity.principalid
    New-AzureRmRoleAssignment -ObjectId $spID -RoleDefinitionName "Reader" -Scope "/subscriptions/<mySubscriptionID>/resourceGroups/<myResourceGroup>/providers/Microsoft.Storage/storageAccounts/<myStorageAcct>"
    ```

## <a name="next-steps"></a>Pasos siguientes

- [Identidades administradas de recursos de Azure](overview.md)
- Para habilitar la identidad administrada de una máquina virtual de Azure, consulte [Configuración de identidades administradas de recursos de Azure en una VM de Azure mediante PowerShell](qs-configure-powershell-windows-vm.md).
