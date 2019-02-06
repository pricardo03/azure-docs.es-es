---
title: Notas de la versión de Azure SQL Data Warehouse de agosto de 2018 | Microsoft Docs
description: Notas de la versión de Azure SQL Data Warehouse.
services: sql-data-warehouse
author: twounder
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 08/13/2018
ms.author: twounder
ms.reviewer: twounder
ms.openlocfilehash: 492bdd30a656a37196cf4d27a2510dbc3a79807d
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2019
ms.locfileid: "55463572"
---
# <a name="whats-new-in-azure-sql-data-warehouse-august-2018"></a>¿Cuáles son las novedades de Azure SQL Data Warehouse? Agosto de 2018
Continuamente se aplican mejoras a Azure SQL Data Warehouse. En este artículo se describen las nuevas características y los cambios que se han incluido en agosto de 2018.

## <a name="automatic-intelligent-insights"></a>Funcionalidad Intelligent Insights automática
Microsoft ha introducido la [funcionalidad Intelligent Insights automática](https://azure.microsoft.com/blog/automatic-intelligent-insights-to-optimize-performance-with-sql-data-warehouse/) para llevar a la nube la promesa de automatización del almacenamiento de datos. Ya no es necesario supervisar el almacenamiento de datos para ver la asimetría de datos y las estadísticas de tabla por debajo de los valores óptimos. Sin costo adicional, SQL Data Warehouse expone información detallada inteligente para todas las instancias de Gen2. Gracias a la integración con [Azure Advisor](https://docs.microsoft.com/azure/advisor/advisor-performance-recommendations), puede recibir automáticamente recomendaciones de procedimientos para mejorar el rendimiento de las cargas de trabajo activas. SQL Data Warehouse analiza las recomendaciones de carga de trabajo y superficies en función del uso. Este análisis tiene lugar diariamente, lo que le permite supervisar los informes y las recomendaciones de uso para mejorar la carga de trabajo.

Puede ver las recomendaciones en el portal de Azure Advisor: ![Recomendaciones del Portal de Azure Advisor para Azure SQL Data Warehouse](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/4e205b6d-df04-48db-8eec-d591f2592cf4.png)

Puede profundizar en cada categoría para ver las recomendaciones para la alerta específica: ![Detalles de recomendaciones del Portal de Azure Advisor para Azure SQL Data Warehouse](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/3c42426e-6969-46e3-9025-c34c0755a302.png)


## <a name="bug-fixes"></a>Corrección de errores

| Título | DESCRIPCIÓN |
|:---|:---|
| **Potential Query failures when the split count exceeds max limit (Posibles errores de consulta cuando el recuento dividido supera el límite máximo)** |Cuando se supera el límite de división del archivo superior de 1 millón, una excepción no atendida provocó el volcado del motor de SQL y el error de todas las consultas. Esta corrección abordó el problema al controlar la excepción correctamente y devolver un error sin provocar el error en las consultas. |
| **Increased ExternalMoveReadersPerNode default value to improve load perfomance** (Valor predeterminado ExternalMoveReadersPerNode aumentado para mejorar el rendimiento de carga) |Este problema se debía a que la configuración de la propiedad ExternalMoveReadersPerNode no estaba sincronizada con la configuración de Service Fabric. Esta regresión provocaba una disminución del rendimiento de carga de Gen2. La corrección devuelve el rendimiento de carga de Gen2 a los parámetros de diseño optimizados.|


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
