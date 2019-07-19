---
title: Información de rendimiento de consultas en Azure Database for MariaDB
description: En este artículo se describe la característica Información de rendimiento de consultas en Azure Database for MariaDB.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 06/27/2019
ms.openlocfilehash: 48ff1fdc08e0df463ec48fd1415c7b67d5beb744
ms.sourcegitcommit: aa66898338a8f8c2eb7c952a8629e6d5c99d1468
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2019
ms.locfileid: "67462104"
---
# <a name="query-performance-insight-in-azure-database-for-mariadb"></a>Información de rendimiento de consultas en Azure Database for MariaDB

**Se aplica a:**  Azure Database for MariaDB 10.2

> [!NOTE]
> Información de rendimiento de consultas está en versión preliminar.

Query Performance Insight le ayuda a identificar rápidamente cuáles son las consultas que más tardan en ejecutarse, cómo cambian con el tiempo y qué esperas están afectándoles.

## <a name="common-scenarios"></a>Escenarios comunes

### <a name="long-running-queries"></a>Consultas de larga duración

- Identificar las consultas de ejecución más largas en las últimas X horas
- Identificar las N principales consultas que esperan recursos
 
### <a name="wait-statistics"></a>Estadísticas de espera

- Descripción de la naturaleza de la espera de una consulta
- Descripción de las tendencias de las esperas de recursos y dónde existe la contención de recursos

## <a name="permissions"></a>Permisos

Los permisos **Propietario** o **Colaborador** necesarios para ver el texto de las consultas en Información de rendimiento de consultas. El  **Lector** puede ver los gráficos y las tablas, pero no el texto de las consultas.

## <a name="prerequisites"></a>Requisitos previos

Para que Información de rendimiento de consultas funcione, debe haber datos en el  [Almacén de consultas](concepts-query-store.md).

## <a name="viewing-performance-insights"></a>Ver información de rendimiento

La vista [Query Performance Insight](concepts-query-performance-insight.md) en Azure Portal detectará visualizaciones en la información de clave del Almacén de consultas.

En la página del portal del servidor de Azure Database for MariaDB, seleccione **Información de rendimiento de consultas** en la sección **Rendimiento inteligente** de la barra de menús.

### <a name="long-running-queries"></a>Consultas de larga duración

La pestaña **Consultas de larga duración** muestra las 5 principales consultas por promedio de duración por ejecución, que se agregan en intervalos de 15 minutos. Para ver más consultas, seleccione en la lista desplegable **Número de consultas** . Al hacerlo, los colores del gráfico pueden cambiar a un identificador de consulta específico.

Puede hacer clic y arrastrar en el gráfico para restringir a un período de tiempo específico. Como alternativa, puede usar los iconos de acercar y alejar para ver un período de tiempo mayor o menor, respectivamente.

![Consultas de larga ejecución de Query Performance Insight](./media/concepts-query-performance-insight/query-performance-insight-landing-page.png)

### <a name="wait-statistics"></a>Estadísticas de espera 

> [!NOTE]
> Las estadísticas de espera están diseñadas para solucionar problemas con el rendimiento de una consulta. Se recomienda activarlas solo para solucionar problemas.

Las estadísticas de espera brindan una vista de los eventos de espera que se producen durante la ejecución de una consulta específica. Puede encontrar más información sobre los tipos de eventos de espera en la [documentación del motor de MySQL](https://go.microsoft.com/fwlink/?linkid=2098206).

Seleccione la pestaña  **Estadísticas de espera** para ver las visualizaciones correspondientes a esperas en el servidor.

Las consultas que se muestran en la vista de estadísticas de espera se agrupan según las consultas que presentan las mayores esperas durante el intervalo de tiempo especificado.

![Estadísticas de espera de Información de rendimiento de consultas](./media/concepts-query-performance-insight/query-performance-insight-wait-statistics.png)

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre la [supervisión y el ajuste](concepts-monitoring.md) en Azure Database for MariaDB.