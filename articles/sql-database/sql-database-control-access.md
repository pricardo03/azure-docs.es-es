---
title: Concesión de acceso
description: Obtenga información acerca de cómo conceder acceso a Microsoft Azure SQL Database y Azure Synapse.
services: sql-database
ms.service: sql-database
ms.subservice: security
titleSuffix: Azure SQL Database and Azure Synapse
ms.custom: sql-data-warehouse, seo-lt-2019
ms.devlang: ''
ms.topic: conceptual
author: VanMSFT
ms.author: vanto
ms.reviewer: carlrab
ms.date: 02/06/2020
ms.openlocfilehash: 5142cc941b37cfef7be79e5129b6df7094bfd00e
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/29/2020
ms.locfileid: "78197851"
---
# <a name="azure-sql-database-and-azure-synapse-access-control"></a>Control de acceso a Azure SQL Database y Azure Synapse

Para proporcionar seguridad, Azure [SQL Database](sql-database-technical-overview.md) y [Azure Synapse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) controlan el acceso con reglas de firewall que limitan la conectividad por dirección IP, con mecanismos de autenticación que requieren a los usuarios que demuestren su identidad y con mecanismos de autorización que limitan a los usuarios el acceso a datos y acciones específicos. 

> [!IMPORTANT]
> Para obtener información general de las características de seguridad de SQL Database, consulte [SQL security overview](sql-database-security-overview.md) (Información general acerca de la seguridad de SQL). Para ver un tutorial, consulte [Protección de bases de datos de Azure SQL](sql-database-security-tutorial.md). Para obtener información general sobre las características de seguridad de SQL Analytics en Azure Synapse, consulte la [información general sobre la seguridad de Azure Synapse](../sql-data-warehouse/sql-data-warehouse-overview-manage-security.md).

## <a name="firewall-and-firewall-rules"></a>Firewall y reglas de firewall

Microsoft Azure SQL Database ofrece un servicio de base de datos relacional para Azure y otras aplicaciones basadas en Internet. Para ayudar a proteger los datos, los firewalls impiden todo acceso al servidor de bases de datos, excepto a aquellos equipos a los que haya concedido permiso. Asimismo, otorgan acceso a las bases de datos según la dirección IP de origen de cada solicitud. Para más información, consulte [Introducción a las reglas de firewall de Azure SQL Database](sql-database-firewall-configure.md).

El servicio Azure SQL Database solo está disponible a través del puerto TCP 1433. Para tener acceso a una instancia de SQL Database desde el equipo, asegúrese de que el firewall de equipos cliente permite la comunicación TCP saliente en el puerto TCP 1433. Si no es necesario para otras aplicaciones, bloquee las conexiones entrantes en el puerto TCP 1433. 

Como parte del proceso de conexión, las conexiones de máquinas virtuales de Azure se redirigen a una dirección IP y un puerto diferente, único para cada rol de trabajo. El número de puerto está comprendido en el rango del 11000 al 11999. Para obtener más información sobre los puertos TCP, consulte [Puertos más allá del 1433 para ADO.NET 4.5 y SQL Database2](sql-database-develop-direct-route-ports-adonet-v12.md).

## <a name="authentication"></a>Authentication

SQL Database admite dos tipos de autenticación:

- **Autenticación de SQL**:

  Este método de autenticación utiliza un nombre de usuario y una contraseña. Al crear el servidor de SQL Database para la base de datos, especificó un inicio de sesión de "administrador de servidor" con un nombre de usuario y una contraseña. Con estas credenciales, puede autenticarse en cualquier base de datos en ese servidor como propietario de la base de datos, o "dbo". 
- **Autenticación con Azure Active Directory**:

  Este método de autenticación usa las identidades administradas por Azure Active Directory y es compatible con dominios administrados e integrados. Si desea usar la autenticación de Azure Active Directory, debe crear otro administrador de servidor llamado "administrador de Azure AD" con permiso para administrar usuarios y grupos de Azure AD. Este administrador también puede realizar todas las operaciones de un administrador de servidor normal. Consulte el tutorial [Conectar a la SQL Database mediante la autenticación de Azure Active Directory](sql-database-aad-authentication.md) , para obtener información acerca de cómo crear un administrador de Azure AD y así habilitar la autenticación de Azure Active Directory.

