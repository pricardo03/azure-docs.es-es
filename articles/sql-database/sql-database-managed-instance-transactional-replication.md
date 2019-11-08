---
title: Replicación transaccional
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
ms.date: 02/08/2019
ms.openlocfilehash: a57d1c85384204c26e75f7138b9514f2b3297bef
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2019
ms.locfileid: "73823309"
---
# <a name="transactional-replication-with-single-pooled-and-instance-databases-in-azure-sql-database"></a>Replicación transaccional con bases de datos únicas, agrupadas y de instancia en Azure SQL Database

La replicación transaccional es una característica de Azure SQL Database y SQL Server que permite replicar datos de una tabla de Azure SQL Database o SQL Server en tablas de bases de datos remotas. Esta característica permite sincronizar varias tablas en bases de datos diferentes.

## <a name="when-to-use-transactional-replication"></a>Cuándo se usa la replicación transaccional

La replicación transaccional resulta útil en los siguientes escenarios:
- Publique los cambios realizados en una o varias tablas de una base de datos y distribúyalos por una o varias de las bases de datos de SQL Server o Azure SQL que se suscribieron para los cambios.
- Mantenga varias bases de datos distribuidas en estado sincronizado.
- Migre las bases de datos de una instancia administrada o de SQL Server a otra base de datos mediante la publicación continua de los cambios.

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

El **distribuidor** es una instancia o un servidor que recopila los cambios en los artículos de un publicador y los distribuye a los suscriptores. El distribuidor puede ser la instancia administrada de Azure SQL Database o SQL Server (cualquier versión, siempre que sea igual o superior que la versión del publicador). 

El **suscriptor** es una instancia o un servidor que recibe los cambios realizados en el publicador. Los suscriptores pueden ser bases de datos únicas, agrupadas o de instancia en bases de datos de Azure SQL Database o SQL Server. Un suscriptor en una base de datos única o agrupada debe configurarse como suscriptor de inserción. 

| Role | Bases de datos únicas y agrupadas | Bases de datos de instancia |
| :----| :------------- | :--------------- |
| **Publicador** | Sin | Sí | 
| **Distribuidor** | Sin | Sí|
| **Suscriptor de extracción** | Sin | Sí|
| **Suscriptor de inserción**| Sí | Sí|
| &nbsp; | &nbsp; | &nbsp; |

  >[!NOTE]
  > No se admite una suscripción de extracción cuando el distribuidor es una base de datos de instancia y el suscriptor no lo es. 

