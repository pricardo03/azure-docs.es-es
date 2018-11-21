---
title: Optimizar el vaciado automático en un servidor de Azure Database for PostgreSQL
description: En este artículo se describe cómo puede optimizar el vaciado automático en un servidor de Azure Database for PostgreSQL.
author: dianaputnam
ms.author: dianas
editor: jasonwhowell
ms.service: postgresql
ms.topic: conceptual
ms.date: 10/22/2018
ms.openlocfilehash: 3f35779337082b7280398bd13ef870c74f3ec082
ms.sourcegitcommit: db2cb1c4add355074c384f403c8d9fcd03d12b0c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2018
ms.locfileid: "51685997"
---
# <a name="optimizing-autovacuum-on-azure-database-for-postgresql-server"></a>Optimizar el vaciado automático en un servidor de Azure Database for PostgreSQL 
En este artículo se describe cómo puede optimizar el vaciado automático en Azure Database for PostgreSQL.

## <a name="overview-of-autovacuum"></a>Información general sobre el vaciado automático
PostgreSQL usa MVCC para permitir una mayor simultaneidad de bases de datos. Cada actualización da como resultado una inserción y eliminación, y cada proceso de eliminación da como resultado que las filas estén marcadas de forma temporal para su eliminación. La marca temporal da como resultado la identificación de tuplas inactivas que se eliminarán más adelante. PostgreSQL logra todo esto al ejecutar un trabajo de vaciado.

Puede activar un trabajo de vaciado de forma manual o automática. Aparecerán más tuplas inactivas cuando la base de datos experimente una gran cantidad de operaciones de actualización o eliminación y aparecerán menos cuando esta esté inactiva.  La necesidad de ejecutar una operación de vaciado con mayor frecuencia es mayor cuando la base de datos tiene una gran carga; así pues, ejecutar los trabajos de vaciado de forma **manual** puede resultarle bastante incómodo.

El vaciado automático se puede configurar y aprovecharse de las opciones de ajuste. Los valores predeterminados que incluye PostgreSQL intentan garantizar que el producto funcione en todo tipo de dispositivos (incluyendo dispositivos Raspberry Pi); asimismo, los valores de configuración ideales dependen de varios factores:
- Total de recursos disponibles - SKU y tamaño de almacenamiento.
- Uso de los recursos.
- Características de objetos individuales.

## <a name="autovacuum-benefits"></a>Ventajas del vaciado automático
Si no ejecuta la opción de vaciado de vez en cuando, las tuplas inactivas que se acumulan en pueden convertirse en:
- Sobredimensionamiento de datos: bases de datos y tablas aún más grandes.
- Índices de mayor tamaño poco óptimos.
- Aumento de E/S.

## <a name="monitoring-bloat-with-autovacuum-queries"></a>Supervisar el sobredimensionamiento con consultas de vaciado automático
La siguiente consulta de muestra está diseñada para identificar el número de tuplas desactivas y activas en una tabla llamada "XYZ": 'SELECT relname, n_dead_tup, n_live_tup, (n_dead_tup/ n_live_tup) AS DeadTuplesRatio, last_vacuum, last_autovacuum FROM pg_catalog.pg_stat_all_tables WHERE relname = 'XYZ' order by n_dead_tup DESC;'.

## <a name="autovacuum-configurations"></a>Configuraciones del vaciado automático
Los parámetros de configuración que controlan el vaciado automático giran en torno a dos preguntas claves:
- ¿Cuándo debe comenzar?
- ¿Cuánto debe limpiar después de iniciarse?

