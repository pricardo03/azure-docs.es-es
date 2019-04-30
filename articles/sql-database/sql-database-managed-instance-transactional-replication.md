---
title: Replicación transaccional con Azure SQL Database | Microsoft Docs
description: Aprenda a usar la replicación transaccional de SQL Server con bases de datos únicas, agrupadas y de instancia en Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.reviewer: carlrab
manager: craigg
ms.date: 02/08/2019
ms.openlocfilehash: 409c1abd7e9f532bb243ecab00228b402215c77e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61313691"
---
# <a name="transactional-replication-with-single-pooled-and-instance-databases-in-azure-sql-database"></a>Replicación transaccional con bases de datos únicas, agrupadas y de instancia en Azure SQL Database

La replicación transaccional es una característica de Azure SQL Database y SQL Server que permite replicar datos de una tabla de Azure SQL Database o SQL Server en tablas de bases de datos remotas. Esta característica permite sincronizar varias tablas en bases de datos diferentes.

## <a name="when-to-use-transactional-replication"></a>Cuándo se usa la replicación transaccional

La replicación transaccional resulta útil en los siguientes escenarios:
- Publique los cambios realizados en una o varias tablas de una base de datos y distribúyalos por una o varias de las bases de datos de SQL Server o Azure SQL que se suscribieron para los cambios.
- Mantenga varias bases de datos distribuidas en estado sincronizado.
- Migre las bases de datos de SQL Server o Instancia administrada a otra base de datos mediante la publicación continua de los cambios.

## <a name="overview"></a>Información general

Los componentes clave de la replicación transaccional se muestran en la siguiente imagen:  

![replicación con SQL Database](media/replication-to-sql-database/replication-to-sql-database.png)

El **publicador** es una instancia o un servidor que publica los cambios realizados en algunas tablas (artículos) mediante el envío de las actualizaciones al distribuidor. La publicación en instancias de Azure SQL Database por parte de servidores de SQL Server locales se admite en las siguientes versiones de SQL Server:

