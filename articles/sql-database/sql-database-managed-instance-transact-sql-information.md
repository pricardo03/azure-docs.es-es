---
title: Diferencias de T-SQL para Instancia administrada
description: En este artículo se describen las diferencias de T-SQL entre una instancia administrada en Azure SQL Database y SQL Server.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein, carlrab, bonova, danil
ms.date: 02/10/2020
ms.custom: seoapril2019
ms.openlocfilehash: d3e631fae4899fffafad9bd140abaae4fb170624
ms.sourcegitcommit: 6ee876c800da7a14464d276cd726a49b504c45c5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2020
ms.locfileid: "77462588"
---
# <a name="managed-instance-t-sql-differences-limitations-and-known-issues"></a>Diferencias, limitaciones y problemas conocidos de T-SQL en la instancia administrada

Este artículo resume y explica las diferencias de sintaxis y comportamiento entre la instancia administrada de Azure SQL Database y el Motor de base de datos de SQL Server local. La opción de implementación de la Instancia administrada proporciona gran compatibilidad con instancias locales del Motor de base de datos de SQL Server. Instancia administrada admite la mayoría de las características del Motor de base de datos de SQL Server.

![Migración](./media/sql-database-managed-instance/migration.png)

Existen algunas limitaciones de PaaS que se introdujeron en Instancia administrada y algunos cambios de comportamiento en comparación con SQL Server. Las diferencias se dividen en las siguientes categorías: <a name="Differences"></a>

