---
title: Modelos de compra de Azure SQL Database | Microsoft Docs
description: Obtenga información acerca del modelo de compra de modelos que son bases de datos disponibles en el servicio de Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: ''
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 12/10/2018
ms.openlocfilehash: 9d566c0f95325635c5ce5030f4d3b22dba7ceb08
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/21/2018
ms.locfileid: "53726038"
---
# <a name="azure-sql-database-purchasing-models"></a>Modelos de compra de Azure SQL Database

Azure SQL Database le permite comprar fácilmente motores de base de datos PaaS completamente administrados que se ajusten a sus necesidades de rendimiento y costos. Según el modelo de implementación de Azure SQL Database, puede seleccionar el modelo de compra que se adapte a sus necesidades:
- [Modelo de compra basado en núcleos virtuales](sql-database-service-tiers-vcore.md) (recomendado) que le permite elegir la cantidad exacta de la capacidad de almacenamiento y proceso que necesita para la carga de trabajo.
- [Modelo de compra basado en DTU](sql-database-service-tiers-dtu.md) donde puede elegir paquetes de proceso y almacenamiento agrupados equilibrados para cargas de trabajo comunes.

Hay disponibles distintos modelos de compra en los modelos de implementación de Azure SQL Database:
- Los [servidores lógicos](sql-database-logical-servers.md) en [Azure SQL Database](sql-database-technical-overview.md) ofrecen el [modelo de compra basado en DTU](sql-database-service-tiers-dtu.md) y el [modelo de compra basado en núcleos virtuales](sql-database-service-tiers-vcore.md). En este modelo de compra, puede elegir [bases de datos únicas](sql-database-single-databases-manage.md) o [grupos elásticos](sql-database-elastic-pool.md).
- Las [Instancias administradas](sql-database-managed-instance.md) de Azure SQL Database solo ofrecen el [modelo de compra basado en núcleos virtuales](sql-database-service-tiers-vcore.md).

> [!IMPORTANT]
> Las [bases de datos de hiperescala (versión preliminar)](sql-database-service-tier-hyperscale.md) están en versión preliminar pública solo para las bases de datos únicas que usan el modelo de compra basado en núcleos virtuales.

En la tabla y el gráfico siguientes se comparan y contrastan estos dos modelos de compra.

