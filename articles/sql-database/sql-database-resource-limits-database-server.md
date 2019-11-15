---
title: Límites de recursos de servidor
description: En este artículo se proporciona información general acerca de los límites de recursos del servidor de Azure SQL Database para bases de datos únicas y grupos elásticos. También se proporciona información acerca de lo qué ocurre cuando se alcanzan o superan dichos límites.
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: sashan,moslake,josack
ms.date: 04/18/2019
ms.openlocfilehash: 907fc89c0d9af01865037f650c407edd97e96645
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2019
ms.locfileid: "73821150"
---
# <a name="sql-database-resource-limits-for-azure-sql-database-server"></a>Límites de recursos de SQL Database para un servidor de Azure SQL Database

En este artículo se proporciona información general acerca de los límites de recursos de SQL Database para un servidor de SQL Database que administra bases de datos únicas y grupos elásticos. También se proporciona información acerca de lo qué ocurre cuando se alcanzan o superan dichos límites.

> [!NOTE]
> Para conocer los límites de instancias administradas, consulte [Límites de recursos de SQL Database para instancias administradas](sql-database-managed-instance-resource-limits.md).

## <a name="maximum-resource-limits"></a>Límites de recursos máximos

| Resource | Límite |
| :--- | :--- |
| Bases de datos por servidor | 5000 |
| Número predeterminado de servidores por suscripción en cualquier región | 20 |
| Número máximo de servidores por suscripción en cualquier región | 200 |  
| Cuota de DTU o eDTU por servidor | 54 000 |  
| Cuota de núcleo virtual por servidor/instancia | 540 |
| Número máximo de grupos por servidor | Limitado por el número de DTU o núcleos virtuales. Por ejemplo, si cada grupo tiene 1000 DTU, un servidor puede admitir 54 grupos.|
|||

> [!NOTE]
> Para obtener más cuota de DTU, eDTU, núcleos virtuales o más servidores que la cantidad predeterminada, se puede enviar una nueva solicitud de soporte técnico en Azure Portal para la suscripción con el tipo de problema "Cuota". La cuota de DTU o eDTU y el límite de base de datos por servidor restringe el número de grupos elásticos por servidor.
> [!IMPORTANT]
> A medida que el número de bases de datos se aproxima al límite por servidor de SQL Database, puede ocurrir lo siguiente:
> - Aumento de la latencia de las consultas en ejecución en la base de datos maestra.  Incluye vistas de las estadísticas del uso de los recursos como sys.resource_stats.
> - Aumento de la latencia en las operaciones de administración y presentación de las perspectivas del portal que implican enumerar las bases de datos del servidor.

### <a name="storage-size"></a>Tamaño de almacenamiento
- Respecto a los recursos de bases de datos únicas, consulte [Límites de los recursos basados en DTU](sql-database-dtu-resource-limits-single-databases.md) o [Límites de los recursos basados en núcleos virtuales](sql-database-vcore-resource-limits-single-databases.md) para conocer los límites del tamaño de almacenamiento por plan de tarifa.

## <a name="what-happens-when-database-resource-limits-are-reached"></a>¿Qué ocurre cuando se alcanzan los límites de recursos de base de datos?

### <a name="compute-dtus-and-edtus--vcores"></a>Proceso (DTU y eDTU o núcleos virtuales)

Cuando el uso del proceso de base de datos (medido por las DTU y eDTU o núcleos virtuales) es elevado, la latencia de las consultas aumenta e incluso puede agotar el tiempo de espera. En estas condiciones, el servicio puede poner en cola a las consultas y se proporcionan recursos para la ejecución a medida que el recurso esté disponible.
Al encontrar un uso de proceso elevado, las opciones de mitigación incluyen:

