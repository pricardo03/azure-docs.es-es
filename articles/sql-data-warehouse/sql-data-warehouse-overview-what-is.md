---
title: ¿Qué es Azure SQL Data Warehouse? | Microsoft Docs
description: Base de datos distribuida de clase empresarial que puede procesar volúmenes de petabytes de datos relacionales y no relacionales. Es el primer almacenamiento de datos en la nube de la industria con la capacidad de aumentarse, reducirse y pausarse en cuestión de segundos.
services: sql-data-warehouse
author: mlee3gsd
manager: craigg
ms.service: sql-data-warehouse
ms.topic: overview
ms.subservice: design
ms.date: 05/30/2019
ms.author: martinle
ms.reviewer: igorstan
mscustom: sqlfreshmay19
ms.openlocfilehash: a9126e9023091dd8c3df71f2aa2558a01227a8be
ms.sourcegitcommit: ef06b169f96297396fc24d97ac4223cabcf9ac33
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66428036"
---
# <a name="what-is-azure-sql-data-warehouse"></a>¿Qué es Azure SQL Data Warehouse?

SQL Data Warehouse es un almacenamiento de datos empresarial (EDW) basado en la nube que emplea el procesamiento paralelo masivo (MPP) para ejecutar rápidamente consultas complejas en petabytes de datos. SQL Data Warehouse se usa como un componente clave de una solución de macrodatos. Importe macrodatos en SQL Data Warehouse con consultas T-SQL de [PolyBase](/sql/relational-databases/polybase/polybase-guide?view=sql-server-2017&viewFallbackFrom=azure-sqldw-latest) simples y, después, use la potencia de MPP para realizar análisis de alto rendimiento. Al realizar la integración y el análisis, el almacenamiento de datos pasará a ser la versión única de certeza con la que puede contar su empresa para obtener información.  

## <a name="key-component-of-big-data-solution"></a>Componente clave de la solución de macrodatos

SQL Data Warehouse es un componente clave de una solución de macrodatos de un extremo a otro en la nube.

![Solución de almacenamiento de datos](media/sql-data-warehouse-overview-what-is/data-warehouse-solution.png) 

En una solución de datos en la nube, los datos provenientes de varios orígenes se ingieren en almacenes de macrodatos. Una vez que están en un almacén de macrodatos, Hadoop, y Spark y los algoritmos de aprendizaje automático preparan y entrenan los datos. Cuando los datos están listos para la realización de un análisis complejo, SQL Data Warehouse usa PolyBase para consultar los almacenes de macrodatos. PolyBase usa las consultas T-SQL estándar para llevar los datos a SQL Data Warehouse.
 
SQL Data Warehouse almacena los datos en tablas relacionales con almacenamiento en columnas. Este formato reduce considerablemente los costos de almacenamiento de datos y mejora el rendimiento de las consultas. Una vez que los datos están almacenados en SQL Data Warehouse, se pueden realizar análisis a gran escala. En comparación con los sistemas de bases de datos tradicionales, las consultas de análisis finalizan en segundos, en lugar de minutos, o en horas, en lugar de días. 

Los resultados del análisis pueden ir a aplicaciones o bases de datos de informes mundiales. En ellas, los analistas empresariales pueden obtener información útil para tomar decisiones empresariales bien informadas.

## <a name="next-steps"></a>Pasos siguientes

- Explore [Arquitectura de Azure SQL Data Warehouse](/azure/sql-data-warehouse/massively-parallel-processing-mpp-architecture).
- [Cree rápidamente una instancia de SQL Data Warehouse][create a SQL Data Warehouse].
- [Cargue datos de ejemplo][load sample data].
- Explore [vídeos](/azure/sql-data-warehouse/sql-data-warehouse-videos).

O bien, examine algunos de estos otros recursos de SQL Data Warehouse.  
* Busque información en [Blogs].
* Envíe una [Solicitud de función].
* Busque en [blogs de Customer Advisory Team].
* [Creación de una incidencia de soporte técnico]
* Busque en el [foro de MSDN].
* Busque en el [foro Stack Overflow].


<!--Image references-->
[1]: ./media/sql-data-warehouse-overview-what-is/dwarchitecture.png

<!--Article references-->
[Creación de una incidencia de soporte técnico]: ./sql-data-warehouse-get-started-create-support-ticket.md
[load sample data]: ./sql-data-warehouse-load-sample-databases.md
[create a SQL Data Warehouse]: ./sql-data-warehouse-get-started-provision.md
[Migration documentation]: ./sql-data-warehouse-overview-migrate.md
[SQL Data Warehouse solution partners]: ./sql-data-warehouse-partner-business-intelligence.md
[Integrated tools overview]: ./sql-data-warehouse-overview-integrate.md
[Backup and restore overview]: ./sql-data-warehouse-restore-database-overview.md
[Azure glossary]: ../azure-glossary-cloud-terminology.md

<!--MSDN references-->

<!--Other Web references-->
[Blogs]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[Blogs de Customer Advisory Team]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Solicitud de función]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Foro de MSDN]: https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=AzureSQLDataWarehouse
[Foro Stack Overflow]: https://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Videos]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
[SLA for SQL Data Warehouse]: https://azure.microsoft.com/support/legal/sla/sql-data-warehouse/v1_0/
[Volume Licensing]: https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=37
[Service Level Agreements]: https://azure.microsoft.com/support/legal/sla/
