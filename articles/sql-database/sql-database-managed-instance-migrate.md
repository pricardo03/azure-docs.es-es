---
title: Migración de una instancia de SQL Server a Instancia administrada de Azure SQL Database | Microsoft Docs
description: Obtenga información acerca de cómo migrar una instancia de SQL Server a Instancia administrada de Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: bonova
ms.author: bonova
ms.reviewer: carlrab
manager: craigg
ms.date: 10/15/2018
ms.openlocfilehash: 6868b842f22a6d107936fcb1e49c46b0c1f58469
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2018
ms.locfileid: "49345312"
---
# <a name="sql-server-instance-migration-to-azure-sql-database-managed-instance"></a>Migración de una instancia de SQL Server a Instancia administrada de Azure SQL Database

En este artículo se describen los métodos para migrar una instancia de SQL Server 2005 o versiones posteriores a una [instancia administrada de Azure SQL Database](sql-database-managed-instance.md).

En un nivel alto, el proceso de migración de la base de datos es parecido a este:

![proceso de migración](./media/sql-database-managed-instance-migration/migration-process.png)

- [Evaluación de la compatibilidad de Instancia administrada](#assess-managed-instance-compatibility)
- [Elección de la opción de conectividad de las aplicaciones](sql-database-managed-instance-connect-app.md)
- [Implementación en una instancia administrada con tamaño óptimo](#deploy-to-an-optimally-sized-managed-instance)
- [Selección del método de migración y la migración](#select-migration-method-and-migrate)
- [Supervisión de aplicaciones](#monitor-applications)

> [!NOTE]
> Para migrar una única base de datos a una sola base de datos o a un grupo elástico, consulte [Migración de una base de datos de SQL Server a Azure SQL Database](sql-database-cloud-migrate.md).

## <a name="assess-managed-instance-compatibility"></a>Evaluación de la compatibilidad de Instancia administrada

En primer lugar, determine si Instancia administrada es compatible con los requisitos de base de datos de su aplicación. Instancia administrada se ha diseñado para poder migrar mediante lift-and-shit la mayoría de las aplicaciones existentes que usan instancias de SQL Server locales o en máquinas virtuales. Sin embargo, a veces podría necesitar características o funcionalidades que todavía no se admiten y el costo de implementar una solución alternativa es demasiado alto.

Use [Data Migration Assistant (DMA)](https://docs.microsoft.com/sql/dma/dma-overview) para detectar posibles problemas de compatibilidad que afecten a la funcionalidad de la base de datos en Azure SQL Database. DMA aún no admite Instancia administrada como destino de migración, pero se recomienda ejecutar la evaluación en Azure SQL Database y revisar atentamente la lista de problemas notificados sobre compatibilidad y paridad de las características con la documentación del producto. Consulte las [características de Azure SQL Database](sql-database-features.md) para comprobar si hay algún problema de bloqueo notificado que no bloquee la Instancia administrada, puesto que la mayoría de los problemas de bloqueo que evitaban el proceso de migración a Azure SQL Database se han eliminado con la Instancia administrada. Por ejemplo, características tales como consultas entre bases de datos, transacciones entre bases de datos dentro de la misma instancia, servidor vinculado a otros orígenes SQL, CLR, tablas temporales globales, vistas de nivel de instancia, Service Broker y similares están disponibles en Instancia administrada.

Si se ha notificado algún problema de bloqueo que no se haya eliminado en la Instancia administrada de Azure SQL Database, quizá deba considerar una alternativa, como [SQL Server en Virtual Machines en Azure](https://azure.microsoft.com/services/virtual-machines/sql-server/). Estos son algunos ejemplos:

- Si necesita acceso directo al sistema operativo o al sistema de archivos, por ejemplo, para instalar agentes personalizados o de otros fabricantes en la misma máquina virtual con SQL Server.
- Si las características tienen dependencias estrictas que aún no se admiten, por ejemplo, FileStream o FileTable, PolyBase y transacciones entre instancias.
- Si es imprescindible que permanezca en una versión específica de SQL Server (2012, por ejemplo).
- Si los requisitos de proceso son mucho menores que los que Instancia administrada ofrece en su versión preliminar pública (un núcleo virtual, por ejemplo) y la consolidación de la base de datos no es una opción aceptable.

## <a name="deploy-to-an-optimally-sized-managed-instance"></a>Implementación en una instancia administrada con un tamaño óptimo

Instancia administrada ha sido diseñado para cargas de trabajo locales que se van a mover a la nube. Presenta un [nuevo modelo de compra](sql-database-service-tiers-vcore.md) que ofrece mayor flexibilidad para seleccionar el nivel adecuado de recursos para las cargas de trabajo. En el mundo local, probablemente está acostumbrado a ajustar el tamaño de estas cargas de trabajo mediante el uso de núcleos físicos y ancho de banda de E/S. El nuevo modelo de compra de Instancia administrada usa núcleos virtuales con almacenamiento adicional y E/S disponible por separado. El modelo de núcleos virtuales es una manera sencilla de comprender los requisitos de proceso en la nube en comparación con lo que usa en su entorno local hoy en día. Este nuevo modelo permite elegir el tamaño adecuado para el entorno de destino en la nube.

Puede seleccionar recursos de almacenamiento y proceso en el momento de la implementación y, posteriormente, cambiarlo sin introducir tiempo de inactividad de la aplicación con [Azure Portal](sql-database-scale-resources.md):

![ajuste de tamaño de la instancia administrada](./media/sql-database-managed-instance-migration/managed-instance-sizing.png)

Para más información sobre la creación de la infraestructura de red virtual y una instancia administrada, consulte el artículo de [creación de una instancia administrada](sql-database-managed-instance-get-started.md).

> [!IMPORTANT]
> Es importante que la red virtual y la subred de destino cumplan siempre los [requisitos de red virtual de instancia administrada de Azure SQL Database](sql-database-managed-instance-vnet-configuration.md#requirements). Cualquier incompatibilidad puede impedir crear nuevas instancias o usar las que ya ha creado.

## <a name="select-migration-method-and-migrate"></a>Selección del método de migración y migración

Instancia administrada está diseñado para escenarios de usuario que requieren la migración masiva de bases de datos desde implementaciones locales o de base de datos de IaaS. Son la opción ideal si se necesita migrar mediante lift-and-shift las aplicaciones back-end que periódicamente usan funcionalidades del nivel de instancia o entre bases de datos. Si este es su caso, puede mover una instancia completa al entorno correspondiente en Azure sin necesidad de rediseñar sus aplicaciones.

Para mover las instancias de SQL, deberá planear cuidadosamente:

- La migración de todas las bases de datos que deben situarse juntas (que se ejecutan en la misma instancia)
- La migración de objetos de nivel de instancia de los que depende la aplicación, incluidos inicios de sesión, credenciales, trabajos y operadores del Agente SQL, y desencadenadores de nivel de servidor.

Instancia administrada es un servicio completamente administrado que permite delegar algunas de las actividades DBA regulares a la plataforma a medida que se crean. Por lo tanto, algunos datos de nivel de instancia no tienen que migrarse, por ejemplo, los trabajos de mantenimiento de copias de seguridad periódicas o la configuración Always On, porque integra [alta disponibilidad](sql-database-high-availability.md).

Instancia administrada admite las siguientes opciones de migración de base de datos (actualmente son los únicos métodos de migración admitidos):

- Azure Database Migration Service: migración prácticamente sin tiempo de inactividad.
- `RESTORE DATABASE FROM URL` nativo: usa copias de seguridad nativas de SQL Server y requiere tiempo de inactividad.

### <a name="azure-database-migration-service"></a>Azure Database Migration Service

[Azure Database Migration Service (DMS)](../dms/dms-overview.md) es un servicio totalmente administrado diseñado para permitir migraciones completas desde varios orígenes de base de datos hasta las plataformas de datos de Azure con un tiempo de inactividad mínimo. Este servicio simplifica las tareas necesarias para mover las bases de datos existentes de SQL Server y de terceros a Azure. En la versión preliminar pública, las opciones de implementación incluyen Azure SQL Database, Instancia administrada y SQL Server en una máquina virtual de Azure. DMS es el método recomendado de migración para las cargas de trabajo de la empresa.

Si usa SQL Server Integration Services (SSIS) en su servidor de SQL Server local, tenga en cuenta que el servicio DMS aún no admite la migración del catálogo de SSIS (SSISDB) que almacena paquetes SSIS; de todos modos, puede aprovisionar Azure-SSIS Integration Runtime (IR) en Azure Data Factory (ADF), para crear un nuevo SSISDB en Azure SQL Database o la instancia administrada, y puede volver a implementar sus paquetes en esta opción. Para ello, consulte [Creación de una instancia de Integration Runtime de SSIS de Azure en Azure Data Factory](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime).

Para más información acerca de este escenario y los pasos de configuración para DMS, consulte cómo [migrar una base de datos local a Instancia administrada con DMS](../dms/tutorial-sql-server-to-managed-instance.md).  

### <a name="native-restore-from-url"></a>Restauración de copias de seguridad nativas desde la dirección URL

La restauración de copias de seguridad nativas (archivos .bak) realizadas desde instancias locales de SQL Server o [SQL Server en Virtual Machines](https://azure.microsoft.com/services/virtual-machines/sql-server/), disponible en [Azure Storage](https://azure.microsoft.com/services/storage/), es una de las principales funcionalidades de Instancia administrada de SQL DB que permite una migración rápida y sencilla de la base de datos sin conexión.

El siguiente diagrama proporciona una introducción general del proceso:

![flujo de migración](./media/sql-database-managed-instance-migration/migration-flow.png)

La siguiente tabla proporciona más información sobre los métodos que puede usar según la versión de SQL Server de origen que esté ejecutando:

|Paso|Motor y versión de SQL|Método de copia de seguridad y restauración|
|---|---|---|
|Colocar la copia de seguridad en Azure Storage|Anteriores a SQL 2012 SP1 CU2|Carga del archivo .bak directamente a Azure Storage|
||2012 SP1 CU2 - 2016|Copia de seguridad directa mediante la sintaxis [WITH CREDENTIAL](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-transact-sql), ya en desuso.|
||2016 y posteriores|Copia de seguridad directa con [WITH SAS CREDENTIAL](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-to-url).|
|Restaurar desde Azure Storage en Instancia administrada|[RESTORE FROM URL WITH SAS CREDENTIAL](sql-database-managed-instance-get-started-restore.md)|

> [!IMPORTANT]
> - Al migrar una base de datos protegida mediante el [Cifrado de datos transparente](transparent-data-encryption-azure-sql.md) a la Instancia administrada de Azure SQL Database mediante la opción de restauración nativa, se debe migrar el certificado correspondiente de SQL Server local o IaaS antes de restaurar la base de datos. Para consultar los pasos detallados, vea [Migración de un certificado TDE a Instancia administrada](sql-database-managed-instance-migrate-tde-certificate.md)
> - No se permite restaurar bases de datos del sistema. Para migrar objetos de nivel de instancia (almacenados en bases de datos maestras o msdb), se recomienda generar scripts y ejecutar scripts de T-SQL en la instancia de destino.

Para obtener una guía rápida que incluya la manera de restaurar una copia de seguridad de base de datos en una instancia administrada mediante las credenciales SAS, consulte el artículo de [Restore from backup to a Managed Instance](sql-database-managed-instance-get-started-restore.md) (Restauración desde una copia de seguridad a una instancia administrada).

> [!VIDEO https://www.youtube.com/embed/RxWYojo_Y3Q]

## <a name="monitor-applications"></a>Supervisión de aplicaciones

Realice un seguimiento del comportamiento y el rendimiento de las aplicaciones después de la migración. En Instancia administrada, algunos cambios se habilitan solo después de [cambiar el nivel de compatibilidad de la base de datos](https://docs.microsoft.com/sql/relational-databases/databases/view-or-change-the-compatibility-level-of-a-database). En la mayoría de los casos, al migrar la base de datos a Azure SQL Database se mantiene su nivel de compatibilidad original. Si el nivel de compatibilidad de una base de datos de usuario era 100 o superior antes de la migración, permanece igual después de la migración. Si el nivel de compatibilidad de una base de datos de usuario era 90 antes de la migración, el nivel de compatibilidad de la base de datos actualizada se establece en 100, que es el nivel de compatibilidad mínimo admitido en Instancia administrada. El nivel de compatibilidad de las bases de datos del sistema es 140.

Para reducir los riesgos de la migración, cambie el nivel de compatibilidad de la base de datos solo después de supervisar el rendimiento. Use Almacén de consultas como herramienta ideal para obtener información sobre el rendimiento de la carga de trabajo antes y después de cambiar el nivel de compatibilidad de la base de datos, tal y como se explica en [Mantener la estabilidad del rendimiento al actualizar a una versión más reciente de SQL Server](https://docs.microsoft.com/sql/relational-databases/performance/query-store-usage-scenarios#CEUpgrade).

Cuando ya esté en una plataforma completamente administrada, aproveche las ventajas que proporciona automáticamente el servicio SQL Database. Por ejemplo, no tiene que crear copias de seguridad en Instancia administrada; el servicio realiza las copias de seguridad automáticamente. Ya no debe preocuparse de programar, seguir y administrar las copias de seguridad. Instancia administrada permite restaurar a un momento dado dentro de este período de retención mediante la [recuperación a un momento dado (PITR)](sql-database-recovery-using-backups.md#point-in-time-restore). Durante la versión preliminar pública, el período de retención se ha fijado en siete días.
Además, no es necesario preocuparse por la configuración de la alta disponibilidad porque la [alta disponibilidad](sql-database-high-availability.md) está integrada.

Para reforzar la seguridad, considere la posibilidad de utilizar algunas de las características que hay disponibles:

- Autenticación con Azure Active Directory en el nivel de base de datos
- Use [características de seguridad avanzadas](sql-database-security-overview.md) como la [auditoría](sql-database-managed-instance-auditing.md), la [detección de amenazas](sql-advanced-threat-protection.md), la [seguridad de nivel de fila](https://docs.microsoft.com/sql/relational-databases/security/row-level-security), y el [enmascaramiento dinámico de datos](https://docs.microsoft.com/sql/relational-databases/security/dynamic-data-masking)) para proteger la instancia.

## <a name="next-steps"></a>Pasos siguientes

- Para más información acerca de las instancias administradas, consulte [¿Qué es una instancia administrada?](sql-database-managed-instance.md)
- Para ver un tutorial que incluye una restauración desde una copia de seguridad, consulte [Creación de una instancia administrada](sql-database-managed-instance-get-started.md).
- Para ver un tutorial que muestra la migración mediante DMS, consulte cómo [migrar una base de datos local a Instancia administrada con DMS](../dms/tutorial-sql-server-to-managed-instance.md).  