A continuación, se incluyen algunos de los parámetros de configuración del vaciado automático que puede actualizar según las preguntas anteriores, junto con algunas pautas:
Parámetro|DESCRIPCIÓN|Valor predeterminado
---|---|---
autovacuum_vacuum_threshold|Especifica el número mínimo de tuplas actualizadas o eliminadas necesarias para desencadenar una operación VACUUM en cualquier tabla. El valor predeterminado es de 50 tuplas. Este parámetro solo se puede configurar en el archivo postgresql.conf o en la línea de comandos del servidor. Asimismo, la configuración se puede anular en tablas individuales cambiando los parámetros de almacenamiento de la tabla en cuestión.|50
autovacuum_vacuum_scale_factor|Especifica una fracción del tamaño de la tabla para agregar a autovacuum_vacuum_threshold cuando se decide activar una operación VACUUM. El valor predeterminado es 0,2 (20 % del tamaño de la tabla). Este parámetro solo se puede configurar en el archivo postgresql.conf o en la línea de comandos del servidor. Asimismo, la configuración se puede anular en tablas individuales cambiando los parámetros de almacenamiento de la tabla en cuestión.|5 %
autovacuum_vacuum_cost_limit|Especifica el valor límite del costo que se usará en las operaciones VACUUM automáticas. Si se especifica -1 (que es el valor predeterminado), se usará el valor normal vacuum_cost_limit. El valor se distribuye proporcionalmente entre los trabajos de vaciado automático en funcionamiento, si es que hay más de un trabajo. La suma de los límites de cada trabajo no excede el valor de esta variable. Este parámetro solo se puede configurar en el archivo postgresql.conf o en la línea de comandos del servidor. Asimismo, la configuración se puede anular en tablas individuales cambiando los parámetros de almacenamiento de la tabla en cuestión.|-1
autovacuum_vacuum_cost_delay|Especifica el valor de demora del costo que se usará en las operaciones VACUUM automáticas. Si se especifica -1, se usará el valor normal vacuum_cost_delay. El valor predeterminado es 20 milisegundos. Este parámetro solo se puede configurar en el archivo postgresql.conf o en la línea de comandos del servidor. Asimismo, la configuración se puede anular en tablas individuales cambiando los parámetros de almacenamiento de la tabla en cuestión.|20 ms
autovacuum_nap_time|Especifica el retraso mínimo entre las ejecuciones de vaciado automáticas en cualquier base de datos determinada. En cada ronda, el demonio examina la base de datos y emite los comandos VACUUM y ANALYZE según sea necesario para las tablas en esa base de datos. El retraso se mide en segundos y el valor predeterminado es de un minuto (1 minuto). Este parámetro solo se puede configurar en el archivo postgresql.conf o en la línea de comandos del servidor.|15 s
autovacuum_max_workers|Especifica el número máximo de procesos de vaciado automático (distintos del selector de vaciado automático) que pueden estar ejecutándose en cualquier momento. El valor predeterminado es tres. Este parámetro solo se puede configurar al inicio del servidor.|3
La configuración anterior se puede anular en tablas individuales cambiando los parámetros de almacenamiento de la tabla en cuestión.  

## <a name="autovacuum-cost"></a>Costo del vaciado automático
A continuación se muestran los "costos" de ejecutar una operación de vaciado:
- Se coloca un bloqueo en las páginas de datos en las que se ejecutará el vaciado.
- El procesamiento y la memoria se usan cuando se está ejecutando el vaciado.

Esto implica que el vaciado no debe ejecutarse con demasiada o poca frecuencia; debe adaptarse a la carga de trabajo. Le recomendamos probar todos los cambios de parámetros del vaciado automático debido a las ventajas y desventajas de cada uno de ellos.

## <a name="autovacuum-start-trigger"></a>Desencadenador de inicio del vaciado automático
El vaciado automático se activa cuando el número de tuplas desactivadas excede autovacuum_vacuum_threshold + autovacuum_vacuum_scale_factor * reltuples ("reltuples" aquí es una constante).

La limpieza que se realiza con el vaciado automático debe estar actualizada en función de la carga de la base de datos; de lo contrario, podría quedarse sin almacenamiento y experimentar una desaceleración general en las consultas. Como se amortiza con el tiempo, la velocidad a la que el vaciado limpia las tuplas desactivadas debe ser igual a la velocidad a la que se crean las tuplas desactivadas.

Tenga en cuenta que las bases de datos con muchas actualizaciones y eliminaciones tienen más tuplas desactivadas y necesitan más espacio. En general, las bases de datos con muchas actualizaciones y eliminaciones se benefician de los valores bajos de autovacuum_vacuum_scale_factor y de autovacuum_vacuum_threshold para evitar la acumulación prolongada de tuplas desactivadas. De todas formas, puede usar valores más altos para ambos parámetros con bases de datos más pequeñas porque la necesidad de vaciado es menos urgente. Le recordamos que el proceso frecuente de vaciado conlleva un costo en el procesamiento y la memoria.