- Aumentar el tamaño de proceso de la base de datos o del grupo elástico para proporcionar a la base de datos más recursos de proceso. Consulte [Scale single database resources](sql-database-single-database-scale.md) (Escala de recursos de bases de datos únicas) y [Scale elastic pool resources](sql-database-elastic-pool-scale.md) (Escala de recursos de grupos elásticos).
- Optimizar las consultas para reducir el uso de recursos de cada consulta. Para más información, consulte [Optimización y sugerencias de consultas](sql-database-performance-guidance.md#query-tuning-and-hinting).

### <a name="storage"></a>Storage

Cuando el espacio de la base de datos alcanza el límite de tamaño máximo, las inserciones y actualizaciones de la base de datos que aumentan el tamaño de los datos producen un error y los clientes reciben un [mensaje de error](sql-database-develop-error-messages.md). Las selecciones y eliminaciones de la base de datos continúan.

Al encontrar un uso elevado de espacio, las opciones de mitigación incluyen:

- Aumentar el tamaño máximo de la base de datos o el grupo elástico, o agregar más almacenamiento. Consulte [Scale single database resources](sql-database-single-database-scale.md) (Escala de recursos de bases de datos únicas) y [Scale elastic pool resources](sql-database-elastic-pool-scale.md) (Escala de recursos de grupos elásticos).
- Si la base de datos está en un grupo elástico, se puede mover fuera del grupo para que su espacio de almacenamiento no se comparta con otras bases de datos.
- Reduzca una base de datos para reclamar el espacio no utilizado. Para obtener más información, consulte [Manage file space in Azure SQL Database](sql-database-file-space-management.md) (Administración de espacio de archivos en Azure SQL Database).

### <a name="sessions-and-workers-requests"></a>Sesiones y trabajos (solicitudes)

El número máximo de sesiones y de trabajos se determina por el nivel de servicio y el tamaño de proceso (DTU y eDTU). Las nuevas solicitudes se rechazan cuando se alcanzan los límites de sesión o de trabajo y los clientes reciben un mensaje de error. Aunque el número de conexiones disponibles se pueden controlar por la aplicación, el número de trabajos simultáneos suele ser más difícil de calcular y controlar. Esto es especialmente cierto durante los períodos de carga máximos cuando se alcanzan los límites de recursos de base de datos y los trabajos se apilan debido a consultas de ejecución más prolongada.

Al encontrar un uso elevado de sesión o de trabajo, las opciones de mitigación incluyen:

- Aumentar el nivel de servicio o el tamaño de proceso de la base de datos o del grupo elástico. Consulte [Scale single database resources](sql-database-single-database-scale.md) (Escala de recursos de bases de datos únicas) y [Scale elastic pool resources](sql-database-elastic-pool-scale.md) (Escala de recursos de grupos elásticos).
- Optimizar las consultas para reducir el uso de recursos de cada consulta si la causa del mayor uso de trabajo es debida a la contención de los recursos de proceso. Para más información, consulte [Optimización y sugerencias de consultas](sql-database-performance-guidance.md#query-tuning-and-hinting).

## <a name="transaction-log-rate-governance"></a>Gobernanza de las tasas de registros de transacciones 
La gobernanza de las tasas de registros de transacciones es un proceso en Azure SQL Database que se usa para limitar las altas tasas de ingesta de cargas de trabajo como la inserción masiva, SELECT INTO y compilaciones de índice. Se realiza un seguimiento de estos límites y se aplican en el nivel de fracciones de segundo a la tasa de generación de registros, lo que limita el rendimiento sin importar cuántas E/S se pueden emitir respecto de los archivos de datos.  Actualmente, las tasas de generación de registros de transacciones se escalan de manera lineal hasta un punto que depende del hardware, con una tasa de registro máxima permitida de 96 MB/s con el modelo de compra de núcleos virtuales. 

> [!NOTE]
> No se controlan ni limitan las E/S físicas reales para los archivos de registro de transacciones. 

Las tasas de registro se establecen de manera que se puedan alcanzar y sostener en diversos escenarios, mientras que el sistema global puede mantener su funcionalidad con un mínimo de impacto en la carga del usuario. La gobernanza de las tasas de registro garantiza que las copias de seguridad del registro de transacciones se mantendrán dentro de los contratos de nivel de servicio de la capacidad de recuperación publicados.  Esta gobernanza también evita un trabajo pendiente excesivo en las réplicas secundarias.

A medida que se generan los registros, se evalúa cada operación para ver si se debe retrasar a fin de mantener una tasa máxima de registro deseada (MB/s por segundo). No se agregan retrasos cuando los registros se vacían en el almacenamiento. En lugar de eso, la gobernanza de tasas de registro se aplica durante la generación de registros misma.

Las tasas reales de generación de registros impuestas en tiempo de ejecución también podrían verse influenciadas por los mecanismos de comentarios, lo que reducirá de manera temporal las tasas de registros permitidas para que el sistema se pueda estabilizar. La administración del espacio de los archivos de registro, para evitar la falta de espacio para registros, y los mecanismos de replicación del grupo de disponibilidad pueden disminuir temporalmente los límites del sistema global. 

El modelado de tráfico del regulador de la tasas de registros se expone a través de los siguientes tipos de espera (expuesto en la DMV [sys.dm_db_wait_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-wait-stats-azure-sql-database)):

| Tipo de espera | Notas |
| :--- | :--- |
| LOG_RATE_GOVERNOR | Limitación de la base de datos |
| POOL_LOG_RATE_GOVERNOR | Limitación del grupo |
| INSTANCE_LOG_RATE_GOVERNOR | Limitación del nivel de instancia |  
| HADR_THROTTLE_LOG_RATE_SEND_RECV_QUEUE_SIZE | Control de comentarios, la replicación física del grupo de disponibilidad en el nivel de servicio Crítico para la empresa/Premium no está al día |  
| HADR_THROTTLE_LOG_RATE_LOG_SIZE | Control de comentarios, limitación de las tasas para evitar quedarse sin espacio para registros |
|||

Cuando encuentre un límite para las tasas de registros que dificulte la alcanzar la escalabilidad deseada, considere estas opciones:
- Escale verticalmente a un nivel mayor con el fin de obtener la tasa de registro máxima de 96 MB/s. 
- Si los datos que se cargan son transitorios, como los datos de ensayo de un proceso de ETL, se pueden cargar en tempdb (que genera un mínimo de registros). 
- En los escenarios de análisis, cárguelos en una tabla cubierta de almacén de columnas en clúster. Esto disminuye la tasa de registros necesario debido a la compresión. Esta técnica sí aumenta la utilización de la CPU y solo se aplica a los conjuntos de datos que se benefician de los índices de almacén de columnas en clúster. 

## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre los límites generales de Azure, consulte [Límites, cuotas y restricciones de suscripción y servicios de Microsoft Azure](../azure-subscription-service-limits.md).
- Para más información sobre las DTU y eDTU, consulte [DTU y eDTU](sql-database-purchase-models.md#dtu-based-purchasing-model).
- Para más información sobre los límites de tamaño de tempdb, consulte [Base de datos tempdb en Azure SQL Database](https://docs.microsoft.com/sql/relational-databases/databases/tempdb-database#tempdb-database-in-sql-database).
