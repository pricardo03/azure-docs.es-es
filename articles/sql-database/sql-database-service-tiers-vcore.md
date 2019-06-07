---
title: 'Servicio Azure SQL Database: núcleos virtuales | Microsoft Docs'
description: El modelo de compra basado en núcleos virtuales le permite escalar los recursos de proceso y almacenamiento, coincide con el rendimiento de forma local y optimizar el precio por separado.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: sashan, moslake, carlrab
manager: craigg
ms.date: 05/06/2019
ms.openlocfilehash: 520dba611e6791fca990e21173424a914c3d8e14
ms.sourcegitcommit: 18a0d58358ec860c87961a45d10403079113164d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/05/2019
ms.locfileid: "66693333"
---
# <a name="choose-among-the-vcore-service-tiers-and-migrate-from-the-dtu-service-tiers"></a>Elija entre los niveles de servicio con núcleo virtual y migrar desde los niveles de servicio DTU

Virtual (núcleo)-modelo de compra basado en le permite escalar los recursos de proceso y almacenamiento, coincide con el rendimiento de forma local y optimizar el precio por separado. También le permite elegir la generación de hardware:

- **Gen4**: Hasta 24 CPU lógicas basado en Intel E5-2673 v3 (Haswell) procesadores a 2,4 GHz, núcleo virtual = 1 PP (núcleo físico), 7 GB por núcleo, adjunta SSD
- **Gen5**: Hasta 80 CPU lógicas basado en Intel E5-2673 v4 (Broadwell) procesadores de 2,3 GHz, núcleo virtual = 1 LP (hyper-threading), 5.1 GB por núcleo, eNVM rápido SSD

El hardware de Gen4 ofrece bastante más memoria por núcleo virtual. Sin embargo, el hardware de Gen5 permite escalar verticalmente mucho más alto los recursos de proceso.

> [!NOTE]
> Para obtener información acerca de los niveles de servicio basado en DTU, consulte [niveles para el modelo de compra basado en DTU de servicio](sql-database-service-tiers-dtu.md). Para obtener información sobre las diferencias entre los niveles de servicio basado en DTU y los modelos de compra basado en núcleos virtuales, consulte [modelos de compra de Azure SQL Database](sql-database-purchase-models.md).

## <a name="service-tier-characteristics"></a>Características de nivel de servicio

El modelo de compra basado en núcleos virtuales ofrece tres niveles de servicio: uso general, a gran escala y crítico para la empresa. Estos niveles de servicio se diferencian por un intervalo de tamaños de proceso, los diseños de alta disponibilidad, los métodos de aislamiento de errores, tipos y tamaños de almacenamiento y los intervalos de E/S.

Debe configurar por separado el periodo necesario de retención y almacenamiento de las copias de seguridad. Para establecer el período de retención de copia de seguridad, abra Azure portal, vaya al servidor (no la base de datos) y, a continuación, vaya a **administrar copias de seguridad** > **Configurar directiva**  >   **En tiempo de restauración de configuración del punto de** > **7 a 35 días**.

En la tabla siguiente se explica las diferencias entre los tres niveles:

