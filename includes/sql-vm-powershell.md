---
author: MikeRayMSFT
ms.service: virtual-machines-sql
ms.topic: include
ms.date: 11/25/2018
ms.author: mikeray
ms.openlocfilehash: e81cdb478a63e1e584aef2c32754bd321d245365
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/27/2018
ms.locfileid: "52440446"
---
## <a name="start-your-powershell-session"></a>Inicio de una sesión de PowerShell
Primero, debe tener la versión más reciente de [Azure PowerShell](https://msdn.microsoft.com/library/mt619274.aspx) instalada y ejecutándose. Para obtener información detallada, vea [Instalación y configuración de Azure PowerShell](/powershell/azureps-cmdlets-docs).

> [!NOTE]
> En los ejemplos de este tema se usa el [modelo de implementación de Azure Resource Manager](../articles/azure-resource-manager/resource-group-overview.md) y, por consiguiente, usaremos los [cmdlets de Azure Resource Manager](https://msdn.microsoft.com/library/azure/mt125356.aspx). 
> 
> 

Ejecute el cmdlet [**Connect-AzureRmAccount**](https://docs.microsoft.com/powershell/module/azurerm.profile/connect-azurermaccount) y aparecerá una pantalla de inicio de sesión donde podrá especificar sus credenciales. Use las mismas credenciales que las utilizadas para iniciar sesión en el Portal de Azure.

    Connect-AzureRmAccount

Si tiene varias suscripciones, use el cmdlet [**Set-AzureRmContext**](https://docs.microsoft.com/powershell/module/azurerm.profile/set-azurermcontext) para seleccionar la suscripción que se debe usar en la sesión de PowerShell. Para ver la suscripción que se usa en la sesión actual de PowerShell, ejecute [**Get-AzureRmContext**](https://docs.microsoft.com/powershell/module/azurerm.profile/get-azurermcontext). Para ver todas las suscripciones, ejecute [**Get-AzureRmSubscription**](https://docs.microsoft.com/powershell/module/servicemanagement/azurerm.profile/get-azurermsubscription).

    Set-AzureRmContext -SubscriptionId '4cac86b0-1e56-bbbb-aaaa-000000000000'

