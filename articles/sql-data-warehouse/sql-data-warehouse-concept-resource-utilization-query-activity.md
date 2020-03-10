---
title: 'Manejabilidad y supervisión: actividad de consultas y uso de recursos'
description: Obtenga información sobre las funcionalidades disponibles para administrar y supervisar Azure Synapse Analytics. Utilice Azure Portal y vistas de administración dinámica (DMV) para comprender la actividad de consultas y el uso de recursos del almacenamiento de datos.
services: sql-data-warehouse
author: kevinvngo
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 02/04/2020
ms.author: kevin
ms.reviewer: jrasnick
ms.custom: azure-synapse
ms.openlocfilehash: 47f142a19ac470fb29e9542941cd94a6b29ce240
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/29/2020
ms.locfileid: "78195930"
---
# <a name="monitoring-resource-utilization-and-query-activity-in-azure-synapse-analytics"></a>Supervisión del uso de recursos y la actividad de consultas en Azure Synapse Analytics
Azure Synapse Analytics proporciona una experiencia de supervisión enriquecida dentro de Azure Portal que expone la información a la carga de trabajo del almacenamiento de datos. Azure Portal es la herramienta recomendada al supervisar el almacenamiento de datos, ya que proporciona períodos de retención configurables, alertas, recomendaciones, y gráficos y paneles personalizables para métricas y registros. El portal también le permite integrarse con otros servicios de supervisión de Azure tales como Operations Management Suite (OMS) y Azure Monitor (registros) para ofrecer una experiencia de supervisión holística no solo para el almacenamiento de datos sino también para toda la plataforma Azure Analytics como experiencia de supervisión integrada. Esta documentación describe las funcionalidades de supervisión disponibles para optimizar y administrar la plataforma de análisis con SQL Analytics. 

## <a name="resource-utilization"></a>Utilización de recursos 
En Azure Portal para SQL Analytics hay disponibles las métricas siguientes. Estas métricas se exponen a través de [Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/data-collection#metrics).


| Nombre de la métrica             | Descripción                                                  | Tipo de agregación |
| ----------------------- | ------------------------------------------------------------ | ---------------- |
| Porcentaje de CPU          | Uso de CPU en todos los nodos del almacenamiento de datos      | Prom., Mín., Máx.    |
| Porcentaje de E/S de datos      | Uso de E/S en todos los nodos del almacenamiento de datos       | Prom., Mín., Máx.    |
| Porcentaje de memoria       | Uso de la memoria (SQL Server) en todos los nodos del almacenamiento de datos | Prom., Mín., Máx.   |
| Consultas activas          | Número de consultas activas que se ejecutan en el sistema             | Sum              |
| Consultas en cola          | Número de consultas en cola en espera para iniciar la ejecución          | Sum              |
| Conexiones correctas  | Número de conexiones correctas con los datos                 | Suma, Recuento       |
| Conexiones con errores      | Número de conexiones erróneas con el servidor           | Suma, Recuento       |
| Bloqueado por el firewall     | Número de inicios de sesión en el almacenamiento de datos que se ha bloqueado     | Suma, Recuento       |
| Límite de DWU               | Objetivo de nivel de servicio del almacenamiento de datos                | Prom., Mín., Máx.    |
| Porcentaje de DWU          | Máximo entre el porcentaje de CPU y porcentaje de E/S de datos        | Prom., Mín., Máx.    |
| DWU utilizada                | Límite de DWU * Porcentaje de DWU                                   | Prom., Mín., Máx.    |
| Porcentaje de aciertos de caché    | (aciertos de caché/error de caché) * 100 donde aciertos de caché corresponde al total de todos los aciertos de segmentos del almacén de columnas en la caché de SSD local y error de caché corresponde a la suma de los errores de segmentos del almacén de columnas en la caché SSD local de todos los nodos | Prom., Mín., Máx.    |
| Porcentaje de caché usada   | (caché usada/capacidad de la memoria caché) * 100 donde la memoria caché usada corresponde al total de todos los bytes en la caché SSD local en todos los nodos y capacidad de la memoria caché corresponde al total de la capacidad de almacenamiento de la caché SSD local en todos los nodos | Prom., Mín., Máx.    |
| Porcentaje de tempdb local | El uso de tempdb local en todos los nodos de ejecución. Los valores se emiten cada cinco minutos | Prom., Mín., Máx.    |

Aspectos que se deben tener en cuenta al visualizar métricas y establecer alertas:

- Las conexiones erróneas y correctas se indican para un almacenamiento de datos determinado, no para el servidor lógico.
- El porcentaje de memoria refleja el uso incluso si el almacenamiento de datos se encuentra en estado de inactividad (no refleja el consumo de memoria de carga de trabajo activa). Use y realice un seguimiento de esta métrica junto con otras (tempdb, memoria caché de Gen2) para tomar una decisión holística sobre si el escalado para conseguir capacidad de la memoria caché adicional aumentará el rendimiento de la carga de trabajo para cumplir sus requisitos.


## <a name="query-activity"></a>Actividad de consultas
Para brindar una experiencia de programación al supervisar SQL Analytics mediante T-SQL, el servicio proporciona un conjunto de vistas de administración dinámica (DMV). Estas vistas resultan útiles para la solución de problemas y la identificación activas de cuellos de botella de rendimiento con la carga de trabajo.

Para ver la lista de las DMV que proporciona SQL Analytics, vea esta [documentación](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-reference-tsql-system-views#sql-data-warehouse-dynamic-management-views-dmvs). 

## <a name="metrics-and-diagnostics-logging"></a>Métricas y registros de diagnóstico
Las métricas y los registros se pueden exportar a Azure Monitor, específicamente al componente de [registros de Azure Monitor](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview), y se puede acceder a ellos mediante programación a través de las [consultas de registros](https://docs.microsoft.com/azure/log-analytics/log-analytics-tutorial-viewdata). La latencia de registro de SQL Analytics es aproximadamente de entre 10 y 15 minutos. Para más detalles sobre los factores que afectan a la latencia, consulte la siguiente documentación.


## <a name="next-steps"></a>Pasos siguientes
Las siguientes guías de procedimientos describen escenarios comunes y casos de uso mientras se supervisa y administra el almacenamiento de datos:

- [Supervisión de la carga de trabajo mediante DMV](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-manage-monitor)
