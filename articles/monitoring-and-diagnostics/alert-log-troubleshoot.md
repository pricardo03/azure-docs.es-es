---
title: Solución de problemas de alertas de registro en Azure Monitor
description: Problemas comunes, errores y resolución para las reglas de alertas de registro en Azure.
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 10/29/2018
ms.author: vinagara
ms.component: alerts
ms.openlocfilehash: 68488788f73c9662b5d1eaa3b670f2120941defc
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2018
ms.locfileid: "51616493"
---
# <a name="troubleshooting-log-alerts-in-azure-monitor"></a>Solución de problemas de alertas de registro en Azure Monitor  
## <a name="overview"></a>Información general
En este artículo se muestra cómo solucionar problemas habituales al configurar las alertas de registro en Azure Monitor. Además, se proporcionan soluciones a las preguntas más frecuentes sobre la funcionalidad o la configuración de las alertas de registro. 

El término **Alertas de registro** se usa para describir las alertas que se activan con base en una consulta personalizada de [Log Analytics](../log-analytics/log-analytics-tutorial-viewdata.md) o [Application Insights](../application-insights/app-insights-analytics.md). Obtenga más información acerca de la funcionalidad, la terminología y los tipos en [Alertas de registro: información general](monitor-alerts-unified-log.md).

> [!NOTE]
> En este artículo no se tienen en cuenta los casos en que Azure Portal muestra una regla de alertas desencadenada y una notificación realizada a través de grupos de acciones asociados. Para estos casos, consulte los detalles en el artículo sobre [grupos de acciones](monitoring-action-groups.md).


## <a name="log-alert-didnt-fire"></a>No se activó la alerta de registro

Estas son algunas causas habituales por las que el estado de una [regla de alerta de registro en Azure Monitor](alert-log.md) configurada no se muestra [como *desencadenada* cuando se espera](monitoring-alerts-managing-alert-states.md). 

### <a name="data-ingestion-time-for-logs"></a>Tiempo de ingesta de datos para registros
La alerta de registro ejecuta periódicamente una consulta basada en [Log Analytics](../log-analytics/log-analytics-tutorial-viewdata.md) o [Application Insights](../application-insights/app-insights-analytics.md). Ya que Log Analytics procesa muchos terabytes de datos de miles de clientes desde diferentes orígenes en todo el mundo, el servicio es susceptible de sufrir retrasos variables. Para obtener más información, consulte [Tiempos de la ingesta de datos en Log Analytics](../log-analytics/log-analytics-data-ingestion-time.md).

Para mitigar el retraso durante la ingesta de datos, el sistema espera y vuelve a intentar la consulta de alerta varias veces si detecta que aún no se ingieren los datos necesarios. El sistema tiene un tiempo de espera establecido que aumenta exponencialmente. La alerta de registro solo se desencadena después de que los datos están disponibles, por lo que el retraso puede deberse a una ingesta de datos de registro lenta. 

