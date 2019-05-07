---
title: 'Script de Azure PowerShell: cambiar la prioridad de conmutación por error para una cuenta de Azure Cosmos'
description: 'Ejemplo de script de Azure PowerShell: cambiar la prioridad de conmutación por error para una cuenta de Azure Cosmos'
author: markjbrown
ms.service: cosmos-db
ms.topic: samples
ms.date: 05/06/2019
ms.author: mjbrown
ms.openlocfilehash: a52f43e9e7508801ea7db9f2bfe7297d0367e629
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65079306"
---
# <a name="change-failover-priority-for-an-azure-cosmos-account-using-powershell"></a>Cambiar la prioridad de conmutación por error de una cuenta de Azure Cosmos mediante PowerShell

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>Script de ejemplo

[!code-powershell[main](../../../../../powershell_scripts/cosmosdb/sql/ps-account-failover-priority-update.ps1 "Update failover priority for an Azure Cosmos account")]

## <a name="clean-up-deployment"></a>Limpieza de la implementación

Después de ejecutar el script de ejemplo, se puede usar el comando siguiente para quitar el grupo de recursos y todos los recursos asociados.

```powershell
Remove-AzResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a>Explicación del script

Este script usa los siguientes comandos. Cada comando de la tabla crea un vínculo a documentación específica del comando.

| Get-Help | Notas |
|---|---|
|**Recursos de Azure**| |
| [Invoke-AzResourceAction](https://docs.microsoft.com/powershell/module/az.resources/invoke-azresourceaction) | Invoca una acción en un recurso. |
|**Grupos de recursos de Azure**| |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | Elimina un grupo de recursos, incluidos todos los recursos anidados. |
|||

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Azure PowerShell, consulte la [documentación de Azure PowerShell](https://docs.microsoft.com/powershell/).

Encontrará más ejemplos de scripts de PowerShell de Azure Cosmos DB en los [scripts de PowerShell de Azure Cosmos DB](../../../powershell-samples.md).