|**Modelo de compra**|**Descripción**|**Más adecuado para**|
|---|---|---|
|Modelo basado en DTU|Este modelo se basa en una medida agrupada de recursos de proceso, almacenamiento y E/S. Los tamaños de proceso se expresan como unidades de transacción de base de datos (DTU) para las bases de datos únicas y como unidades de transacción de base de datos elásticas (eDTU) para los grupos elásticos. Para más información sobre las DTU y las eDTU, consulte [¿Qué son las DTU y las eDTU?](sql-database-service-tiers.md#dtu-based-purchasing-model)|Recomendado para los clientes que desean opciones de recursos simples y configuradas previamente.|
|Modelo basado en núcleos virtuales|Este modelo le permite elegir los recursos de proceso y almacenamiento de manera independiente. También permite usar Ventaja híbrida de Azure para SQL Server para ahorrar en los costos.|Recomendado para los clientes que valoran la flexibilidad, el control y la transparencia.|
||||  

![Modelo de precios](./media/sql-database-service-tiers/pricing-model.png)

## <a name="vcore-based-purchasing-model"></a>Modelo de compra basado en núcleos virtuales

Un núcleo virtual representa la CPU lógica que ofrece una opción para elegir entre varias generaciones de hardware y las características físicas de hardware (por ejemplo, el número de núcleos, memoria, el tamaño de almacenamiento). El modelo de compra basado en núcleos virtuales le ofrece flexibilidad, control, transparencia de consumo de recursos individuales y una manera sencilla de trasladar los requisitos de carga de trabajo locales a la nube. Este modelo le permite escalar los recursos de proceso, memoria y almacenamiento en función de las necesidades de la carga de trabajo. En el modelo de compra basado en núcleos virtuales, puede elegir entre los niveles de servicio [De uso general](sql-database-high-availability.md#basic-standard-and-general-purpose-service-tier-availability) y [Crítico para la empresa](sql-database-high-availability.md#premium-and-business-critical-service-tier-availability) para [bases de datos únicas](sql-database-single-database-scale.md), [instancias administradas](sql-database-managed-instance.md) y [grupos elásticos](sql-database-elastic-pool.md). Para las bases de datos únicas, también puede elegir el nivel de servicio [Hiperescala (versión preliminar)](sql-database-service-tier-hyperscale.md).

El modelo de compra basado en núcleos virtuales permite elegir los recursos de proceso y almacenamiento de manera independiente, igualar el rendimiento local y optimizar el precio. En un modelo de compra basado en núcleos virtuales, los clientes pagan por:

- Proceso (nivel de servicio + número de núcleos virtuales y cantidad de memoria + generación de hardware)
- Tipo y cantidad de almacenamiento de datos y registros
- Almacenamiento de copia de seguridad (RA-GRS)

> [!IMPORTANT]
> Los recursos de proceso, E/S y almacenamiento de datos y registros se cobran por base de datos o grupo elástico. El almacenamiento de copia de seguridad se cobra por cada base de datos. Para más información sobre los precios de Instancia administrada, consulte [Instancia administrada de Azure SQL Database](sql-database-managed-instance.md).
> **Limitaciones regionales:** El modelo de compra basado en núcleos virtuales aún no está disponible en las regiones siguientes: Europa Occidental, Centro de Francia, Sur de Reino Unido, Oeste de Reino Unido y Sudeste de Australia.

Si la base de datos o el grupo elástico consumen más de 300 DTU, la conversión a núcleos virtuales puede reducir los costos. Puede realizar la conversión mediante la API de su elección o usar Azure Portal, sin experimentar tiempo de inactividad. Sin embargo, la conversión no es necesaria. Si el modelo de compra de DTU satisface sus requisitos de rendimiento y empresariales, debe seguir usándolo. Si decide pasar del modelo de DTU al modelo de núcleos virtuales, debe seleccionar el tamaño de proceso mediante la siguiente regla general: cada 100 DTU del nivel Estándar requieren al menos 1 núcleo virtual en el nivel De uso general, y cada 125 DTU del nivel Premium requieren como mínimo 1 núcleo virtual en el nivel Crítico para la empresa.

## <a name="dtu-based-purchasing-model"></a>Modelo de compra basado en DTU

La unidad de transacción de base de datos (DTU) representa una medida combinada de CPU, memoria, lecturas y escrituras. El modelo de compra basado en DTU ofrece un conjunto de agrupaciones preconfiguradas de recursos de proceso y almacenamiento incluido para impulsar diferentes niveles de rendimiento de la aplicación. Los clientes que prefieren la simplicidad de una agrupación preconfigurada y pagos fijos cada mes pueden encontrar el modelo basado en DTU más adecuado para sus necesidades. En el modelo de compra basado en DTU, los clientes pueden elegir entre los niveles de servicio **Básico**, **Estándar** y **Premium** tanto para [bases de datos únicas](sql-database-single-database-scale.md) como para [grupos elásticos](sql-database-elastic-pool.md). Este modelo de compra no está disponible en [las instancias administradas](sql-database-managed-instance.md).

### <a name="database-transaction-units-dtus"></a>Unidades de transacción de base de datos (DTU)

Para una sola base de datos de Azure SQL en un tamaño de proceso específico dentro de un [nivel de servicio](sql-database-single-database-scale.md), Microsoft garantiza un determinado nivel de recursos para esa base de datos (independiente de cualquier otra base de datos en la nube de Azure), de forma que ofrece un nivel de rendimiento predecible. La cantidad de recursos se calcula como un número de unidades de transacción de base de datos o DTU y es una medida combinada de recursos de proceso, almacenamiento y E/S. La relación entre estos recursos se determinó originalmente mediante una [carga de trabajo OLTP de prueba comparativa](sql-database-benchmark-overview.md) diseñada para representar las típicas cargas de trabajo OLTP reales. Cuando la carga de trabajo supera la cantidad de cualquiera de estos recursos, se limita el rendimiento, con lo que se obtiene un rendimiento y tiempos de espera más lentos. Los recursos usados por la carga de trabajo no afectan a los recursos disponibles para otras bases de datos SQL en la nube de Azure, y los recursos usados por otras cargas de trabajo no afectan a los recursos disponibles para la base de datos SQL.

![rectángulo de selección](./media/sql-database-what-is-a-dtu/bounding-box.png)

Las DTU son especialmente útiles para comprender la cantidad relativa de recursos entre bases de datos de Azure SQL en diferentes tamaños de proceso y niveles de servicio. Por ejemplo, duplicar el número de DTU al aumentar el tamaño de proceso de una base de datos equivale a duplicar el conjunto de recursos disponibles para esa base de datos. Por ejemplo, una base de datos Premium P11 con 1750 DTU proporciona una potencia de proceso de DTU 350 veces mayor que una base de datos básica con 5 DTU.  

Para obtener información más detallada sobre el consumo de recursos (DTU) de la carga de trabajo, use [Información de rendimiento de consultas de Azure SQL Database](sql-database-query-performance.md) para:

- Identificar las consultas principales por CPU, duración y recuento de ejecuciones, que se pueden ajustar para mejorar el rendimiento. Por ejemplo, una consulta que realiza muchas operaciones de E/S puede beneficiarse del uso de [técnicas de optimización en memoria](sql-database-in-memory.md) para hacer un mejor uso de la memoria disponible en un cierto nivel de servicio y tamaño de proceso.
- Profundizar en los detalles de una consulta, ver su texto e historial de uso de recursos.
- Tener acceso a recomendaciones de ajuste del rendimiento que muestran las acciones realizadas por el [SQL Database Advisor](sql-database-advisor.md).

### <a name="elastic-database-transaction-units-edtus"></a>Unidades de transacción de base de datos elástica (eDTU)

En lugar de proporcionar un conjunto exclusivo de recursos (DTU) que puede que no siempre sea necesario para una SQL Database que está siempre disponible, puede colocar las bases de datos en un [grupo elástico](sql-database-elastic-pool.md) en un servidor de SQL Database que comparte un grupo de recursos entre esas bases de datos. Los recursos compartidos de un grupo elástico se miden mediante unidades de transacción de bases de datos elásticas o eDTU. Los grupos elásticos proporcionan una solución sencilla y rentable para administrar los objetivos de rendimiento de varias bases de datos que tienen patrones de uso muy diferentes e imprevisibles. Un grupo elástico garantiza que una base de datos del grupo no puede consumir los recursos, mientras que asegura que cada base de datos del grupo tenga disponible siempre una cantidad mínima de recursos necesarios.

A un grupo se le asigna un número fijo de eDTU, por un precio fijo. Dentro del grupo elástico, a las bases de datos individuales se les proporciona la flexibilidad de escalarse automáticamente dentro de los límites configurados. Una base de datos con cargas más elevadas consumirá más eDTU para satisfacer la demanda. Las bases de datos con cargas más bajas consumirán menos eDTU. Las bases de datos sin cargas no consumirán ningún eDTU. Mediante el aprovisionamiento de recursos para todo el grupo, en lugar de por base de datos, se simplifican las tareas de administración, de forma que se proporciona un presupuesto del grupo predecible.

Se pueden agregar eDTU adicionales a un grupo existente sin que la base de datos experimente tiempo de inactividad ni que las bases de datos del grupo resulten afectadas. De igual forma, si las eDTU adicionales dejan de necesitarse, se pueden quitar de cualquier grupo existente en cualquier momento. Puede agregar o quitar bases de datos del grupo, o limitar la cantidad de eDTU que puede usar una base de datos cuando está sobrecargada para reservar eDTU para otras bases de datos. Si una base de datos infrautiliza recursos de forma predecible, es posible sacarla del grupo y configurarla como una base de datos única con una cantidad predecible de recursos necesarios.

### <a name="determine-the-number-of-dtus-needed-by-a-workload"></a>Determinar el número de DTU necesarias para la carga de trabajo

Si desea migrar una carga de trabajo de máquina virtual existente local o de SQL Server en Azure SQL Database, puede usar la [calculadora de DTU](http://dtucalculator.azurewebsites.net/) para hacer una estimación del número aproximado de DTU que se necesitan. Para una carga de trabajo existente de Azure SQL Database, puede usar la [información de rendimiento de consultas de SQL Database](sql-database-query-performance.md) para comprender el consumo de recursos de la base de datos (DTU) y obtener información más detallada para optimizar la carga de trabajo. También puede usar la DMV [sys.dm_db_ resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) para ver el consumo de recursos de la última hora. Como alternativa, la vista de catálogo [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) muestra el consumo de recursos de los últimos 14 días, aunque con una fidelidad inferior media de cinco minutos.

### <a name="workloads-that-benefit-from-an-elastic-pool-of-resources"></a>Cargas de trabajo que se benefician de un grupo elástico de recursos

Los grupos son apropiados para un amplio número de bases de datos con patrones de utilización específicos. Para una base de datos determinada, este patrón está caracterizado por una utilización media baja con picos de utilización relativamente poco frecuentes. SQL Database evalúa automáticamente el historial de uso de recursos de bases de datos en un servidor de SQL Database existente y recomienda la configuración de grupo apropiada en Azure Portal. Para más información, consulte [¿Cuándo se debe utilizar un grupo elástico?](sql-database-elastic-pool.md)

## <a name="next-steps"></a>Pasos siguientes

- Consulte este artículo sobre el [modelo de compra basado en núcleos virtuales](sql-database-service-tiers-vcore.md) si quiere obtener más información al respecto.
- Con respecto al modelo de compra basado en DTU, consulte [Modelo de compra basado en DTU](sql-database-service-tiers-dtu.md).
