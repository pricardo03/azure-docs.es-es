---
title: 'Optimización de la supervisión y el rendimiento: Azure SQL Database | Microsoft Docs'
description: Sugerencias para la optimización del rendimiento de Azure SQL Database a través de la evaluación y la mejora.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: jrasnick, carlrab
ms.date: 01/25/2019
ms.openlocfilehash: 83ff39e9f3b7f95256466c74011e55ebdc22a7a9
ms.sourcegitcommit: d70c74e11fa95f70077620b4613bb35d9bf78484
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/11/2019
ms.locfileid: "70910526"
---
# <a name="monitoring-and-performance-tuning"></a>Optimización de la supervisión y el rendimiento

Azure SQL Database proporciona herramientas y métodos que puede usar para supervisar el uso fácilmente, agregar o quitar recursos (como CPU, memoria o E/S), solucionar posibles problemas y hacer recomendaciones para mejorar el rendimiento de una base de datos. Las características de Azure SQL Database pueden corregir automáticamente los problemas de las bases de datos. 

El ajuste automático permite a una base de datos adaptarse a la carga de trabajo y optimizar automáticamente el rendimiento. Sin embargo, es posible que para algunas incidencias personalizadas se necesite la solución de problemas. En este artículo se describen algunos procedimientos recomendados y herramientas que puede usar para solucionar los problemas de rendimiento.

