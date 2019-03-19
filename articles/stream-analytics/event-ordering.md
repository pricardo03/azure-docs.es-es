---
title: Configurar el orden de las directivas de Azure Stream Analytics de eventos
description: En este artículo se describe cómo configurar incluso los pedidos de configuración en Stream Analytics
services: stream-analytics
author: sidram
ms.author: sidram
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/12/2019
ms.openlocfilehash: 970eeb871775e24abb87c8b977e214645e514d3b
ms.sourcegitcommit: f331186a967d21c302a128299f60402e89035a8d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "58190567"
---
# <a name="configuring-event-ordering-policies-for-azure-stream-analytics"></a>Configurar el orden de las directivas de Azure Stream Analytics de eventos

En este artículo se describe cómo configurar y usar la llegada tardía y directivas de desorden de eventos en Azure Stream Analytics. Estas directivas se aplican solo cuando se usa el [TIMESTAMP BY](https://docs.microsoft.com/stream-analytics-query/timestamp-by-azure-stream-analytics) cláusula en la consulta.

## <a name="event-time-and-arrival-time"></a>Hora del evento y la hora de llegada

El trabajo de Stream Analytics puede procesar eventos según cualquiera *hora del evento* o *hora de llegada*. **Tiempo de evento/aplicación** es la marca de tiempo presente en la carga del evento (cuando se generó el evento). **Hora de llegada** es la marca de tiempo cuando se recibe el evento en el origen de entrada (almacenamiento de Event Hubs o IoT Hub o Blob). 

De forma predeterminada, Stream Analytics procesa eventos por *hora de llegada*, pero puede elegir procesar los eventos por *hora del evento* utilizando [TIMESTAMP BY](https://docs.microsoft.com/stream-analytics-query/timestamp-by-azure-stream-analytics) cláusula en la consulta. Las directivas en tiempo de ejecución de llegada y de desorden son aplicables solo si procesa eventos según la hora del evento. Tenga en cuenta los requisitos de latencia y lo correcto para su escenario al configurar estos valores. 

## <a name="what-is-late-arrival-policy"></a>¿Qué es Directiva de llegadas tarde?

En ocasiones, los eventos llegan en tiempo de ejecución debido a varias razones. Por ejemplo, un evento que llega tarde 40 segundos tendrá tiempo de evento = hora 00:10:00 y llegada = 00:10:40. Si establece la directiva de llegada tardía en 15 segundos, todos los eventos que llegan más tarde de 15 segundos cualquiera se eliminarán (no se procesan por Stream Analytics) o se ha ajustado la hora del evento. En el ejemplo anterior, como el evento llega a 40 segundos en tiempo de ejecución (más de conjunto de directivas), su hora del evento se ajustará al máximo en las últimas llegada directiva 00:10:25 (hora de llegada - valor de la directiva de llegada tardía). Directiva predeterminada de llegada tardía es 5 segundos.

## <a name="what-is-out-of-order-policy"></a>¿Qué es Directiva de fuera de orden? 

Evento puede llegar desordenados también. Después de hora del evento se ajusta en función de la directiva de llegadas tarde, también puede elegir automáticamente anular o ajustar los eventos que están fuera de secuencia. Si establece esta directiva en 8 segundos, los eventos que llegan desordenados, pero dentro de la ventana de 8 segundos se reordenan según la hora del evento. Los eventos que llegan más tarde se se descartan o ajustan el valor de directiva de fuera de secuencia máxima. Directiva de fuera de orden predeterminado es 0 segundos. 

## <a name="adjust-or-drop-events"></a>Ajustar o quitar eventos

Si los eventos llegan en tiempo de ejecución o de desorden en función de las directivas que ha configurado, puede colocar dichos eventos (no se procesan por Stream Analytics) o tener el ajusta su hora del evento.

Nos gustaría ver un ejemplo de estas directivas en acción.
<br> **Directiva de llegadas tarde:** 15 segundos
<br> **Directiva de fuera de secuencia:** 8 segundos

| Nº de eventos | Hora del evento | Hora de llegada | System.Timestamp | Explicación |
| --- | --- | --- | --- | --- |
| **1** | 00:10:00  | 00:10:40  | 00:10:25  | Evento llega a nivel de tolerancia en tiempo de ejecución y el exterior. Hasta hora del evento se ajusta al máximo tolerancia de llegada tardía.  |
| **2** | 00:10:30 | 00:10:41  | 00:10:30  | Evento que ha llegado en tiempo de ejecución, pero dentro de nivel de tolerancia. Por lo que no se ajustan la hora del evento.  |
| **3** | 00:10:42 | 00:10:42 | 00:10:42 | Evento que ha llegado a tiempo. No es necesitado ningún ajuste.  |
| **4** | 00:10:38  | 00:10:43  | 00:10:38 | Evento que ha llegado de desorden, pero dentro de la tolerancia de 8 segundos. Por lo tanto, no se ajustan hora del evento. Con fines de análisis, este evento se considera que el número de evento 4 anterior.  |
| **5** | 00:10:35 | 00:10:45  | 00:10:37 | Evento que ha llegado tolerancia de desorden como fuera de los 8 segundos. Por lo tanto, la hora del evento se ajusta a un máximo de tolerancia de desorden. |

## <a name="can-these-settings-delay-output-of-my-job"></a>¿Esta configuración puede retrasar la salida de mi trabajo? 

Sí. De forma predeterminada, el desorden de directiva se establece en cero (00 minutos y 00 segundos). Si cambia el valor predeterminado, la salida del trabajo primer es diferida por este valor (o superior). 

Si una de las particiones de las entradas no recibe los eventos, debe esperar la salida se retrasa el valor de directiva de llegada tardía. Para saber por qué, lea la sección de error InputPartition siguiente. 

## <a name="i-see-lateinputevents-messages-in-my-activity-log"></a>Veo mensajes LateInputEvents en mi registro de actividad

Estos mensajes se muestran para informarle de que los eventos se llegar tarde y se descartan o ajustan según la configuración. Puede omitir estos mensajes si ha configurado la directiva de llegada tardía adecuadamente. 

Ejemplo de este mensaje es: <br>
<code>
{"message Time":"2019-02-04 17:11:52Z","error":null,
"message":"First Occurred: 02/04/2019 17:11:48 | Resource Name: ASAjob | Message: Source 'ASAjob' had 24 data errors of kind 'LateInputEvent' between processing times '2019-02-04T17:10:49.7250696Z' and '2019-02-04T17:11:48.7563961Z'. Input event with application timestamp '2019-02-04T17:05:51.6050000' and arrival time '2019-02-04T17:10:44.3090000' was sent later than configured tolerance.","type":"DiagnosticMessage","correlation ID":"49efa148-4asd-4fe0-869d-a40ba4d7ef3b"} 
</code>

## <a name="i-see-inputpartitionnotprogressing-in-my-activity-log"></a>Veo InputPartitionNotProgressing en mi registro de actividad

El origen de entrada (Event Hub o IoT Hub) es probable que tiene varias particiones. Azure Stream Analytics genera una salida para la marca de tiempo t1 solo después de todas las particiones se combinan como mínimo en tiempo t1. Por ejemplo, supongamos que la consulta proviene de una partición de un centro de eventos que a su vez tiene dos particiones. Una de las particiones, P1, tiene eventos hasta la hora t1. La otra partición, P2, tiene eventos hasta la hora t1 + x. A continuación, se produce la salida hasta que la hora t1. Pero si hay una partición explícita mediante la cláusula de PartitionId, ambas particiones progresan de manera independiente. 

Cuando se combinan varias particiones del mismo flujo de entrada, la tolerancia de llegada tardía es la cantidad máxima de tiempo que cada partición espera nuevos datos. Si hay una partición del centro de eventos, o si la instancia de IoT Hub no recibe las entradas, la escala de tiempo para esa partición no avanza hasta que alcanza el umbral de tolerancia de llegada tardía. Esto retrasa la salida por el umbral de tolerancia de llegada tardía. En tales casos, es posible que vea el siguiente mensaje:
<br><code>
{"message Time":"2/3/2019 8:54:16 PM UTC","message":"Input Partition [2] does not have additional data for more than [5] minute(s). Partition will not progress until either events arrive or late arrival threshold is met.","type":"InputPartitionNotProgressing","correlation ID":"2328d411-52c7-4100-ba01-1e860c757fc2"} 
</code><br><br>
Este mensaje para informarle de que al menos una partición de entrada está vacía y retrasará la salida por el umbral de llegada tardía. Para solucionar este problema, se recomienda, ya sea: 1. Asegúrese de que todas las particiones de Event Hub o IoT Hub reciben la entrada. 2. Usar partición mediante la cláusula de PartitionID en la consulta. 

## <a name="next-steps"></a>Pasos siguientes
* [Consideraciones sobre el control de tiempo](stream-analytics-time-handling.md)
* [Métricas disponibles para Stream Analytics](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-monitoring#metrics-available-for-stream-analytics)
