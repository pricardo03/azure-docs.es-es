---
title: Optimización de la recopilación de estadísticas de consulta en servidor de Azure Database for PostgreSQL
description: En este artículo se describe cómo puede optimizar la recopilación de estadísticas de consulta en un servidor de Azure Database for PostgreSQL.
author: dianaputnam
ms.author: dianas
editor: jasonwhowell
ms.service: postgresql
ms.topic: conceptual
ms.date: 10/25/2018
ms.openlocfilehash: 076442d85d7f628504cca95c36f3e99f4d0c5117
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/06/2018
ms.locfileid: "52966694"
---
# <a name="optimize-query-statistics-collection-in-azure-database-for-postgresql-server"></a>Optimización de la recopilación de estadísticas de consulta en un servidor de Azure Database for PostgreSQL 
En este artículo se describe cómo optimizar la recopilación de estadísticas de consulta en un servidor de Azure Database for PostgreSQL.

## <a name="using-pgstatsstatements"></a>Uso de pg_stats_statements
**Pg_stat_statements** es una extensión de PostgreSQL habilitada de manera predeterminada en Azure Database for PostgreSQL. La extensión proporciona un medio para realizar el seguimiento de las estadísticas de ejecución de todas las instrucciones SQL ejecutadas por un servidor. Sin embargo, este módulo enlaza con cada ejecución de la consulta y genera un costo de rendimiento no trivial. Habilitar **pg_stat_statements** fuerza las escrituras de texto de consulta en archivos en disco.

Para los clientes que tienen consultas únicas con texto de consulta largo o que no supervisan **pg_stat_statements** de forma activa, se recomienda deshabilitar **pg_stat_statements** para un mejor rendimiento al establecer `pg_stat_statements.track = NONE`.

En algunas cargas de trabajo del cliente, hemos visto una mejora del rendimiento de hasta un 50 por ciento al deshabilitar **pg_stat_statements**. Sin embargo, la concesión que uno hace al deshabilitar pg_stat_statements es la incapacidad de solucionar problemas de rendimiento.

Para establecer `pg_stat_statements.track = NONE`:

- En Azure Portal, vaya a la [página de administración de PostgreSQL y seleccione la hoja de parámetros del servidor](howto-configure-server-parameters-using-portal.md).

![Hoja de parámetros del servidor de PostgreSQL](./media/howto-optimize-query-stats-collection/pg_stats_statements_portal.png)

- Con la [CLI de Azure](howto-configure-server-parameters-using-cli.md), establezca az postgres server configuration set `--name pg_stat_statements.track --resource-group myresourcegroup --server mydemoserver --value NONE`.

## <a name="using-query-store"></a>Uso del Almacén de consultas 
La característica [Almacén de consultas](concepts-query-store.md) de Azure Database for PostgreSQL proporciona un método de mejor rendimiento para realizar un seguimiento de las estadísticas de consulta, y se recomienda como alternativa al uso de *pg_stats_statements*. 

## <a name="next-steps"></a>Pasos siguientes
Considere la posibilidad de establecer `pg_stat_statements.track = NONE` en [Azure Portal](howto-configure-server-parameters-using-portal.md) o con la [CLI de Azure](howto-configure-server-parameters-using-cli.md).

Para más información, consulte [Escenarios de uso de Almacén de consultas](concepts-query-store-scenarios.md) y [Procedimientos recomendados para usar Almacén de consultas](concepts-query-store-best-practices.md). 
