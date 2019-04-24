---
title: Optimización de la recopilación de estadísticas de consulta en un servidor de Azure Database for PostgreSQL
description: En este artículo se describe cómo puede optimizar la recopilación de estadísticas de consulta en un servidor de Azure Database for PostgreSQL.
author: WenJason
ms.author: v-jay
ms.service: postgresql
ms.topic: conceptual
origin.date: 10/25/2018
ms.date: 02/18/2019
ms.openlocfilehash: 428a22f79008130448bef65f14322d6880b1b367
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60422717"
---
# <a name="optimize-query-statistics-collection-on-an-azure-database-for-postgresql-server"></a>Optimización de la recopilación de estadísticas de consulta en un servidor de Azure Database for PostgreSQL 
En este artículo se describe cómo optimizar la recopilación de estadísticas de consulta en un servidor de Azure Database for PostgreSQL.

## <a name="use-pgstatsstatements"></a>Uso de pg_stats_statements
**Pg_stat_statements** es una extensión de PostgreSQL habilitada de manera predeterminada en Azure Database for PostgreSQL. La extensión proporciona un medio para realizar el seguimiento de las estadísticas de ejecución para todas las instrucciones SQL ejecutadas por un servidor. Este módulo enlaza con cada ejecución de la consulta y genera un costo de rendimiento no trivial. Habilitar **pg_stat_statements** fuerza las escrituras de texto de consulta en archivos en disco.

Si tiene consultas únicas con texto de consulta largo o no supervisa activamente **pg_stat_statements**, deshabilite **pg_stat_statements** para obtener un mejor rendimiento. Para hacerlo, cambie la configuración a `pg_stat_statements.track = NONE`.

En algunas cargas de trabajo del cliente se ha visto una mejora del rendimiento de hasta un 50 por ciento al deshabilitar **pg_stat_statements**. La concesión que se hace al deshabilitar pg_stat_statements es la incapacidad de solucionar problemas de rendimiento.

Para establecer `pg_stat_statements.track = NONE`:

- En Azure Portal, vaya a la [página de administración de PostgreSQL y seleccione la hoja de parámetros del servidor](howto-configure-server-parameters-using-portal.md).

  ![Hoja de parámetros del servidor de PostgreSQL](./media/howto-optimize-query-stats-collection/pg_stats_statements_portal.png)

- Con la [CLI de Azure](howto-configure-server-parameters-using-cli.md), establezca la configuración de az postgres server en `--name pg_stat_statements.track --resource-group myresourcegroup --server mydemoserver --value NONE`.

## <a name="use-the-query-store"></a>Uso del Almacén de consultas 
La característica [Almacén de consultas](concepts-query-store.md) de Azure Database for PostgreSQL proporciona una manera más eficaz de realizar el seguimiento de las estadísticas de consulta. Se recomienda esta característica como alternativa al uso de *pg_stats_statements*. 

## <a name="next-steps"></a>Pasos siguientes
Considere la posibilidad de establecer `pg_stat_statements.track = NONE` en [Azure Portal](howto-configure-server-parameters-using-portal.md) o con la [CLI de Azure](howto-configure-server-parameters-using-cli.md).

Para más información, consulte: 
- [Escenarios de uso del Almacén de consultas](concepts-query-store-scenarios.md) 
- [Procedimientos recomendados sobre Almacén de consultas](concepts-query-store-best-practices.md) 
