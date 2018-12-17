---
title: Lenguaje de consulta de Log Analytics en Azure Monitor | Microsoft Docs
description: Referencias a recursos para aprender a escribir consultas en Log Analytics.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 10/29/2018
ms.author: bwren
ms.openlocfilehash: 32e64ce7772d562ea34a0d74afbd737be27d247d
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/06/2018
ms.locfileid: "52968875"
---
# <a name="log-analytics-query-language"></a>Lenguaje de consulta de Log Analytics
Log Analytics proporciona análisis y recopilación de registros de Azure Monitor. Se basa en Azure Data Explorer y usa una versión del mismo lenguaje de consulta. La [documentación del lenguaje de consulta de Azure Data Explorer](/azure/kusto/query) incluye todos los detalles del lenguaje y debe ser el principal recurso para escribir consultas de Log Analytics. Esta página proporciona vínculos a otros recursos para aprender a escribir consultas y conocer las diferencias con la implementación de Log Analytics del lenguaje.

## <a name="getting-started"></a>Introducción

- [Introducción a Log Analytics en Azure Portal](get-started-portal.md) es una lección sobre cómo escribir consultas y trabajar con los resultados en Azure Portal.
-  [Introducción a Log Analytics en Azure Portal](get-started-queries.md) es una lección sobre cómo escribir consultas con datos de Log Analytics.

## <a name="concepts"></a>Conceptos
- [Análisis de los datos de Log Analytics en Azure Monitor](../../azure-monitor/log-query/log-query-overview.md) ofrece una breve introducción a las consultas de registro y describe cómo se estructuran los datos de Log Analytics.
- [Visualización y análisis de datos en Azure Log Analytics](../../azure-monitor/log-query/portals.md) se explican los portales donde se crean y ejecutan las consultas de Log Analytics.

## <a name="reference"></a>Referencia

- [Referencia del lenguaje de consulta](/azure/kusto/query) es la referencia completa del lenguaje de consulta de Data Explorer.
- [Diferencias del lenguaje de consulta de Log Analytics](data-explorer-difference.md) describe las diferencias entre las versiones del lenguaje de consulta de Data Explorer.
- [Propiedades estándar de los registros de Log Analytics](../../azure-monitor/platform/log-standard-properties.md) describe las propiedades que son estándar para todos los datos de Log Analytics.
- [Realización de búsquedas de registros entre recursos en Log Analytics](../../azure-monitor/log-query/cross-workspace-query.md) describe cómo escribir consultas que usan datos procedentes de varias áreas de trabajo de Log Analytics y aplicaciones de Application Insights.


## <a name="examples"></a>Ejemplos

- [Ejemplos de consultas de Log Analytics](examples.md) ofrece ejemplos de consultas con datos de Log Analytics.



## <a name="lessons"></a>Lecciones

- [Trabajar con cadenas en las consultas de Log Analytics](string-operations.md) describe cómo trabajar con datos de cadena.
- [Trabajar con valores de fecha y hora en consultas de Log Analytics](datetime-operations.md) describe cómo trabajar con datos de fecha y hora. 
- [Agregaciones en consultas de Log Analytics](aggregations.md) y [Agregaciones avanzadas en consultas de Log Analytics](advanced-aggregations.md) describen cómo agregar y resumir datos.
- [Combinaciones en consultas de Log Analytics](joins.md) describe cómo combinar datos de varias tablas.
- [Trabajo con JSON y estructuras de datos en consultas de Log Analytics](json-data-structures.md) describe cómo analizar datos JSON.
- [Escribir consultas avanzadas en Log Analytics](advanced-query-writing.md) describe las estrategias para crear consultas complejas y reutilizar código.
- [Creación de gráficos y diagramas de las consultas de Log Analytics](charts.md) describe cómo visualizar los datos de una consulta.

## <a name="cheatsheets"></a>Hojas de características clave

-  [Hoja de referencia rápida del lenguaje de consulta SQL a Azure Log Analytics](sql-cheatsheet.md) ayuda a los usuarios que ya están familiarizados con SQL.
-  [Hoja de referencia rápida del lenguaje de consulta Splunk en Log Analytics ](sql-cheatsheet.md) ayuda a los usuarios que ya están familiarizados con Splunk.
 
## <a name="next-steps"></a>Pasos siguientes

- Obtenga acceso a toda la [documentación de referencia del lenguaje de consulta de Data Explorer](/azure/kusto/query/).