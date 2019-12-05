---
title: Notas de la versión
description: Obtenga información sobre las nuevas características y mejoras en el servicio Azure SQL Database y en la documentación de Azure SQL Database.
services: sql-database
author: stevestein
ms.service: sql-database
ms.subservice: service
ms.devlang: ''
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: sstein
ms.openlocfilehash: bdea9af04008ef68578ff8c136760f2493a2ae35
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2019
ms.locfileid: "74279230"
---
# <a name="sql-database-release-notes"></a>Notas de la versión de SQL Database

En este artículo se enumeran las características de SQL Database que se encuentran actualmente en versión preliminar pública. Para las actualizaciones y mejoras de SQL Database, consulte [Actualizaciones del servicio SQL Database](https://azure.microsoft.com/updates/?product=sql-database). Para ver las actualizaciones y mejoras en otros servicios de Azure, consulte [actualizaciones de los servicios](https://azure.microsoft.com/updates).

## <a name="features-in-public-preview"></a>Características en versión preliminar pública

### <a name="single-databasetabsingle-database"></a>[Base de datos única](#tab/single-database)

| Característica | Detalles |
| ---| --- |
| Nuevas generaciones de hardware de las series Fsv2 y M| Para obtener más información, vea [Hardware generations](sql-database-service-tiers-vcore.md#hardware-generations) (Generaciones de hardware).|
| [Azure Private Link](https://azure.microsoft.com/updates/private-link-now-available-in-preview/)| Private Link simplifica la arquitectura de red y protege la conexión entre los puntos de conexión en Azure manteniendo los datos en la red de Azure, lo que elimina la exposición de datos a Internet pública. Private Link también le permite crear y representar sus propios servicios en Azure. |
| Recuperación de base de datos acelerada con bases de datos únicas y grupos elásticos | Para más información, consulte [Recuperación de base de datos acelerada](sql-database-accelerated-database-recovery.md).|
|Número aproximado de valores no nulos únicos|Para más información, consulte [Procesamiento de consultas aproximado](https://docs.microsoft.com/sql/relational-databases/performance/intelligent-query-processing#approximate-query-processing).|
|Modo por lotes en el almacén de filas (en el nivel de compatibilidad 150)|Para más información, consulte [Modo por lotes en el almacén de filas](https://docs.microsoft.com/sql/relational-databases/performance/intelligent-query-processing#batch-mode-on-rowstore).|
| Clasificación y detección de datos  |Para más información, consulte [Clasificación y detección de datos de Azure SQL Database y Azure SQL Data Warehouse](sql-database-data-discovery-and-classification.md).|
| Trabajos de base de datos elástica | Para más información, consulte [Creación, configuración y administración de trabajos elásticos](elastic-jobs-overview.md). |
| Consultas elásticas | Para más información, consulte [Introducción a la consulta elástica](sql-database-elastic-query-overview.md). |
| Transacciones elásticas | [Transacciones distribuidas en bases de datos en la nube](sql-database-elastic-transactions-overview.md). |
|Comentarios sobre la concesión de memoria (modo de fila) (en el nivel de compatibilidad 150)|Para más información, consulte [Comentarios de concesión de memoria del modo de fila](https://docs.microsoft.com/sql/relational-databases/performance/intelligent-query-processing#row-mode-memory-grant-feedback).|
| Editor de consultas de Azure Portal |Para más información, consulte [Uso del editor de consultas SQL de Azure Portal para conectarse a datos y consultarlos](sql-database-connect-query-portal.md).|
| R services o aprendizaje automático con bases de datos únicas y grupos elásticos |Para más información, consulte [Machine Learning Services en Azure SQL Database](https://docs.microsoft.com/sql/advanced-analytics/what-s-new-in-sql-server-machine-learning-services?view=sql-server-2017#machine-learning-services-in-azure-sql-database).|
|SQL Analytics|Para más información, consulte [Azure SQL Analytics](../azure-monitor/insights/azure-sql.md).|
|Compilación diferida de variables de tabla (en el nivel de compatibilidad 150)|Para más información, consulte [Compilación diferida de variables de tabla](https://docs.microsoft.com/sql/relational-databases/performance/intelligent-query-processing#table-variable-deferred-compilation).|
| &nbsp; |

### <a name="managed-instancetabmanaged-instance"></a>[Instancia administrada](#tab/managed-instance)

| Característica | Detalles |
| ---| --- |
| <a href="/azure/sql-database/sql-database-managed-instance-connectivity-architecture#service-aided-subnet-configuration-public-preview-in-east-us-and-west-us">Configuración de subred asistida por servicio</a> | Una forma cómoda y segura de administrar la configuración de subred. |
| <a href="/azure/sql-database/sql-database-instance-pools">Grupos de instancias</a> | Una manera útil y rentable de migrar pequeñas instancias de SQL a la nube. |
| <a href="https://aka.ms/managed-instance-tde-byok">Cifrado de datos transparente (TDE) con Bring Your Own Key (BYOK)</a> |Para más información, consulte [Cifrado de datos transparente de Azure SQL con una clave administrada por el cliente de Azure Key Vault: Compatibilidad con Bring Your Own Key](transparent-data-encryption-byok-azure-sql.md).|
| <a href="https://aka.ms/managed-instance-aadlogins">Entidades de seguridad (inicios de sesión) del servidor de Azure AD con SSMS</a> | Cree inicios de sesión de nivel de servidor con <a href="https://docs.microsoft.com/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current">CREATE LOGIN FROM EXTERNAL PROVIDER</a>. |
| [Replicación transaccional](sql-database-managed-instance-transactional-replication.md) | Replique los cambios de las tablas en otras bases de datos colocadas en instancias administradas, bases de datos únicas o instancias de SQL Server, o bien actualice las tablas cuando se cambien algunas filas en otras instancias administradas o en otras instancias de SQL Server. Para más información, consulte [Configuración de la replicación en una base de datos de instancia administrada de Azure SQL Database](replication-with-sql-database-managed-instance.md). |
| Detección de amenazas |Para más información, consulte [Configuración de la detección de amenazas en Instancia administrada de Azure SQL Database](sql-database-managed-instance-threat-detection.md).|
| Recreación de las bases de datos con instancias administradas |Para más información, consulte [Re-create dropped databases in Azure SQL Managed Instance](https://medium.com/azure-sqldb-managed-instance/re-create-dropped-databases-in-azure-sql-managed-instance-dc369ed60266) (Recreación de bases de datos eliminadas en Instancia administrada de Azure SQL Database).|
| &nbsp; |

---

## <a name="new-features"></a>Nuevas características

### <a name="managed-instance-h2-2019-updates"></a>Actualizaciones de la instancia administrada H2 2019

- [Los grupos de conmutación por error automática](https://azure.microsoft.com/updates/azure-sql-database-auto-failover-groups-feature-now-available-in-all-regions/) permiten replicar todas las bases de datos de la instancia principal en una instancia secundaria de otra región.
- Configure el comportamiento de la instancia administrada con [marcas de seguimiento globales](https://azure.microsoft.com/updates/global-trace-flags-are-now-available-in-azure-sql-database-managed-instance/).

### <a name="managed-instance-h1-2019-updates"></a>Actualizaciones de la instancia administrada H1 2019

Las características siguientes están habilitadas en el modelo de implementación de instancia administrada de H1 2019:
  - Compatibilidad con las suscripciones con el <a href="https://aka.ms/sql-mi-visual-studio-subscribers"> crédito mensual de Azure para suscriptores de Visual Studio</a> y mayores [límites regionales](sql-database-managed-instance-resource-limits.md#regional-resource-limitations).
  - Compatibilidad con <a href="https://docs.microsoft.com/sharepoint/administration/deploy-azure-sql-managed-instance-with-sharepoint-servers-2016-2019"> SharePoint 2016 y SharePoint 2019 </a> y <a href="https://docs.microsoft.com/business-applications-release-notes/october18/dynamics365-business-central/support-for-azure-sql-database-managed-instance"> Dynamics 365 Business Central </a>
  - Creación de instancias con la <a href="https://aka.ms/managed-instance-collation">intercalación de nivel de servidor</a> y la <a href="https://azure.microsoft.com/updates/managed-instance-time-zone-ga/">zona horaria</a> que elija.
  - Las instancias administradas están protegidas por el <a href="sql-database-managed-instance-management-endpoint-verify-built-in-firewall.md">firewall integrado</a>.
  - Configuración de las instancias para que usen [puntos de conexión públicos](sql-database-managed-instance-public-endpoint-configure.md), la conexión de [invalidación de proxy](sql-database-connectivity-architecture.md#connection-policy) para obtener un mejor rendimiento de red, <a href="https://aka.ms/four-cores-sql-mi-update"> cuatro núcleos virtuales en la generación de hardware de gen5</a> y <a href="https://aka.ms/managed-instance-configurable-backup-retention">configuración de la retención de copia de seguridad hasta 35 días</a> para la restauración a un momento dado. La retención de copias de seguridad a largo plazo (hasta 10 años) aún no está habilitada, por lo que se pueden usar <a href="https://docs.microsoft.com/sql/relational-databases/backup-restore/copy-only-backups-sql-server">copias de seguridad de solo copia</a> como alternativa.
  - Las nuevas funcionalidades permiten <a href="https://medium.com/@jocapc/geo-restore-your-databases-on-azure-sql-instances-1451480e90fa">restaurar geográficamente la base de datos en otro centro de datos mediante PowerShell</a>, [cambiar el nombre de la base de datos](https://azure.microsoft.com/updates/azure-sql-database-managed-instance-database-rename-is-supported/) y [eliminar un clúster virtual](sql-database-managed-instance-delete-virtual-cluster.md).
  - El nuevo [rol de colaborador de instancia](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#sql-managed-instance-contributor) integrado permite el cumplimiento de la separación de los derechos (SoD) con los principios de seguridad y el cumplimiento de los estándares de la empresa.
  - La instancia administrada está disponible en las siguientes regiones de Azure Government de disponibilidad general (US Gov Texas, US Gov Arizona), así como en Norte de China 2 y Este de China 2. También está disponible en las siguientes regiones públicas: Centro de Australia, Centro de Australia 2, Sur de Brasil, Sur de Francia, Centro de Emiratos Árabes Unidos, Norte de Emiratos Árabes Unidos, Norte de Sudáfrica, Oeste de Sudáfrica.

## <a name="fixed-known-issues"></a>Problemas conocidos corregidos

- **Agosto de 2019**: las bases de datos independientes son totalmente compatibles con la instancia administrada.
- **Octubre de 2019**: la restauración de una base de datos a un momento dado dl nivel de servicio Crítico para la empresa al De uso general no se realizará correctamente si la base de datos de origen contiene objetos OLTP en memoria.
- **Octubre de 2019**: puede usar la característica **Correo electrónico de base de datos** con servidores de correo externos (que no son de Azure) mediante una conexión segura.
- **Noviembre de 2019**: se comprueba la coherencia de la base de datos mediante `DBCC CHECKDB` después de restaurar la base de datos de Azure Blob Storage.

## <a name="updates"></a>Actualizaciones

Para obtener una lista de las mejoras y actualizaciones de SQL Database, consulte las [actualizaciones del servicio SQL Database](https://azure.microsoft.com/updates/?product=sql-database).

Para ver las actualizaciones y mejoras de todos los servicios de Azure, consulte las [actualizaciones de los servicios](https://azure.microsoft.com/updates).

## <a name="contribute-to-content"></a>Contribución al contenido

Para colaborar en la documentación de Azure SQL Database, consulte la [Guía para colaboradores de Microsoft Docs](https://docs.microsoft.com/contribute/).
