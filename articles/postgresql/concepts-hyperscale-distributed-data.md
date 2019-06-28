---
title: 'Datos distribuidos en Azure Database for PostgreSQL: Hiperescala (Citus) (versión preliminar)'
description: Tablas y particiones de base de datos distribuidas en el grupo de servidores.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 05/06/2019
ms.openlocfilehash: 9020ee690d93a1b477471fac4a482a909fca5935
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65077341"
---
# <a name="distributed-data-in-azure-database-for-postgresql--hyperscale-citus-preview"></a>Datos distribuidos en Azure Database for PostgreSQL: Hiperescala (Citus) (versión preliminar)

En este artículo se describen los tres tipos de tablas de Hiperescala (Citus).
Muestra la forma en que se almacenan las tablas distribuidas como particiones de base de datos y la manera en que las particiones de base de datos se colocan en nodos.

## <a name="table-types"></a>Tipos de tablas

Hay tres tipos de tablas en un grupo de servidores Hiperescala, cada una se utiliza para propósitos diferentes.

### <a name="type-1-distributed-tables"></a>Tipo 1: tablas distribuidas

El primer tipo y más común son las *distribuidas*. Aparecen como tablas normales para las instrucciones SQL, pero están *particionadas* horizontalmente en nodos de trabajo. Esto significa que las filas de la tabla se almacenan en nodos diferentes, en tablas de fragmentos llamadas *particiones de base de datos*.

Hiperescala no solo ejecuta SQL, sino también las instrucciones de DDL de un clúster, por lo que al cambiar el esquema de una tabla distribuida se actualizan en cascada todas las particiones de base de datos de la tabla a través de los trabajos.

#### <a name="distribution-column"></a>Columna de distribución

Hiperescala usa el particionamiento algorítmico para asignar filas a particiones de base de datos. La asignación se realiza de forma determinista en función del valor de una columna de tabla denominada *columna de distribución.* El administrador de clústeres debe designar esta columna al distribuir una tabla.
Realizar la elección correcta es importante para el rendimiento y la funcionalidad.

### <a name="type-2-reference-tables"></a>Tipo 2: tablas de referencia

Una tabla de referencia es un tipo de tabla distribuida en la que todo su contenido se concentra en una sola partición de base de datos. La partición de base de datos se replica en todos los trabajos, por lo que las consultas en cualquier trabajo pueden tener acceso a la información de referencia localmente, sin la sobrecarga de red de solicitar filas de otro nodo. Las tablas de referencia no tienen ninguna columna de distribución porque no es necesario distinguir particiones de base de datos independientes por cada fila.

Las tablas de referencia suelen ser pequeñas y se usan para almacenar los datos que son pertinentes para las consultas que se ejecutan en cualquier nodo de trabajo. Por ejemplo, se enumeran valores como estados de pedido o categorías de productos.

### <a name="type-3-local-tables"></a>Tipo 3: tablas locales

Cuando se usa Hiperescala, el nodo de coordinación al que se conecta es una base de datos PostgreSQL normal. Puede crear tablas normales en el coordinador y elegir no particionarlas.

Un buen candidato para las tablas locales serían pequeñas tablas administrativas que no participan en las consultas de combinación. Por ejemplo, una tabla de usuarios para el inicio de sesión de aplicación y la autenticación.

## <a name="shards"></a>Particiones de base de datos

En la sección anterior, se describe la manera en que las tablas distribuidas se almacenan como particiones de base de datos en nodos de trabajo. En esta sección se profundiza más en los detalles técnicos.

La tabla de metadatos `pg_dist_shard` del coordinador contiene una fila para cada partición de base de datos de cada tabla distribuida en el sistema. La fila asocia un id. de partición con un intervalo de enteros en un espacio de hash (shardminvalue, shardmaxvalue):

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

Si el nodo de coordinación quiere determinar qué partición de base de datos contiene una fila de `github_events`, se aplica un código hash al valor de la columna de distribución de la fila y comprueba qué intervalo de la partición de base de datos contiene el valor con hash. (Los intervalos se definen para que la imagen de la función hash sea su unión independiente).

### <a name="shard-placements"></a>Colocaciones de particiones

Supongamos que la partición de base de datos 102027 está asociada a la fila en cuestión. La fila se leerá o escribirá en una tabla denominada `github_events_102027` de uno de los trabajos. ¿Qué trabajo? Se determina completamente por las tablas de metadatos y la asignación de partición de base de datos para el trabajo se conoce como *colocación* de la partición.

El nodo de coordinación vuelve a escribir consultas en fragmentos que hacen referencia a las tablas específicas como `github_events_102027`, y ejecuta esos fragmentos en los trabajos adecuados. Este es un ejemplo de una consulta que se ejecuta en segundo plano para buscar el nodo que contiene el id. de partición de base de datos 102027.

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
- Aprender a [elegir una columna de distribución](concepts-hyperscale-choose-distribution-column.md) para las tablas distribuidas
