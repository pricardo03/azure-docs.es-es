---
title: Conceptos sobre servidores de Azure Database for PostgreSQL
description: En este artículo se incluyen consideraciones e instrucciones para configurar y administrar servidores de Azure Database for PostgreSQL.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 05/06/2019
ms.openlocfilehash: d03cfd49887adf1f6a4650e374d3e13eeca735a4
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "65077476"
---
# <a name="table-colocation-in-azure-database-for-postgresql--hyperscale-citus-preview"></a>Colocación de la tabla en Azure Database for PostgreSQL: hiperescala (Citus) (versión preliminar)

Ubicación compartida significa almacenar información relacionada juntos en los mismos nodos. Pueden ir a las consultas rápida cuando los datos necesarios todo están disponibles sin ningún tráfico de red. Colocación de los datos relacionados en diferentes nodos permite que las consultas se ejecuten eficazmente en paralelo en cada nodo.

## <a name="data-colocation-for-hash-distributed-tables"></a>Colocación de datos de tablas distribuidas por hash

En gran escala, una fila se almacena en una partición si el hash del valor de la columna de distribución se encuentra dentro del intervalo de hash de la partición. Las particiones con el mismo intervalo de hash siempre se colocan en el mismo nodo. Las filas con valores de columna de distribución equitativa siempre están en el mismo nodo en las tablas.

![Particiones](media/concepts-hyperscale-colocation/colocation-shards.png)

## <a name="a-practical-example-of-colocation"></a>Un ejemplo práctico de colocación

Tenga en cuenta las siguientes tablas que pueden formar parte de un análisis de web de varios inquilinos SaaS:

```sql
CREATE TABLE event (
  tenant_id int,
  event_id bigint,
  page_id int,
  payload jsonb,
  primary key (tenant_id, event_id)
);

CREATE TABLE page (
  tenant_id int,
  page_id int,
  path text,
  primary key (tenant_id, page_id)
);
```

Ahora queremos responder a las consultas que se pueden emitir mediante un panel orientados al cliente, como: "Devuelve el número de visitas de la semana pasada para todas las páginas a partir de ' / blog' en el inquilino seis."

Si los datos se encontraban en una opción de implementación de servidor único, esto podría expresarse fácilmente nuestra consulta mediante el amplio conjunto de operaciones relacionales que ofrece SQL:

```sql
SELECT page_id, count(event_id)
FROM
  page
LEFT JOIN  (
  SELECT * FROM event
  WHERE (payload->>'time')::timestamptz >= now() - interval '1 week'
) recent
USING (tenant_id, page_id)
WHERE tenant_id = 6 AND path LIKE '/blog%'
GROUP BY page_id;
```

Siempre y cuando la [espacio de trabajo](https://en.wikipedia.org/wiki/Working_set) para esta consulta se ajusta en la memoria, una tabla de servidor único es una solución adecuada. Sin embargo, vamos a considerar las oportunidades de escalar el modelo de datos con la opción de implementación a gran escala.

### <a name="distributing-tables-by-id"></a>Distribución de tablas por Id.

Las consultas de servidor único comenzar ralentizar a medida que crece el número de inquilinos y los datos almacenados para cada inquilino. El espacio de trabajo se detiene de conexión en la memoria y CPU se convierte en un cuello de botella.

En este caso, se puede particionar los datos entre varios nodos de uso a gran escala. La opción primera y más importante que debe tomar al particionamiento es la columna de distribución. Comencemos con una opción ingenua de usar `event_id` para la tabla de eventos y `page_id` para el `page` tabla:

```sql
-- naively use event_id and page_id as distribution columns

SELECT create_distributed_table('event', 'event_id');
SELECT create_distributed_table('page', 'page_id');
```

Cuando los datos se dispersan entre distintos trabajos, como se haría en un único nodo de PostgreSQL no realizamos una combinación. En su lugar, necesitamos emitir dos consultas:

```sql
-- (Q1) get the relevant page_ids
SELECT page_id FROM page WHERE path LIKE '/blog%' AND tenant_id = 6;

-- (Q2) get the counts
SELECT page_id, count(*) AS count
FROM event
WHERE page_id IN (/*…page IDs from first query…*/)
  AND tenant_id = 6
  AND (payload->>'time')::date >= now() - interval '1 week'
GROUP BY page_id ORDER BY count DESC LIMIT 10;
```

Después, los resultados de los dos pasos deben combinarse mediante la aplicación.

Ejecuta las consultas, debe consultar datos en particiones repartidas entre los nodos.

![consultas ineficaces](media/concepts-hyperscale-colocation/colocation-inefficient-queries.png)

En este caso, la distribución de datos crea desventajas importantes:

-   Sobrecarga de la consulta de cada partición, ejecutar varias consultas
-   Devuelve el número de filas al cliente una sobrecarga de P1
-   P2 se vuelvan grandes
-   La necesidad de escribir consultas en varios pasos requiere cambios en la aplicación

Dado que los datos están dispersos, se pueden paralelizar las consultas. Sin embargo, sólo es útil si la cantidad de trabajo que realiza la consulta es considerablemente mayor que la sobrecarga de consultar varias particiones.

### <a name="distributing-tables-by-tenant"></a>Distribución de tablas por inquilino

En a gran escala, se garantiza que las filas con el mismo valor de columna de distribución en el mismo nodo. Empezar de nuevo, podemos crear nuestras tablas con `tenant_id` como columna de distribución.

```sql
-- co-locate tables by using a common distribution column
SELECT create_distributed_table('event', 'tenant_id');
SELECT create_distributed_table('page', 'tenant_id', colocate_with => 'event');
```

Ahora a gran escala puede responder a la consulta original de servidor único sin modificaciones (P1):

```sql
SELECT page_id, count(event_id)
FROM
  page
LEFT JOIN  (
  SELECT * FROM event
  WHERE (payload->>'time')::timestamptz >= now() - interval '1 week'
) recent
USING (tenant_id, page_id)
WHERE tenant_id = 6 AND path LIKE '/blog%'
GROUP BY page_id;
```

Debido al filtro y combinación en tenant_id, hiperescala sabe que toda la consulta se puede responder con el conjunto de particiones colocados que contienen los datos para ese inquilino determinado. La consulta en un solo paso puede responder a un único nodo de PostgreSQL.

![consulta mejor](media/concepts-hyperscale-colocation/colocation-better-query.png)

En algunos casos, las consultas y los esquemas de tabla deben cambiarse para incluir el identificador del inquilino en las restricciones unique y condiciones de combinación. Sin embargo, esto suele ser un cambio sencillo.

## <a name="next-steps"></a>Pasos siguientes

- Vea cómo los datos de inquilino están colocados en el [tutorial de varios inquilinos](tutorial-design-database-hyperscale-multi-tenant.md)
