---
title: Configurar la replicación en una base de datos de instancia administrada de Azure SQL Database | Microsoft Docs
description: Aprenda a configurar la replicación transaccional en una base de datos de instancia administrada de Azure SQL Database
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: allenwux
ms.author: xiwu
ms.reviewer: mathoma
manager: craigg
ms.date: 02/07/2019
ms.openlocfilehash: c72c4d21f948d6d6c4d1d4598efa0e13de9705a6
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2019
ms.locfileid: "64926194"
---
# <a name="configure-replication-in-an-azure-sql-database-managed-instance-database"></a>Configuración de la replicación en una base de datos de instancia administrada de Azure SQL Database

La replicación transaccional permite replicar datos en una base de datos de instancia administrada de Azure SQL Database desde una base de datos de SQL Server u otro tipo de bases de datos de instancias. 

También puede usar la replicación transaccional para insertar los cambios realizados en una instancia de la base de datos en la instancia administrada de Azure SQL Database para:

- Una base de datos de SQL Server.
- Una sola base de datos en Azure SQL Database.
- Una base de datos agrupada en un grupo elástico de Azure SQL Database.
 
La replicación transaccional está en versión preliminar pública en [instancia administrada de Azure SQL Database](sql-database-managed-instance.md). Una Instancia administrada puede hospedar bases de datos del publicador, distribuidor y suscriptor. Consulte las [configuraciones de la replicación transaccional](sql-database-managed-instance-transactional-replication.md#common-configurations) para ver las opciones disponibles.

  > [!NOTE]
  > Este artículo está pensado para orientar a un usuario en la configuración de replicación con una base de datos de Azure a instancia administrada de un extremo a extremo, empezando por la creación del grupo de recursos. Si ya ha administrado instancias implementadas, ir directamente a [paso 4](#4---create-a-publisher-database) para crear la base de datos del publicador o [paso 6](#6---configure-distribution) si ya tiene una base de datos del publicador y el suscriptor y está listo para comenzar configurar la replicación.  

## <a name="requirements"></a>Requisitos

Se requiere la configuración de una instancia administrada para que funcione como un publicador o un distribuidor:

- La instancia administrada no debe participar actualmente en una relación de replicación geográfica.
- Que el publicador de la instancia administrada está en la misma red virtual como el distribuidor y el suscriptor, o [emparejamiento de vNet](../virtual-network/tutorial-connect-virtual-networks-powershell.md) se ha establecido entre las redes virtuales de las tres entidades. 
- La conectividad usa la autenticación de SQL entre los participantes de la replicación.
- Un recurso compartido de cuenta de Azure Storage para el directorio de trabajo de replicación.
- El puerto 445 (salida TCP) está abierto en las reglas de seguridad de NSG para las instancias administradas para acceder al recurso compartido de archivos de Azure. 


 > [!NOTE]
 > Las bases de datos únicas y agrupadas de Azure SQL Database solo pueden actuar como suscriptores. 


## <a name="features"></a>Características

Admite:

- Una combinación de la replicación de instantáneas y la replicación transaccional de instancias locales de SQL Server e instancias administradas de Azure SQL Database.
- Los suscriptores pueden ser en bases de datos de SQL Server local, solo las bases de datos administrados/de instancias en Azure SQL Database o bases de datos agrupadas en grupos elásticos de Azure SQL Database.
- Replicación unidireccional o bidireccional.

La siguientes características no pueden utilizarse en las instancias administradas de Azure SQL Database:

- [Suscripciones actualizables](/sql/relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication).
- [Replicación geográfica activa](sql-database-active-geo-replication.md) y [grupos de conmutación por error automática](sql-database-auto-failover-group.md) no debe usarse si se configura la replicación transaccional.
 
## <a name="1---create-a-resource-group"></a>1 - crear un grupo de recursos

Use la [portal Azure](https://portal.azure.com) para crear un grupo de recursos con el nombre `SQLMI-Repl`.  

## <a name="2---create-managed-instances"></a>2 - Creación de instancias administradas

Use la [portal Azure](https://portal.azure.com) para crear dos [las instancias administradas](sql-database-managed-instance-create-tutorial-portal.md) en la misma red virtual y subred. Las dos instancias administradas deben tener un nombre:

- `sql-mi-pub`
- `sql-mi-sub`

También deberá [configurar una máquina virtual de Azure para conectar](sql-database-managed-instance-configure-vm.md) a Azure SQL Database de las instancias administradas. 

## <a name="3---create-azure-storage-account"></a>3 - Creación de cuenta de Azure Storage

[Crear una cuenta de almacenamiento de Azure](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account#create-a-storage-account) para el directorio de trabajo y, a continuación, cree un [recurso compartido de archivos](../storage/files/storage-how-to-create-file-share.md) dentro de la cuenta de almacenamiento. 

Copie la ruta de acceso del recurso compartido de archivo en el formato: `\\storage-account-name.file.core.windows.net\file-share-name`

Copie las claves de acceso de almacenamiento con el formato: `DefaultEndpointsProtocol=https;AccountName=<Storage-Account-Name>;AccountKey=****;EndpointSuffix=core.windows.net`

 Para obtener más información, consulte [Visualización y copia de las claves de acceso de almacenamiento](../storage/common/storage-account-manage.md#access-keys). 

## <a name="4---create-a-publisher-database"></a>4 - Creación de una base de datos del publicador

Conectarse a su `sql-mi-pub` administra la instancia mediante SQL Server Management Studio y ejecute el siguiente código Transact-SQL (T-SQL) para crear la base de datos del publicador:

```sql
USE [master]
GO

CREATE DATABASE [ReplTran_PUB]
GO

USE [ReplTran_PUB]
GO
CREATE TABLE ReplTest (
    ID INT NOT NULL PRIMARY KEY,
    c1 VARCHAR(100) NOT NULL,
    dt1 DATETIME NOT NULL DEFAULT getdate()
)
GO


USE [ReplTran_PUB]
GO

INSERT INTO ReplTest (ID, c1) VALUES (6, 'pub')
INSERT INTO ReplTest (ID, c1) VALUES (2, 'pub')
INSERT INTO ReplTest (ID, c1) VALUES (3, 'pub')
INSERT INTO ReplTest (ID, c1) VALUES (4, 'pub')
INSERT INTO ReplTest (ID, c1) VALUES (5, 'pub')
GO
SELECT * FROM ReplTest
GO
```

## <a name="5---create-a-subscriber-database"></a>5: creación de una base de datos de suscriptor

Conectarse a su `sql-mi-sub` administra la instancia mediante SQL Server Management Studio y ejecute el siguiente código de T-SQL para crear la base de datos de suscriptor vacía:

```sql
USE [master]
GO

CREATE DATABASE [ReplTran_SUB]
GO

USE [ReplTran_SUB]
GO
CREATE TABLE ReplTest (
    ID INT NOT NULL PRIMARY KEY,
    c1 VARCHAR(100) NOT NULL,
    dt1 DATETIME NOT NULL DEFAULT getdate()
)
GO
```

## <a name="6---configure-distribution"></a>6: configurar la distribución

Conectarse a su `sql-mi-pub` administra la instancia mediante SQL Server Management Studio y ejecute el siguiente código Transact-SQL para configurar la base de datos de distribución. 

```sql
USE [master]
GO

EXEC sp_adddistributor @distributor = @@ServerName;
EXEC sp_adddistributiondb @database = N'distribution';
GO
```

## <a name="7---configure-publisher-to-use-distributor"></a>7: configurar el publicador para utilizar el distribuidor 

En el publicador de la instancia administrada `sql-mi-pub`, cambiar la ejecución de consultas en [SQLCMD](/sql/ssms/scripting/edit-sqlcmd-scripts-with-query-editor) modo y ejecute el código siguiente para registrar el distribuidor nuevo con el publicador. 

```sql
:setvar username loginUsedToAccessSourceManagedInstance
:setvar password passwordUsedToAccessSourceManagedInstance
:setvar file_storage "\\storage-account-name.file.core.windows.net\file-share-name"
:setvar file_storage_key "DefaultEndpointsProtocol=https;AccountName=<Storage-Account-Name>;AccountKey=****;EndpointSuffix=core.windows.net"


USE [master]
EXEC sp_adddistpublisher
  @publisher = @@ServerName,
  @distribution_db = N'distribution',
  @security_mode = 0,
  @login = N'$(username)',
  @password = N'$(password)',
  @working_directory = N'$(file_storage)',
  @storage_connection_string = N'$(file_storage_key)';
```

Este script configura un publicador local en la instancia administrada, se agrega un servidor vinculado y crea un conjunto de trabajos del Agente SQL Server. 

## <a name="8---create-publication-and-subscriber"></a>8 - crear la publicación y suscriptor

Uso de [SQLCMD](/sql/ssms/scripting/edit-sqlcmd-scripts-with-query-editor) modo, ejecute el siguiente script de Transact-SQL para habilitar la replicación de la base de datos y configurar la replicación entre el publicador, distribuidor y suscriptor. 

```sql
-- Set variables
:setvar username sourceLogin
:setvar password sourcePassword
:setvar source_db ReplTran_PUB
:setvar publication_name PublishData
:setvar object ReplTest
:setvar schema dbo
:setvar target_server "sql-mi-sub.wdec33262scj9dr27.database.windows.net"
:setvar target_username targetLogin
:setvar target_password targetPassword
:setvar target_db ReplTran_SUB

-- Enable replication for your source database
USE [$(source_db)]
EXEC sp_replicationdboption
  @dbname = N'$(source_db)',
  @optname = N'publish',
  @value = N'true';

-- Create your publication
EXEC sp_addpublication
  @publication = N'$(publication_name)',
  @status = N'active';


-- Configure your log reaer agent
EXEC sp_changelogreader_agent
  @publisher_security_mode = 0,
  @publisher_login = N'$(username)',
  @publisher_password = N'$(password)',
  @job_login = N'$(username)',
  @job_password = N'$(password)';

-- Add the publication snapshot
EXEC sp_addpublication_snapshot
  @publication = N'$(publication_name)',
  @frequency_type = 1,
  @publisher_security_mode = 0,
  @publisher_login = N'$(username)',
  @publisher_password = N'$(password)',
  @job_login = N'$(username)',
  @job_password = N'$(password)';

-- Add the ReplTest table to the publication
EXEC sp_addarticle 
  @publication = N'$(publication_name)',
  @type = N'logbased',
  @article = N'$(object)',
  @source_object = N'$(object)',
  @source_owner = N'$(schema)';

-- Add the subscriber
EXEC sp_addsubscription
  @publication = N'$(publication_name)',
  @subscriber = N'$(target_server)',
  @destination_db = N'$(target_db)',
  @subscription_type = N'Push';

-- Create the push subscription agent
EXEC sp_addpushsubscription_agent
  @publication = N'$(publication_name)',
  @subscriber = N'$(target_server)',
  @subscriber_db = N'$(target_db)',
  @subscriber_security_mode = 0,
  @subscriber_login = N'$(target_username)',
  @subscriber_password = N'$(target_password)',
  @job_login = N'$(target_username)',
  @job_password = N'$(target_password)';

-- Initialize the snapshot
EXEC sp_startpublication_snapshot
  @publication = N'$(publication_name)';
```

## <a name="9---modify-agent-parameters"></a>9 - modificar parámetros del agente

Instancia administrada de Azure SQL Database está experimentando algunos problemas de back-end con la conectividad con los agentes de replicación. Aunque este problema es que se redirige dirigido, la solución para aumentar el valor de tiempo de espera de inicio de sesión para los agentes de replicación. 

Ejecute el siguiente comando de Transact-SQL en el publicador para aumentar el tiempo de espera de inicio de sesión: 

```sql
-- Increase login timeout to 150s
update msdb..sysjobsteps set command = command + N' -LoginTimeout 150' 
where subsystem in ('Distribution','LogReader','Snapshot') and command not like '%-LoginTimeout %'
```

Ejecute el siguiente comando de Transact-SQL para establecer el tiempo de espera de inicio de sesión en el valor predeterminado, si necesita hacerlo:

```sql
-- Increase login timeout to 30
update msdb..sysjobsteps set command = command + N' -LoginTimeout 30' 
where subsystem in ('Distribution','LogReader','Snapshot') and command not like '%-LoginTimeout %'
```

Reinicie los tres agentes para aplicar estos cambios. 

## <a name="10---test-replication"></a>10 - probar la replicación

Una vez que se ha configurado la replicación, puede probarla insertando nuevos elementos en el publicador y observar los cambios se propagan al suscriptor. 

Ejecute el siguiente fragmento de Transact-SQL para ver las filas en el suscriptor:

```sql
select * from dbo.ReplTest
```

Ejecute el siguiente fragmento de Transact-SQL para insertar filas adicionales en el publicador y, a continuación, compruebe las filas de nuevo en el suscriptor. 

```sql
INSERT INTO ReplTest (ID, c1) VALUES (15, 'pub')
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Para quitar la publicación, ejecute el siguiente comando de T-SQL:

```sql
-- Drops the publication
USE [ReplTran_PUB]
EXEC sp_droppublication @publication = N'PublishData'
GO
```

Para quitar la opción de replicación de la base de datos, ejecute el siguiente comando de T-SQL:

```sql
-- Disables publishing of the database
USE [ReplTran_PUB]
EXEC sp_removedbreplication
GO
```

Para deshabilitar la publicación y distribución, ejecute el siguiente comando de T-SQL:

```sql
-- Drops the distributor
USE [master]
EXEC sp_dropdistributor @no_checks = 1
GO
```

Puede limpiar los recursos de Azure por [eliminar los recursos de la instancia administrada desde el grupo de recursos](../azure-resource-manager/manage-resources-portal.md#delete-resources) y, a continuación, eliminar el grupo de recursos `SQLMI-Repl`. 

   
## <a name="see-also"></a>Vea también

- [Replicación transaccional](sql-database-managed-instance-transactional-replication.md)
- [¿Qué es la Instancia administrada?](sql-database-managed-instance.md)
