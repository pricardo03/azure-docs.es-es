---
title: 'Ejemplo de PowerShell: Actualizar el esquema de sincronización de SQL Data Sync | Microsoft Docs'
description: Script de ejemplo de Azure PowerShell para actualizar el esquema de sincronización de SQL Data Sync
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: ''
ms.devlang: PowerShell
ms.topic: sample
author: allenwux
ms.author: xiwu
ms.reviewer: douglasl
manager: craigg
ms.date: 01/10/2018
ms.openlocfilehash: bd3e3a7383098d74f3753908585d3392fafcadc4
ms.sourcegitcommit: 715813af8cde40407bd3332dd922a918de46a91a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "47056614"
---
# <a name="use-powershell-to-update-the-sync-schema-in-an-existing-sync-group"></a>Usar PowerShell para actualizar el esquema de sincronización en un grupo de sincronización existente

En este ejemplo de PowerShell se actualiza el esquema de sincronización en un grupo de sincronización de SQL Data Sync. Cuando se sincronizan varias tablas, este script le ayuda a actualizar eficazmente el esquema de sincronización. En este ejemplo se muestra el uso del script de **UpdateSyncSchema**, que está disponible en GitHub como [UpdateSyncSchema.ps1](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/sql-data-sync/UpdateSyncSchema.ps1).

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Si decide instalar y usar PowerShell localmente, para este tutorial se requiere la versión 5.7.0 del módulo de Azure PowerShell o cualquier versión posterior. Ejecute `Get-Module -ListAvailable AzureRM` para encontrar la versión. Si necesita actualizarla, consulte [Instalación del módulo de Azure PowerShell](/powershell/azure/install-azurerm-ps). Si PowerShell se ejecuta localmente, también debe ejecutar `Connect-AzureRmAccount` para crear una conexión con Azure.


Para obtener información general acerca de SQL Data Sync, consulte [Sincronización de datos entre varias bases de datos locales y de la nube con Azure SQL Data Sync](../sql-database-sync-data.md).

## <a name="sample-script"></a>Script de ejemplo

### <a name="example-1---add-all-tables-to-the-sync-schema"></a>Ejemplo 1: Agregar todas las tablas al esquema de sincronización

En el ejemplo siguiente se actualiza el esquema de base de datos y agregan todas las tablas válidas de la base de datos central en el esquema de sincronización.

```powershell-interactive
UpdateSyncSchema.ps1 -SubscriptionId <subscription_id> -ResourceGroupName <resource_group_name> -ServerName <server_name> -DatabaseName <database_name> -SyncGroupName <sync_group_name> -RefreshDatabaseSchema $true -AddAllTables $true
```

### <a name="example-2---add-and-remove-tables-and-columns"></a>Ejemplo 2: Agregar y quitar tablas y columnas

En el ejemplo siguiente se agregan `[dbo].[Table1]` y `[dbo].[Table2].[Column1]` al esquema de sincronización y se quita `[dbo].[Table3]`.

```powershell-interactive
UpdateSyncSchema.ps1 -SubscriptionId <subscription_id> -ResourceGroupName <resource_group_name> -ServerName <server_name> -DatabaseName <database_name> -SyncGroupName <sync_group_name> -TablesAndColumnsToAdd "[dbo].[Table1],[dbo].[Table2].[Column1]" -TablesAndColumnsToRemove "[dbo].[Table3]"
```

## <a name="script-parameters"></a>Parámetros de script

El script **UpdateSyncSchema** tiene los parámetros siguientes:

