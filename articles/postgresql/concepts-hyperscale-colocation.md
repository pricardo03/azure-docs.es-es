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
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65077476"
---
# <a name="table-colocation-in-azure-database-for-postgresql--hyperscale-citus-preview"></a>Coubicación en tabla de Azure Database for PostgreSQL: Hiperescala (Citus) (versión preliminar)

La coubicación significa almacenar junta la información relacionada, en los mismos nodos. Las consultas pueden ser rápidas cuando todos los datos necesarios están disponibles sin ningún tráfico de red. La coubicación de los datos relacionados en diferentes nodos permite que las consultas se ejecuten eficazmente en paralelo en cada nodo.

## <a name="data-colocation-for-hash-distributed-tables"></a>Coubicación de datos para tablas distribuidas mediante una función hash

En Hiperescala, se almacena una fila en una partición de base de datos si el hash del valor de la columna de distribución se encuentra dentro del intervalo de hash de la partición de base de datos. Las particiones de base de datos con el mismo intervalo de hash siempre se colocan en el mismo nodo. Las filas con valores de columna de distribución iguales siempre se encuentran en el mismo nodo en las tablas.

![particiones de base de datos](media/concepts-hyperscale-colocation/colocation-shards.png)

## <a name="a-practical-example-of-colocation"></a>Un ejemplo práctico de coubicación

Considere las siguientes tablas que pueden formar parte de un análisis web multiinquilino SaaS:

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

Queremos responder a las consultas que se pueden emitir mediante un panel orientado al cliente, como: "Devolver el número de visitas de la semana pasada para todas las páginas que empiezan por "/blog" en el inquilino seis."

Si los datos se encuentran en una opción de implementación de servidor único, nuestra consulta podría expresarse fácilmente mediante el amplio conjunto de operaciones relacionales que ofrece SQL:

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

Siempre y cuando el [espacio de trabajo](https://en.wikipedia.org/wiki/Working_set) para esta consulta se ajuste a la memoria, una tabla de servidor único es una solución adecuada. Sin embargo, vamos a considerar las oportunidades de escalado del modelo de datos con la opción de implementación de Hiperescala.

### <a name="distributing-tables-by-id"></a>Distribución de tablas por id.

Las consultas de servidor único empiezan a ralentizarse a medida que crece el número de inquilinos y los datos almacenados para cada inquilino. El espacio de trabajo deja de ajustarse a la memoria y CPU se convierte en un cuello de botella.

En este caso, se pueden particionar los datos entre varios nodos mediante Hiperescala. La primera elección y la más importante que se debe tomar en el particionamiento es la columna de distribución. Comencemos por la elección simple de usar `event_id` para la tabla de eventos y `page_id` para la tabla `page`:

```sql
-- naively use event_id and page_id as distribution columns

SELECT create_distributed_table('event', 'event_id');
SELECT create_distributed_table('page', 'page_id');
```

Cuando los datos se dispersan entre distintos trabajos, no podemos realizar una combinación como se haría en un único nodo de PostgreSQL. En su lugar, necesitamos emitir dos consultas:

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

Después, la aplicación debe combinar los resultados de los dos pasos.

La ejecución de las consultas debe consultar los datos de particiones de base de datos repartidos entre los nodos.

![consultas ineficaces](media/concepts-hyperscale-colocation/colocation-inefficient-queries.png)

En este caso, la distribución de datos crea inconvenientes importantes:

-   La sobrecarga de consultar cada partición de base de datos, mediante la ejecución de varias consultas.
-   La sobrecarga debida a que Q1 devuelve muchas filas al cliente.
-   El crecimiento de Q2.
-   La necesidad de escribir consultas en varios pasos requiere cambios en la aplicación.

Dado que los datos están dispersos, se pueden paralelizar las consultas. Sin embargo, solo es beneficioso si la cantidad de trabajo que realiza la consulta es considerablemente mayor que la sobrecarga de consultar varias particiones de base de datos.

### <a name="distributing-tables-by-tenant"></a>Distribución de tablas por inquilino

En Hiperescala, se garantiza que las filas con el mismo valor de columna de distribución van a estar en el mismo nodo. Si empezamos de nuevo, podemos crear nuestras tablas con `tenant_id` como columna de distribución.

```sql
-- co-locate tables by using a common distribution column
SELECT create_distributed_table('event', 'tenant_id');
SELECT create_distributed_table('page', 'tenant_id', colocate_with => 'event');
```

Ahora, Hiperescala puede responder a la consulta original de servidor único sin modificaciones (Q1):

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

Debido al filtro y la combinación en tenant_id, Hiperescala sabe que toda la consulta se puede responder con el conjunto de particiones de base de datos coubicadas que contienen los datos para ese inquilino determinado. Un único nodo de PostgreSQL puede resolver la consulta en un solo paso.

![consulta mejor](media/concepts-hyperscale-colocation/colocation-better-query.png)

En algunos casos, las consultas y los esquemas de tabla deben cambiarse para incluir el id. de inquilino en las restricciones únicas y condiciones de combinación. Sin embargo, esto suele ser un cambio sencillo.

## <a name="next-steps"></a>Pasos siguientes

- Vea cómo se coubican los datos de inquilino en el [tutorial de multiinquilino](tutorial-design-database-hyperscale-multi-tenant.md)