### <a name="incorrect-time-period-configured"></a>Período de tiempo configurado incorrectamente
Como se describe en el artículo sobre la [terminología para alertas de registro](monitor-alerts-unified-log.md#log-search-alert-rule---definition-and-types), el período de tiempo que se indica en la configuración especifica el intervalo de tiempo para la consulta. La consulta devuelve solo los registros que se crearon dentro de este intervalo de tiempo. El período de tiempo restringe los datos capturados para la consulta de registros, a fin de evitar abusos y cualquier comando de tiempo (como “ago”) utilizado en las consultas de registros. 
*Por ejemplo, si el período de tiempo se establece en 60 minutos, y la consulta se ejecuta a la 1:15 p. m., se usan solo los registros creados entre las 12:15 p. m. y la 1:15 p. m. para la consulta de registros. Si la consulta de registro usa un comando de tiempo como *ago (1d)*, la consulta usa solo datos entre las 12:15 p. m. y la 1:15 p. m. porque el período de tiempo está establecido en ese intervalo.*

Por lo tanto, debe comprobar que el período de tiempo en la configuración coincida con la consulta. En el ejemplo anterior, si la consulta de registro utiliza *ago (1d)* como indica el marcador verde, el período de tiempo debe establecerse en 24 horas o 1440 minutos (como se indica en rojo) para garantizar que la consulta se ejecute como estaba previsto.

![Período de tiempo](./media/monitor-alerts-unified/LogAlertTimePeriod.png)

### <a name="suppress-alerts-option-is-set"></a>Se ha establecido la opción Suprimir alertas
Como se describe en el paso 8 del artículo sobre cómo [crear una regla de alerta de registro en Azure Portal](alert-log.md#managing-log-alerts-from-the-azure-portal), las alertas de registro proporcionan una opción para **Suprimir alertas** con el fin de suprimir las acciones de desencadenamiento y notificación durante un período de tiempo configurado. Como resultado, es posible que piense que una alerta no se activó, cuando en realidad sí lo hizo, pero se suprimió.  

![Suprimir alertas](./media/monitor-alerts-unified/LogAlertSuppress.png)

### <a name="metric-measurement-alert-rule-is-incorrect"></a>Regla de alertas de unidades métricas
**Las alertas de registro de unidades métricas** constituyen un subtipo de alertas de registro que tienen funcionalidades especiales y una sintaxis de consulta de alerta restringida. Una regla de alertas de registro de unidades métricas requiere que la salida de la consulta sea una serie de métricas de tiempo: una tabla con distintos períodos de tiempo de igual tamaño junto con los valores totales correspondientes. Además, los usuarios pueden decidir que haya variables adicionales en la tabla junto con AggregatedValue. Estas variables se pueden usar para ordenar la tabla. 

Por ejemplo, suponga que se ha configurado una regla de alertas de registro de unidades métricas como:
- la consulta era: `search *| summarize AggregatedValue = count() by $table, bin(timestamp, 1h)`  
- período de tiempo de 6 horas
- umbral de 50
- lógica de alerta de tres infracciones consecutivas
- Se ha elegido Aggregate Upon como $table

Ya que el comando incluye *summarize... by* y se han proporcionado dos variables (timestamp y $table), el sistema elegirá $table para "Aggregate Upon". Se ordenará la tabla de resultados por el campo *$table*, tal como se muestra a continuación y, luego, observará los distintos valores de AggregatedValue para cada tipo de tabla (por ejemplo, availabilityResults) para ver si se han producido 3 o más infracciones consecutivas.

![Ejecución de consultas de unidades métricas con varios valores](./media/monitor-alerts-unified/LogMMQuery.png)

Como "Aggregate Upon" es $table, los datos se ordenan por la columna $table (se muestra en rojo). A continuación, se agrupan y buscan los tipos de campo "Aggregate Upon" con el valor $table. Por ejemplo, los valores de availabilityResults se considerarán como un trazado o entidad (se resalta en naranja). En este trazado o entidad de valor, el servicio de alertas comprueba si se producen tres infracciones consecutivas (se muestra en verde) para las que se desencadenará la alerta del valor de tabla "availabilityResults". De forma similar, si para cualquier otro valor de $table se detectan tres infracciones consecutivas, se desencadenará otra notificación de alerta para ellas y el servicio de alertas ordenará automáticamente los valores de un trazado o entidad (se muestra en naranja) por tiempo.

Ahora suponga que se modificó la regla de alertas de registro de unidades métricas y la consulta era `search *| summarize AggregatedValue = count() by bin(timestamp, 1h)`; el resto de la configuración permanece igual que antes, incluida la lógica de alerta para tres infracciones consecutivas. La opción "Aggregate Upon", en este caso, será de forma predeterminada: timestamp. Puesto que se proporciona un solo valor en la consulta para summarize... by (que es) timestamp, de manera similar al ejemplo anterior, al final de la ejecución, la salida será la que se muestra a continuación. 

   ![Ejecución de consultas de unidades métricas con un valor singular](./media/monitor-alerts-unified/LogMMtimestamp.png)

Como "Aggregate Upon" es timestamp, los datos se ordenan por la columna timestamp (se muestra en rojo); a continuación, agrupamos por timestamp; por ejemplo, los valores de `2018-10-17T06:00:00Z` se considerarán como un trazado o entidad (se resalta en naranja). En este trazado o entidad de valor, el servicio de alertas no encontrará ninguna infracción consecutiva (ya que cada valor de timestamp tiene una sola entrada) y, por tanto, la alerta nunca se desencadenará. Por lo tanto, en este caso, el usuario debe:
- Agregar una variable ficticia o una variable existente (como $table) para ordenar correctamente con el campo "Aggregate Upon" configurado
- O volver a configurar una regla de alertas para usar, en su lugar, la lógica de alerta basada en *infracción total* correctamente
 
## <a name="log-alert-fired-unnecessarily"></a>Alerta de registro activada innecesariamente
A continuación, se detallan algunas causas habituales que pueden desencadenar una [regla de alertas de registro en Azure Monitor](alert-log.md) configurada cuando se muestra en las [alertas de Azure](monitoring-alerts-managing-alert-states.md) cuando no se espera que se active.

### <a name="alert-triggered-by-partial-data"></a>Alerta desencadenada por datos parciales
Log Analytics y Application Insights utilizan Analytics, que está sujeto a retrasos de ingesta y procesamiento debido a los cuales, en el momento en que se ejecuta la consulta de alerta de registro proporcionada, puede que no haya ningún dato disponible o que haya pocos. Para obtener más información, consulte [Tiempos de la ingesta de datos en Log Analytics](../log-analytics/log-analytics-data-ingestion-time.md).

Según cómo esté configurada la regla de alertas, puede producirse una activación incorrecta en caso de que no haya ningún dato en los registros, o sean parciales, en el momento de la ejecución de la alerta. En tales casos, se recomienda cambiar la configuración o la consulta de alerta. *Por ejemplo, si la regla de alertas de registro se configura para desencadenarse cuando el número de resultados de una consulta de Analytics sea menor que 5, cuando no haya datos (cero registros) o los resultados sean parciales (un registro), se desencadenará la regla de alertas. Mientras que, después del retraso de la ingesta, cuando la misma consulta se ejecute en Analytics con los datos completos puede proporcionar resultados de 10 registros.*

### <a name="alert-query-output-misunderstood"></a>Salida de la consulta de alerta no comprendida
Para las alertas de registro, el usuario proporciona la lógica de alertas a través de la consulta de Analytics. La consulta de Analytics proporcionada puede emplear varias funciones matemáticas y de macrodatos para crear construcciones específicas. El servicio de alertas ejecutará la consulta proporcionada por el cliente a intervalos especificados con los datos para el período de tiempo especificado; el servicio de alertas realiza cambios sutiles en la consulta proporcionada, según el tipo de alerta elegido y lo mismo puede observarse en la sección "Consulta que se va a ejecutar" en la pantalla Configurar lógica de señal, como se muestra a continuación: ![Consulta que se va a ejecutar](./media/monitor-alerts-unified/LogAlertPreview.png)
 
Lo que se muestra en la sección **Consulta que se va a ejecutar** es lo que ejecutará el servicio de alertas de registro; el usuario puede ejecutar la consulta indicada, así como el intervalo de tiempo a través del [portal de Analytics](../log-analytics/log-analytics-log-search-portals.md) o [Analytics API](https://docs.microsoft.com/rest/api/loganalytics/), si desea comprender, antes de la creación de la alerta, la salida de la consulta de alerta.
 
## <a name="next-steps"></a>Pasos siguientes

* Más información sobre las [alertas de registro en las alertas de Azure](monitor-alerts-unified-log.md).
* Más información sobre [Application Insights](../application-insights/app-insights-analytics.md)
* Más información sobre [Log Analytics](../log-analytics/log-analytics-overview.md). 

