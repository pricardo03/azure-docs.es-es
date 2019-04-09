---
title: Diferencias de T-SQL en Instancia administrada de Azure SQL Database | Microsoft Docs
description: En este artículo se describen las diferencias de T-SQL entre una instancia administrada en Azure SQL Database y SQL Server.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: carlrab, bonova
manager: craigg
ms.date: 03/13/2019
ms.openlocfilehash: 208370884d89a7a2585f320c037284d6657732db
ms.sourcegitcommit: e43ea344c52b3a99235660960c1e747b9d6c990e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/04/2019
ms.locfileid: "59010607"
---
# <a name="azure-sql-database-managed-instance-t-sql-differences-from-sql-server"></a>Diferencias de T-SQL en Instancia administrada de Azure SQL Database

La opción de implementación de la instancia administrada proporciona gran compatibilidad con instancias locales del Motor de base de datos de SQL Server. La instancia administrada admite la mayoría de las características del Motor de base de datos de SQL Server.

![migración](./media/sql-database-managed-instance/migration.png)

Puesto que todavía hay algunas diferencias de comportamiento y de sintaxis, en este artículo se resumen y se explican estas diferencias. <a name="Differences"></a>

- [Disponibilidad](#availability), incluidas las diferencias en [Always-On](#always-on-availability) y [Copias de seguridad](#backup).
- [Seguridad](#security), incluidas las diferencias en [Auditoría](#auditing), [Certificados](#certificates), [Credenciales](#credential), [Proveedores de servicios criptográficos](#cryptographic-providers), [Inicios de sesión y usuarios](#logins--users), [Clave maestra de servicio y clave de servicio](#service-key-and-service-master-key).
- [Configuración](#configuration), incluidas las diferencias en [Extensión del grupo de búferes](#buffer-pool-extension), [Intercalación](#collation), [Niveles de compatibilidad](#compatibility-levels),[Creación de reflejo de la base de datos ](#database-mirroring), [Opciones de base de datos](#database-options), [Agente SQL Server](#sql-server-agent), [Opciones de tabla](#tables).
- [Funcionalidades](#functionalities), incluidas [BULK INSERT/OPENROWSET](#bulk-insert--openrowset), [CLR](#clr), [DBCC](#dbcc), [Transacciones distribuidas](#distributed-transactions), [Eventos extendidos](#extended-events), [Bibliotecas externas](#external-libraries), [Filestream y Filetable](#filestream-and-filetable), [Búsqueda semántica de texto completo](#full-text-semantic-search), [Servidores vinculados](#linked-servers), [PolyBase](#polybase), [Replicación](#replication), [RESTORE](#restore-statement), [Service Broker](#service-broker), [Funciones, procedimientos almacenados, desencadenadores](#stored-procedures-functions-triggers).
- [Características que tienen un comportamiento diferente en instancias administradas](#Changes)
- [Problemas conocidos y limitaciones temporales](#Issues)

## <a name="availability"></a>Disponibilidad

### <a name="always-on-availability"></a>Always-On

La [alta disponibilidad](sql-database-high-availability.md) está integrada en Instancia administrada y no la pueden controlar los usuarios. No se admiten las siguientes instrucciones:

- [CREAR PUNTO DE CONEXIÓN... PARA DATABASE_MIRRORING](https://docs.microsoft.com/sql/t-sql/statements/create-endpoint-transact-sql)
- [CREAR GRUPO DE DISPONIBILIDAD](https://docs.microsoft.com/sql/t-sql/statements/create-availability-group-transact-sql)
- [COMANDO ALTER AVAILABILITY GROUP](https://docs.microsoft.com/sql/t-sql/statements/alter-availability-group-transact-sql)
- [QUITAR GRUPO DE DISPONIBILIDAD](https://docs.microsoft.com/sql/t-sql/statements/drop-availability-group-transact-sql)
- La cláusula [SET HADR](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-set-hadr) de la instrucción [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql)

### <a name="backup"></a>Copia de seguridad

Las instancias administradas hacen copias de seguridad automáticas y permiten a los usuarios crear copias de seguridad `COPY_ONLY` de bases de datos completas. No se admiten copias de seguridad de instantáneas de archivos, de registro ni diferenciales.

- Con una Instancia administrada, puede hacer una copia de seguridad de una base de datos de instancia solo en una cuenta de Azure Blob Storage:
  - Solo se admite `BACKUP TO URL`.
  - `FILE`, `TAPE`, y no se admiten dispositivos de copia de seguridad  
- Se admite la mayoría de las opciones de `WITH` generales.
  - `COPY_ONLY` es obligatorio
  - `FILE_SNAPSHOT` no admitido
  - No se admiten las opciones de cinta `REWIND`, `NOREWIND`, `UNLOAD` y `NOUNLOAD`.
  - No se admiten las opciones específicas de registro `NORECOVERY`, `STANDBY` y `NO_TRUNCATE`.

 Limitaciones:  

- Con una Instancia administrada, puede hacer una copia de seguridad de una base de datos de instancia en una copia de seguridad con hasta 32 franjas, lo que es suficiente para bases de datos de hasta 4 TB si se usa la compresión de copia de seguridad.
- Tamaño de franja de copia de seguridad máximo mediante la `BACKUP` comando en una instancia administrada es de 195 GB (tamaño máximo de blob). Aumente el número de franjas en el comando de copia de seguridad para reducir el tamaño de cada franja y permanecer dentro de este límite.

    > [!TIP]
    > Para solucionar esta limitación cuando la copia de seguridad de una base de datos de SQL Server en un entorno local o en una máquina virtual, puede hacer lo siguiente:
    >
    > - Copia de seguridad en `DISK` en lugar de la copia de seguridad `URL`
    > - Cargar los archivos de copia de seguridad en Blob storage
    > - Restaurar en la instancia administrada
    >
    > El `Restore` comando en una instancia administrada admite mayores tamaños de blob en los archivos de copia de seguridad porque se usa un tipo de blob distinto para el almacenamiento de los archivos de copia de seguridad cargados.

Para más información acerca de las copias de seguridad mediante T-SQL, consulte [BACKUP](https://docs.microsoft.com/sql/t-sql/statements/backup-transact-sql).

## <a name="security"></a>Seguridad

### <a name="auditing"></a>Auditoría

Las diferencias clave entre la auditoría en bases de datos en Azure SQL Database y bases de datos en SQL Server son las siguientes:

- Con la opción de implementación de Instancia administrada en Azure SQL Database, la auditoría funciona en el nivel de servidor y almacena los archivos de registro `.xel` en Azure Blob Storage.
- Con las opciones de implementación de base de datos única y grupo elástico en Azure SQL Database, la auditoría funciona en el nivel de la base de datos.
- En las máquinas virtuales o instancias locales de SQL Server, la auditoría funciona en el nivel de servidor, pero almacena los eventos en registros de eventos de Windows o del sistema de archivos.
  
En Instancia administrada, la auditoría de XEvent admite Azure Blob Storage como destino. No se admiten archivos ni registros de Windows.

Las principales diferencias en la sintaxis de `CREATE AUDIT` para la auditoría en Azure Blob Storage son:

- Se proporciona una nueva sintaxis de `TO URL` que permite especificar la dirección URL del contenedor de Azure Blob Storage donde se colocarán los archivos `.xel`.
- La sintaxis `TO FILE` no se admite porque una instancia administrada no puede acceder a los recursos compartidos de archivos de Windows.

Para más información, consulte:  

- [CREAR AUDITORÍA DE SERVIDOR](https://docs.microsoft.com/sql/t-sql/statements/create-server-audit-transact-sql)  
- [LA INSTRUCCIÓN ALTER SERVER AUDIT](https://docs.microsoft.com/sql/t-sql/statements/alter-server-audit-transact-sql)
- [Auditoría](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine)

### <a name="certificates"></a>Certificados

Una Instancia administrada no puede acceder a los recursos compartidos de archivos ni a las carpetas de Windows, por lo que se aplican las siguientes restricciones:

- `CREATE FROM`/`BACKUP TO` archivo no es compatible con los certificados
- `CREATE`/`BACKUP` certificado de `FILE` / `ASSEMBLY` no se admite. No se pueden usar archivos de clave privada.  

Consulte [CREATE CERTIFICATE](https://docs.microsoft.com/sql/t-sql/statements/create-certificate-transact-sql) y [BACKUP CERTIFICATE](https://docs.microsoft.com/sql/t-sql/statements/backup-certificate-transact-sql).  
  
**Solución alternativa**: aplique un script al certificado o la clave privada, guarde como archivo .sql y cree a partir del archivo binario:

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

### <a name="logins--users"></a>Inicios de sesión y usuarios

- Se admiten los inicios de sesión SQL creados con `FROM CERTIFICATE`, `FROM ASYMMETRIC KEY` y `FROM SID`. Consulte [CREATE LOGIN](https://docs.microsoft.com/sql/t-sql/statements/create-login-transact-sql).
- Se admiten las entidades de seguridad (inicios de sesión) del servidor de Azure Active Directory (Azure AD) creadas con la sintaxis [CREATE LOGIN](https://docs.microsoft.com/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current) o la sintaxis [CREATE USER FROM LOGIN [Azure AD Login]](https://docs.microsoft.com/sql/t-sql/statements/create-user-transact-sql?view=azuresqldb-mi-current) (**versión preliminar pública**). Estos son los inicios de sesión creados en el nivel de servidor.

    La instancia administrada admite las entidades de seguridad de la base de datos de Azure AD con la sintaxis `CREATE USER [AADUser/AAD group] FROM EXTERNAL PROVIDER`. Esto también se conoce como usuarios de bases de datos contenidos en Azure AD.

- No se admiten los inicios de sesión de Windows creados con `CREATE LOGIN ... FROM WINDOWS`. Use los usuarios e inicios de sesión de Azure Active Directory.
- El usuario de Azure AD que creó la instancia tiene [privilegios de administrador sin restricciones](sql-database-manage-logins.md#unrestricted-administrative-accounts).
- Los usuarios de nivel de base de datos de Azure Active Directory (Azure AD) que no son administradores pueden crearse con la sintaxis `CREATE USER ... FROM EXTERNAL PROVIDER`. Consulte [CREATE USER ... FROM EXTERNAL PROVIDER](sql-database-manage-logins.md#non-administrator-users).
- Las entidades de seguridad (inicios de sesión) del servidor de Azure AD admiten las características SQL en una única instancia de MI. No se admiten las funciones que requieren una interacción entre instancias, con independencia de que se encuentren en el mismo inquilino de Azure AD o en un inquilino diferente para los usuarios de Azure AD. Ejemplos de estas características son los siguientes:

  - Replicación transaccional de SQL y
  - Servidor de vínculos

- No se admite el establecimiento de un inicio de sesión de Azure AD asignado a un grupo de Azure AD como propietario de la base de datos.
- Se admite la suplantación de las entidades de seguridad a nivel de servidor de Azure AD mediante otras entidades de seguridad de Azure AD, como la cláusula [EXECUTE AS](/sql/t-sql/statements/execute-as-transact-sql). Limitación EXECUTE AS:

  - No se admite la cláusula EXECUTE AS USER para usuarios de Azure AD cuando el nombre es diferente del nombre de inicio de sesión. Por ejemplo, cuando el usuario se crea mediante la sintaxis CREATE USER [myAadUser] FROM LOGIN [john@contoso.com] y se intenta suplantar la identidad mediante EXEC AS USER = _myAadUser_. Cuando cree un usuario en **USER** a partir de una entidad de seguridad (inicio de sesión) de un servidor de Azure AD, especifique valor de user_name igual que el valor de login_name que se obtiene de **LOGIN**.
  - Solo las entidades de seguridad (inicios de sesión) a nivel de servidor SQL que forman parte del rol `sysadmin` pueden ejecutar las siguientes operaciones dirigidas a las entidades de seguridad de Azure AD:

    - EXECUTE AS USER
    - EXECUTE AS LOGIN

- Limitaciones de la **versión preliminar pública** para las entidades de seguridad (inicios de sesión) del servidor de Azure AD:

  - Limitaciones de administrador de Active Directory para Instancia administrada:

    - El administrador de Azure AD usado para configurar la instancia administrada no se puede usar para crear una entidad de seguridad (inicio de sesión) de un servidor de Azure AD dentro de la instancia administrada. Debe crear la primera entidad de seguridad (inicio de sesión) de un servidor de Azure AD mediante una cuenta de SQL Server que sea `sysadmin`. Esta es una limitación temporal que se quitará cuando las entidades de seguridad (inicios de sesión) de un servidor de Azure AD pasen a ser de disponibilidad general. Si intenta usar una cuenta de administrador de Azure AD para crear el inicio de sesión, verá el siguiente error: `Msg 15247, Level 16, State 1, Line 1 User does not have permission to perform this action.`
      - Actualmente, el primer inicio de sesión de Azure AD creado en la base de datos maestra debe crearlo la cuenta estándar de SQL Server (no de Azure AD) que es un `sysadmin` mediante la cláusula [CREATE LOGIN](/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current) FROM EXTERNAL PROVIDER. Después de la disponibilidad general, esta limitación se eliminará y el administrador de Active Directory podrá crear un inicio de sesión inicial de Azure AD para Instancia administrada.
    - No se admite DacFx (exportación/importación) que se utiliza con SQL Server Management Studio (SSMS) o SqlPackage para los inicios de sesión de Azure AD. Esta limitación se quitará cuando las entidades de seguridad (inicios de sesión) de un servidor de Azure AD pasen a ser de disponibilidad general.
    - Uso de entidades de seguridad (inicios de sesión) del servidor de Azure AD con SSMS

      - No se admiten los inicios de sesión de scripting de Azure AD (mediante cualquier inicio de sesión autenticado).
      - Intellisense no reconoce la instrucción **CREATE LOGIN FROM EXTERNAL PROVIDER** y mostrará un subrayado rojo.

- Solo el inicio de sesión de la entidad de seguridad a nivel de servidor (creado por el proceso de aprovisionamiento de Instancia administrada), los miembros de los roles de servidor (`securityadmin` o `sysadmin`) u otros inicios de sesión con permiso ALTER ANY LOGIN a nivel de servidor pueden crear entidades de seguridad (inicios de sesión) a nivel de servidor de Azure AD en la base de datos maestra para Instancia administrada.
- Si el inicio de sesión es una entidad de seguridad de SQL, solo los inicios de sesión que forman parte del rol `sysadmin` pueden utilizar el comando create para crear inicios de sesión para una cuenta de Azure AD.
- El inicio de sesión de Azure AD debe ser un miembro de una instancia de Azure AD dentro del mismo directorio utilizado para Instancia administrada de Azure SQL.
- Las entidades de seguridad (inicio de sesión) del servidor de Azure AD son visibles en el explorador de objetos a partir de SSMS 18.0 preview 5.
- Se permite la superposición de las entidades de seguridad (inicios de sesión) del servidor de Azure AD con una cuenta de administrador de Azure AD. Las entidades de seguridad (inicios de sesión) del servidor de Azure AD tienen prioridad sobre el administrador de Azure AD cuando se resuelve la entidad de seguridad y se aplican permisos a Instancia administrada.
- Durante la autenticación, se aplica la siguiente secuencia para resolver la entidad de seguridad de autenticación:

    1. Si la cuenta de Azure AD existe como directamente asignada a la entidad de seguridad (inicio de sesión) del servidor de Azure AD (presente en sys.server_principals como tipo "E"), conceda acceso y aplique los permisos de la entidad de seguridad (inicio de sesión) del servidor de Azure AD.
    2. Si la cuenta de Azure AD es un miembro de un grupo de Azure AD que está asignado a la entidad de seguridad (inicio de sesión) del servidor de Azure AD (presente en sys.server_principals como tipo "X"), conceda acceso y aplique los permisos del inicio de sesión del grupo de Azure AD.
    3. Si la cuenta de Azure AD es un administrador de Azure AD configurado en un portal especial para Instancia administrada (no existe en las vistas de sistema de Instancia administrada), aplique permisos fijos especiales del administrador de Azure AD para Instancia administrada (modo heredado).
    4. Si la cuenta de Azure AD existe como directamente asignada a un usuario de Azure AD de una base de datos (en sys.database_principals como tipo "E"), conceda acceso y aplique los permisos del usuario de la base de datos de Azure AD.
    5. Si la cuenta de Azure AD es un miembro de un grupo de Azure AD que está asignado a un usuario de Azure AD de una base de datos (en sys.database_principals como tipo "X"), conceda acceso y aplique los permisos del inicio de sesión del grupo de Azure AD.
    6. Si hay un inicio de sesión de Azure AD asignado a una cuenta de usuario de Azure AD o a una cuenta de grupo de Azure AD, al resolver la autenticación del usuario, se aplicarán todos los permisos de este inicio de sesión de Azure AD.

### <a name="service-key-and-service-master-key"></a>Clave maestra de servicio y clave de servicio

- No se permite realizar [copias de seguridad de la clave maestra](https://docs.microsoft.com/sql/t-sql/statements/backup-master-key-transact-sql) (administrada por el servicio de SQL Database).
- No se permite [restaurar la clave maestra](https://docs.microsoft.com/sql/t-sql/statements/restore-master-key-transact-sql) (administrada por el servicio de SQL Database).
- No se permite realizar [copias de seguridad de la clave maestra de servicio](https://docs.microsoft.com/sql/t-sql/statements/backup-service-master-key-transact-sql) (administrada por el servicio de SQL Database).
- No se permite [restaurar la clave maestra de servicio](https://docs.microsoft.com/sql/t-sql/statements/restore-service-master-key-transact-sql) (administrada por el servicio de SQL Database).

## <a name="configuration"></a>Configuración

### <a name="buffer-pool-extension"></a>Extensión del grupo de búferes

- No se admite la [extensión del grupo de búferes](https://docs.microsoft.com/sql/database-engine/configure-windows/buffer-pool-extension).
- `ALTER SERVER CONFIGURATION SET BUFFER POOL EXTENSION` no se admite. Consulte [ALTER SERVER CONFIGURATION](https://docs.microsoft.com/sql/t-sql/statements/alter-server-configuration-transact-sql).

### <a name="collation"></a>Collation

La intercalación de la instancia predeterminada es `SQL_Latin1_General_CP1_CI_AS` y puede especificarse como un parámetro de creación. Consulte [Intercalaciones](https://docs.microsoft.com/sql/t-sql/statements/collations).

### <a name="compatibility-levels"></a>Niveles de compatibilidad

- Los niveles de compatibilidad admitidos son: 100, 110, 120, 130, 140  
- No se admiten los niveles de compatibilidad menores que 100.
- El nivel de compatibilidad predeterminado es 140 para las bases de datos nuevas. Para las bases de datos restauradas, el nivel de compatibilidad no cambiará si era 100 o superior.

Consulte [Nivel de compatibilidad de ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level).

### <a name="database-mirroring"></a>Creación de reflejo de la base de datos

No se admite la creación de reflejo de la base de datos.

- `ALTER DATABASE SET PARTNER` y `SET WITNESS` no se admiten las opciones.
- `CREATE ENDPOINT … FOR DATABASE_MIRRORING` no se admite.

Para más información, consulte [ALTER DATABASE SET PARTNER y SET WITNESS](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-database-mirroring) y [CREATE ENDPOINT … FOR DATABASE_MIRRORING](https://docs.microsoft.com/sql/t-sql/statements/create-endpoint-transact-sql).

### <a name="database-options"></a>Opciones de base de datos

- No se permite usar varios archivos de registro.
- No se admiten objetos en memoria caché en el nivel de servicio de uso general.  
- Hay un límite de 280 archivos por lo que implica 280 archivos máximo por base de datos de instancia de propósito General. Datos y registro en General archivos propósito nivel cuentan para este límite. [Nivel crítico de negocio admite hasta 32.767 archivos por base de datos](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-managed-instance-resource-limits#service-tier-characteristics).
- La base de datos no puede contener grupos de archivos que contengan datos de secuencia de archivos.  Se producirá un error en la restauración si el archivo .bak contiene datos `FILESTREAM`.  
- Todos los archivos se colocan en Azure Blob Storage. La E/S y el rendimiento por archivo dependen del tamaño de cada archivo individual.  

#### <a name="create-database-statement"></a>Instrucción CREATE DATABASE

Se aplican las siguientes limitaciones de `CREATE DATABASE`:

- No se pueden definir archivos y grupos de archivos.  
- `CONTAINMENT` no se admite la opción.  
- `WITH`no se admiten las opciones.  
   > [!TIP]
   > Como alternativa, use `ALTER DATABASE` después de `CREATE DATABASE` para establecer las opciones de la base de datos para agregar archivos o para establecer el contenedor.  

- `FOR ATTACH` no se admite la opción
- `AS SNAPSHOT OF` no se admite la opción

Para más información, consulte [CREATE DATABASE](https://docs.microsoft.com/sql/t-sql/statements/create-database-sql-server-transact-sql).

#### <a name="alter-database-statement"></a>Instrucción ALTER DATABASE

Algunas propiedades de archivo no se pueden establecer ni cambiar:

- No se puede especificar la ruta de acceso del archivo en la instrucción T-SQL `ALTER DATABASE ADD FILE (FILENAME='path')`. Quite `FILENAME` del script porque una Instancia administrada coloca automáticamente los archivos.  
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

- La configuración del Agente SQL es de solo lectura. El procedimiento `sp_set_agent_properties` no se admite en la instancia administrada.  
- Trabajos
  - Se admiten los pasos de trabajo de T-SQL.
  - Se admiten los siguientes trabajos de replicación:
    - Lector del registro de transacciones
    - Instantánea
    - Distribuidor.
  - Se admiten los pasos de trabajo de SSIS.
  - Actualmente no se admiten otros tipos de pasos de trabajo, incluidos:
    - No se admite el paso de trabajo de replicación de mezcla.  
    - No se admite el lector de colas.  
    - Aún no se admite el shell de comandos.
  - Las instancias administradas no pueden acceder a los recursos externos (por ejemplo, recursos compartidos de red a través de robocopy).  
  - No se admite Analysis Services.
- Las notificaciones se admiten parcialmente.
- Se admite la notificación por correo electrónico; es necesario configurar un perfil de Correo electrónico de base de datos. Agente SQL puede usar el perfil de correo electrónico de una base de datos y debe llamarse `AzureManagedInstance_dbmail_profile`.  
  - El buscapersonas no se admite.  
  - No se admite NetSend.
  - Aún no se admiten las alertas.
  - No se admiten servidores proxy.  
- No se admite el registro de eventos.

Las siguientes características no se admiten en estos momentos pero se habilitarán en el futuro:

- Servidores proxy
- Programación de trabajos de CPU inactiva
- Habilitación o deshabilitación del agente
- Alertas

Para más información acerca del Agente SQL Server, consulte [Agente SQL Server](https://docs.microsoft.com/sql/ssms/agent/sql-server-agent).

### <a name="tables"></a>Tablas

No se admite lo siguiente:

- `FILESTREAM`
- `FILETABLE`
- `EXTERNAL TABLE`
- `MEMORY_OPTIMIZED`  

Para más información sobre cómo crear y modificar tablas, consulte [CREATE TABLE](https://docs.microsoft.com/sql/t-sql/statements/create-table-transact-sql) y [ALTER TABLE](https://docs.microsoft.com/sql/t-sql/statements/alter-table-transact-sql).

## <a name="functionalities"></a>Funcionalidades

### <a name="bulk-insert--openrowset"></a>Bulk insert/openrowset

Una instancia administrada no puede acceder a los recursos compartidos de archivos ni carpetas de Windows, por lo que los archivos se deben importar desde Azure Blob Storage:

- `DATASOURCE` se requiere en `BULK INSERT` comando durante la importación de archivos desde Azure Blob storage. Consulte [BULK INSERT](https://docs.microsoft.com/sql/t-sql/statements/bulk-insert-transact-sql).
- `DATASOURCE` se requiere en `OPENROWSET` funcionar cuando se lee el contenido de un archivo desde Azure Blob storage. Consulte [OPENROWSET](https://docs.microsoft.com/sql/t-sql/functions/openrowset-transact-sql).

### <a name="clr"></a>CLR

Una instancia administrada no puede acceder a los recursos compartidos de archivos ni a las carpetas de Windows, por lo que se aplican las siguientes restricciones:

- Solo se admite `CREATE ASSEMBLY FROM BINARY`. Consulte [CREATE ASSEMBLY FROM BINARY](https://docs.microsoft.com/sql/t-sql/statements/create-assembly-transact-sql).  
- `CREATE ASSEMBLY FROM FILE` no se admite. Consulte [CREATE ASSEMBLY FROM FILE](https://docs.microsoft.com/sql/t-sql/statements/create-assembly-transact-sql).
- `ALTER ASSEMBLY` no se puede hacer referencia a archivos. Consulte [ALTER ASSEMBLY](https://docs.microsoft.com/sql/t-sql/statements/alter-assembly-transact-sql).

### <a name="dbcc"></a>DBCC

Las instancias administradas no admiten instrucciones DBCC no documentadas que estén habilitadas en SQL Server.

- `Trace Flags` no se admiten. Consulte [Marcas de seguimiento](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql).
- `DBCC TRACEOFF` no se admite. Consulte [DBCC TRACEOFF](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceoff-transact-sql).
- `DBCC TRACEON` no se admite. Consulte [DBCC TRACEON](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceon-transact-sql).

### <a name="distributed-transactions"></a>Transacciones distribuidas

Actualmente no se admiten MSDTC ni las [Transacciones elásticas](sql-database-elastic-transactions-overview.md) en las instancias administradas.

### <a name="extended-events"></a>Eventos extendidos

No se admiten algunos destinos específicos de Windows para XEvents:

- `etw_classic_sync target` no se admite. Guarde los archivos `.xel` en Azure Blob Storage. Consulte [Destino etw_classic_sync](https://docs.microsoft.com/sql/relational-databases/extended-events/targets-for-extended-events-in-sql-server#etw_classic_sync_target-target).
- `event_file target` no se admite. Guarde los archivos `.xel` en Azure Blob Storage. Consulte [Destino event_file](https://docs.microsoft.com/sql/relational-databases/extended-events/targets-for-extended-events-in-sql-server#event_file-target).

### <a name="external-libraries"></a>Bibliotecas externas

Aún no se admiten las bibliotecas externas de R y Python para análisis en base de datos. Consulte [Machine Learning Services en SQL Server](https://docs.microsoft.com/sql/advanced-analytics/r/sql-server-r-services).

### <a name="filestream-and-filetable"></a>FileStream y Filetable

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

- Destinos admitidos: SQL Server y SQL Database
- Destinos no admitidos: archivos, Analysis Services y otros RDBMS.

Operaciones

- No se admiten las transacciones de escritura entre instancias.
- `sp_dropserver` se admite para quitar un servidor vinculado. Consulte [sp_dropserver](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-dropserver-transact-sql).
- `OPENROWSET` función puede utilizarse para ejecutar consultas solo en instancias de SQL Server (ya sean administradas, de forma local o en máquinas virtuales). Consulte [OPENROWSET](https://docs.microsoft.com/sql/t-sql/functions/openrowset-transact-sql).
- `OPENDATASOURCE` función puede utilizarse para ejecutar consultas solo en instancias de SQL Server (ya sean administradas, de forma local o en máquinas virtuales). Solo se admiten los valores `SQLNCLI`, `SQLNCLI11` y `SQLOLEDB` como proveedor. Por ejemplo: `SELECT * FROM OPENDATASOURCE('SQLNCLI', '...').AdventureWorks2012.HumanResources.Employee`. Consulte [OPENDATASOURCE](https://docs.microsoft.com/sql/t-sql/functions/opendatasource-transact-sql).

### <a name="polybase"></a>PolyBase

No se admiten tablas externas que hacen referencia a archivos en HDFS o Azure Blob Storage. Para más información acerca de Polybase, consulte [Polybase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide).

### <a name="replication"></a>Replicación

La replicación está disponible para la versión preliminar de Instancia administrada. Para más información sobre la replicación, consulte [Replicación de SQL Server](https://docs.microsoft.com/sql/relational-databases/replication/replication-with-sql-database-managed-instance).

### <a name="restore-statement"></a>Instrucción RESTORE

- Sintaxis admitida
  - `RESTORE DATABASE`
  - `RESTORE FILELISTONLY ONLY`
  - `RESTORE HEADER ONLY`
  - `RESTORE LABELONLY ONLY`
  - `RESTORE VERIFYONLY ONLY`
- Sintaxis no admitida
  - `RESTORE LOG ONLY`
  - `RESTORE REWINDONLY ONLY`
- Origen  
  - `FROM URL` (Azure Blob storage) es la única opción admitida.
  - `FROM DISK`/`TAPE`/ dispositivo de copia de seguridad no se admite.
  - No se admiten los conjuntos de copia de seguridad.
- `WITH` no se admiten las opciones (No `DIFFERENTIAL`, `STATS`, etcetera.)
- `ASYNC RESTORE` -Restauración continúa incluso si se interrumpe la conexión del cliente. Si la conexión se interrumpe, puede usar `sys.dm_operation_status` para ver el estado de una operación de restauración (y para crear y eliminar una base de datos). Consulte [sys.dm_operation_status](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database).  

Las siguientes opciones de base de datos se establecen o invalidan, y no se pueden cambiar más adelante:  

- `NEW_BROKER` (si el agente no está habilitado en el archivo .bak)  
- `ENABLE_BROKER` (si el agente no está habilitado en el archivo .bak)  
- `AUTO_CLOSE=OFF` (si tiene una base de datos en el archivo .bak `AUTO_CLOSE=ON`)  
- `RECOVERY FULL` (si tiene una base de datos en el archivo .bak `SIMPLE` o `BULK_LOGGED` modo de recuperación)
- Se agrega un grupo de archivos optimizado para memoria y se le asigna el nombre XTP si aún no se encuentra en el archivo .bak de origen.  
- Se cambia el nombre de todos los grupos de archivos optimizados para memoria existentes a XTP.  
- `SINGLE_USER` y `RESTRICTED_USER` opciones se convierten en `MULTI_USER`

 Limitaciones:  

- `.BAK` no se puede restaurar los archivos que contienen varios conjuntos de copia de seguridad.
- `.BAK` no se puede restaurar los archivos que contienen varios archivos de registro.
- Se producirá un error en la restauración si el archivo .bak contiene datos `FILESTREAM`.
- No se puede restaurar las copias de seguridad que contiene las bases de datos que tienen objetos en memoria activa en la instancia de propósito General.  
Para más información acerca de las instrucciones Restore, consulte [Instrucciones RESTORE](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-transact-sql).

### <a name="service-broker"></a>Service Broker

No se admite el agente de servicio entre instancias:

- `sys.routes` -Requisito previo: seleccione la dirección de sys.routes. La dirección debe ser LOCAL en todas las rutas. Consulte [sys.routes](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-routes-transact-sql).
- `CREATE ROUTE` -no puede usar `CREATE ROUTE` con `ADDRESS` distinto `LOCAL`. Consulte [CREATE ROUTE](https://docs.microsoft.com/sql/t-sql/statements/create-route-transact-sql).
- `ALTER ROUTE` no se puede `ALTER ROUTE` con `ADDRESS` distinto `LOCAL`. Consulte [ALTER ROUTE](https://docs.microsoft.com/sql/t-sql/statements/alter-route-transact-sql).  

### <a name="stored-procedures-functions-triggers"></a>Funciones, procedimientos almacenados, desencadenadores

- `NATIVE_COMPILATION` no se admite en el nivel de uso General.
- No se admiten las siguientes opciones de [sp_configure](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-configure-transact-sql):
  - `allow polybase export`
  - `allow updates`
  - `filestream_access_level`
  - `remote data archive`
  - `remote proc trans`
- `sp_execute_external_scripts` no se admite. Consulte [sp_execute_external_scripts](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql#examples).
- `xp_cmdshell` no se admite. Consulte [xp_cmdshell](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/xp-cmdshell-transact-sql).
- `Extended stored procedures` no se admiten, incluidos `sp_addextendedproc` y `sp_dropextendedproc`. Consulte [Procedimientos almacenados extendidos](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/general-extended-stored-procedures-transact-sql).
- `sp_attach_db`, `sp_attach_single_file_db`, y `sp_detach_db` no se admiten. Consulte [sp_attach_db](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-attach-db-transact-sql), [sp_attach_single_file_db](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-attach-single-file-db-transact-sql), y [sp_detach_db](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-detach-db-transact-sql).

## <a name="Changes"></a> Cambios de comportamiento

Las siguientes variables, funciones y vistas devuelven resultados diferentes:

- `SERVERPROPERTY('EngineEdition')` Devuelve el valor 8. Esta propiedad identifica de forma única una instancia administrada. Consulte [SERVERPROPERTY](https://docs.microsoft.com/sql/t-sql/functions/serverproperty-transact-sql).
- `SERVERPROPERTY('InstanceName')` Devuelve NULL, porque no existe el concepto de instancia, tal y como para SQL Server no se aplica a una instancia administrada. Consulte [SERVERPROPERTY('InstanceName')](https://docs.microsoft.com/sql/t-sql/functions/serverproperty-transact-sql).
- `@@SERVERNAME` Devuelve nombre DNS completo "conectable", por ejemplo, my-managed-instance.wcus17662feb9ce98.database.windows.net. Consulte [@@SERVERNAME](https://docs.microsoft.com/sql/t-sql/functions/servername-transact-sql).  
- `SYS.SERVERS` -Devuelve el completo "conectable" nombre DNS, como `myinstance.domain.database.windows.net` para las propiedades "name" y "data_source". Consulte [SYS.SERVERS](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-servers-transact-sql).
- `@@SERVICENAME` Devuelve NULL, porque no existe el concepto de servicio tal y como para SQL Server no se aplica a una instancia administrada. Consulte [@@SERVICENAME](https://docs.microsoft.com/sql/t-sql/functions/servicename-transact-sql).
- `SUSER_ID` se admite. Devuelve NULL si el inicio de sesión de Azure AD no está en sys.syslogins. Consulte [SUSER_ID](https://docs.microsoft.com/sql/t-sql/functions/suser-id-transact-sql).  
- `SUSER_SID` no se admite. Devuelve datos incorrectos (problema conocido temporal). Consulte [SUSER_SID](https://docs.microsoft.com/sql/t-sql/functions/suser-sid-transact-sql).
- `GETDATE()` y otras funciones integradas de fecha y hora siempre devuelve la hora en la zona horaria UTC. Consulte [GETDATE](https://docs.microsoft.com/sql/t-sql/functions/getdate-transact-sql).

## <a name="Issues"></a>Problemas conocidos y limitaciones

### <a name="tempdb-size"></a>Tamaño de TEMPDB

Tamaño máximo de archivo de `tempdb` no puede ser mayor que 24 GB/core en el nivel de uso General. Max `tempdb` tamaño en el nivel crítico para la empresa es limitado con el tamaño de almacenamiento de instancia. `tempdb` siempre se divide en archivos de 12 datos. No se puede cambiar este tamaño máximo por archivo y no se pueden agregar nuevos archivos a `tempdb`. Algunas consultas podrían devolver un error si necesitan más de 24GB / núcleo en `tempdb`.

### <a name="cannot-restore-contained-database"></a>No se puede restaurar la base de datos independiente

No se puede restaurar la instancia administrada [bases de datos independientes](https://docs.microsoft.com/sql/relational-databases/databases/contained-databases). En el momento restauración de las bases de datos independientes existentes no funcionan en instancia administrada. Este problema se quitará pronto y mientras tanto, se recomienda para quitar la opción de contención de las bases de datos que se colocan en la instancia administrada y no utilice la opción de contención para las bases de datos de producción.

### <a name="exceeding-storage-space-with-small-database-files"></a>Exceder el espacio de almacenamiento con archivos de base de datos pequeños

`CREATE DATABASE `, `ALTER DATABASE ADD FILE`, y `RESTORE DATABASE` instrucciones pueden producir un error porque la instancia puede alcanzar el límite de almacenamiento de Azure.

Cada instancia administrada de propósito General tiene hasta 35 TB de almacenamiento reservado para el espacio de disco Premium de Azure, y cada archivo de base de datos se coloca en un disco físico independiente. Los posibles tamaños de disco son: 128 GB, 256 GB, 512 GB, 1 TB o 4 TB. El espacio no utilizado en el disco no se cobra, pero la suma total de los tamaños de disco Premium de Azure no puede superar los 35 TB. En algunos casos, una instancia administrada que no necesita 8 TB en total puede superar los 35 TB de límite de Azure en tamaño de almacenamiento debido a la fragmentación interna.

Por ejemplo, podría tener una instancia administrada de propósito General uno archivo 1,2 TB de tamaño que se coloca en un disco de 4 TB y 248 archivos (cada 1 GB de tamaño) que se colocan en discos independientes de 128 GB. En este ejemplo:

- El tamaño de almacenamiento total del disco es de 1 x 4 TB + 248 x 128 GB = 35 TB.
- El espacio total reservado para las bases de datos en la instancia es de 1 x 1,2 TB + 248 x 1 GB = 1,4 TB.

Esto ilustra que, en determinadas circunstancias, debido a una distribución muy específica de archivos, una instancia administrada podría alcanzar los 35 TB reservados para el disco adjunto de Azure Premium cuando no se lo espere.

En este ejemplo, las bases de datos existentes seguirán funcionando y pueden crecer sin ningún problema, siempre y cuando no se agreguen nuevos archivos. Sin embargo, no se podrían crear ni restaurar nuevas bases de datos porque no hay suficiente espacio para nuevas unidades de disco, incluso si el tamaño total de todas las bases de datos no alcanza el límite de tamaño de la instancia. El error que se devuelve en ese caso no está claro.

También puede [identificar el número de archivos restantes](https://medium.com/azure-sqldb-managed-instance/how-many-files-you-can-create-in-general-purpose-azure-sql-managed-instance-e1c7c32886c1) usar vistas del sistema. Si se alcanza este límite intenta [vacíos y eliminar algunos de los archivos más pequeños mediante la instrucción DBCC SHRINKFILE](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-shrinkfile-transact-sql#d-emptying-a-file) o shitch a [nivel crítico para la empresa que no tiene este límite](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-managed-instance-resource-limits#service-tier-characteristics).

### <a name="incorrect-configuration-of-sas-key-during-database-restore"></a>Configuración incorrecta de la clave SAS durante la restauración de una base de datos

`RESTORE DATABASE` que lee el archivo .bak podría estar intentando para leer constantemente el archivo .bak y devuelve errores tras un período largo de tiempo si la firma de acceso compartido en `CREDENTIAL` es incorrecta. Ejecute RESTORE HEADERONLY antes de restaurar una base de datos para asegurarse de que la clave SAS es correcta.
Asegúrese de quitar el signo `?` inicial de la clave SAS generada mediante Azure Portal.

### <a name="tooling"></a>Herramientas

SQL Server Management Studio (SSMS) y SQL Server Data Tools (SSDT) podrían tener algunos problemas al acceder a una instancia administrada.

- Actualmente no se admite el uso de entidades de seguridad (inicios de sesión) y usuarios del servidor Azure AD (**versión preliminar pública**) con SSDT.
- No se admite en SSMS la creación de scripts para entidades de seguridad (inicios de sesión) y usuarios del servidor de Azure AD (**versión preliminar pública**).

### <a name="incorrect-database-names-in-some-views-logs-and-messages"></a>Nombres incorrectos de base de datos en algunas vistas, registros y mensajes

Varias vistas del sistema, contadores de rendimiento, mensajes de error, XEvents y entradas de registro de errores muestran identificadores de base de datos GUID en lugar de los nombres reales de base de datos. No confíe en estos identificadores GUID porque se reemplazarán por los nombres reales de base de datos en el futuro.

### <a name="database-mail"></a>Correo electrónico de base de datos

`@query` parámetro [sp_send_db_mail](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-send-dbmail-transact-sql) procedimiento no funcionan.

### <a name="database-mail-profile"></a>Perfil de Correo electrónico de base de datos

El perfil de correo electrónico de base de datos utilizado por el Agente SQL se debe llamar a `AzureManagedInstance_dbmail_profile`. No hay ninguna restricción con respecto a otros nombres de perfil de correo electrónico de base de datos.

### <a name="error-logs-are-not-persisted"></a>Los registros de errores no son persistentes

Los registros de errores que están disponibles en la instancia administrada no son persistentes y su tamaño no está incluido en el límite de almacenamiento máximo. Es posible que se borren automáticamente los registros de errores en caso de conmutación por error.

### <a name="error-logs-are-verbose"></a>Los registros de errores son detallados

Una instancia administrada coloca información detallada en los registros de errores, y muchos de ellos no son pertinentes. En el futuro se reducirá la cantidad de información de los registros de errores.

**Solución alternativa**: use un procedimiento personalizado para leer los registros de errores que filtran algunas entradas que no son pertinentes. Para más información, consulte [Managed Instance – sp_readmierrorlog](https://blogs.msdn.microsoft.com/sqlcat/2018/05/04/azure-sql-db-managed-instance-sp_readmierrorlog/) (Instancia administrada – sp_readmierrorlog).

### <a name="transaction-scope-on-two-databases-within-the-same-instance-isnt-supported"></a>No se admite el ámbito de transacción en dos bases de datos de la misma instancia

`TransactionScope` clase de .NET no funciona si dos consultas se envían a las dos bases de datos en la misma instancia en el mismo ámbito de transacción:

```C#
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

**Solución alternativa**: use [SqlConnection.ChangeDatabase(String)](https://docs.microsoft.com/dotnet/api/system.data.sqlclient.sqlconnection.changedatabase) para utilizar otra base de datos en el contexto de conexión en lugar de usar dos conexiones.

### <a name="clr-modules-and-linked-servers-sometime-cant-reference-local-ip-address"></a>Los módulos de CLR y los servidores vinculados en algún momento no pueden hacer referencia a la dirección IP local.

Los módulos de CLR colocados en una instancia administrada y las consultas distribuidas o servidores vinculados que hacen referencia a la instancia actual en algún momento no pueden resolver la dirección IP de la instancia local. Este error es un problema transitorio.

**Solución alternativa**: use conexiones de contexto en el módulo de CLR, si es posible.

### <a name="tde-encrypted-databases-dont-support-user-initiated-backups"></a>Las bases de datos cifrado TDE no son compatibles con las copias de seguridad iniciadas por el usuario

No se puede ejecutar `BACKUP DATABASE ... WITH COPY_ONLY` en una base de datos cifrada con Cifrado de datos transparente (TDE). TDE fuerza el cifrado de las copias de seguridad con claves TDE internas y la clave no se puede exportar, por lo que no podrá restaurar la copia de seguridad.

**Solución alternativa**: Use copias de seguridad automáticas y restauración a un momento dado o deshabilite el cifrado en la base de datos.

## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre las instancias administradas, consulte [¿Qué es una instancia administrada?](sql-database-managed-instance.md)
- Para obtener una lista de características y una comparación, consulte [Características comunes de SQL](sql-database-features.md).
- Para consultar una guía de inicio rápido que muestra cómo crear una nueva instancia administrada, consulte el artículo de [creación de instancias administradas](sql-database-managed-instance-get-started.md).
