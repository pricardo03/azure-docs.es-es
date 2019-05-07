---
title: 'Datos distribuidos en Azure Database for PostgreSQL: hiperescala (Citus) (versión preliminar)'
description: Las tablas y particiones distribuidas en el grupo de servidores.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 05/06/2019
ms.openlocfilehash: 9020ee690d93a1b477471fac4a482a909fca5935
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65077341"
---
# <a name="distributed-data-in-azure-database-for-postgresql--hyperscale-citus-preview"></a>Datos distribuidos en Azure Database for PostgreSQL: hiperescala (Citus) (versión preliminar)

En este artículo se describe los tipos de tres tabla en hiperescala (Citus).
Muestra las tablas distribuidas en cómo se almacenan como particiones y la manera en que las particiones se coloquen en nodos.

## <a name="table-types"></a>Tipos de tabla

Hay tres tipos de tablas en un grupo de servidores a gran escala, cada uno se utiliza para propósitos diferentes.

### <a name="type-1-distributed-tables"></a>Tipo 1: las tablas distribuidas

El primer tipo y más común, es *distribuida* tablas. Aparecen como tablas normales a instrucciones SQL, pero horizontalmente *con particiones* en nodos de trabajo. Esto significa que las filas de la tabla se almacenan en nodos diferentes, en las tablas de fragmento llamados *particiones*.

A gran escala ejecuta no solo SQL pero las instrucciones de DDL a lo largo de un clúster, por lo que cambiar el esquema de una tabla distribuida en cascada para actualizar las particiones de todos los de la tabla a través de los trabajadores.

#### <a name="distribution-column"></a>Columna de distribución

A gran escala usa particionamiento algorítmico para asignar las filas a las particiones. La asignación se toma forma determinista en función del valor de una columna de tabla denominada la *columna de distribución.* El Administrador de clústeres debe designar esta columna cuando la distribución de una tabla.
Realizar la elección correcta es importante para el rendimiento y la funcionalidad.

### <a name="type-2-reference-tables"></a>Tipo 2: las tablas de referencia

Una tabla de referencia es un tipo de tabla con distribución todo cuyo contenido se concentra en una sola partición. La partición se replica en todos los trabajadores, por lo que las consultas en cualquier trabajo pueden tener acceso a la información de referencia localmente, sin la sobrecarga de red de solicitar las filas de otro nodo. Las tablas de referencia no tienen ninguna columna de distribución porque no es necesario distinguir particiones independientes por cada fila.

Las tablas de referencia suelen ser pequeños y se usan para almacenar los datos que es relevantes para las consultas se ejecuten en cualquier nodo de trabajo. Por ejemplo, se enumeran valores como estados del orden o categorías de producto.

### <a name="type-3-local-tables"></a>Tipo 3: tablas locales

Cuando se usa a gran escala, el nodo coordinador a que se conecta es una base de datos PostgreSQL normal. Puede crear tablas normales en el coordinador y elija no para particiones.

Un buen candidato para las tablas locales sería pequeñas tablas administrativas que no participan en las consultas de combinación. Por ejemplo, una tabla de usuarios para inicio de sesión de aplicación y la autenticación.

## <a name="shards"></a>Particiones

La sección anterior se describe las tablas distribuidas cómo se almacenan como particiones en nodos de trabajo. En esta sección obtiene más en los detalles técnicos.

El `pg_dist_shard` tabla de metadatos en el coordinador contiene una fila para cada partición de cada tabla distribuida en el sistema. La fila coincide con un Id. de partición con un intervalo de enteros en un espacio de hash (shardminvalue, shardmaxvalue):

```sql
SELECT * from pg_dist_shard;
 logicalrelid  | shardid | shardstorage | shardminvalue | shardmaxvalue 
---------------+---------+--------------+---------------+---------------
 github_events |  102026 | t            | 268435456     | 402653183
 github_events |  102027 | t            | 402653184     | 536870911
 github_events |  102028 | t            | 536870912     | 671088639
 github_events |  102029 | t            | 671088640     | 805306367
 (4 rows)
```

Si desea que el nodo coordinador determinar qué partición contiene una fila de `github_events`, se aplica un algoritmo hash del valor de la columna de distribución en la fila y comprueba qué partición\'intervalo s contiene el valor de hash. (Los intervalos se definen para que la imagen de la función hash es su unión separado).

### <a name="shard-placements"></a>Ubicaciones de particiones

Supongamos que esa partición 102027 está asociada a la fila en cuestión. La fila se pueden leer o escribir en una tabla denominada `github_events_102027` en uno de los trabajadores. ¿Qué trabajo? Que está totalmente determinado por las tablas de metadatos, y la asignación de partición para el trabajo se conoce como la partición *colocación*.

El nodo coordinador vuelve a escribir consultas en fragmentos que hacen referencia a las tablas específicas como `github_events_102027`, y se ejecuta esos fragmentos en los trabajos adecuados. Este es un ejemplo de una consulta que se ejecute en segundo plano para buscar el nodo que contiene 102027 de Id. de partición.

```sql
SELECT
    shardid,
    node.nodename,
    node.nodeport
FROM pg_dist_placement placement
JOIN pg_dist_node node
  ON placement.groupid = node.groupid
 AND node.noderole = 'primary'::noderole
WHERE shardid = 102027;
```

    ┌─────────┬───────────┬──────────┐
    │ shardid │ nodename  │ nodeport │
    ├─────────┼───────────┼──────────┤
    │  102027 │ localhost │     5433 │
    └─────────┴───────────┴──────────┘

## <a name="next-steps"></a>Pasos siguientes
- Aprenda a [elegir una columna de distribución](concepts-hyperscale-choose-distribution-column.md) para las tablas distribuidas
