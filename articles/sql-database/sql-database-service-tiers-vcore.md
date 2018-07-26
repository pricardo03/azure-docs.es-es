---
title: 'Servicio Azure SQL Database: núcleos virtuales | Microsoft Docs'
description: El modelo de compra basado en núcleos virtuales permite escalar los recursos de proceso y almacenamiento de manera independiente, igualar el rendimiento local y optimizar el precio.
services: sql-database
author: CarlRabeler
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 07/16/2018
manager: craigg
ms.author: carlrab
ms.openlocfilehash: d18076486704d5f03acd2253650762c3bd24b0af
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/17/2018
ms.locfileid: "39091499"
---
# <a name="choosing-a-vcore-service-tier-compute-memory-storage-and-io-resources"></a>Selección del nivel de servicio basado en núcleos virtuales, y los recursos de proceso, memoria, almacenamiento y E/S

Los niveles de servicio se diferencian por una variedad de niveles de rendimiento, el diseño de alta disponibilidad, el aislamiento de errores, los tipos de almacenamiento y el intervalo de E/S. El cliente debe configurar por separado el periodo necesario de retención y almacenamiento de las copias de seguridad. Con el modelo de núcleos virtuales, es posible obtener un ahorro de hasta un 30 % en las bases de datos únicas y los grupos elásticos con la [Ventaja para uso híbrido de Azure para SQL Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md).

La tabla siguiente le ayudará a comprender las diferencias entre estos dos niveles:

||**Uso general**|**Crítico para la empresa**|
|---|---|---|
|Más adecuado para|La mayoría de las cargas de trabajo empresariales. Ofrece opciones de proceso y almacenamiento equilibradas y escalables orientadas al presupuesto.|Aplicaciones empresariales con elevados requisitos de E/S. Ofrece la máxima resistencia a errores mediante varias réplicas aisladas.|
|Compute|De 1 a 80 núcleos virtuales, Gen4 y Gen5 |De 1 a 80 núcleos virtuales, Gen4 y Gen5|
|Memoria|Gen4: 7 GB por núcleo<br>Gen5: 5,5 GB por núcleo | Gen4: 7 GB por núcleo<br>Gen5: 5,5 GB por núcleo |
|Storage|Almacenamiento remoto Premium, de 5 GB a 4 TB|Almacenamiento local de SSD, de 5 GB a 4 TB|
|Rendimiento de E/S (aproximado)|500 IOPS por núcleo virtual con 7000 IOPS como máximo|5000 IOPS por núcleo con 200 000 IOPS como máximo|
|Disponibilidad|1 réplica, sin escalado de lectura|3 réplicas, 1 [escalado de lectura](sql-database-read-scale-out.md), HA con redundancia de zona|
|Copias de seguridad|RA-GRS, de 7 a 35 días (7 días de forma predeterminada)|RA-GRS, de 7 a 35 días (7 días de forma predeterminada)|
|En memoria|N/D|Compatible|
|||

> [!IMPORTANT]
> Si tiene menos de un núcleo virtual de capacidad de proceso, use el modelo de compra basado en DTU.

Consulte [Preguntas más frecuentes sobre SQL Database](sql-database-faq.md) para conocer las respuestas a las preguntas más frecuentes. 

## <a name="storage-considerations"></a>Consideraciones sobre el almacenamiento

Tenga en cuenta lo siguiente.
- El almacenamiento asignado lo usan los archivos de datos (MDF) y los archivos de registros (LDF).
- Cada nivel de rendimiento admite un tamaño máximo de base de datos, con un tamaño máximo predeterminado de 32 GB.
- Cuando se configura el tamaño de base de datos requerido (tamaño de MDF), el 30 % de almacenamiento adicional se agrega automáticamente para admitir LDF.
- Puede seleccionar cualquier tamaño de base de datos entre 10 GB y el máximo admitido.
 - Para Standard Storage, aumente o disminuya el tamaño en incrementos de 10 GB.
 - Para Premium Storage, aumente o disminuya el tamaño en incrementos de 250 GB.
- En el nivel de servicio De uso general, `tempdb` usa una SSD asociada y este costo de almacenamiento se incluye en el precio del núcleo virtual.
- En el nivel de servicio Crítico para la empresa, `tempdb` comparte la SSD asociada con los archivos MDF y LDF y el costo de almacenamiento de tempDB se incluye en el precio del núcleo virtual.

> [!IMPORTANT]
> Se le cobra por el almacenamiento total asignado de MDF y LDF.

Para supervisar el tamaño total actual de MDF y LDF, use [sp_spaceused](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-spaceused-transact-sql). Para supervisar el tamaño actual de los archivos MDF y LDF individuales, use [sys.database_files](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-database-files-transact-sql).

## <a name="backups-and-storage"></a>Copias de seguridad y almacenamiento