||**Uso general**|**Crítico para la empresa**|**Hyperscale**|
|---|---|---|---|
|Más adecuado para|La mayoría de las cargas de trabajo empresariales. Proceso de ofertas económicas, equilibrado y escalable y opciones de almacenamiento.|Aplicaciones de negocio con grandes requisitos de E/S. Ofrece la máxima resistencia a errores mediante el uso de varias réplicas aisladas.|La mayoría cargas de trabajo de negocio con almacenamiento altamente escalable y los requisitos de escalado de lectura.|
|Proceso|**Aprovisiona el proceso**:<br/>Gen4: núcleos virtuales de 1 a 24<br/>Gen5: núcleos virtuales de 2 a 80<br/>**Proceso sin servidor**:<br/>Gen5: 0.5 - 4 núcleos virtuales|**Aprovisiona el proceso**:<br/>Gen4: núcleos virtuales de 1 a 24<br/>Gen5: núcleos virtuales de 2 a 80|**Aprovisiona el proceso**:<br/>Gen4: núcleos virtuales de 1 a 24<br/>Gen5: núcleos virtuales de 2 a 80|
|Memoria|**Aprovisiona el proceso**:<br/>Gen4: 7 GB por núcleo virtual<br/>Gen5: 5,1 GB por núcleo virtual<br/>**Proceso sin servidor**:<br/>Gen5: 3 GB por núcleo virtual|**Aprovisiona el proceso**:<br/>Gen4: 7 GB por núcleo virtual<br/>Gen5: 5,1 GB por núcleo virtual |**Aprovisiona el proceso**:<br/>Gen4: 7 GB por núcleo virtual<br/>Gen5: 5,1 GB por núcleo virtual|
|Almacenamiento|Usa almacenamiento remoto.<br/>**Base de datos única aprovisionado proceso**:<br/>5 GB – 4 TB<br/>**Informática de base de datos única**:<br/>5 GB A 1 TB<br/>**Instancia administrada**: 32 GB - 8 TB |Usa el almacenamiento SSD local.<br/>**Base de datos única aprovisionado proceso**:<br/>5 GB – 4 TB<br/>**Instancia administrada**:<br/>32 GB - 4 TB |Crecimiento automático flexible del almacenamiento según sea necesario. Admite hasta 100 TB de almacenamiento. Usa el almacenamiento SSD local para la memoria caché local del grupo de búferes y almacenamiento de datos local. Utiliza almacenamiento remoto de Azure como almacén de datos a largo plazo final. |
|Rendimiento de E/S (aproximado)|**Base de datos única**: 500 IOPS por núcleo virtual con el número máximo de 7000 IOPS.<br/>**Instancia administrada**: Depende de [tamaño del archivo](../virtual-machines/windows/premium-storage-performance.md#premium-storage-disk-sizes).|5000 IOPS por núcleo con 200 000 IOPS como máximo|A gran escala es una arquitectura de varios niveles con almacenamiento en caché en varios niveles. IOPs efectivos depende de la carga de trabajo.|
|Disponibilidad|1 réplica, no hay réplicas de escalado de lectura|3 réplicas, 1 [réplica de escalado de lectura](sql-database-read-scale-out.md),<br/>con redundancia de zona de alta disponibilidad (HA)|1 réplica de lectura y escritura, además de 0 a 4 [réplicas de escalado de lectura](sql-database-read-scale-out.md)|
|Copias de seguridad|[Almacenamiento con redundancia geográfica de acceso de lectura (RA-GRS)](../storage/common/storage-designing-ha-apps-with-ragrs.md), 7 a 35 días (7 días de forma predeterminada)|[RA-GRS](../storage/common/storage-designing-ha-apps-with-ragrs.md), de 7 a 35 días (7 días de forma predeterminada)|Instantánea de copias de seguridad basadas en el almacenamiento remoto de Azure. Los procesos de restauración usan estas instantáneas para conseguir una recuperación rápida. Las copias de seguridad son instantáneos y no afectan al rendimiento de E/S del proceso. Restauraciones rápidas y no están una operación de tamaño de datos (tomar minutos en lugar de horas o días).|
|En memoria|No compatible|Compatible|No compatible|
|||

> [!NOTE]
> Puede obtener una base de datos SQL de Azure gratuita en el nivel de servicio básico, junto con una cuenta gratuita de Azure. Para obtener más información, consulte [crear una base de datos en la nube administrado con su cuenta gratuita de Azure](https://azure.microsoft.com/free/services/sql-database/).

- Para obtener más información acerca de los límites de recursos de memoria con núcleo virtual, consulte [los límites de recursos de memoria con núcleo virtual en una sola base de datos](sql-database-vcore-resource-limits-single-databases.md) y [los límites de recursos de memoria con núcleo virtual en una instancia administrada](sql-database-managed-instance.md#vcore-based-purchasing-model).
- Para obtener más información sobre el uso general y los niveles de servicio críticos de negocio, consulte [niveles de servicio de uso General y crítico para la empresa](sql-database-service-tiers-general-purpose-business-critical.md).
- Para obtener más información sobre el nivel de servicio a gran escala en el modelo de compra basado en núcleos virtuales, consulte [nivel de servicio de hiperescala](sql-database-service-tier-hyperscale.md).  

## <a name="azure-hybrid-benefit"></a>Ventaja híbrida de Azure

En el nivel de proceso aprovisionada del modelo de compra basado en núcleos virtuales, puede intercambiar sus licencias existentes para obtener descuentos en la base de datos SQL mediante [ventaja híbrida de Azure para SQL Server](https://azure.microsoft.com/pricing/hybrid-benefit/). Esta ventaja de Azure permite ahorrar hasta un 30 por ciento en Azure SQL Database mediante el uso de las licencias de SQL Server locales con Software Assurance.

![Precios](./media/sql-database-service-tiers/pricing.png)

Con la ventaja híbrida de Azure, puede elegir pagar solo por la infraestructura subyacente de Azure mediante el uso de la licencia de SQL Server existente para el motor de base de datos SQL sí (precios de proceso básica), o puede pagar de la infraestructura subyacente y el servidor SQL Server licencias (incluidas en la licencia de precios).

Puede elegir o cambiar su modelo de licencias mediante el portal de Azure o mediante una de las siguientes API:

- Para establecer o actualizar el tipo de licencia mediante PowerShell:

  - [New-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/new-azsqldatabase)
  - [Set-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabase)
  - [New-AzSqlInstance](https://docs.microsoft.com/powershell/module/az.sql/new-azsqlinstance)
  - [Set-AzSqlInstance](https://docs.microsoft.com/powershell/module/az.sql/set-azsqlinstance)

- Para establecer o actualizar el tipo de licencia mediante la CLI de Azure:

  - [az sql db create](https://docs.microsoft.com/cli/azure/sql/db#az-sql-db-create)
  - [az sql db update](https://docs.microsoft.com/cli/azure/sql/db#az-sql-db-update)
  - [az sql mi create](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-create)
  - [az sql mi update](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-update)

- Para establecer o actualizar el tipo de licencia mediante la API de REST:

  - [Databases - Create Or Update](https://docs.microsoft.com/rest/api/sql/databases/createorupdate)
  - [Databases - Update](https://docs.microsoft.com/rest/api/sql/databases/update)
  - [Managed Instances - Create Or Update](https://docs.microsoft.com/rest/api/sql/managedinstances/createorupdate)
  - [Managed Instances - Update](https://docs.microsoft.com/rest/api/sql/managedinstances/update)

## <a name="migrate-from-the-dtu-based-model-to-the-vcore-based-model"></a>Migrar desde el modelo basado en DTU para el modelo basado en núcleos virtuales

### <a name="migrate-a-database"></a>Migración de una base de datos

Migrar una base de datos desde el modelo de compra basado en DTU para el modelo de compra basado en núcleos virtuales es similar a actualizar o degradar entre los niveles de servicio estándar y premium en el modelo de compra basado en DTU.

### <a name="migrate-databases-with-geo-replication-links"></a>Migrar bases de datos con vínculos de replicación geográfica

Migración desde el modelo basado en DTU para el modelo de compra basado en núcleos virtuales es similar a actualizar o degradar las relaciones de replicación geográfica entre bases de datos en los niveles de servicio estándar y premium. Durante la migración, no tendrá que detener la replicación geográfica, pero debe seguir estas reglas de secuenciación:

- Al actualizar, debe actualizar primero la base de datos secundaria y luego la principal.
- Al degradar, invierta el orden; es decir, debe degradar primero la base de datos principal y luego la secundaria.

Cuando se usa la replicación geográfica entre dos grupos elásticos, se recomienda designar un grupo como principal y el otro como secundario. En ese caso, cuando migra grupos elásticos debe usar la misma guía de secuenciación. Sin embargo, si tiene grupos elásticos que contienen bases de datos principales y secundaria, tratar el grupo con la utilización más alta que la principal y seguir las reglas de secuenciación como corresponda.  

En la tabla siguiente se proporciona instrucciones para escenarios de migración específicos:

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

\* Cada 100 Dtu en el nivel estándar requieren como mínimo 1 núcleo, y cada 125 Dtu en el nivel premium requieren como mínimo 1 núcleo.

### <a name="migrate-failover-groups"></a>Migrar grupos de conmutación por error

La migración de grupos de conmutación por error con varias bases de datos requiere la migración individual de las bases de datos principales y secundarias. Durante ese proceso, se aplican las mismas consideraciones y reglas de secuenciación. Después de las bases de datos se convierten en el modelo de compra basado en núcleos virtuales, el grupo de conmutación por error permanecerá en vigor con la misma configuración de directiva.

### <a name="create-a-geo-replication-secondary-database"></a>Crear una base de datos secundaria de replicación geográfica

Puede crear una base de datos secundaria replicación geográfica (geo-secondary) sólo mediante el mismo nivel de servicio que utilizó para la base de datos principal. Para las bases de datos con una tasa alta de generación de registro, se recomienda crear la replicación geográfica secundaria con el mismo tamaño de proceso que la réplica principal.

Si está creando una replicación geográfica secundaria en el grupo elástico para una sola base de datos principal, asegúrese de que el `maxVCore` configuración para el grupo coincide con tamaño de proceso de la base de datos principal. Si está creando una replicación geográfica secundaria para un elemento principal en otro grupo elástico, se recomienda que los grupos tienen el mismo `maxVCore` configuración.

### <a name="use-database-copy-to-convert-a-dtu-based-database-to-a-vcore-based-database"></a>Usar la copia de base de datos para convertir una base de datos basado en DTU en una base de datos basado en núcleos virtuales

Puede copiar cualquier base de datos con un tamaño de proceso basado en DTU en una base de datos que tenga un tamaño de proceso basado en núcleos virtuales sin restricciones o secuencias especiales, siempre y cuando el tamaño de proceso de destino admita el tamaño máximo de base de datos de la base de datos de origen. La copia de base de datos crea una instantánea de los datos a partir de la hora de inicio de la operación de copia y no sincroniza datos entre el origen y el destino.

## <a name="next-steps"></a>Pasos siguientes

- Para los tamaños de proceso específico y las opciones de tamaño de almacenamiento disponibles para bases de datos únicas, consulte [límites de recursos basado en núcleos virtuales de SQL Database para bases de datos únicas](sql-database-vcore-resource-limits-single-databases.md).
- Para los tamaños de proceso específico y las opciones de tamaño de almacenamiento disponibles para los grupos elásticos, consulte [límites de recursos basado en núcleos virtuales de base de datos SQL para grupos elásticos](sql-database-vcore-resource-limits-elastic-pools.md#general-purpose-service-tier-storage-sizes-and-compute-sizes).