Existen distintos [tipos de replicación](https://docs.microsoft.com/sql/relational-databases/replication/types-of-replication):


| Replicación | Bases de datos únicas y agrupadas | Bases de datos de instancia|
| :----| :------------- | :--------------- |
| [**Transaccional estándar**](https://docs.microsoft.com/sql/relational-databases/replication/transactional/transactional-replication) | Sí (solo como suscriptor) | Sí | 
| [**Instantánea**](https://docs.microsoft.com/sql/relational-databases/replication/snapshot-replication) | Sí (solo como suscriptor) | Sí|
| [**Replicación de mezcla**](https://docs.microsoft.com/sql/relational-databases/replication/merge/merge-replication) | Sin | Sin|
| [**Punto a punto**](https://docs.microsoft.com/sql/relational-databases/replication/transactional/peer-to-peer-transactional-replication) | Sin | Sin|
| [**Bidireccional**](https://docs.microsoft.com/sql/relational-databases/replication/transactional/bidirectional-transactional-replication) | Sin | Sí|
| [**Suscripciones actualizables**](https://docs.microsoft.com/sql/relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication) | Sin | Sin|
| &nbsp; | &nbsp; | &nbsp; |

  >[!NOTE]
  > - Al intentar configurar la replicación con una versión anterior, puede producirse el error número MSSQL_REPL20084 (El proceso no pudo conectarse al suscriptor) y MSSQ_REPL40532 (No se puede abrir el servidor \<nombre> solicitado por el inicio de sesión. Error de inicio de sesión).
  > - Para usar todas las características de Azure SQL Database, debe usar las versiones más recientes de [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) y [SQL Server Data Tools (SSDT)](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt).
  
  ### <a name="supportability-matrix-for-instance-databases-and-on-premises-systems"></a>Matriz de compatibilidad para bases de datos de instancia y sistemas locales
  La matriz de compatibilidad de replicación para bases de datos de instancia es la misma que para SQL Server local. 
  
  | **Publicador**   | **Distribuidor** | **Suscriptor** |
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
- El puerto 445 (salida TCP) debe estar abierto en las reglas de seguridad de la subred de la instancia administrada para acceder al recurso compartido de archivos de Azure. 
- El puerto 1433 (salida TCP) debe estar abierto si el publicador o distribuidor se encuentran en una instancia administrada y el suscriptor es local.


>[!NOTE]
> - Podría producirse el error 53 al conectarse a un archivo de Azure Storage si el puerto 445 del grupo de seguridad de red (NSG) saliente está bloqueado cuando el distribuidor es una base de datos de instancia y el suscriptor es local. [Actualice el NSG de la red virtual](/azure/storage/files/storage-troubleshoot-windows-file-connection-problems) para resolver este problema. 
> - Si las bases de datos del publicador y del distribuidor de una instancia administrada usan [grupos de conmutación por error automática](sql-database-auto-failover-group.md), el administrador de la instancia administrada debe [eliminar todas las publicaciones de la base de datos principal anterior y volver a configurarlas en la nueva principal después de producirse una conmutación por error](sql-database-managed-instance-transact-sql-information.md#replication).

### <a name="compare-data-sync-with-transactional-replication"></a>Comparación de Data Sync con replicación transaccional

| | Sincronización de datos | Replicación transaccional |
|---|---|---|
| Ventajas | - Compatibilidad activo-activo<br/>- Bidireccional entre el entorno local y Azure SQL Database | - Menor latencia<br/>- Coherencia transaccional<br/>- Reutilización de la topología existente después de la migración |
| Desventajas | - 5 minutos o más de latencia<br/>- Sin coherencia transaccional<br/>- Mayor impacto en el rendimiento | - No se puede publicar desde una base de datos única ni agrupada de Azure SQL Database<br/>- Alto costo de mantenimiento |
| | | |

## <a name="common-configurations"></a>Configuraciones comunes

En general, el publicador y el distribuidor deben estar en la nube o en el entorno local. Se admiten las siguientes configuraciones: 

### <a name="publisher-with-local-distributor-on-a-managed-instance"></a>Publicador con distribuidor local en una instancia administrada

![Instancia única como publicador y distribuidor](media/replication-with-sql-database-managed-instance/01-single-instance-asdbmi-pubdist.png)

El publicador y el distribuidor se configuran dentro de una única instancia administrada y los cambios se distribuyen a otra instancia administrada, a una base de datos única, una base de datos agrupada o una instancia local de SQL Server. 

### <a name="publisher-with-remote-distributor-on-a-managed-instance"></a>Publicador con distribuidor remoto en una instancia administrada

En esta configuración, una instancia administrada publica los cambios al distribuidor que se encuentra en otra instancia administrada, que puede atender muchas instancias administradas de origen y distribuir los cambios a uno o varios destinos en una instancia administrada, base de datos única, base de datos agrupada o instancia de SQL Server.

![Instancias independientes para el publicador y el distribuidor](media/replication-with-sql-database-managed-instance/02-separate-instances-asdbmi-pubdist.png)

El publicador y el distribuidor se configuran en dos Instancias administradas. Esta configuración presenta algunas restricciones: 

- Las dos instancias administradas están en la misma red virtual.
- Las dos Instancias administradas están en la misma ubicación.


### <a name="publisher-and-distributor-on-premises-with-a-subscriber-on-a-single-pooled-and-instance-database"></a>Publicador y distribuidor locales con un suscriptor en una base de datos única, agrupada o de instancia 

![Base de datos de Azure SQL como suscriptor](media/replication-with-sql-database-managed-instance/03-azure-sql-db-subscriber.png)
 
En esta configuración, el suscriptor es una base de datos de Azure SQL (base de datos única, agrupada o de instancia). Esta configuración admite la migración desde el entorno local a Azure. Si un suscriptor está en una base de datos única o agrupada, debe estar en modo de inserción.  


## <a name="next-steps"></a>Pasos siguientes

1. [Configure la replicación entre dos instancias administradas](replication-with-sql-database-managed-instance.md). 
1. [Cree una publicación](https://docs.microsoft.com/sql/relational-databases/replication/publish/create-a-publication).
1. [Cree una suscripción de inserción](https://docs.microsoft.com/sql/relational-databases/replication/create-a-push-subscription) mediante el nombre del servidor de Azure SQL Database como suscriptor (por ejemplo, `N'azuresqldbdns.database.windows.net`) y el nombre de Azure SQL Database como base de datos de destino (por ejemplo, **AdventureWorks**. )
1. Obtenga información sobre las [limitaciones de la replicación transaccional para una instancia administrada](sql-database-managed-instance-transact-sql-information.md#replication).



## <a name="see-also"></a>Otras referencias  

- [Replicación con una instancia administrada y un grupo de conmutación por error](sql-database-managed-instance-transact-sql-information.md#replication)
- [Replicación en SQL Database](replication-to-sql-database.md)
- [Replicación en una instancia administrada](replication-with-sql-database-managed-instance.md)
- [Create a Publication](https://docs.microsoft.com/sql/relational-databases/replication/publish/create-a-publication) (Creación de una publicación)
- [Create a Push Subscription](https://docs.microsoft.com/sql/relational-databases/replication/create-a-push-subscription/) (Creación de una suscripción de inserción)
- [Tipos de replicación](https://docs.microsoft.com/sql/relational-databases/replication/types-of-replication)
- [Supervisión (replicación)](https://docs.microsoft.com/sql/relational-databases/replication/monitor/monitoring-replication)
- [Inicializar una suscripción](https://docs.microsoft.com/sql/relational-databases/replication/initialize-a-subscription)  
