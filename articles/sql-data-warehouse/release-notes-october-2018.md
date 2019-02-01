---
title: Notas de la versión de Azure SQL Data Warehouse de octubre de 2018 | Microsoft Docs
description: Notas de la versión de Azure SQL Data Warehouse.
services: sql-data-warehouse
author: twounder
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 12/04/2018
ms.author: mausher
ms.reviewer: twounder
ms.openlocfilehash: 81096eeb1edcd6681bdc887d267ee477d78a8578
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2019
ms.locfileid: "55472089"
---
# <a name="whats-new-in-azure-sql-data-warehouse-october-2018"></a>¿Cuáles son las novedades de Azure SQL Data Warehouse? Octubre de 2018
Continuamente se aplican mejoras a Azure SQL Data Warehouse. En este artículo se describen las nuevas características y los cambios que se han incluido en octubre de 2018.

## <a name="devops-for-data-warehousing"></a>DevOps para almacenamiento de datos
La muy esperada característica para SQL Data Warehouse (SQL DW) ya está en versión preliminar con compatibilidad para SQL Server Data Tools (SSDT) en Visual Studio. Ahora los equipos de desarrolladores pueden colaborar mediante un único código base con control de versiones e implementar cambios rápidamente en cualquier instancia del mundo. ¿Le interesa participar? Esta característica está disponible hoy en versión preliminar. Para registrarse, vaya al [formulario de inscripción para la versión preliminar de SQL Server Data Tools (SSDT) para SQL Data Warehouse en Visual Studio](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR4-brmKy3TZOjoktwuHd7S1UODkwQ1lVMEw1NDBGRjNLRDNWOFlQRUpIRi4u). Dada la gran demanda, estamos administrando la aceptación para probar la versión preliminar a fin de garantizar la mejor experiencia para nuestros clientes. Una vez inscrito, nuestro objetivo es confirmar su estado en un plazo de siete días laborables.

## <a name="row-level-security-generally-available"></a>Disponibilidad general de seguridad de nivel de fila
Azure SQL Data Warehouse (SQL DW) ahora es compatible con la seguridad de nivel de fila (RLS), agregando así una funcionalidad eficaz para proteger su información confidencial. Con la introducción de RLS, puede implementar directivas de seguridad para controlar el acceso a las filas de las tablas, por ejemplo para determinar quién puede acceder a qué filas. RLS hace posible este control pormenorizado del acceso sin necesidad de rediseñar el almacén de datos. La seguridad de nivel de fila simplifica el modelo de seguridad global, porque la lógica de restricción del acceso se encuentra en el propio nivel de la base de datos y no en otra aplicación lejos de los datos. RLS elimina también la necesidad de incluir vistas para filtrar filas con el fin de administrar el control del acceso. Esta característica de seguridad de nivel empresarial para todos nuestros clientes no acarrea costos adicionales.

## <a name="advanced-advisors"></a>Asesores avanzados
La optimización avanzada de Azure SQL Data Warehouse (SQL DW) ahora es aún más sencilla gracias a nuevas métricas y recomendaciones de almacenamiento de datos. En Azure Advisor dispone de más recomendaciones avanzadas de rendimiento, como las siguientes:
1.  Caché adaptable: reciba un aviso cuando sea necesario escalar los recursos para optimizar el uso de la memoria caché.
2.  Distribución de tablas: determine cuándo deben replicarse tablas para reducir el movimiento de los datos y aumentar el rendimiento de la carga de trabajo. 
3.  Tempdb: comprenda cuándo debe escalar y configurar clases de recursos para reducir la contención de tempdb.

