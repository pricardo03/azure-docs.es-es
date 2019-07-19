---
title: 'Servicio Azure SQL Database: núcleos virtuales | Microsoft Docs'
description: El modelo de compra basado en núcleo virtual permite escalar los recursos de proceso y de almacenamiento de manera independiente, igualar el rendimiento local y optimizar el precio.
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
ms.date: 06/26/2019
ms.openlocfilehash: e9d1ce3bcd3bf958be0a7837e8416300af03f5a2
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2019
ms.locfileid: "67449747"
---
# <a name="choose-among-the-vcore-service-tiers-and-migrate-from-the-dtu-service-tiers"></a>Elija entre los niveles de servicio de núcleo virtual y migre desde los niveles de servicio DTU

El modelo de compra basado en núcleo virtual permite escalar los recursos de proceso y de almacenamiento de manera independiente, igualar el rendimiento local y optimizar el precio. También le permite elegir la generación de hardware:

- **Gen4**: hasta 24 CPU lógicas basadas en procesadores Intel E5-2673 v3 (Haswell) 2.4-GHz, núcleo virtual = 1 PP (núcleo físico), 7 GB por núcleo, SSD adjunta
- **Gen5**: hasta 80 CPU lógicas basadas en procesadores Intel E5-2673 v4 (Broadwell) 2.3-GHz, núcleo virtual = 1 LP (hyper-thread), 5,1 GB por núcleo, eNVM SSD rápida

El hardware de Gen4 ofrece bastante más memoria por núcleo virtual. Sin embargo, el hardware de Gen5 permite escalar verticalmente mucho más alto los recursos de proceso.

> [!IMPORTANT]
> Las nuevas bases de datos de Gen4 ya no se admiten en la región AustraliaEast.
> [!NOTE]
> Para más información sobre los niveles de servicio basados en DTU, consulte el [Niveles de servicio en el modelo de compra basado en DTU](sql-database-service-tiers-dtu.md). Para información sobre las diferencias entre los niveles de servicio basados en DTU y los modelos de compra basados en núcleos virtuales, consulte [Modelos de compra de Azure SQL Database](sql-database-purchase-models.md).

## <a name="service-tier-characteristics"></a>Características del nivel de servicio

El modelo de compra basado en núcleo virtual ofrece tres niveles de servicio: de uso general, hiperescala y crítico para la empresa. Estos niveles de servicio se diferencian por una variedad de tamaños de proceso, diseños de alta disponibilidad, métodos de aislamiento de errores, tipos y tamaños de almacenamiento e intervalos de E/S.

Debe configurar por separado el periodo necesario de retención y almacenamiento de las copias de seguridad. Para establecer el período de retención de copia de seguridad, abra Azure Portal, vaya al servidor (no a la base de datos) y, a continuación, vaya a **Administrar copias de seguridad** > **Configurar directiva** > **Configuración de restauración a un momento dado** > **7 a 35 días**.

La tabla siguiente explica las diferencias entre los tres niveles:

