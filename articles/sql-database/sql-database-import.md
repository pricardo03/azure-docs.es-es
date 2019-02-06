---
title: Importación de un archivo BACPAC para crear una base de datos de Azure SQL | Microsoft Docs
description: Cree una base de datos de Azure SQL Database nueva mediante la importación de archivo BACPAC.
services: sql-database
ms.service: sql-database
ms.subservice: migration
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: douglaslMS
ms.author: douglasl
ms.reviewer: carlrab
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: c1b6c55475c1600c89c1ac1cae9dee0068b92070
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2019
ms.locfileid: "55478226"
---
# <a name="quickstart-import-a-bacpac-file-to-a-new-azure-sql-database"></a>Inicio rápido: Importación de un archivo BACPAC en una base de datos de Azure SQL Database

Puede migrar una base de datos SQL Server a una base de datos Azure SQL mediante un archivo[BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) (un archivo ZIP con una extensión `.bacpac` que tiene los metadatos y los datos de una base de datos). Puede importar un archivo BACPAC desde Azure Blob Storage (solo almacenamiento estándar) o desde el almacenamiento local en una ubicación local. Para maximizar la velocidad de importación, puede especificar un nivel mayor de servicio y de tamaño de proceso (por ejemplo, P6). A continuación, puede reducir verticalmente cuando la importación se lleve a cabo de forma correcta. El nivel de compatibilidad de la base de datos importada se basa en el nivel de compatibilidad de la base de datos de origen.

> [!IMPORTANT]
> Después de importar la base de datos, puede elegir usar la base de datos en el nivel de compatibilidad actual (nivel 100 para la base de datos AdventureWorks2008R2) o en un nivel superior. Para más información acerca de las implicaciones y las opciones para la utilización de una base de datos en un nivel de compatibilidad específico, consulte [ALTER DATABASE (Transact-SQL) Compatibility Level](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level) [Nivel de compatibilidad de ALTER DATABASE (Transact-SQL)]. Vea también [ALTER DATABASE SCOPED CONFIGURATION](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) para obtener información sobre los valores de nivel de base de datos adicionales relacionados con los niveles de compatibilidad.

## <a name="import-from-a-bacpac-file-in-the-azure-portal"></a>Importación de un archivo BACPAC en Azure Portal

