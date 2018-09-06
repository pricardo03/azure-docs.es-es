---
title: 'Recomendaciones sobre SQL Data Warehouse: conceptos | Microsoft Docs'
description: Obtenga información sobre las recomendaciones de SQL Data Warehouse y cómo se generan.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 07/27/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 35ae3d1a8a6de2d348f90e2f55b732421b879917
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/30/2018
ms.locfileid: "43307644"
---
# <a name="sql-data-warehouse-recommendations"></a>Recomendaciones de SQL Data Warehouse

En este artículo se describen las recomendaciones de SQL Data Warehouse mediante Azure Advisor.  

SQL Data Warehouse proporciona recomendaciones para garantizar que el almacenamiento de datos está optimizado de forma coherente para el rendimiento. Las recomendaciones de Data Warehouse están totalmente integradas en [Azure Advisor](https://docs.microsoft.com/azure/advisor/advisor-performance-recommendations) para ofrecerle los mejores procedimiento recomendado directamente en [Azure Portal](https://aka.ms/Azureadvisor). SQL Data Warehouse analiza el estado actual del almacenamiento de datos, recopila datos de telemetría y emite recomendaciones para una carga de trabajo activa con una cadencia diaria. Los escenarios de recomendaciones de Data Warehouse admitidos se describen a continuación junto con instrucciones sobre cómo aplicar las acciones recomendadas.

Si tiene algún comentario sobre el asesor de SQL Data Warehouse o experimenta problemas, póngase en contacto con [sqldwadvisor@service.microsoft.com](mailto:sqldwadvisor@service.microsoft.com).   

Haga clic [aquí](https://aka.ms/Azureadvisor) para consultar las recomendaciones de hoy. Actualmente esta característica solo es aplicable a almacenamientos de datos Gen2. 

## <a name="data-skew"></a>Asimetría de datos

La asimetría de datos puede provocar cuellos de botella de recursos o movimientos de datos adicionales al ejecutar una carga de trabajo. En la siguiente documentación se describe cómo identificar la asimetría de datos y evitar que suceda al seleccionar una clave de distribución óptima.

- [Identificación y eliminación de la asimetría](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-distribute#how-to-tell-if-your-distribution-column-is-a-good-choice) 

## <a name="no-or-outdated-statistics"></a>Ninguna estadística o estadísticas obsoletas

La existencia de estadísticas deficientes puede afectar gravemente al rendimiento de las consultas, ya que puede dar lugar a que el optimizador de consultas de SQL Data Warehouse genere planes de consulta deficientes. En la documentación siguiente se describen los procedimientos recomendados para crear y actualizar estadísticas:

- [Creación y actualización de estadísticas de tabla](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-statistic)

Para estas dos recomendaciones, el asesor ejecuta continuamente el siguiente [script T-SQL](https://github.com/Microsoft/sql-data-warehouse-samples/blob/master/samples/sqlops/MonitoringScripts/ImpactedTables) para identificar tablas afectadas por recomendaciones de asimetría y estadísticas.
