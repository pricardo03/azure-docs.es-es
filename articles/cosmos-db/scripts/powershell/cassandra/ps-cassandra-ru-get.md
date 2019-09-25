---
title: 'Script de Azure PowerShell: Obtener procesamiento (RU/s) para Cassandra API en Azure Cosmos DB'
description: 'Script de Azure PowerShell: Obtener procesamiento (RU/s) para Cassandra API en Azure Cosmos DB'
author: markjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: sample
ms.date: 07/03/2019
ms.author: mjbrown
ms.openlocfilehash: b2544a3c704318839f0c2b956db4d39e83d08d3f
ms.sourcegitcommit: 83df2aed7cafb493b36d93b1699d24f36c1daa45
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/22/2019
ms.locfileid: "71179013"
---
# <a name="get-throughput-rus-for-a-keyspace-or-table-for-azure-cosmos-db---cassandra-api"></a>Obtener procesamiento (RU/s) para una tabla en Azure Cosmos DB: Cassandra API

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>Script de ejemplo

[!code-powershell[main](../../../../../powershell_scripts/cosmosdb/cassandra/ps-cassandra-ru-get.ps1 "Get throughput on a keyspace or table for Cassandra API")]

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
| [New-AzResource](https://docs.microsoft.com/powershell/module/az.resources/new-azresource) | Crea un recurso. |
|**Grupos de recursos de Azure**| |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | Elimina un grupo de recursos, incluidos todos los recursos anidados. |
|||

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Azure PowerShell, consulte la [documentación de Azure PowerShell](https://docs.microsoft.com/powershell/).

Encontrará más ejemplos de scripts de PowerShell de Azure Cosmos DB en los [scripts de PowerShell de Azure Cosmos DB](../../../powershell-samples.md).