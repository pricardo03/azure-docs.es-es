---
title: Descripción de la supervisión de trabajos en Azure Stream Analytics
description: En este artículo se describe cómo supervisar los trabajos de Azure Stream Analytics en Azure Portal.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/07/2018
ms.custom: seodec18
ms.openlocfilehash: 656f749fd2a930c51bfd7d1a99642fae87694846
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2018
ms.locfileid: "53096630"
---
# <a name="understand-stream-analytics-job-monitoring-and-how-to-monitor-queries"></a>Descripción de la supervisión del trabajo de Stream Analytics y cómo supervisar consultas

## <a name="introduction-the-monitor-page"></a>Introducción: La página de supervisión
Azure Portal muestra las métricas de rendimiento clave que se pueden usar para supervisar y solucionar problemas de rendimiento del trabajo y consultas. Para ver estas métricas, vaya al trabajo de Stream Analytics para el que le interesa ver las métricas y consulte la sección **Supervisión** de la página de información general.  

![Vínculo de supervisión de trabajos de Stream Analytics](./media/stream-analytics-monitoring/02-stream-analytics-monitoring-block.png)

Se mostrará la siguiente ventana:

![Panel de supervisión de trabajos de Stream Analytics](./media/stream-analytics-monitoring/01-stream-analytics-monitoring.png)  

## <a name="metrics-available-for-stream-analytics"></a>Métricas disponibles para Stream Analytics
| Métrica                 | Definición                               |
| ---------------------- | ---------------------------------------- |
| Eventos de entrada pendientes       | Número de eventos de entrada que están pendientes. |
| Errores de conversión de datos | Número de eventos de salida que no se pudieron convertir al esquema de salida previsto. |
| Primeros eventos de entrada       | Número de eventos recibidos primero. |
| Solicitudes de función con errores | Número de llamadas a la función Azure Machine Learning con error (si corresponde). |
| Eventos de función        | Número de eventos enviados a la función Azure Machine Learning (si corresponde). |
| Solicitudes de función      | Número de llamadas a la función Azure Machine Learning (si corresponde). |
| Errores de deserialización de entrada       | Número de eventos que no se pudieron deserializar.  |
| Bytes del evento de entrada      | Cantidad de datos recibidos por el trabajo de Stream Analytics, en bytes. Puede usarse para validar que los eventos que se envían al origen de entrada. |
| Eventos de entrada           | Cantidad de datos recibidos por el trabajo de Stream Analytics, en términos de recuento de eventos. Puede usarse para validar que los eventos que se envían al origen de entrada. |
| Orígenes de entrada recibidos       | Número de eventos procedentes de un origen de entrada. |
| Eventos de entrada retrasada      | Número de eventos que llegan tarde del origen y que se han eliminado o cuya marca de tiempo se ha ajustado, en función de la configuración de la Directiva de ordenación de eventos del ajuste del Período de tolerancia de fuera de servicio. |
| Eventos que no funcionan    | Número de eventos recibidos fuera de orden que se eliminan o se les asigna una marca de tiempo ajustada, según la Directiva de ordenación de eventos. Puede verse afectado por la configuración del ajuste de Período de tolerancia de fuera de servicio. |
| Eventos de salida          | Cantidad de datos enviados por el trabajo de Stream Analytics al destino de salida, en términos de recuento de eventos. |
| Errores de tiempo de ejecución         | El número total de errores relacionados con el procesamiento de consultas (sin incluir los errores encontrados durante la ingesta de eventos o los resultados de salida) |
| SU % uso       | El uso de las unidades de streaming asignadas a un trabajo en la pestaña Escala del trabajo. Si este indicador llega o supera el 80 %, existe una gran probabilidad de que el procesamiento de eventos se retrase o deje de avanzar. |
| Retraso de la marca de agua       | Retraso máximo de la marca de agua en todas las particiones de todas las salidas del trabajo. |

## <a name="customizing-monitoring-in-the-azure-portal"></a>Personalización de la supervisión en Azure Portal
Puede ajustar el tipo de gráfico, las métricas que se muestran y el intervalo de tiempo en la configuración de Editar gráfico. Para obtener detalles, vea [Personalización de la supervisión](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).

  ![Gráfico del tiempo de supervisión de consultas de Stream Analytics](./media/stream-analytics-monitoring/08-stream-analytics-monitoring.png)  


## <a name="latest-output"></a>Salida más reciente
Otro punto de datos interesante para supervisar el trabajo es la hora de la salida más reciente, que se muestra en la página Introducción.
La hora es el tiempo de aplicación (es decir, el tiempo usando la marca de tiempo desde los datos de eventos) de la salida más reciente de su trabajo.

## <a name="get-help"></a>Obtención de ayuda
Para obtener más ayuda, pruebe nuestro [foro de Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Pasos siguientes
* [Introducción a Azure Stream Analytics](stream-analytics-introduction.md)
* [Introducción al uso de Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Escalación de trabajos de Azure Stream Analytics](stream-analytics-scale-jobs.md)
* [Referencia del lenguaje de consulta de Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referencia de API de REST de administración de Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)

