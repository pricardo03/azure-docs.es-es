---
title: Replicación con Instancia administrada de Azure SQL Database | Microsoft Docs
description: Obtenga más información acerca del uso de replicación de SQL Server con Instancia administrada de Azure SQL Database
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
ms.date: 09/25/2018
ms.openlocfilehash: 95c27bcc99f08cb1e4998e43a6a2abd508bee0ac
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2018
ms.locfileid: "47228173"
---
# <a name="replication-with-sql-database-managed-instance"></a>Replicación con Instancia administrada de SQL Database

La replicación está disponible para su versión preliminar en [Instancia administrada de Azure SQL Database](sql-database-managed-instance.md). Una Instancia administrada puede hospedar las bases de datos del publicador, distribuidor y suscriptor.

## <a name="common-configurations"></a>Configuraciones comunes

En general, el publicador y el distribuidor deben estar en la nube o en el entorno local. Se admiten las siguientes configuraciones:

- **Publicador con distribuidor local en Instancia administrada**

   ![Replication-with-azure-sql-db-single-managed-instance-publisher-distributor](./media/replication-with-sql-database-managed-instance/01-single-instance-asdbmi-pubdist.png)

   Las bases de datos del publicador y del distribuidor se configuran en una sola Instancia administrada.

- **Publicador con distribuidor remoto en Instancia administrada**

   ![Replication-with-azure-sql-db-separate-managed-instances-publisher-distributor](./media/replication-with-sql-database-managed-instance/02-separate-instances-asdbmi-pubdist.png)

   El publicador y el distribuidor se configuran en dos Instancias administradas. En esta configuración:

  - Las dos Instancias administradas están en la misma red virtual.

  - Las dos Instancias administradas están en la misma ubicación.

- **Publicador y distribuidor locales con suscriptor en la Instancia administrada**

   ![Replication-from-on-premises-to-azure-sql-db-subscriber](./media/replication-with-sql-database-managed-instance/03-azure-sql-db-subscriber.png)

   En esta configuración, una Azure SQL Database es un suscriptor. Esta configuración admite la migración desde el entorno local a Azure. En el rol de suscriptor, la base de datos SQL no requiere la Instancia administrada. Sin embargo, puede usar una Instancia administrada de SQL Database como paso de migración desde el entorno local a Azure. Para obtener más información acerca de los suscriptores de Azure SQL Database, consulte [Replication to SQL Database](replication-to-sql-database.md) (Replicación a SQL Database).

## <a name="requirements"></a>Requisitos

Se requiere el publicador y el distribuidor en Azure SQL Database:

- Instancia administrada de Azure SQL Database.

   >[!NOTE]
   >Las bases de datos de SQL Azure Database que no están configuradas con Instancia administrada solo pueden ser suscriptores.

- Todas las instancias de SQL Server deben estar en la misma red virtual.

- La conectividad usa la autenticación de SQL entre los participantes de la replicación.

- Un recurso compartido de cuenta de Azure Storage para el directorio de trabajo de replicación.

## <a name="features"></a>Características

Admite:

- Mezcla de replicación de instantáneas y transaccional de instancias locales y de Instancia administrada de Azure SQL Database.

- Los suscriptores pueden ser bases de datos únicas y locales de Azure SQL Database o bases de datos agrupadas en grupos elásticos de Azure SQL Database.

- Replicación unidireccional o bidireccional

## <a name="configure-publishing-and-distribution-example"></a>Configuración del ejemplo de publicación y distribución

