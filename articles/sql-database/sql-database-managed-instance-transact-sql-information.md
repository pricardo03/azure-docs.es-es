---
title: Diferencias de Azure SQL Database Managed Instance T-SQL | Microsoft Docs
description: En este artículo se describen las diferencias de T-SQL entre una instancia administrada en Azure SQL Database y SQL Server.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein, carlrab, bonova
manager: craigg
ms.date: 03/13/2019
ms.custom: seoapril2019
ms.openlocfilehash: 5c8a15aa5198983a56a0238c1bb56f9345d07acc
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/28/2019
ms.locfileid: "66258590"
---
# <a name="azure-sql-database-managed-instance-t-sql-differences-from-sql-server"></a>Diferencias de T-SQL en Instancia administrada de Azure SQL Database

En este artículo se resume y explica las diferencias de sintaxis y el comportamiento entre instancia administrada de Azure SQL Database y el motor de base de datos local SQL Server. Se tratan los siguientes temas: <a name="Differences"></a>

- [Disponibilidad](#availability) incluye las diferencias en [Always-On](#always-on-availability) y [copias de seguridad](#backup).
- [Seguridad](#security) incluye las diferencias en [auditoría](#auditing), [certificados](#certificates), [credenciales](#credential), [proveedores de servicios criptográficos](#cryptographic-providers), [inicios de sesión y usuarios](#logins-and-users)y el [clave de servicio y la clave maestra de servicio](#service-key-and-service-master-key).
- [Configuración](#configuration) incluye las diferencias en [extensión del grupo de búferes](#buffer-pool-extension), [intercalación](#collation), [los niveles de compatibilidad](#compatibility-levels), [la creación de reflejo de base de datos ](#database-mirroring), [opciones de base de datos](#database-options), [del Agente SQL Server](#sql-server-agent), y [opciones de tabla](#tables).
- [Funcionalidades](#functionalities) incluye [BULK INSERT/OPENROWSET](#bulk-insert--openrowset), [CLR](#clr), [DBCC](#dbcc), [transacciones distribuidas](#distributed-transactions), [eventos extendidos](#extended-events), [bibliotecas externas](#external-libraries), [filestream y FileTable](#filestream-and-filetable), [búsqueda semántica de texto completo](#full-text-semantic-search), [servidores vinculados](#linked-servers), [PolyBase](#polybase), [replicación](#replication), [restaurar](#restore-statement), [Service Broker](#service-broker), [procedimientos almacenados, funciones y desencadenadores](#stored-procedures-functions-and-triggers).
- [Configuración del entorno](#Environment) , como las configuraciones de redes virtuales y la subred.
- [Las características que tienen un comportamiento diferente en las instancias administradas](#Changes).
- [Problemas conocidos y limitaciones temporales](#Issues).

La opción de implementación de la instancia administrada proporciona gran compatibilidad con instancias locales del Motor de base de datos de SQL Server. Instancia administrada admite la mayoría de las características del Motor de base de datos de SQL Server.

![Migración](./media/sql-database-managed-instance/migration.png)

## <a name="availability"></a>Disponibilidad

### <a name="always-on-availability"></a>Siempre activado

[Alta disponibilidad](sql-database-high-availability.md) está integrada en instancia administrada y no se puede controlar los usuarios. No se admiten las siguientes instrucciones:

- [CREATE ENDPOINT … FOR DATABASE_MIRRORING](https://docs.microsoft.com/sql/t-sql/statements/create-endpoint-transact-sql)
- [CREATE AVAILABILITY GROUP](https://docs.microsoft.com/sql/t-sql/statements/create-availability-group-transact-sql)
- [ALTER AVAILABILITY GROUP](https://docs.microsoft.com/sql/t-sql/statements/alter-availability-group-transact-sql)
- [DROP AVAILABILITY GROUP](https://docs.microsoft.com/sql/t-sql/statements/drop-availability-group-transact-sql)
- El [SET HADR](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-set-hadr) cláusula de la [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql) instrucción

### <a name="backup"></a>Copia de seguridad

Las instancias administradas tienen copias de seguridad automáticas, por lo que los usuarios pueden crear bases de datos completas `COPY_ONLY` copias de seguridad. No se admiten copias de seguridad de instantánea de archivos, registro y diferenciales.

- Con una instancia administrada, puede realizar copias de seguridad de una base de datos de instancia únicamente a una cuenta de almacenamiento de blobs de Azure:
  - Solo se admite `BACKUP TO URL`.
  - `FILE`, `TAPE`, y no se admiten los dispositivos de copia de seguridad.
- La mayoría de lo general `WITH` se admiten las opciones.
  - `COPY_ONLY` es obligatorio.
  - `FILE_SNAPSHOT` no se admite.
  - Las opciones de cinta: `REWIND`, `NOREWIND`, `UNLOAD`, y `NOUNLOAD` no se admiten.
  - Opciones específicas del registro: `NORECOVERY`, `STANDBY`, y `NO_TRUNCATE` no se admiten.

 Limitaciones: 

- Con una instancia administrada, puede crear una copia de una base de datos de la instancia una copia de seguridad con hasta 32 franjas, que es suficiente para bases de datos de hasta 4 TB si se utiliza la compresión de copia de seguridad.
- El tamaño de franja de copia de seguridad máximo mediante el uso de la `BACKUP` comando en una instancia administrada es de 195 GB, que es el tamaño máximo de blob. Aumente el número de franjas en el comando de copia de seguridad para reducir el tamaño de cada franja y permanecer dentro de este límite.

    > [!TIP]
    > Para solucionar esta limitación, realizar una copia de una base de datos de SQL Server en un entorno local o en una máquina virtual, hacer lo siguiente:
    >
    > - Copia de seguridad en `DISK` en lugar de hacer copia de seguridad `URL`.
    > - Cargue los archivos de copia de seguridad en almacenamiento de blobs.
    > - Restaurar en la instancia administrada.
    >
    > El `Restore` comando en una instancia administrada admite mayores tamaños de blob en los archivos de copia de seguridad porque se usa un tipo de blob distinto para el almacenamiento de los archivos de copia de seguridad cargados.

Para más información acerca de las copias de seguridad mediante T-SQL, consulte [BACKUP](https://docs.microsoft.com/sql/t-sql/statements/backup-transact-sql).

## <a name="security"></a>Seguridad

### <a name="auditing"></a>Auditoría

Las diferencias clave entre la auditoría en bases de datos en Azure SQL Database y bases de datos en SQL Server son las siguientes:

- Con la opción de implementación instancia administrada de Azure SQL Database, la auditoría funciona en el nivel de servidor. El `.xel` los archivos de registro se almacenan en Azure Blob storage.
- Con las opciones de implementación de base de datos única y grupo elástico en Azure SQL Database, la auditoría funciona en el nivel de la base de datos.
- En SQL Server locales o máquinas virtuales, la auditoría funciona en el nivel de servidor. Los eventos se almacenan en el sistema de archivos o los registros de eventos de Windows.
 
En Instancia administrada, la auditoría de XEvent admite Azure Blob Storage como destino. No se admiten los registros de archivo y Windows.

Las principales diferencias en la sintaxis de `CREATE AUDIT` para la auditoría en Azure Blob Storage son:

- Una nueva sintaxis `TO URL` es siempre que puede usar para especificar la dirección URL del contenedor de almacenamiento Blob de Azure donde la `.xel` se colocan los archivos.
- La sintaxis `TO FILE` no se admite porque una instancia administrada no puede tener acceso a recursos compartidos de archivos de Windows.

Para más información, consulte: 

- [CREATE SERVER AUDIT](https://docs.microsoft.com/sql/t-sql/statements/create-server-audit-transact-sql) 
- [ALTER SERVER AUDIT](https://docs.microsoft.com/sql/t-sql/statements/alter-server-audit-transact-sql)
- [Auditoría](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine)

### <a name="certificates"></a>Certificados

Una instancia administrada no puede tener acceso a recursos compartidos de archivos y carpetas de Windows, por lo que se aplican las restricciones siguientes:

- El `CREATE FROM` / `BACKUP TO` archivo no es compatible con los certificados.
- El `CREATE` / `BACKUP` certificado de `FILE` / `ASSEMBLY` no se admite. No se pueden usar archivos de clave privada. 

Consulte [CREATE CERTIFICATE](https://docs.microsoft.com/sql/t-sql/statements/create-certificate-transact-sql) y [BACKUP CERTIFICATE](https://docs.microsoft.com/sql/t-sql/statements/backup-certificate-transact-sql). 
 
**Solución alternativa**: Script para el certificado o la clave privada, guarde como archivo .sql y cree del binario:

```sql
CREATE CERTIFICATE  
   FROM BINARY = asn_encoded_certificate
WITH PRIVATE KEY (<private_key_options>)
```

### <a name="credential"></a>Credential:

Solo se admiten identidades de Azure Key Vault y `SHARED ACCESS SIGNATURE`. No se admiten usuarios de Windows.

Consulte [CREATE CREDENTIAL](https://docs.microsoft.com/sql/t-sql/statements/create-credential-transact-sql) y [ALTER CREDENTIAL](https://docs.microsoft.com/sql/t-sql/statements/alter-credential-transact-sql).

### <a name="cryptographic-providers"></a>Proveedores de servicios criptográficos

Una instancia administrada no puede tener acceso a archivos, por lo que no se pueden crear proveedores de servicios criptográficos:

- `CREATE CRYPTOGRAPHIC PROVIDER` no se admite. Consulte [CREATE CRYPTOGRAPHIC PROVIDER](https://docs.microsoft.com/sql/t-sql/statements/create-cryptographic-provider-transact-sql).
- `ALTER CRYPTOGRAPHIC PROVIDER` no se admite. Consulte [ALTER CRYPTOGRAPHIC PROVIDER](https://docs.microsoft.com/sql/t-sql/statements/alter-cryptographic-provider-transact-sql).

### <a name="logins-and-users"></a>Inicios de sesión y usuarios

- Inicios de sesión SQL creados mediante el uso de `FROM CERTIFICATE`, `FROM ASYMMETRIC KEY`, y `FROM SID` son compatibles. Consulte [CREATE LOGIN](https://docs.microsoft.com/sql/t-sql/statements/create-login-transact-sql).
- Entidades de servidor de Azure Active Directory (Azure AD) (inicios de sesión) creadas con el [CREATE LOGIN](https://docs.microsoft.com/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current) sintaxis o [crear usuario de inicio de sesión [inicio de sesión de Azure AD]](https://docs.microsoft.com/sql/t-sql/statements/create-user-transact-sql?view=azuresqldb-mi-current) sintaxis se admiten (versión preliminar pública). Estos inicios de sesión se crean en el nivel de servidor.

    La instancia administrada admite las entidades de seguridad de la base de datos de Azure AD con la sintaxis `CREATE USER [AADUser/AAD group] FROM EXTERNAL PROVIDER`. Esta característica es también conocida como usuarios de base de datos de Azure AD contenido.

- Inicios de sesión de Windows creados con el `CREATE LOGIN ... FROM WINDOWS` sintaxis no se admiten. Use los usuarios e inicios de sesión de Azure Active Directory.
- El usuario de Azure AD que creó la instancia tiene [unrestricted privilegios de administrador](sql-database-manage-logins.md#unrestricted-administrative-accounts).
- Los usuarios de nivel de base de datos de Azure AD no son administradores pueden crearse mediante el uso de la `CREATE USER ... FROM EXTERNAL PROVIDER` sintaxis. Consulte [CREATE USER ... FROM EXTERNAL PROVIDER](sql-database-manage-logins.md#non-administrator-users).
- Entidades de servidor de Azure AD (inicios de sesión) admiten las características de SQL dentro de una instancia administrada. No se admiten las características que requieren la interacción entre instancias, independientemente de si está dentro de la misma instancia de AD Azure de inquilinos o inquilinos diferentes, para usuarios de Azure AD. Ejemplos de estas características son los siguientes:

  - Replicación transaccional de SQL.
  - Servidor de vínculos.

- No se admite el establecimiento de un inicio de sesión de Azure AD asignado a un grupo de Azure AD como propietario de la base de datos.
- Se admite la suplantación de entidades de seguridad de nivel de servidor de Azure AD mediante el uso de otras entidades de seguridad de Azure AD, como la [EXECUTE AS](/sql/t-sql/statements/execute-as-transact-sql) cláusula. EXECUTE AS limitaciones son:

  - No se admite EXECUTE AS USER para usuarios de Azure AD cuando el nombre difiere del nombre de inicio de sesión. Un ejemplo es cuando el usuario se crea mediante la sintaxis de CREATE USER [myAadUser] desde el inicio de sesión [john@contoso.com] y se intenta la suplantación mediante EXEC AS USER = _myAadUser_. Cuando creas un **usuario** desde una entidad de servidor de Azure AD (inicio de sesión), especifique el user_name como el mismo login_name de **inicio de sesión**.
  - Solo el nivel de SQL Server las entidades de seguridad (inicios de sesión) que forman parte de la `sysadmin` pueden ejecutar las siguientes operaciones que tienen como destino las entidades de seguridad de Azure AD:

    - EXECUTE AS USER
    - EXECUTE AS LOGIN

- Limitaciones de la versión preliminar pública de entidades de seguridad del servidor de Azure AD (inicios de sesión):

  - Limitaciones de administración activas Directory para instancia administrada:

    - El Administrador de Azure AD que usó para configurar la instancia administrada no se puede usar para crear un servidor de Azure AD principal (inicio de sesión) dentro de la instancia administrada. Debe crear la primera Azure principal del servidor de AD (inicio de sesión) con una cuenta de SQL Server es un `sysadmin` rol. Después de entidades de seguridad del servidor de Azure AD (inicios de sesión) están disponibles con carácter general, se quitará esta limitación temporal. Si intenta usar una cuenta de administrador de Azure AD para crear el inicio de sesión, verá el siguiente error: `Msg 15247, Level 16, State 1, Line 1 User does not have permission to perform this action.`
      - Actualmente, se debe crear el primer inicio de sesión de Azure AD creado en la base de datos maestra mediante la cuenta de SQL Server estándar (que no son de Azure AD) es un `sysadmin` rol utilizando [CREATE LOGIN](/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current) de proveedor externo. Después de la disponibilidad general, se quitará esta limitación. A continuación, puede crear un inicial inicio de sesión de Azure AD usando el Administrador de Active Directory para la instancia administrada.
    - DacFx (exportación/importación) se utiliza con SQL Server Management Studio o SqlPackage no se admite para los inicios de sesión de Azure AD. Esta limitación se eliminará después de entidades de seguridad del servidor de Azure AD (inicios de sesión) están disponibles con carácter general.
    - Uso de entidades de seguridad del servidor de Azure AD (inicios de sesión) con SQL Server Management Studio:

      - No se admiten inicios de sesión de Azure AD que usan cualquier inicio de sesión autenticado de secuencias de comandos.
      - IntelliSense no reconoce la instrucción de creación de inicio de sesión de proveedor externo y muestra un subrayado rojo.

- Solo el nivel de servidor de inicio de sesión principal, que se crea la instancia administrada de proceso, los miembros de los roles de servidor de aprovisionamiento, como `securityadmin` o `sysadmin`, o Azure AD, pueden crear otros inicios de sesión con permiso ALTER ANY LOGIN en el nivel de servidor entidades de servidor (inicios de sesión) en la base de datos maestra para la instancia administrada.
- Si el inicio de sesión es una entidad de seguridad de SQL, solo los inicios de sesión que forman parte de la `sysadmin` rol puede usar el comando create a inicios de sesión de creación de una cuenta de Azure AD.
- El inicio de sesión de Azure AD debe ser un miembro de Azure AD en el mismo directorio que se usa para la instancia administrada de Azure SQL Database.
- Entidades de servidor de Azure AD (inicios de sesión) son visibles en el Explorador de objetos a partir de SQL Server Management Studio 18,0 preview 5.
- Se permite la superposición de las entidades de seguridad (inicios de sesión) del servidor de Azure AD con una cuenta de administrador de Azure AD. Entidades de servidor de Azure AD (inicios de sesión) tienen prioridad sobre el Administrador de Azure AD al resolver la entidad de seguridad y aplicar permisos a la instancia administrada.
- Durante la autenticación, se aplica la siguiente secuencia para resolver la entidad de seguridad de autenticación:

    1. Si existe la cuenta de Azure AD directamente asignada a la entidad de seguridad de servidor de Azure AD (inicio de sesión), que está presente en sys.server_principals como tipo "E", conceder acceso y se aplican los permisos de la entidad de seguridad del servidor de Azure AD (inicio de sesión).
    2. Si la cuenta de Azure AD es un miembro de un grupo de Azure AD que está asignado a la entidad de seguridad de servidor de Azure AD (inicio de sesión), que está presente en sys.server_principals como tipo de "X", conceder acceso y se aplican los permisos del inicio de sesión de grupo de Azure AD.
    3. Si la cuenta de Azure AD es un especial configurada por el portal de administrador de Azure AD para instancia administrada, que no existe en las vistas del sistema de instancia administrada, se aplican permisos fijos especiales de la administración de Azure AD para la instancia administrada (modo heredado).
    4. Si existe la cuenta de Azure AD como directamente asignada a un usuario de Azure AD en una base de datos, que está presente en YS.database_principals como tipo "E", conceder acceso y se aplican los permisos del usuario de base de datos de Azure AD.
    5. Si la cuenta de Azure AD es un miembro de un grupo de Azure AD que se asigna a un usuario de Azure AD en una base de datos, que está presente en YS.database_principals como tipo "X", conceder acceso y se aplican los permisos del inicio de sesión de grupo de Azure AD.
    6. Si hay un inicio de sesión de Azure AD asignada a una cuenta de usuario de Azure AD o una cuenta de grupo de Azure AD, que se resuelve como el usuario que se autentica, se aplican todos los permisos de este inicio de sesión de Azure AD.

### <a name="service-key-and-service-master-key"></a>Clave maestra de servicio y clave de servicio

- [Copia de seguridad de clave maestra](https://docs.microsoft.com/sql/t-sql/statements/backup-master-key-transact-sql) no se admite (administradas por el servicio SQL Database).
- [Restaurar la clave maestra](https://docs.microsoft.com/sql/t-sql/statements/restore-master-key-transact-sql) no se admite (administradas por el servicio SQL Database).
- [Copia de seguridad de clave maestra de servicio](https://docs.microsoft.com/sql/t-sql/statements/backup-service-master-key-transact-sql) no se admite (administradas por el servicio SQL Database).
- [Restauración de la clave maestra de servicio](https://docs.microsoft.com/sql/t-sql/statements/restore-service-master-key-transact-sql) no se admite (administradas por el servicio SQL Database).

## <a name="configuration"></a>Configuración

### <a name="buffer-pool-extension"></a>Extensión del grupo de búferes

- [Extensión del grupo de búferes](https://docs.microsoft.com/sql/database-engine/configure-windows/buffer-pool-extension) no se admite.
- `ALTER SERVER CONFIGURATION SET BUFFER POOL EXTENSION` no se admite. Consulte [ALTER SERVER CONFIGURATION](https://docs.microsoft.com/sql/t-sql/statements/alter-server-configuration-transact-sql).

### <a name="collation"></a>Collation

La intercalación de la instancia predeterminada es `SQL_Latin1_General_CP1_CI_AS` y puede especificarse como un parámetro de creación. Consulte [Intercalaciones](https://docs.microsoft.com/sql/t-sql/statements/collations).

### <a name="compatibility-levels"></a>Niveles de compatibilidad

- Niveles de compatibilidad admitidos son 100, 110, 120, 130 y 140.
- No se admiten los niveles de compatibilidad menores que 100.
- El nivel de compatibilidad predeterminado es 140 para las bases de datos nuevas. Para las bases de datos restauradas, el nivel de compatibilidad permanece sin cambios si era 100 y versiones posteriores.

Consulte [Nivel de compatibilidad de ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level).

### <a name="database-mirroring"></a>Creación de reflejo de la base de datos

No se admite la creación de reflejo de la base de datos.

- Las opciones `ALTER DATABASE SET PARTNER` y `SET WITNESS` no se admiten.
- `CREATE ENDPOINT … FOR DATABASE_MIRRORING` no se admite.

Para más información, consulte [ALTER DATABASE SET PARTNER y SET WITNESS](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-database-mirroring) y [CREATE ENDPOINT … FOR DATABASE_MIRRORING](https://docs.microsoft.com/sql/t-sql/statements/create-endpoint-transact-sql).

### <a name="database-options"></a>Opciones de base de datos

- No se permite usar varios archivos de registro.
- No se admiten objetos en memoria caché en el nivel de servicio de uso general. 
- Hay un límite de 280 archivos por instancia de propósito General, lo que implica un máximo de 280 archivos por base de datos. Los archivos de datos y de registro en el nivel de uso General se cuentan para este límite. [El nivel crítico para la empresa admite hasta 32.767 archivos por base de datos](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-resource-limits#service-tier-characteristics).
- La base de datos no puede contener grupos de archivos que contienen datos filestream. Error en la restauración si el archivo .bak contiene `FILESTREAM` datos. 
- Todos los archivos se colocan en Azure Blob Storage. La E/S y el rendimiento por archivo dependen del tamaño de cada archivo individual.

#### <a name="create-database-statement"></a>Instrucción CREATE DATABASE

Las siguientes limitaciones se aplican a `CREATE DATABASE`:

- No se pueden definir archivos y grupos de archivos. 
- El `CONTAINMENT` no se admite la opción. 
- `WITH` no se admiten las opciones. 
   > [!TIP]
   > Como alternativa, use `ALTER DATABASE` después `CREATE DATABASE` para establecer las opciones de base de datos para agregar archivos o para establecer el contenedor. 

- El `FOR ATTACH` no se admite la opción.
- El `AS SNAPSHOT OF` no se admite la opción.

Para más información, consulte [CREATE DATABASE](https://docs.microsoft.com/sql/t-sql/statements/create-database-sql-server-transact-sql).

#### <a name="alter-database-statement"></a>Instrucción ALTER DATABASE

Algunas propiedades de archivo no se pueden establecer ni cambiar:

- No se puede especificar una ruta de acceso de archivo en el `ALTER DATABASE ADD FILE (FILENAME='path')` instrucción T-SQL. Quite `FILENAME` del script porque una instancia administrada coloca automáticamente los archivos. 
- No se puede cambiar un nombre de archivo mediante el uso de la `ALTER DATABASE` instrucción.

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

- Configuración del Agente SQL Server es de solo lectura. El procedimiento `sp_set_agent_properties` no se admite en instancia administrada. 
- Trabajos
  - Se admiten los pasos de trabajo de T-SQL.
  - Se admiten los siguientes trabajos de replicación:
    - Lector del registro de transacciones
    - Instantánea
    - Distribuidor.
  - Los pasos de trabajo SSIS se admiten.
  - Actualmente no se admiten otros tipos de pasos de trabajo:
    - No se admite el paso de trabajo de replicación de mezcla. 
    - No se admite el lector de colas. 
    - Aún no se admite el shell de comandos.
  - Las instancias administradas no pueden obtener acceso a recursos externos, por ejemplo, recursos compartidos de red a través de robocopy. 
  - No se admiten SQL Server Analysis Services.
- Las notificaciones se admiten parcialmente.
- Se admite la notificación por correo electrónico, aunque requiere que configure un perfil de correo electrónico de base de datos. Agente SQL Server puede utilizar solo un perfil de correo electrónico de base de datos, y debe llamarse `AzureManagedInstance_dbmail_profile`. 
  - El buscapersonas no se admite. 
  - No se admite NetSend.
  - Aún no se admiten las alertas.
  - No se admiten servidores proxy. 
- No se admite el registro de eventos.

Las siguientes características actualmente no se admiten, pero se habilitarán en el futuro:

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

Para obtener información acerca de cómo crear y modificar las tablas, consulte [CREATE TABLE](https://docs.microsoft.com/sql/t-sql/statements/create-table-transact-sql) y [ALTER TABLE](https://docs.microsoft.com/sql/t-sql/statements/alter-table-transact-sql).

## <a name="functionalities"></a>Funcionalidades

### <a name="bulk-insert--openrowset"></a>Bulk insert/openrowset

Una instancia administrada no puede tener acceso a recursos compartidos de archivos y carpetas de Windows, por lo que se deben importar los archivos de Azure Blob storage:

- `DATASOURCE` se requiere en el `BULK INSERT` comando aunque importar archivos desde Azure Blob storage. Consulte [BULK INSERT](https://docs.microsoft.com/sql/t-sql/statements/bulk-insert-transact-sql).
- `DATASOURCE` se requiere en el `OPENROWSET` funcionar cuando se lee el contenido de un archivo desde Azure Blob storage. Consulte [OPENROWSET](https://docs.microsoft.com/sql/t-sql/functions/openrowset-transact-sql).

### <a name="clr"></a>CLR

Una instancia administrada no puede tener acceso a recursos compartidos de archivos y carpetas de Windows, por lo que se aplican las restricciones siguientes:

- Solo se admite `CREATE ASSEMBLY FROM BINARY`. Consulte [CREATE ASSEMBLY FROM BINARY](https://docs.microsoft.com/sql/t-sql/statements/create-assembly-transact-sql). 
- `CREATE ASSEMBLY FROM FILE` no se admite. Consulte [CREATE ASSEMBLY FROM FILE](https://docs.microsoft.com/sql/t-sql/statements/create-assembly-transact-sql).
- `ALTER ASSEMBLY` no puede hacer referencia a archivos. Consulte [ALTER ASSEMBLY](https://docs.microsoft.com/sql/t-sql/statements/alter-assembly-transact-sql).

### <a name="dbcc"></a>DBCC

Instrucciones DBCC no documentadas que estén habilitadas en SQL Server no se admiten en las instancias administradas.

- `Trace flags` no se admiten. Consulte [marcas de seguimiento](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql).
- `DBCC TRACEOFF` no se admite. Consulte [DBCC TRACEOFF](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceoff-transact-sql).
- `DBCC TRACEON` no se admite. Consulte [DBCC TRACEON](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceon-transact-sql).

### <a name="distributed-transactions"></a>Transacciones distribuidas

MSDTC y [transacciones elásticas](sql-database-elastic-transactions-overview.md) actualmente no se admiten en las instancias administradas.

### <a name="extended-events"></a>Eventos extendidos

No se admiten algunos destinos de Windows específicos para eventos extendidos (XEvents):

- El `etw_classic_sync` no se admite el destino. Store `.xel` archivos en Azure Blob storage. Consulte [Destino etw_classic_sync](https://docs.microsoft.com/sql/relational-databases/extended-events/targets-for-extended-events-in-sql-server#etw_classic_sync_target-target).
- El `event_file` no se admite el destino. Store `.xel` archivos en Azure Blob storage. Consulte [Destino event_file](https://docs.microsoft.com/sql/relational-databases/extended-events/targets-for-extended-events-in-sql-server#event_file-target).

### <a name="external-libraries"></a>Bibliotecas externas

R y Python en bases de datos, las bibliotecas externas no admiten aún. Consulte [Machine Learning Services en SQL Server](https://docs.microsoft.com/sql/advanced-analytics/r/sql-server-r-services).

### <a name="filestream-and-filetable"></a>FileStream y FileTable

- los datos FileStream no se admiten.
- La base de datos no puede contener grupos de archivos con `FILESTREAM` datos.
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

- Destinos admitidos son SQL Server y SQL Database.
- Los destinos que no se admiten son archivos, Analysis Services y otros RDBMS.

Operaciones

- No se admiten las transacciones de escritura entre instancias.
- Se admite `sp_dropserver` para quitar un servidor vinculado. Consulte [sp_dropserver](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-dropserver-transact-sql).
- El `OPENROWSET` función puede utilizarse para ejecutar consultas solo en instancias de SQL Server. Pueden ser administrados, de forma local, o en máquinas virtuales. Consulte [OPENROWSET](https://docs.microsoft.com/sql/t-sql/functions/openrowset-transact-sql).
- El `OPENDATASOURCE` función puede utilizarse para ejecutar consultas solo en instancias de SQL Server. Pueden ser administrados, de forma local, o en máquinas virtuales. Solo el `SQLNCLI`, `SQLNCLI11`, y `SQLOLEDB` se admiten los valores como un proveedor. Un ejemplo es `SELECT * FROM OPENDATASOURCE('SQLNCLI', '...').AdventureWorks2012.HumanResources.Employee`. Consulte [OPENDATASOURCE](https://docs.microsoft.com/sql/t-sql/functions/opendatasource-transact-sql).

### <a name="polybase"></a>PolyBase

Tablas de externas que hacen referencia a que los archivos en HDFS o Azure Blob storage no se admiten. Para obtener información sobre PolyBase, consulte [PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide).

### <a name="replication"></a>Replicación

La replicación está disponible para la versión preliminar de Instancia administrada. Para obtener información acerca de la replicación, vea [replicación de SQL Server](https://docs.microsoft.com/sql/relational-databases/replication/replication-with-sql-database-managed-instance).

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
  - `FROM URL` (Azure Blob storage) es la única opción admitida.
  - No se admite `FROM DISK`/`TAPE`/dispositivo de copia de seguridad.
  - No se admiten los conjuntos de copia de seguridad.
- `WITH` no se admiten las opciones, como no `DIFFERENTIAL` o `STATS`.
- `ASYNC RESTORE`: Restauración continúa incluso si se interrumpe la conexión de cliente. Si la conexión se interrumpe, puede comprobar el `sys.dm_operation_status` ver el estado de una operación de restauración y una base de datos CREATE y DROP. Consulte [sys.dm_operation_status](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database). 

Las siguientes opciones de base de datos se establece o se invalida y no se puede cambiar más adelante: 

- `NEW_BROKER` Si el agente no está habilitado en el archivo .bak. 
- `ENABLE_BROKER` Si el agente no está habilitado en el archivo .bak. 
- `AUTO_CLOSE=OFF` Si tiene una base de datos en el archivo .bak `AUTO_CLOSE=ON`. 
- `RECOVERY FULL` Si tiene una base de datos en el archivo .bak `SIMPLE` o `BULK_LOGGED` modo de recuperación.
- Se agrega un grupo de archivos optimizados para memoria y se denomina XTP si no en el archivo .bak de origen. 
- Se cambia el nombre de cualquier grupo de archivos optimizados para memoria existente a XTP. 
- `SINGLE_USER` y `RESTRICTED_USER` opciones se convierten en `MULTI_USER`.

 Limitaciones: 

- `.BAK` no se puede restaurar los archivos que contienen varios conjuntos de copia de seguridad. 
- `.BAK` no se puede restaurar los archivos que contienen varios archivos de registro.
- Error en la restauración si el archivo .bak contiene `FILESTREAM` datos.
- No se puede restaurar las copias de seguridad que contienen bases de datos con objetos en memoria activos en una instancia de propósito General. Para obtener información acerca de las instrucciones restore, consulte [instrucciones RESTORE](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-transact-sql).

### <a name="service-broker"></a>Service Broker

No se admite el agente de servicio entre instancias:

- `sys.routes`: Como requisito previo, debe seleccionar la dirección de sys.routes. La dirección debe ser LOCAL en todas las rutas. Consulte [sys.routes](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-routes-transact-sql).
- `CREATE ROUTE`: No puede usar `CREATE ROUTE` con `ADDRESS` distinto `LOCAL`. Consulte [CREATE ROUTE](https://docs.microsoft.com/sql/t-sql/statements/create-route-transact-sql).
- `ALTER ROUTE`: No puede usar `ALTER ROUTE` con `ADDRESS` distinto `LOCAL`. Consulte [ALTER ROUTE](https://docs.microsoft.com/sql/t-sql/statements/alter-route-transact-sql). 

### <a name="stored-procedures-functions-and-triggers"></a>Procedimientos almacenados, funciones y desencadenadores

- `NATIVE_COMPILATION` no se admite en el nivel de uso General.
- No se admiten las siguientes opciones de [sp_configure](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-configure-transact-sql): 
  - `allow polybase export`
  - `allow updates`
  - `filestream_access_level`
  - `remote data archive`
  - `remote proc trans`
- `sp_execute_external_scripts` no se admite. Consulte [sp_execute_external_scripts](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql#examples).
- `xp_cmdshell` no se admite. Consulte [xp_cmdshell](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/xp-cmdshell-transact-sql).
- `Extended stored procedures` no se admiten, que incluye `sp_addextendedproc`  y `sp_dropextendedproc`. Consulte [procedimientos almacenados extendidos](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/general-extended-stored-procedures-transact-sql).
- No se admiten `sp_attach_db`, `sp_attach_single_file_db`, y `sp_detach_db`. Consulte [sp_attach_db](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-attach-db-transact-sql), [sp_attach_single_file_db](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-attach-single-file-db-transact-sql), y [sp_detach_db](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-detach-db-transact-sql).

## <a name="Environment"></a>Restricciones de Environmet

### <a name="subnet"></a>Subred
- En la subred reservada para la instancia administrada no se puede colocar cualquier otro recurso (por ejemplo máquinas virtuales). Colocar estos recursos en otras subredes.
- Subred debe tener un número suficiente de disponible [direcciones IP](sql-database-managed-instance-connectivity-architecture.md#network-requirements). Mínimo es 16, aunque se recomienda tener al menos 32 direcciones IP en la subred.
- [Los puntos de conexión de servicio no se pueden asociadas a la subred de la instancia administrada](sql-database-managed-instance-connectivity-architecture.md#network-requirements). Asegúrese de que la opción de puntos de conexión de servicio está deshabilitada cuando se crea la red virtual.
- El número y los tipos de instancias que se pueden colocar en la subred tienen algunos [límites y restricciones](sql-database-managed-instance-resource-limits.md#strategies-for-deploying-mixed-general-purpose-and-business-critical-instances)
- Hay algunos [reglas de seguridad que se deben aplicar en la subred](sql-database-managed-instance-connectivity-architecture.md#network-requirements).

### <a name="vnet"></a>VNET
- Red virtual puede implementarse mediante el modelo de recursos: no se admite el modelo clásico de red virtual.
- Algunos servicios, como entornos de App Service, Logic apps y las instancias administradas (se usa para la replicación geográfica, replicación transaccional, o a través de servidores vinculados) no pueden acceder a las instancias administradas en diferentes regiones si sus redes virtuales están conectadas mediante [emparejamiento global](../virtual-network/virtual-networks-faq.md#what-are-the-constraints-related-to-global-vnet-peering-and-load-balancers). Puede conectarse a estos recursos a través de ExpressRoute o redes a través de las puertas de enlace de red virtual.

## <a name="Changes"></a> Cambios de comportamiento

Las siguientes variables, funciones y vistas devuelven resultados diferentes:

- `SERVERPROPERTY('EngineEdition')` Devuelve el valor 8. Esta propiedad identifica de forma única una instancia administrada. Consulte [SERVERPROPERTY](https://docs.microsoft.com/sql/t-sql/functions/serverproperty-transact-sql).
- `SERVERPROPERTY('InstanceName')` Devuelve NULL porque no existe el concepto de instancia, tal y como para SQL Server no se aplica a una instancia administrada. Consulte [SERVERPROPERTY('InstanceName')](https://docs.microsoft.com/sql/t-sql/functions/serverproperty-transact-sql).
- `@@SERVERNAME` Devuelve un nombre completo "conectable" de DNS, por ejemplo, my-managed-instance.wcus17662feb9ce98.database.windows.net. Consulte [@@SERVERNAME](https://docs.microsoft.com/sql/t-sql/functions/servername-transact-sql). 
- `SYS.SERVERS` Devuelve un nombre completo "conectable" de DNS, como `myinstance.domain.database.windows.net` para las propiedades "name" y "data_source". Consulte [SYS.SERVERS](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-servers-transact-sql).
- `@@SERVICENAME` Devuelve NULL porque no existe el concepto de servicio tal y como para SQL Server no se aplica a una instancia administrada. Consulte [@@SERVICENAME](https://docs.microsoft.com/sql/t-sql/functions/servicename-transact-sql).
- Se admite `SUSER_ID`. Devuelve NULL si el inicio de sesión de Azure AD no está en sys.syslogins. Consulte [SUSER_ID](https://docs.microsoft.com/sql/t-sql/functions/suser-id-transact-sql). 
- `SUSER_SID` no se admite. Devuelven datos incorrectos, que es un problema conocido de temporal. Consulte [SUSER_SID](https://docs.microsoft.com/sql/t-sql/functions/suser-sid-transact-sql). 

## <a name="Issues"></a>Problemas conocidos y limitaciones

### <a name="tempdb-size"></a>Tamaño de TEMPDB

El tamaño máximo del archivo de `tempdb` no puede ser mayor que 24 GB por núcleo en un nivel de uso General. El máximo `tempdb` tamaño en un nivel crítico para la empresa es limitado con el tamaño de almacenamiento de instancia. El `tempdb` base de datos siempre se divide en archivos de 12 datos. No se puede cambiar este tamaño máximo por archivo, y no se puede agregar nuevos archivos a `tempdb`. Algunas consultas podrían devolver un error si necesitan más de 24 GB por núcleo en `tempdb`. `tempdb` es siempre vuelve a crear como base de datos vacía cuando el inicio de instancia o conmutación por error y cualquier cambio realizado en `tempdb` no se conservará. 

### <a name="cant-restore-contained-database"></a>No se puede restaurar la base de datos independiente

No se puede restaurar la instancia administrada [bases de datos independientes](https://docs.microsoft.com/sql/relational-databases/databases/contained-databases). No funciona en el momento restauración de las bases de datos independientes existentes en la instancia administrada. Pronto se resolverá este problema. Mientras tanto, se recomienda que quite la opción de contención de las bases de datos que se colocan en la instancia administrada. No use la opción de contención para las bases de datos de producción. 

### <a name="exceeding-storage-space-with-small-database-files"></a>Exceder el espacio de almacenamiento con archivos de base de datos pequeños

`CREATE DATABASE`, `ALTER DATABASE ADD FILE`, y `RESTORE DATABASE` instrucciones pueden producir un error porque la instancia puede alcanzar el límite de almacenamiento de Azure.

Cada instancia administrada de propósito General tiene hasta 35 TB de almacenamiento reservado para el espacio de disco Premium de Azure. Cada archivo de base de datos se coloca en un disco físico independiente. Los posibles tamaños de disco son: 128 GB, 256 GB, 512 GB, 1 TB o 4 TB. No se le cobrará el espacio no utilizado en el disco, pero la suma total de los tamaños de disco Premium de Azure no puede superar los 35 TB. En algunos casos, una instancia administrada que no necesita 8 TB en total puede superar los 35 TB Azure Limitar tamaño de almacenamiento debido a la fragmentación interna.

Por ejemplo, una instancia administrada de uso General puede tener un archivo de 1,2 TB de tamaño que se colocan en un disco de 4 TB. También podría tener 248 archivos cada 1 GB de tamaño que se colocan en discos independientes de 128 GB. En este ejemplo:

- El tamaño de almacenamiento total del disco es de 1 x 4 TB + 248 x 128 GB = 35 TB.
- El espacio total reservado para las bases de datos en la instancia es de 1 x 1,2 TB + 248 x 1 GB = 1,4 TB.

En este ejemplo se muestra que en determinadas circunstancias, debido a una distribución específica de archivos, una instancia administrada podría alcanzar el límite de 35 TB que está reservado para un disco de Premium de Azure conectado cuando no puede esperar que.

En este ejemplo, bases de datos existentes seguirán funcionando y puede crecer sin ningún problema siempre y cuando no se agregan nuevos archivos. Nuevas bases de datos no se puede crear o restaurar porque no hay suficiente espacio para nuevas unidades de disco, incluso si el tamaño total de todas las bases de datos no alcanza el límite de tamaño de instancia. El error que se devuelve en ese caso no está claro.

También puede [identificar el número de archivos restantes](https://medium.com/azure-sqldb-managed-instance/how-many-files-you-can-create-in-general-purpose-azure-sql-managed-instance-e1c7c32886c1) mediante el uso de vistas del sistema. Si se alcanza este límite, intenta [vacíos y eliminar algunos de los archivos más pequeños mediante la instrucción DBCC SHRINKFILE](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-shrinkfile-transact-sql#d-emptying-a-file) o cambie a la [nivel crítico para la empresa, que no tiene este límite](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-resource-limits#service-tier-characteristics).

### <a name="incorrect-configuration-of-the-sas-key-during-database-restore"></a>Restaurar la configuración incorrecta de la clave SAS durante la base de datos

`RESTORE DATABASE` que lee el archivo .bak podría estar intentando para leer constantemente el .bak de archivos y devolver un error tras un largo período de tiempo si la firma de acceso compartido en `CREDENTIAL` es incorrecta. Ejecute RESTORE HEADERONLY antes de restaurar una base de datos para asegurarse de que la clave SAS es correcta.
Asegúrese de que quita el interlineado `?` de la clave SAS que se genera mediante el portal de Azure.

### <a name="tooling"></a>Herramientas

SQL Server Management Studio y SQL Server Data Tools podrían tener algunos problemas, mientras que acceden a una instancia administrada.

- No se admite el uso de entidades de seguridad del servidor de Azure AD (inicios de sesión) y los usuarios (versión preliminar pública) con SQL Server Data Tools actualmente.
- Secuencias de comandos para entidades de seguridad del servidor de Azure AD (inicios de sesión) y los usuarios (versión preliminar) no se admiten en SQL Server Management Studio.

### <a name="incorrect-database-names-in-some-views-logs-and-messages"></a>Nombres incorrectos de base de datos en algunas vistas, registros y mensajes

Varias vistas del sistema, contadores de rendimiento, mensajes de error, XEvents y entradas de registro de errores muestran identificadores de base de datos GUID en lugar de los nombres reales de base de datos. No se basan en estos identificadores GUID porque se reemplazan con los nombres de base de datos real en el futuro.

### <a name="database-mail"></a>Correo electrónico de base de datos

El `@query` parámetro en el [sp_send_db_mail](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-send-dbmail-transact-sql) procedimiento no funciona.

### <a name="database-mail-profile"></a>Perfil de correo electrónico de base de datos

El perfil de correo electrónico de base de datos utilizado por el Agente SQL Server debe llamarse `AzureManagedInstance_dbmail_profile`. No hay ninguna restricción para otros nombres de perfil de correo electrónico de base de datos.

### <a name="error-logs-arent-persisted"></a>No se conservan los registros de errores

No se conservan los registros de errores que están disponibles en instancia administrada, y su tamaño no se incluye en el límite máximo de almacenamiento. Los registros de errores es posible que se borran automáticamente si se produce la conmutación por error.

### <a name="error-logs-are-verbose"></a>Los registros de errores son detallados

Una instancia administrada coloca información detallada en los registros de errores, y gran parte de él no es relevante. La cantidad de información en los registros de errores se reducirá en el futuro.

**Solución alternativa**: Usar un procedimiento personalizado para leer los registros de errores que filtra algunas entradas irrelevantes. Para obtener más información, consulte [instancia administrada: sp_readmierrorlog](https://blogs.msdn.microsoft.com/sqlcat/2018/05/04/azure-sql-db-managed-instance-sp_readmierrorlog/).

### <a name="transaction-scope-on-two-databases-within-the-same-instance-isnt-supported"></a>No se admite el ámbito de transacción en dos bases de datos en la misma instancia

La `TransactionScope` clase en .NET no funciona si dos consultas se envían a dos bases de datos en la misma instancia en el mismo ámbito de transacción:

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

Aunque este código funciona con datos en la misma instancia, requiere MSDTC.

**Solución alternativa**: Usar [SqlConnection.ChangeDatabase(String)](https://docs.microsoft.com/dotnet/api/system.data.sqlclient.sqlconnection.changedatabase) para usar otra base de datos en un contexto de conexión en lugar de usar dos conexiones.

### <a name="clr-modules-and-linked-servers-sometimes-cant-reference-a-local-ip-address"></a>Módulos CLR y a veces, los servidores vinculados no se pueden hacer referencia a una dirección IP local

Módulos CLR colocados en una instancia administrada y servidores vinculados o consultas distribuidas que hacen referencia a una instancia actual a veces no pueden resolver la dirección IP de una instancia local. Este error es un problema transitorio.

**Solución alternativa**: Utilizar conexiones de contexto en un módulo CLR si es posible.

### <a name="tde-encrypted-databases-with-a-service-managed-key-dont-support-user-initiated-backups"></a>Las bases de datos tenga cifrado TDE con una clave administrada por el servicio no son compatibles con las copias de seguridad iniciadas por el usuario

No se puede ejecutar `BACKUP DATABASE ... WITH COPY_ONLY` en una base de datos se cifra con administrados por el servicio de datos cifrado transparente (TDE). TDE administrado por un servicio hace que las copias de seguridad se cifre con una clave interna de TDE. No se puede exportar la clave, por lo que no se puede restaurar la copia de seguridad.

**Solución alternativa**: Usar copias de seguridad automáticas y restauración en el momento, o usar [administrada por el cliente (BYOK) TDE](https://docs.microsoft.com/azure/sql-database/transparent-data-encryption-azure-sql#customer-managed-transparent-data-encryption---bring-your-own-key) en su lugar. También puede deshabilitar el cifrado en la base de datos.

## <a name="next-steps"></a>Pasos siguientes

- Para obtener más información acerca de las instancias administradas, vea [¿qué es una instancia administrada?](sql-database-managed-instance.md)
- Para una de las características y la lista de comparación, consulte [comparación de características de Azure SQL Database](sql-database-features.md).
- Guía de inicio rápido que muestra cómo crear una nueva instancia administrada, consulte [crear una instancia administrada](sql-database-managed-instance-get-started.md).
