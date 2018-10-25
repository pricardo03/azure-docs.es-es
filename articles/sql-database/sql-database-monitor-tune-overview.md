---
title: 'Optimización de la supervisión y el rendimiento: Azure SQL Database | Microsoft Docs'
description: Sugerencias para la optimización del rendimiento de Azure SQL Database a través de la evaluación y la mejora.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: danimir
ms.author: v-daljep
ms.reviewer: carlrab
manager: craigg
ms.date: 10/16/2018
ms.openlocfilehash: 5ef15b7a757b87c14bf0bd764bdd6ca6e6da64e0
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2018
ms.locfileid: "49379062"
---
# <a name="monitoring-and-performance-tuning"></a>Optimización de la supervisión y el rendimiento

Azure SQL Database se administra automáticamente y es un servicio de datos flexible donde, de manera sencilla, puede supervisar el uso, agregar o quitar recursos (CPU, memoria, E/S), buscar recomendaciones que pueden mejorar el rendimiento de la base de datos o permitir que la base de datos se adapte a la carga de trabajo y optimice el rendimiento automáticamente.

## <a name="the-state-of-an-active-query"></a>Estado de una consulta activa

Para mejorar el rendimiento de Azure SQL Database, debe comprender que cada solicitud de consulta activa de la aplicación está en ejecución o en estado de espera. Al solucionar un problema de rendimiento en Azure SQL Database, tenga presente el siguiente gráfico:

![Estados de carga de trabajo](./media/sql-database-monitor-tune-overview/workload-states.png)

Para una carga de trabajo con problemas de rendimiento, dichos problemas pueden deberse a la contención de la CPU (una condición **relacionada con la ejecución**) o a la existencia de consultas individuales que esperan algo (una condición **relacionada con la espera**).

- **Uso excesivo de la CPU en su instancia de Azure SQL Database**:

  Es posible que observe un uso excesivo de la CPU que causa problemas de rendimiento en las siguientes condiciones:

  - Demasiadas consultas en ejecución
  - Demasiadas consultas en compilación
  - Una o varias consultas en ejecución usan un plan de consulta que no alcanza el nivel óptimo

  Si este es el caso para su carga de trabajo, su objetivo es identificar y optimizar las consultas asociadas, o bien actualizar el tamaño de proceso o el nivel de servicio para aumentar la capacidad de Azure SQL Database a fin de asimilar los requisitos de la CPU. Para obtener más información sobre el escalado de recursos de bases de datos únicas, consulte [Escalar recursos de base de datos única en Azure SQL Database](sql-database-single-database-scale.md) y, para el escalado de recursos de grupos elásticos, consulte [Escalar recursos de grupos elásticos en Azure SQL Database](sql-database-elastic-pool-scale.md).

- **Una consulta individual está esperando algo**

  Las consultas individuales pueden tener problemas de rendimiento debido a una consulta que espera algo. En este escenario, el objetivo es eliminar o reducir el tiempo de espera.

### <a name="determine-if-you-have-a-running-related-performance-issue"></a>Determine si tiene un problema de rendimiento relacionado con la ejecución

Para identificar los problemas de rendimiento relacionados con la ejecución dispone de varios métodos. Los métodos más comunes son:

