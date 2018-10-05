---
title: Supervisión de Azure SQL Database con vistas de administración dinámica | Microsoft Docs
description: Obtenga información sobre cómo detectar y diagnosticar problemas comunes de rendimiento con vistas de administración dinámica para supervisar Microsoft Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 08/08/2018
ms.openlocfilehash: 97907eee9982fdf6a804bc13edbf8c14efa4ce42
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2018
ms.locfileid: "47161393"
---
# <a name="monitoring-azure-sql-database-using-dynamic-management-views"></a>Supervisar Azure SQL Database con vistas de administración dinámica
Microsoft Azure SQL Database habilita un subconjunto de vistas de administración dinámica para diagnosticar problemas de rendimiento, que pueden deberse a consultas bloqueadas o de ejecución prolongada, cuellos de botella de recursos, planes de consulta deficientes, etc. En este tema se ofrece información sobre cómo encontrar problemas comunes de rendimiento con vistas de administración dinámica.

SQL Database admite parcialmente tres categorías de vistas de administración dinámica:

* Vistas de administración dinámica relacionadas con bases de datos.
* Vistas de administración dinámica relacionadas con ejecuciones.
* Vistas de administración dinámica relacionadas con transacciones.

Para obtener información detallada sobre las vistas de administración dinámica, vea [Vistas y funciones de administración dinámica (Transact-SQL)](https://msdn.microsoft.com/library/ms188754.aspx) en los libros en pantalla de SQL Server.

## <a name="permissions"></a>Permisos
En SQL Database, para realizar consultas en una vista de administración dinámica se requiere disponer de permisos **VIEW DATABASE STATE** . El permiso **VIEW DATABASE STATE** devuelve información sobre todos los objetos de la base de datos actual.
Para conceder el permiso **VIEW DATABASE STATE** a un usuario de base de datos en concreto, ejecute la consulta siguiente:

```GRANT VIEW DATABASE STATE TO database_user; ```

En una instancia de SQL Server local, las vistas de administración dinámica devuelven la información de estado del servidor. En SQL Database, devuelven información relativa únicamente a la base de datos lógica actual.

## <a name="calculating-database-size"></a>Calculando tamaño de la base de datos
La siguiente consulta devuelve el tamaño de la base de datos en megabytes:

```
-- Calculates the size of the database.
SELECT SUM(CAST(FILEPROPERTY(name, 'SpaceUsed') AS bigint) * 8192.) / 1024 / 1024 AS DatabaseSizeInMB
FROM sys.database_files
WHERE type_desc = 'ROWS';
GO
```

La consulta siguiente devuelve el tamaño de objetos individuales (en megabytes) de la base de datos:

```
-- Calculates the size of individual database objects.
SELECT sys.objects.name, SUM(reserved_page_count) * 8.0 / 1024
FROM sys.dm_db_partition_stats, sys.objects
WHERE sys.dm_db_partition_stats.object_id = sys.objects.object_id
GROUP BY sys.objects.name;
GO
```

## <a name="monitoring-connections"></a>Supervisión de conexiones
Puede usar la vista [sys.dm_exec_connections](https://msdn.microsoft.com/library/ms181509.aspx) para recuperar información sobre las conexiones establecidas con un servidor concreto de Azure SQL Database y los detalles de cada conexión. Además, la vista [sys.dm_exec_sessions](https://msdn.microsoft.com/library/ms176013.aspx) resulta útil para recuperar información sobre todas las conexiones de usuario activas y las tareas internas.
La siguiente consulta recupera información sobre la conexión actual:

```
SELECT
    c.session_id, c.net_transport, c.encrypt_option,
    c.auth_scheme, s.host_name, s.program_name,
    s.client_interface_name, s.login_name, s.nt_domain,
    s.nt_user_name, s.original_login_name, c.connect_time,
    s.login_time
FROM sys.dm_exec_connections AS c
JOIN sys.dm_exec_sessions AS s
    ON c.session_id = s.session_id
WHERE c.session_id = @@SPID;
```

> [!NOTE]
> Al ejecutar las vistas **sys.dm_exec_requests** y **sys.dm_exec_sessions**, si el usuario tiene permiso **VIEW DATABASE STATE** en la base de datos, verá todas las sesiones en ejecución en la base de datos; en caso contrario, el usuario solo verá la sesión actual.
> 
> 

## <a name="monitor-resource-use"></a>Supervisión del uso de recursos

Puede supervisar el uso de recursos con [Información de rendimiento de consultas de SQL Database](sql-database-query-performance.md) y [Almacén de consultas](https://msdn.microsoft.com/library/dn817826.aspx).

También puede supervisar el uso con estas dos vistas:

* [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx)
* [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx)

### <a name="sysdmdbresourcestats"></a>sys.dm_db_resource_stats
Puede usar la vista [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) en cada SQL Database. La vista **sys.dm_db_resource_stats** muestra los datos de uso reciente de recursos en relación con el nivel de servicio. Porcentajes medios para CPU, E/S de datos, escritura de registros y memoria, se registran cada 15 segundos y se mantienen durante una hora.

Dado que esta vista proporciona una panorámica más granular del uso de recursos, use primero **sys.dm_db_resource_stats** para cualquier análisis o solución de problemas de estado actual. Por ejemplo, esta consulta muestra el uso medio y máximo de recursos para la base de datos actual durante la última hora:

    SELECT  
        AVG(avg_cpu_percent) AS 'Average CPU use in percent',
        MAX(avg_cpu_percent) AS 'Maximum CPU use in percent',
        AVG(avg_data_io_percent) AS 'Average data IO in percent',
        MAX(avg_data_io_percent) AS 'Maximum data IO in percent',
        AVG(avg_log_write_percent) AS 'Average log write use in percent',
        MAX(avg_log_write_percent) AS 'Maximum log write use in percent',
        AVG(avg_memory_usage_percent) AS 'Average memory use in percent',
        MAX(avg_memory_usage_percent) AS 'Maximum memory use in percent'
    FROM sys.dm_db_resource_stats;  

Para otras consultas, consulte los ejemplos de [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx).

### <a name="sysresourcestats"></a>Sys.resource_stats
La vista [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) de la base de datos **maestra** proporciona información adicional que puede ayudar a supervisar el rendimiento de la base de datos SQL Database en su nivel de servicio y tamaño de proceso específicos. Los datos se recopilan cada cinco minutos y se mantienen durante aproximadamente 14 días. Esta vista es útil para realizar análisis históricos a largo plazo de cómo la base de datos SQL usa los recursos.

En el siguiente gráfico se muestra el uso de recursos de CPU para una base de datos Premium con el tamaño de proceso P2 durante cada hora de una semana. Este gráfico en concreto empieza el lunes, muestra 5 días laborables y, a continuación, un fin de semana cuando hay mucha menos actividad en la aplicación.

![Uso de recursos de base de datos SQL](./media/sql-database-performance-guidance/sql_db_resource_utilization.png)

Según los datos, esta base de datos tiene actualmente una carga máxima de CPU superior al 50 % de uso de la CPU respecto al tamaño de proceso P2 (mediodía del martes). Si la CPU es el factor dominante en el perfil de recursos de la aplicación, puede decidir que P2 es el tamaño de proceso adecuado para garantizar que la carga de trabajo siempre se ajuste. Si espera que una aplicación crezca con el tiempo, es una buena idea tener un búfer de recursos adicionales para que la aplicación no alcance el límite del nivel de rendimiento. Si aumenta el tamaño de proceso, puede ayudar a evitar errores visibles para el cliente que pueden producirse cuando una base de datos no tiene suficiente capacidad para procesar las solicitudes de manera eficaz, especialmente en entornos sensibles a la latencia. Un ejemplo es una base de datos que admite una aplicación que dibuja páginas web basadas en los resultados de las llamadas de base de datos.

Otros tipos de aplicaciones pueden interpretar el mismo gráfico de manera diferente. Por ejemplo, si una aplicación intenta procesar datos de nóminas todos los días y tiene el mismo gráfico, este tipo de modelo de "trabajo por lotes" podría funcionar bien en un tamaño de proceso P1. El tamaño de proceso P1 tiene 100 DTU en comparación con las 200 DTU en el tamaño de proceso P2. El tamaño de proceso P1 proporciona la mitad de rendimiento que el tamaño de proceso P2. Por lo tanto, el 50 por ciento de uso de CPU en P2 es igual al 100 por cien de uso de CPU en P1. Si la aplicación no tiene tiempos de espera, puede dar igual que un trabajo tarde 2 o 2,5 horas en completarse, siempre que se termine hoy. Una aplicación de esta categoría probablemente pueda usar un tamaño de proceso P1. Puede aprovechar el hecho de que hay períodos de tiempo durante el día en que el uso de recursos es menor, lo que significa que las "cargas elevadas" podrían retrasarse a uno de esos momentos más tarde ese día. El tamaño de proceso P1 podría ser conveniente para ese tipo de aplicación (y ahorrar dinero), siempre y cuando los trabajos se puedan finalizar a tiempo cada día.

Azure SQL Database muestra información sobre los recursos consumidos para cada base de datos activa en la vista **sys.resource_stats** de la base de datos **maestra** de cada servidor. Los datos de la tabla se agregan en intervalos de 5 minutos. Con los niveles de servicio Básico, Estándar y Premium, los datos pueden tardar más de cinco minutos en aparecer en la tabla, así que estos datos son mejores para el análisis histórico que para el análisis casi en tiempo real. Consulte la vista **sys.resource_stats** para ver el historial reciente de una base de datos y para validar si la reserva elegida proporciona el rendimiento que quiere cuando lo necesita.

> [!NOTE]
> Tiene que estar conectado a la base de datos **maestra** de su servidor lógico de SQL Database para poder consultar **sys.resource_stats** en los ejemplos siguientes.
> 
> 

En este ejemplo se muestra cómo se exponen los datos en esta vista:

    SELECT TOP 10 *
    FROM sys.resource_stats
    WHERE database_name = 'resource1'
    ORDER BY start_time DESC

![La vista de catálogo sys.resource_stats](./media/sql-database-performance-guidance/sys_resource_stats.png)

En el siguiente ejemplo se muestran distintas maneras en que puede usar la vista de catálogo **sys.resource_stats** para obtener información sobre cómo SQL Database usa los recursos:

1. Para ver el uso de recursos de la semana pasada para la base de datos userdb1, puede ejecutar esta consulta:
   
        SELECT *
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND
              start_time > DATEADD(day, -7, GETDATE())
        ORDER BY start_time DESC;
2. Para evaluar si la carga de trabajo se ajusta bien al tamaño de proceso, tiene que explorar en profundidad cada aspecto de las métricas de recursos: CPU, lecturas, escrituras, número de trabajadores y número de sesiones. Esta es una consulta revisada mediante **sys.resource_stats** para notificar los valores medio y máximo de estas métricas de recursos:
   
        SELECT
            avg(avg_cpu_percent) AS 'Average CPU use in percent',
            max(avg_cpu_percent) AS 'Maximum CPU use in percent',
            avg(avg_data_io_percent) AS 'Average physical data IO use in percent',
            max(avg_data_io_percent) AS 'Maximum physical data IO use in percent',
            avg(avg_log_write_percent) AS 'Average log write use in percent',
            max(avg_log_write_percent) AS 'Maximum log write use in percent',
            avg(max_session_percent) AS 'Average % of sessions',
            max(max_session_percent) AS 'Maximum % of sessions',
            avg(max_worker_percent) AS 'Average % of workers',
            max(max_worker_percent) AS 'Maximum % of workers'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());
3. Con la información anterior sobre los valores promedio y máximo de cada métrica de recursos, puede evaluar si la carga de trabajo se ajusta bien al tamaño de proceso que eligió. Por lo general, los valores medios de **sys.resource_stats** son una buena referencia para el tamaño de destino. Debería ser su vara de medida principal. Por ejemplo, podría estar usando el nivel de servicio Estándar con el tamaño de proceso S2. Los porcentajes de uso medio de CPU y de lecturas y escrituras de E/S están por debajo del 40 por ciento, el número medio de trabajadores está por debajo de 50 y el número medio de sesiones está por debajo de 200. La carga de trabajo podría ajustarse al tamaño de proceso S1. Es fácil ver si la base de datos se ajusta a los límites de trabajadores y de sesión. Para ver si una base de datos se ajusta a un tamaño de proceso inferior con respecto a CPU, lecturas y escrituras, divida el número de DTU del tamaño de proceso inferior por el número de DTU de su tamaño de proceso actual y multiplique el resultado por 100:
   
    **S1 DTU / S2 DTU * 100 = 20 / 50 * 100 = 40**
   
    El resultado es la diferencia porcentual de rendimiento relativa entre los dos tamaños de proceso. Si el uso de recursos no supera esta cantidad, la carga de trabajo podría ajustarse al tamaño de proceso inferior. Sin embargo, debe examinar todos los intervalos de valores de uso de recursos y determinar, según el porcentaje, con qué frecuencia se ajustaría la carga de trabajo de la base de datos al tamaño de proceso inferior. La siguiente consulta proporciona el porcentaje de ajuste por dimensión de recursos según el umbral del 40 % que hemos calculado en este ejemplo.
   
        SELECT
            (COUNT(database_name) - SUM(CASE WHEN avg_cpu_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'CPU Fit Percent'
            ,(COUNT(database_name) - SUM(CASE WHEN avg_log_write_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Log Write Fit Percent'
            ,(COUNT(database_name) - SUM(CASE WHEN avg_data_io_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Physical Data IO Fit Percent'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());
   
    En función de su objetivo de nivel de servicio de la base de datos, puede decidir si la carga de trabajo se ajusta al tamaño de proceso inferior. Si el objetivo de la carga de trabajo de la base de datos es del 99,9 % y la consulta anterior devuelve valores mayores que el 99,9 % para las tres dimensiones de recursos, es probable que la carga de trabajo se ajuste al tamaño de proceso inferior.
   
    El porcentaje de ajuste también ofrece información detallada sobre si debería pasar al siguiente tamaño de proceso superior para cumplir su objetivo. Por ejemplo, userdb1 muestra el siguiente uso de CPU durante la semana pasada:
   
   | Porcentaje medio de CPU | Porcentaje máximo de CPU |
   | --- | --- |
   | 24,5 |100,00 |
   
    El promedio de CPU es aproximadamente un cuarto del límite del tamaño de proceso, que se ajustaría bien al tamaño de proceso de la base de datos. Sin embargo, el valor máximo muestra que la base de datos alcanza el límite del tamaño de proceso. ¿Necesita pasar al siguiente tamaño de proceso superior? Observe cuántas veces la carga de trabajo alcanza el 100 % y compárelo con el objetivo de la carga de trabajo de la base de datos.
   
        SELECT
        (COUNT(database_name) - SUM(CASE WHEN avg_cpu_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'CPU fit percent'
        ,(COUNT(database_name) - SUM(CASE WHEN avg_log_write_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Log write fit percent'
        ,(COUNT(database_name) - SUM(CASE WHEN avg_data_io_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Physical data IO fit percent'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());
   
    Si esta consulta devuelve un valor inferior al 99,9 por ciento para cualquiera de las tres dimensiones de recursos, plantéese subir al siguiente tamaño de proceso o usar técnicas de optimización de aplicaciones para reducir la carga en la base de datos SQL.
4. Este ejercicio también tiene en cuenta el aumento de la carga de trabajo proyectada en el futuro.

En los grupos elásticos, puede supervisar bases de datos individuales del grupo con las técnicas descritas en esta sección. Sin embargo, también puede supervisar el grupo en conjunto. Para más información, consulte el artículo sobre la [supervisión y administración de un grupo de bases de datos elásticas](sql-database-elastic-pool-manage-portal.md).


### <a name="maximum-concurrent-requests"></a>Número máximo de solicitudes simultáneas
Para ver el número de solicitudes simultáneas, ejecute esta consulta Transact-SQL en la base de datos SQL:

    SELECT COUNT(*) AS [Concurrent_Requests]
    FROM sys.dm_exec_requests R

Para analizar la carga de trabajo de una base de datos SQL Server local, modifique esta consulta para filtrar por la base de datos específica que quiere analizar. Por ejemplo, si tiene una base de datos local denominada MyDatabase, esta consulta Transact-SQL devuelve el número de solicitudes simultáneas en esa base de datos.

    SELECT COUNT(*) AS [Concurrent_Requests]
    FROM sys.dm_exec_requests R
    INNER JOIN sys.databases D ON D.database_id = R.database_id
    AND D.name = 'MyDatabase'

Se trata simplemente de una instantánea en un solo momento dado. Para comprender mejor la carga de trabajo y los requisitos de solicitudes simultáneas, debe recopilar muchas muestras durante un período de tiempo.

### <a name="maximum-concurrent-logins"></a>Número máximo de inicios de sesión simultáneos
Puede analizar los patrones de usuario y aplicación para hacerse una idea de la frecuencia de los inicios de sesión. También podría ejecutar cargas reales en un entorno de prueba para asegurarse de que no está alcanzando este u otros límites descritos en este artículo. No hay una única consulta o vista de administración dinámica (DMV) que pueda mostrar el número o el historial de inicios de sesión simultáneos.

Si varios clientes usan la misma cadena de conexión, el servicio autentica cada inicio de sesión. Si 10 usuarios se conectan de forma simultánea a una base de datos con el mismo nombre de usuario y contraseña, habrá 10 conexiones simultáneas. Este límite se aplica solo a la duración del inicio de sesión y la autenticación. Si los mismos 10 usuarios se conectan a la base de datos de forma secuencial, el número de inicios de sesión simultáneos nunca será superior a uno.

> [!NOTE]
> Este límite no se aplica actualmente a las bases de datos de grupos elásticos.
> 
> 

### <a name="maximum-sessions"></a>Número máximo de sesiones
Para ver el número de sesiones activas actuales, ejecute esta consulta de Transact-SQL en la base de datos SQL:

    SELECT COUNT(*) AS [Sessions]
    FROM sys.dm_exec_connections

Si va a analizar una carga de trabajo de SQL Server local, modifique la consulta para centrarse en una base de datos específica. Esta consulta le ayuda a determinar las posibles necesidades de sesión para esa base de datos si la mueve a Azure SQL Database.

    SELECT COUNT(*)  AS [Sessions]
    FROM sys.dm_exec_connections C
    INNER JOIN sys.dm_exec_sessions S ON (S.session_id = C.session_id)
    INNER JOIN sys.databases D ON (D.database_id = S.database_id)
    WHERE D.name = 'MyDatabase'

Nuevamente, estas consultas devuelven un número puntual. Si recopila varias muestras con el tiempo, tendrá una mejor comprensión del uso de la sesión.

Para el análisis de SQL Database, puede obtener estadísticas históricas sobre las sesiones consultando la vista [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) y revisando la columna **active_session_count**. 

## <a name="monitoring-query-performance"></a>Supervisión del rendimiento de las consultas
Las consultas de ejecución lenta o prolongada pueden consumir una cantidad significativa de recursos del sistema. En esta sección se muestra cómo usar vistas de administración dinámica para detectar algunos problemas comunes de rendimiento de las consultas. Una referencia anterior pero todavía útil para solucionar problemas, es el artículo de Microsoft TechNet, [Solución de problemas de rendimiento en SQL Server 2008](http://download.microsoft.com/download/D/B/D/DBDE7972-1EB9-470A-BA18-58849DB3EB3B/TShootPerfProbs2008.docx) .

### <a name="finding-top-n-queries"></a>Búsqueda de las N mejores consultas
El siguiente ejemplo devuelve información acerca de las cinco consultas principales clasificadas en función del tiempo promedio de CPU. Este ejemplo agrega las consultas conforme a sus hash de consulta, por lo que las consultas lógicamente equivalentes se agrupan por sus consumos de recursos acumulativos.

```
SELECT TOP 5 query_stats.query_hash AS "Query Hash",
    SUM(query_stats.total_worker_time) / SUM(query_stats.execution_count) AS "Avg CPU Time",
    MIN(query_stats.statement_text) AS "Statement Text"
FROM
    (SELECT QS.*,
    SUBSTRING(ST.text, (QS.statement_start_offset/2) + 1,
    ((CASE statement_end_offset
        WHEN -1 THEN DATALENGTH(ST.text)
        ELSE QS.statement_end_offset END
            - QS.statement_start_offset)/2) + 1) AS statement_text
     FROM sys.dm_exec_query_stats AS QS
     CROSS APPLY sys.dm_exec_sql_text(QS.sql_handle) as ST) as query_stats
GROUP BY query_stats.query_hash
ORDER BY 2 DESC;
```

### <a name="monitoring-blocked-queries"></a>Supervisión de consultas bloqueadas
Las consultas lentas o de larga ejecución pueden contribuir al consumo excesivo de recursos y ser la consecuencia de consultas bloqueadas. La causa del bloqueo puede ser un diseño deficiente de las aplicaciones, los planes de consulta incorrectos, la falta de índices útiles, etc. Puede usar la vista sys.dm_tran_locks para obtener información sobre la actividad de bloqueo actual en el servidor de Azure SQL Database. Para obtener un ejemplo de código, consulte [sys.dm_tran_locks (Transact-SQL)](https://msdn.microsoft.com/library/ms190345.aspx) en los Libros en pantalla de SQL Server.

### <a name="monitoring-query-plans"></a>Supervisión de planes de consulta
Un plan de consulta ineficaz también puede aumentar el consumo de CPU. En el ejemplo siguiente, se usa la vista [sys.dm_exec_query_stats](https://msdn.microsoft.com/library/ms189741.aspx) para determinar qué consulta emplea la mayor cantidad acumulativa de CPU.

```
SELECT
    highest_cpu_queries.plan_handle,
    highest_cpu_queries.total_worker_time,
    q.dbid,
    q.objectid,
    q.number,
    q.encrypted,
    q.[text]
FROM
    (SELECT TOP 50
        qs.plan_handle,
        qs.total_worker_time
    FROM
        sys.dm_exec_query_stats qs
    ORDER BY qs.total_worker_time desc) AS highest_cpu_queries
    CROSS APPLY sys.dm_exec_sql_text(plan_handle) AS q
ORDER BY highest_cpu_queries.total_worker_time DESC;
```

## <a name="see-also"></a>Otras referencias
[Introducción a SQL Database](sql-database-technical-overview.md)

