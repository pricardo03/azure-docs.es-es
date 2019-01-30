---
title: Notas de la versión de Azure SQL Data Warehouse de diciembre de 2018 | Microsoft Docs
description: Notas de la versión de Azure SQL Data Warehouse.
services: sql-data-warehouse
author: twounder
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 12/12/2018
ms.author: mausher
ms.reviewer: twounder
ms.openlocfilehash: 074276e4550b9b2e347e5cd30c597a1d09f6cb2f
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2019
ms.locfileid: "54440131"
---
# <a name="whats-new-in-azure-sql-data-warehouse-version---100101060"></a>¿Cuáles son las novedades de Azure SQL Data Warehouse versión 10.0.10106.0?
Azure SQL Data Warehouse (SQL DW) está en continua mejora. En este artículo se describen las nuevas características y los cambios que se han incluido en SQL DW versión 10.0.10106.0.

## <a name="query-restartability---ctas-and-insertselect"></a>Reinicio de consultas: CTAS e instrucción Insert/Select
En contadas ocasiones (como, por ejemplo, problemas intermitentes de conexión de red o errores en los nodos), las consultas que se ejecutan en Azure SQL DW pueden producir error. Otras instrucciones de ejecución de larga duración, como las operaciones con [CREATE TABLE AS SELECT (CTAS)](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-develop-ctas) e INSERT-SELECT están más expuestas a este posible problema. En esta versión, Azure SQL DW implementa la lógica de reintentos para las instrucciones CTAS e INSERT-SELECT (además de para las instrucciones SELECT que se anunciaron anteriormente) lo cual permite al sistema administrar de forma transparente estos problemas transitorios y evitar que las consultas generen errores. El sistema configura el número de reintentos y la lista de errores transitorios administrados.

## <a name="return-order-by-optimization"></a>Optimización de la opción "Order By" para los resultados devueltos
Las consultas SELECT…ORDER BY experimentan una considerable mejora del rendimiento en esta versión.  Anteriormente, el motor de ejecución de consultas ordenaba los resultados en cada nodo de proceso y los transmitía al nodo de control que, a continuación, combinaba los resultados. Con esta mejora, todos los nodos de proceso enviaban los resultados a un único nodo de proceso que, a continuación, los combinaba y devolvía los resultados ordenados al usuario a través del nodo de proceso.  Esto ofrece una mejora significativa del rendimiento si el conjunto de resultados de la consulta contiene un número elevado de filas.

## <a name="data-movement-enhancements-for-partitionmove-and-broadcastmove"></a>Mejoras del movimiento de datos para PartitionMove y BroadcastMove
En Azure SQL Data Warehouse Gen2, los pasos de movimiento de datos del tipo ShuffleMove aprovechan las técnicas de movimiento de datos instantáneo que se describen en [este blog de mejoras de rendimiento](https://azure.microsoft.com/blog/lightning-fast-query-performance-with-azure-sql-data-warehouse/).  En esta versión, los tipos de movimiento de datos PartitionMove y BroadcastMove ahora también cuentan con las mismas técnicas de movimiento de datos instantáneo.  Las consultas de usuario que usan estos tipos de pasos de movimiento de datos experimentarán una mejora del rendimiento.  No se requiere ningún cambio en el código para aprovechar las ventajas de estas mejoras de rendimiento.

## <a name="next-steps"></a>Pasos siguientes
Ahora que ya conoce un poco SQL Data Warehouse, aprenda a [crear una instancia de SQL Data Warehouse][create a SQL Data Warehouse] rápidamente. Si no está familiarizado con Azure, el [glosario de Azure][Azure glossary] puede serte útil para aprender nueva terminología. O bien, examine algunos de estos otros recursos de SQL Data Warehouse.  

* [Casos de éxito de clientes]
* [Blogs]
* [Solicitud de función]
* [Vídeos]
* [Blogs de Customer Advisory Team]
* [Foro Stack Overflow]
* [Twitter]


[Blogs]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[Blogs de Customer Advisory Team]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Casos de éxito de clientes]: https://azure.microsoft.com/case-studies/?service=sql-data-warehouse
[Solicitud de función]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Foro Stack Overflow]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Vídeos]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
[create a SQL Data Warehouse]: ./create-data-warehouse-portal.md
[Azure glossary]: ../azure-glossary-cloud-terminology.md