| Parámetro | Notas |
|---|---|
| $SubscriptionId | Suscripción en la que se creó el grupo de sincronización. |
| $ResourceGroupName | Grupo de recursos en el que se creó el grupo de sincronización.|
| $ServerName | Nombre de servidor de la base de datos central.|
| $DatabaseName | Nombre de la base de datos central. |
| $SyncGroupName | Nombre del grupo de sincronización. |
| $MemberName | Especifique el nombre de miembro si quiere cargar el esquema de base de datos desde el miembro de sincronización en lugar de desde la base de datos central. Si quiere cargar el esquema de base de datos desde la central, deje vacío este parámetro. |
| $TimeoutInSeconds | Tiempo de espera después del cual el script actualiza el esquema de base de datos. El valor predeterminado es 900 segundos. |
| $RefreshDatabaseSchema | Permite especificar si el script debe actualizar el esquema de la base de datos. Si el esquema de la base de datos cambió desde la configuración anterior (por ejemplo, si agregó una tabla o una columna nueva), deberá actualizar el esquema antes de volver a configurarlo. El valor predeterminado es false. |
| $AddAllTables | Si este valor es true, todas las tablas y columnas válidas se agregan al esquema de sincronización. Se omiten los valores de $TablesAndColumnsToAdd y $TablesAndColumnsToRemove. |
| $TablesAndColumnsToAdd | Permite especificar las tablas o columnas que se agregarán al esquema de sincronización. Cada nombre de tabla o columna debe delimitarse totalmente con el nombre del esquema. Por ejemplo: `[dbo].[Table1]`, `[dbo].[Table2].[Column1]`. Pueden especificarse varios nombres de tabla o columna y separarse por comas (,). |
| $TablesAndColumnsToRemove | Permite especificar las tablas o columnas que se eliminarán del esquema de sincronización. Cada nombre de tabla o columna debe delimitarse totalmente con el nombre de esquema. Por ejemplo: `[dbo].[Table1]`, `[dbo].[Table2].[Column1]`. Pueden especificarse varios nombres de tabla o columna y separarse por comas (,). |
|||

## <a name="script-explanation"></a>Explicación del script

El script **UpdateSyncSchema** usa los comandos siguientes. Cada comando de la tabla crea un vínculo a documentación específica del comando.

| Get-Help | Notas |
|---|---|
| [Get-AzureRmSqlSyncGroup](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqlsyncgroup) | Devuelve información acerca de un grupo de sincronización. |
| [Update-AzureRmSqlSyncGroup](https://docs.microsoft.com/powershell/module/azurerm.sql/update-azurermsqlsyncgroup) | Actualiza un grupo de sincronización. |
| [Get-AzureRmSqlSyncMember](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqlsyncmember) | Devuelve información acerca de un miembro de sincronización. |
| [Get-AzureRmSqlSyncSchema](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqlsyncschema) | Devuelve información acerca de un esquema de sincronización. |
| [Update-AzureRmSqlSyncSchema](https://docs.microsoft.com/powershell/module/azurerm.sql/update-azurermsqlsyncschema) | Actualiza un esquema de sincronización. |
|||

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Azure PowerShell, consulte la [documentación de Azure PowerShell](/powershell/azure/overview).

Encontrará más ejemplos de scripts de PowerShell de SQL Database en los [scripts de PowerShell de Azure SQL Database](../sql-database-powershell-samples.md).

Para más información sobre SQL Data Sync, consulte:

-   [Sincronización de datos entre varias bases de datos locales y de la nube con SQL Data Sync](../sql-database-sync-data.md)
-   [Configuración de Azure SQL Data Sync](../sql-database-get-started-sql-data-sync.md)
-   [Procedimientos recomendados para SQL Data Sync de Azure](../sql-database-best-practices-data-sync.md)
-   [Supervisión de Azure SQL Data Sync con Log Analytics](../sql-database-sync-monitor-oms.md)
-   [Solución de problemas de SQL Data Sync de Azure](../sql-database-troubleshoot-data-sync.md)

-   Para obtener ejemplos completos de PowerShell que muestren cómo configurar SQL Data Sync:
    -   [Uso de PowerShell para sincronizar entre varias bases de datos SQL de Azure.](sql-database-sync-data-between-sql-databases.md)
    -   [Uso de PowerShell para realizar la sincronización entre Azure SQL Database y una base de datos de SQL Server local](sql-database-sync-data-between-azure-onprem.md)

Para más información sobre SQL Database, consulte:

-   [Información general de SQL Database](../sql-database-technical-overview.md)
-   [Administración del ciclo de vida de las aplicaciones](https://msdn.microsoft.com/library/jj907294.aspx)
