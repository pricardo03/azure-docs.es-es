---
title: 'Servicio Azure SQL Database: núcleos virtuales | Microsoft Docs'
description: El modelo de compra basado en núcleos virtuales permite escalar los recursos de proceso y almacenamiento de manera independiente, igualar el rendimiento local y optimizar el precio.
services: sql-database
ms.service: sql-database
ms.subservice: ''
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: CarlRabeler
ms.author: carlrab
ms.reviewer: sashan, moslake
manager: craigg
ms.date: 09/26/2018
ms.openlocfilehash: 3fb1357b7a70579fa527a896d5bd359749b10ad6
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2018
ms.locfileid: "47407662"
---
# <a name="choosing-a-vcore-service-tier-compute-memory-storage-and-io-resources"></a>Selección del nivel de servicio basado en núcleos virtuales, y los recursos de proceso, memoria, almacenamiento y E/S

El modelo de compra basado en núcleos virtuales permite escalar los recursos de proceso y almacenamiento de manera independiente, igualar el rendimiento local y optimizar el precio. También le permite elegir la generación del hardware:
- Gen 4: hasta 24 CPU lógicas basadas en procesadores Intel E5-2673 v3 (Haswell) de 2,4 GHz, núcleo virtual = 1 PP (núcleo físico), 7 GB por núcleo, SSD conectada
- Gen 5: hasta 80 CPU lógicas basadas en procesadores Intel E5-2673 v4 (Broadwell) de 2,3 GHz, núcleo virtual = 1 LP (hyper-thread), 5,5 GB por núcleo, SSD eNVM rápido

El modelo de núcleo virtual también permite usar [Ventaja híbrida de Azure para SQL Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md) para ahorrar en los costos.

## <a name="service-tier-characteristics"></a>Características del nivel de servicios

El modelo de núcleo virtual ofrece dos niveles de servicio: De uso General y Crítico para la empresa. Los niveles de servicio se diferencian por una variedad de tamaños de proceso, un diseño de alta disponibilidad, el aislamiento de errores, los tipos de almacenamiento y el intervalo de E/S. El cliente debe configurar por separado el periodo necesario de retención y almacenamiento de las copias de seguridad.

La tabla siguiente le ayudará a comprender las diferencias entre estos dos niveles:

