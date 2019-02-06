---
title: 'Manejabilidad y supervisión de Azure SQL Data Warehouse: actividad de consultas, uso de recursos | Microsoft Docs'
description: Obtenga información sobre las funcionalidades disponibles para administrar y supervisar Azure SQL Data Warehouse. Utilice Azure Portal y vistas de administración dinámica (DMV) para comprender la actividad de consultas y el uso de recursos del almacenamiento de datos.
services: sql-data-warehouse
author: kevinvngo
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 11/27/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 4613a16ee27168dd5c00435ee04fa5a7f95f4d97
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2019
ms.locfileid: "55460427"
---
# <a name="monitoring-resource-utilization-and-query-activity-in-azure-sql-data-warehouse"></a>Supervisión del uso de recursos y la actividad consultas en Azure SQL Data Warehouse
Azure SQL Data Warehouse ofrece una experiencia de supervisión enriquecida en Azure Portal que expone la información de la carga de trabajo del almacenamiento de datos. Azure Portal es la herramienta recomendada al supervisar el almacenamiento de datos, ya que proporciona períodos de retención configurables, alertas, recomendaciones, y gráficos y paneles personalizables para métricas y registros. El portal también le permite integrarse con otros servicios de supervisión de Azure tales como Operations Management Suite (OMS)/Log Analytics y Azure Monitor para ofrecer una experiencia de supervisión holística no solo para el almacenamiento de datos sino también para toda la plataforma Azure Analytics como experiencia de supervisión integrada. Esta documentación describe las funcionalidades de supervisión disponibles para optimizar y administrar la plataforma Analytics con SQL Data Warehouse. 

## <a name="resource-utilization"></a>Uso de recursos 
Las métricas siguientes están disponibles en Azure Portal para SQL Data Warehouse. Estas métricas se exponen a través de [Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/data-collection#metrics).

> [!NOTE]
> A partir de noviembre de 2018, el equipo de ingeniería está solucionando un problema que hace que el porcentaje de CPU y el porcentaje de E/S se informen muy por debajo del valor real. Esto hace que el porcentaje y el uso de DWU también se informen muy por debajo. 

| Nombre de métrica                           | DESCRIPCIÓN     | Tipo de agregación |
| --------------------------------------- | ---------------- | --------------------------------------- |
| Porcentaje de CPU                          | Uso de CPU en todos los nodos del almacenamiento de datos | Máxima      |
| Porcentaje de E/S de datos                      | Uso de E/S en todos los nodos del almacenamiento de datos | Máxima   |
| Conexiones correctas                  | Número de conexiones correctas con los datos | Total            |
| Conexiones con errores                      | Número de conexiones erróneas con el servidor | Total            |
| Bloqueado por el firewall                     | Número de inicios de sesión en el almacenamiento de datos que se ha bloqueado | Total            |
| Límite de DWU                              | Objetivo de nivel de servicio del almacenamiento de datos | Máxima   |
| Porcentaje de DWU                          | Máximo entre el porcentaje de CPU y porcentaje de E/S de datos | Máxima   |
| DWU utilizada                                | Límite de DWU * Porcentaje de DWU | Máxima   |
| Porcentaje de aciertos de caché | (aciertos de caché/error de caché) * 100 donde aciertos de caché corresponde al total de todos los aciertos de segmentos del almacén de columnas en la caché de SSD local y error de caché corresponde a la suma de los errores de segmentos del almacén de columnas en la caché SSD local de todos los nodos | Máxima |
| Porcentaje de caché usada | (caché usada/capacidad de la memoria caché) * 100 donde la memoria caché usada corresponde al total de todos los bytes en la caché SSD local en todos los nodos y capacidad de la memoria caché corresponde al total de la capacidad de almacenamiento de la caché SSD local en todos los nodos | Máxima |
| Porcentaje de tempdb local | El uso de tempdb local en todos los nodos de ejecución. Los valores se emiten cada cinco minutos | Máxima |

## <a name="query-activity"></a>Actividad de consultas
Para brindar una experiencia de programación al supervisar SQL Data Warehouse mediante T-SQL, el servicio proporciona un conjunto de vistas de administración dinámica (DMV). Estas vistas resultan útiles para la solución de problemas y la identificación activas de cuellos de botella de rendimiento con la carga de trabajo.

Para ver la lista de las DMV que ofrece SQL Data Warehouse, consulte esta [documentación](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-reference-tsql-system-views#sql-data-warehouse-dynamic-management-views-dmvs). 

## <a name="metrics-and-diagnostics-logging"></a>Métricas y registros de diagnóstico
Las métricas y los registros se pueden exportar a Azure Monitor, específicamente al componente [Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview), y se permtie el acceso mediante programación a través de [Búsqueda de registros](https://docs.microsoft.com/azure/log-analytics/log-analytics-tutorial-viewdata).


> [!NOTE]
> A partir de noviembre de 2018, los registros de SQL Data Warehouse se están implementando.

## <a name="next-steps"></a>Pasos siguientes
Las siguientes guías de procedimientos describen escenarios comunes y casos de uso mientras se supervisa y administra el almacenamiento de datos:

- [Supervisión de la carga de trabajo mediante DMV](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-manage-monitor)