En esta sección se muestra cómo crear, en [Azure Portal](https://portal.azure.com), una base de datos de Azure SQL a partir de un archivo BACPAC almacenado en Azure Blob Storage. El portal *solo* admite importar un archivo BACPAC desde Azure Blob Storage.

> [!NOTE]
> [Instancia administrada de Azure SQL Database](sql-database-managed-instance.md) permite importar desde un archivo BACPAC mediante los otros métodos que se describen en este artículo, pero actualmente no admite la migración mediante Azure Portal.

Para importar una base de datos en Azure Portal, abra la página del servidor de SQL Database que hospedará la importación y, en la barra de herramientas, seleccione **Importar base de datos**.  

   ![Importación de base de datos](./media/sql-database-import/import.png)

Seleccione la cuenta de almacenamiento, el contenedor y el archivo BACPAC que quiere importar. Especifique el tamaño de la nueva base de datos (por lo general, el mismo que el origen) y proporcione las credenciales del servidor SQL Server de destino. 

### <a name="monitor-imports-progress"></a>Supervisión del progreso de la importación

Para supervisar el progreso de una importación, abra la página del servidor de la base de datos importada y, en **Configuración**, seleccione **Historial de importación y exportación**. Cuando se realiza correctamente, la importación tiene el estado **Completada**.

Para comprobar que la base de datos está activa en el servidor, seleccione **Bases de datos SQL** y compruebe que la base de datos nueva está **En línea**.

## <a name="import-from-a-bacpac-file-using-sqlpackage"></a>Importación de un archivo BACPAC mediante SqlPackage

Para importar una base de datos SQL mediante la utilidad de línea de comandos [SqlPackage](https://docs.microsoft.com/sql/tools/sqlpackage), consulte [Parámetros y propiedades de la importación](https://docs.microsoft.com/sql/tools/sqlpackage#import-parameters-and-properties). SqlPackage incluye las versiones más recientes de [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) y [SQL Server Data Tools para Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx). También puede descargar la versión más reciente de [SqlPackage](https://www.microsoft.com/download/details.aspx?id=53876) desde el Centro de descarga de Microsoft.

Para escalado y rendimiento, se recomienda usar SqlPackage en la mayoría de los entornos de producción. Consulte cómo [migrar de SQL Server a Azure SQL Database con archivos BACPAC](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/) en el blog de Customer Advisory Team de SQL Server.

El comando SqlPackage siguiente importa la base de datos **AdventureWorks2008R2** desde el almacenamiento local a un servidor de Azure SQL Database denominado **mynewserver20170403**. Crea una base de datos llamada **myMigratedDatabase** con un nivel de servicio **Premium** y un objetivo de servicio **P6**. Cambie estos valores según corresponda para su entorno.

```cmd
SqlPackage.exe /a:import /tcs:"Data Source=mynewserver20170403.database.windows.net;Initial Catalog=myMigratedDatabase;User Id=<your_server_admin_account_user_id>;Password=<your_server_admin_account_password>" /sf:AdventureWorks2008R2.bacpac /p:DatabaseEdition=Premium /p:DatabaseServiceObjective=P6
```

> [!IMPORTANT]
> Un servidor de SQL Database escucha en el puerto 1433. Para conectarse a un servidor de SQL Database desde detrás de un firewall corporativo, el firewall debe tener abierto este puerto.
>

Este ejemplo muestra cómo importar una base de datos con SqlPackage con Autenticación universal de Active Directory.

```cmd
SqlPackage.exe /a:Import /sf:testExport.bacpac /tdn:NewDacFX /tsn:apptestserver.database.windows.net /ua:True /tid:"apptest.onmicrosoft.com"
```

## <a name="import-from-a-bacpac-file-using-powershell"></a>Importación de un archivo BACPAC mediante PowerShell

Use el cmdlet [AzureRmSqlDatabaseImport New](/powershell/module/azurerm.sql/new-azurermsqldatabaseimport) para enviar una solicitud de importación base de datos al servicio Azure SQL Database. En función del tamaño de la base de datos, la importación puede tardar en completarse.

 ```powershell
 $importRequest = New-AzureRmSqlDatabaseImport 
    -ResourceGroupName "<your_resource_group>" `
    -ServerName "<your_server>" `
    -DatabaseName "<your_database>" `
    -DatabaseMaxSizeBytes "<database_size_in_bytes>" `
    -StorageKeyType "StorageAccessKey" `
    -StorageKey $(Get-AzureRmStorageAccountKey -ResourceGroupName "<your_resource_group>" -StorageAccountName "<your_storage_account").Value[0] `
    -StorageUri "https://myStorageAccount.blob.core.windows.net/importsample/sample.bacpac" `
    -Edition "Standard" `
    -ServiceObjectiveName "P6" `
    -AdministratorLogin "<your_server_admin_account_user_id>" `
    -AdministratorLoginPassword $(ConvertTo-SecureString -String "<your_server_admin_account_password>" -AsPlainText -Force)

 ```

 Puede usar el cmdlet [Get-AzureRmSqlDatabaseImportExportStatus](/powershell/module/azurerm.sql/get-azurermsqldatabaseimportexportstatus) para comprobar el progreso de la importación. Si el cmdlet se ejecuta inmediatamente después de la solicitud, por lo general devuelve **Estado: En curso**. La importación se completa cuando se ve **Estado: Correcto**.

```powershell
$importStatus = Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink
[Console]::Write("Importing")
while ($importStatus.Status -eq "InProgress")
{
    $importStatus = Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink
    [Console]::Write(".")
    Start-Sleep -s 10
}
[Console]::WriteLine("")
$importStatus
```

> [!TIP]
Para ver otro ejemplo de script, consulte [Importación de una base de datos desde un archivo BACPAC](scripts/sql-database-import-from-bacpac-powershell.md).

## <a name="limitations"></a>Limitaciones

No se admite la importación a una base de datos que esté en un grupo elástico. Puede importar los datos en una base de datos única y después moverla a un grupo elástico.

## <a name="import-using-wizards"></a>Importación mediante asistentes

También puede usar estos asistentes.

- [Asistente para importación de una aplicación de capa de datos en SQL Server Management Studio](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/import-a-bacpac-file-to-create-a-new-user-database#using-the-import-data-tier-application-wizard).
- [Asistente para importación y exportación de SQL Server](https://docs.microsoft.com/sql/integration-services/import-export-data/start-the-sql-server-import-and-export-wizard)

## <a name="next-steps"></a>Pasos siguientes

- Para información sobre cómo conectarse a una base de datos SQL importada y cómo realizar consultas en ella, lea [Inicio rápido: Azure SQL Database: uso de SQL Server Management Studio para conectarse a los datos y realizar consultas en ellos](sql-database-connect-query-ssms.md).
- Consulte cómo [migrar de SQL Server a Azure SQL Database con archivos BACPAC](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/) en el blog de Customer Advisory Team de SQL Server.
- Para información sobre el proceso de migración de bases de datos SQL Server completo, incluidas las recomendaciones relacionadas con el rendimiento, consulte [Migración de una base de datos de SQL Server a Azure SQL Database](sql-database-cloud-migrate.md).
- Para obtener información sobre cómo administrar y compartir de forma segura claves de almacenamiento y firmas de acceso compartido, vea la [Guía de seguridad de Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-security-guide).