El motor de la base de datos cierra las conexiones que permanecen inactivas durante más de 30 minutos. La conexión debe volver a iniciar sesión para poder usarse. Las conexiones con SQL Database que están constantemente activas requieren volver a ser autorizadas (acción realizada por el motor de la base de datos) al menos cada 10 horas. El motor de la base de datos intenta repetir la autorización con la contraseña que se envió inicialmente y no se requiere ninguna acción del usuario. Por motivos de rendimiento, cuando se restablece una contraseña en SQL Database, la conexión no se vuelve a autenticar, incluso si esta se restablece debido a la agrupación de conexiones. Esto es diferente del comportamiento de SQL Database local. Si la contraseña se ha cambiado desde que se autorizó inicialmente la conexión, es necesario terminar la conexión y establecer una nueva conexión con la nueva contraseña. Los usuarios con el permiso `KILL DATABASE CONNECTION` pueden terminar explícitamente una conexión con SQL Database mediante el uso del comando [KILL](https://docs.microsoft.com/sql/t-sql/language-elements/kill-transact-sql).

Las cuentas de usuario se pueden crear en la base de datos maestra y se pueden conceder permisos en todas las bases de datos del servidor, o se pueden crear en la misma base de datos (se denominan usuarios de bases de datos independientes). Para más información sobre cómo crear y administrar inicios de sesión, consulte [Administración de inicios de sesión](sql-database-manage-logins.md). Para mejorar la portabilidad y escalabilidad, utilice bases de datos independientes. Para más información sobre los usuarios de bases de datos independientes, consulte [Usuarios de bases de datos independientes: llévense sus bases de datos donde quieran](https://docs.microsoft.com/sql/relational-databases/security/contained-database-users-making-your-database-portable), [CREATE USER (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/create-user-transact-sql) y [Bases de datos independientes](https://docs.microsoft.com/sql/relational-databases/databases/contained-databases).

Como procedimiento recomendado, la aplicación debe usar una cuenta dedicada para autenticar. De esta forma, puede limitar los permisos concedidos a la aplicación y reducir los riesgos de actividad malintencionada en caso de que el código de la aplicación sea vulnerable a ataques por inyección de código SQL. Se recomienda crear un [usuario de base de datos independiente](https://docs.microsoft.com/sql/relational-databases/security/contained-database-users-making-your-database-portable), ya que esto permitirá que la aplicación se autentique directamente en la base de datos. 

## <a name="authorization"></a>Authorization

Autorización indica las acciones que pueden realizar los usuarios en una base de datos de Azure SQL, algo que controlan las [pertenencias a roles](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/database-level-roles) y los [permisos de nivel de objeto](https://docs.microsoft.com/sql/relational-databases/security/permissions-database-engine) de las bases de datos de la cuenta de usuario. Como procedimiento recomendado, debe conceder a los usuarios los privilegios mínimos necesarios. La cuenta de administrador de servidor con la que se está conectando forma parte de db_owner, que tiene autoridad para realizar cualquier acción en la base de datos. Guarde esta cuenta para implementar las actualizaciones de los esquemas y otras operaciones de administración. Utilice la cuenta "ApplicationUser" con permisos más limitados para conectarse desde la aplicación a la base de datos con los privilegios mínimos que necesita la aplicación. Para más información, consulte [Administración de inicios de sesión](sql-database-manage-logins.md).

Normalmente, solo los administradores necesitan tener acceso a la base de datos `master`. El acceso rutinario a cada base de datos de usuario debe llevarse a cabo a través de los usuarios de la base de datos independiente que no son administradores creados en cada base de datos. Al utilizar los usuarios de la base de datos independiente, no es necesario que cree inicios de sesión en la base de datos `master`. Para obtener más información, vea [Usuarios de base de datos independiente: hacer que la base de datos sea portátil](https://docs.microsoft.com/sql/relational-databases/security/contained-database-users-making-your-database-portable).

Debe familiarizarse con las siguientes características que pueden utilizarse para limitar o elevar los permisos:

- La [suplantación](https://docs.microsoft.com/dotnet/framework/data/adonet/sql/customizing-permissions-with-impersonation-in-sql-server) y la [firma de módulos](https://docs.microsoft.com/dotnet/framework/data/adonet/sql/signing-stored-procedures-in-sql-server) se pueden usar para elevar los permisos temporalmente de forma segura.
- [seguridad del nivel de fila](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) permite filtrar las filas que puede ver el usuario.
- [enmascaramiento de datos](sql-database-dynamic-data-masking-get-started.md) puede utilizarse para limitar la exposición de información confidencial.
- [procedimientos almacenados](https://docs.microsoft.com/sql/relational-databases/stored-procedures/stored-procedures-database-engine) puede utilizarse para limitar las acciones que se pueden realizar en la base de datos.

## <a name="next-steps"></a>Pasos siguientes

- Para obtener información general de las características de seguridad de SQL Database, consulte [SQL security overview](sql-database-security-overview.md) (Información general acerca de la seguridad de SQL).
- Para más información acerca de las reglas de firewall, consulte [Reglas de firewall](sql-database-firewall-configure.md).
- Para obtener información acerca de los usuarios e inicios de sesión, consulte [Administración de inicios de sesión](sql-database-manage-logins.md). 
- Para analizar la supervisión proactiva, consulte [Auditorías de SQL Database](sql-database-auditing.md) y [Detección de amenazas de SQL Database](sql-database-threat-detection.md).
- Para ver un tutorial, consulte [Protección de bases de datos de Azure SQL](sql-database-security-tutorial.md).