Para asegurarse de que una base de datos se ejecuta sin problemas, debe hacer lo siguiente:
- [Supervisar el rendimiento de la base de datos](#monitor-database-performance) para asegurarse de que los recursos que tiene asignados pueden controlar la carga de trabajo. Si la base de datos está alcanzando los límites de recursos, puede hacer lo siguiente:
   - Identificar y optimizar las consultas que consumen más recursos
   - Agregar más recursos mediante la [actualización del nivel de servicio](https://docs.microsoft.com/azure/sql-database/sql-database-scale-resources)
- [Solucionar los problemas de rendimiento](#troubleshoot-performance-problems) para identificar por qué se produjo un posible problema y determinar la causa principal. Después de identificar la causa principal, siga los pasos para solucionar el problema.

## <a name="monitor-database-performance"></a>Supervisión del rendimiento de una base de datos

Para supervisar el rendimiento de una base de datos SQL en Azure, supervise inicialmente los recursos usados, en relación con el nivel elegido de rendimiento de base de datos. Supervise los siguientes recursos:
 - **Uso de CPU**: compruebe si la base de datos alcanza el 100 % del uso de CPU durante un período de tiempo prolongado. Un uso alto de la CPU puede indicar la necesidad de identificar y ajustar las consultas que usan la mayor parte de la capacidad de proceso. También podría indicar que la base de datos o la instancia se deben actualizar a un nivel de servicio superior. 
 - **Estadísticas de espera**: use [sys.dm_os_wait_stats (Transact-SQL)](/sql/relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql) para determinar cuánto van a esperar las consultas. Las consultas pueden estar esperando recursos, en esperas de colas o esperas externas. 
 - **Uso de E/S**: compruebe si la base de datos está llegando a los límites de E/S del almacenamiento subyacente.
 - **Uso de memoria**: la cantidad de memoria disponible para la base de datos o la instancia es proporcional al número de núcleos virtuales. Asegúrese de que la memoria es suficiente para la carga de trabajo. La duración prevista de la página es uno de los parámetros que puede indicar la rapidez con la que se quitan las páginas de la memoria.

El servicio Azure SQL Database incluye herramientas y recursos para ayudarlo a solucionar los posibles problemas de rendimiento. Puede identificar oportunidades para mejorar y optimizar el rendimiento de las consultas sin cambiar los recursos con las [recomendaciones para la optimización del rendimiento](sql-database-advisor.md). 

Los motivos comunes por los que se obtiene poco rendimiento de la base de datos son la falta de índices o la poca optimización de las consultas. Puede aplicar recomendaciones de optimización para mejorar el rendimiento de la carga de trabajo. También puede permitir que Azure SQL Database [optimice automáticamente el rendimiento de las consultas](sql-database-automatic-tuning.md) mediante la aplicación de todas las recomendaciones identificadas. Luego, compruebe que las recomendaciones han mejorado el rendimiento de la base de datos.

> [!NOTE]
> La indexación solo está disponible en bases de datos únicas y grupos elásticos. No está disponible en instancias administradas.

Elija una de las siguientes opciones para supervisar y solucionar problemas de rendimiento de la base de datos:

- En [Azure Portal](https://portal.azure.com), seleccione **Bases de datos SQL** y elija la base de datos. En el gráfico **Supervisión**, busque los recursos que se aproximan a su utilización máxima. El consumo de DTU se muestra de manera predeterminada. Seleccione **Editar** para cambiar el intervalo de tiempo y los valores mostrados.
- Herramientas como SQL Server Management Studio proporcionan muchos informes útiles, como el [panel de rendimiento](https://docs.microsoft.com/sql/relational-databases/performance/performance-dashboard). Use estos informes para supervisar el uso de los recursos e identificar las consultas que consumen más. Puede usar el [Almacén de consultas](https://docs.microsoft.com/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store#Regressed) para identificar las consultas cuyo rendimiento se ha revertido.
- En [Azure Portal](https://portal.azure.com), use [Información de rendimiento de consultas](sql-database-query-performance.md) para identificar las consultas que usan más recursos. Esta característica solo está disponible en bases de datos únicas y grupos elásticos.
- Use [SQL Database Advisor](sql-database-advisor-portal.md) para ver recomendaciones que le ayuden a crear y quitar índices, parametrizar consultas y corregir problemas de esquemas. Esta característica solo está disponible en bases de datos únicas y grupos elásticos.
- Use [Azure SQL Intelligent Insights](sql-database-intelligent-insights.md) para supervisar automáticamente el rendimiento de la base de datos. Cuando se detecta un problema de rendimiento, se genera un registro de diagnóstico. En él se proporcionan detalles y un análisis de la causa principal (RCA) del problema. Cuando es posible, se proporciona una recomendación para la mejora del rendimiento.
- [Habilite el ajuste automático](sql-database-automatic-tuning-enable.md) para permitir que Azure SQL Database corrija automáticamente los problemas de rendimiento.
- Use las [vistas de administración dinámica (DMV)](sql-database-monitoring-with-dmvs.md), los [eventos extendidos](sql-database-xevent-db-diff-from-svr.md) y el [Almacén de consultas](https://docs.microsoft.com/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store) para obtener soluciones más detalladas de los problemas de rendimiento.

> [!TIP]
> Después de identificar un problema de rendimiento, consulte nuestra [guía de rendimiento](sql-database-performance-guidance.md) para encontrar técnicas que mejoren el rendimiento de Azure SQL Database.

## <a name="troubleshoot-performance-problems"></a>Solución de problemas de rendimiento

Para diagnosticar y resolver problemas de rendimiento, debe averiguar primero el estado de cada consulta activa y las condiciones que causan esos problemas de rendimiento, que son pertinentes para cada estado de la carga de trabajo. Para mejorar el rendimiento de Azure SQL Database, debe comprender que cada solicitud de consulta activa de la aplicación está en estado de ejecución o de espera. Cuando solucione un problema de rendimiento en Azure SQL Database, tenga en cuenta el siguiente diagrama.

![Estados de carga de trabajo](./media/sql-database-monitor-tune-overview/workload-states.png)

Los problemas de rendimiento en una carga de trabajo pueden deberse a la contención de la CPU (una condición *relacionada con la ejecución*) o a consultas individuales que están esperando algo (una condición *relacionada con la espera*).

Los problemas relacionados con la ejecución pueden tener estas causas:
- **Problemas de compilación**: SQL Query Optimizer puede producir un plan poco óptimo debido a estadísticas obsoletas o a la estimación incorrecta del número de filas que se procesarán o de la memoria necesaria. Si se sabe que la consulta se ha ejecutado con más rapidez en el pasado o en otra instancia (ya sea una instancia administrada o una instancia de SQL Server), compare los planes de ejecución reales para ver si son diferentes. Intente aplicar sugerencias de consulta o estadísticas o índices de recompilaciones para obtener un plan mejor. Habilite la corrección automática de los planes en Azure SQL Database para mitigar estos problemas de forma automática.
- **Problemas de ejecución**: si el plan de consulta es óptimo, probablemente alcanza los límites de recursos de la base de datos, como el rendimiento de escritura de registros. O bien, podría estar usando índices fragmentados que se deben volver a generar. Los problemas de ejecución también pueden producirse cuando un gran número de consultas simultáneas necesitan los mismos recursos. Los *problemas relacionados con la espera* suelen tener que ver con problemas de ejecución, ya que es probable que las consultas que no se ejecutan de forma eficaz estén esperando recursos.

Los problemas relacionados con la espera pueden tener estas causas:
- **Bloqueo**: es posible que una consulta mantenga el bloqueo en objetos de la base de datos mientras otras intentan acceder a los mismos objetos. Puede identificar las consultas de bloqueo mediante vistas DMV o herramientas de supervisión.
- **Problemas de E/S**: es posible que las consultas estén esperando a que se escriban las páginas en los archivos de datos o registro. En este caso, consulte las estadísticas de espera `INSTANCE_LOG_RATE_GOVERNOR`, `WRITE_LOG` o `PAGEIOLATCH_*` en la vista DMV.
- **Problemas de tempdb**: si la carga de trabajo usa tablas temporales o se producen pérdidas de datos de TempDb en los planes, es posible que las consultas tengan un problema con el rendimiento de TempDb. 
- **Problemas relacionados con la memoria**: si la carga de trabajo no tiene suficiente memoria, la duración prevista de la página podría reducirse o las consultas podrían obtener menos memoria de la que necesitan. En algunos casos, la inteligencia integrada en el optimizador de consultas corregirá estos problemas.
 
En las siguientes secciones se describe cómo identificar y solucionar algunos de estos problemas.

## <a name="performance-problems-related-to-running"></a>Problemas de rendimiento relacionados con la ejecución

Como norma general, si el uso de la CPU se sitúa sistemáticamente en un 80 por ciento o por encima, el problema de rendimiento tiene que ver con la ejecución. Un problema relacionado con la ejecución puede deberse a recursos de CPU insuficientes. O podría estar relacionado con una de las siguientes condiciones:

- Demasiadas consultas en ejecución
- Demasiadas consultas en compilación
- Una o varias consultas en ejecución usan un plan de consulta por debajo del nivel óptimo

Si se encuentra con un problema de rendimiento relacionado con la ejecución, el objetivo será identificar el problema preciso mediante uno o varios métodos. Estos métodos son las maneras más comunes de identificar problemas relacionados con la ejecución:

- Use [Azure Portal](sql-database-manage-after-migration.md#monitor-databases-using-the-azure-portal) para supervisar el porcentaje de uso de la CPU.
- Use las siguientes [vistas DMV](sql-database-monitoring-with-dmvs.md):

  - [sys.dm_db_resource_stats](sql-database-monitoring-with-dmvs.md#monitor-resource-use) devuelve el consumo de CPU, E/S y memoria de una instancia de SQL Database. Hay una fila por cada intervalo de 15 segundos, incluso si no hay ninguna actividad en la base de datos. Los datos históricos se conservan durante una hora.
  - [sys.resource_stats](sql-database-monitoring-with-dmvs.md#monitor-resource-use) devuelve datos de almacenamiento y uso de CPU para Azure SQL Database. Los datos se recopilan y agregan en intervalos de cinco minutos.

> [!IMPORTANT]
> Para solucionar los problemas de uso de la CPU de consultas T-SQL que usan las vistas DMV sys.dm_db_resource_stats y sys.resource_stats, consulte [Identificación de problemas de rendimiento de la CPU](sql-database-monitoring-with-dmvs.md#identify-cpu-performance-issues).

### <a name="ParamSniffing"></a> Consultas que tienen problemas de PSP

El problema del plan que distingue entre parámetros (PSP) sucede cuando el optimizador de consultas genera un plan de ejecución de consultas que es óptimo solo para un valor de parámetro específico (o un conjunto de valores) y el plan almacenado en caché no es óptimo para los valores de parámetros que se usan en ejecuciones consecutivas. Los planes que no son óptimos pueden provocar problemas de rendimiento de consultas y reducir el rendimiento general de la carga de trabajo. 

Para más información sobre el procesamiento de consultas y el examen de parámetros, consulte la [Guía de arquitectura de procesamiento de consultas](/sql/relational-databases/query-processing-architecture-guide#ParamSniffing).

Varias soluciones alternativas pueden mitigar los problemas de PSP. Cada solución lleva asociada una serie de ventajas e inconvenientes:

- Use la sugerencia de consulta [RECOMPILE](https://docs.microsoft.com/sql/t-sql/queries/hints-transact-sql-query) en cada ejecución de consulta. Esta solución alternativa tiene en cuenta el tiempo de compilación y el aumento del uso de la CPU para obtener mejor calidad en el plan. La opción `RECOMPILE` no es a menudo posible con cargas de trabajo que requieren un alto rendimiento.
- Use la sugerencia de consulta [OPTION (OPTIMIZE FOR…)](https://docs.microsoft.com/sql/t-sql/queries/hints-transact-sql-query) para invalidar el valor del parámetro real con un valor de parámetro típico que cree un plan lo suficientemente bueno para la mayoría de las posibilidades que ofrece el valor de parámetro. Esta opción requiere tener una buena comprensión de los valores óptimos de los parámetros y de las características del plan asociado.
- Use la sugerencia de consulta [OPTION (OPTIMIZE FOR UNKNOWN)](https://docs.microsoft.com/sql/t-sql/queries/hints-transact-sql-query) para invalidar el valor del parámetro real y así poder usar la media del vector de densidad. Otra forma de hacerlo es capturar los valores de los parámetros entrantes en variables locales y luego usar esas variables locales en los predicados en lugar de utilizar los parámetros en sí. En el caso de esta corrección, la densidad media debe ser *suficientemente buena*.
- Desactive el examen de parámetros por completo mediante la sugerencia de consulta [DISABLE_PARAMETER_SNIFFING](https://docs.microsoft.com/sql/t-sql/queries/hints-transact-sql-query).
- Use la sugerencia de consulta [KEEPFIXEDPLAN](https://docs.microsoft.com/sql/t-sql/queries/hints-transact-sql-query) para evitar recompilaciones en la caché. Esta solución asume que el plan común suficientemente bueno es el que ya está en la caché. También puede deshabilitar las actualizaciones automáticas de las estadísticas para reducir las posibilidades de que se expulse el plan bueno y se compile un nuevo plan malo.
- Para forzar el plan explícitamente, use la sugerencia de consulta [USE PLAN](https://docs.microsoft.com/sql/t-sql/queries/hints-transact-sql-query), vuelva a escribir la consulta y agregue la sugerencia al texto de consulta. O bien, establezca un plan específico mediante el Almacén de consultas o la habilitación del [ajuste automático](sql-database-automatic-tuning.md).
- Reemplace el procedimiento único con un conjunto anidado de procedimientos que se pueden usar según la lógica condicional y los valores de los parámetros asociados.
- Cree alternativas de ejecución de cadenas dinámicas en una definición de procedimiento estático.

Para más información sobre cómo resolver los problemas de PSP, consulte estas entradas de blog:

- [Huelo un parámetro](https://blogs.msdn.microsoft.com/queryoptteam/2006/03/31/i-smell-a-parameter/)
- [Conor vs. dynamic SQL vs. procedures vs. plan quality for parameterized queries](https://blogs.msdn.microsoft.com/conor_cunningham_msft/2009/06/03/conor-vs-dynamic-sql-vs-procedures-vs-plan-quality-for-parameterized-queries/) (Conor frente a SKL dinámico, procedimientos y calidad del plan de las consultas parametrizadas)
- [Técnicas de optimización de consultas de SQL en SQL Server: Examen de parámetros](https://www.sqlshack.com/query-optimization-techniques-in-sql-server-parameter-sniffing/)

### <a name="compile-activity-caused-by-improper-parameterization"></a>Actividad de compilación causada por una parametrización incorrecta

Cuando una consulta tiene literales, el motor de base de datos parametriza automáticamente la instrucción o un usuario la parametriza explícitamente para reducir el número de compilaciones. Un alto número de compilaciones de una consulta que usa el mismo patrón pero diferentes valores literales puede dar lugar a un uso elevado de la CPU. De manera similar, si solo parametriza parcialmente una consulta que sigue teniendo literales, el motor de base de datos no la parametriza más.  

Este es un ejemplo de una consulta parcialmente parametrizada:

```sql
SELECT * 
FROM t1 JOIN t2 ON t1.c1 = t2.c1
WHERE t1.c1 = @p1 AND t2.c2 = '961C3970-0E54-4E8E-82B6-5545BE897F8F'
```

En el ejemplo anterior, `t1.c1` toma `@p1`, pero `t2.c2` continúa tomando GUID como elemento literal. En este caso, si cambia el valor de `c2`, la consulta se trata como una consulta diferente y se produce una nueva compilación. Para reducir las compilaciones en este ejemplo, también parametrizaría el GUID.

La siguiente consulta muestra el número de consultas en función del hash de consulta para determinar si una consulta está correctamente parametrizada:

```sql
SELECT  TOP 10  
  q.query_hash
  , count (distinct p.query_id ) AS number_of_distinct_query_ids
  , min(qt.query_sql_text) AS sampled_query_text
FROM sys.query_store_query_text AS qt
  JOIN sys.query_store_query AS q
     ON qt.query_text_id = q.query_text_id
  JOIN sys.query_store_plan AS p 
     ON q.query_id = p.query_id
  JOIN sys.query_store_runtime_stats AS rs 
     ON rs.plan_id = p.plan_id
  JOIN sys.query_store_runtime_stats_interval AS rsi
     ON rsi.runtime_stats_interval_id = rs.runtime_stats_interval_id
WHERE
  rsi.start_time >= DATEADD(hour, -2, GETUTCDATE())
  AND query_parameterization_type_desc IN ('User', 'None')
GROUP BY q.query_hash
ORDER BY count (distinct p.query_id) DESC
```

### <a name="factors-that-affect-query-plan-changes"></a>Factores que afectan a los cambios del plan de consulta

Una recompilación del plan de ejecución de consultas puede producir un plan de consulta generado que difiera del que inicialmente se almacenó en caché. Hay varias razones por las que un plan original existente se puede recompilar automáticamente:
- Cambios en el esquema al que la consulta hace referencia.
- Cambios en los datos de las tablas a las que la consulta hace referencia. 
- Cambios en las opciones de contexto de consulta.

Es posible que un plan compilado se expulse de la caché por diversos motivos, por ejemplo:

- Se reinicia la instancia.
- La configuración con ámbito de la base de datos ha cambiado.
- Presión de la memoria.
- Solicitudes explícitas para borrar la caché.

Si usa una sugerencia RECOMPILE, el plan no se almacenará en caché.

Una recompilación (o una compilación nueva tras la expulsión de la caché) aún puede provocar que se genere un plan de ejecución de consulta idéntico al original. Cuando el plan cambia del plan anterior u original, es probable que se produzcan estas explicaciones:

- **Diseño físico cambiado**: por ejemplo, los índices recién creados abarcan con mayor efectividad los requisitos de una consulta. Los nuevos índices se pueden usar en una nueva compilación si el optimizador de consultas decide que es más óptimo usar ese nuevo índice que usar la estructura de datos seleccionada originalmente para la primera versión de la ejecución de la consulta.  Cualquier cambio físico en los objetos a los que se hace referencia puede dar lugar a una nueva opción de plan en tiempo de compilación.

- **Diferencias en los recursos del servidor**: cuando un plan de un sistema difiere del plan de otro sistema, la disponibilidad de recursos, como el número de procesadores disponibles, puede influir en el plan que se genera.  Por ejemplo, si un sistema tiene más procesadores, podría elegirse un plan paralelo. 

- **Estadísticas diferentes**: las estadísticas asociadas a los objetos a los que se hace referencia han cambiado o son significativamente diferentes de las estadísticas originales del sistema.  Si las estadísticas cambian y se produce una recompilación, el optimizador de consultas usa las estadísticas a partir de cuando cambiaron. Las frecuencias y las distribuciones de datos de las estadísticas revisadas pueden diferir de las de la compilación original.  Estos cambios se usan para crear estimaciones de cardinalidad. (Las *estimaciones de cardinalidad* son el número de filas que se espera que fluyan a través del árbol lógico de consultas). Los cambios en las estimaciones de cardinalidad pueden hacer que elijamos diferentes operadores físicos y órdenes asociadas de operaciones.  Incluso los pequeños cambios en las estadísticas pueden dar lugar a un plan de ejecución de consultas modificado.

- **Versión del estimador de cardinalidad o nivel de compatibilidad de la base de datos modificados**:  los cambios en el nivel de compatibilidad de la base de datos pueden permitir nuevas estrategias y características que podrían producir un plan de ejecución de consultas diferente.  Más allá del nivel de compatibilidad de la base de datos, la deshabilitación o habilitación de la marca de seguimiento 4199 o el cambio del estado de la configuración con ámbito de la base de datos, QUERY_OPTIMIZER_HOTFIXES también puede influir en las opciones del plan de ejecución de consultas en tiempo de compilación.  Las marcas de seguimiento 9481 (forzar CE heredada) y 2312 (forzar CE predeterminada) también afectan al plan. 

### <a name="resolve-problem-queries-or-provide-more-resources"></a>Resolver consultas de problemas o proporcionar más recursos

Una vez identificado el problema, puede ajustar las consultas del problema o actualizar el tamaño de proceso o el nivel de servicio para aumentar la capacidad de su instancia de SQL Database a fin de asimilar los requisitos de la CPU. 

Para más información, consulte [Escalar recursos de base de datos única en Azure SQL Database](sql-database-single-database-scale.md) y [Escalar recursos de grupos elásticos en Azure SQL Database](sql-database-elastic-pool-scale.md). Para información sobre cómo escalar una instancia administrada, consulte [Límites de recursos de nivel de instancia](sql-database-managed-instance-resource-limits.md#instance-level-resource-limits).

### <a name="performance-problems-caused-by-increased-workload-volume"></a>Problemas de rendimiento causados por un mayor volumen de carga de trabajo

Un aumento en el tráfico de la aplicación y en el volumen de la carga de trabajo puede incrementar el uso de la CPU. Sin embargo, debe tener cuidado para diagnosticar correctamente este problema. Cuando vea un problema de CPU elevada, responda a estas preguntas para determinar si el aumento se debe a cambios en el volumen de la carga de trabajo:

- ¿Son las consultas de la aplicación la causa del problema del uso elevado de la CPU?
- Para las consultas que más consumen CPU que pueda identificar:

   - ¿Había varios planes de ejecución asociados a la misma consulta? Si es así, ¿por qué?
   - En el caso de las consultas con el mismo plan de ejecución, ¿los tiempos de ejecución eran coherentes? ¿Aumentó el número de ejecuciones? En ese caso, es probable que el aumento de la carga de trabajo ocasione problemas de rendimiento.

Para resumir, si el plan de ejecución de consultas no se ejecutó de manera diferente pero el uso de la CPU aumentó junto con el número de ejecuciones, es probable que exista un problema de rendimiento relacionado con el aumento de la carga de trabajo.

No siempre es fácil identificar los cambios en el volumen de la carga de trabajo que está llevando a un problema con la CPU. Tenga en cuenta estos factores: 

- **Cambio en el uso de los recursos**: por ejemplo, considere un escenario donde el uso de la CPU aumentó al 80 % durante un período prolongado de tiempo.  El uso de CPU por sí solo no significa que el volumen de la carga de trabajo cambie. Las regresiones en el plan de ejecución de consultas y los cambios en la distribución de datos también pueden contribuir a un mayor uso de los recursos, incluso si la aplicación ejecuta la misma carga de trabajo.

- **La aparición de una nueva consulta**: una aplicación puede agregar un nuevo conjunto de consultas en diferentes momentos.

- **Aumento o disminución del número de solicitudes**: este escenario es la medida más obvia de la carga de trabajo. El número de consultas no siempre coincide con un mayor uso de los recursos. Sin embargo, esta métrica sigue siendo un indicio importante, suponiendo que otros factores no hayan cambiado.

## <a name="waiting-related-performance-problems"></a>Problemas de rendimiento relacionados con la espera 

Si está seguro de que el problema de rendimiento no está relacionado con el uso elevado de la CPU o con la ejecución, el problema está relacionado con la espera. Es decir, los recursos de CPU no se están usando de manera eficiente porque la CPU está a la espera de algún otro recurso. En este caso, identifique qué están esperando los recursos de CPU. 

Estos métodos se usan normalmente para mostrar las categorías principales de tipos de espera:

- Use el [Almacén de consultas](https://docs.microsoft.com/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store) para buscar estadísticas de espera de cada consulta a lo largo del tiempo. En el Almacén de consultas, los tipos de espera se combinan en categorías de espera. Puede encontrar la asignación de categorías de espera a tipos de espera en [sys.query_store_wait_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-query-store-wait-stats-transact-sql#wait-categories-mapping-table).
- Use [sys.dm_db_wait_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-wait-stats-azure-sql-database) para devolver información sobre todas las esperas encontradas por los subprocesos ejecutados durante la operación. Puede usar esta vista agregada para diagnosticar problemas de rendimiento con Azure SQL Database y también con consultas y lotes específicos.
- Use [sys.dm_os_waiting_tasks](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-os-waiting-tasks-transact-sql) para devolver información sobre la cola de tareas que están a la espera de recursos.

En escenarios de CPU elevada, puede ser que el Almacén de consultas y las estadísticas de espera no reflejen el uso de CPU en los siguientes casos:

- Las consultas que consumen mucha CPU todavía se están ejecutando.
- Las consultas que consumen mucha CPU se estaban ejecutando cuando se produjo una conmutación por error.

Las vistas DMV que realizan un seguimiento del Almacén de consultas y de las estadísticas de espera muestran los resultados únicamente de las consultas realizadas correctamente y de las que han agotado el tiempo de espera. No muestran los datos de las instrucciones que se ejecutan actualmente hasta que finaliza la instrucción. Use la vista de administración dinámica [sys.dm_exec_requests](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql) para realizar un seguimiento de las consultas en ejecución y del tiempo de trabajo asociado.

En el gráfico que se encuentra cerca del principio de este artículo se muestra que las esperas más comunes son:

- Bloqueos
- E/S
- Contención relacionada con TempDB
- Esperas de concesión de memoria

> [!IMPORTANT]
> Para ver un conjunto de consultas T-SQL que usan vistas DMV para solucionar problemas relacionados con la espera, vea:
>
> - [Identificar problemas de rendimiento de E/S](sql-database-monitoring-with-dmvs.md#identify-io-performance-issues)
> - [Identificar esperas de concesión de memoria](sql-database-monitoring-with-dmvs.md#identify-memory-grant-wait-performance-issues)
> - [TigerToolbox waits and latches](https://github.com/Microsoft/tigertoolbox/tree/master/Waits-and-Latches) (TigerToolbox: esperas y bloqueos temporales)
> - [TigerToolbox: usp_whatsup](https://github.com/Microsoft/tigertoolbox/tree/master/usp_WhatsUp)

## <a name="improve-database-performance-with-more-resources"></a>Mejora del rendimiento de la base de datos con más recursos

Si no hay elementos procesables que puedan mejorar el rendimiento de la base de datos, puede cambiar la cantidad de recursos disponibles en Azure SQL Database. Cambie el [nivel de servicio DTU](sql-database-service-tiers-dtu.md) de una sola base de datos para asignar más recursos. O bien, aumente el número de eDTU de un grupo elástico en cualquier momento. También, si usa el [modelo de compra basado en núcleo virtual](sql-database-service-tiers-vcore.md), cambie el nivel de servicio o aumente los recursos asignados a la base de datos.

En el caso de bases de datos individuales, puede [cambiar los niveles de servicio o los recursos de proceso](sql-database-single-database-scale.md) a petición para mejorar el rendimiento de la base de datos. Para varias bases de datos, considere el uso de [grupos elásticos](sql-database-elastic-pool-guidance.md) para escalar recursos automáticamente.

## <a name="tune-and-refactor-application-or-database-code"></a>Optimización y refactorización del código de la aplicación o base de datos

Puede optimizar el código de aplicación de la base de datos, cambiar índices, forzar planes o usar sugerencias para adaptar manualmente la base de datos a la carga de trabajo. Para información sobre cómo ajustar y volver a escribir el código, consulte [Guía para la optimización del rendimiento](sql-database-performance-guidance.md).

## <a name="next-steps"></a>Pasos siguientes

- Para habilitar el ajuste automático en Azure SQL Database y permitir que administre totalmente la carga de trabajo, consulte [Habilitación del ajuste automático](sql-database-automatic-tuning-enable.md).
- Para usar el ajuste manual, revise [Recomendaciones de optimización en Azure Portal](sql-database-advisor-portal.md). Aplique manualmente las recomendaciones que mejoren el rendimiento de las consultas.
- Cambie los recursos que están disponibles en la base de datos; para ello, modifique los [niveles de servicio de Azure SQL Database](sql-database-performance-guidance.md).
