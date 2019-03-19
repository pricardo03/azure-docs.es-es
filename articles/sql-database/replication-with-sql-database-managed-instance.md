---
title: Configuración de la replicación de una base de datos de instancias administradas de Azure SQL Database | Microsoft Docs
description: Aprenda a configurar la replicación transaccional en una base de datos de instancias administradas de Azure SQL Database
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
ms.openlocfilehash: 164448f2e96b796d21419e90a3965390f22d7d38
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/12/2019
ms.locfileid: "57762983"
---
# <a name="configure-replication-in-an-azure-sql-database-managed-instance-database"></a>Configuración de la replicación en una base de datos de instancias administradas de Azure SQL Database

La replicación transaccional permite replicar datos en una base de datos de instancias administradas de Azure SQL Database desde una base de datos de SQL Server u otro tipo de bases de datos de instancias. También puede usar la replicación transaccional para insertar los cambios realizados en una base de datos de Instancia administrada de Azure SQL Database en una base de datos de SQL Server, en una base de datos única de Azure SQL Database o en una base de datos agrupada en un grupo elástico de Azure SQL Database. La replicación está disponible en versión preliminar pública en [Instancia administrada de Azure SQL Database](sql-database-managed-instance.md). Una Instancia administrada puede hospedar bases de datos del publicador, distribuidor y suscriptor. Consulte las [configuraciones de la replicación transaccional](sql-database-managed-instance-transactional-replication.md#common-configurations) para ver las opciones disponibles.

## <a name="requirements"></a>Requisitos

Para configurar una instancia administrada de forma que funcione como un publicador o distribuidor, deben darse las siguientes condiciones:

- La instancia administrada no debe participar actualmente en una relación de replicación geográfica.

   >[!NOTE]
   >Las bases de datos únicas y agrupadas de Azure SQL Database solo pueden actuar como suscriptores.

- Las instancias administradas deben estar en la misma red virtual.

- La conectividad usa la autenticación de SQL entre los participantes de la replicación.

- Un recurso compartido de cuenta de Azure Storage para el directorio de trabajo de replicación.

- El puerto 445 (salida TCP) debe estar abierto en las reglas de seguridad de la subred de la instancia administrada para tener acceso al recurso compartido de archivos de Azure

## <a name="features"></a>Características

Admite:

- Una combinación de la replicación de instantáneas y la replicación transaccional de instancias locales de SQL Server e instancias administradas de Azure SQL Database.
- Los suscriptores pueden ser bases de datos locales de SQL Server, bases de datos únicas de Azure SQL Database o bases de datos agrupadas en grupos elásticos de Azure SQL Database.
- Replicación unidireccional o bidireccional.

La siguientes características no pueden utilizarse en las instancias administradas de Azure SQL Database:

- Suscripciones actualizables.
- Replicación geográfica activa.

## <a name="configure-publishing-and-distribution-example"></a>Configuración del ejemplo de publicación y distribución

1. [Cree una instancia administrada de Azure SQL Database](sql-database-managed-instance-create-tutorial-portal.md) en el portal.
2. [Cree una cuenta de Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account#create-a-storage-account) para el directorio de trabajo.

   Asegúrese de copiar las claves de almacenamiento. Consulte [Visualización y copia de las claves de acceso de almacenamiento](../storage/common/storage-account-manage.md#access-keys
).
3. Cree una base de datos de instancias para el publicador.

   En los scripts de ejemplo siguientes, reemplace `<Publishing_DB>` por el nombre de esta base de datos de instancias.

4. Cree un usuario de base de datos con autenticación de SQL para el distribuidor. Utilice una contraseña segura.

   En los scripts de ejemplo siguientes, use `<SQL_USER>` y `<PASSWORD>` con el usuario y la contraseña de la base de datos de esta cuenta de SQL Server.

5. [Conéctese a Instancia administrada de Azure SQL Database](sql-database-connect-query-ssms.md).

6. Ejecute la consulta siguiente para agregar el distribuidor y la base de datos de distribución.

   ```sql
   USE [master]
   GO
   EXEC sp_adddistributor @distributor = @@ServerName;
   EXEC sp_adddistributiondb @database = N'distribution';
   ```

7. Para configurar un editor para usar una base de datos de distribución especificada, actualice y ejecute la siguiente consulta.

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

8. Configure el publicador de la replicación.

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

9. Agregue el artículo, la suscripción y el agente de la suscripción de inserción.

   Para agregar estos objetos, actualice el script siguiente.

   - Reemplace `<Object_Name>` por el nombre del objeto de publicación.
   - Reemplace `<Object_Schema>` por el nombre del esquema de origen.
   - Reemplace los otros parámetros entre corchetes angulares `<>` para que coincidan con los valores de los scripts anteriores.

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
   
## <a name="see-also"></a>Vea también

- [Replicación transaccional](sql-database-managed-instance-transactional-replication.md)
- [¿Qué es la Instancia administrada?](sql-database-managed-instance.md)
