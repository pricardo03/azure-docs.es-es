---
title: Replicación en Azure SQL Database | Microsoft Docs
description: Más información acerca del uso de la replicación de SQL Server con bases de datos únicas de Azure SQL Database en bases de datos de grupos elásticos
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
ms.openlocfilehash: 360aa53a02beabccc685df97ad8549dfb88ff5c8
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2018
ms.locfileid: "47228288"
---
# <a name="replication-to-sql-database-single-and-pooled-databases"></a>Replicación en base de datos agrupadas y únicas de SQL Database

La replicación de SQL Server puede configurarse para bases de datos únicas y agrupadas en un [servidor lógico](sql-database-logical-servers.md) de Azure SQL Database.  
 
 ### <a name="supported-configurations"></a>**Configuraciones admitidas:**  
 -  SQL Server puede ser una instancia de SQL Server en ejecución en el entorno local o una instancia de SQL Server en ejecución en una máquina virtual de Azure en la nube. Para obtener más información, consulte [Introducción a SQL Server en máquinas virtuales de Azure](https://azure.microsoft.com/documentation/articles/virtual-machines-sql-server-infrastructure-services/).  
 - Azure SQL Database debe ser un suscriptor de inserción de un publicador de SQL Server.  
 -  La base de datos de distribución y los agentes de replicación no pueden colocarse en Azure SQL Database.  
 - Se admiten replicaciones transaccionales unidireccionales y de instantáneas. La replicación transaccional punto a punto y la replicación de mezcla no se admiten.
 - La replicación está disponible para su versión preliminar en Instancia administrada de Azure SQL Database. Instancia administrada puede hospedar las bases de datos del publicador, distribuidor y suscriptor. Para más información, consulte [Replication with SQL Database Managed Instance](replication-with-sql-database-managed-instance.md) (Replicación con Instancia administrada de Azure SQL Database).
 
## <a name="versions"></a>Versiones  
- El publicador y el distribuidor deben pertenecer al menos a una de las siguientes versiones:  
 - SQL Server 2017 (14.x) 
 - SQL Server 2016 (13.x) 
 - SQL Server 2014 (12.x) SP1 CU3 
 - SQL Server 2014 (12.x) RTM CU10 
 - SQL Server 2012 (11.x) SP2 CU8 o SP3 
   
- Al intentar configurar la replicación con una versión anterior, puede producirse el error número MSSQL_REPL20084 (El proceso no pudo conectarse al suscriptor) y MSSQL_REPL40532 (No se puede abrir el servidor \<name> solicitado por el inicio de sesión. Error de inicio de sesión).  
- Para usar todas las características de Azure SQL Database, debe usar las versiones más recientes de [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) y [SQL Server Data Tools](https://docs.microsoft.com//sql/ssdt/download-sql-server-data-tools-ssdt?view=sql-server-2017).  
   
 ## <a name="remarks"></a>Comentarios  
- La replicación puede configurarse mediante el uso de [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) o mediante la ejecución de instrucciones Transact-SQL en el publicador. No se puede configurar la replicación mediante Azure Portal.  
- La replicación solo puede usar inicios de sesión de autenticación de SQL Server para conectarse a Azure SQL Database. 
- Las tablas replicadas deben tener una clave principal.  
- Debe tener una suscripción de Azure existente.  
- El suscriptor de Azure SQL Database puede estar en cualquier región.  
- Una sola publicación en SQL Server puede admitir los suscriptores de SQL Server (entorno local y SQL Server en una máquina virtual de Azure) y Azure SQL Database.  
- La administración, la supervisión y la solución de problemas de la replicación deben realizarse desde SQL Server local.  
- Azure SQL Database solo admite las suscripciones de inserción.  
- Solo se admite `@subscriber_type = 0` en **sp_addsubscription** para SQL Database.  
- Azure SQL Database no admite replicaciones bidireccionales, inmediatas, actualizables o de punto a punto.      
   
 ## <a name="replication-architecture"></a>Arquitectura de replicación  
 ![replication-to-sql-database](./media/replication-to-sql-database/replication-to-sql-database.png)  
   
 ## <a name="scenarios"></a>Escenarios  
   
 ### <a name="typical-replication-scenario"></a>Escenario típico de replicación  
   
 1.  Cree una publicación de replicación transaccional en una base de datos de SQL Server local.  
   
 2.  En la instancia de SQL Server local, use el **Asistente para nueva suscripción** o las instrucciones Transact-SQL para crear una inserción en la suscripción de Azure SQL Database.  
   
 3.  El conjunto de datos inicial es normalmente una instantánea que crea el Agente de instantáneas y distribuye y aplica el Agente de distribución. El conjunto de datos inicial también se puede suministrar a través de una copia de seguridad u otros medios, como SQL Server Integration Services.  
   
 ### <a name="data-migration-scenario"></a>Escenario de migración de datos  
   
 1.  Utilice la replicación transaccional para replicar datos desde una base de datos de SQL Server local a Azure SQL Database.  
   
 2.  Redirija al cliente o a las aplicaciones de nivel intermedio para actualizar la copia de Azure SQL Database.  
   
 3.  Deje de actualiza la versión de SQL Server de la tabla y quite la publicación.  
   
## <a name="limitations"></a>Limitaciones  
Las siguientes opciones no se admiten para las suscripciones de Azure SQL Database:  
-   Copiar asociaciones de grupos de archivos  
-   Copiar esquemas de partición de tabla  
-   Copiar esquemas de partición de índice  
-   Copiar estadísticas definidas por el usuario  
-   Copiar enlaces predeterminados  
-   Copiar enlaces de reglas  
-   Copiar índices de texto completo  
-   Copiar XML XSD  
-   Copiar índices XML  
-   Copiar permisos  
-   Copiar índices espaciales  
-   Copiar índices filtrados  
-   Copiar atributos de compresión de datos  
-   Copiar atributos de columna dispersa  
-   Convertir una secuencia de archivos en tipos de datos MAX  
-   Convertir HierarchyId en tipos de datos MAX  
-   Convertir una instancia espacial en tipos de datos MAX  
-   Copiar propiedades extendidas  
-   Copiar permisos  
   
### <a name="limitations-to-be-determined"></a>Limitaciones que se deben determinar 
-   Copiar intercalaciones  
-   La ejecución en una transacción serializada de SP  
   
## <a name="examples"></a>Ejemplos  
Cree una publicación y una suscripción de inserción. Para más información, consulte:  
-   [Create a Publication](https://docs.microsoft.com/sql/relational-databases/replication/publish/create-a-publication) (Creación de una publicación)  
   
-   [Create a Push Subscription](https://docs.microsoft.com/sql/relational-databases/replication/create-a-push-subscription/create-a-push-subscription) (Creación de una suscripción de inserción) mediante el nombre del servidor lógico de Azure SQL Database como suscriptor (por ejemplo, **N'azuresqldbdns.database.windows.net'**) y el nombre de Azure SQL Database como la base de datos de destino (por ejemplo, **AdventureWorks**).  
   
 ## <a name="see-also"></a>Otras referencias  
 - [Create a Publication](https://docs.microsoft.com/sql/relational-databases/replication/publish/create-a-publication) (Creación de una publicación)   
 - [Create a Push Subscription](https://docs.microsoft.com/sql/relational-databases/replication/create-a-push-subscription/create-a-push-subscription) (Creación de una suscripción de inserción)   
 - [Tipos de replicación](https://docs.microsoft.com/sql/relational-databases/replication/types-of-replication)   
 - [Supervisar (replicación)](https://docs.microsoft.com/sql/relational-databases/replication/monitor/monitoring-replication)   
 - [Inicializar una suscripción](https://docs.microsoft.com/sql/relational-databases/replication/initialize-a-subscription)  
