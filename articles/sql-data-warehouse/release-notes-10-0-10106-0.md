---
title: Notas de la versión de Azure SQL Data Warehouse | Microsoft Docs
description: Notas de la versión de Azure SQL Data Warehouse.
services: sql-data-warehouse
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 02/09/2019
author: mlee3gsd
ms.author: anumjs
ms.reviewer: jrasnick
manager: craigg
ms.openlocfilehash: 51932ebf7d5bdc6830098ce7136a3eee7255ffe1
ms.sourcegitcommit: b3d74ce0a4acea922eadd96abfb7710ae79356e0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/14/2019
ms.locfileid: "56245514"
---
# <a name="azure-sql-data-warehouse-release-notes"></a>Notas de la versión de Azure SQL Data Warehouse
En este artículo se resumen las nuevas características y mejoras de las versiones recientes de [SQL Server en VM de Azure](sql-data-warehouse-overview-what-is.md). En el artículo también se enumeran las actualizaciones importantes de contenido que no están directamente relacionadas con la versión, pero que se publican en el mismo período de tiempo. Para mejoras en otros servicios de Azure, consulte [las Actualizaciones del servicio](https://azure.microsoft.com/updates).

## <a name="sql-data-warehouse-version-100101060-january"></a>Versión de SQL Data Warehouse 10.0.10106.0 (enero)

### <a name="service-improvements"></a>Mejoras en el servicio

| Mejoras en el servicio | Detalles |
| --- | --- |
| **Reinicio de consultas: CTAS e instrucción Insert/Select** | En contadas ocasiones (como, por ejemplo, problemas intermitentes de conexión de red o errores en los nodos), las consultas que se ejecutan en Azure SQL DW pueden producir error. Otras instrucciones de ejecución de larga duración, como las operaciones con [CREATE TABLE AS SELECT (CTAS)](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-develop-ctas) e INSERT-SELECT están más expuestas a este posible problema. Con esta versión, Azure SQL DW implementa lógica de reintento para las CTAS e instrucciones Insert/Select, además de las instrucciones SELECT anunciadas anteriormente. Los cambios permiten que el servicio controle los problemas transitorios de forma transparente y evitan las consultas con errores. El sistema configura el número de reintentos y la lista de errores transitorios administrados.|
|**Optimización de la opción "Order By" para los resultados devueltos**|Las consultas SELECT…ORDER BY experimentan una considerable mejora del rendimiento en esta versión.   Ahora, todos los nodos de ejecución envían sus resultados a un único nodo de ejecución, que combina y ordena los resultados, los cuales se devuelven al usuario a través del nodo de ejecución.  La combinación a través de un único nodo de ejecución da lugar a una mejora significativa del rendimiento si el conjunto de resultados de la consulta contiene un número elevado de filas. Anteriormente, el motor de ejecución de consultas ordenaba los resultados en cada nodo de proceso y los transmitía al nodo de control que, a continuación, combinaba los resultados.|
|**Mejoras del movimiento de datos para PartitionMove y BroadcastMove**|En Azure SQL Data Warehouse Gen2, los pasos de movimiento de datos del tipo ShuffleMove usan las técnicas de movimiento de datos instantáneo que se describen en el [blog de mejoras de rendimiento](https://azure.microsoft.com/blog/lightning-fast-query-performance-with-azure-sql-data-warehouse/). En esta versión, los tipos de movimiento de datos PartitionMove y BroadcastMove ahora también cuentan con las mismas técnicas de movimiento de datos instantáneo. Las consultas de usuario que usan estos tipos de pasos de movimiento de datos se ejecutarán con una mejora del rendimiento. No se requiere ningún cambio en el código para aprovechar las ventajas de estas mejoras de rendimiento.|

### <a name="documentation-improvements"></a>Mejoras en la documentación

| Mejoras en la documentación | Detalles |
| --- | --- |
|None | |
| | |

## <a name="next-steps"></a>Pasos siguientes
[Creación de una instancia de SQL Data Warehouse](./create-data-warehouse-portal.md)

## <a name="more-information"></a>Más información
- [Blog: Azure SQL Data Warehouse](https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/)
- [Blogs de Customer Advisory Team](https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/)
- [Casos de éxito de clientes](https://azure.microsoft.com/case-studies/?service=sql-data-warehouse)
- [Foro Stack Overflow](http://stackoverflow.com/questions/tagged/azure-sqldw)
- [Twitter](https://twitter.com/hashtag/SQLDW)
- [Vídeos](https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse)
- [Glosario de Azure](../azure-glossary-cloud-terminology.md)