- SQL Server 2019 (versión preliminar)
- SQL Server 2016 a SQL 2017
- SQL Server 2014 SP1 CU3 o superior (12.00.4427)
- SQL Server 2014 RTM CU10 (12.00.2556)
- SQL Server 2012 SP3 o superior (11.0.6020)
- SQL Server 2012 SP2 CU8 (11.0.5634.0)
- Para otras versiones de SQL Server que no admiten la publicación en los objetos de Azure e puede utilizar el método de [volver a publicar datos](https://docs.microsoft.com/sql/relational-databases/replication/republish-data) para mover datos a versiones más recientes de SQL Server. 

El **distribuidor** es una instancia o un servidor que recopila los cambios en los artículos de un publicador y los distribuye a los suscriptores. El distribuidor puede ser la Instancia administrada de Azure SQL Database o SQL Server (cualquier versión, siempre que sea igual o superior que la versión del publicador). 

El **suscriptor** es una instancia o un servidor que recibe los cambios realizados en el publicador. Los suscriptores pueden ser bases de datos únicas, agrupadas o de instancia en bases de datos de Azure SQL Database o SQL Server. Un suscriptor en una base de datos única o agrupada debe configurarse como suscriptor de inserción. 

| Rol | Bases de datos únicas y agrupadas | Bases de datos de instancia |
| :----| :------------- | :--------------- |
| **Publicador** | Sin  | Sí | 
| **Distribuidor** | Sin  | Sí|
| **Suscriptor de extracción** | Sin  | Sí|
| **Suscriptor de inserción**| Sí | Sí|
| &nbsp; | &nbsp; | &nbsp; |

  >[!NOTE]
  > Una suscripción de extracción no se admite cuando el distribuidor es una base de datos de instancia y el suscriptor no lo es. 

Existen distintos [tipos de replicación](https://docs.microsoft.com/sql/relational-databases/replication/types-of-replication):


| Replicación | Bases de datos únicas y agrupadas | Bases de datos de instancia|
| :----| :------------- | :--------------- |
| [**Transaccional**](https://docs.microsoft.com/sql/relational-databases/replication/transactional/transactional-replication) | Sí (solo como suscriptor) | Sí | 
| [**Instantánea**](https://docs.microsoft.com/sql/relational-databases/replication/snapshot-replication) | Sí (solo como suscriptor) | Sí|
| [**Replicación de mezcla**](https://docs.microsoft.com/sql/relational-databases/replication/merge/merge-replication) | Sin  | Sin |
| [**Punto a punto**](https://docs.microsoft.com/sql/relational-databases/replication/transactional/peer-to-peer-transactional-replication) | Sin  | Sin |
| **Unidireccional** | Sí | Sí|
| [**Bidireccional**](https://docs.microsoft.com/sql/relational-databases/replication/transactional/bidirectional-transactional-replication) | Sin  | Sí|
| [**Suscripciones actualizables**](https://docs.microsoft.com/sql/relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication) | Sin  | Sin |
| &nbsp; | &nbsp; | &nbsp; |

  >[!NOTE]
  > - Al intentar configurar la replicación con una versión anterior, puede producirse el error número MSSQL_REPL20084 (El proceso no pudo conectarse al suscriptor) y MSSQ_REPL40532 (No se puede abrir el servidor \<nombre> solicitado por el inicio de sesión. Error de inicio de sesión).
  > - Para usar todas las características de Azure SQL Database, debe usar las versiones más recientes de [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) y [SQL Server Data Tools (SSDT)](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt).
  
  ### <a name="supportabilty-matrix-for-instance-databases-and-on-premises-systems"></a>Matriz de Supportabilty para sistemas de bases de datos de instancia y en el entorno local
  La matriz de compatibilidad de replicación por ejemplo, las bases de datos es el mismo que el de SQL Server local. 
  
  | **Publicador**   | **Distribuidor** | **suscriptor** |
| :------------   | :-------------- | :------------- |
| SQL Server 2017 | SQL Server 2017 | SQL Server 2017 <br/> SQL Server 2016 <br/> SQL Server 2014 |
| SQL Server 2016 | SQL Server 2017 <br/> SQL Server 2016 | SQL Server 2017 <br/>SQL Server 2016 <br/> SQL Server 2014 <br/> SQL Server 2012 |
| SQL Server 2014 | SQL Server 2017 <br/> SQL Server 2016 <br/> SQL Server 2014 <br/>| SQL Server 2017 <br/> SQL Server 2016 <br/> SQL Server 2014 <br/> SQL Server 2012 <br/> SQL Server 2008 R2 <br/> SQL Server 2008 |
| SQL Server 2012 | SQL Server 2017 <br/> SQL Server 2016 <br/> SQL Server 2014 <br/>SQL Server 2012 <br/> | SQL Server 2016 <br/> SQL Server 2014 <br/> SQL Server 2012 <br/> SQL Server 2008 R2 <br/> SQL Server 2008 | 
| SQL Server 2008 R2 <br/> SQL Server 2008 | SQL Server 2017 <br/> SQL Server 2016 <br/> SQL Server 2014 <br/>SQL Server 2012 <br/> SQL Server 2008 R2 <br/> SQL Server 2008 | SQL Server 2014 <br/> SQL Server 2012 <br/> SQL Server 2008 R2 <br/> SQL Server 2008 <br/>  |
| &nbsp; | &nbsp; | &nbsp; |

## <a name="requirements"></a>Requisitos

- La conectividad usa la autenticación de SQL entre los participantes de la replicación. 
- Un recurso compartido de cuenta de Azure Storage para el directorio de trabajo empleado para la replicación. 
- El puerto 445 (salida TCP) debe estar abierto en las reglas de seguridad de la subred de instancia administrada para acceder al recurso compartido de archivos de Azure. 
- El puerto 1433 (salida TCP) debe estar abierto si el publicador o distribuidor se encuentran en una instancia administrada y el suscriptor es local.

  >[!NOTE]
  > Puede encontrar el error 53 al conectarse a un archivo de almacenamiento de Azure si el puerto 445 del grupo (NSG) de seguridad de red saliente está bloqueado cuando el distribuidor es una base de datos de instancia y el suscriptor es local. [Actualice la red virtual NSG](/azure/storage/files/storage-troubleshoot-windows-file-connection-problems) para resolver este problema. 

### <a name="compare-data-sync-with-transactional-replication"></a>Comparación de Data Sync con replicación transaccional

| | Sincronización de datos | Replicación transaccional |
|---|---|---|
| Ventajas | - Compatibilidad activo-activo<br/>- Bidireccional entre local y Azure SQL Database | - Menor latencia<br/>- Coherencia transaccional<br/>- Reutilización de la topología existente después de la migración |
| Desventajas | - 5 minutos o más de latencia<br/>- Sin coherencia transaccional<br/>- Mayor impacto en el rendimiento | - No se puede publicar desde una base de datos única ni agrupada de Azure SQL Database<br/>- Alto costo de mantenimiento |
| | | |

## <a name="common-configurations"></a>Configuraciones comunes

En general, el publicador y el distribuidor deben estar en la nube o en el entorno local. Se admiten las siguientes configuraciones: 

### <a name="publisher-with-local-distributor-on-a-managed-instance"></a>Publicador con distribuidor local en una instancia administrada

![Instancia única como publicador y distribuidor](media/replication-with-sql-database-managed-instance/01-single-instance-asdbmi-pubdist.png)

El publicador y el distribuidor se configuran dentro de una única Instancia administrada y los cambios se distribuyen a otra Instancia administrada, a una base de datos única, una base de datos agrupada o una instancia local de SQL Server. En esta configuración, el publicador o distribuidor de la instancia administrada no puede configurarse con [grupos de conmutación por error automática y replicación geográfica](sql-database-auto-failover-group.md).

### <a name="publisher-with-remote-distributor-on-a-managed-instance"></a>Publicador con distribuidor remoto en una instancia administrada

En esta configuración, una Instancia administrada publica los cambios al distribuidor que se encuentra en otra Instancia administrada, que puede atender muchas Instancias administradas de origen y distribuir los cambios a uno o varios destinos en Instancias administradas, bases de datos únicas, bases de datos agrupadas o SQL Server.

![Instancias independientes para el publicador y el distribuidor](media/replication-with-sql-database-managed-instance/02-separate-instances-asdbmi-pubdist.png)

El publicador y el distribuidor se configuran en dos instancias administradas. En esta configuración

- Las dos instancias administradas están en la misma red virtual.
- Las dos instancias administradas están en la misma ubicación.
- Las instancias administradas que hospedan bases de datos de publicador y distribuidor no se pueden [replicar geográficamente mediante grupos de conmutación por error automática](sql-database-auto-failover-group.md).

### <a name="publisher-and-distributor-on-premises-with-a-subscriber-on-a-single-pooled-and-instance-database"></a>Publicador y distribuidor locales con un suscriptor en una base de datos única, agrupada o de instancia 

![Base de datos de Azure SQL como suscriptor](media/replication-with-sql-database-managed-instance/03-azure-sql-db-subscriber.png)
 
En esta configuración, el suscriptor es una base de datos de Azure SQL (base de datos única, agrupada o de instancia). Esta configuración admite la migración desde el entorno local a Azure. Si un suscriptor está en una base de datos única o agrupada, debe estar en modo de inserción.  


## <a name="next-steps"></a>Pasos siguientes

1. [Configure la replicación transaccional para una instancia administrada](replication-with-sql-database-managed-instance.md#configure-publishing-and-distribution-example). 
1. [Cree una publicación](https://docs.microsoft.com/sql/relational-databases/replication/publish/create-a-publication).
1. [Cree una suscripción de inserción](https://docs.microsoft.com/sql/relational-databases/replication/create-a-push-subscription) mediante el nombre del servidor de Azure SQL Database como suscriptor (por ejemplo, `N'azuresqldbdns.database.windows.net`) y el nombre de Azure SQL Database como base de datos de destino (por ejemplo, **AdventureWorks**. )



## <a name="see-also"></a>Vea también  

- [Replicación en SQL Database](replication-to-sql-database.md)
- [Replicación en Instancia administrada](replication-with-sql-database-managed-instance.md)
- [Create a Publication](https://docs.microsoft.com/sql/relational-databases/replication/publish/create-a-publication) (Creación de una publicación)
- [Create a Push Subscription](https://docs.microsoft.com/sql/relational-databases/replication/create-a-push-subscription/) (Creación de una suscripción de inserción)
- [Tipos de replicación](https://docs.microsoft.com/sql/relational-databases/replication/types-of-replication)
- [Supervisión (replicación)](https://docs.microsoft.com/sql/relational-databases/replication/monitor/monitoring-replication)
- [Inicializar una suscripción](https://docs.microsoft.com/sql/relational-databases/replication/initialize-a-subscription)  