||**Uso general**|**Crítico para la empresa**|**Hiperescala**|
|---|---|---|---|
|Más adecuado para|La mayoría de las cargas de trabajo empresariales. Ofrece opciones de proceso y almacenamiento equilibradas y escalables pensando en el presupuesto.|Aplicaciones empresariales con elevados requisitos de E/S. Ofrece la máxima resistencia a errores mediante el uso de varias réplicas aisladas.|La mayoría de las cargas de trabajo de una empresa que tengan requisitos altamente escalables de almacenamiento y escalado de lectura.|
|Proceso|**Proceso aprovisionado**:<br/>Gen4: 1 a 24 núcleos virtuales<br/>Gen5: 2 a 80 núcleos virtuales<br/>**Proceso sin servidor**:<br/>Gen5: 0,5 a 4 núcleos virtuales|**Proceso aprovisionado**:<br/>Gen4: 1 a 24 núcleos virtuales<br/>Gen5: 2 a 80 núcleos virtuales|**Proceso aprovisionado**:<br/>Gen4: 1 a 24 núcleos virtuales<br/>Gen5: 2 a 80 núcleos virtuales|
|Memoria|**Proceso aprovisionado**:<br/>Gen4: 7 GB por núcleo virtual<br/>Gen5: 5,1 GB por núcleo virtual<br/>**Proceso sin servidor**:<br/>Gen5: 3 GB por núcleo virtual|**Proceso aprovisionado**:<br/>Gen4: 7 GB por núcleo virtual<br/>Gen5: 5,1 GB por núcleo virtual |**Proceso aprovisionado**:<br/>Gen4: 7 GB por núcleo virtual<br/>Gen5: 5,1 GB por núcleo virtual|
|Storage|Usa el almacenamiento remoto.<br/>**Proceso aprovisionado con una base de datos única**:<br/>5 GB – 4 TB<br/>**Proceso sin servidor con una base de datos única**:<br/>5 GB - 1 TB<br/>**Instancia administrada**: 32 GB - 8 TB |Usa almacenamiento local de SSD.<br/>**Proceso aprovisionado con una base de datos única**:<br/>5 GB – 4 TB<br/>**Instancia administrada**:<br/>32 GB - 4 TB |Crecimiento automático flexible de almacenamiento según sea necesario. Admite hasta 100 TB de almacenamiento. Utiliza almacenamiento SSD local para la caché del grupo de búferes local y almacenamiento de datos local. Utiliza almacenamiento remoto de Azure como almacén de datos final a largo plazo. |
|Rendimiento de E/S (aproximado)|**Base de datos única**: 500 IOPS por núcleo virtual con 7000 IOPS como máximo.<br/>**Instancia administrada**: Depende del [tamaño del archivo](../virtual-machines/windows/premium-storage-performance.md#premium-storage-disk-sizes).|5000 IOPS por núcleo con 200 000 IOPS como máximo|Hiperescala es una arquitectura de varios niveles con almacenamiento en caché en varios niveles. Los IOPS efectivos dependen de la carga de trabajo.|
|Disponibilidad|1 réplica, sin réplicas de escalado de lectura|3 réplicas, 1 [réplica de escalado de lectura](sql-database-read-scale-out.md),<br/>Alta disponibilidad (HA) con redundancia de zona|1 réplica de lectura y escritura, además de 0 a 4 [réplicas de escalado de lectura](sql-database-read-scale-out.md)|
|Copias de seguridad|[Almacenamiento con redundancia geográfica con acceso de lectura (RA-GRS)](../storage/common/storage-designing-ha-apps-with-ragrs.md), de 7 a 35 días (7 días de forma predeterminada)|[RA-GRS](../storage/common/storage-designing-ha-apps-with-ragrs.md), de 7 a 35 días (7 días de forma predeterminada)|Copias de seguridad basadas en instantáneas en el almacenamiento remoto de Azure. Los procesos de restauración usan estas instantáneas para conseguir una recuperación rápida. Las copias de seguridad son instantáneas y no afectan al rendimiento de E/S del proceso. Las restauraciones son rápidas y no son operaciones relacionadas con el tamaño de los datos (tardan minutos en lugar de horas o días).|
|En memoria|No compatible|Compatible|No compatible|
|||

> [!NOTE]
> Puede obtener una base de datos de Azure SQL gratuita en el nivel de servicio Básico junto con una cuenta gratuita de Azure. Para más información, consulte [Cree una base de datos administrada en la nube con su cuenta gratuita de Azure](https://azure.microsoft.com/free/services/sql-database/).

- Para más información, sobre los límites de recurso de un núcleo virtual consulte [Límites de recursos de núcleo virtual para una base de datos única](sql-database-vcore-resource-limits-single-databases.md) y [Límites de recursos de núcleo virtual en una instancia administrada](sql-database-managed-instance.md#vcore-based-purchasing-model).
- Para más información acerca de los niveles de servicio uso general y crítico para la empresa, consulte [Niveles de servicio de uso general y crítico para la empresa](sql-database-service-tiers-general-purpose-business-critical.md).
- Para más información sobre el nivel de servicio hiperescala en el modelo de compra basado en núcleo virtual, consulte [Nivel de servicio Hiperescala](sql-database-service-tier-hyperscale.md).  

## <a name="azure-hybrid-benefit"></a>Ventaja híbrida de Azure

En el nivel de proceso aprovisionado del modelo de compra basado en núcleo virtual, puede intercambiar sus licencias existentes por tarifas de descuento en SQL Database mediante la [Ventaja híbrida de Azure para SQL Server](https://azure.microsoft.com/pricing/hybrid-benefit/). Esta ventaja de Azure le permite ahorrar hasta un 30 % en Azure SQL Database al utilizar las licencias de SQL Server locales con Software Assurance.

![Precios](./media/sql-database-service-tiers/pricing.png)

Con la Ventaja híbrida de Azure, puede elegir pagar solo por la infraestructura subyacente de Azure mediante el uso de la licencia existente de SQL Server para el motor de base de datos SQL (precios de proceso básicos), o pagar tanto por la infraestructura subyacente como por la licencia de SQL Server (precio con licencia incluida).

Puede elegir o cambiar el modelo de licencia mediante Azure Portal o con una de las siguientes API:

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

- Para establecer o actualizar el tipo de licencia mediante la API REST:

  - [Databases - Create Or Update](https://docs.microsoft.com/rest/api/sql/databases/createorupdate)
  - [Databases - Update](https://docs.microsoft.com/rest/api/sql/databases/update)
  - [Managed Instances - Create Or Update](https://docs.microsoft.com/rest/api/sql/managedinstances/createorupdate)
  - [Managed Instances - Update](https://docs.microsoft.com/rest/api/sql/managedinstances/update)

## <a name="migrate-from-the-dtu-based-model-to-the-vcore-based-model"></a>Migrar desde el modelo basado en DTU al modelo basado en núcleos virtuales

### <a name="migrate-a-database"></a>Migración de una base de datos

La migración de una base de datos del modelo de compra basado en DTU al modelo de compra basado en núcleo virtual es similar a la actualización o degradación entre los niveles de servicio Estándar y Premium en el modelo de compra basado en DTU.

### <a name="migrate-databases-with-geo-replication-links"></a>Migración de bases de datos con vínculos de replicación geográfica

La migración desde el modelo basado en DTU al modelo basado en núcleos virtuales es similar a la actualización o degradación de las relaciones de replicación geográfica entre las bases de datos en los niveles de servicio Estándar y Premium. Durante la migración, no tendrá que detener la replicación geográfica, pero tiene que seguir estas reglas de secuenciación:

- Al actualizar, debe actualizar primero la base de datos secundaria y luego la principal.
- Al degradar, invierta el orden; es decir, debe degradar primero la base de datos principal y luego la secundaria.

Cuando se usa la replicación geográfica entre dos grupos elásticos, se recomienda designar un grupo como principal y otro como secundario. En ese caso, cuando migre grupos elásticos debe usar la misma guía de secuenciación. Sin embargo, si tiene grupos elásticos que contienen bases de datos principales y secundarias, trate al grupo con la utilización más alta como principal y siga las reglas de secuenciación como corresponda.  

En la tabla siguiente se proporciona una guía para escenarios de migración específicos:

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

\* Cada 100 DTU en el nivel estándar requieren como mínimo 1 núcleo virtual, y cada 125 DTU en el nivel premium requieren como mínimo 1 núcleo virtual.

### <a name="migrate-failover-groups"></a>Migración de grupos de conmutación por error

La migración de grupos de conmutación por error con varias bases de datos requiere la migración individual de las bases de datos principales y secundarias. Durante ese proceso, se aplican las mismas consideraciones y reglas de secuenciación. Después de que las bases de datos se convierten al modelo de compra basado en núcleo virtual, el grupo de conmutación por error permanecerá en vigor con la misma configuración de directiva.

### <a name="create-a-geo-replication-secondary-database"></a>Creación de una base de datos secundaria de replicación geográfica

Puede crear una base de datos secundaria de replicación geográfica (geo-secundaria) solo con el mismo nivel de servicio que utilizó para la base de datos principal. Para las bases de datos con una tasa alta de generación de registro, se recomienda crear la replicación geográfica secundaria con el mismo tamaño de proceso que la principal.

Si va a crear una base de datos secundaria de replicación geográfica en el grupo elástico para una única base de datos principal, asegúrese de que el valor `maxVCore` del grupo coincida con el tamaño de proceso de la base de datos principal. Si va a crear una base de datos secundaria de replicación geográfica para una principal en otro grupo elástico, se recomienda que los grupos tengan la misma configuración de `maxVCore`.

### <a name="use-database-copy-to-convert-a-dtu-based-database-to-a-vcore-based-database"></a>Uso de una copia de base de datos para convertir una base de datos basada en DTU en una base de datos basada en núcleos virtuales

Puede copiar cualquier base de datos con un tamaño de proceso basado en DTU en una base de datos que tenga un tamaño de proceso basado en núcleos virtuales sin restricciones o secuencias especiales, siempre y cuando el tamaño de proceso de destino admita el tamaño máximo de base de datos de la base de datos de origen. La copia de base de datos crea una instantánea de los datos a partir de la hora de inicio de la operación de copia y no sincroniza los datos entre el origen y el destino.

## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre las opciones de tamaños de proceso y de almacenamiento específicas que hay disponibles para las bases de datos únicas, consulte [Límites de recursos basados en núcleos virtuales de SQL Database para bases de datos únicas](sql-database-vcore-resource-limits-single-databases.md).
- Para más información sobre las opciones de tamaño de proceso y de almacenamiento específicas que hay disponibles para los grupos elásticos, consulte [Límites de recursos basados en núcleos virtuales de Azure SQL Database para grupos elásticos](sql-database-vcore-resource-limits-elastic-pools.md#general-purpose-service-tier-storage-sizes-and-compute-sizes).
