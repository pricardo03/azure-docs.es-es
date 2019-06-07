---
title: Modelos de compra de Azure SQL Database | Microsoft Docs
description: Obtenga información sobre los modelos de compra que están disponibles para Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: carlrab
manager: craigg
ms.date: 04/26/2019
ms.openlocfilehash: 98c19732c372fbcda3ca8e746d002f94c2687b22
ms.sourcegitcommit: 087ee51483b7180f9e897431e83f37b08ec890ae
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66431374"
---
# <a name="choose-between-the-vcore-and-the-dtu-purchasing-models"></a>Elija entre la memoria con núcleo virtual y lo modelos de compra de DTU

Azure SQL Database le permite comprar fácilmente una plataforma totalmente administrada como un motor de base de datos de servicio (PaaS) que se adapte a sus necesidades de rendimiento y costo. Según el modelo de implementación que ha elegido para la base de datos de SQL Azure, puede seleccionar el modelo de compra que le convenga:

- [Virtual (núcleo)-modelo de compra basado en](sql-database-service-tiers-vcore.md) (recomendado). Este modelo de compra permite elegir entre un nivel de proceso aprovisionada y un nivel de proceso sin servidor (versión preliminar). Con el nivel de proceso aprovisionada, elige la cantidad exacta de los recursos de proceso que siempre se aprovisionan para la carga de trabajo. Con el nivel de proceso sin servidor, debe especificar el escalado automático de los recursos de proceso a través de un intervalo configurable de proceso. También automáticamente con este nivel de proceso, puede pausar y reanudar la base de datos según la actividad de carga de trabajo. El precio unitario de núcleos virtuales por unidad de tiempo es inferior en el nivel de proceso aprovisionada que se encuentra en el nivel de proceso sin servidor.
- [Unidad de transacción de base de datos (DTU)-modelo de compra basado en](sql-database-service-tiers-dtu.md). Este modelo de compra proporciona paquetes de proceso y almacenamiento integrados con equilibrio de cargas de trabajo comunes.

Existen diferentes modelos de compra para diferentes modelos de implementación de Azure SQL Database:

- Las opciones de implementación de [base de datos única](sql-database-single-databases-manage.md) y [grupo elástico](sql-database-elastic-pool.md) de [Azure SQL Database](sql-database-technical-overview.md) ofrecen ambas el [modelo de compra basado en DTU](sql-database-service-tiers-dtu.md) y el [modelo de compra basado en núcleo virtual](sql-database-service-tiers-vcore.md).
- El [instancia administrada](sql-database-managed-instance.md) opción de implementación de Azure SQL Database ofrece solo la [modelo de compra basado en núcleos virtuales](sql-database-service-tiers-vcore.md).
- El [nivel de servicio de hiperescala](sql-database-service-tier-hyperscale.md) está disponible para bases de datos únicas que usan el [modelo de compra basado en núcleos virtuales](sql-database-service-tiers-vcore.md).

La siguiente tabla y gráfico de comparan y contrastar basado en núcleos virtuales y los modelos de compra basado en DTU:

|**Modelo de compra**|**Descripción**|**Más adecuado para**|
|---|---|---|
|Modelo basado en DTU|Este modelo se basa en una medida combinada de proceso, almacenamiento y recursos de E/S. Tamaños de proceso se expresan en Dtu para bases de datos únicas y en unidades de transacción de bases de datos elásticas (Edtu) para los grupos elásticos. Para obtener más información sobre Dtu y Edtu, consulte [¿cuáles son las Dtu y Edtu?](sql-database-purchase-models.md#dtu-based-purchasing-model).|Recomendado para los clientes que desean simple, opciones de recursos preconfigurada.|
|Modelo basado en núcleos virtuales|Este modelo le permite elegir los recursos de proceso y almacenamiento de manera independiente. El modelo de compra basado en núcleo virtual también le permite usar la [Ventaja híbrida de Azure para SQL Server](https://azure.microsoft.com/pricing/hybrid-benefit/) para ahorrar en los costos.|Recomendado para los clientes que valoran la flexibilidad, el control y la transparencia.|
||||  

![comparación de los modelos de precios](./media/sql-database-service-tiers/pricing-model.png)

## <a name="compute-costs"></a>Costos de proceso

### <a name="provisioned-compute-costs"></a>Costos de proceso aprovisionada

En el nivel de proceso aprovisionada, el costo de proceso refleja la capacidad de proceso total que se ha aprovisionado para la aplicación.

En el nivel de servicio Crítico para la empresa, se asignan automáticamente como mínimo tres réplicas. Para reflejar esta asignación de recursos de proceso adicional, el precio en el modelo de compra basado en núcleos virtuales es aproximadamente 2,7 x mayor en el nivel de servicio crítico de negocio que se encuentra en el nivel de servicio de uso general. Del mismo modo, el precio más alto de almacenamiento por GB en el nivel de servicio crítico del negocio refleja la E/S alta y baja latencia del almacenamiento SSD.

El costo del almacenamiento de copia de seguridad es el mismo para el nivel de servicio crítico del negocio y el nivel de servicio de uso general porque ambos niveles usan almacenamiento estándar.

### <a name="serverless-compute-costs"></a>Costos de proceso sin servidor

Para obtener una descripción de cómo se define la capacidad de proceso y los costos se calculan para la capa de proceso sin servidor, consulte [SQL Database sin servidor (versión preliminar)](sql-database-serverless.md).

## <a name="storage-costs"></a>Costos de almacenamiento

Diferentes tipos de almacenamiento se facturan de forma diferente. Para el almacenamiento de datos, se le cobrará por el almacenamiento aprovisionado según el tamaño máximo de base de datos o grupo que seleccione. El costo no cambia a menos que se reduzca o aumente ese máximo. El almacenamiento de copia de seguridad se asocia a las copias de seguridad automatizadas de la instancia y su ubicación es dinámica. Al aumentar el período de retención de copia de seguridad aumenta el almacenamiento de copia de seguridad que consume la instancia.

De forma predeterminada, los 7 días de copias de seguridad automatizadas de las bases de datos se copian en una cuenta de almacenamiento de blobs estándar de almacenamiento con redundancia geográfica de acceso de lectura (RA-GRS). Se usa este almacenamiento, copias de seguridad completas semanales, copias de seguridad diferenciales diarias y copias del registro de transacciones, que se copian cada 5 minutos. El tamaño de los registros de transacciones depende de la tasa de cambio de la base de datos. Se proporciona una cantidad de almacenamiento mínimo igual al 100 por ciento del tamaño de base de datos sin costo adicional. El consumo adicional de almacenamiento de copia de seguridad se cobra en GB/mes.

Para más información sobre los precios de almacenamiento, consulte la página de [precios](https://azure.microsoft.com/pricing/details/sql-database/single/).

## <a name="vcore-based-purchasing-model"></a>Modelo de compra basado en núcleo virtual

Un núcleo () representa una CPU lógica y le ofrece la opción de elegir entre varias generaciones de hardware y las características físicas del hardware (por ejemplo, el número de núcleos, la memoria y el tamaño de almacenamiento). El modelo de compra basado en núcleos virtuales le ofrece flexibilidad, control, transparencia del consumo de recursos individuales, y una manera sencilla de trasladar los requisitos de carga de trabajo a la nube localmente. Este modelo permite elegir los recursos de proceso, memoria y almacenamiento en función de sus necesidades de carga de trabajo.

En el modelo de compra basado en núcleos virtuales, puede elegir entre el [de propósito general](sql-database-high-availability.md#basic-standard-and-general-purpose-service-tier-availability) y [crítico para la empresa](sql-database-high-availability.md#premium-and-business-critical-service-tier-availability) niveles para de servicio [bases de datos únicas](sql-database-single-database-scale.md), [ los grupos elásticos](sql-database-elastic-pool.md), y [las instancias administradas](sql-database-managed-instance.md). Para las bases de datos únicos, también puede elegir el [nivel de servicio de hiperescala](sql-database-service-tier-hyperscale.md).

El modelo de compra basado en núcleos virtuales le permite elegir recursos de proceso y almacenamiento, coincide con el rendimiento de forma local y optimizar el precio por separado. En el modelo de compra basado en núcleos virtuales, se paga por:

- (El nivel de servicio + el número de núcleos virtuales y la cantidad de memoria y la generación de hardware) de recursos de proceso.
- El tipo y cantidad de almacenamiento de datos y de registro.
- Almacenamiento de copia de seguridad (RA-GRS).

> [!IMPORTANT]
> Recursos de proceso, E/S y el almacenamiento de datos y de registro se cobran por base de datos o un grupo elástico. Almacenamiento de copia de seguridad se cobra por cada base de datos. Para más información sobre los cargos de instancia administrada, consulte [Instancias administradas](sql-database-managed-instance.md).
> **Limitaciones regionales:** Para conocer la lista actual de regiones admitidas, consulte [Productos disponibles por región](https://azure.microsoft.com/global-infrastructure/services/?products=sql-database&regions=all). Para crear una instancia administrada en una región que no se admite actualmente [enviar una solicitud de soporte técnico a través del portal de Azure](sql-database-managed-instance-resource-limits.md#obtaining-a-larger-quota-for-sql-managed-instance).

Si su base de datos única o un grupo elástico consume más de 300 Dtu, convertir en el modelo de compra basado en núcleos virtuales puede reducir los costos. Puede convertir mediante el uso de la API de su elección o mediante el portal de Azure, sin tiempo de inactividad. Sin embargo, la conversión no es obligatorio y no se realiza automáticamente. Si el modelo de compra basado en DTU satisface sus requisitos de rendimiento y empresariales, debe seguir usándolo.

Para convertir el modelo de compra basado en DTU en el modelo de compra basado en núcleos virtuales, seleccione el tamaño de proceso mediante el uso de las siguientes reglas generales:

- Cada 100 Dtu en el nivel estándar requieren al menos 1 núcleo virtual en el nivel de servicio de uso general.
- Cada 125 Dtu en el nivel premium requieren como mínimo 1 núcleo en el nivel de servicio crítico del negocio.

## <a name="dtu-based-purchasing-model"></a>Modelo de compra basado en DTU

Una unidad de transacción de base de datos (DTU) representa una medida combinada de CPU, memoria, lecturas y escrituras. El modelo de compra basado en DTU ofrece un conjunto de agrupaciones preconfiguradas de recursos de proceso y almacenamiento incluido para impulsar diferentes niveles de rendimiento de la aplicación. Si prefiere la simplicidad de una agrupación preconfigurada y pagos fijos cada mes, el modelo basado en DTU podría ser más adecuado para sus necesidades.

En el modelo de compra basado en DTU, puede elegir entre básico, estándar y niveles de servicio premium para ambos [bases de datos únicas](sql-database-single-database-scale.md) y [grupos elásticos](sql-database-elastic-pool.md). No está disponible para el modelo de compra basado en DTU [las instancias administradas](sql-database-managed-instance.md).

### <a name="database-transaction-units-dtus"></a>Unidades de transacción de base de datos (DTU)

Para una sola base de datos en un determinado proceso tamaño dentro de un [nivel de servicio](sql-database-single-database-scale.md), Microsoft garantiza un cierto nivel de recursos para esa base de datos (independientemente de cualquier otra base de datos en la nube de Azure). Esta garantía proporciona un nivel de rendimiento predecible. La cantidad de recursos asignados a una base de datos se calcula como un número de Dtu y es una medida combinada de proceso, almacenamiento y recursos de E/S.

La relación entre estos recursos se determinó originalmente mediante un [carga de trabajo de prueba comparativa de procesamiento de transacciones en línea (OLTP)](sql-database-benchmark-overview.md) diseñada para ser típica de cargas de trabajo OLTP reales. Cuando la carga de trabajo supera la cantidad de cualquiera de estos recursos, se limita la capacidad de proceso, lo que resulta en el rendimiento más lento y los tiempos de espera.

Los recursos utilizados por la carga de trabajo no afecten a los recursos disponibles para otras bases de datos SQL en la nube de Azure. Del mismo modo, los recursos utilizados por otras cargas de trabajo no afecten a los recursos disponibles para la base de datos SQL.

![rectángulo de selección](./media/sql-database-what-is-a-dtu/bounding-box.png)

Las Dtu son especialmente útiles para comprender los recursos relativos que se asignan a las bases de datos SQL de Azure en los niveles de servicio y los tamaños diferentes de proceso. Por ejemplo:

- Duplicar el número de Dtu aumentando el tamaño de proceso de una base de datos equivale a duplicar el conjunto de recursos disponibles para esa base de datos.
- Una base de datos de premium servicio nivel P11 con 1750 Dtu proporciona DTU 350 veces mayor potencia de computación que una base de datos de nivel de servicio básico con 5 Dtu.  

Para obtener información más detallada sobre el consumo de recursos (DTU) de la carga de trabajo, use [query performance Insight](sql-database-query-performance.md) para:

- Identificar las consultas principales por CPU, duración y recuento de ejecuciones que se puede ajustar para mejorar el rendimiento. Por ejemplo, una consulta intensivo de E/s puede beneficiarse [técnicas de optimización en memoria](sql-database-in-memory.md) para hacer un mejor uso de la memoria disponible en un determinado nivel de servicio y el tamaño de proceso.
- Explorar en profundidad los detalles de una consulta para ver su texto y su historial de uso de recursos.
- Obtener acceso a recomendaciones de optimización del rendimiento que muestran las acciones realizadas por [SQL Database Advisor](sql-database-advisor.md).

### <a name="elastic-database-transaction-units-edtus"></a>Unidades de transacción de bases de datos elásticas (Edtu)

Para las bases de datos SQL que están siempre disponibles, sino que proporcionan un conjunto dedicado de recursos (Dtu) que no siempre pueden necesitar, puede colocar estas bases de datos en un [grupo elástico](sql-database-elastic-pool.md). Las bases de datos en un grupo elástico se encuentran en un único servidor de Azure SQL Database y comparten un grupo de recursos.

Los recursos compartidos de un grupo elástico se miden por unidades de transacción de bases de datos elásticas (Edtu). Los grupos elásticos proporcionan una solución sencilla y rentable para administrar los objetivos de rendimiento de varias bases de datos que tienen patrones de uso muy diferentes e imprevisibles. Un grupo elástico garantiza que todos los recursos no pueden usarse por una base de datos en el grupo, asegurándose de que cada base de datos en el grupo siempre tiene una cantidad mínima de recursos necesarios disponibles.

A un grupo se le asigna un número fijo de eDTU, por un precio fijo. En el grupo elástico, bases de datos individuales pueden escalar automáticamente dentro de los límites configurados. Una base de datos bajo una carga mayor consumirá más Edtu para satisfacer la demanda. Las bases de datos con cargas más pequeñas consumirá menos Edtu. Las bases de datos sin cargas no consumirán ningún eDTU. Dado que los recursos se aprovisionan para todo el grupo, en lugar de por base de datos, los grupos elásticos simplifican las tareas de administración y proporcionan un presupuesto predecible para el grupo.

Puede agregar Edtu adicionales a un grupo existente sin tiempo de inactividad de la base de datos y sin tener impacto en las bases de datos en el grupo. De forma similar, si ya no necesita las Edtu adicionales, quitarlos de un grupo existente en cualquier momento. También puede agregar las bases de datos o quitar bases de datos de un grupo en cualquier momento. Para reservar Edtu para otras bases de datos, limite el número de Edtu que puede usar una base de datos bajo una carga elevada. Si una base de datos sistemáticamente desaprovecha recursos, muévalo fuera del grupo y configurarla como una sola base de datos con una cantidad predecible de los recursos necesarios.

### <a name="determine-the-number-of-dtus-needed-by-a-workload"></a>Determinar el número de DTU necesarias para la carga de trabajo

Si desea migrar una existente en el entorno local o la carga de trabajo de máquina virtual de SQL Server a Azure SQL Database, use el [Calculadora de DTU](https://dtucalculator.azurewebsites.net/) para aproximar el número de Dtu necesarias. Para una carga de trabajo de Azure SQL Database existente, use [query performance Insight](sql-database-query-performance.md) para comprender el consumo de recursos de la base de datos (Dtu) y obtener información más detallada para optimizar la carga de trabajo. El [sys.dm_db_ resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) vista de administración dinámica (DMV) le permite ver el consumo de recursos de la última hora. El [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) vista de catálogo muestra el consumo de recursos para los últimos 14 días, pero con una fidelidad inferior Media de cinco minutos.

### <a name="workloads-that-benefit-from-an-elastic-pool-of-resources"></a>Cargas de trabajo que se benefician de un grupo elástico de recursos

Los grupos son adecuados para las bases de datos con un promedio de utilización de recursos baja y picos de utilización relativamente poco frecuentes. Base de datos SQL se evalúa como el uso de recursos históricos de las bases de datos en un servidor de base de datos SQL existente y recomienda la configuración de grupo adecuado en el portal de Azure automáticamente. Para obtener más información, consulte [cuándo se debe considerar un grupo elástico de SQL Database?](sql-database-elastic-pool.md).

## <a name="frequently-asked-questions-faqs"></a>Preguntas más frecuentes (P+F)

### <a name="do-i-need-to-take-my-application-offline-to-convert-from-a-dtu-based-service-tier-to-a-vcore-based-service-tier"></a>¿Es necesario tomar mi aplicación sin conexión para convertir de un nivel de servicio basado en DTU en un nivel de servicio basado en núcleos virtuales?

No. No es necesario desconectar la aplicación. Los nuevos niveles de servicio ofrecen un método de conversión en línea simple que es similar al proceso existente de actualizar las bases de datos desde el estándar para el nivel de servicio premium y al revés. Puede iniciar esta conversión mediante el uso de Azure portal, PowerShell, la CLI de Azure, Transact-SQL o la API de REST. Consulte [Administración de bases de datos únicas](sql-database-single-database-scale.md) y [Administración de grupos elásticos](sql-database-elastic-pool.md).

### <a name="can-i-convert-a-database-from-a-service-tier-in-the-vcore-based-purchasing-model-to-a-service-tier-in-the-dtu-based-purchasing-model"></a>¿Se puede convertir una base de datos de un nivel de servicio en el modelo de compra basado en núcleos virtuales a un nivel de servicio en el modelo de compra basado en DTU?

Sí, puede convertir fácilmente la base de datos en cualquier objetivo de rendimiento mediante el uso de Azure portal, PowerShell, la CLI de Azure, Transact-SQL o la API de REST. Consulte [Administración de bases de datos únicas](sql-database-single-database-scale.md) y [Administración de grupos elásticos](sql-database-elastic-pool.md).

## <a name="next-steps"></a>Pasos siguientes

- Para obtener más información sobre el modelo de compra basado en núcleos virtuales, consulte [modelo de compra basado en núcleos virtuales](sql-database-service-tiers-vcore.md).
- Para obtener más información sobre el modelo de compra basado en DTU, consulte [modelo de compra basado en DTU](sql-database-service-tiers-dtu.md).