1. [Cree una Instancia administrada de Azure SQL Database](http://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-create-tutorial-portal) en el portal.

1. [Cree una cuenta de Azure Storage](http://docs.microsoft.com/azure/storage/common/storage-create-storage-account#create-a-storage-account) para el directorio de trabajo. Asegúrese de copiar las claves de almacenamiento. Consulte [Visualización y copia de las claves de acceso de almacenamiento](http://docs.microsoft.com/azure/storage/common/storage-create-storage-account#manage-your-storage-access-keys).

1. Cree una base de datos para el publicador.

   En los scripts de ejemplo siguientes, reemplace `<Publishing_DB>` por el nombre de esta base de datos.

1. Cree un usuario de base de datos con autenticación de SQL para el distribuidor. Consulte [Creación de usuarios de base de datos](http://docs.microsoft.com/azure/sql-database/sql-database-security-tutorial#creating-database-users). Utilice una contraseña segura.

   En los scripts de ejemplo siguientes, use `<SQL_USER>` y `<PASSWORD>` con el usuario y la contraseña de la base de datos de esta cuenta de SQL Server.

1. [Conéctese a Instancia administrada de Azure SQL Database](http://docs.microsoft.com/azure/sql-database/sql-database-connect-query-ssms).

1. Ejecute la consulta siguiente para agregar el distribuidor y la base de datos de distribución.

   ```sql
   USE [master]
   GO
   EXEC sp_adddistributor @distributor = @@ServerName;
   EXEC sp_adddistributiondb @database = N'distribution';
   ```

1. Para configurar un publicador para utilizar una base de datos de distribución específica, actualice y ejecute la consulta siguiente.

   Reemplace `<SQL_USER>` y `<PASSWORD>` por la cuenta y la contraseña de SQL Server.

   Reemplace `\\<STORAGE_ACCOUNT>.file.core.windows.net\<SHARE>` por el valor de la cuenta de almacenamiento.  

   Reemplace `<STORAGE_CONNECTION_STRING>` por la cadena de conexión de la pestaña **Claves de acceso** de la cuenta de almacenamiento de Microsoft Azure.

   Después de actualizar la consulta siguiente, ejecútela. 

   ```sql
   USE [master]
   EXEC sp_adddistpublisher @publisher = @@ServerName,
                @distribution_db = N'distribution',
                @security_mode = 0,
                @login = N'<SQL_USER>',
                @password = N'<PASSWORD>',
                @working_directory = N'\\<STORAGE_ACCOUNT>.file.core.windows.net\<SHARE>',
                @storage_connection_string = N'<STORAGE_CONNECTION_STRING>';
   GO
   ```

1. Configure el publicador de la replicación. 

    En la siguiente consulta, reemplace `<Publishing_DB>` por el nombre de la base de datos del publicador.

    Reemplace `<Publication_Name>` por el nombre para la publicación.

    Reemplace `<SQL_USER>` y `<PASSWORD>` por la cuenta y la contraseña de SQL Server.

    Después de actualizar la consulta, ejecútela para crear la publicación.

   ```sql
   USE [<Publishing_DB>]
   EXEC sp_replicationdboption @dbname = N'<Publishing_DB>',
                @optname = N'publish',
                @value = N'true';

   EXEC sp_addpublication @publication = N'<Publication_Name>',
                @status = N'active';

   EXEC sp_changelogreader_agent @publisher_security_mode = 0,
                @publisher_login = N'<SQL_USER>',
                @publisher_password = N'<PASSWORD>',
                @job_login = N'<SQL_USER>',
                @job_password = N'<PASSWORD>';

   EXEC sp_addpublication_snapshot @publication = N'<Publication_Name>',
                @frequency_type = 1,
                @publisher_security_mode = 0,
                @publisher_login = N'<SQL_USER>',
                @publisher_password = N'<PASSWORD>',
                @job_login = N'<SQL_USER>',
                @job_password = N'<PASSWORD>'
   ```

1. Agregue el artículo, la suscripción y el agente de la suscripción de inserción. 

   Para agregar estos objetos, actualice el script siguiente.

   Reemplace `<Object_Name>` por el nombre del objeto de publicación.

   Reemplace `<Object_Schema>` por el nombre del esquema de origen. 

   Reemplace los otros parámetros entre corchetes angulares `<>` para que coincidan con los valores de los scripts anteriores. 

   ```sql
   EXEC sp_addarticle @publication = N'<Publication_Name>',
                @type = N'logbased',
                @article = N'<Object_Name>',
                @source_object = N'<Object_Name>',
                @source_owner = N'<Object_Schema>'

   EXEC sp_addsubscription @publication = N'<Publication_Name>',
                @subscriber = @@ServerName,
                @destination_db = N'<Subscribing_DB>',
                @subscription_type = N'Push'

   EXEC sp_addpushsubscription_agent @publication = N'<Publication_Name>',
                @subscriber = @@ServerName,
                @subscriber_db = N'<Subscribing_DB>',
                @subscriber_security_mode = 0,
                @subscriber_login = N'<SQL_USER>',
                @subscriber_password = N'<PASSWORD>',
                @job_login = N'<SQL_USER>', 
                @job_password = N'<PASSWORD>'
   GO
   ```

## <a name="limitations"></a>Limitaciones

No se admiten las siguientes características:

- Suscripciones actualizables

- Replicación geográfica activa

## <a name="see-also"></a>Otras referencias

- [¿Qué es la Instancia administrada?](http://docs.microsoft.com/azure/sql-database/sql-database-managed-instance)
