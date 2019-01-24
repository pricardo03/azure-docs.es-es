---
title: Solución de problemas de alertas de registro en Azure Monitor
description: Problemas comunes, errores y resolución para las reglas de alertas de registro en Azure.
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 10/29/2018
ms.author: vinagara
ms.subservice: alerts
ms.openlocfilehash: 56d76cd43b63a389569ae39c1e987a5fccbb9793
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2019
ms.locfileid: "54429453"
---
# <a name="troubleshooting-log-alerts-in-azure-monitor"></a>Solución de problemas de alertas de registro en Azure Monitor  

## <a name="overview"></a>Información general

En este artículo se muestra cómo solucionar problemas habituales al configurar las alertas de registro en Azure Monitor. Además, se proporcionan soluciones a las preguntas más frecuentes sobre la funcionalidad o la configuración de las alertas de registro. 

El término **Alertas de registro** describe las alertas que se activan a partir de en una consulta personalizada de [Log Analytics](../learn/tutorial-viewdata.md) o [Application Insights](../../azure-monitor/app/analytics.md). Obtenga más información acerca de la funcionalidad, la terminología y los tipos en [Alertas de registro: información general](../platform/alerts-unified-log.md).

> [!NOTE]
> En este artículo no se tienen en cuenta los casos en que Azure Portal muestra una regla de alertas desencadenada y una notificación realizada a través de grupos de acciones asociados. Para estos casos, consulte los detalles en el artículo sobre [grupos de acciones](../platform/action-groups.md).


## <a name="log-alert-didnt-fire"></a>No se activó la alerta de registro

Estas son algunas causas habituales por las que el estado de una [regla de alerta de registro en Azure Monitor](../platform/alerts-log.md) configurada no se muestra [como *desencadenada* cuando se espera](../platform/alerts-managing-alert-states.md). 

### <a name="data-ingestion-time-for-logs"></a>Tiempo de ingesta de datos para registros

La alerta de registro ejecuta periódicamente una consulta basada en [Log Analytics](../learn/tutorial-viewdata.md) o [Application Insights](../../azure-monitor/app/analytics.md). Ya que Log Analytics procesa muchos terabytes de datos de miles de clientes desde diferentes orígenes en todo el mundo, el servicio es susceptible de sufrir retrasos variables. Para obtener más información, consulte [Tiempos de la ingesta de datos en Log Analytics](../platform/data-ingestion-time.md).

Para mitigar el retraso durante la ingesta de datos, el sistema espera y vuelve a intentar la consulta de alerta varias veces si detecta que aún no se ingieren los datos necesarios. El sistema tiene un tiempo de espera establecido que aumenta exponencialmente. La alerta de registro solo se desencadena después de que los datos están disponibles, por lo que el retraso puede deberse a una ingesta de datos de registro lenta. 

### <a name="incorrect-time-period-configured"></a>Período de tiempo configurado incorrectamente

