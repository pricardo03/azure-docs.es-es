---
title: 'Ejemplo de script de Azure PowerShell: escalado de la instancia de servicio | Microsoft Docs'
description: 'Ejemplo de script de Azure PowerShell: escalado de la instancia de servicio'
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.devlang: na
ms.topic: sample
ms.date: 11/16/2017
ms.author: apimpm
ms.custom: mvc
ms.openlocfilehash: 4281cad8ccecb9b36caa5ff9ed662e5f6c6f40c5
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2019
ms.locfileid: "54426563"
---
# <a name="scale-the-service-instance"></a>Escalado de la instancia de servicio

Este script de ejemplo escala la instancia de servicio de API Management y le agrega una región.

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Si decide instalar y usar PowerShell localmente, para este tutorial se requiere la versión 3.6 del módulo de Azure PowerShell, o cualquier versión posterior. Ejecute ` Get-Module -ListAvailable AzureRM` para encontrar la versión. Si necesita actualizarla, consulte [Instalación del módulo de Azure PowerShell](/powershell/azure/azurerm/install-azurerm-ps). Si PowerShell se ejecuta localmente, también debe ejecutar `Connect-AzureRmAccount` para crear una conexión con Azure.

## <a name="sample-script"></a>Script de ejemplo

[!code-powershell[main](../../../powershell_scripts/api-management/scale-and-addregion-apim-service/scale_and_addregion_apim_service.ps1 "Scale the APIM service instance")]

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no se necesiten, puede usar el comando [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) para quitar el grupo de recursos y todos los recursos relacionados.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre el módulo de Azure PowerShell, consulte la [documentación de Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).

Puede encontrar ejemplos de Azure PowerShell para Azure API Management en los [ejemplos de PowerShell](../powershell-samples.md).
