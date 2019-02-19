---
title: Ejemplo de PowerShell para restaurar una instancia de Azure SQL Database a partir de una copia de seguridad | Microsoft Docs
description: Script de ejemplo de Azure PowerShell para restaurar base de datos única de Azure SQL a partir de copias de seguridad con redundancia geográfica
services: sql-database
ms.service: sql-database
ms.subservice: backup-restore
ms.custom: ''
ms.devlang: PowerShell
ms.topic: sample
author: mashamsft
ms.author: mathoma
ms.reviewer: carlrab
manager: craigg
ms.date: 02/08/2019
ms.openlocfilehash: 730d944edfc16e9ef399333540b14fcfced4ce44
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/11/2019
ms.locfileid: "55996396"
---
# <a name="use-powershell-to-restore-an-azure-sql-single-database-from-backups"></a>Uso de PowerShell para restaurar una base de datos única de Azure SQL a partir de copias de seguridad

Este script de PowerShell restaura una instancia de Azure SQL Database desde una copia de seguridad con redundancia geográfica y restaura una instancia de Azure SQL Database eliminada a la copia de seguridad más reciente y restaura una instancia de Azure SQL Database a un momento dado.  

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Si decide instalar y usar PowerShell localmente, para este tutorial se requiere la versión 5.7.0 del módulo de Azure PowerShell o cualquier versión posterior. Ejecute `Get-Module -ListAvailable AzureRM` para encontrar la versión. Si necesita actualizarla, consulte [Instalación del módulo de Azure PowerShell](/powershell/azure/install-az-ps). Si PowerShell se ejecuta localmente, también debe ejecutar `Connect-AzureRmAccount` para crear una conexión con Azure.

## <a name="sample-script"></a>Script de ejemplo

[!code-powershell-interactive[main](../../../powershell_scripts/sql-database/restore-database/restore-database.ps1?highlight=17-18 "Create SQL Database")]

## <a name="clean-up-deployment"></a>Limpieza de la implementación

Después de ejecutar el script de ejemplo, se puede usar el comando siguiente para quitar el grupo de recursos y todos los recursos asociados.

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $resourcegroupname
```

## <a name="script-explanation"></a>Explicación del script

Este script usa los siguientes comandos. Cada comando de la tabla crea un vínculo a documentación específica del comando.

| Get-Help | Notas |
|---|---|
| [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup) | Crea un grupo de recursos en el que se almacenan todos los recursos. | [New-AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver) | Crea un servidor de SQL Database que hospeda una base de datos única o un grupo elástico. |
| [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) | Crea una base de datos en un servidor de SQL Database como base de datos única o agrupada. |
[Get-AzureRmSqlDatabaseGeoBackup](/powershell/module/azurerm.sql/get-azurermsqldatabasegeobackup) | Obtiene una copia de seguridad con redundancia geográfica de una base de datos única o agrupada. |
| [Restore-AzureRmSqlDatabase](/powershell/module/azurerm.sql/restore-azurermsqldatabase) | Restaura una base de datos de SQL única o agrupada. |
|[Remove-AzureRmSqlDatabase](/powershell/module/azurerm.sql/remove-azurermsqldatabase) | Elimina una base de datos de SQL única o agrupada. |
| [Get-AzureRmSqlDeletedDatabaseBackup](/powershell/module/azurerm.sql/get-azurermsqldeleteddatabasebackup) | Obtiene una base de datos única o agrupada eliminada que se puede restaurar. |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Elimina un grupo de recursos, incluidos todos los recursos anidados. |

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Azure PowerShell, consulte la [documentación de Azure PowerShell](/powershell/azure/overview).

Encontrará más ejemplos de scripts de PowerShell de SQL Database en los [scripts de PowerShell de Azure SQL Database](../sql-database-powershell-samples.md).