- [Disponibilidad](#availability), incluye las diferencias en [Grupos de disponibilidad AlwaysOn](#always-on-availability-groups) y [Copias de seguridad](#backup).
- [Seguridad](#security), incluye las diferencias en [Auditoría](#auditing), [Certificados](#certificates), [Credenciales](#credential), [Proveedores de servicios criptográficos](#cryptographic-providers), [Inicios de sesión y usuarios](#logins-and-users), [Clave maestra de servicio y clave de servicio](#service-key-and-service-master-key).
- [Configuración](#configuration), incluye las diferencias en [Extensión del grupo de búferes](#buffer-pool-extension), [Intercalación](#collation), [Niveles de compatibilidad](#compatibility-levels),[Creación de reflejo de la base de datos ](#database-mirroring), [Opciones de base de datos](#database-options), [Agente SQL Server](#sql-server-agent) y [Opciones de tabla](#tables).
- [Funcionalidades](#functionalities), incluidas [BULK INSERT/OPENROWSET](#bulk-insert--openrowset), [CLR](#clr), [DBCC](#dbcc), [Transacciones distribuidas](#distributed-transactions), [Eventos extendidos](#extended-events), [Bibliotecas externas](#external-libraries), [Filestream y Filetable](#filestream-and-filetable), [Búsqueda semántica de texto completo](#full-text-semantic-search), [Servidores vinculados](#linked-servers), [PolyBase](#polybase), [Replicación](#replication), [RESTORE](#restore-statement), [Service Broker](#service-broker), [Funciones, procedimientos almacenados y desencadenadores](#stored-procedures-functions-and-triggers).
- [Configuración del entorno](#Environment), como las configuraciones de redes virtuales y subredes.

La mayoría de estas características son restricciones de arquitectura y representan características de servicio.

En esta página también se explican los [Problemas temporales conocidos](#Issues) que se detectaron en la instancia administrada y se resolverán en el futuro.

## <a name="availability"></a>Disponibilidad

### <a name="always-on-availability-groups"></a>Grupos de disponibilidad AlwaysOn

La [alta disponibilidad](sql-database-high-availability.md) está integrada en la instancia administrada y no la pueden controlar los usuarios. No se admiten las siguientes instrucciones:

- [CREATE ENDPOINT … FOR DATABASE_MIRRORING](/sql/t-sql/statements/create-endpoint-transact-sql)
- [CREATE AVAILABILITY GROUP](/sql/t-sql/statements/create-availability-group-transact-sql)
- [ALTER AVAILABILITY GROUP](/sql/t-sql/statements/alter-availability-group-transact-sql)
- [DROP AVAILABILITY GROUP](/sql/t-sql/statements/drop-availability-group-transact-sql)
- La cláusula [SET HADR](/sql/t-sql/statements/alter-database-transact-sql-set-hadr) de la instrucción [ALTER DATABASE](/sql/t-sql/statements/alter-database-transact-sql)

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
- No se puede ejecutar `BACKUP DATABASE ... WITH COPY_ONLY` en una base de datos cifrada con Cifrado de datos transparente (TDE) administrado por el servicio. TDE administrado por un servicio hace que las copias de seguridad se cifren con una clave interna de TDE. No es posible exportar la clave, por lo que no se puede restaurar la copia de seguridad. Use copias de seguridad automáticas y restauración a un momento dado, o use [Cifrado de datos transparente administrado por el cliente (BYOK)](transparent-data-encryption-azure-sql.md#customer-managed-transparent-data-encryption---bring-your-own-key) en su lugar. También puede deshabilitar el cifrado en la base de datos.
- Las copias de seguridad manuales en el almacenamiento de blobs de Azure solo se admiten para [cuentas de BlockBlobStorage](/azure/storage/common/storage-account-overview#types-of-storage-accounts).
- El tamaño máximo de una franja de copia de seguridad con el uso del comando `BACKUP` en una instancia administrada es de 195 GB, lo cual es el tamaño máximo del blob. Aumente el número de franjas en el comando de copia de seguridad para reducir el tamaño de las franjas y permanecer dentro de este límite.

    > [!TIP]
    > Para solucionar esta limitación, cuando realice una copia de seguridad de una base de datos desde un servidor de SQL Server en un entorno local o en una máquina virtual, puede:
    >
    > - Realizar la copia de seguridad en `DISK` en lugar de hacerlo en `URL`.
    > - Cargar los archivos de copia de seguridad en Blob Storage.
    > - Realizar la restauración en la instancia administrada.
    >
    > El comando `Restore` de una instancia administrada admite tamaños de blob más grandes en los archivos de copia de seguridad porque se usa un tipo de blob distinto para el almacenamiento de los archivos de copia de seguridad cargados.

Para más información acerca de las copias de seguridad mediante T-SQL, consulte [BACKUP](/sql/t-sql/statements/backup-transact-sql).

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

- [CREATE SERVER AUDIT](/sql/t-sql/statements/create-server-audit-transact-sql) 
- [ALTER SERVER AUDIT](/sql/t-sql/statements/alter-server-audit-transact-sql)
- [Auditoría](/sql/relational-databases/security/auditing/sql-server-audit-database-engine)

### <a name="certificates"></a>Certificados

Una instancia administrada no puede acceder a los recursos compartidos de archivos ni a las carpetas de Windows, por lo que se aplican las siguientes restricciones:

- El archivo `CREATE FROM`/`BACKUP TO` no se admite para certificados.
- No se admite el certificado `CREATE`/`BACKUP` de `FILE`/`ASSEMBLY`. No se pueden usar archivos de clave privada. 

Consulte [CREATE CERTIFICATE](/sql/t-sql/statements/create-certificate-transact-sql) y [BACKUP CERTIFICATE](/sql/t-sql/statements/backup-certificate-transact-sql). 
 
**Solución alternativa**: En lugar de crear una copia de seguridad del certificado y restaurar la copia de seguridad, [obtenga el contenido binario del certificado y la clave privada, almacénelo como archivo .sql y cree a partir del binario](/sql/t-sql/functions/certencoded-transact-sql#b-copying-a-certificate-to-another-database):

```sql
CREATE CERTIFICATE  
   FROM BINARY = asn_encoded_certificate
WITH PRIVATE KEY (<private_key_options>)
```

### <a name="credential"></a>Credential:

Solo se admiten identidades de Azure Key Vault y `SHARED ACCESS SIGNATURE`. No se admiten usuarios de Windows.

Consulte [CREATE CREDENTIAL](/sql/t-sql/statements/create-credential-transact-sql) y [ALTER CREDENTIAL](/sql/t-sql/statements/alter-credential-transact-sql).

### <a name="cryptographic-providers"></a>Proveedores de servicios criptográficos

Una instancia administrada no puede acceder a archivos, por lo que no se pueden crear proveedores de servicios criptográficos:

- `CREATE CRYPTOGRAPHIC PROVIDER` no se admite. Consulte [CREATE CRYPTOGRAPHIC PROVIDER](/sql/t-sql/statements/create-cryptographic-provider-transact-sql).
- `ALTER CRYPTOGRAPHIC PROVIDER` no se admite. Consulte [ALTER CRYPTOGRAPHIC PROVIDER](/sql/t-sql/statements/alter-cryptographic-provider-transact-sql).

### <a name="logins-and-users"></a>Inicios de sesión y usuarios

- Se admiten los inicios de sesión de SQL creados con `FROM CERTIFICATE`, `FROM ASYMMETRIC KEY` y `FROM SID`. Consulte [CREATE LOGIN](/sql/t-sql/statements/create-login-transact-sql).
- Se admiten las entidades de seguridad (inicios de sesión) del servidor de Azure Active Directory (Azure AD) creadas con la sintaxis [CREATE LOGIN](/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current) o la sintaxis [CREATE USER FROM LOGIN [Azure AD Login]](/sql/t-sql/statements/create-user-transact-sql?view=azuresqldb-mi-current). Estos inicios de sesión se crean en el nivel de servidor.

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

- Se admite la exportación e importación de bases de datos con archivos bacpac para usuarios de Azure AD en una instancia administrada mediante [SSMS V18.4 o una versión posterior](/sql/ssms/download-sql-server-management-studio-ssms) o [SQLPackage.exe](/sql/tools/sqlpackage-download).
  - Se admiten las siguientes configuraciones mediante el uso del archivo bacpac de la base de datos: 
    - Exportar o importar una base de datos entre diferentes instancias administradas en el mismo dominio de Azure AD.
    - Exportar una base de datos desde una instancia administrada e importarla a SQL Database en el mismo dominio de Azure AD. 
    - Exportar una base de datos desde SQL Database e importarla a una instancia administrada en el mismo dominio de Azure AD.
    - Exportar una base de datos desde una instancia administrada e importarla a SQL Server (versión 2012 o posterior).
      - En esta configuración, todos los usuarios de Azure AD se crean como entidades de seguridad de SQL Database (usuarios) sin inicios de sesión. El tipo de usuario se muestra como SQL (visible como SQL_USER en sys.database_principals). Sus permisos y roles se conservan en los metadatos de la base de datos de SQL Server y se pueden usar para la suplantación. Sin embargo, no se pueden usar para obtener acceso e iniciar sesión en SQL Server con sus credenciales.

- Solo el inicio de sesión de la entidad de seguridad a nivel de servidor (el que crea el proceso de aprovisionamiento de la instancia administrada), los miembros de los roles de servidor como `securityadmin` o `sysadmin`, u otros inicios de sesión con permiso ALTER ANY LOGIN en el nivel de servidor pueden crear entidades de seguridad (inicios de sesión) de servidor de Azure AD en la base de datos maestra para la instancia administrada.
- Si el inicio de sesión es una entidad de seguridad de SQL, solo los inicios de sesión que forman parte del rol `sysadmin` pueden utilizar el comando create para crear inicios de sesión para una cuenta de Azure AD.
- El inicio de sesión de Azure AD debe ser miembro de una instancia de Azure AD dentro del mismo directorio que se usa para la instancia administrada de Azure SQL Database.
- Las entidades de seguridad (inicio de sesión) del servidor de Azure AD son visibles en el explorador de objetos a partir de la versión preliminar 5 de SQL Server Management Studio 18.0.
- Se permite la superposición de las entidades de seguridad (inicios de sesión) del servidor de Azure AD con una cuenta de administrador de Azure AD. Las entidades de seguridad (inicios de sesión) del servidor de Azure AD tienen prioridad sobre el administrador de Azure AD cuando se resuelve la entidad de seguridad y se aplican permisos a la instancia administrada.
- Durante la autenticación, se aplica la siguiente secuencia para resolver la entidad de seguridad de autenticación:

    1. Si la cuenta de Azure AD existe como directamente asignada a la entidad de seguridad (inicio de sesión) del servidor de Azure AD que está presente en sys.server_principals como tipo "E", conceda acceso y aplique los permisos de la entidad de seguridad (inicio de sesión) del servidor de Azure AD.
    2. Si la cuenta de Azure AD es un miembro de un grupo de Azure AD que está asignado a la entidad de seguridad (inicio de sesión) del servidor de Azure AD (presente en sys.server_principals como tipo "X"), conceda acceso y aplique los permisos del inicio de sesión del grupo de Azure AD.
    3. Si la cuenta de Azure AD es un administrador de Azure AD configurado en un portal especial para la instancia administrada, que no existe en las vistas de sistema de la instancia administrada, aplique permisos fijos especiales del administrador de Azure AD para la instancia administrada (modo heredado).
    4. Si la cuenta de Azure AD existe como directamente asignada a un usuario de Azure AD de una base de datos, presente en sys.database_principals como tipo "E", conceda acceso y aplique los permisos del usuario de la base de datos de Azure AD.
    5. Si la cuenta de Azure AD es un miembro de un grupo de Azure AD que está asignado a un usuario de Azure AD de una base de datos, presente en sys.database_principals como tipo "X", conceda acceso y aplique los permisos del inicio de sesión del grupo de Azure AD.
    6. Si hay un inicio de sesión de Azure AD asignado a una cuenta de usuario de Azure AD o a una cuenta de grupo de Azure AD, la cual resuelve la autenticación del usuario, se aplicarán todos los permisos de este inicio de sesión de Azure AD.

### <a name="service-key-and-service-master-key"></a>Clave maestra de servicio y clave de servicio

- No se admiten las [copias de seguridad de la clave maestra](/sql/t-sql/statements/backup-master-key-transact-sql) (administrada por el servicio de SQL Database).
- No se admite la [restauración de la clave maestra](/sql/t-sql/statements/restore-master-key-transact-sql) (administrada por el servicio de SQL Database).
- No se admiten las [copias de seguridad de la clave maestra del servicio](/sql/t-sql/statements/backup-service-master-key-transact-sql) (administrada por el servicio de SQL Database).
- No se admite la [restauración de la clave maestra del servicio](/sql/t-sql/statements/restore-service-master-key-transact-sql) (administrada por el servicio de SQL Database).

## <a name="configuration"></a>Configuración

### <a name="buffer-pool-extension"></a>Extensión del grupo de búferes

- No se admite la [extensión del grupo de búferes](/sql/database-engine/configure-windows/buffer-pool-extension).
- `ALTER SERVER CONFIGURATION SET BUFFER POOL EXTENSION` no se admite. Consulte [ALTER SERVER CONFIGURATION](/sql/t-sql/statements/alter-server-configuration-transact-sql).

### <a name="collation"></a>Intercalación

La intercalación de la instancia predeterminada es `SQL_Latin1_General_CP1_CI_AS` y puede especificarse como un parámetro de creación. Consulte [Intercalaciones](/sql/t-sql/statements/collations).

### <a name="compatibility-levels"></a>Niveles de compatibilidad

- Los niveles de compatibilidad admitidos son 100, 110, 120, 130, 140 y 150.
- No se admiten los niveles de compatibilidad menores que 100.
- El nivel de compatibilidad predeterminado es 140 para las bases de datos nuevas. Para las bases de datos restauradas, el nivel de compatibilidad no cambiará si era 100 o superior.

Consulte [Nivel de compatibilidad de ALTER DATABASE](/sql/t-sql/statements/alter-database-transact-sql-compatibility-level).

### <a name="database-mirroring"></a>Creación de reflejo de la base de datos

No se admite la creación de reflejo de la base de datos.

- Las opciones `ALTER DATABASE SET PARTNER` y `SET WITNESS` no se admiten.
- `CREATE ENDPOINT … FOR DATABASE_MIRRORING` no se admite.

Para más información, consulte [ALTER DATABASE SET PARTNER y SET WITNESS](/sql/t-sql/statements/alter-database-transact-sql-database-mirroring) y [CREATE ENDPOINT … FOR DATABASE_MIRRORING](/sql/t-sql/statements/create-endpoint-transact-sql).

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

Para más información, consulte [CREATE DATABASE](/sql/t-sql/statements/create-database-sql-server-transact-sql).

#### <a name="alter-database-statement"></a>instrucción ALTER DATABASE

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

Para más información, consulte [ALTER DATABASE](/sql/t-sql/statements/alter-database-transact-sql-file-and-filegroup-options).

### <a name="sql-server-agent"></a>Agente SQL Server

- La habilitación o deshabilitación del Agente SQL Server no se admite actualmente en la instancia administrada. El Agente SQL se ejecuta de forma continua.
- La configuración del Agente SQL Server es de solo lectura. El procedimiento `sp_set_agent_properties` no se admite en la instancia administrada. 
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

Para más información acerca del Agente SQL Server, consulte [Agente SQL Server](/sql/ssms/agent/sql-server-agent).

### <a name="tables"></a>Tablas

No se admiten los siguientes tipos de tablas:

- [FILESTREAM](/sql/relational-databases/blob/filestream-sql-server)
- [FILETABLE](/sql/relational-databases/blob/filetables-sql-server)
- [EXTERNAL TABLE](/sql/t-sql/statements/create-external-table-transact-sql) (Polybase)
- [MEMORY_OPTIMIZED](/sql/relational-databases/in-memory-oltp/introduction-to-memory-optimized-tables) (únicamente no se admite en el nivel Uso general)

Para más información sobre cómo crear y modificar tablas, consulte [CREATE TABLE](/sql/t-sql/statements/create-table-transact-sql) y [ALTER TABLE](/sql/t-sql/statements/alter-table-transact-sql).

## <a name="functionalities"></a>Funcionalidades

### <a name="bulk-insert--openrowset"></a>BULK INSERT/OPENROWSET

Una instancia administrada no puede acceder a los recursos compartidos de archivos ni carpetas de Windows, por lo que los archivos se deben importar desde Azure Blob Storage:

- `DATASOURCE` es necesario en el comando `BULK INSERT` durante la importación de archivos desde Azure Blob Storage. Consulte [BULK INSERT](/sql/t-sql/statements/bulk-insert-transact-sql).
- `DATASOURCE` se necesita en la función `OPENROWSET` cuando se lee el contenido de un archivo desde Azure Blob Storage. Consulte [OPENROWSET](/sql/t-sql/functions/openrowset-transact-sql).
- `OPENROWSET` se puede usar para leer datos de otras bases de datos únicas de Azure SQL, instancias administradas o instancias de SQL Server. No se admiten otros orígenes, como bases de datos de Oracle o archivos de Excel.

### <a name="clr"></a>CLR

Una instancia administrada no puede acceder a los recursos compartidos de archivos ni a las carpetas de Windows, por lo que se aplican las siguientes restricciones:

- Solo se admite `CREATE ASSEMBLY FROM BINARY`. Consulte [CREATE ASSEMBLY FROM BINARY](/sql/t-sql/statements/create-assembly-transact-sql). 
- `CREATE ASSEMBLY FROM FILE` no se admite. Consulte [CREATE ASSEMBLY FROM FILE](/sql/t-sql/statements/create-assembly-transact-sql).
- `ALTER ASSEMBLY` no puede hacer referencia a archivos. Consulte [ALTER ASSEMBLY](/sql/t-sql/statements/alter-assembly-transact-sql).

### <a name="database-mail-db_mail"></a>Correo electrónico de base de datos: (db_mail)
 - `sp_send_dbmail` no puede enviar datos adjuntos con el parámetro @file_attachments. El sistema de archivos local y los recursos compartidos externos, o Azure Blob Storage, no son accesibles con este procedimiento.
 - Vea los problemas conocidos relacionados con el parámetro `@query` y la autenticación.
 
### <a name="dbcc"></a>DBCC

Las instancias administradas no admiten instrucciones DBCC no documentadas que estén habilitadas en SQL Server.

- Solo se admite un número limitado de marcas de seguimiento globales. No se admiten las `Trace flags` en el nivel de sesión. Consulte [Marcas de seguimiento](/sql/t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql).
- [DBCC TRACEOFF](/sql/t-sql/database-console-commands/dbcc-traceoff-transact-sql) y [DBCC TRACEON](/sql/t-sql/database-console-commands/dbcc-traceon-transact-sql) funcionan con el número limitado de marcas trace-flags globales.
- No se puede usar [DBCC CHECKDB](/sql/t-sql/database-console-commands/dbcc-checkdb-transact-sql) con las opciones REPAIR_ALLOW_DATA_LOSS, REPAIR_FAST y REPAIR_REBUILD porque la base de datos no se puede establecer en el modo `SINGLE_USER`. Consulte las [diferencias de ALTER DATABASE](#alter-database-statement). El equipo de soporte técnico de Azure se encarga de los posibles daños en la base de datos. Póngase en contacto con el soporte técnico de Azure si observa daños en la base de datos que se deben corregir.

### <a name="distributed-transactions"></a>Distributed transactions

Actualmente no se admiten MSDTC ni las [Transacciones elásticas](sql-database-elastic-transactions-overview.md) en las instancias administradas.

### <a name="extended-events"></a>Eventos extendidos

No se admiten algunos destinos específicos de Windows para Extended Events (XEvents):

- No se admite el destino `etw_classic_sync`. Almacene los archivos `.xel` en Azure Blob Storage. Consulte [Destino etw_classic_sync](/sql/relational-databases/extended-events/targets-for-extended-events-in-sql-server#etw_classic_sync_target-target).
- No se admite el destino `event_file`. Almacene los archivos `.xel` en Azure Blob Storage. Consulte [Destino event_file](/sql/relational-databases/extended-events/targets-for-extended-events-in-sql-server#event_file-target).

### <a name="external-libraries"></a>Bibliotecas externas

Aún no se admiten las bibliotecas externas de R y Python para análisis en base de datos. Consulte [Machine Learning Services en SQL Server](/sql/advanced-analytics/r/sql-server-r-services).

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

Para más información, consulte [FILESTREAM](/sql/relational-databases/blob/filestream-sql-server) y [FileTables](/sql/relational-databases/blob/filetables-sql-server).

### <a name="full-text-semantic-search"></a>Búsqueda semántica de texto completo

No se admite la [búsqueda semántica](/sql/relational-databases/search/semantic-search-sql-server).

### <a name="linked-servers"></a>Servidores vinculados

Los servidores vinculados en instancias administradas admiten un número limitado de destinos:

- Los destinos admitidos son las instancias administradas, las bases de datos únicas y las instancias de SQL Server. 
- Los servidores vinculados no admiten transacciones de escritura distribuidas (MS DTC).
- Destinos no admitidos: archivos, Analysis Services y otros RDBMS. Intente usar la importación de CSV nativa desde Azure Blob Storage mediante `BULK INSERT` o `OPENROWSET` como alternativa para la importación de archivos.

Operaciones

- No se admiten las transacciones de escritura entre instancias.
- Se admite `sp_dropserver` para quitar un servidor vinculado. Consulte [sp_dropserver](/sql/relational-databases/system-stored-procedures/sp-dropserver-transact-sql).
- La función `OPENROWSET` se puede usar para ejecutar consultas solo en instancias de SQL Server. Se pueden administrar de forma local o en máquinas virtuales. Consulte [OPENROWSET](/sql/t-sql/functions/openrowset-transact-sql).
- La función `OPENDATASOURCE` se puede usar para ejecutar consultas solo en instancias de SQL Server. Se pueden administrar de forma local o en máquinas virtuales. Solo se admiten los valores `SQLNCLI`, `SQLNCLI11` y `SQLOLEDB` como proveedor. Un ejemplo es `SELECT * FROM OPENDATASOURCE('SQLNCLI', '...').AdventureWorks2012.HumanResources.Employee`. Consulte [OPENDATASOURCE](/sql/t-sql/functions/opendatasource-transact-sql).
- Los servidores vinculados no se pueden usar para leer archivos (Excel y CSV) de los recursos compartidos de red. Intente usar [BULK INSERT](/sql/t-sql/statements/bulk-insert-transact-sql#e-importing-data-from-a-csv-file) o [OPENROWSET](/sql/t-sql/functions/openrowset-transact-sql#g-accessing-data-from-a-csv-file-with-a-format-file) que lee archivos CSV desde Azure Blob Storage. Realice un seguimiento de estas solicitudes en el [elemento de comentarios de la instancia administrada](https://feedback.azure.com/forums/915676-sql-managed-instance/suggestions/35657887-linked-server-to-non-sql-sources)|.

### <a name="polybase"></a>PolyBase

No se admiten tablas externas que hacen referencia a archivos en HDFS o Azure Blob Storage. Para más información acerca de Polybase, consulte [Polybase](/sql/relational-databases/polybase/polybase-guide).

### <a name="replication"></a>Replicación

- Se admiten los tipos de replicación de instantánea y bidireccional. No se admite la replicación de mezcla, la replicación punto a punto y las suscripciones actualizables.
- La [replicación transaccional](sql-database-managed-instance-transactional-replication.md) está disponible para la versión preliminar pública en la instancia administrada con algunas restricciones:
    - Todos los tipos de participantes de la replicación (editor, distribuidor, suscriptor de extracción y suscriptor de inserción) se pueden colocar en instancias administradas, pero el editor y el distribuidor deben estar tanto en la nube como en el entorno local.
    - Las instancias administradas pueden comunicarse con las versiones recientes de SQL Server. Para obtener más información, consulte la [matriz de versiones compatibles](sql-database-managed-instance-transactional-replication.md#supportability-matrix-for-instance-databases-and-on-premises-systems).
    - La replicación transaccional tiene algunos [requisitos de red adicionales](sql-database-managed-instance-transactional-replication.md#requirements).

Para obtener más información sobre la configuración de la replicación transaccional, vea los siguientes tutoriales:
- [Replicación entre un suscriptor y un publicador de MI](replication-with-sql-database-managed-instance.md)
- [Replicación entre el publicador de MI, el distribuidor de MI y el suscriptor de SQL Server](sql-database-managed-instance-configure-replication-tutorial.md)

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
- `ASYNC RESTORE`: la restauración continúa aunque se interrumpa la conexión con el cliente. Si la conexión se interrumpe, puede usar `sys.dm_operation_status` para ver el estado de una operación de restauración y para crear y eliminar una base de datos. Consulte [sys.dm_operation_status](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database). 

Las siguientes opciones de base de datos se establecen o invalidan, y no se pueden cambiar más adelante: 

- `NEW_BROKER` si el agente no está habilitado en el archivo .bak. 
- `ENABLE_BROKER` si el agente no está habilitado en el archivo .bak. 
- `AUTO_CLOSE=OFF` si una base de datos en el archivo .bak tiene `AUTO_CLOSE=ON`. 
- `RECOVERY FULL` si una base de datos en el archivo .bak tiene los modos de recuperación `SIMPLE` o `BULK_LOGGED`.
- Se agrega un grupo de archivos optimizado para memoria y se le asigna el nombre XTP si aún no se encuentra en el archivo .bak de origen. 
- Se cambia el nombre de todos los grupos de archivos optimizados para memoria existentes a XTP. 
- Las opciones `SINGLE_USER` y `RESTRICTED_USER` se convierten en `MULTI_USER`.

Limitaciones: 

- Las copias de seguridad de las bases de datos dañadas pueden restaurarse en función del tipo de daños, pero no se realizarán copias de seguridad automatizadas mientras no se corrija el daño. Asegúrese de ejecutar `DBCC CHECKDB` en la instancia de origen y de usar la copia de seguridad `WITH CHECKSUM` para evitar este problema.
- La restauración de un archivo `.BAK` de una base de datos que contenga alguna de las limitaciones descritas en este documento (por ejemplo, objetos `FILESTREAM` o `FILETABLE`) no se puede llevar a cabo en la instancia administrada.
- Los archivos `.BAK` que contienen varios conjuntos de copia de seguridad no se pueden restaurar. 
- Los archivos `.BAK` que contienen varios archivos de registro no se pueden restaurar.
- Las copias de seguridad que contienen bases de datos con un tamaño superior a 8 TB, objetos OLTP en memoria activos o una cantidad de archivos superior a 280 por instancia no se pueden restaurar en una instancia de Uso general. 
- Las copias de seguridad que contienen bases de datos con un tamaño superior a 4 TB u objetos OLTP en memoria con un tamaño total superior al descrito en los [límites de recursos](sql-database-managed-instance-resource-limits.md) no se pueden restaurar en una instancia de tipo Crítico para la empresa.
Para más información acerca de las instrucciones Restore, consulte [Instrucciones RESTORE](/sql/t-sql/statements/restore-statements-transact-sql).

 > [!IMPORTANT]
 > Las mismas limitaciones se aplican a una operación de restauración a un momento dado integrada. Por ejemplo, no es posible restaurar una base de datos de Uso general de más de 4 TB en una instancia de tipo Crítico para la empresa. Una base de datos de tipo Crítico para la empresa con archivos OLTP en memoria o más de 280 archivos no se puede restaurar en la instancia de Uso general.

### <a name="service-broker"></a>Service Broker

No se admite el agente de servicio entre instancias:

- `sys.routes`: Como requisito previo, debe seleccionar la dirección de sys.routes. La dirección debe ser LOCAL en todas las rutas. Consulte [sys.routes](/sql/relational-databases/system-catalog-views/sys-routes-transact-sql).
- `CREATE ROUTE`: no se puede usar `CREATE ROUTE` con un valor de `ADDRESS` distinto de `LOCAL`. Consulte [CREATE ROUTE](/sql/t-sql/statements/create-route-transact-sql).
- `ALTER ROUTE`: no se puede usar `ALTER ROUTE` con un valor de `ADDRESS` distinto de `LOCAL`. Consulte [ALTER ROUTE](/sql/t-sql/statements/alter-route-transact-sql). 

### <a name="stored-procedures-functions-and-triggers"></a>Procedimientos almacenados, funciones y desencadenadores

- `NATIVE_COMPILATION` no se admite en el nivel De uso general.
- No se admiten las siguientes opciones de [sp_configure](/sql/relational-databases/system-stored-procedures/sp-configure-transact-sql): 
  - `allow polybase export`
  - `allow updates`
  - `filestream_access_level`
  - `remote data archive`
  - `remote proc trans`
- `sp_execute_external_scripts` no se admite. Consulte [sp_execute_external_scripts](/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql#examples).
- `xp_cmdshell` no se admite. Consulte [xp_cmdshell](/sql/relational-databases/system-stored-procedures/xp-cmdshell-transact-sql).
- No se admiten `Extended stored procedures`, lo cual incluye `sp_addextendedproc` y `sp_dropextendedproc`. Consulte [Procedimientos almacenados extendidos](/sql/relational-databases/system-stored-procedures/general-extended-stored-procedures-transact-sql).
- No se admiten `sp_attach_db`, `sp_attach_single_file_db`, y `sp_detach_db`. Consulte [sp_attach_db](/sql/relational-databases/system-stored-procedures/sp-attach-db-transact-sql), [sp_attach_single_file_db](/sql/relational-databases/system-stored-procedures/sp-attach-single-file-db-transact-sql), y [sp_detach_db](/sql/relational-databases/system-stored-procedures/sp-detach-db-transact-sql).

### <a name="system-functions-and-variables"></a>Variables y funciones del sistema

Las siguientes variables, funciones y vistas devuelven resultados diferentes:

- `SERVERPROPERTY('EngineEdition')` devuelve el valor 8. Esta propiedad identifica de forma única una instancia administrada. Consulte [SERVERPROPERTY](/sql/t-sql/functions/serverproperty-transact-sql).
- `SERVERPROPERTY('InstanceName')` devuelve NULL, porque el concepto de instancia tal y como existe para SQL Server no se aplica a una instancia administrada. Consulte [SERVERPROPERTY('InstanceName')](/sql/t-sql/functions/serverproperty-transact-sql).
- `@@SERVERNAME` devuelve el nombre DNS completo "conectable", por ejemplo, my-managed-instance.wcus17662feb9ce98.database.windows.net. Consulte [@@SERVERNAME](/sql/t-sql/functions/servername-transact-sql). 
- `SYS.SERVERS` devuelve un nombre DNS completo "conectable", por ejemplo, `myinstance.domain.database.windows.net`, para las propiedades "name" y "data_source". Consulte [SYS.SERVERS](/sql/relational-databases/system-catalog-views/sys-servers-transact-sql).
- `@@SERVICENAME` devuelve NULL, porque el concepto de servicio tal y como existe para SQL Server no se aplica a una instancia administrada. Consulte [@@SERVICENAME](/sql/t-sql/functions/servicename-transact-sql).
- `SUSER_ID` es compatible. Devuelve NULL si el inicio de sesión de Azure AD no está en sys.syslogins. Consulte [SUSER_ID](/sql/t-sql/functions/suser-id-transact-sql). 
- `SUSER_SID` no se admite. Se devuelven los datos incorrectos, lo cual es un problema temporal conocido. Consulte [SUSER_SID](/sql/t-sql/functions/suser-sid-transact-sql). 

## <a name="Environment"></a>Restricciones del entorno

### <a name="subnet"></a>Subnet
-  No se puede colocar ningún otro recurso (por ejemplo, máquinas virtuales) en la subred en la que ha implementado la instancia administrada. Implemente estos recursos con una subred diferente.
- La subred debe tener un número de [direcciones IP](sql-database-managed-instance-connectivity-architecture.md#network-requirements) disponibles suficiente. El mínimo es 16, aunque se recomienda tener al menos 32 direcciones IP en la subred.
- [No se pueden asociar los puntos de conexión de servicio con la subred de la instancia administrada](sql-database-managed-instance-connectivity-architecture.md#network-requirements). Asegúrese de que la opción de puntos de conexión de servicio esté deshabilitada al crear la red virtual.
- El número de núcleos virtuales y tipos de instancias que se pueden implementar en una región tiene algunas [restricciones y límites](sql-database-managed-instance-resource-limits.md#regional-resource-limitations).
- Hay algunas [reglas de seguridad que se deben aplicar en la subred](sql-database-managed-instance-connectivity-architecture.md#network-requirements).

### <a name="vnet"></a>VNET
- La red virtual se puede implementar mediante el modelo con Resource Manager. No se admite el modelo clásico.
- Después de crear una instancia administrada, no se admite el traslado de la instancia administrada o la red virtual a otro grupo de recursos o suscripción.
- Algunos servicios, como App Service Environment, Logic Apps y las instancias administradas (que se usan para la replicación geográfica, la replicación transaccional o a través de servidores vinculados), no pueden acceder a las instancias administradas de diferentes regiones si sus redes virtuales están conectadas mediante [emparejamiento global](../virtual-network/virtual-networks-faq.md#what-are-the-constraints-related-to-global-vnet-peering-and-load-balancers). Se puede conectar a estos recursos a través de ExpressRoute o de red virtual a red virtual a través de puertas de enlace de red virtuales.

### <a name="tempdb"></a>TEMPDB

El tamaño máximo del archivo `tempdb` no puede ser mayor de 24 GB por núcleo en un nivel De uso general. El tamaño máximo de `tempdb` en un nivel Crítico para la empresa está limitado por el tamaño de almacenamiento de instancia. El tamaño del archivo de registro `Tempdb` está limitado a 120 GB en el nivel de uso general. Algunas consultas podrían devolver un error si necesitan más de 24 GB por núcleo en `tempdb` o si producen datos de registro superiores a 120 GB.

### <a name="msdb"></a>MSDB

Los siguientes esquemas MSDB de la instancia administrada deben ser propiedad de sus respectivos roles predefinidos:

- Roles generales
  - TargetServersRole
- [Roles fijos de base de datos](https://docs.microsoft.com/sql/ssms/agent/sql-server-agent-fixed-database-roles?view=sql-server-ver15)
  - SQLAgentUserRole
  - SQLAgentReaderRole
  - SQLAgentOperatorRole
- [Roles de DatabaseMail](https://docs.microsoft.com/sql/relational-databases/database-mail/database-mail-configuration-objects?view=sql-server-ver15#DBProfile):
  - DatabaseMailUserRole
- [Roles de Integration Services](https://docs.microsoft.com/sql/integration-services/security/integration-services-roles-ssis-service?view=sql-server-ver15):
  - db_ssisadmin
  - db_ssisltduser
  - db_ssisoperator
  
> [!IMPORTANT]
> El cambio de los nombres de roles predefinidos, los nombres de esquema y los propietarios de esquemas por parte de los clientes afectará al funcionamiento normal del servicio. Los cambios que se realicen en estos se revertirán a los valores predefinidos en cuanto se detecten, o bien en la siguiente actualización del servicio en la última para garantizar el funcionamiento normal del servicio.

### <a name="error-logs"></a>Registros de error

Una instancia administrada coloca información detallada en los registros de errores. Existen muchos eventos internos del sistema que se archivan en el registro de errores. use un procedimiento personalizado para leer los registros de errores que filtran algunas entradas que no son pertinentes. Para obtener más información, vea [instancia administrada – sp_readmierrorlog](https://blogs.msdn.microsoft.com/sqlcat/2018/05/04/azure-sql-db-managed-instance-sp_readmierrorlog/) o [Extensión de instancia administrada (versión preliminar)](/sql/azure-data-studio/azure-sql-managed-instance-extension#logs) para Azure Data Studio.

## <a name="Issues"></a> Problemas conocidos


### <a name="limitation-of-manual-failover-via-portal-for-failover-groups"></a>Limitación de la conmutación por error manual a través del portal para grupos de conmutación por error

**Fecha:** Enero de 2020

Si el grupo de conmutación por error abarca instancias de distintas suscripciones o grupos de recursos de Azure, la conmutación por error manual no se puede iniciar desde la instancia principal del grupo de conmutación por error.

**Solución alternativa**: Inicie la conmutación por error mediante el portal desde la base de datos geográfica secundaria.

### <a name="sql-agent-roles-need-explicit-execute-permissions-for-non-sysadmin-logins"></a>Los roles del Agente SQL necesitan permisos de ejecución (EXECUTE) explícitos para los inicios de sesión que no sean sysadmin

**Fecha:** Diciembre de 2019

Si los inicios de sesión que no son de sysadmin se agregan a cualquiera de los [roles fijos de base de datos del Agente SQL](https://docs.microsoft.com/sql/ssms/agent/sql-server-agent-fixed-database-roles), existe un problema en el que los permisos EXECUTE explícitos deben concederse a los procedimientos almacenados maestros para que estos inicios de sesión funcionen. Si se encuentra este problema, aparece el mensaje de error "Se denegó el permiso EXECUTE en el objeto <nombre_objeto> (Microsoft SQL Server, error: 229)".

**Solución alternativa**: Una vez que agregue inicios de sesión a cualquiera de los roles fijos de base de datos del Agente SQL: SQLAgentUserRole, SQLAgentReaderRole o SQLAgentOperatorRole, para cada uno de los inicios de sesión agregados a estos roles, ejecute el siguiente script T-SQL para conceder explícitamente permisos de ejecución a los procedimientos almacenados enumerados.

```tsql
USE [master]
GO
CREATE USER [login_name] FOR LOGIN [login_name]
GO
GRANT EXECUTE ON master.dbo.xp_sqlagent_enum_jobs TO [login_name]
GRANT EXECUTE ON master.dbo.xp_sqlagent_is_starting TO [login_name]
GRANT EXECUTE ON master.dbo.xp_sqlagent_notify TO [login_name]
```

### <a name="sql-agent-jobs-can-be-interrupted-by-agent-process-restart"></a>Los trabajos del Agente SQL pueden ser interrumpidos por el reinicio del proceso del agente

**Fecha:** Diciembre de 2019

El Agente SQL crea una sesión cada vez que se inicia el trabajo, lo que aumenta gradualmente el consumo de memoria. Para evitar alcanzar el límite de memoria interna que bloquearía la ejecución de los trabajos programados, el proceso del agente se reiniciará una vez que el consumo de memoria alcance el umbral. Puede provocar interrumpir la ejecución de los trabajos que se ejecutan en el momento del reinicio.

### <a name="in-memory-oltp-memory-limits-are-not-applied"></a>No se aplican los límites de memoria de OLTP en memoria

**Fecha:** Octubre de 2019

El nivel de servicio Crítico para la empresa no aplicará correctamente los [límites de memoria máximos para los objetos optimizados para memoria](sql-database-managed-instance-resource-limits.md#in-memory-oltp-available-space) en algunos casos. La instancia administrada puede permitir que la carga de trabajo use más memoria para las operaciones de OLTP en memoria, lo que puede afectar a la disponibilidad y la estabilidad de la instancia. Es posible que las consultas de OLTP en memoria que alcanzan los límites no generen errores de inmediato. Pronto se resolverá este problema. Las consultas que usan en mayor grado OLTP en memoria producirán un error antes si llegan a los [límites](sql-database-managed-instance-resource-limits.md#in-memory-oltp-available-space).

**Solución alternativa:** [Supervise el uso de almacenamiento de OLTP en memoria ](https://docs.microsoft.com/azure/sql-database/sql-database-in-memory-oltp-monitoring) con [SQL Server Management Studio](/sql/relational-databases/in-memory-oltp/monitor-and-troubleshoot-memory-usage#bkmk_Monitoring) para asegurarse de que la carga de trabajo no esté usando más memoria de la disponible. Aumente los límites de memoria que dependen del número de núcleos virtuales, u optimice la carga de trabajo para usar menos memoria.

### <a name="wrong-error-returned-while-trying-to-remove-a-file-that-is-not-empty"></a>Se mostró un error al intentar quitar un archivo que no está vacío

**Fecha:** Octubre de 2019

SQL Server/Instancia administrada [no permiten al usuario quitar un archivo que no está vacío](/sql/relational-databases/databases/delete-data-or-log-files-from-a-database#Prerequisites). Si intenta quitar un archivo de datos no vacío mediante la instrucción `ALTER DATABASE REMOVE FILE`, el error `Msg 5042 – The file '<file_name>' cannot be removed because it is not empty` no se mostrará inmediatamente. Instancia administrada seguirá intentando quitar el archivo y se producirá un error en la operación después de 30 minutos con `Internal server error`.

**Solución alternativa**: Quite el contenido del archivo mediante el comando `DBCC SHRINKFILE (N'<file_name>', EMPTYFILE)`. Si este es el único archivo del grupo de archivos, debe eliminar los datos de la tabla o partición asociada a este grupo de archivos antes de reducir el archivo y, opcionalmente, cargar estos datos en otra tabla o partición.

### <a name="change-service-tier-and-create-instance-operations-are-blocked-by-ongoing-database-restore"></a>Las operaciones de cambio de nivel de servicio y creación de instancia se bloquean con la restauración en curso de la base de datos

**Fecha:** Septiembre de 2019

La instrucción `RESTORE` en curso, el proceso de migración del servicio de migración de datos y la restauración a un momento dado integrada bloquean la actualización del nivel de servicio o del cambio de tamaño de la instancia existente, así como la creación de nuevas instancias hasta que finalice el proceso de restauración. El proceso de restauración bloqueará estas operaciones en las instancias administradas y en los grupos de instancias de la misma subred en la que se ejecute el proceso de restauración. Las instancias de los grupos de instancias no se ven afectadas. Las operaciones de creación o cambio de nivel de servicio no se realizarán correctamente o se agotará el tiempo de espera; continuarán cuando el proceso de restauración se haya completado o cancelado.

**Solución alternativa**: espere a que finalice el proceso de restauración; también puede cancelarlo si la operación de creación o actualización del nivel de servicio tiene mayor prioridad.

### <a name="resource-governor-on-business-critical-service-tier-might-need-to-be-reconfigured-after-failover"></a>Es posible que sea necesario volver a configurar Resource Governor en el nivel de servicio Crítico para la empresa después de la conmutación por error

**Fecha:** Septiembre de 2019

La característica [Resource Governor](/sql/relational-databases/resource-governor/resource-governor) que le permite limitar los recursos asignados a la carga de trabajo de usuario puede clasificar incorrectamente alguna carga de trabajo de usuario después de una conmutación por error o un cambio de nivel de servicio iniciado por el usuario (por ejemplo, el cambio de número máximo de núcleos virtuales o tamaño máximo de almacenamiento de instancia).

**Solución alternativa**: Ejecute `ALTER RESOURCE GOVERNOR RECONFIGURE` periódicamente o como parte del trabajo del Agente SQL que ejecuta la tarea de SQL cuando la instancia se inicia si usa [Resource Governor](/sql/relational-databases/resource-governor/resource-governor).

### <a name="cross-database-service-broker-dialogs-must-be-re-initialized-after-service-tier-upgrade"></a>Los cuadros de diálogo de Service Broker entre bases de datos se deben volver a inicializar después de la actualización del nivel de servicio

**Fecha:** Agosto de 2019

Los cuadros de diálogo de Service Broker entre bases de datos dejarán de enviar mensajes a los servicios de otras bases de datos después de la operación para cambiar el nivel de servicio. Los mensajes **no se pierden** y se pueden encontrar en la cola del remitente. Todos los cambios de tamaño de almacenamiento en núcleos virtuales o instancias de Instancia administrada harán que `service_broke_guid` cambie el valor de la vista [sys.databases](/sql/relational-databases/system-catalog-views/sys-databases-transact-sql) para todas las bases de datos. Todos los `DIALOG` creados con la instrucción [BEGIN DIALOG](/sql/t-sql/statements/begin-dialog-conversation-transact-sql) que hace referencia a los Service Broker de otra base de datos dejarán de entregar mensajes al servicio de destino.

**Solución alternativa:** Detenga todas las actividades que usen conversaciones con cuadros de diálogo de Service Broker entre bases de datos antes de actualizar el nivel de servicio y vuelva a inicializarlos después. Si quedan mensajes que no se entregaron después del cambio de nivel de servicio, lea los mensajes de la cola de origen y vuelva a enviarlos a la cola de destino.

### <a name="impersonification-of-azure-ad-login-types-is-not-supported"></a>No se admite la suplantación de tipos de inicio de sesión de Azure AD

**Fecha:** Julio de 2019

No se admite la suplantación con `EXECUTE AS USER` o `EXECUTE AS LOGIN` de las siguientes entidades de seguridad de AAD:
-   Usuarios de AAD con alias. Se devuelve el error siguiente en este caso: `15517`.
- Inicios de sesión y usuarios de AAD basados en aplicaciones de AAD o entidades de servicio. Se devuelve el error siguiente en este caso: `15517` y `15406`.

### <a name="query-parameter-not-supported-in-sp_send_db_mail"></a>No se admite el parámetro @query en sp_send_db_mail

**Fecha:** Abril de 2019

El parámetro `@query` del procedimiento [sp_send_db_mail](/sql/relational-databases/system-stored-procedures/sp-send-dbmail-transact-sql) no funciona.

### <a name="transactional-replication-must-be-reconfigured-after-geo-failover"></a>La replicación transaccional debe volver a configurarse después de la conmutación por error geográfica

**Fecha:** Marzo de 2019

Si la replicación transaccional se habilita en una base de datos en un grupo de conmutación por error automática, el administrador de la instancia administrada debe limpiar todas las publicaciones de la base de datos principal anterior y volver a configurarlas en la nueva base principal después de que se produzca una conmutación por error a otra región. Consulte [Replicación](#replication) para obtener más detalles.

### <a name="aad-logins-and-users-are-not-supported-in-ssdt"></a>No se admiten inicios de sesión y usuarios de AAD en SSDT

**Fecha:** Noviembre de 2019

SQL Server Data Tools no es totalmente compatible con los inicios de sesión y los usuarios de Azure Active Directory.

### <a name="temporary-database-is-used-during-restore-operation"></a>La base de datos temporal se usa durante la operación RESTORE

Cuando una base de datos se restaura en Instancia administrada, el servicio de restauración crea primero una base de datos vacía con el nombre deseado para asignar el nombre a la instancia. Después de un tiempo, esta base de datos se quitará y se iniciará la restauración de la base de datos real. La base de datos que se encuentra en estado de *restauración* será temporal y tendrá un valor de GUID aleatorio en lugar del nombre. El nombre temporal se cambiará al nombre deseado especificado en la instrucción `RESTORE` una vez que se complete el proceso de restauración. En la fase inicial, el usuario puede acceder a la base de datos vacía e incluso crear tablas o cargar datos en esta base de datos. Esta base de datos temporal se quitará cuando el servicio de restauración inicie la segunda fase.

**Solución alternativa**: No acceda a la base de datos que va a restaurar hasta que vea que la restauración se ha completado.

### <a name="tempdb-structure-and-content-is-re-created"></a>Se vuelve a crear la estructura y el contenido de TEMPDB

La base de datos `tempdb` siempre se divide en 12 archivos de datos y la estructura de archivos no se puede cambiar. No se puede cambiar el tamaño máximo por archivo y no se pueden agregar nuevos archivos a `tempdb`. Siempre se vuelve a crear `Tempdb` como base de datos vacía cuando se inicia la instancia o se realiza una conmutación por error. Cualquier cambio que se realice en `tempdb` no se conservará.

### <a name="exceeding-storage-space-with-small-database-files"></a>Exceder el espacio de almacenamiento con archivos de base de datos pequeños

Se puede producir un error en las instrucciones `CREATE DATABASE`, `ALTER DATABASE ADD FILE` y `RESTORE DATABASE` porque la instancia puede alcanzar el límite de almacenamiento de Azure.

Cada instancia administrada de uso general tiene hasta 35 TB de almacenamiento reservado para el espacio en disco Premium de Azure. Cada archivo de base de datos se coloca en un disco físico independiente. Los posibles tamaños de disco son: 128 GB, 256 GB, 512 GB, 1 TB o 4 TB. El espacio no utilizado en el disco no se cobra, pero la suma total de los tamaños de disco Premium de Azure no puede superar los 35 TB. En algunos casos, una instancia administrada que no necesita 8 TB en total puede superar los 35 TB de límite de Azure en tamaño de almacenamiento debido a la fragmentación interna.

Por ejemplo, una instancia administrada de Uso general puede tener un archivo grande de 1,2 TB situado en un disco de 4 TB. También podría tener 248 archivos de 1 GB cada uno situados en discos independientes de 128 GB. En este ejemplo:

- El tamaño de almacenamiento total del disco es de 1 x 4 TB + 248 x 128 GB = 35 TB.
- El espacio total reservado para las bases de datos en la instancia es de 1 x 1,2 TB + 248 x 1 GB = 1,4 TB.

Este ejemplo ilustra que, en determinadas circunstancias, debido a una distribución muy específica de archivos, una instancia administrada podría alcanzar el límite de 35 TB que está reservado para el disco adjunto Premium de Azure cuando no se lo espere.

En este ejemplo, las bases de datos existentes seguirán funcionando y pueden crecer sin ningún problema, siempre y cuando no se agreguen nuevos archivos. No se podrían crear ni restaurar nuevas bases de datos porque no hay suficiente espacio para nuevas unidades de disco, incluso si el tamaño total de todas las bases de datos no alcanza el límite de tamaño de la instancia. El error que se devuelve en ese caso no está claro.

También puede [identificar el número de archivos restantes](https://medium.com/azure-sqldb-managed-instance/how-many-files-you-can-create-in-general-purpose-azure-sql-managed-instance-e1c7c32886c1) mediante el uso de vistas del sistema. Si se alcanza este límite, intente [vaciar y eliminar algunos de los archivos más pequeños mediante la instrucción DBCC SHRINKFILE](/sql/t-sql/database-console-commands/dbcc-shrinkfile-transact-sql#d-emptying-a-file) o cambie al [nivel Crítico para la empresa, que no tiene este límite](/azure/sql-database/sql-database-managed-instance-resource-limits#service-tier-characteristics).

### <a name="guid-values-shown-instead-of-database-names"></a>Se muestran valores de GUID en lugar de nombres de base de datos

Varias vistas del sistema, contadores de rendimiento, mensajes de error, XEvents y entradas de registro de errores muestran identificadores de base de datos GUID en lugar de los nombres reales de base de datos. No confíe en estos identificadores GUID porque se reemplazarán por los nombres reales de las bases de datos en el futuro.

### <a name="error-logs-arent-persisted"></a>Los registros de errores no se conservan

Los registros de errores que están disponibles en la instancia administrada no se conservan y su tamaño no está incluido en el límite de almacenamiento máximo. Es posible que se borren automáticamente los registros de errores en caso de conmutación por error. Puede haber huecos en el historial del registro de errores porque Instancia administrada se ha migrado varias veces en varias máquinas virtuales.

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

**Solución alternativa:** use [SqlConnection.ChangeDatabase(String)](/dotnet/api/system.data.sqlclient.sqlconnection.changedatabase) para utilizar otra base de datos en un contexto de conexión en lugar de usar dos conexiones.

### <a name="clr-modules-and-linked-servers-sometimes-cant-reference-a-local-ip-address"></a>Los módulos de CLR y los servidores vinculados en algún momento no pueden hacer referencia a una dirección IP local.

Los módulos de CLR colocados en una instancia administrada y las consultas distribuidas o servidores vinculados que hacen referencia a una instancia actual en algún momento no pueden resolver la dirección IP de una instancia local. Este error es un problema transitorio.

**Solución alternativa:** use conexiones de contexto en un módulo de CLR, si es posible.

## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre instancias administradas, consulte [¿Qué es una instancia administrada?](sql-database-managed-instance.md)
- Para obtener una lista de características y ver una comparativa, consulte [Comparación de características de Azure SQL Database](sql-database-features.md).
- Para consultar un inicio rápido que muestra cómo crear una nueva instancia administrada, consulte [Creación de una instancia administrada](sql-database-managed-instance-get-started.md).