Hay una integración mayor entre las métricas de almacenamiento de datos y [Azure Monitor](https://azure.microsoft.com/blog/enhanced-capabilities-to-monitor-manage-and-integrate-sql-data-warehouse-in-the-azure-portal/), incluido un gráfico de supervisión personalizable mejorado para disponer de métricas casi en tiempo real en la hoja de información general. Ya no es necesario salir de la hoja de información general de almacenamiento de datos para acceder a las métricas de Azure Monitor cuando se está supervisando el uso o validando y aplicando recomendaciones de almacenamiento de datos. Además, hay nuevas métricas disponibles, como tempdb y el uso de caché adaptable, para complementar las recomendaciones de rendimiento.

## <a name="advanced-tuning-with-integrated-advisors"></a>Optimización avanzada con asesores integrados
La optimización avanzada de Azure SQL Data Warehouse (SQL DW) ahora es aún más sencilla gracias a nuevas métricas y recomendaciones de almacenamiento de datos y a un nuevo diseño de la hoja de información general del portal que proporciona una experiencia integrada con Azure Advisor y Azure Monitor.

## <a name="accelerated-database-recovery-adr"></a>Recuperación de base de datos acelerada (ADR)
La recuperación de base de datos acelerada (ADR) de Azure SQL Data Warehouse está ahora disponible en versión preliminar pública. ADR es un nuevo motor de SQL Server que mejora considerablemente la disponibilidad de la base de datos, en especial en presencia de transacciones de ejecución prolongada, al rediseñar por completo el proceso de recuperación actual desde cero. Las principales ventajas de ADR son una recuperación de base de datos rápida y coherente y una reversión de transacción instantánea.

## <a name="azure-monitor-diagnostics-logs"></a>Registros de diagnóstico de Azure Monitor
Ahora SQL Data Warehouse (SQL DW) ofrece conclusiones mejoradas de las cargas de trabajo analíticas porque se integra directamente con los registros de diagnóstico de Azure Monitor. Esta nueva funcionalidad permite a los desarrolladores analizar el comportamiento de las cargas de trabajo a lo largo de un extenso período de tiempo y tomar decisiones informadas para la optimización de las consultas o la administración de la capacidad. Ahora hemos introducido un proceso de registro externo mediante [registros de diagnóstico de Azure Monitor](https://docs.microsoft.com/azure/monitoring/monitoring-data-collection?toc=/azure/azure-monitor/toc.json#logs) que proporcionan información adicional sobre la carga de trabajo del almacén de datos. Con un solo clic, ahora es posible configurar registros de diagnóstico de las funcionalidades de solución de problemas de rendimiento histórico de consultas mediante [Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-queries). Los registros de diagnóstico de Azure Monitor admiten períodos de retención personalizables debido a que se guardan en una cuenta de almacenamiento con fines de auditoría, a la capacidad de transmitir los registros a centros de eventos para obtener conclusiones de telemetría casi en tiempo real y a la capacidad de analizar los registros en Log Analytics mediante consultas de registro. Los registros de diagnóstico constan de vistas de telemetría del almacenamiento de datos equivalentes a las DMV de solución de problemas de rendimiento más utilizadas para SQL Data Warehouse. En esta versión inicial, se han habilitado las siguientes vistas de administración dinámica de sistemas:

- [sys.dm_pdw_exec_requests](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql)
- [sys.dm_pdw_request_steps](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql)
- [sys.dm_pdw_dms_workers](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-dms-workers-transact-sql)
- [sys.dm_pdw_waits](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-waits-transact-sql)
- [sys.dm_pdw_sql_requests](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-sql-requests-transact-sql)

## <a name="columnstore-memory-management"></a>Administración de memoria del almacén de columnas
A medida que aumenta el número de grupos de filas del almacén de columnas comprimidos, aumenta la memoria necesaria para administrar los metadatos internos del segmento de columnas de esos grupos de filas.  Como resultado, el rendimiento de las consultas y las consultas ejecutadas en algunas de las vistas de administración dinámica (DMV) del almacén de columnas pueden verse degradados.  Se realizaron mejoras en esta versión para optimizar el tamaño de los metadatos internos en estos casos, lo que ha dado lugar a una experiencia y un rendimiento mejorados de dichas consultas. 

## <a name="azure-data-lake-storage-gen2-integration-ga"></a>Integración de Azure Data Lake Storage Gen2 (GA)
Azure SQL Data Warehouse (SQL DW) ahora tiene integración nativa con Azure Data Lake Storage Gen2. Los clientes ahora pueden cargar datos mediante tablas externas desde ABFS en SQL DW. Esta funcionalidad permite a los clientes realizar la integración con sus lagos de datos en Data Lake Storage Gen2. 

## <a name="bug-fixes"></a>Corrección de errores

| Título | DESCRIPCIÓN |
|:---|:---|
| **Errores de CETAS a Parquet en clases de recursos pequeñas en almacenes de datos de DW2000 y más** | Esta corrección identifica correctamente una referencia nula en la ruta de código de CREATE EXTERNAL TABLE AS SELECT a Parquet. |
|**El valor de la columna de identidad puede perderse en alguna operación de CTAS** | El valor de una columna de identidad puede no conservarse durante una operación de CTAS a otra tabla. Notificado en un blog: [https://blog.westmonroepartners.com/azure-sql-dw-identity-column-bugs/](https://blog.westmonroepartners.com/azure-sql-dw-identity-column-bugs/). |
| **Error interno en algunos casos, cuando se finaliza una sesión mientras todavía se está ejecutando una consulta** | Esta corrección desencadena una excepción InvalidOperationException si se finaliza una sesión cuando todavía se está ejecutando la consulta. |
| **(Implementado en noviembre de 2018) Los clientes experimentaban un rendimiento subóptimo al intentar cargar varios archivos pequeños desde ADLS (Gen1) con Polybase.** | El rendimiento del sistema experimentaba cuellos de botella durante la validación del token de seguridad de AAD. Los problemas de rendimiento se mitigaron al habilitar el almacenamiento en caché de los tokens de seguridad. |


## <a name="next-steps"></a>Pasos siguientes
Ahora que ya conoce un poco SQL Data Warehouse, aprenda a [crear una instancia de SQL Data Warehouse][create a SQL Data Warehouse] rápidamente. Si no está familiarizado con Azure, el [Glosario de Azure][Azure glossary] le puede resultar útil para consultar la nueva terminología que se encuentre. O bien, examine algunos de estos otros recursos de SQL Data Warehouse.  

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