Como se describe en el artículo sobre la [terminología para alertas de registro](../platform/alerts-unified-log.md#log-search-alert-rule---definition-and-types), el período de tiempo que se indica en la configuración especifica el intervalo de tiempo para la consulta. La consulta devuelve solo los registros que se crearon dentro de este intervalo de tiempo. El período restringe los datos capturados de la consulta de registros, con el fin de evitar abusos y evita todos los comandos de tiempo (como *ago*) utilizados en la consulta de registros. Por ejemplo, si el período se establece en 60 minutos, y la consulta se ejecuta a la 1:15 p. m., los registros creados entre las 12:15 p. m. y la 1:15 p. m. son los únicos que se usan para la consulta de registros. Si la consulta de registros usa un comando de tiempo como *ago (1d)*, seguirá utilizando solo los datos entre las 12:15 p. m. y la 1:15 p. m., ya que el período está establecido en ese intervalo.*

Por lo tanto, debe comprobar que el período de tiempo en la configuración coincida con la consulta. En el ejemplo anterior, si la consulta de registro utiliza *ago (1d)* como indica el marcador verde, el período de tiempo debe establecerse en 24 horas o 1440 minutos (como se indica en rojo) para garantizar que la consulta se ejecute como estaba previsto.

![Período de tiempo](media/alert-log-troubleshoot/LogAlertTimePeriod.png)

### <a name="suppress-alerts-option-is-set"></a>Se ha establecido la opción Suprimir alertas

Como se describe en el paso 8 del artículo sobre cómo [crear una regla de alerta de registro en Azure Portal](../platform/alerts-log.md#managing-log-alerts-from-the-azure-portal), las alertas de registro proporcionan una opción para **Suprimir alertas** con el fin de suprimir las acciones de desencadenamiento y notificación durante un período de tiempo configurado. Como resultado, es posible que piense que una alerta no se activó, cuando en realidad sí lo hizo, pero se suprimió.  

![Suprimir alertas](media/alert-log-troubleshoot/LogAlertSuppress.png)

### <a name="metric-measurement-alert-rule-is-incorrect"></a>Regla de alertas de unidades métricas

**Las alertas de registro de unidades métricas** constituyen un subtipo de alertas de registro que tienen funcionalidades especiales y una sintaxis de consulta de alerta restringida. Una regla de alertas de registro de unidades métricas requiere que la salida de la consulta sea una serie de métricas de tiempo: una tabla con distintos períodos de tiempo de igual tamaño junto con los valores totales correspondientes. Además, los usuarios pueden decidir que haya variables adicionales en la tabla junto con AggregatedValue. Estas variables se pueden usar para ordenar la tabla. 

Por ejemplo, suponga que se ha configurado una regla de alertas de registro de unidades métricas como:

- la consulta era: `search *| summarize AggregatedValue = count() by $table, bin(timestamp, 1h)`  
- período de tiempo de 6 horas
- umbral de 50
- lógica de alerta de tres infracciones consecutivas
- Se ha elegido Aggregate Upon como $table

Como en el comando se incluye *summarize ... by* (resumir … por) y se han proporcionado dos variables (timestamp y $table), el sistema elegirá $table para *Aggregate Upon*. Se ordenará la tabla de resultados por el campo *$table*, tal como se muestra a continuación y, luego, observará los distintos valores de AggregatedValue para cada tipo de tabla (por ejemplo, availabilityResults) para ver si se han producido 3 o más infracciones consecutivas.

![Ejecución de consultas de unidades métricas con varios valores](media/alert-log-troubleshoot/LogMMQuery.png)

Como *Aggregate Upon* se define en *$table*, los datos se ordenan por la columna $table (en ROJO); a continuación, se agrupan y se buscan los tipos de campo *Aggregate Upon* con el valor $table. Por ejemplo, los valores de availabilityResults se considerarán como un trazado o entidad (se resalta en naranja). En este trazado o entidad de valor, el servicio de alerta comprueba si se producen tres infracciones consecutivas (se muestra en verde) para las que se desencadenará la alerta del valor de tabla “availabilityResults”. De forma similar, si para cualquier otro valor de $table se detectan tres infracciones consecutivas, se desencadenará otra notificación de alerta para lo mismo y el servicio de alertas ordenará automáticamente los valores en un trazado o una entidad (se muestra en naranja) por tiempo.

Ahora suponga que se modificó la regla de alertas de registro de unidades métricas y la consulta era `search *| summarize AggregatedValue = count() by bin(timestamp, 1h)`; el resto de la configuración permanece igual que antes, incluida la lógica de alerta para tres infracciones consecutivas. La opción "Aggregate Upon", en este caso, será de forma predeterminada: timestamp. Como se proporciona un solo valor en la consulta para *summarize ... by* (que es) *timestamp*, de manera similar al ejemplo anterior, al final de la ejecución, la salida será la que se muestra a continuación.

   ![Ejecución de consultas de unidades métricas con un valor singular](media/alert-log-troubleshoot/LogMMtimestamp.png)

Como *Aggregate Upon* se define en “timestamp”, los datos se ordenan por la columna *timestamp* (se muestra en ROJO); a continuación, agrupamos por timestamp; por ejemplo, los valores de `2018-10-17T06:00:00Z` se considerarán como un trazado o entidad (se resalta en naranja). En este trazado o entidad de valor, el servicio de alerta no encontrará ninguna infracción consecutiva (ya que cada valor de “timestamp” tiene una sola entrada) y, por tanto, la alerta nunca se desencadenará. Por lo tanto, en ese caso, el usuario tiene que:

- Agregar una variable ficticia o una variable existente (como $table) para ordenar correctamente con el campo "Aggregate Upon" configurado
- O volver a configurar una regla de alertas para usar, en su lugar, la lógica de alerta basada en *infracción total* correctamente

## <a name="log-alert-fired-unnecessarily"></a>Alerta de registro activada innecesariamente

A continuación, se detallan algunas causas habituales que pueden desencadenar una [regla de alertas de registro en Azure Monitor](../platform/alerts-log.md) configurada cuando se muestra en las [alertas de Azure](../platform/alerts-managing-alert-states.md) cuando no se espera que se active.

### <a name="alert-triggered-by-partial-data"></a>Alerta desencadenada por datos parciales

Log Analytics y Application Insights utilizan Analytics, que está sujeto a retrasos de ingesta y procesamiento debido a los cuales, en el momento en que se ejecuta la consulta de alerta de registro proporcionada, puede que no haya ningún dato disponible o que haya pocos. Para obtener más información, consulte [Tiempos de la ingesta de datos en Log Analytics](../platform/data-ingestion-time.md).

En función de cómo esté configurada la regla de alertas, puede producirse una activación incorrecta si no hay ningún dato en los registros, o los datos que hay son parciales, en el momento de la ejecución de la alerta. En tales casos, se recomienda cambiar la configuración o la consulta de alerta. 

Por ejemplo, si la regla de alertas de registro se configura para desencadenarse cuando el número de resultados de una consulta de Analytics sea menor que 5, la alerta se desencadenará cuando no haya datos (cero registros) o los resultados sean parciales (un registro). Sin embargo, después del retraso de la ingesta de datos, la misma consulta con todos los datos podría proporcionar un resultado de 10 registros.

### <a name="alert-query-output-misunderstood"></a>Salida de la consulta de alerta no comprendida

La lógica de las alertas de registros se proporcionan en una consulta de Analytics. La consulta de Analytics puede usar varias funciones matemáticas y macrodatos.  El servicio de generación de alertas ejecuta la consulta a los intervalos especificados con los datos del período especificado. Este servicio realiza pequeños cambios en la consulta proporcionada en función del tipo de alerta elegido. Esto puede verse en la sección "Query to be executed" (Consulta que se va a ejecutar) de la pantalla *Configure signal logic* (Configurar lógica de señal), como se muestra a continuación: ![Query to be executed](media/alert-log-troubleshoot/LogAlertPreview.png)

Lo que se muestra en el cuadro **Query to be executed** es lo que ejecuta el servicio de alertas de registros. La consulta indicada se puede ejecutar también como intervalo de tiempo a través del [portal de Analytics](../log-query/portals.md) o [Analytics API](https://docs.microsoft.com/rest/api/loganalytics/) si se desea saber cuál puede ser el resultado de la consulta de la alerta antes de crear la alerta.

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre las [alertas de registro en las alertas de Azure](../platform/alerts-unified-log.md).
- Más información sobre [Application Insights](../../azure-monitor/app/analytics.md)
- Más información sobre [Log Analytics](../../log-analytics/log-analytics-overview.md).
