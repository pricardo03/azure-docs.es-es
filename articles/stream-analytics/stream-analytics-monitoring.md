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
ms.openlocfilehash: 43dd8be998e0f8f3b5a2b783c6a01d5b5ef3da12
ms.sourcegitcommit: 8fc5f676285020379304e3869f01de0653e39466
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/09/2019
ms.locfileid: "65506923"
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
| Eventos de entrada pendientes       | Número de eventos de entrada que están pendientes. Un valor distinto de cero para esta métrica implica que el trabajo no puede mantenerse al día con el número de eventos entrantes. Si este valor se aumenta lentamente o sistemáticamente y pasa a ser distinto de cero, debe escalar el trabajo horizontalmente. Para más información, visite [Descripción y ajuste de las unidades de streaming](stream-analytics-streaming-unit-consumption.md). |
| Errores de conversión de datos | Número de eventos de salida que no se pudieron convertir al esquema de salida previsto. La directiva de errores puede cambiarse a "Drop" para quitar los eventos que se producen en este escenario. |
| Primeros eventos de entrada       | Eventos cuya marca de tiempo de aplicación es anterior a su hora de llegada por más de cinco minutos. |
| Solicitudes de funciones con errores | Número de llamadas a la función Azure Machine Learning con error (si corresponde). |
| Eventos de funciones        | Número de eventos enviados a la función Azure Machine Learning (si corresponde). |
| Solicitudes de funciones      | Número de llamadas a la función Azure Machine Learning (si corresponde). |
| Errores de deserialización de entrada       | Número de eventos de entrada que no se pudieron deserializar.  |
| Bytes del evento de entrada      | Cantidad de datos recibidos por el trabajo de Stream Analytics, en bytes. Puede usarse para validar que los eventos que se envían al origen de entrada. |
| Eventos de entrada           | Número de registros deserializados de los eventos de entrada. Este recuento no incluye los eventos de entrada que se producirán errores de deserialización. |
| Orígenes de entrada recibidos       | Número de mensajes recibidos por el trabajo. Centro de eventos, un mensaje es un único EventData. Para blobs, un mensaje es un único blob. Tenga en cuenta que se cuentan los orígenes de entrada antes de la deserialización. Si hay errores de deserialización, orígenes de entrada pueden ser mayores que los eventos de entrada. En caso contrario, puede ser menor o igual que los eventos de entrada dado que cada mensaje puede contener varios eventos. |
| Eventos de entrada retrasada      | Eventos que llegaron más tarde que el período de tolerancia de llegada tardía configurado. Más información sobre los [puntos a tener en cuenta sobre el orden de eventos de Azure Stream Analytics](stream-analytics-out-of-order-and-late-events.md). |
| Eventos que no funcionan    | Número de eventos recibidos fuera de orden que se eliminan o se les asigna una marca de tiempo ajustada, según la Directiva de ordenación de eventos. Puede verse afectado por la configuración del ajuste de Período de tolerancia de fuera de servicio. |
| Eventos de salida          | Cantidad de datos enviados por el trabajo de Stream Analytics al destino de salida, en términos de recuento de eventos. |
| Errores en runtime         | Número total de errores relacionados con el procesamiento de consulta (excepto los errores encontrados durante la ingesta de eventos o generar resultados) |
| % de utilización de la unidad de streaming       | El uso de las unidades de streaming asignadas a un trabajo en la pestaña Escala del trabajo. Si este indicador llega o supera el 80 %, existe una gran probabilidad de que el procesamiento de eventos se retrase o deje de avanzar. |
| Retraso de la marca de agua       | Retraso máximo de la marca de agua en todas las particiones de todas las salidas del trabajo. |

Puede usar estas métricas para [supervisar el rendimiento del trabajo de Stream Analytics](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-set-up-alerts#scenarios-to-monitor). 

## <a name="customizing-monitoring-in-the-azure-portal"></a>Personalización de la supervisión en Azure Portal
Puede ajustar el tipo de gráfico, las métricas que se muestran y el intervalo de tiempo en la configuración de Editar gráfico. Para obtener detalles, vea [Personalización de la supervisión](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).

  ![Gráfico del tiempo de supervisión de consultas de Stream Analytics](./media/stream-analytics-monitoring/08-stream-analytics-monitoring.png)  


## <a name="latest-output"></a>Salida más reciente
Otro punto de datos interesante para supervisar el trabajo es la hora de la salida más reciente, que se muestra en la página Introducción.
La hora es el tiempo de aplicación (es decir, el tiempo usando la marca de tiempo desde los datos de eventos) de la salida más reciente de su trabajo.

## <a name="get-help"></a>Obtener ayuda
Para obtener más ayuda, pruebe nuestro [foro de Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Pasos siguientes
* [Introducción a Azure Stream Analytics](stream-analytics-introduction.md)
* [Introducción al uso de Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Escalación de trabajos de Azure Stream Analytics](stream-analytics-scale-jobs.md)
* [Referencia del lenguaje de consulta de Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referencia de API de REST de administración de Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)