- Use [Azure Portal](#monitor-databases-using-the-azure-portal) para supervisar el porcentaje de uso de la CPU.
- Use las [vistas de administración dinámicas](sql-database-monitoring-with-dmvs.md) siguientes:

  - [sys.dm_db_resource_stats](sql-database-monitoring-with-dmvs.md#monitor-resource-use) devuelve el consumo de CPU, E/S y memoria para una base de datos de Azure SQL Database. Hay una fila para cada 15 segundos, incluso si no hay ninguna actividad en la base de datos. Los datos históricos se conservan durante una hora.
  - [sys.resource_stats](sql-database-monitoring-with-dmvs.md#monitor-resource-use) devuelve datos de almacenamiento y uso de CPU para una instancia de Azure SQL Database. Los datos se recopilan y agregan en intervalos de cinco minutos.

> [!TIP]
> Como pauta general, si el uso de la CPU se mantiene en un 80 % o por encima de este porcentaje, tiene un problema de rendimiento relacionado con la ejecución.

### <a name="determine-if-you-have-a-waiting-related-performance-issue"></a>Determine si tiene un problema de rendimiento relacionado con la espera

En primer lugar, asegúrese de que no sea un problema de rendimiento relacionado con la ejecución debido a un uso excesivo de la CPU. Si no es así, el siguiente paso es identificar las principales esperas asociadas con la carga de trabajo de su aplicación.  Métodos comunes para mostrar las principales categorías de tipo de espera:

- El [Almacén de consultas](https://docs.microsoft.com/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store) proporciona las estadísticas de espera por consulta con el tiempo. En el Almacén de consultas, los tipos de espera se combinan en categorías de espera. La asignación de categorías de espera a tipos de espera está disponible en [sys.query_store_wait_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-query-store-wait-stats-transact-sql?view=sql-server-2017#wait-categories-mapping-table).
- [sys.dm_db_wait_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-wait-stats-azure-sql-database) devuelve información acerca de todas las esperas encontradas por los subprocesos ejecutados durante la operación. Puede usar esta vista agregada para diagnosticar problemas de rendimiento con Azure SQL Database y también con consultas y lotes específicos.
- [sys.dm_os_waiting_tasks](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-os-waiting-tasks-transact-sql) devuelve información acerca de la cola de espera de las tareas que están esperando en algún recurso.

Como se muestra en el gráfico anterior, las esperas más comunes son:

- Bloqueos
- E/S
- contención relacionada con tempdb
- Esperas de concesión de memoria

En función de lo que vea, cada categoría de espera tiene una ruta de solución de problemas diferente.

## <a name="overview-of-monitoring-database-performance-in-azure-sql-database"></a>Información general de la supervisión de rendimiento de bases de datos con Azure SQL Database

La supervisión del rendimiento de una base de datos SQL en Azure comienza con la supervisión del uso de recursos, en relación con el nivel de rendimiento elegido para la base de datos. La supervisión le ayudará a determinar si la base de datos tiene un exceso de capacidad o si tiene problemas porque se ha alcanzado el número máximo de recursos y, después, a decidir si es el momento de ajustar el tamaño de proceso y el nivel de servicio de la base de datos en el [modelo de compra basado en DTU](sql-database-service-tiers-dtu.md) o el [modelo de compra basado en núcleos virtuales](sql-database-service-tiers-vcore.md). Puede supervisar la base de datos en [Azure Portal](https://portal.azure.com) mediante las siguientes herramientas gráficas o mediante las [vistas de administración dinámicas (DMV)](sql-database-monitoring-with-dmvs.md) de SQL.

Azure SQL Database le permite identificar las oportunidades para mejorar y optimizar el rendimiento de las consultas sin cambiar los recursos mediante la revisión de [recomendaciones para la optimización del rendimiento](sql-database-advisor.md). Los motivos comunes por los que se obtiene poco rendimiento de la base de datos son la falta de índices o la poca optimización de las consultas. Puede aplicar estas recomendaciones de optimización para mejorar el rendimiento de la carga de trabajo.
También puede permitir que Azure SQL Database [optimice el rendimiento de las consultas automáticamente](sql-database-automatic-tuning.md) aplicando todas las recomendaciones identificadas y comprobando que mejoran el rendimiento de la base de datos. Dispone de las siguientes opciones para supervisar el rendimiento de la base de datos y solucionar problemas relacionados con dicho rendimiento:

- En [Azure Portal](https://portal.azure.com), haga clic en **Bases de datos SQL**, seleccione la base de datos y luego use el gráfico de supervisión para buscar recursos que se acerquen a su máximo. El consumo de DTU se muestra de manera predeterminada. Haga clic en **Editar** para cambiar el intervalo de tiempo y los valores que se muestran.
- Use [Información de rendimiento de consultas](sql-database-query-performance.md) para identificar las consultas que consumen la mayoría de los recursos.
- Usar [SQL Database Advisor](sql-database-advisor-portal.md) para ver recomendaciones para crear y quitar índices, parametrizar consultas y solucionar problemas de esquema.
- Use [Intelligent Insights de Azure SQL](sql-database-intelligent-insights.md) para supervisar automáticamente el rendimiento de su base de datos. Una vez que se detecta un problema de rendimiento, se genera un registro de diagnóstico con los detalles y el análisis de la causa principal (RCA) del problema. Cuando es posible, se proporciona una recomendación para la mejora del rendimiento.
- [Habilitar la optimización automática](sql-database-automatic-tuning-enable.md) y dejar que SQL Azure Database corrija problemas de rendimiento automáticamente.
- Puede usar [vistas de administración dinámica (DMV)](sql-database-monitoring-with-dmvs.md), eventos extendidos (`XEvents`)(sql-database/sql-database-xevent-db-diff-from-svr.md) y el [almacén de consultas](https://docs.microsoft.com/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store) para obtener parámetros de rendimiento en tiempo real. Consulte la [guía de rendimiento](sql-database-performance-guidance.md) para buscar las técnicas que puede usar para mejorar el rendimiento de Azure SQL Database si identifica algún problema al usar estos informes o vistas.

## <a name="monitor-databases-using-the-azure-portal"></a>Supervisión de bases de datos mediante el Portal de Azure

En [Azure Portal](https://portal.azure.com/), puede supervisar la utilización de una base de datos única; para ello, debe seleccionar la base de datos y hacer clic en el gráfico **Supervisión**. Al hacer esto, se abrirá la ventana **Métrica** que se puede cambiar haciendo clic en el botón **Editar gráfico**. Agregue las siguientes métricas:

- Porcentaje de CPU
- Porcentaje de DTU
- Porcentaje de E/S de datos
- Porcentaje de tamaño de base de datos

Una vez agregadas estas métricas, podrá verlas en el gráfico **Supervisión** con más información en la ventana **Métrica**. Las cuatro métricas muestran el porcentaje de uso medio, en relación con la **DTU** de la base de datos. Consulte los artículos sobre el [modelo de compra basado en DTU](sql-database-service-tiers-dtu.md) y el [modelo de compra basado en núcleos virtuales](sql-database-service-tiers-vcore.md) para obtener más información sobre los niveles de servicio.  

![Supervisión del nivel de servicio del rendimiento de la base de datos.](./media/sql-database-single-database-monitoring/sqldb_service_tier_monitoring.png)

También se pueden configurar alertas en las métricas de rendimiento. Haga clic en el botón **Agregar alerta** de la ventana **Métrica**. Siga el asistente para configurar la alerta. Tiene la opción de que se genere una alerta si la métrica supera un umbral determinado, o si no llega a él.

Por ejemplo, si espera que crezca la carga de trabajo de una base de datos, puede configurar una alerta por correo electrónico cada vez que la base de datos alcance el 80% en cualquiera de las métricas de rendimiento. Esto se puede usar como advertencia prematura para saber cuándo puede tener que cambiar al tamaño de proceso inmediatamente superior.

Las métricas de rendimiento también pueden ayudarle a determinar si puede cambiar a un tamaño de proceso inferior. Suponga que usa una base de datos Estándar S2 y todas las métricas de rendimiento muestran que, de media, la base de datos no usa más del 10% en ningún momento. Es probable que la base de datos funcione bien en Estándar S1. Sin embargo, tenga en cuenta las cargas de trabajo que tienen picos o fluctúan antes de tomar la decisión de cambiar a un tamaño de proceso inferior.

## <a name="improving-database-performance-with-more-resources"></a>Mejora del rendimiento de la base de datos con más recursos

Por último, si no hay elementos para procesar que puedan mejorar el rendimiento de la base de datos, puede cambiar la cantidad de recursos disponibles en Azure SQL Database. Puede asignar más recursos cambiando el [nivel de servicio de DTU](sql-database-service-tiers-dtu.md) de una única base de datos o aumentar las eDTU de un grupo elástico en cualquier momento. O bien, si usa el [modelo de compra basado en núcleos virtuales](sql-database-service-tiers-vcore.md), puede cambiar el nivel de servicio o aumentar los recursos asignados a la base de datos.

1. Para bases de datos individuales, puede [cambiar los niveles de servicio](sql-database-service-tiers-dtu.md) o los [recursos de proceso](sql-database-service-tiers-vcore.md) a petición para mejorar el rendimiento de la base de datos.
2. Para varias bases de datos, considere el uso de [grupos elásticos](sql-database-elastic-pool-guidance.md) para escalar recursos automáticamente.

## <a name="tune-and-refactor-application-or-database-code"></a>Optimización y refactorización del código de la aplicación o base de datos

Puede cambiar el código de la aplicación para usar la base de datos, cambiar índices, forzar planes o usar sugerencias de forma más óptima para adaptar manualmente la base de datos a la carga de trabajo. En el artículo sobre el [tema de la guía de rendimiento](sql-database-performance-guidance.md) puede buscar algunas pautas y sugerencias para la optimización manual y volver a escribir el código.

## <a name="next-steps"></a>Pasos siguientes

- Para habilitar la característica de ajuste automático en Azure SQL Database y permitir que administre totalmente la carga de trabajo, consulte [Habilitación del ajuste automático](sql-database-automatic-tuning-enable.md).
- Para utilizar la optimización manual, puede revisar [SQL Database Advisor mediante Azure Portal](sql-database-advisor-portal.md) y aplicar manualmente las recomendaciones que mejoran el rendimiento de las consultas.
- Cambiar los recursos que están disponibles en la base de datos cambiando los [niveles de servicio de Azure SQL Database](sql-database-performance-guidance.md).
