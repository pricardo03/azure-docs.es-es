---
title: 'Script de Azure PowerShell: actualizar una cuenta de Azure Cosmos'
description: 'Ejemplo de script de Azure PowerShell: actualizar una cuenta de Azure Cosmos con regiones agregadas'
author: markjbrown
ms.service: cosmos-db
ms.topic: samples
ms.date: 05/06/2019
ms.author: mjbrown
ms.openlocfilehash: 4a695fd569459e6372465273675620cd40d7e6a7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66247835"
---
# <a name="update-an-azure-cosmos-account-and-add-a-region-using-powershell"></a>Actualizar una cuenta de Azure Cosmos y agregue una región mediante PowerShell.

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>Script de ejemplo

[!code-powershell[main](../../../../../powershell_scripts/cosmosdb/sql/ps-account-update.ps1 "Update and add regions to an Azure Cosmos account")]

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
| [Get-AzResource](https://docs.microsoft.com/powershell/module/az.resources/get-azresource) | Obtiene un recurso. |
| [Set-AzResource](https://docs.microsoft.com/powershell/module/az.resources/set-azresource) | Actualiza un recurso. |
|**Grupos de recursos de Azure**| |
| [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) | Crea un grupo de recursos en el que se almacenan todos los recursos. |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | Elimina un grupo de recursos, incluidos todos los recursos anidados. |
|||

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Azure PowerShell, consulte la [documentación de Azure PowerShell](https://docs.microsoft.com/powershell/).

Encontrará más ejemplos de scripts de PowerShell de Azure Cosmos DB en los [scripts de PowerShell de Azure Cosmos DB](../../../powershell-samples.md).