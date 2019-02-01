---
title: Uso del módulo de directivas de Azure Stack | Microsoft Docs
description: Aprenda a restringir una suscripción de Azure para que se comporte como una suscripción de Azure Stack
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 937ef34f-14d4-4ea9-960b-362ba986f000
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2018
ms.author: sethm
ms.lastreviewed: 11/29/2018
ms.openlocfilehash: 2e1b7257e7ffc4460d86018a6318e33f95e01700
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2019
ms.locfileid: "55246271"
---
# <a name="manage-azure-policy-using-the-azure-stack-policy-module"></a>Administración de la directiva de Azure con el módulo de directivas de Azure Stack

*Se aplica a: Sistemas integrados de Azure Stack y Kit de desarrollo de Azure Stack*

El módulo de directivas de Azure Stack le permite configurar una suscripción de Azure con la misma disponibilidad de servicios y control de versiones que Azure Stack. El módulo utiliza el cmdlet [New-AzureRmPolicyDefinition](/powershell/module/azurerm.resources/new-azurermpolicydefinition) para crear una directiva de Azure. Esta directiva limita los tipos de recursos y los servicios que están disponibles en una suscripción. Después, puede crear una asignación de la directiva dentro del ámbito que corresponda utilizando el cmdlet [New-AzureRmPolicyAssignment](/powershell/module/azurerm.resources/new-azurermpolicyassignment). Después de configurar la directiva, puede usar su suscripción de Azure para desarrollar aplicaciones destinadas a Azure Stack.

## <a name="install-the-module"></a>Instalación del módulo

1. Instale la versión apropiada del módulo AzureRM PowerShell, tal y como se describe en el paso 1 de [Instalación de PowerShell para Azure Stack](azure-stack-powershell-install.md).
2. [Descargue las herramientas de Azure Stack desde GitHub](azure-stack-powershell-download.md).
3. [Configure PowerShell para utilizarlo con Azure Stack](azure-stack-powershell-configure-user.md).
4. Importe el módulo AzureStack.Policy.psm1:

    ```PowerShell
    Import-Module .\Policy\AzureStack.Policy.psm1
    ```

## <a name="apply-policy-to-azure-subscription"></a>Aplicación de la directiva a la suscripción de Azure

Puede utilizar el comando siguiente para aplicar una directiva predeterminada de Azure Stack con su suscripción de Azure. Antes de ejecutar este comando, reemplace `Azure Subscription Name` por el nombre de la suscripción de Azure.

```PowerShell
Add-AzureRmAccount
$s = Select-AzureRmSubscription -SubscriptionName "Azure Subscription Name"
$policy = New-AzureRmPolicyDefinition -Name AzureStackPolicyDefinition -Policy (Get-AzsPolicy)
$subscriptionID = $s.Subscription.SubscriptionId
New-AzureRmPolicyAssignment -Name AzureStack -PolicyDefinition $policy -Scope /subscriptions/$subscriptionID

```

## <a name="apply-policy-to-a-resource-group"></a>Aplicación de la directiva a un grupo de recursos

Puede querer aplicar directivas que sean más granular. Por ejemplo, puede tener otros recursos que se ejecuten en la misma suscripción. Puede delimitar la aplicación de directivas a un grupo de recursos específico, lo que le permite probar sus aplicaciones para Azure Stack con recursos de Azure. Antes de ejecutar el comando siguiente, reemplace `Azure Subscription Name` por el nombre de la suscripción de Azure.

```PowerShell
Add-AzureRmAccount
$rgName = 'myRG01'
$s = Select-AzureRmSubscription -SubscriptionName "Azure Subscription Name"
$policy = New-AzureRmPolicyDefinition -Name AzureStackPolicyDefinition -Policy (Get-AzsPolicy)
$subscriptionID = $s.Subscription.SubscriptionId
New-AzureRmPolicyAssignment -Name AzureStack -PolicyDefinition $policy -Scope /subscriptions/$subscriptionID/resourceGroups/$rgName
```

## <a name="policy-in-action"></a>Directiva en acción

Una vez que haya implementado la directiva de Azure, recibirá un error si intenta implementar un recurso prohibido por la directiva.

![Resultado del error de implementación de un recurso debido a la restricción de la directiva](./media/azure-stack-policy-module/image1.png)

## <a name="next-steps"></a>Pasos siguientes

* [Implementación de plantillas con PowerShell](azure-stack-deploy-template-powershell.md)
* [Implementación de plantillas con la CLI de Azure](azure-stack-deploy-template-command-line.md)
* [Implementación de plantillas con Visual Studio](azure-stack-deploy-template-visual-studio.md)
