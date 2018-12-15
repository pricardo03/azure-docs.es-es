---
title: Hoja de referencia rápida del lenguaje de consulta SQL a Azure Log Analytics | Microsoft Docs
description: Funciones comunes que se utilizan en diferentes escenarios de consultas de Log Analytics.
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
ms.date: 08/21/2018
ms.author: bwren
ms.openlocfilehash: 35438644842d5280bd789efa135805ba9943cb8b
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2018
ms.locfileid: "53183054"
---
# <a name="sql-to-log-analytics-query-language-cheat-sheet"></a>Hoja de referencia rápida del lenguaje de consulta SQL a Azure Log Analytics 

En la tabla siguiente se ayuda a los usuarios que están familiarizados con SQL a aprender el lenguaje de consulta de Log Analytics. Eche un vistazo al comando T-SQL para resolver escenarios comunes y su equivalente con Log Analytics.

## <a name="sql-to-log-analytics"></a>SQL a Log Analytics

DESCRIPCIÓN                             |Consulta SQL                                                                                          |Consulta de Azure Log Analytics
----------------------------------------|---------------------------------------------------------------------------------------------------|----------------------------------------
Seleccionar todos los datos de una tabla            |`SELECT * FROM dependencies`                                                                       |<code>dependencies</code>
Selección de columnas específicas en una tabla    |`SELECT name, resultCode FROM dependencies`                                                        |<code>dependencies <br>&#124; project name, resultCode</code>
Seleccionar 100 registros de una tabla         |`SELECT TOP 100 * FROM dependencies`                                                               |<code>dependencies <br>&#124; take 100</code>
Evaluación de un valor nulo                         |`SELECT * FROM dependencies WHERE resultCode IS NOT NULL`                                          |<code>dependencies <br>&#124; where isnotnull(resultCode)</code>
Comparación de cadenas: igualdad             |`SELECT * FROM dependencies WHERE name = "abcde"`                                                  |<code>dependencies <br>&#124; where name == "abcde"</code>
Comparación de cadenas: subcadena            |`SELECT * FROM dependencies WHERE like "%bcd%"`                                                    |<code>dependencies <br>&#124; where name contains "bcd"</code>
Comparación de cadenas: carácter comodín             |`SELECT * FROM dependencies WHERE name like "abc%"`                                                |<code>dependencies <br>&#124; where name startswith "abc"</code>
Comparación de fechas: último día             |`SELECT * FROM dependencies WHERE timestamp > getdate()-1`                                         |<code>dependencies <br>&#124; where timestamp > ago(1d)</code>
Comparación de fechas: intervalo de fechas             |`SELECT * FROM dependencies WHERE timestamp BETWEEN '2016-10-01' AND '2016-11-01'`                 |<code>dependencies <br>&#124; where timestamp between (datetime(2016-10-01) .. datetime(2016-10-01))</code>
Comparación de valores booleanos                      |`SELECT * FROM dependencies WHERE !(success)`                                                      |<code>dependencies <br>&#124; where success == "False"</code>
Sort                                    |`SELECT name, timestamp FROM dependencies ORDER BY timestamp asc`                                  |<code>dependencies <br>&#124; order by timestamp asc</code>
Distinct                                |`SELECT DISTINCT name, type  FROM dependencies`                                                    |<code>dependencies <br>&#124; summarize by name, type</code>
Agrupación, agregación                   |`SELECT name, AVG(duration) FROM dependencies GROUP BY name`                                       |<code>dependencies <br>&#124; summarize avg(duration) by name</code>
Alias de columna, extensión                  |`SELECT operation_Name as Name, AVG(duration) as AvgD FROM dependencies GROUP BY name`             |<code>dependencies <br>&#124; summarize AvgD=avg(duration) by operation_Name <br>&#124; project Name=operation_Name, AvgD</code>
Primeros n registros por medida                |`SELECT TOP 100 name, COUNT(*) as Count FROM dependencies GROUP BY name ORDER BY Count asc`        |<code>dependencies <br>&#124; summarize Count=count() by name <br>&#124; top 100 by Count asc</code>
Unión                                   |`SELECT * FROM dependencies UNION SELECT * FROM exceptions`                                        |<code>union dependencies, exceptions</code>
Unión: con condiciones                  |`SELECT * FROM dependencies WHERE value > 4 UNION SELECT * FROM exceptions value < 5`              |<code>dependencies <br>&#124; where value > 4 <br>&#124; union (exceptions <br>&#124; where value < 5)</code>
Unión                                    |`SELECT * FROM dependencies JOIN exceptions ON dependencies.operation_Id = exceptions.operation_Id`|<code>dependencies <br>&#124; join (exceptions) on operation_Id == operation_Id</code>


## <a name="next-steps"></a>Pasos siguientes

- Repase una lección sobre [la escritura de consultas en Log Analytics](get-started-queries.md).