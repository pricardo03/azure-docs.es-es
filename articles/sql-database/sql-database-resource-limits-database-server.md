---
title: Límites de recursos del servidor de Azure SQL Database | Microsoft Docs
description: En este artículo se proporciona información general acerca de los límites de recursos del servidor de Azure SQL Database para bases de datos únicas y grupos elásticos. También se proporciona información acerca de lo qué ocurre cuando se alcanzan o superan dichos límites.
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: CarlRabeler
ms.author: carlrab
ms.reviewer: sashan,moslake
manager: craigg
ms.date: 02/07/2019
ms.openlocfilehash: 670ca1b8ba16122d4e969a41f8679e1a6d1b27c6
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/11/2019
ms.locfileid: "55990111"
---
# <a name="sql-database-resource-limits-for-azure-sql-database-server"></a>Límites de recursos de SQL Database para un servidor de Azure SQL Database

En este artículo se proporciona información general acerca de los límites de recursos de SQL Database para un servidor de SQL Database que administra bases de datos únicas y grupos elásticos. También se proporciona información acerca de lo qué ocurre cuando se alcanzan o superan dichos límites.

> [!NOTE]
> Para conocer los límites de instancias administradas, consulte [Límites de recursos de SQL Database para instancias administradas](sql-database-managed-instance-resource-limits.md).

## <a name="maximum-resource-limits"></a>Límites de recursos máximos

| Recurso | Límite |
| :--- | :--- |
| Bases de datos por servidor | 5000 |
| Número predeterminado de servidores por suscripción en cualquier región | 20 |
| Número máximo de servidores por suscripción en cualquier región | 200 |  
| Cuota de DTU o eDTU por servidor | 54 000 |  
| Cuota de núcleo virtual por servidor/instancia | 540 |
| Número máximo de grupos por servidor | Limitado por el número de DTU o núcleos virtuales. Por ejemplo, si cada grupo tiene 1000 DTU, un servidor puede admitir 54 grupos.|
||||

> [!NOTE]
> Para obtener más cuota de DTU, eDTU, núcleos virtuales o más servidores que la cantidad predeterminada, se puede enviar una nueva solicitud de soporte técnico en Azure Portal para la suscripción con el tipo de problema "Cuota". La cuota de DTU o eDTU y el límite de base de datos por servidor restringe el número de grupos elásticos por servidor.
> [!IMPORTANT]
> A medida que el número de bases de datos se aproxima al límite por servidor de SQL Database, puede ocurrir lo siguiente:
> - Aumento de la latencia de las consultas en ejecución en la base de datos maestra.  Incluye vistas de las estadísticas del uso de los recursos como sys.resource_stats.
> - Aumento de la latencia en las operaciones de administración y presentación de las perspectivas del portal que implican enumerar las bases de datos del servidor.

## <a name="what-happens-when-database-resource-limits-are-reached"></a>¿Qué ocurre cuando se alcanzan los límites de recursos de base de datos?

### <a name="compute-dtus-and-edtus--vcores"></a>Proceso (DTU y eDTU o núcleos virtuales)

Cuando el uso del proceso de base de datos (medido por las DTU y eDTU o núcleos virtuales) es elevado, la latencia de las consultas aumenta e incluso puede agotar el tiempo de espera. En estas condiciones, el servicio puede poner en cola a las consultas y se proporcionan recursos para la ejecución a medida que el recurso esté disponible.
Al encontrar un uso de proceso elevado, las opciones de mitigación incluyen:

- Aumentar el tamaño de proceso de la base de datos o del grupo elástico para proporcionar a la base de datos más recursos de proceso. Consulte [Scale single database resources](sql-database-single-database-scale.md) (Escala de recursos de bases de datos únicas) y [Scale elastic pool resources](sql-database-elastic-pool-scale.md) (Escala de recursos de grupos elásticos).
- Optimizar las consultas para reducir el uso de recursos de cada consulta. Para más información, consulte [Optimización y sugerencias de consultas](sql-database-performance-guidance.md#query-tuning-and-hinting).

### <a name="storage"></a>Almacenamiento

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

## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre los límites generales de Azure, consulte [Límites, cuotas y restricciones de suscripción y servicios de Microsoft Azure](../azure-subscription-service-limits.md).
- Para más información sobre las DTU y eDTU, consulte [DTU y eDTU](sql-database-purchase-models.md#dtu-based-purchasing-model).
- Para más información sobre los límites de tamaño de tempdb, consulte [Base de datos tempdb en SQL Database](https://docs.microsoft.com/sql/relational-databases/databases/tempdb-database#tempdb-database-in-sql-database).
