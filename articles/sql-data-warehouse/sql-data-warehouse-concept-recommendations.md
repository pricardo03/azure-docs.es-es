---
title: 'Recomendaciones sobre SQL Data Warehouse: conceptos | Microsoft Docs'
description: Obtenga información sobre las recomendaciones de SQL Data Warehouse y cómo se generan.
services: sql-data-warehouse
author: kevinvngo
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 11/05/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: b275f23209979e1a8068ecd99465f7b52392bc6c
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "58082915"
---
# <a name="sql-data-warehouse-recommendations"></a>Recomendaciones de SQL Data Warehouse

En este artículo se describen las recomendaciones de SQL Data Warehouse mediante Azure Advisor.  

SQL Data Warehouse proporciona recomendaciones para garantizar que el almacenamiento de datos está optimizado de forma coherente para el rendimiento. Las recomendaciones de Data Warehouse están totalmente integradas con [Azure Advisor](https://docs.microsoft.com/azure/advisor/advisor-performance-recommendations) para ofrecerle los procedimientos recomendados directamente en [Azure Portal](https://aka.ms/Azureadvisor). SQL Data Warehouse analiza el estado actual del almacenamiento de datos, recopila datos de telemetría y emite recomendaciones para una carga de trabajo activa con una cadencia diaria. Los escenarios de recomendaciones de Data Warehouse admitidos se describen a continuación junto con instrucciones sobre cómo aplicar las acciones recomendadas.

Si tiene algún comentario sobre el asesor de SQL Data Warehouse o experimenta problemas, póngase en contacto con [sqldwadvisor@service.microsoft.com](mailto:sqldwadvisor@service.microsoft.com).   

Haga clic [aquí](https://aka.ms/Azureadvisor) para consultar las recomendaciones de hoy. Actualmente esta característica solo es aplicable a almacenamientos de datos Gen2. 

## <a name="data-skew"></a>Asimetría de datos

La asimetría de datos puede provocar cuellos de botella de recursos o movimientos de datos adicionales al ejecutar una carga de trabajo. En la siguiente documentación se describe cómo identificar la asimetría de datos y evitar que suceda al seleccionar una clave de distribución óptima.

- [Identificación y eliminación de la asimetría](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-distribute#how-to-tell-if-your-distribution-column-is-a-good-choice) 

## <a name="no-or-outdated-statistics"></a>Ninguna estadística o estadísticas obsoletas

La existencia de estadísticas deficientes puede afectar gravemente al rendimiento de las consultas, ya que puede dar lugar a que el optimizador de consultas de SQL Data Warehouse genere planes de consulta deficientes. En la documentación siguiente se describen los procedimientos recomendados para crear y actualizar estadísticas:

- [Creación y actualización de estadísticas de tabla](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-statistics)

Para ver la lista de las tablas afectadas por estas recomendaciones, ejecute el [script de T-SQL](https://github.com/Microsoft/sql-data-warehouse-samples/blob/master/samples/sqlops/MonitoringScripts/ImpactedTables) siguiente. Advisor ejecuta continuamente el mismo script de T-SQL para generar estas recomendaciones.

## <a name="replicate-tables"></a>Replicación de tablas

Para obtener recomendaciones sobre tablas replicadas, Advisor detecta los candidatos de tabla en función de las siguientes características físicas:

- Tamaño de la tabla replicada
- Número de columnas
- Tipo de distribución de la tabla
- Número de particiones

Advisor aprovecha continuamente la heurística basada en la carga de trabajo, como la frecuencia de acceso a la tabla, las filas devueltas como promedio y los umbrales relativos al tamaño y la actividad del almacenamiento de datos para asegurarse de que se generaron recomendaciones de alta calidad. 

A continuación se describe la heurística basada en la carga de trabajo que puede encontrar en Azure Portal para cada recomendación de tabla replicada:

- Promedio de análisis: porcentaje promedio de filas devueltas de la tabla para cada acceso a la tabla durante los siete últimos días.
- Lectura frecuente, sin actualización: indica que la tabla no se actualizó durante los siete últimos días, mientras se muestra la actividad de acceso.
- Relación de lectura y actualización: la relación entre la frecuencia con que se accedió a la tabla y cuándo se actualizó durante los siete últimos días.
- Actividad: mide el uso basado en la actividad de acceso. Esto compara la actividad de acceso a la tabla en relación con la actividad de acceso promedio a la tabla en el almacenamiento de datos durante los siete últimos días. 

Actualmente Advisor solo mostrará como máximo cuatro candidatos de tabla replicada simultáneamente con índices de almacén de columnas agrupados, dando prioridad a la máxima actividad.

> [!IMPORTANT]
> La recomendación de tabla replicada no es una prueba completa y no tiene en cuenta las operaciones de movimiento de datos de las cuentas. Estamos trabajando para agregar esto como un método heurístico, pero mientras tanto, debe validar siempre la carga de trabajo después de aplicar la recomendación. Póngase en contacto con sqldwadvisor@service.microsoft.com si detecta recomendaciones de tabla replicada que causan la regresión de la carga de trabajo. Para más información sobre las tablas replicadas, visite la siguiente [documentación](https://docs.microsoft.com/azure/sql-data-warehouse/design-guidance-for-replicated-tables#what-is-a-replicated-table).
