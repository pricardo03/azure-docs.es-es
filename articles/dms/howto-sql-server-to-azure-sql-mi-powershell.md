---
title: Migración de SQL Server a Instancia administrada de Azure SQL Database con Database Migration Service y PowerShell | Microsoft Docs
description: Aprenda a migrar de SQL Server local a Instancia administrada de Azure SQL Database mediante Azure PowerShell.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 04/29/2019
ms.openlocfilehash: d83410efd26f8c2078d3abdb01d061db0b83d33d
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/07/2019
ms.locfileid: "65233716"
---
# <a name="migrate-sql-server-on-premises-to-an-azure-sql-database-managed-instance-using-azure-powershell"></a>Migración de SQL Server local a una instancia administrada de Azure SQL Database mediante Azure PowerShell
En este artículo, migrará la **Adventureworks2016** base de datos restaurado a una instancia local de SQL Server 2005 o anterior a una base de datos de SQL Azure instancia administrada mediante el uso de Microsoft Azure PowerShell. Puede migrar las bases de datos desde una instancia de SQL Server local a una instancia administrada de Azure SQL Database mediante la `Az.DataMigration` módulo en Microsoft Azure PowerShell.

En este artículo, aprenderá a:
> [!div class="checklist"]
>
> * Cree un grupo de recursos.
> * Crear una instancia de Azure Database Migration Service.
> * Cree un proyecto de migración en una instancia de Azure Database Migration Service.
> * Ejecutar la migración.

[!INCLUDE [online-offline](../../includes/database-migration-service-offline-online.md)]

En este artículo incluye detalles sobre cómo realizar migraciones en línea y sin conexión.

## <a name="prerequisites"></a>Requisitos previos

Para completar estos pasos, necesitará lo siguiente:

