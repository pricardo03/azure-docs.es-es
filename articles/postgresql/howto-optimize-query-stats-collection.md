---
title: Optimizar la recopilación de estadísticas de consulta en una base de datos de Azure Database for PostgreSQL - servidor único
description: Este artículo describe cómo puede optimizar la recopilación de estadísticas de consulta en una base de datos de Azure Database for PostgreSQL - servidor único
author: dianaputnam
ms.author: dianas
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 7425ee7916fd71625f336a7af35f6481d1ed2474
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65068952"
---
# <a name="optimize-query-statistics-collection-on-an-azure-database-for-postgresql---single-server"></a>Optimizar la recopilación de estadísticas de consultas en Azure Database for PostgreSQL: servidor único
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
