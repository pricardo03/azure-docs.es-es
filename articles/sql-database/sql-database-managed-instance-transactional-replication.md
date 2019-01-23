---
title: La replicación transaccional con un servidor lógico de SQL Azure e Instancia administrada de Azure SQL | Microsoft Docs
description: Aprenda a usar la replicación transaccional de SQL Server con los servidores lógicos de Azure SQL Database e Instancia administrada de SQL.
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
ms.date: 01/08/2019
ms.openlocfilehash: d94173f9b1940613c26451658b90c956c71876fb
ms.sourcegitcommit: a1cf88246e230c1888b197fdb4514aec6f1a8de2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/16/2019
ms.locfileid: "54353250"
---
# <a name="transactional-replication-with-azure-sql-logical-server-and-azure-sql-managed-instance"></a>La replicación transaccional con un servidor lógico de SQL Azure e Instancia administrada de Azure SQL

La replicación transaccional es una característica de Azure SQL Database, Instancia administrada y SQL Server que permite replicar datos de una tabla de Azure SQL Database o SQL Server en tablas de bases de datos remotas. Esta característica permite sincronizar varias tablas en bases de datos diferentes.

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

El **suscriptor** es una instancia o un servidor que recibe los cambios realizados en el publicador. Los suscriptores pueden ser una instancia lógica o una instancia administrada de Azure SQL Database o SQL Server. Un suscriptor en un servidor lógico debe estar configurado como suscriptor de inserción. 

| Rol | Servidor lógico | Instancia administrada |
| :----| :------------- | :--------------- |
| **Publicador** | Sin  | SÍ | 
| **Distribuidor** | Sin  | SÍ|
| **Suscriptor de extracción** | Sin  | SÍ|
| **Suscriptor de inserción**| SÍ | SÍ|
| &nbsp; | &nbsp; | &nbsp; |

