---
title: Diferencias de T-SQL en Instancia administrada de Azure SQL Database | Microsoft Docs
description: En este artículo se describen las diferencias de T-SQL entre una instancia administrada en Azure SQL Database y SQL Server.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein, carlrab, bonova
ms.date: 07/07/2019
ms.custom: seoapril2019
ms.openlocfilehash: fd029c1e7b67d308e3e1fdbedbdc90ea430b4f5b
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2019
ms.locfileid: "68567252"
---
# <a name="azure-sql-database-managed-instance-t-sql-differences-from-sql-server"></a>Diferencias de T-SQL en Instancia administrada de Azure SQL Database

Este artículo resume y explica las diferencias de sintaxis y comportamiento entre Instancia administrada de Azure SQL Database y el Motor de base de datos de SQL Server local. Se van a analizar los siguientes temas: <a name="Differences"></a>

- [Disponibilidad](#availability), incluye las diferencias en [Always-On](#always-on-availability) y [Copias de seguridad](#backup).
- [Seguridad](#security), incluye las diferencias en [Auditoría](#auditing), [Certificados](#certificates), [Credenciales](#credential), [Proveedores de servicios criptográficos](#cryptographic-providers), [Inicios de sesión y usuarios](#logins-and-users), [Clave maestra de servicio y clave de servicio](#service-key-and-service-master-key).
- [Configuración](#configuration), incluye las diferencias en [Extensión del grupo de búferes](#buffer-pool-extension), [Intercalación](#collation), [Niveles de compatibilidad](#compatibility-levels),[Creación de reflejo de la base de datos ](#database-mirroring), [Opciones de base de datos](#database-options), [Agente SQL Server](#sql-server-agent) y [Opciones de tabla](#tables).
- [Funcionalidades](#functionalities), incluidas [BULK INSERT/OPENROWSET](#bulk-insert--openrowset), [CLR](#clr), [DBCC](#dbcc), [Transacciones distribuidas](#distributed-transactions), [Eventos extendidos](#extended-events), [Bibliotecas externas](#external-libraries), [Filestream y Filetable](#filestream-and-filetable), [Búsqueda semántica de texto completo](#full-text-semantic-search), [Servidores vinculados](#linked-servers), [PolyBase](#polybase), [Replicación](#replication), [RESTORE](#restore-statement), [Service Broker](#service-broker), [Funciones, procedimientos almacenados y desencadenadores](#stored-procedures-functions-and-triggers).
- [Configuración del entorno](#Environment), como las configuraciones de redes virtuales y subredes.
- [Características que tienen un comportamiento diferente en instancias administradas](#Changes).
- [Limitaciones temporales y problemas conocidos](#Issues).

La opción de implementación de la instancia administrada proporciona gran compatibilidad con instancias locales del Motor de base de datos de SQL Server. Instancia administrada admite la mayoría de las características del Motor de base de datos de SQL Server.

![Migración](./media/sql-database-managed-instance/migration.png)

## <a name="availability"></a>Disponibilidad

### <a name="always-on-availability"></a>Always On

La [alta disponibilidad](sql-database-high-availability.md) está integrada en Instancia administrada y no la pueden controlar los usuarios. No se admiten las siguientes instrucciones:

- [CREATE ENDPOINT … FOR DATABASE_MIRRORING](https://docs.microsoft.com/sql/t-sql/statements/create-endpoint-transact-sql)
- [CREATE AVAILABILITY GROUP](https://docs.microsoft.com/sql/t-sql/statements/create-availability-group-transact-sql)
- [ALTER AVAILABILITY GROUP](https://docs.microsoft.com/sql/t-sql/statements/alter-availability-group-transact-sql)
- [DROP AVAILABILITY GROUP](https://docs.microsoft.com/sql/t-sql/statements/drop-availability-group-transact-sql)
- La cláusula [SET HADR](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-set-hadr) de la instrucción [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql)

### <a name="backup"></a>Copia de seguridad

Las instancias administradas hacen copias de seguridad automáticas, por lo que los usuarios pueden crear copias de seguridad `COPY_ONLY` de bases de datos completas. No se admiten copias de seguridad de instantáneas de archivos, de registro ni diferenciales.

- Con una instancia administrada, puede hacer una copia de seguridad de una base de datos de instancia solo en una cuenta de Azure Blob Storage:
  - Solo se admite `BACKUP TO URL`.
  - No se admiten `FILE`, `TAPE` ni dispositivos de copia de seguridad.
- Se admite la mayoría de las opciones de `WITH` generales.
  - `COPY_ONLY` es obligatorio.
  - `FILE_SNAPSHOT` no se admite.
  - No se admiten las opciones de cinta `REWIND`, `NOREWIND`, `UNLOAD` y `NOUNLOAD`.
  - No se admiten las opciones específicas de registro `NORECOVERY`, `STANDBY` y `NO_TRUNCATE`.

Limitaciones: 

- Con una instancia administrada, puede hacer una copia de seguridad de una base de datos de instancia en una copia de seguridad con hasta 32 franjas, lo cual es suficiente para bases de datos de hasta 4 TB si se usa la compresión de copia de seguridad.
- El tamaño máximo de una franja de copia de seguridad con el uso del comando `BACKUP` en una instancia administrada es de 195 GB, lo cual es el tamaño máximo del blob. Aumente el número de franjas en el comando de copia de seguridad para reducir el tamaño de cada franja y permanecer dentro de este límite.

    > [!TIP]
    > Para solucionar esta limitación, cuando realice una copia de seguridad de una base de datos desde un servidor de SQL Server en un entorno local o en una máquina virtual, puede:
    >
    > - Realizar la copia de seguridad en `DISK` en lugar de hacerlo en `URL`.
    > - Cargar los archivos de copia de seguridad en Blob Storage.
    > - Realizar la restauración en la instancia administrada.
    >
    > El comando `Restore` de una instancia administrada admite tamaños de blob más grandes en los archivos de copia de seguridad porque se usa un tipo de blob distinto para el almacenamiento de los archivos de copia de seguridad cargados.

Para más información acerca de las copias de seguridad mediante T-SQL, consulte [BACKUP](https://docs.microsoft.com/sql/t-sql/statements/backup-transact-sql).

## <a name="security"></a>Seguridad

### <a name="auditing"></a>Auditoría

Las diferencias clave entre la auditoría en bases de datos en Azure SQL Database y bases de datos en SQL Server son las siguientes:

- Con la opción de implementación de instancia administrada en Azure SQL Database, la auditoría funciona en el nivel de servidor. Los archivos de registro `.xel` se almacenan en Azure Blob Storage.
- Con las opciones de implementación de base de datos única y grupo elástico en Azure SQL Database, la auditoría funciona en el nivel de la base de datos.
- En servidores de SQL Server locales o en los de máquinas virtuales, la auditoría funciona en el nivel de servidor. Los eventos se almacenan en el sistema de archivos o en los registros de eventos de Windows.
 
En Instancia administrada, la auditoría de XEvent admite Azure Blob Storage como destino. No se admiten archivos ni registros de Windows.

Las principales diferencias en la sintaxis de `CREATE AUDIT` para la auditoría en Azure Blob Storage son:

- Se proporciona una nueva sintaxis de `TO URL` que puede usar para especificar la dirección URL del contenedor de Azure Blob Storage donde se colocarán los archivos `.xel`.
- La sintaxis `TO FILE` no se admite porque una instancia administrada no puede acceder a los recursos compartidos de archivos de Windows.

Para más información, consulte: 

- [CREATE SERVER AUDIT](https://docs.microsoft.com/sql/t-sql/statements/create-server-audit-transact-sql) 
- [ALTER SERVER AUDIT](https://docs.microsoft.com/sql/t-sql/statements/alter-server-audit-transact-sql)
- [Auditoría](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine)

### <a name="certificates"></a>Certificados

Una instancia administrada no puede acceder a los recursos compartidos de archivos ni a las carpetas de Windows, por lo que se aplican las siguientes restricciones:

- El archivo `CREATE FROM`/`BACKUP TO` no se admite para certificados.
- No se admite el certificado `CREATE`/`BACKUP` de `FILE`/`ASSEMBLY`. No se pueden usar archivos de clave privada. 

Consulte [CREATE CERTIFICATE](https://docs.microsoft.com/sql/t-sql/statements/create-certificate-transact-sql) y [BACKUP CERTIFICATE](https://docs.microsoft.com/sql/t-sql/statements/backup-certificate-transact-sql). 
 
**Solución alternativa**: aplique el script al certificado o la clave privada, guarde como archivo .sql y cree a partir del archivo binario:

```sql
CREATE CERTIFICATE  
   FROM BINARY = asn_encoded_certificate
WITH PRIVATE KEY (<private_key_options>)
```

### <a name="credential"></a>Credential:

Solo se admiten identidades de Azure Key Vault y `SHARED ACCESS SIGNATURE`. No se admiten usuarios de Windows.

Consulte [CREATE CREDENTIAL](https://docs.microsoft.com/sql/t-sql/statements/create-credential-transact-sql) y [ALTER CREDENTIAL](https://docs.microsoft.com/sql/t-sql/statements/alter-credential-transact-sql).

### <a name="cryptographic-providers"></a>Proveedores de servicios criptográficos

Una instancia administrada no puede acceder a archivos, por lo que no se pueden crear proveedores de servicios criptográficos:

- `CREATE CRYPTOGRAPHIC PROVIDER` no se admite. Consulte [CREATE CRYPTOGRAPHIC PROVIDER](https://docs.microsoft.com/sql/t-sql/statements/create-cryptographic-provider-transact-sql).
- `ALTER CRYPTOGRAPHIC PROVIDER` no se admite. Consulte [ALTER CRYPTOGRAPHIC PROVIDER](https://docs.microsoft.com/sql/t-sql/statements/alter-cryptographic-provider-transact-sql).

### <a name="logins-and-users"></a>Inicios de sesión y usuarios

- Se admiten los inicios de sesión de SQL creados con `FROM CERTIFICATE`, `FROM ASYMMETRIC KEY` y `FROM SID`. Consulte [CREATE LOGIN](https://docs.microsoft.com/sql/t-sql/statements/create-login-transact-sql).
- Se admiten las entidades de seguridad (inicios de sesión) del servidor de Azure Active Directory (Azure AD) creadas con la sintaxis [CREATE LOGIN](https://docs.microsoft.com/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current) o la sintaxis [CREATE USER FROM LOGIN [Azure AD Login]](https://docs.microsoft.com/sql/t-sql/statements/create-user-transact-sql?view=azuresqldb-mi-current) (versión preliminar pública). Estos inicios de sesión se crean en el nivel de servidor.

    La instancia administrada admite las entidades de seguridad de la base de datos de Azure AD con la sintaxis `CREATE USER [AADUser/AAD group] FROM EXTERNAL PROVIDER`. Esta característica también se conoce como usuarios de bases de datos independientes de Azure AD.

- No se admiten los inicios de sesión de Windows creados con la sintaxis `CREATE LOGIN ... FROM WINDOWS`. Use los usuarios e inicios de sesión de Azure Active Directory.
- El usuario de Azure AD que creó la instancia tiene [privilegios de administrador sin restricciones](sql-database-manage-logins.md#unrestricted-administrative-accounts).
- Los usuarios de nivel de base de datos de Azure AD que no son administradores pueden crearse con la sintaxis `CREATE USER ... FROM EXTERNAL PROVIDER`. Consulte [CREATE USER ... FROM EXTERNAL PROVIDER](sql-database-manage-logins.md#non-administrator-users).
- Las entidades de seguridad (inicios de sesión) del servidor de Azure AD admiten las características SQL en una única instancia administrada. No se admiten las características que requieren una interacción entre instancias, con independencia de que se encuentren en el mismo inquilino de Azure AD o en inquilinos diferentes, para los usuarios de Azure AD. Ejemplos de estas características son los siguientes:

  - Replicación transaccional de SQL.
  - Servidor de vínculos.

- No se admite el establecimiento de un inicio de sesión de Azure AD asignado a un grupo de Azure AD como propietario de la base de datos.
- Se admite la suplantación de las entidades de seguridad en el nivel de servidor de Azure AD mediante otras entidades de seguridad de Azure AD, como la cláusula [EXECUTE AS](/sql/t-sql/statements/execute-as-transact-sql). Las limitaciones de EXECUTE AS son:

  - No se admite la cláusula EXECUTE AS USER para usuarios de Azure AD cuando el nombre es diferente del nombre de inicio de sesión. Por ejemplo, cuando el usuario se crea mediante la sintaxis CREATE USER [myAadUser] FROM LOGIN [john@contoso.com] y se intenta suplantar la identidad mediante EXEC AS USER = _myAadUser_. Cuando cree un usuario en **USER** a partir de una entidad de seguridad (inicio de sesión) de un servidor de Azure AD, especifique un valor de user_name igual que el valor de login_name que se obtiene de **LOGIN**.
  - Solo las entidades de seguridad (inicios de sesión) a nivel de servidor SQL que forman parte del rol `sysadmin` pueden ejecutar las siguientes operaciones dirigidas a las entidades de seguridad de Azure AD:

    - EXECUTE AS USER
    - EXECUTE AS LOGIN

- Limitaciones de la versión preliminar pública para las entidades de seguridad (inicios de sesión) del servidor de Azure AD:

  - Limitaciones de administrador de Active Directory para Instancia administrada:

    - El administrador de Azure AD usado para configurar la instancia administrada no se puede usar para crear una entidad de seguridad (inicio de sesión) de un servidor de Azure AD dentro de la instancia administrada. Debe crear la primera entidad de seguridad (inicio de sesión) de un servidor de Azure AD mediante una cuenta de SQL Server que tenga el rol `sysadmin`. Esta es una limitación temporal que se quitará cuando las entidades de seguridad (inicios de sesión) de un servidor de Azure AD pasen a ser de disponibilidad general. Si intenta usar una cuenta de administrador de Azure AD para crear el inicio de sesión, verá el siguiente error: `Msg 15247, Level 16, State 1, Line 1 User does not have permission to perform this action.`
      - Actualmente, el primer inicio de sesión de Azure AD creado en la base de datos maestra debe crearlo la cuenta estándar de SQL Server (no de Azure AD) que tenga el rol `sysadmin` mediante la cláusula [CREATE LOGIN](/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current) FROM EXTERNAL PROVIDER. Después de la fase de disponibilidad general, se eliminará esta limitación. Posteriormente, podrá crear un inicio de sesión de Azure AD inicial mediante el administrador de Active Directory para Instancia administrada.
    - No se admite el uso de DacFx (exportación/importación) con SQL Server Management Studio o SqlPackage para los inicios de sesión de Azure AD. Esta es una limitación que se quitará cuando las entidades de seguridad (inicios de sesión) de un servidor de Azure AD pasen a ser de disponibilidad general.
    - Uso de entidades de seguridad (inicios de sesión) del servidor de Azure AD con SQL Server Management Studio:

      - No se admiten los inicios de sesión de scripting de Azure AD que usen cualquier inicio de sesión autenticado.
      - Intellisense no reconoce la instrucción CREATE LOGIN FROM EXTERNAL PROVIDER y muestra un subrayado rojo.

- Solo el inicio de sesión de la entidad de seguridad a nivel de servidor (el que crea el proceso de aprovisionamiento de Instancia administrada), los miembros de los roles de servidor como `securityadmin` o `sysadmin`, u otros inicios de sesión con permiso ALTER ANY LOGIN en el nivel de servidor pueden crear entidades de seguridad (inicios de sesión) en el nivel de servidor de Azure AD en la base de datos maestra para Instancia administrada.
- Si el inicio de sesión es una entidad de seguridad de SQL, solo los inicios de sesión que forman parte del rol `sysadmin` pueden utilizar el comando create para crear inicios de sesión para una cuenta de Azure AD.
- El inicio de sesión de Azure AD debe ser un miembro de una instancia de Azure AD dentro del mismo directorio que se usa para Instancia administrada de Azure SQL Database.
- Las entidades de seguridad (inicio de sesión) del servidor de Azure AD son visibles en el explorador de objetos a partir de la versión preliminar 5 de SQL Server Management Studio 18.0.
- Se permite la superposición de las entidades de seguridad (inicios de sesión) del servidor de Azure AD con una cuenta de administrador de Azure AD. Las entidades de seguridad (inicios de sesión) del servidor de Azure AD tienen prioridad sobre el administrador de Azure AD cuando se resuelve la entidad de seguridad y se aplican permisos a la instancia administrada.
- Durante la autenticación, se aplica la siguiente secuencia para resolver la entidad de seguridad de autenticación:

    1. Si la cuenta de Azure AD existe como directamente asignada a la entidad de seguridad (inicio de sesión) del servidor de Azure AD que está presente en sys.server_principals como tipo "E", conceda acceso y aplique los permisos de la entidad de seguridad (inicio de sesión) del servidor de Azure AD.
    2. Si la cuenta de Azure AD es un miembro de un grupo de Azure AD que está asignado a la entidad de seguridad (inicio de sesión) del servidor de Azure AD (presente en sys.server_principals como tipo "X"), conceda acceso y aplique los permisos del inicio de sesión del grupo de Azure AD.
    3. Si la cuenta de Azure AD es un administrador de Azure AD configurado en un portal especial de Instancia administrada, que no existe en las vistas de sistema de Instancia administrada, aplique permisos fijos especiales del administrador de Azure AD para Instancia administrada (modo heredado).
    4. Si la cuenta de Azure AD existe como directamente asignada a un usuario de Azure AD de una base de datos, presente en sys.database_principals como tipo "E", conceda acceso y aplique los permisos del usuario de la base de datos de Azure AD.
    5. Si la cuenta de Azure AD es un miembro de un grupo de Azure AD que está asignado a un usuario de Azure AD de una base de datos, presente en sys.database_principals como tipo "X", conceda acceso y aplique los permisos del inicio de sesión del grupo de Azure AD.
    6. Si hay un inicio de sesión de Azure AD asignado a una cuenta de usuario de Azure AD o a una cuenta de grupo de Azure AD, la cual resuelve la autenticación del usuario, se aplicarán todos los permisos de este inicio de sesión de Azure AD.

### <a name="service-key-and-service-master-key"></a>Clave maestra de servicio y clave de servicio

- No se admiten las [copias de seguridad de la clave maestra](https://docs.microsoft.com/sql/t-sql/statements/backup-master-key-transact-sql) (administrada por el servicio de SQL Database).
- No se admite la [restauración de la clave maestra](https://docs.microsoft.com/sql/t-sql/statements/restore-master-key-transact-sql) (administrada por el servicio de SQL Database).
- No se admiten las [copias de seguridad de la clave maestra del servicio](https://docs.microsoft.com/sql/t-sql/statements/backup-service-master-key-transact-sql) (administrada por el servicio de SQL Database).
- No se admite la [restauración de la clave maestra del servicio](https://docs.microsoft.com/sql/t-sql/statements/restore-service-master-key-transact-sql) (administrada por el servicio de SQL Database).

## <a name="configuration"></a>Configuración

### <a name="buffer-pool-extension"></a>Extensión del grupo de búferes

- No se admite la [extensión del grupo de búferes](https://docs.microsoft.com/sql/database-engine/configure-windows/buffer-pool-extension).
- `ALTER SERVER CONFIGURATION SET BUFFER POOL EXTENSION` no se admite. Consulte [ALTER SERVER CONFIGURATION](https://docs.microsoft.com/sql/t-sql/statements/alter-server-configuration-transact-sql).

### <a name="collation"></a>Collation

La intercalación de la instancia predeterminada es `SQL_Latin1_General_CP1_CI_AS` y puede especificarse como un parámetro de creación. Consulte [Intercalaciones](https://docs.microsoft.com/sql/t-sql/statements/collations).

### <a name="compatibility-levels"></a>Niveles de compatibilidad

- Los niveles de compatibilidad admitidos son 100, 110, 120, 130 y 140.
- No se admiten los niveles de compatibilidad menores que 100.
- El nivel de compatibilidad predeterminado es 140 para las bases de datos nuevas. Para las bases de datos restauradas, el nivel de compatibilidad no cambiará si era 100 o superior.

Consulte [Nivel de compatibilidad de ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level).

### <a name="database-mirroring"></a>Creación de reflejo de la base de datos

No se admite la creación de reflejo de la base de datos.

- Las opciones `ALTER DATABASE SET PARTNER` y `SET WITNESS` no se admiten.
- `CREATE ENDPOINT … FOR DATABASE_MIRRORING` no se admite.

Para más información, consulte [ALTER DATABASE SET PARTNER y SET WITNESS](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-database-mirroring) y [CREATE ENDPOINT … FOR DATABASE_MIRRORING](https://docs.microsoft.com/sql/t-sql/statements/create-endpoint-transact-sql).

### <a name="database-options"></a>Opciones de base de datos

- No se permite usar varios archivos de registro.
- No se admiten objetos en memoria caché en el nivel de servicio de uso general. 
- Hay un límite de 280 archivos por instancia de uso general, lo cual implica un máximo de 280 archivos por base de datos. Los datos y archivos de registro en el nivel de uso general cuentan para este límite. [El nivel Crítico para la empresa admite 32 767 archivos por base de datos](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-resource-limits#service-tier-characteristics).
- La base de datos no puede contener grupos de archivos que contengan datos de secuencia de archivos. Se producirá un error en la restauración si el archivo .bak contiene datos `FILESTREAM`. 
- Todos los archivos se colocan en Azure Blob Storage. La E/S y el rendimiento por archivo dependen del tamaño de cada archivo individual.

#### <a name="create-database-statement"></a>Instrucción CREATE DATABASE

Se aplican las siguientes limitaciones a `CREATE DATABASE`:

- No se pueden definir archivos y grupos de archivos. 
- La opción `CONTAINMENT` no se admite. 
- Las opciones `WITH` no se admiten. 
   > [!TIP]
   > Como alternativa, use `ALTER DATABASE` después de `CREATE DATABASE` para establecer las opciones de la base de datos para agregar archivos o establecer el contenedor. 

- La opción `FOR ATTACH` no se admite.
- La opción `AS SNAPSHOT OF` no se admite.

Para más información, consulte [CREATE DATABASE](https://docs.microsoft.com/sql/t-sql/statements/create-database-sql-server-transact-sql).

#### <a name="alter-database-statement"></a>Instrucción ALTER DATABASE

Algunas propiedades de archivo no se pueden establecer ni cambiar:

- No se puede especificar una ruta de acceso del archivo en la instrucción T-SQL `ALTER DATABASE ADD FILE (FILENAME='path')`. Quite `FILENAME` del script porque una instancia administrada coloca automáticamente los archivos. 
- El nombre del archivo no se puede cambiar mediante la instrucción `ALTER DATABASE`.

Las siguientes opciones se establecen de forma predeterminada y no se pueden cambiar:

- `MULTI_USER`
- `ENABLE_BROKER ON`
- `AUTO_CLOSE OFF`

Las opciones siguientes no se pueden modificar:

- `AUTO_CLOSE`
- `AUTOMATIC_TUNING(CREATE_INDEX=ON|OFF)`
- `AUTOMATIC_TUNING(DROP_INDEX=ON|OFF)`
- `DISABLE_BROKER`
- `EMERGENCY`
- `ENABLE_BROKER`
- `FILESTREAM`
- `HADR`
- `NEW_BROKER`
- `OFFLINE`
- `PAGE_VERIFY`
- `PARTNER`
- `READ_ONLY`
- `RECOVERY BULK_LOGGED`
- `RECOVERY_SIMPLE`
- `REMOTE_DATA_ARCHIVE` 
- `RESTRICTED_USER`
- `SINGLE_USER`
- `WITNESS`

Para más información, consulte [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-file-and-filegroup-options).

### <a name="sql-server-agent"></a>Agente SQL Server

- La habilitación o deshabilitación del Agente SQL Server no se admite actualmente en la instancia administrada. El Agente SQL siempre está en ejecución.
- La configuración del Agente SQL Server es de solo lectura. El procedimiento `sp_set_agent_properties` no se admite en Instancia administrada. 
- Trabajos
  - Se admiten los pasos de trabajo de T-SQL.
  - Se admiten los siguientes trabajos de replicación:
    - Lector del registro de transacciones
    - Instantánea
    - Distribuidor.
  - Se admiten los pasos de trabajo de SSIS.
  - Actualmente no se admiten estos otros tipos de pasos de trabajo:
    - No se admite el paso de trabajo de replicación de mezcla. 
    - No se admite el lector de colas. 
    - Aún no se admite el shell de comandos.
  - Las instancias administradas no pueden acceder a los recursos externos como, por ejemplo, a los recursos compartidos de red a través de robocopy. 
  - No se admite SQL Server Analysis Services.
- Las notificaciones se admiten parcialmente.
- Se admite la notificación por correo electrónico, aunque es necesario configurar un perfil de Correo electrónico de base de datos. Agente SQL Server solo puede usar un perfil de Correo electrónico de base de datos y se debe llamar `AzureManagedInstance_dbmail_profile`. 
  - El buscapersonas no se admite.
  - No se admite NetSend.
  - Aún no se admiten las alertas.
  - No se admiten los servidores proxy.
- No se admite EventLog.

Actualmente, no se admiten las siguientes características del Agente SQL:

- Servidores proxy
- Programación de trabajos en una CPU inactiva
- Habilitar o deshabilitar un agente
- Alertas

Para más información acerca del Agente SQL Server, consulte [Agente SQL Server](https://docs.microsoft.com/sql/ssms/agent/sql-server-agent).

### <a name="tables"></a>Tablas

No se admiten las siguientes tablas:

- `FILESTREAM`
- `FILETABLE`
- `EXTERNAL TABLE`
- `MEMORY_OPTIMIZED` 

Para más información sobre cómo crear y modificar tablas, consulte [CREATE TABLE](https://docs.microsoft.com/sql/t-sql/statements/create-table-transact-sql) y [ALTER TABLE](https://docs.microsoft.com/sql/t-sql/statements/alter-table-transact-sql).

## <a name="functionalities"></a>Funcionalidades

### <a name="bulk-insert--openrowset"></a>Bulk insert/openrowset

Una instancia administrada no puede acceder a los recursos compartidos de archivos ni carpetas de Windows, por lo que los archivos se deben importar desde Azure Blob Storage:

- `DATASOURCE` es necesario en el comando `BULK INSERT` durante la importación de archivos desde Azure Blob Storage. Consulte [BULK INSERT](https://docs.microsoft.com/sql/t-sql/statements/bulk-insert-transact-sql).
- `DATASOURCE` se necesita en la función `OPENROWSET` cuando se lee el contenido de un archivo desde Azure Blob Storage. Consulte [OPENROWSET](https://docs.microsoft.com/sql/t-sql/functions/openrowset-transact-sql).

### <a name="clr"></a>CLR

Una instancia administrada no puede acceder a los recursos compartidos de archivos ni a las carpetas de Windows, por lo que se aplican las siguientes restricciones:

- Solo se admite `CREATE ASSEMBLY FROM BINARY`. Consulte [CREATE ASSEMBLY FROM BINARY](https://docs.microsoft.com/sql/t-sql/statements/create-assembly-transact-sql). 
- `CREATE ASSEMBLY FROM FILE` no se admite. Consulte [CREATE ASSEMBLY FROM FILE](https://docs.microsoft.com/sql/t-sql/statements/create-assembly-transact-sql).
- `ALTER ASSEMBLY` no puede hacer referencia a archivos. Consulte [ALTER ASSEMBLY](https://docs.microsoft.com/sql/t-sql/statements/alter-assembly-transact-sql).

### <a name="dbcc"></a>DBCC

Las instancias administradas no admiten instrucciones DBCC no documentadas que estén habilitadas en SQL Server.

- `Trace flags` no se admiten. Consulte [Marcas de seguimiento](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql).
- `DBCC TRACEOFF` no se admite. Consulte [DBCC TRACEOFF](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceoff-transact-sql).
- `DBCC TRACEON` no se admite. Consulte [DBCC TRACEON](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceon-transact-sql).

### <a name="distributed-transactions"></a>Transacciones distribuidas

Actualmente no se admiten MSDTC ni las [Transacciones elásticas](sql-database-elastic-transactions-overview.md) en las instancias administradas.

### <a name="extended-events"></a>Eventos extendidos

No se admiten algunos destinos específicos de Windows para Extended Events (XEvents):

- No se admite el destino `etw_classic_sync`. Almacene los archivos `.xel` en Azure Blob Storage. Consulte [Destino etw_classic_sync](https://docs.microsoft.com/sql/relational-databases/extended-events/targets-for-extended-events-in-sql-server#etw_classic_sync_target-target).
- No se admite el destino `event_file`. Almacene los archivos `.xel` en Azure Blob Storage. Consulte [Destino event_file](https://docs.microsoft.com/sql/relational-databases/extended-events/targets-for-extended-events-in-sql-server#event_file-target).

### <a name="external-libraries"></a>Bibliotecas externas

Aún no se admiten las bibliotecas externas de R y Python para análisis en base de datos. Consulte [Machine Learning Services en SQL Server](https://docs.microsoft.com/sql/advanced-analytics/r/sql-server-r-services).

### <a name="filestream-and-filetable"></a>FileStream y FileTable

- No se admiten datos de secuencia de archivos.
- La base de datos no puede contener grupos de archivos con datos `FILESTREAM`.
- `FILETABLE` no se admite.
- Las tablas no pueden tener tipos `FILESTREAM`.
- No se admiten las siguientes funciones:
  - `GetPathLocator()`
  - `GET_FILESTREAM_TRANSACTION_CONTEXT()`
  - `PathName()`
  - `GetFileNamespacePat)`
  - `FileTableRootPath()`

Para más información, consulte [FILESTREAM](https://docs.microsoft.com/sql/relational-databases/blob/filestream-sql-server) y [FileTables](https://docs.microsoft.com/sql/relational-databases/blob/filetables-sql-server).

### <a name="full-text-semantic-search"></a>Búsqueda semántica de texto completo

No se admite la [búsqueda semántica](https://docs.microsoft.com/sql/relational-databases/search/semantic-search-sql-server).

### <a name="linked-servers"></a>Servidores vinculados

Los servidores vinculados en instancias administradas admiten un número limitado de destinos:

- Los destinos admitidos son las instancias administradas, las bases de datos únicas y las instancias de SQL Server. 
- Los servidores vinculados no admiten transacciones de escritura distribuidas (MS DTC).
- Destinos no admitidos: archivos, Analysis Services y otros RDBMS. Intente usar la importación de CSV nativa desde Azure Blob Storage mediante `BULK INSERT` o `OPENROWSET` como alternativa para la importación de archivos.

Operaciones

- No se admiten las transacciones de escritura entre instancias.
- Se admite `sp_dropserver` para quitar un servidor vinculado. Consulte [sp_dropserver](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-dropserver-transact-sql).
- La función `OPENROWSET` se puede usar para ejecutar consultas solo en instancias de SQL Server. Se pueden administrar de forma local o en máquinas virtuales. Consulte [OPENROWSET](https://docs.microsoft.com/sql/t-sql/functions/openrowset-transact-sql).
- La función `OPENDATASOURCE` se puede usar para ejecutar consultas solo en instancias de SQL Server. Se pueden administrar de forma local o en máquinas virtuales. Solo se admiten los valores `SQLNCLI`, `SQLNCLI11` y `SQLOLEDB` como proveedor. Un ejemplo es `SELECT * FROM OPENDATASOURCE('SQLNCLI', '...').AdventureWorks2012.HumanResources.Employee`. Consulte [OPENDATASOURCE](https://docs.microsoft.com/sql/t-sql/functions/opendatasource-transact-sql).
- Los servidores vinculados no se pueden usar para leer archivos (Excel y CSV) de los recursos compartidos de red. Intente usar [BULK INSERT](https://docs.microsoft.com/sql/t-sql/statements/bulk-insert-transact-sql#e-importing-data-from-a-csv-file) o [OPENROWSET](https://docs.microsoft.com/sql/t-sql/functions/openrowset-transact-sql#g-accessing-data-from-a-csv-file-with-a-format-file) que lee archivos CSV desde Azure Blob Storage. Realice un seguimiento de estas solicitudes en el [elemento de comentarios de la Instancia administrada](https://feedback.azure.com/forums/915676-sql-managed-instance/suggestions/35657887-linked-server-to-non-sql-sources)|.

### <a name="polybase"></a>PolyBase

No se admiten tablas externas que hacen referencia a archivos en HDFS o Azure Blob Storage. Para más información acerca de Polybase, consulte [Polybase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide).

### <a name="replication"></a>Replicación

La [replicación transaccional](sql-database-managed-instance-transactional-replication.md) está disponible para la versión preliminar pública en la Instancia administrada con algunas restricciones:
- Todos los tipos de participantes de la replicación (editor, distribuidor, suscriptor de extracción y suscriptor de inserción) se pueden colocar en la Instancia administrada, pero el editor y el distribuidor no se pueden colocar en instancias diferentes.
- Se admiten los tipos de replicación transaccional, de instantánea y bidireccional. Sin embargo, no se admite la replicación de mezcla, la replicación punto a punto y las suscripciones actualizables.
- La Instancia administrada puede comunicarse con las versiones recientes de SQL Server. Consulte las versiones admitidas [aquí](sql-database-managed-instance-transactional-replication.md#supportability-matrix-for-instance-databases-and-on-premises-systems).
- La replicación transaccional tiene algunos [requisitos de red adicionales](sql-database-managed-instance-transactional-replication.md#requirements).

Para obtener información sobre la configuración de la replicación, vea el [tutorial de replicación](replication-with-sql-database-managed-instance.md).

### <a name="restore-statement"></a>Instrucción RESTORE 

- Sintaxis admitida:
  - `RESTORE DATABASE`
  - `RESTORE FILELISTONLY ONLY`
  - `RESTORE HEADER ONLY`
  - `RESTORE LABELONLY ONLY`
  - `RESTORE VERIFYONLY ONLY`
- Sintaxis no admitida:
  - `RESTORE LOG ONLY`
  - `RESTORE REWINDONLY ONLY`
- Origen: 
  - La única opción admitida es `FROM URL` (Azure Blob Storage).
  - No se admite `FROM DISK`/`TAPE`/dispositivo de copia de seguridad.
  - No se admiten los conjuntos de copia de seguridad.
- Las opciones `WITH` no se admiten, como, por ejemplo, `DIFFERENTIAL` o `STATS`.
- `ASYNC RESTORE`: la restauración continúa aunque se interrumpa la conexión con el cliente. Si la conexión se interrumpe, puede usar `sys.dm_operation_status` para ver el estado de una operación de restauración y para crear y eliminar una base de datos. Consulte [sys.dm_operation_status](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database). 

Las siguientes opciones de base de datos se establecen o invalidan, y no se pueden cambiar más adelante: 

- `NEW_BROKER` si el agente no está habilitado en el archivo .bak. 
- `ENABLE_BROKER` si el agente no está habilitado en el archivo .bak. 
- `AUTO_CLOSE=OFF` si una base de datos en el archivo .bak tiene `AUTO_CLOSE=ON`. 
- `RECOVERY FULL` si una base de datos en el archivo .bak tiene los modos de recuperación `SIMPLE` o `BULK_LOGGED`.
- Se agrega un grupo de archivos optimizado para memoria y se le asigna el nombre XTP si aún no se encuentra en el archivo .bak de origen. 
- Se cambia el nombre de todos los grupos de archivos optimizados para memoria existentes a XTP. 
- Las opciones `SINGLE_USER` y `RESTRICTED_USER` se convierten en `MULTI_USER`.

Limitaciones: 

- Los archivos `.BAK` que contienen varios conjuntos de copia de seguridad no se pueden restaurar. 
- Los archivos `.BAK` que contienen varios archivos de registro no se pueden restaurar.
- Se producirá un error en la restauración si el archivo .bak contiene datos `FILESTREAM`.
- Las copias de seguridad que contienen bases de datos que tienen objetos activos en memoria no se pueden restaurar en una instancia de uso general. Para más información acerca de las instrucciones Restore, consulte [Instrucciones RESTORE](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-transact-sql).

### <a name="service-broker"></a>Service Broker

No se admite el agente de servicio entre instancias:

- `sys.routes`: Como requisito previo, debe seleccionar la dirección de sys.routes. La dirección debe ser LOCAL en todas las rutas. Consulte [sys.routes](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-routes-transact-sql).
- `CREATE ROUTE`: no se puede usar `CREATE ROUTE` con un valor de `ADDRESS` distinto de `LOCAL`. Consulte [CREATE ROUTE](https://docs.microsoft.com/sql/t-sql/statements/create-route-transact-sql).
- `ALTER ROUTE`: no se puede usar `ALTER ROUTE` con un valor de `ADDRESS` distinto de `LOCAL`. Consulte [ALTER ROUTE](https://docs.microsoft.com/sql/t-sql/statements/alter-route-transact-sql). 

### <a name="stored-procedures-functions-and-triggers"></a>Procedimientos almacenados, funciones y desencadenadores

- `NATIVE_COMPILATION` no se admite en el nivel De uso general.
- No se admiten las siguientes opciones de [sp_configure](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-configure-transact-sql): 
  - `allow polybase export`
  - `allow updates`
  - `filestream_access_level`
  - `remote data archive`
  - `remote proc trans`
- `sp_execute_external_scripts` no se admite. Consulte [sp_execute_external_scripts](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql#examples).
- `xp_cmdshell` no se admite. Consulte [xp_cmdshell](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/xp-cmdshell-transact-sql).
- No se admiten `Extended stored procedures`, lo cual incluye `sp_addextendedproc` y `sp_dropextendedproc`. Consulte [Procedimientos almacenados extendidos](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/general-extended-stored-procedures-transact-sql).
- No se admiten `sp_attach_db`, `sp_attach_single_file_db`, y `sp_detach_db`. Consulte [sp_attach_db](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-attach-db-transact-sql), [sp_attach_single_file_db](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-attach-single-file-db-transact-sql), y [sp_detach_db](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-detach-db-transact-sql).

## <a name="Environment"></a>Restricciones del entorno

### <a name="subnet"></a>Subnet
- En la subred reservada para Instancia administrada no se puede colocar ningún otro recurso (por ejemplo máquinas virtuales). Coloque estos recursos en otras subredes.
- La subred debe tener un número de [direcciones IP](sql-database-managed-instance-connectivity-architecture.md#network-requirements) disponibles suficiente. El mínimo es 16, aunque se recomienda tener al menos 32 direcciones IP en la subred.
- [No se pueden asociar los puntos de conexión de servicio con la subred de la instancia administrada](sql-database-managed-instance-connectivity-architecture.md#network-requirements). Asegúrese de que la opción de puntos de conexión de servicio esté deshabilitada al crear la red virtual.
- El número de núcleos virtuales y tipos de instancias que se pueden implementar en una región tiene algunas [restricciones y límites](sql-database-managed-instance-resource-limits.md#regional-resource-limitations).
- Hay algunas [reglas de seguridad que se deben aplicar en la subred](sql-database-managed-instance-connectivity-architecture.md#network-requirements).

### <a name="vnet"></a>VNET
- La red virtual se puede implementar mediante el modelo con Resource Manager. No se admite el modelo clásico.
- Después de crear una instancia administrada, no se admite el traslado de la instancia administrada o la red virtual a otro grupo de recursos o suscripción.
- Algunos servicios, como App Service Environment, Logic Apps e Instancia administrada (que se usan para la replicación geográfica, la replicación transaccional, o a través de servidores vinculados) no pueden acceder a las instancias administradas de diferentes regiones si sus redes virtuales están conectadas mediante [emparejamiento global](../virtual-network/virtual-networks-faq.md#what-are-the-constraints-related-to-global-vnet-peering-and-load-balancers). Se puede conectar a estos recursos a través de ExpressRoute o de red virtual a red virtual a través de puertas de enlace de red virtuales.

## <a name="Changes"></a> Cambios de comportamiento

Las siguientes variables, funciones y vistas devuelven resultados diferentes:

- `SERVERPROPERTY('EngineEdition')` devuelve el valor 8. Esta propiedad identifica de forma única una instancia administrada. Consulte [SERVERPROPERTY](https://docs.microsoft.com/sql/t-sql/functions/serverproperty-transact-sql).
- `SERVERPROPERTY('InstanceName')` devuelve NULL, porque el concepto de instancia tal y como existe para SQL Server no se aplica a una instancia administrada. Consulte [SERVERPROPERTY('InstanceName')](https://docs.microsoft.com/sql/t-sql/functions/serverproperty-transact-sql).
- `@@SERVERNAME` devuelve el nombre DNS completo "conectable", por ejemplo, my-managed-instance.wcus17662feb9ce98.database.windows.net. Consulte [@@SERVERNAME](https://docs.microsoft.com/sql/t-sql/functions/servername-transact-sql). 
- `SYS.SERVERS` devuelve un nombre DNS completo "conectable", por ejemplo, `myinstance.domain.database.windows.net`, para las propiedades "name" y "data_source". Consulte [SYS.SERVERS](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-servers-transact-sql).
- `@@SERVICENAME` devuelve NULL, porque el concepto de servicio tal y como existe para SQL Server no se aplica a una instancia administrada. Consulte [@@SERVICENAME](https://docs.microsoft.com/sql/t-sql/functions/servicename-transact-sql).
- Se admite `SUSER_ID`. Devuelve NULL si el inicio de sesión de Azure AD no está en sys.syslogins. Consulte [SUSER_ID](https://docs.microsoft.com/sql/t-sql/functions/suser-id-transact-sql). 
- `SUSER_SID` no se admite. Se devuelven los datos incorrectos, lo cual es un problema temporal conocido. Consulte [SUSER_SID](https://docs.microsoft.com/sql/t-sql/functions/suser-sid-transact-sql). 

## <a name="Issues"></a>Problemas conocidos y limitaciones

### <a name="tempdb-size"></a>Tamaño de TEMPDB

El tamaño máximo del archivo `tempdb` no puede ser mayor de 24 GB por núcleo en un nivel De uso general. El tamaño máximo de `tempdb` en un nivel Crítico para la empresa está limitado por el tamaño de almacenamiento de instancia. El tamaño del archivo de registro `tempdb` está limitado a 120 GB en los niveles De uso general y Crítico para la empresa. La base de datos `tempdb` siempre se divide en 12 archivos de datos. No se puede cambiar este tamaño máximo por archivo y no se pueden agregar nuevos archivos a `tempdb`. Algunas consultas podrían devolver un error si necesitan más de 24 GB por núcleo en `tempdb` o si producen un registro superior a 120 GB. Siempre se vuelve a crear `tempdb` como base de datos vacía cuando se inicia la instancia o se realiza una conmutación por error. Cualquier cambio que se realice en `tempdb` no se conservará. 

### <a name="cant-restore-contained-database"></a>No se puede restaurar la base de datos independiente

Instancia administrada no puede restaurar [bases de datos independientes](https://docs.microsoft.com/sql/relational-databases/databases/contained-databases). La restauración a un momento dado de las bases de datos independientes existentes no funciona en Instancia administrada. Pronto se resolverá este problema. Mientras tanto, le recomendamos que quite la opción de independencia de las bases de datos colocadas en Instancia administrada. No use esta opción con bases de datos de producción. 

### <a name="exceeding-storage-space-with-small-database-files"></a>Exceder el espacio de almacenamiento con archivos de base de datos pequeños

Se puede producir un error en las instrucciones `CREATE DATABASE`, `ALTER DATABASE ADD FILE` y `RESTORE DATABASE` porque la instancia puede alcanzar el límite de almacenamiento de Azure.

Cada instancia administrada de uso general tiene hasta 35 TB de almacenamiento reservado para el espacio en disco Premium de Azure. Cada archivo de base de datos se coloca en un disco físico independiente. Los posibles tamaños de disco son: 128 GB, 256 GB, 512 GB, 1 TB o 4 TB. El espacio no utilizado en el disco no se cobra, pero la suma total de los tamaños de disco Premium de Azure no puede superar los 35 TB. En algunos casos, una instancia administrada que no necesita 8 TB en total puede superar los 35 TB de límite de Azure en tamaño de almacenamiento debido a la fragmentación interna.

Por ejemplo, una instancia administrada de uso general puede tener un archivo de 1,2 TB de tamaño situado en un disco de 4 TB. También podría tener 248 archivos cada uno de 1 GB de tamaño situados en discos independientes de 128 GB. En este ejemplo:

- El tamaño de almacenamiento total del disco es de 1 x 4 TB + 248 x 128 GB = 35 TB.
- El espacio total reservado para las bases de datos en la instancia es de 1 x 1,2 TB + 248 x 1 GB = 1,4 TB.

Este ejemplo ilustra que, en determinadas circunstancias, debido a una distribución muy específica de archivos, una instancia administrada podría alcanzar el límite de 35 TB que está reservado para el disco adjunto Premium de Azure cuando no se lo espere.

En este ejemplo, las bases de datos existentes seguirán funcionando y pueden crecer sin ningún problema, siempre y cuando no se agreguen nuevos archivos. No se podrían crear ni restaurar nuevas bases de datos porque no hay suficiente espacio para nuevas unidades de disco, incluso si el tamaño total de todas las bases de datos no alcanza el límite de tamaño de la instancia. El error que se devuelve en ese caso no está claro.

También puede [identificar el número de archivos restantes](https://medium.com/azure-sqldb-managed-instance/how-many-files-you-can-create-in-general-purpose-azure-sql-managed-instance-e1c7c32886c1) mediante el uso de vistas del sistema. Si se alcanza este límite, intente [vaciar y eliminar algunos de los archivos más pequeños mediante la instrucción DBCC SHRINKFILE](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-shrinkfile-transact-sql#d-emptying-a-file) o cambie al [nivel Crítico para la empresa, que no tiene este límite](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-resource-limits#service-tier-characteristics).

### <a name="incorrect-configuration-of-the-sas-key-during-database-restore"></a>Configuración incorrecta de la clave SAS durante la restauración de una base de datos

`RESTORE DATABASE`, que lee el archivo .bak, podría estar intentando leer constantemente el archivo .bak y devuelve un error después de un período largo de tiempo si la firma de acceso compartido de `CREDENTIAL` es incorrecta. Ejecute RESTORE HEADERONLY antes de restaurar una base de datos para asegurarse de que la clave SAS es correcta.
Asegúrese de quitar el signo `?` inicial de la clave SAS generada mediante Azure Portal.

### <a name="tooling"></a>Herramientas

SQL Server Management Studio y SQL Server Data Tools podrían tener algunos problemas al acceder a una instancia administrada.

- Actualmente no se admite el uso de entidades de seguridad (inicios de sesión) y usuarios(versión preliminar pública) del servidor de Azure AD con SQL Server Data Tools.
- No se admite la creación de scripts para entidades de seguridad (inicios de sesión) y usuarios (versión preliminar pública) del servidor de Azure AD en SQL Server Management Studio.

### <a name="incorrect-database-names-in-some-views-logs-and-messages"></a>Nombres incorrectos de base de datos en algunas vistas, registros y mensajes

Varias vistas del sistema, contadores de rendimiento, mensajes de error, XEvents y entradas de registro de errores muestran identificadores de base de datos GUID en lugar de los nombres reales de base de datos. No confíe en estos identificadores GUID porque se reemplazarán por los nombres reales de las bases de datos en el futuro.

### <a name="database-mail"></a>Correo electrónico de base de datos

El parámetro `@query` del procedimiento [sp_send_db_mail](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-send-dbmail-transact-sql) no funciona.

### <a name="database-mail-profile"></a>Perfil de Correo electrónico de base de datos

El perfil de Correo electrónico de base de datos que utiliza el Agente SQL Server debe llamarse `AzureManagedInstance_dbmail_profile`. No hay ninguna restricción para otros nombres de perfil de Correo electrónico de base de datos.

### <a name="error-logs-arent-persisted"></a>Los registros de errores no se conservan

Los registros de errores que están disponibles en Instancia administrada no se conservan y su tamaño no está incluido en el límite de almacenamiento máximo. Es posible que se borren automáticamente los registros de errores en caso de conmutación por error.

### <a name="error-logs-are-verbose"></a>Los registros de errores son detallados

Una instancia administrada coloca información detallada en los registros de errores y gran parte de esta no es pertinente. En el futuro se reducirá la cantidad de información de los registros de errores.

**Solución alternativa**: use un procedimiento personalizado para leer los registros de errores que filtran algunas entradas que no son pertinentes. Para más información, consulte [Instancia administrada: sp_readmierrorlog](https://blogs.msdn.microsoft.com/sqlcat/2018/05/04/azure-sql-db-managed-instance-sp_readmierrorlog/).

### <a name="transaction-scope-on-two-databases-within-the-same-instance-isnt-supported"></a>No se admite el ámbito de transacción en dos bases de datos de la misma instancia

La clase `TransactionScope` de .NET no funciona si dos consultas se envían a dos bases de datos de la misma instancia en el mismo ámbito de transacción:

```csharp
using (var scope = new TransactionScope())
{
    using (var conn1 = new SqlConnection("Server=quickstartbmi.neu15011648751ff.database.windows.net;Database=b;User ID=myuser;Password=mypassword;Encrypt=true"))
    {
        conn1.Open();
        SqlCommand cmd1 = conn1.CreateCommand();
        cmd1.CommandText = string.Format("insert into T1 values(1)");
        cmd1.ExecuteNonQuery();
    }

    using (var conn2 = new SqlConnection("Server=quickstartbmi.neu15011648751ff.database.windows.net;Database=b;User ID=myuser;Password=mypassword;Encrypt=true"))
    {
        conn2.Open();
        var cmd2 = conn2.CreateCommand();
        cmd2.CommandText = string.Format("insert into b.dbo.T2 values(2)");        cmd2.ExecuteNonQuery();
    }

    scope.Complete();
}

```

Aunque este código funciona con datos en la misma instancia, requería el coordinador de transacciones distribuidas.

**Solución alternativa**: use [SqlConnection.ChangeDatabase(String)](https://docs.microsoft.com/dotnet/api/system.data.sqlclient.sqlconnection.changedatabase) para utilizar otra base de datos en un contexto de conexión en lugar de usar dos conexiones.

### <a name="clr-modules-and-linked-servers-sometimes-cant-reference-a-local-ip-address"></a>Los módulos de CLR y los servidores vinculados en algún momento no pueden hacer referencia a una dirección IP local.

Los módulos de CLR colocados en una instancia administrada y las consultas distribuidas o servidores vinculados que hacen referencia a una instancia actual en algún momento no pueden resolver la dirección IP de una instancia local. Este error es un problema transitorio.

**Solución alternativa**: use conexiones de contexto en un módulo de CLR, si es posible.

### <a name="tde-encrypted-databases-with-a-service-managed-key-dont-support-user-initiated-backups"></a>Las bases de datos con cifrado TDE con una clave administrada por el servicio no son compatibles con las copias de seguridad iniciadas por el usuario

No se puede ejecutar `BACKUP DATABASE ... WITH COPY_ONLY` en una base de datos cifrada con Cifrado de datos transparente (TDE) administrado por el servicio. TDE administrado por un servicio hace que las copias de seguridad se cifren con una clave interna de TDE. No es posible exportar la clave, por lo que no se puede restaurar la copia de seguridad.

**Solución alternativa**: Use copias de seguridad automáticas y restauración a un momento dado, o use [Cifrado de datos transparente administrado por el cliente (BYOK)](https://docs.microsoft.com/azure/sql-database/transparent-data-encryption-azure-sql#customer-managed-transparent-data-encryption---bring-your-own-key) en su lugar. También puede deshabilitar el cifrado en la base de datos.

### <a name="point-in-time-restore-follows-time-by-the-time-zone-set-on-the-source-instance"></a>La restauración a un momento dado sigue la hora de la zona horaria establecida en la instancia de origen.

Actualmente, la restauración a un momento dado interpreta la hora a la que se debe realizar la restauración según la zona horaria de la instancia de origen, en lugar de seguir la hora UTC.
Consulte los [problemas conocidos de la zona horaria de la Instancia administrada](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-timezone#known-issues) para obtener más detalles.

## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre instancias administradas, consulte [¿Qué es una instancia administrada?](sql-database-managed-instance.md)
- Para obtener una lista de características y ver una comparativa, consulte [Comparación de características de Azure SQL Database](sql-database-features.md).
- Para consultar un inicio rápido que muestra cómo crear una nueva instancia administrada, consulte [Creación de una instancia administrada](sql-database-managed-instance-get-started.md).
