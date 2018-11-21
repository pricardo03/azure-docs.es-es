---
title: Optimización de inserciones masivas en un servidor de Azure Database for PostgreSQL
description: En este artículo se describe cómo puede optimizar las operaciones de inserción masiva en un servidor de Azure Database for PostgreSQL.
author: dianaputnam
ms.author: dianas
editor: jasonwhowell
ms.service: postgresql
ms.topic: conceptual
ms.date: 10/22/2018
ms.openlocfilehash: 2fe3c3cc71823880d71223334b89816199561ca9
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2018
ms.locfileid: "51628886"
---
# <a name="optimizing-bulk-inserts-and-use-of-transient-data-on-azure-database-for-postgresql-server"></a>Optimización de las inserciones masivas y del uso de datos transitorios en un servidor de Azure Database for PostgreSQL 
En este artículo se describe cómo puede optimizar las operaciones de inserción masiva y el uso de datos transitorios en un servidor de Azure Database for PostgreSQL.

## <a name="using-unlogged-tables"></a>Uso de tablas sin registrar
Los clientes que tengan operaciones de carga de trabajo que impliquen datos transitorios o que inserten grandes conjuntos de datos de forma masiva deben considerar la posibilidad de usar tablas sin registrar.

Las tablas sin registrar son una característica de PostgreSQL que puede usarse de forma eficaz para optimizar las inserciones masivas. PostgreSQL usa el registro de escritura previa (WAL), que ofrece atomicidad y durabilidad, dos de las propiedades ACID, de forma predeterminada. Insertar en una tabla sin registrar significaría que PostgreSQL haría inserciones sin escribir en el registro de transacciones, que en sí es una operación de E/S, lo que hace que estas tablas sean considerablemente más rápidas que las tablas normales.

A continuación se muestran las opciones para crear una tabla sin registrar:
- Cree una tabla sin registrar con la sintaxis: `CREATE UNLOGGED TABLE <tableName>`
- Convierta una tabla registrada existente en una tabla sin registrar con la sintaxis: `ALTER <tableName> SET UNLOGGED`.  Esto se puede invertir mediante la sintaxis: `ALTER <tableName> SET LOGGED`

## <a name="unlogged-table-tradeoff"></a>Concesiones de las tablas sin registrar
Las tablas sin registrar no son seguras para bloqueos. Una tabla sin registrar se trunca automáticamente después de un bloqueo o está sujeta a un apagado no limpio. Además, el contenido de una tabla sin registrar no se replica en los servidores en espera. Los índices creados en una tabla sin registrar también se quitan del registro automáticamente.  Una vez que se completa la operación de inserción, puede convertir la tabla en registrada para que la inserción sea duradera.

Sin embargo, en algunas cargas de trabajo de clientes, hemos experimentado aproximadamente una mejora de rendimiento del 15 al 20 por ciento al usar tablas sin registrar.

## <a name="next-steps"></a>Pasos siguientes
Revise la carga de trabajo en busca de usos de datos transitorios e inserciones masivas de gran tamaño.  

Revise la siguiente documentación de PostgreSQL: [Create Table SQL Commands](https://www.postgresql.org/docs/current/static/sql-createtable.html)