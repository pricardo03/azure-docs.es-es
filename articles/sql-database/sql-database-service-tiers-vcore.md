---
title: 'Servicio Azure SQL Database: núcleos virtuales | Microsoft Docs'
description: El modelo de compra basado en núcleos virtuales permite escalar los recursos de proceso y almacenamiento de manera independiente, igualar el rendimiento local y optimizar el precio.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: CarlRabeler
ms.author: carlrab
ms.reviewer: sashan, moslake
manager: craigg
ms.date: 02/07/2019
ms.openlocfilehash: da43d1261b0ba9bd65998fbaa9fe9e364e686071
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/11/2019
ms.locfileid: "55992133"
---
# <a name="vcore-service-tiers-azure-hybrid-benefit-and-migration"></a>Niveles de servicio de núcleo virtual, ventaja híbrida de Azure y migración

El modelo de compra basado en núcleos virtuales permite escalar los recursos de proceso y almacenamiento de manera independiente, igualar el rendimiento local y optimizar el precio. También le permite elegir la generación del hardware:

- Gen 4: hasta 24 CPU lógicas basadas en procesadores Intel E5-2673 v3 (Haswell) de 2,4 GHz, núcleo virtual = 1 PP (núcleo físico), 7 GB por núcleo, SSD conectada
- Gen 5: hasta 80 CPU lógicas basadas en procesadores Intel E5-2673 v4 (Broadwell) de 2,3 GHz, núcleo virtual = 1 LP (hyper-thread), 5,1 GB por núcleo, SSD eNVM rápida

El hardware de Gen4 ofrece bastante más memoria por núcleo virtual. Sin embargo, el hardware de Gen5 permite escalar verticalmente mucho más alto los recursos de proceso.

> [!NOTE]
> Para más información sobre los niveles de servicio basados en DTU, consulte [Niveles de servicio basados en DTU](sql-database-service-tiers-dtu.md). Para obtener información acerca de cómo distinguir los niveles de servicio basados en DTU y los niveles de servicio basados en núcleos virtuales, consulte [Modelos de compra de Azure SQL Database](sql-database-purchase-models.md).

## <a name="service-tier-characteristics"></a>Características del nivel de servicios

El modelo de núcleo virtual ofrece tres niveles de servicio: De uso General, Hiperescala y Crítico para la empresa. Los niveles de servicio se diferencian por una variedad de tamaños de proceso, un diseño de alta disponibilidad, el aislamiento de errores, los tipos y el tamaño de almacenamiento y el intervalo de E/S. Debe configurar por separado el periodo necesario de retención y almacenamiento de las copias de seguridad. En Azure Portal, vaya a Servidor (no la base de datos) > Copia de seguridad administrada > Configurar directiva > Configuración de restauración a un momento dado > de 7 a 35 días.

La tabla siguiente le ayudará a comprender las diferencias entre estos tres niveles:

||**Uso general**|**Crítico para la empresa**|**Hiperescala (versión preliminar)**|
|---|---|---|---|
|Más adecuado para|La mayoría de las cargas de trabajo empresariales. Ofrece opciones de proceso y almacenamiento equilibradas y escalables orientadas al presupuesto.|Aplicaciones empresariales con elevados requisitos de E/S. Ofrece la máxima resistencia a errores mediante varias réplicas aisladas.|La mayoría de las cargas de trabajo de una empresa que tengan requisitos de almacenamiento y un escalado de lectura que sean altamente escalables.|
|Proceso|Gen4: De 1 a 24 núcleos virtuales<br/>Gen5: De 1 a 80 núcleos virtuales|Gen4: De 1 a 24 núcleos virtuales<br/>Gen5: De 1 a 80 núcleos virtuales|Gen4: De 1 a 24 núcleos virtuales<br/>Gen5: De 1 a 80 núcleos virtuales|
|Memoria|Gen4: 7 GB por núcleo<br>Gen5: 5,1 GB por núcleo | Gen4: 7 GB por núcleo<br>Gen5: 5,1 GB por núcleo |Gen4: 7 GB por núcleo<br>Gen5: 5,1 GB por núcleo|
|Almacenamiento|Usa [almacenamiento remoto Premium](../virtual-machines/windows/premium-storage.md):<br/>Base de datos única: 5 GB – 4 TB<br/>Instancia administrada: 32 GB - 8 TB |Usa almacenamiento local de SSD:<br/>Base de datos única: 5 GB – 4 TB<br/>Instancia administrada: 32 GB - 4 TB |Flexible; crecimiento automático de almacenamiento según sea necesario. Admite hasta 100 TB de almacenamiento y mucho más. Almacenamiento SSD local para la caché del grupo de búferes local y el almacenamiento de datos local. Almacenamiento remoto de Azure como almacén de datos final a largo plazo. |
|Rendimiento de E/S (aproximado)|Base de datos única: 500 IOPS por núcleo virtual con 7000 IOPS como máximo</br>Instancia administrada: Depende del [tamaño del archivo](../virtual-machines/windows/premium-storage-performance.md#premium-storage-disk-sizes)|5000 IOPS por núcleo con 200 000 IOPS como máximo|TBD|
|Disponibilidad|1 réplica, sin escalado de lectura|3 réplicas, 1 [réplica de escalado de lectura](sql-database-read-scale-out.md),<br/>Con alta disponibilidad y redundancia de zona|?|
|Copias de seguridad|[RA-GRS](../storage/common/storage-designing-ha-apps-with-ragrs.md), de 7 a 35 días (7 días de forma predeterminada)|[RA-GRS](../storage/common/storage-designing-ha-apps-with-ragrs.md), de 7 a 35 días (7 días de forma predeterminada)|Copias de seguridad basadas en instantáneas que se encuentran en el almacenamiento remoto de Azure, y restauraciones que usan esas instantáneas para una recuperación rápida. Las copias de seguridad son instantáneas y no afectan el rendimiento de E/S del proceso. Las restauraciones son muy rápidas y no tienen el tamaño de una operación de datos (tardan minutos en lugar de horas o días).|
|En memoria|No compatible|Compatible|No compatible|
|||

> [!NOTE]
> Si quiere explorar Azure, puede obtener una instancia gratuita de Azure SQL Database en el nivel de servicio Básico junto con una cuenta gratuita de Azure. Para obtener información, consulte [Cree una base de datos administrada en la nube con su cuenta gratuita de Azure](https://azure.microsoft.com/free/services/sql-database/).

- Para más información, consulte [Límites de recursos de núcleo virtual para una base de datos única](sql-database-vcore-resource-limits-single-databases.md) y [Límites de recursos de núcleo virtual en una Instancia administrada](sql-database-managed-instance.md#vcore-based-purchasing-model).
- Para obtener más información acerca de los niveles de servicio de uso general y crítico para la empresa, consulte [Niveles de servicio de uso general y críticos para la empresa](sql-database-service-tiers-general-purpose-business-critical.md).
- Para obtener información sobre el nivel de servicio de hiperescala en el modelo de compra basado en núcleos virtuales, consulte [Capa de servicio de hiperescala](sql-database-service-tier-hyperscale.md).  

> [!IMPORTANT]
> Si tiene menos de un núcleo virtual de capacidad de proceso, use el modelo de compra basado en DTU.

## <a name="azure-hybrid-benefit"></a>Ventaja híbrida de Azure

En el modelo de compra basado en núcleos virtuales, puede intercambiar sus licencias existentes por tarifas de descuento en SQL Database mediante la [Ventaja híbrida de Azure para SQL Server](https://azure.microsoft.com/pricing/hybrid-benefit/). Esta ventaja de Azure le permite usar las licencias de SQL Server locales para ahorrar hasta un 30 % en Azure SQL Database al utilizarlas con Software Assurance.

![Precios](./media/sql-database-service-tiers/pricing.png)

Con la Ventaja híbrida de Azure, puede elegir pagar solo por la infraestructura subyacente de Azure mediante la licencia existente de SQL Server para el motor de base de datos SQL (**BasePrice**) o pagar tanto por la infraestructura subyacente como por la licencia de SQL Server (**LicenseIncluded**). Puede elegir o cambiar el modelo de licencia mediante Azure Portal o con una de las siguientes API.

- Para establecer o actualizar el tipo de licencia mediante PowerShell:

  - [New-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/new-azsqldatabase):
  - [Set-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql)
  - [New-AzSqlInstance](https://docs.microsoft.com/powershell/module/az.sql/new-azsqlinstance)
  - [Set-AzSqlInstance](https://docs.microsoft.com/powershell/module/az.sql)

- Para establecer o actualizar el tipo de licencia mediante la CLI de Azure:

  - [az sql db create](https://docs.microsoft.com/cli/azure/sql/db#az-sql-db-create)
  - [az sql db update](https://docs.microsoft.com/cli/azure/sql/db#az-sql-db-update)
  - [az sql mi create](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-create)
  - [az sql mi update](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-update)

- Para establecer o actualizar el tipo de licencia mediante la API REST:

  - [Databases - Create Or Update](https://docs.microsoft.com/rest/api/sql/databases/createorupdate)
  - [Databases - Update](https://docs.microsoft.com/rest/api/sql/databases/update)
  - [Managed Instances - Create Or Update](https://docs.microsoft.com/rest/api/sql/managedinstances/createorupdate)
  - [Managed Instances - Update](https://docs.microsoft.com/rest/api/sql/managedinstances/update)

## <a name="migration-from-dtu-model-to-vcore-model"></a>Migración del modelo DTU al modelo con núcleo virtual

### <a name="migration-of-a-database"></a>Migración de una base de datos

La migración de una base de datos del modelo de compra basado en DTU al modelo de compra basado en núcleos virtuales es similar a la actualización o cambio a una versión anterior entre las bases de datos Estándar y Premium en el modelo de compra basado en DTU.

### <a name="migration-of-databases-with-geo-replication-links"></a>Migración de bases de datos con vínculos de replicación geográfica

La migración desde el modelo basado en DTU al modelo basado en núcleos virtuales es similar a la actualización o degradación de las relaciones de replicación geográfica entre las bases de datos Estándar y Premium. No es necesario terminar la replicación geográfica, pero el usuario debe respetar las reglas de secuenciación. Al actualizar, debe actualizar primero la base de datos secundaria y luego la principal. Al degradar, invierta el orden; es decir, debe degradar primero la base de datos principal y luego la secundaria.

Cuando se usa la replicación geográfica entre dos grupos elásticos, se recomienda designar un grupo como principal y otro como secundario. En ese caso, la migración de grupos elásticos debe seguir las mismas instrucciones.  Sin embargo, es técnicamente posible que un grupo elástico contenga bases de datos principales y secundarias. En este caso, para migrarlas correctamente se debe tratar el grupo con la utilización más alta como principal y seguir las reglas de secuenciación como corresponda.  

En la tabla siguiente se proporcionan instrucciones para escenarios de migración específicos:

|Nivel de servicio actual|Nivel de servicio de destino|Tipo de migración|Acciones del usuario|
|---|---|---|---|
|Estándar|Uso general|Lateral|Puede migrar en cualquier orden, pero debe asegurarse de tener un tamaño adecuado de núcleo virtual*|
|Premium|Crítico para la empresa|Lateral|Puede migrar en cualquier orden, pero debe asegurarse de tener un tamaño adecuado de núcleo virtual*|
|Estándar|Crítico para la empresa|Actualizar|Debe migrar primero la secundaria|
|Crítico para la empresa|Estándar|Degradar|Debe migrar primero la principal|
|Premium|Uso general|Degradar|Debe migrar primero la principal|
|Uso general|Premium|Actualizar|Debe migrar primero la secundaria|
|Crítico para la empresa|Uso general|Degradar|Debe migrar primero la principal|
|Uso general|Crítico para la empresa|Actualizar|Debe migrar primero la secundaria|
||||

\* Cada 100 DTU del nivel Estándar requieren como mínimo 1 núcleo virtual y cada 125 DTU de nivel Premium requieren como mínimo un núcleo.

### <a name="migration-of-failover-groups"></a>Migración de grupos de conmutación por error

La migración de grupos de conmutación por error con varias bases de datos requiere la migración individual de las bases de datos principales y secundarias. Durante ese proceso, se aplican las mismas consideraciones y reglas de secuenciación. Después de que las bases de datos se convierten al modelo basado en núcleos virtuales, el grupo de conmutación por error permanecerá en vigor con la misma configuración de directiva.

### <a name="creation-of-a-geo-replication-secondary"></a>Creación de una base de datos secundaria de replicación geográfica

Solo puede crear una base de datos secundaria de replicación geográfica con el mismo nivel de servicio que la principal. Para una base de datos con una tasa de generación de registros elevada, es muy recomendable que la base de datos secundaria se cree con el mismo tamaño de proceso que la principal. Si va a crear una base de datos secundaria con replicación geográfica en el grupo elástico para una única base de datos principal, se recomienda que el valor `maxVCore` del grupo coincida con el tamaño de proceso de la base de datos principal. Si va a crear una base de datos secundaria con replicación geográfica en el grupo elástico para una principal de otro grupo elástico, se recomienda que los grupos tengan la misma configuración de `maxVCore`.

### <a name="using-database-copy-to-convert-a-dtu-based-database-to-a-vcore-based-database"></a>Uso de una copia de base de datos para convertir una base de datos basada en DTU en una base de datos basada en núcleos virtuales

Puede copiar cualquier base de datos con un tamaño de proceso basado en DTU en una base de datos que tenga un tamaño de proceso basado en núcleos virtuales sin restricciones o secuencias especiales, siempre y cuando el tamaño de proceso de destino admita el tamaño máximo de base de datos de la base de datos de origen. La copia de base de datos crea una instantánea de los datos a partir de la hora de inicio de la operación de copia y no realiza la sincronización de datos entre el origen y el destino.

## <a name="next-steps"></a>Pasos siguientes

- Para obtener más información sobre las opciones de tamaños de proceso y de tamaños de almacenamiento específicas que hay disponibles para las bases de datos únicas, consulte [SQL Database vCore-based resource limits for single databases](sql-database-vcore-resource-limits-single-databases.md#general-purpose-service-tier-storage-sizes-and-compute-sizes) (Límites de recursos basados en núcleos virtuales de SQL Database para bases de datos únicas).
- Para más información sobre las opciones de tamaño de proceso y de tamaño de almacenamiento específicas que hay disponibles para los grupos elásticos, consulte [SQL Database vCore-based resource limits for elastic pools](sql-database-vcore-resource-limits-elastic-pools.md#general-purpose-service-tier-storage-sizes-and-compute-sizes) (Límites de recursos basados en núcleos virtuales de Azure SQL Database para grupos elásticos).