Para admitir las funcionalidades Restauración a un momento dado (PITR) y Retención a largo plazo (LTR) de SQL Database, se asigna almacenamiento a las copias de seguridad de base de datos. Este almacenamiento se asigna por separado para cada base de datos y se factura como dos cargos independientes por base de datos. 

- **PITR**: las copias de seguridad de base de datos individuales se copian en el almacenamiento de RA-GRS automáticamente. El tamaño de almacenamiento aumenta dinámicamente a medida que se crean las nuevas copias de seguridad.  El almacenamiento se usa para realizar cada cinco minutos copias de seguridad completas semanales, copias de seguridad diferenciales diarias y copias de seguridad de registros de transacciones. El consumo de almacenamiento depende de la tasa de cambio de la base de datos y del período de retención. Puede configurar un período de retención diferente para cada base de datos de entre 7 y 35 días. Se ofrece una cantidad de almacenamiento mínimo igual al tamaño de los datos sin costo adicional. En la mayoría de las bases de datos, esta cantidad es suficiente para almacenar copias de seguridad durante 7 días.
- **LTR**: SQL Database ofrece la opción de configurar la retención a largo plazo de copias de seguridad completas durante un período máximo de 10 años. Si la directiva de LTR está habilitada, estas copias de seguridad se almacenan en almacenamiento RA-GRS automáticamente, pero puede controlar la frecuencia con que se realizan las copias de seguridad. Para satisfacer los distintos requisitos de cumplimiento, puede seleccionar distintos períodos de retención para copias de seguridad semanales, mensuales o anuales. Esta configuración definirá la cantidad de almacenamiento que se usará para las copias de seguridad de LTR. Para estimar el costo del almacenamiento de LTR, se puede usar la calculadora de precios de LTR. Para más información, consulte [retención a largo plazo](sql-database-long-term-retention.md).

## <a name="azure-hybrid-use-benefit"></a>Ventaja de uso híbrido de Azure

En el modelo de compra basado en núcleos virtuales, puede intercambiar sus licencias existentes por tarifas de descuento en SQL Database mediante la [Ventaja híbrida de Azure para SQL Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md). Esta ventaja de Azure le permite usar las licencias de SQL Server locales para ahorrar hasta un 30 % en Azure SQL Database al utilizarlas con Software Assurance.

![Precios](./media/sql-database-service-tiers/pricing.png)

## <a name="migration-of-single-databases-with-geo-replication-links"></a>Migración de bases de datos únicas con vínculos de replicación geográfica

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

## <a name="migration-of-failover-groups"></a>Migración de grupos de conmutación por error 

La migración de grupos de conmutación por error con varias bases de datos requiere la migración individual de las bases de datos principales y secundarias. Durante ese proceso, se aplican las mismas consideraciones y reglas de secuenciación. Después de que las bases de datos se convierten al modelo basado en núcleos virtuales, el grupo de conmutación por error permanecerá en vigor con la misma configuración de directiva. 

## <a name="creation-of-a-geo-replication-secondary"></a>Creación de una base de datos secundaria de replicación geográfica

Solo puede crear una base de datos secundaria de replicación geográfica con el mismo nivel de servicio que la principal. Para una base de datos con una tasa de generación de registros elevada, es muy recomendable que la base de datos secundaria se cree con el mismo nivel de rendimiento que la principal. Si va a crear una base de datos secundaria con replicación geográfica en el grupo elástico para una única base de datos principal, se recomienda que el valor `maxVCore` del grupo coincida con el nivel de rendimiento de la base de datos principal. Si va a crear una base de datos secundaria con replicación geográfica en el grupo elástico para una principal de otro grupo elástico, se recomienda que los grupos tengan la misma configuración de `maxVCore`.

## <a name="using-database-copy-to-convert-a-dtu-based-database-to-a-vcore-based-database"></a>Uso de una copia de base de datos para convertir una base de datos basada en DTU en una base de datos basada en núcleos virtuales

Puede copiar cualquier base de datos con un nivel de rendimiento basado en DTU en una base de datos con un nivel de rendimiento basado en núcleos virtuales sin restricciones o secuencia especial, siempre y cuando el nivel de rendimiento de destino admita el tamaño máximo de base de datos de la base de datos de origen. Esto se debe a que la copia de base de datos crea una instantánea de los datos a partir de la hora de inicio de la operación de copia y no realiza la sincronización de datos entre el origen y el destino. 

## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre niveles de rendimiento y opciones de tamaño de almacenamiento específicos disponibles para bases de datos únicas, consulte [Límites de recursos basados en núcleos virtuales de SQL Database para bases de datos únicas](sql-database-vcore-resource-limits-single-databases.md#single-database-storage-sizes-and-performance-levels).
- Para más información sobre niveles de rendimiento y opciones de tamaño de almacenamiento específicos disponibles para grupos elásticos, consulte [Límites de recursos basados en núcleos virtuales de Azure SQL Database para grupos elásticos](sql-database-vcore-resource-limits-elastic-pools.md#elastic-pool-storage-sizes-and-performance-levels).