* [SQL Server 2016 o posterior](https://www.microsoft.com/sql-server/sql-server-downloads) (cualquier edición).
* Una copia local de la **AdventureWorks2016** base de datos, que está disponible para su descarga [aquí](https://docs.microsoft.com/sql/samples/adventureworks-install-configure?view=sql-server-2017).
* Para habilitar el protocolo TCP/IP, que se deshabilita de forma predeterminada con la instalación de SQL Server Express. Habilite el protocolo TCP/IP siguiendo el artículo [Habilitar o deshabilitar un protocolo de red de servidor](https://docs.microsoft.com/sql/database-engine/configure-windows/enable-or-disable-a-server-network-protocol#SSMSProcedure).
* Configurar [Firewall de Windows para el acceso al motor de base de datos](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).
* Una suscripción de Azure. Si no tiene una, [cree una cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.
* Una instancia administrada de Azure SQL Database. Puede crear una instancia administrada de Azure SQL Database siguiendo la información en el artículo [crear una instancia administrada de Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-get-started).
* Para descargar e instalar [Data Migration Assistant](https://www.microsoft.com/download/details.aspx?id=53595) v3.3 o versiones posteriores.
* Azure Virtual Network (VNet) creados mediante el modelo de implementación de Azure Resource Manager, que proporciona el servicio de migración de base de datos de Azure con conectividad de sitio a sitio con los servidores de origen local utilizando [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) o [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).
* Una evaluación completada de la migración de base de datos y esquema de forma local mediante Data Migration Assistant, como se describe en el artículo [realizar una evaluación de migración de SQL Server](https://docs.microsoft.com/sql/dma/dma-assesssqlonprem).
* Para descargar e instalar el `Az.DataMigration` módulo (versión 0.7.2 o posterior) desde la Galería de PowerShell mediante el uso de [Install-Module PowerShell cmdlet](https://docs.microsoft.com/powershell/module/powershellget/Install-Module?view=powershell-5.1).
* Para asegurarse de que las credenciales usadas para conectarse a la instancia de SQL Server de origen tienen el [CONTROL SERVER](https://docs.microsoft.com/sql/t-sql/statements/grant-server-permissions-transact-sql) permiso.
* Para asegurarse de que las credenciales usadas para conectarse a Azure SQL Database de destino instancia administrada tiene el permiso CONTROL DATABASE en las bases de datos de instancia de destino administrado de Azure SQL Database.

    > [!IMPORTANT]
    > Para migraciones en línea, deberá ya ha configurado las credenciales de Azure Active Directory. Para obtener más información, vea el artículo [usar el portal para crear una entidad de servicio y aplicación que puede tener acceso a recursos de Azure AD](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal).

## <a name="sign-in-to-your-microsoft-azure-subscription"></a>Inicie sesión su suscripción de Microsoft Azure

Inicie sesión en su suscripción de Azure mediante PowerShell. Para obtener más información, vea el artículo [inicie sesión con Azure PowerShell](https://docs.microsoft.com/powershell/azure/authenticate-azureps).

## <a name="create-a-resource-group"></a>Crear un grupo de recursos

Un grupo de recursos de Azure es un contenedor lógico en el que se implementan y se administran los recursos de Azure.

Crear un grupo de recursos mediante el [ `New-AzResourceGroup` ](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) comando.

En el ejemplo siguiente se crea un grupo de recursos denominado *myResourceGroup* en el *East US* región.

```powershell
New-AzResourceGroup -ResourceGroupName myResourceGroup -Location EastUS
```

## <a name="create-an-instance-of-azure-database-migration-service"></a>Creación de una instancia de Azure Database Migration Service

Puede crear una instancia nueva de Azure Database Migration Service mediante el cmdlet `New-AzDataMigrationService`.
Este cmdlet espera los siguientes parámetros requeridos:

* *Nombre del grupo de recursos de Azure*. Puede usar [ `New-AzResourceGroup` ](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) comando para crear un grupo de recursos de Azure como se mostró anteriormente y proporcionar su nombre como un parámetro.
* *Nombre del servicio*. Cadena que se corresponde con el nombre de servicio único de Azure Database Migration Service.
* *Ubicación*. Especifica la ubicación del servicio. Especifique una ubicación de centro de datos de Azure, por ejemplo, oeste de Estados Unidos o sudeste asiático.
* *SKU*. Este parámetro corresponde al nombre de SKU de DMS. Los nombres de Sku admitidos actualmente son *Basic_1vCore*, *Basic_2vCores*, *GeneralPurpose_4vCores*.
* *Identificador de subred virtual*. Puede usar el cmdlet [ `New-AzVirtualNetworkSubnetConfig` ](https://docs.microsoft.com//powershell/module/az.network/new-azvirtualnetworksubnetconfig) para crear una subred.

En el ejemplo siguiente se crea un servicio denominado *MyDMS* en el grupo de recursos *MyDMSResourceGroup* ubicado en el *East US* región con una red virtual denominada  *MyVNET* y una subred denominada *MySubnet*.

> [!IMPORTANT]
> El siguiente fragmento de código es para una migración sin conexión, que no requiere una instancia de Azure Database Migration Service basado en una SKU Premium. Para realizar una migración en línea, el valor del parámetro - Sku debe incluir una SKU Premium.

```powershell
$vNet = Get-AzVirtualNetwork -ResourceGroupName MyDMSResourceGroup -Name MyVNET

$vSubNet = Get-AzVirtualNetworkSubnetConfig -VirtualNetwork $vNet -Name MySubnet

$service = New-AzDms -ResourceGroupName myResourceGroup `
  -ServiceName MyDMS `
  -Location EastUS `
  -Sku Basic_2vCores `  
  -VirtualSubnetId $vSubNet.Id`
```

## <a name="create-a-migration-project"></a>Creación de un proyecto de migración

Cree un proyecto de migración después de crear una instancia de Azure Database Migration Service. Un proyecto de Azure Database Migration Service requiere información de conexión para las instancias de origen y de destino, así como una lista de bases de datos que desee migrar como parte del proyecto.

### <a name="create-a-database-connection-info-object-for-the-source-and-target-connections"></a>Creación de un objeto de información de conexión de base de datos para las conexiones de origen y de destino

Puede crear un objeto de información de conexión de base de datos mediante el `New-AzDmsConnInfo` cmdlet, que espera los siguientes parámetros:

* *ServerType*. El tipo de conexión de base de datos solicitado, por ejemplo, SQL, Oracle o MySQL. Utilice SQL para SQL Server y Azure SQL.
* *DataSource*. El nombre o dirección IP de una instancia de Azure SQL Database o instancia de SQL Server.
* *AuthType*. El tipo de autenticación para la conexión, que puede ser SqlAuthentication o WindowsAuthentication.
* *TrustServerCertificate*. Este parámetro establece un valor que indica si el canal se cifra mientras se omite recorrer la cadena de certificados para validar la confianza. El valor puede ser `$true` o `$false`.

En el ejemplo siguiente se crea un objeto de información de conexión para un origen de SQL Server denominado *MySourceSQLServer* mediante autenticación de sql:

```powershell
$sourceConnInfo = New-AzDmsConnInfo -ServerType SQL `
  -DataSource MySourceSQLServer `
  -AuthType SqlAuthentication `
  -TrustServerCertificate:$true
```

El ejemplo siguiente muestra la creación de información de conexión para un servidor de instancia administrada de Azure SQL Database denominado 'targetmanagedinstance.database.windows.net' mediante la autenticación de sql:

```powershell
$targetConnInfo = New-AzDmsConnInfo -ServerType SQL `
  -DataSource "targetmanagedinstance.database.windows.net" `
  -AuthType SqlAuthentication `
  -TrustServerCertificate:$false
```

### <a name="provide-databases-for-the-migration-project"></a>Ofrecimiento de bases de datos para el proyecto de migración

Crear una lista de `AzDataMigrationDatabaseInfo` objetos que especifica las bases de datos como parte del proyecto de Azure Database Migration Service, que se puede proporcionar como parámetro para la creación del proyecto. Puede usar el cmdlet `New-AzDataMigrationDatabaseInfo` crear `AzDataMigrationDatabaseInfo`.

En el ejemplo siguiente se crea el `AzDataMigrationDatabaseInfo` del proyecto de la **AdventureWorks2016** de base de datos y lo agrega a la lista para proporcionarlo como parámetro para la creación del proyecto.

```powershell
$dbInfo1 = New-AzDataMigrationDatabaseInfo -SourceDatabaseName AdventureWorks
$dbList = @($dbInfo1)
```

### <a name="create-a-project-object"></a>Creación de un objeto del proyecto

Por último, puede crear un proyecto de Azure Database Migration Service llamado *MyDMSProject* ubicado en *East US* mediante `New-AzDataMigrationProject` y agregar las conexiones de origen y de destino creadas anteriormente y el lista de bases de datos para migrar.

```powershell
$project = New-AzDataMigrationProject -ResourceGroupName myResourceGroup `
  -ServiceName $service.Name `
  -ProjectName MyDMSProject `
  -Location EastUS `
  -SourceType SQL `
  -TargetType SQLMI `
  -SourceConnection $sourceConnInfo `
  -TargetConnection $targetConnInfo `
  -DatabaseInfo $dbList
```

## <a name="create-and-start-a-migration-task"></a>Creación e inicio de una tarea de migración

A continuación, crear e iniciar una tarea de Azure Database Migration Service. Esta tarea requiere información de credenciales de conexión para el origen y destino, así como la lista de tablas de base de datos que desea migrar y la información ya proporcionada con el proyecto creado como requisito previo.

### <a name="create-credential-parameters-for-source-and-target"></a>Creación de parámetros de credenciales de origen y de destino

Crear conexión de credenciales de seguridad como un [PSCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential?redirectedfrom=MSDN&view=powershellsdk-1.1.0) objeto.

El ejemplo siguiente muestra la creación de *PSCredential* objetos para las conexiones de origen y destino, proporcionando contraseñas como variables de cadena *$sourcePassword* y *$ contraseñaDestino*.

```powershell
$secpasswd = ConvertTo-SecureString -String $sourcePassword -AsPlainText -Force
$sourceCred = New-Object System.Management.Automation.PSCredential ($sourceUserName, $secpasswd)
$secpasswd = ConvertTo-SecureString -String $targetPassword -AsPlainText -Force
$targetCred = New-Object System.Management.Automation.PSCredential ($targetUserName, $secpasswd)
```

### <a name="create-a-backup-fileshare-object"></a>Crear un objeto de recurso compartido de archivos de copia de seguridad

Ahora cree un objeto de recurso compartido de archivos que representa el recurso compartido local de red SMB a la que Azure Database Migration Service puede tomar el origen de las copias de seguridad de base de datos mediante el `New-AzDmsFileShare` cmdlet.

```powershell
$backupPassword = ConvertTo-SecureString -String $password -AsPlainText -Force
$backupCred = New-Object System.Management.Automation.PSCredential ($backupUserName, $backupPassword)

$backupFileSharePath="\\10.0.0.76\SharedBackup"
$backupFileShare = New-AzDmsFileShare -Path $backupFileSharePath -Credential $backupCred
```

### <a name="create-selected-database-object"></a>Creación del objeto de la base de datos seleccionada

El siguiente paso consiste en seleccionar las bases de datos de origen y destino mediante el uso de la `New-AzDmsSelectedDB` cmdlet.

El ejemplo siguiente es para migrar una base de datos de SQL Server a una instancia administrada de Azure SQL Database:

```powershell
$selectedDbs = @()
$selectedDbs += New-AzDmsSelectedDB -MigrateSqlServerSqlDbMi `
  -Name AdventureWorks2016 `
  -TargetDatabaseName AdventureWorks2016 `
  -BackupFileShare $backupFileShare `
```

Si necesita una instancia de SQL Server completa instancia administrada de una migración mediante lift-and-shift en una base de datos de SQL Azure, a continuación, a continuación se proporciona un bucle para aprovechar todas las bases de datos desde el origen. En el ejemplo siguiente, $Server, $SourceUserName y $SourcePassword, proporcionar el origen de los detalles de SQL Server.

```powershell
$Query = "(select name as Database_Name from master.sys.databases where Database_id>4)";
$Databases= (Invoke-Sqlcmd -ServerInstance "$Server" -Username $SourceUserName
-Password $SourcePassword -database master -Query $Query)
$selectedDbs=@()
foreach($DataBase in $Databases.Database_Name)
    {
      $SourceDB=$DataBase
      $TargetDB=$DataBase
      
$selectedDbs += New-AzureRmDmsSelectedDB -MigrateSqlServerSqlDbMi `
                                              -Name $SourceDB `
                                              -TargetDatabaseName $TargetDB `
                                              -BackupFileShare $backupFileShare
      }
```

### <a name="sas-uri-for-azure-storage-container"></a>URI de SAS del contenedor de Azure Storage

Cree una variable que contenga el URI de SAS que proporciona a Azure Database Migration Service acceso al contenedor de cuentas de almacenamiento en el que el servicio carga los archivos de copia de seguridad.

```powershell
$blobSasUri="https://mystorage.blob.core.windows.net/test?st=2018-07-13T18%3A10%3A33Z&se=2019-07-14T18%3A10%3A00Z&sp=rwdl&sv=2018-03-28&sr=c&sig=qKlSA512EVtest3xYjvUg139tYSDrasbftY%3D"
```

### <a name="additional-configuration-requirements"></a>Requisitos de configuración adicionales

Hay algunos requisitos adicionales que deben satisfacerse, pero difieren dependiendo de si está realizando una migración en línea o sin conexión.

#### <a name="offline-migrations"></a>Migraciones sin conexión

Para migraciones sin conexión, realice las siguientes tareas de configuración adicionales.

* **Seleccione los inicios de sesión**. Cree una lista de inicios de sesión para la migración, tal como se muestra en el ejemplo siguiente:

    ```powershell
    $selectedLogins = @(“user1”, “user2”)
    ```

    > [!IMPORTANT]
    > Actualmente, Azure Database Migration Service solo admite la migración de los inicios de sesión SQL.

* **Seleccione los trabajos del agente**. Crear lista del agente de trabajos que desea migrar tal como se muestra en el ejemplo siguiente:

    ```powershell
    $selectedAgentJobs = @("agentJob1", "agentJob2")
    ```

    > [!IMPORTANT]
    > Actualmente, Azure Database Migration Service solo admite trabajos con los pasos de trabajo del subsistema de T-SQL.

#### <a name="online-migrations"></a>Migraciones en línea

Para migraciones en línea, realice las siguientes tareas de configuración adicionales.

* **Configurar la aplicación Azure Active Directory**

    ```powershell
    $pwd = "Azure App Key"
    $appId = "Azure App Id"
    $AppPasswd = ConvertTo-SecureString $pwd -AsPlainText -Force
    $app = New-AzDmsAdApp -ApplicationId $appId -AppKey $AppPasswd
    ```

* **Configurar el recurso de almacenamiento**

    ```powershell
    $storageResourceId = "Storage Resource Id"
    ```

### <a name="create-and-start-the-migration-task"></a>Crear e iniciar la tarea de migración

Utilice el cmdlet `New-AzDataMigrationTask` para crear e iniciar una tarea de migración.

#### <a name="specify-parameters"></a>Especificación de parámetros

##### <a name="common-parameters"></a>Parámetros comunes

Independientemente de si está realizando una migración en línea o sin conexión, el `New-AzDataMigrationTask` cmdlet espera los siguientes parámetros:

* *TaskType*. Tipo de tarea de migración que se espera crear para SQL Server con el tipo de migración de Instancia administrada de Azure SQL Database *MigrateSqlServerSqlDbMi*. 
* *Nombre del grupo de recursos*. Nombre del grupo de recursos de Azure en el que se crea la tarea.
* *ServiceName*. Instancia de Azure Database Migration Service en la que crear la tarea.
* *ProjectName*. Nombre del proyecto de Azure Database Migration Service en el que se va a crear la tarea. 
* *TaskName*. Nombre de la tarea que se va a crear. 
* *SourceConnection*. AzDmsConnInfo objeto que representa la conexión de SQL Server de origen.
* *TargetConnection*. AzDmsConnInfo objeto que representa la conexión de la instancia administrada de Azure SQL Database de destino.
* *SourceCred*. Objeto [PSCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential?redirectedfrom=MSDN&view=powershellsdk-1.1.0) para la conexión al servidor de origen.
* *TargetCred*. Objeto [PSCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential?redirectedfrom=MSDN&view=powershellsdk-1.1.0) para la conexión al servidor de destino.
* *SelectedDatabase*. AzDataMigrationSelectedDB objeto que representa la asignación de base de datos de origen y de destino.
* *BackupFileShare*. Objeto FileShare que representa el recurso compartido de red local en el que Azure Database Migration Service puede realizar las copias de seguridad de la base de datos de origen.
* *BackupBlobSasUri*. URI de SAS que proporciona a Azure Database Migration Service acceso al contenedor de cuentas de almacenamiento en el que el servicio carga los archivos de copia de seguridad. Aprenda a obtener el identificador URI de SAS del contenedor de blobs.
* *SelectedLogins*. Lista de los inicios de sesión seleccionados para la migración.
* *SelectedAgentJobs*. Lista de los trabajos de agente seleccionados para la migración.

##### <a name="additional-parameters"></a>Parámetros adicionales

El `New-AzDataMigrationTask` cmdlet también espera parámetros que son únicos para el tipo de migración, o sin conexión, que se va a realizar.

* **Migraciones sin conexión**. Para realizar migraciones sin conexión, el `New-AzDataMigrationTask` cmdlet también espera los siguientes parámetros:

  * *SelectedLogins*. Lista de los inicios de sesión seleccionados para la migración.
  * *SelectedAgentJobs*. Lista de los trabajos de agente seleccionados para la migración.

* **Migraciones en línea**. Para migraciones en línea, el `New-AzDataMigrationTask` cmdlet también espera los siguientes parámetros.

* *AzureActiveDirectoryApp*. Aplicación de Active Directory.
* *StorageResourceID*. Identificador de recurso de cuenta de almacenamiento.

#### <a name="create-and-start-an-offline-migration-task"></a>Crear e iniciar una tarea de migración sin conexión

En el ejemplo siguiente se crea e inicia una tarea de migración sin conexión denominada **myDMSTask**:

```powershell
$migTask = New-AzDataMigrationTask -TaskType MigrateSqlServerSqlDbMi `
  -ResourceGroupName myResourceGroup `
  -ServiceName $service.Name `
  -ProjectName $project.Name `
  -TaskName myDMSTask `
  -SourceConnection $sourceConnInfo `
  -SourceCred $sourceCred `
  -TargetConnection $targetConnInfo `
  -TargetCred $targetCred `
  -SelectedDatabase  $selectedDbs `
  -BackupFileShare $backupFileShare `
  -BackupBlobSasUri $blobSasUri `
  -SelectedLogins $selectedLogins `
  -SelectedAgentJobs $selectedJobs `
```

#### <a name="create-and-start-an-online-migration-task"></a>Crear e iniciar una tarea de migración en línea

El ejemplo siguiente se crea e inicia una tarea de migración en línea denominada **myDMSTask**:

```powershell
$migTask = New-AzDataMigrationTask -TaskType MigrateSqlServerSqlDbMiSync `
  -ResourceGroupName myResourceGroup `
  -ServiceName $service.Name `
  -ProjectName $project.Name `
  -TaskName myDMSTask `
  -SourceConnection $sourceConnInfo `
  -SourceCred $sourceCred `
  -TargetConnection $targetConnInfo `
  -TargetCred $targetCred `
  -SelectedDatabase  $selectedDbs `
  -BackupFileShare $backupFileShare `
  -SelectedDatabase $selectedDbs `
  -AzureActiveDirectoryApp $app `
  -StorageResourceId $storageResourceId
```

## <a name="monitor-the-migration"></a>Supervisión de la migración

Para supervisar la migración, realice las tareas siguientes.

1. Consolide todos los detalles de la migración en una variable denominada $CheckTask.

    Para combinar los detalles de la migración, como propiedades, el estado y la información de la base de datos asociados con la migración, use el siguiente fragmento de código:

    ```powershell
    $CheckTask= Get-AzDataMigrationTask     -ResourceGroupName myResourceGroup `
                                            -ServiceName $service.Name `
                                        -ProjectName $project.Name `
                                            -Name myDMSTask `
                                            -ResultType DatabaseLevelOutput `
                        -Expand 
    Write-Host ‘$CheckTask.ProjectTask.Properties.Output’
    ```

2. Use el `$CheckTask` variable para obtener el estado actual de la tarea de migración.

    Para usar el `$CheckTask` variable para obtener el estado actual de la tarea de migración, puede supervisar la tarea de migración ejecutando consultando la propiedad de estado de la tarea, tal como se muestra en el ejemplo siguiente:

    ```powershell
    if (($CheckTask.ProjectTask.Properties.State -eq "Running") -or ($CheckTask.ProjectTask.Properties.State -eq "Queued"))
    {
      write-host "migration task running"
    }
    Else if($CheckTask.ProjectTask.Properties.State -eq "Succeeded")
    { 
      write-host "Migration task is completed Successfully"
    }
    Else if($CheckTask.ProjectTask.Properties.State -eq "Failed" -or $CheckTask.ProjectTask.Properties.State -eq "FailedInputValidation"  -or $CheckTask.ProjectTask.Properties.State -eq "Faulted")
    { 
      write-host “Migration Task Failed”
    }
    ```

## <a name="performing-the-cutover-online-migrations-only"></a>Realizar el traslado (migraciones en línea sólo)

Con una migración en línea, se realiza una copia de seguridad completa y restaure las bases de datos y, a continuación, continúa el trabajo en la restauración de los registros de transacciones que se almacenan en el BackupFileShare.

Cuando la base de datos en una instancia administrada de Azure SQL Database se actualiza con los datos más recientes y está sincronizada con la base de datos de origen, puede realizar una migración.

El ejemplo siguiente completará el cutover\migration. Los usuarios invocar este comando a su entera discreción.

```powershell
$command = Invoke-AzDmsCommand -CommandType CompleteSqlMiSync `
                               -ResourceGroupName myResourceGroup `
                               -ServiceName $service.Name `
                               -ProjectName $project.Name `
                               -TaskName myDMSTask `
                               -DatabaseName "Source DB Name"
```

## <a name="deleting-the-instance-of-azure-database-migration-service"></a>Eliminando la instancia de Azure Database Migration Service

Una vez completada la migración, puede eliminar la instancia de Azure Database Migration Service:

```powershell
Remove-AzDms -ResourceGroupName myResourceGroup -ServiceName MyDMS
```

## <a name="additional-resources"></a>Recursos adicionales

Para obtener información acerca de escenarios de migración adicionales (pares de origen o destino), consulte Microsoft [Guía de migración de base de datos](https://datamigration.microsoft.com/).

## <a name="next-steps"></a>Pasos siguientes

Obtener más información acerca de Azure Database Migration Service en el artículo [¿qué es Azure Database Migration Service?](https://docs.microsoft.com/azure/dms/dms-overview).
