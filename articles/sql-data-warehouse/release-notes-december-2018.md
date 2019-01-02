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
ms.openlocfilehash: 21baa89293c74ec49720bffc2506e20789fe9e55
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2018
ms.locfileid: "53411384"
---
# <a name="whats-new-in-azure-sql-data-warehouse-december-2018"></a>¿Cuáles son las novedades de Azure SQL Data Warehouse? Diciembre de 2018
Continuamente se aplican mejoras a Azure SQL Data Warehouse. En este artículo se describen las nuevas características y los cambios que se han incluido en diciembre de 2018.

## <a name="virtual-network-service-endpoints-generally-available"></a>Puntos de conexión de servicio de red virtual disponibles con carácter general
Esta versión incluye la disponibilidad general de los puntos de conexión de servicio de red virtual (VNet) para Azure SQL Data Warehouse en todas las regiones de Azure. Los puntos de conexión de servicio de red virtual le permiten aislar la conectividad con el servidor lógico desde una subred o un conjunto de subredes concretos dentro de la red virtual. El tráfico hacia Azure SQL Data Warehouse desde su red virtual siempre permanecerá dentro de la red troncal de Azure. Esta ruta directa será preferible a cualquier ruta específica que tome tráfico de Internet a través de dispositivos virtuales o locales. No se realiza ningún cargo adicional por el acceso a la red virtual a través de los puntos de conexión de servicio. El modelo de precios vigente para [Azure SQL Data Warehouse](https://azure.microsoft.com/pricing/details/sql-data-warehouse/gen2/) se aplica tal cual.

Con esta versión, también habilitamos la conectividad de PolyBase a [Azure Data Lake Storage Gen2](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-introduction) (ADLS) a través del controlador del [Sistema de archivos de blobs de Azure](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-abfs-driver). Azure Data Lake Storage Gen2 incluye todas las cualidades necesarias para el ciclo de vida completo de los datos de análisis en Azure Storage. Las características de los dos servicios de almacenamiento de Azure existentes, Azure Blob Storage y Azure Data Lake Storage Gen1, convergen. Las características de [Azure Data Lake Storage Gen1](https://docs.microsoft.com/azure/data-lake-store/index), como la semántica del sistema de archivos, la seguridad de nivel de archivo y la escala se combinan con las funcionalidades de recuperación ante desastres o alta disponibilidad, y de almacenamiento por niveles de bajo costo de [Azure Blob Storage](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction). 

Con PolyBase, también puede importar datos a Azure SQL Data Warehouse desde Azure Storage protegido a la red virtual. De igual forma, también se admite la exportación de datos desde Azure SQL Data Warehouse a Azure Storage protegido a la red virtual a través de Polybase. 

Para más información sobre los puntos de conexión de servicio de red virtual en Azure SQL Data Warehouse, consulte la [entrada de blog](https://azure.microsoft.com/blog/general-availability-of-vnet-service-endpoints-for-azure-sql-data-warehouse/) o la [documentación](https://docs.microsoft.com/azure/sql-database/sql-database-vnet-service-endpoint-rule-overview?toc=/azure/sql-data-warehouse/toc.json).

## <a name="automatic-performance-monitoring-preview"></a>Supervisión de rendimiento automática (versión preliminar)
[Almacén de consultas](https://docs.microsoft.com/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store?view=sql-server-2017) ahora está disponible en versión preliminar para Azure SQL Data Warehouse. Almacén de consultas está diseñado para ayudarle a solucionar problemas de rendimiento de las consultas mediante el seguimiento de consultas, planes de consulta, estadísticas de tiempo de ejecución e historial de consultas para ayudarle a supervisar la actividad y el rendimiento de su almacén de datos. Almacén de consultas es un conjunto de almacenes internos y de vistas de administración dinámica (DMV) que le permiten lo siguiente:

- Identificación y optimización de las consultas que consumen más recursos
- Identificación y mejora de las cargas de trabajo puntuales
- Evaluación del rendimiento de las consultas y del impacto en el plan mediante cambios en las estadísticas, índices o tamaño del sistema (configuración de DWU)
- Visualización del texto de consulta completo para todas las consultas ejecutadas

Almacén de consultas contiene tres almacenes reales:  
- Un almacén de planes para conservar la información del plan de ejecución 
- Un almacén de estadísticas de ejecución para conservar la información de estadísticas de ejecución
- Un almacén de estadísticas de espera para conservar la información de estadísticas de espera. 

SQL Data Warehouse administra estos almacenes de forma automática y proporciona un número ilimitado de consultas almacenadas en los últimos siete días sin costo adicional. Habilitar Almacén de consultas es tan sencillo como ejecutar la instrucción ALTER DATABASE de T-SQL:

```sql
ALTER DATABASE [Database Name] SET QUERY_STORE = ON;
```
Para más información sobre Almacén de consultas en Azure SQL Data Warehouse, consulte el artículo [Supervisión del rendimiento mediante el Almacén de consultas](https://docs.microsoft.com/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store?view=sql-server-2017) y las DMV de Almacén de consultas, como [sys.query_store_query](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-query-store-query-transact-sql?view=sql-server-2017). Esta es la [entrada de blog](https://azure.microsoft.com/blog/automatic-performance-monitoring-in-azure-sql-data-warehouse-with-query-store/) donde se comunica la versión.

## <a name="lower-compute-tiers-for-azure-sql-data-warehouse-gen2"></a>Reducción de los niveles de proceso para Azure SQL Data Warehouse Gen2
Azure SQL Data Warehouse Gen2 ahora admite menores niveles de proceso. Los clientes pueden experimentar las principales características de rendimiento, flexibilidad y seguridad de Azure SQL Data Warehouse desde 100 cDWU ([unidades de almacenamiento de datos](https://docs.microsoft.com/azure/sql-data-warehouse/what-is-a-data-warehouse-unit-dwu-cdwu)) y escalar a 30 000 cDWU en cuestión de minutos. A partir de mediados de diciembre de 2018, los clientes pueden beneficiarse del rendimiento y la flexibilidad de Gen2 con niveles de proceso más bajos en [regiones](https://docs.microsoft.com/azure/sql-data-warehouse/gen2-lower-tier-regions), mientras que el resto de las regiones estarán disponibles durante 2019.

Al eliminar el punto de entrada para el almacenamiento de datos de próxima generación, Microsoft abre las puertas a los clientes que desean evaluar todos los beneficios de un almacenamiento de datos seguro y de alto rendimiento sin tener que adivinar cuál es el mejor entorno de pruebas para ellos. Los clientes pueden comenzar desde 100 cDWU, por debajo del punto de entrada actual de 500 cDWU. SQL Data Warehouse Gen2 sigue admitiendo las operaciones de pausa y reanudación y va más allá de la flexibilidad en el proceso. Gen2 también admite una capacidad de almacenamiento ilimitada en almacenamiento en columnas junto con 2,5 veces más memoria por consulta, hasta 128 consultas simultáneas y características de [almacenamiento en caché adaptativo](https://azure.microsoft.com/blog/adaptive-caching-powers-azure-sql-data-warehouse-performance-gains/). Estas características, como promedio, proporcionan cinco veces más rendimiento en comparación con la misma unidad de almacenamiento de datos en Gen1 al mismo precio. Las copias de seguridad con redundancia geográfica son estándar para Gen2 con protección de datos garantizada integrada. Azure SQL Data Warehouse Gen2 está listo para escalar cuando lo esté el usuario.

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