||**Uso general**|**Crítico para la empresa**|**Hiperescala (versión preliminar)**|
|---|---|---|---|
|Más adecuado para|La mayoría de las cargas de trabajo empresariales. Ofrece opciones de proceso y almacenamiento equilibradas y escalables orientadas al presupuesto.|Aplicaciones empresariales con elevados requisitos de E/S. Ofrece la máxima resistencia a errores mediante varias réplicas aisladas.|La mayoría de las cargas de trabajo de una empresa que tengan requisitos de almacenamiento y un escalado de lectura que sean altamente escalables.|
|Compute|Gen4: de 1 a 24 núcleos virtuales<br/>Gen5: de 1 a 80 núcleos virtuales|Gen4: de 1 a 24 núcleos virtuales<br/>Gen5: de 1 a 80 núcleos virtuales|Gen4: de 1 a 24 núcleos virtuales<br/>Gen5: de 1 a 80 núcleos virtuales|
|Memoria|Gen4: 7 GB por núcleo<br>Gen5: 5,5 GB por núcleo | Gen4: 7 GB por núcleo<br>Gen5: 5,5 GB por núcleo |Gen4: 7 GB por núcleo<br>Gen5: 5,5 GB por núcleo|
|Storage|[Premium Storage (remoto)](../virtual-machines/windows/premium-storage.md),<br/>Base de datos única: de 5 GB a 4 TB<br/>Instancia administrada: de 32 GB a 8 TB |Almacenamiento local de SSD,<br/>Base de datos única: de 5 GB a 4 TB<br/>Instancia administrada: de 32 GB a 4 TB |Flexible; crecimiento automático de almacenamiento según sea necesario. Admite hasta 100 TB de almacenamiento y mucho más. Almacenamiento SSD local para la caché del grupo de búferes local y el almacenamiento de datos local. Almacenamiento remoto de Azure como almacén de datos final a largo plazo. |
|Rendimiento de E/S (aproximado)|Base de datos única: 500 IOPS por núcleo virtual con 7000 IOPS como máximo</br>Instancia administrada: depende del [tamaño de archivo](../virtual-machines/windows/premium-storage-performance.md#premium-storage-disk-sizes)|5000 IOPS por núcleo con 200 000 IOPS como máximo|TBD|
|Disponibilidad|1 réplica, sin escalado de lectura|3 réplicas, 1 [réplica de escalado de lectura](sql-database-read-scale-out.md),<br/>Con alta disponibilidad y redundancia de zona|?|
|Copias de seguridad|[RA-GRS](../storage/common/storage-designing-ha-apps-with-ragrs.md), de 7 a 35 días (7 días de forma predeterminada)|[RA-GRS](../storage/common/storage-designing-ha-apps-with-ragrs.md), de 7 a 35 días (7 días de forma predeterminada)|Copias de seguridad basadas en instantáneas que se encuentran en el almacenamiento remoto de Azure, y restauraciones que usan esas instantáneas para una recuperación rápida. Las copias de seguridad son instantáneas y no afectan el rendimiento de E/S del proceso. Las restauraciones son muy rápidas y no son del tamaño de las operaciones de datos (en minutos, no en horas/días).|
|En memoria|No compatible|Compatible|No compatible|
|||

Para obtener más información, consulte [vCore resource limits in Single database](sql-database-vcore-resource-limits-single-databases.md) (Límites de recursos de núcleo virtual en una base de datos única) y [Límites de recursos de núcleo virtual en una Instancia administrada](sql-database-managed-instance.md#vcore-based-purchasing-model). 

> [!IMPORTANT]
> Si tiene menos de un núcleo virtual de capacidad de proceso, use el modelo de compra basado en DTU.

Consulte [Preguntas más frecuentes sobre SQL Database](sql-database-faq.md) para conocer las respuestas a las preguntas más frecuentes. 

## <a name="storage-considerations"></a>Consideraciones sobre el almacenamiento

### <a name="general-purpose-and-business-critical-service-tiers"></a>Niveles de servicio de uso general y críticos para la empresa.
Tenga en cuenta lo siguiente.
- El almacenamiento asignado lo usan los archivos de datos (MDF) y los archivos de registros (LDF).
- Cada tamaño del proceso de una base de datos única admite un tamaño máximo de base de datos, con un tamaño máximo predeterminado de 32 GB.
- Cuando se configura el tamaño de base de datos única requerido (tamaño de MDF), el 30 % de almacenamiento adicional se agrega automáticamente para admitir LDF
- El tamaño de almacenamiento en la instancia administrada debe especificarse en múltiplos de 32 GB.
- Puede seleccionar cualquier tamaño de base de datos única entre 10 GB y el máximo admitido.
 - Para Standard Storage, aumente o disminuya el tamaño en incrementos de 10 GB.
 - Para Premium Storage, aumente o disminuya el tamaño en incrementos de 250 GB.
- En el nivel de servicio De uso general, `tempdb` usa una SSD asociada y este costo de almacenamiento se incluye en el precio del núcleo virtual.
- En el nivel de servicio Crítico para la empresa, `tempdb` comparte la SSD asociada con los archivos MDF y LDF y el costo de almacenamiento de tempDB se incluye en el precio del núcleo virtual.

> [!IMPORTANT]
> Se le cobra por el almacenamiento total asignado de MDF y LDF.

Para supervisar el tamaño total actual de MDF y LDF, use [sp_spaceused](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-spaceused-transact-sql). Para supervisar el tamaño actual de los archivos MDF y LDF individuales, use [sys.database_files](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-database-files-transact-sql).

> [!IMPORTANT]
> En algunas circunstancias, puede que deba reducir una base de datos para reclamar el espacio no utilizado. Para más información, consulte [Administración del espacio de archivo en Azure SQL Database](sql-database-file-space-management.md).

### <a name="hyperscale-service-tier-preview"></a>Nivel de servicio de hiperescala (versión preliminar)

Almacenamiento que se administra de forma automática en la base de datos de hiperescala. Almacenamiento que crece según sea necesario. Almacenamiento de tipo "registro infinito" en los SSD de almacenamiento rápido de Azure premium que no tiene la necesidad de truncar el registro con frecuencia.

## <a name="backups-and-storage"></a>Copias de seguridad y almacenamiento

### <a name="general-purpose-and-business-critical-service-tiers"></a>Niveles de servicio de uso general y críticos para la empresa.

Para admitir las funcionalidades Restauración a un momento dado (PITR) y [Retención a largo plazo (LTR)](sql-database-long-term-retention.md) de SQL Database, se asigna almacenamiento a las copias de seguridad de base de datos. Este almacenamiento se asigna por separado para cada base de datos y se factura como dos cargos independientes por base de datos. 

- **PITR**: las copias de seguridad de base de datos individuales se copian en el [almacenamiento de RA-GRS](../storage/common/storage-designing-ha-apps-with-ragrs.md) automáticamente. El tamaño de almacenamiento aumenta dinámicamente a medida que se crean las nuevas copias de seguridad.  El almacenamiento se usa para realizar cada cinco minutos copias de seguridad completas semanales, copias de seguridad diferenciales diarias y copias de seguridad de registros de transacciones. El consumo de almacenamiento depende de la tasa de cambio de la base de datos y del período de retención. Puede configurar un período de retención diferente para cada base de datos de entre 7 y 35 días. Se ofrece una cantidad de almacenamiento mínimo igual al tamaño de los datos sin costo adicional. En la mayoría de las bases de datos, esta cantidad es suficiente para almacenar copias de seguridad durante 7 días.
- **LTR**: SQL Database ofrece la opción de configurar la retención a largo plazo de copias de seguridad completas durante un período máximo de 10 años. Si la directiva de LTR está habilitada, estas copias de seguridad se almacenan en almacenamiento RA-GRS automáticamente, pero puede controlar la frecuencia con que se realizan las copias de seguridad. Para satisfacer los distintos requisitos de cumplimiento, puede seleccionar distintos períodos de retención para copias de seguridad semanales, mensuales o anuales. Esta configuración definirá la cantidad de almacenamiento que se usará para las copias de seguridad de LTR. Para estimar el costo del almacenamiento de LTR, se puede usar la calculadora de precios de LTR. Para más información, consulte [retención a largo plazo](sql-database-long-term-retention.md).

### <a name="hyperscale-service-tier-preview"></a>Nivel de servicio de hiperescala (versión preliminar)

En el nivel de servicio de hiperescala, las copias de seguridad están basadas en instantáneas y se almacenan en el almacenamiento remoto de Azure. Los procesos de restauración usan estas instantáneas para conseguir una recuperación rápida. Las copias de seguridad son instantáneas y no afectan el rendimiento de E/S del proceso. Las restauraciones son muy rápidas y no son del tamaño de las operaciones de datos (en minutos, no en horas/días).

## <a name="azure-hybrid-use-benefit"></a>Ventaja de uso híbrido de Azure

En el modelo de compra basado en núcleos virtuales, puede intercambiar sus licencias existentes por tarifas de descuento en SQL Database mediante la [Ventaja híbrida de Azure para SQL Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md). Esta ventaja de Azure le permite usar las licencias de SQL Server locales para ahorrar hasta un 30 % en Azure SQL Database al utilizarlas con Software Assurance.

![Precios](./media/sql-database-service-tiers/pricing.png)

## <a name="migration-from-dtu-model-to-vcore-model"></a>Migración del modelo DTU al modelo con núcleo virtual

### <a name="migration-of-single-databases-with-geo-replication-links"></a>Migración de bases de datos únicas con vínculos de replicación geográfica

La migración del modelo basado en DTU al modelo basado en núcleos virtuales es similar a actualizar o degradar las relaciones de replicación geográfica entre las bases de datos Estándar y Premium. No es necesario terminar la replicación geográfica, pero el usuario debe respetar las reglas de secuenciación. Al actualizar, debe actualizar primero la base de datos secundaria y luego la principal. Al degradar, invierta el orden; es decir, debe degradar primero la base de datos principal y luego la secundaria. 

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

Puede copiar cualquier base de datos con un tamaño de proceso basado en DTU en una base de datos que tenga un tamaño de proceso basado en núcleos virtuales sin restricciones o secuencias especiales, siempre y cuando el tamaño de proceso de destino admita el tamaño máximo de base de datos de la base de datos de origen. Esto se debe a que la copia de base de datos crea una instantánea de los datos a partir de la hora de inicio de la operación de copia y no realiza la sincronización de datos entre el origen y el destino. 

## <a name="next-steps"></a>Pasos siguientes

- Para obtener más información sobre las opciones de tamaños de proceso y de tamaños de almacenamiento específicas que hay disponibles para las bases de datos únicas, consulte [SQL Database vCore-based resource limits for single databases](sql-database-vcore-resource-limits-single-databases.md#single-database-storage-sizes-and-compute-sizes) (Límites de recursos basados en núcleos virtuales de SQL Database para bases de datos únicas).
- Para más información sobre las opciones de tamaño de proceso y de tamaño de almacenamiento específicas que hay disponibles para grupos elásticos, consulte [SQL Database vCore-based resource limits for elastic pools](sql-database-vcore-resource-limits-elastic-pools.md#elastic-pool-storage-sizes-and-compute-sizes) (Límites de recursos basados en núcleos virtuales de Azure SQL Database para grupos elásticos).