Existen distintos [tipos de replicación](https://docs.microsoft.com/sql/relational-databases/replication/types-of-replication?view=sql-server-2017):


| Replicación | Servidor lógico | Instancia administrada |
| :----| :------------- | :--------------- |
| [**Transaccional**](https://docs.microsoft.com/sql/relational-databases/replication/transactional/transactional-replication) | Sí (solo como suscriptor) | SÍ | 
| [**Instantánea**](https://docs.microsoft.com/sql/relational-databases/replication/snapshot-replication) | Sí (solo como suscriptor) | SÍ|
| [**Replicación de mezcla**](https://docs.microsoft.com/sql/relational-databases/replication/merge/merge-replication) | Sin  | Sin |
| [**Punto a punto**](https://docs.microsoft.com/sql/relational-databases/replication/transactional/peer-to-peer-transactional-replication) | Sin  | Sin |
| **Unidireccional** | SÍ | SÍ|
| [**Bidireccional**](https://docs.microsoft.com/sql/relational-databases/replication/transactional/bidirectional-transactional-replication) | Sin  | SÍ|
| [**Suscripciones actualizables**](https://docs.microsoft.com/sql/relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication) | Sin  | Sin |
| &nbsp; | &nbsp; | &nbsp; |

  >[!NOTE]
  > - Al intentar configurar la replicación con una versión anterior, puede producirse el error número MSSQL_REPL20084 (El proceso no pudo conectarse al suscriptor) y MSSQL_REPL40532 (No se puede abrir el servidor <name> solicitado por el inicio de sesión. Error de inicio de sesión).
  > - Para usar todas las características de Azure SQL Database, debe usar las versiones más recientes de [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017) y [SQL Server Data Tools (SSDT)](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt?view=sql-server-2017).

## <a name="requirements"></a>Requisitos
- La conectividad usa la autenticación de SQL entre los participantes de la replicación. 
- Un recurso compartido de cuenta de Azure Storage para el directorio de trabajo empleado para la replicación. 
- El puerto 445 (salida TCP) debe estar abierto en las reglas de seguridad de la subred de Instancia administrada para acceder al recurso compartido de archivos de Azure. 
- El puerto 1433 (salida TCP) debe estar abierto si el publicador o distribuidor se encuentran en una instancia administrada y el suscriptor es local. 

## <a name="common-configurations"></a>Configuraciones comunes
En general, el publicador y el distribuidor deben estar en la nube o en el entorno local. Se admiten las siguientes configuraciones: 

### <a name="publisher-with-local-distributor-on-a-managed-instance"></a>Publicador con distribuidor local en una instancia administrada

![Instancia única como publicador y distribuidor ](media/replication-with-sql-database-managed-instance/01-single-instance-asdbmi-pubdist.png)

El publicador y el distribuidor se configuran dentro de una sola instancia administrada y los cambios se distribuyen a otra instancia administrada, a una base de datos única o a una instancia de SQL Server local. En esta configuración, el publicador o distribuidor de la instancia administrada no puede configurarse con [grupos de conmutación por error automática y replicación geográfica](sql-database-auto-failover-group.md).

### <a name="publisher-with-remote-distributor-on-a-managed-instance"></a>Publicador con distribuidor remoto en una instancia administrada

En esta configuración, una instancia administrada publica los cambios al distribuidor que se encuentra en otra instancia administrada, que a su vez puede atender muchas instancias administradas de origen y distribuir los cambios a una o varias instancias administradas, a una base de datos única o a SQL Server.

![Instancias independientes para el publicador y el distribuidor](media/replication-with-sql-database-managed-instance/02-separate-instances-asdbmi-pubdist.png)

El publicador y el distribuidor se configuran en dos instancias administradas. En esta configuración

- Las dos instancias administradas están en la misma red virtual.
- Las dos instancias administradas están en la misma ubicación.
- Las instancias administradas que hospedan bases de datos de publicador y distribuidor no se pueden [replicar geográficamente mediante grupos de conmutación por error automática](sql-database-auto-failover-group.md).

### <a name="publisher-and-distributor-on-premises-with-a-subscriber-on-a-managed-instance-or-logical-server"></a>Publicador y distribuidor locales con un suscriptor en una instancia administrada o un servidor lógico 

![Base de datos de Azure SQL como suscriptor](media/replication-with-sql-database-managed-instance/03-azure-sql-db-subscriber.png)
 
En esta configuración, el suscriptor es una base de datos de Azure SQL (instancia administrada o servidor lógico). Esta configuración admite la migración desde el entorno local a Azure. Si un suscriptor se encuentra en un servidor lógico, debe encontrarse en modo de inserción.  

## <a name="next-steps"></a>Pasos siguientes
1. [Configure la replicación transaccional para una instancia administrada](https://docs.microsoft.com/azure/sql-database/replication-with-sql-database-managed-instance#configure-publishing-and-distribution-example). 
1. [Cree una publicación](https://docs.microsoft.com/sql/relational-databases/replication/publish/create-a-publication).
1. [Cree una suscripción de inserción](https://docs.microsoft.com/sql/relational-databases/replication/create-a-push-subscription) mediante el nombre del servidor lógico de Azure SQL Database como suscriptor (por ejemplo, `N'azuresqldbdns.database.windows.net`) y el nombre de Azure SQL Database como base de datos de destino (por ejemplo, **AdventureWorks**). )


## <a name="see-also"></a>Otras referencias  

- [Replicación en SQL Database](replication-to-sql-database.md)
- [Replicación en Instancia administrada](replication-with-sql-database-managed-instance.md)
- [Create a Publication](https://docs.microsoft.com/sql/relational-databases/replication/publish/create-a-publication) (Creación de una publicación)
- [Create a Push Subscription](https://docs.microsoft.com/sql/relational-databases/replication/create-a-push-subscription/) (Creación de una suscripción de inserción)
- [Tipos de replicación](https://docs.microsoft.com/sql/relational-databases/replication/types-of-replication)
- [Supervisión (replicación)](https://docs.microsoft.com/sql/relational-databases/replication/monitor/monitoring-replication)
- [Inicializar una suscripción](https://docs.microsoft.com/sql/relational-databases/replication/initialize-a-subscription)  