El factor de escala predeterminado del 20 por ciento funciona bien en tablas con un bajo porcentaje de tuplas desactivadas, pero no en tablas con un alto porcentaje de tuplas desactivadas. Por ejemplo, en una tabla de 20 GB esto se traduce en 4 GB de tuplas desactivadas; si se trata de una tabla de 1 TB, le corresponderían son 200 GB de tuplas desactivadas.

Con PostgreSQL puede configurar estos parámetros a nivel de tabla o de instancia. Hoy en día, estos parámetros se pueden establecer a nivel de tabla solo en Azure Database for PostgreSQL.

## <a name="estimating-the-cost-of-autovacuum"></a>Estimar el costo del vaciado automático
Ejecutar el vaciado automático es "costoso" y hay parámetros para controlar el tiempo de ejecución de las operaciones de vaciado. Los siguientes parámetros le ayudarán a estimar el costo de ejecutar el vaciado:
- vacuum_cost_page_hit = 1
- vacuum_cost_page_miss = 10
- vacuum_cost_page_dirty = 20

El proceso de vaciado lee las páginas físicas y busca tuplas desactivadas. Se considera que cada página en shared_buffers tiene un costo de 1 (vacuum_cost_page_hit); todas las demás páginas tienen un costo de 20 (vacuum_cost_page_dirty) si existen tuplas desactivadas, o de 10 (vacuum_cost_page_miss) si no existen tuplas desactivadas. La operación de vaciado se detiene cuando el proceso excede autovacuum_vacuum_cost_limit.  

Una vez que se alcanza el límite, el proceso se detiene durante el tiempo que especificó el parámetro autovacuum_vacuum_cost_delay antes de volver a iniciarlo. Si no se alcanza el límite, el vaciado automático comienza después del valor que especificó el parámetro autovacuum_nap_time.

En resumen, los parámetros autovacuum_vacuum_cost_delay y autovacuum_vacuum_cost_limit controlan cuántos datos se pueden limpiar por unidad de tiempo. Tenga en cuenta que los valores predeterminados son demasiado bajos para la mayoría de los planes de tarifa. Los valores óptimos para estos parámetros dependen de esos planes de tarifa y deben configurarse en consecuencia.

El parámetro autovacuum_max_workers determina el número máximo de procesos de vaciado automático que pueden ejecutarse simultáneamente.

Con PostgreSQL puede configurar estos parámetros a nivel de tabla o de instancia. Hoy en día, estos parámetros se pueden establecer a nivel de tabla solo en Azure Database for PostgreSQL.

## <a name="optimizing-autovacuum-per-table"></a>Optimizar el vaciado automático por tabla
Todos los parámetros de configuración anteriores pueden configurarse en función de la tabla; por ejemplo:
```sql
ALTER TABLE t SET (autovacuum_vacuum_threshold = 1000);
ALTER TABLE t SET (autovacuum_vacuum_scale_factor = 0.1);
ALTER TABLE t SET (autovacuum_vacuum_cost_limit = 1000);
ALTER TABLE t SET (autovacuum_vacuum_cost_delay = 10);
```

El vaciado automático es un proceso sincrónico que se realiza por tabla. Cuanto mayor es el porcentaje de tuplas desactivadas que tiene una tabla, mayor es el "costo" del vaciado automático.  Si divide las tablas que tienen una alta tasa de actualizaciones y eliminaciones en varias tablas, podrá paralelizar el vaciado automático y reducirá el "costo" de completar la operación de vaciado automático en una tabla. También puede aumentar el número de trabajos paralelos de vaciado automático para asegurarse de que los trabajos se programan de forma independiente.

## <a name="next-steps"></a>Pasos siguientes
Revise la siguiente documentación de PostgreSQL para obtener más información sobre el uso y optimización del vaciado automático:
 - Documentación de PostgreSQL: [Chapter 18, Server Configuration](https://www.postgresql.org/docs/9.5/static/runtime-config-autovacuum.html) (Capítulo 18; configuración del servidor)
 - Documentación de PostgreSQL: [Chapter 24, Routine Database Maintenance Tasks](https://www.postgresql.org/docs/9.6/static/routine-vacuuming.html) (Capítulo 24; tareas rutinarias de mantenimiento de base de datos)
