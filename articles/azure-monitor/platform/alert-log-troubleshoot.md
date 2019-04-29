---
title: Solución de problemas de las alertas del registro en Azure Monitor | Microsoft Docs
description: Common problemas, errores y resolución de las reglas de alerta de registro en Azure.
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 10/29/2018
ms.author: vinagara
ms.subservice: alerts
ms.openlocfilehash: 0c7189f1d43a114532b30b0c1aabe6f7cd4402d8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60775989"
---
# <a name="troubleshooting-log-alerts-in-azure-monitor"></a>Solución de problemas de alertas de registro en Azure Monitor  

## <a name="overview"></a>Información general

Este artículo muestra cómo resolver problemas comunes observados al configurar las alertas del registro en Azure Monitor. Además, se proporcionan soluciones a las preguntas más frecuentes sobre la funcionalidad o la configuración de las alertas de registro. 

El término **alertas del registro** se describen las alertas que fire se basa en una consulta de registro en un [área de trabajo de Log Analytics](../learn/tutorial-viewdata.md) o [Application Insights](../../azure-monitor/app/analytics.md). Obtenga más información acerca de la funcionalidad, la terminología y los tipos en [Alertas de registro: información general](../platform/alerts-unified-log.md).

> [!NOTE]
> En este artículo no se tienen en cuenta los casos en que Azure Portal muestra una regla de alertas desencadenada y una notificación realizada a través de grupos de acciones asociados. Para estos casos, consulte los detalles en el artículo sobre [grupos de acciones](../platform/action-groups.md).

## <a name="log-alert-didnt-fire"></a>No se activó la alerta de registro

Estas son algunas causas habituales por las que el estado de una [regla de alerta de registro en Azure Monitor](../platform/alerts-log.md) configurada no se muestra [como *desencadenada* cuando se espera](../platform/alerts-managing-alert-states.md). 

### <a name="data-ingestion-time-for-logs"></a>Tiempo de ingesta de datos para registros

La alerta de registro ejecuta periódicamente una consulta basada en [Log Analytics](../learn/tutorial-viewdata.md) o [Application Insights](../../azure-monitor/app/analytics.md). Puesto que Azure Monitor procesa muchos terabytes de datos procedentes de miles de clientes de diversos orígenes en todo el mundo, el servicio es susceptible a un retraso de tiempo diferentes. Para obtener más información, consulte [tiempo de recopilación de datos en registros de Azure Monitor](../platform/data-ingestion-time.md).

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

Log Analytics y Application Insights utilizan Analytics, que está sujeto a retrasos de ingesta y procesamiento debido a los cuales, en el momento en que se ejecuta la consulta de alerta de registro proporcionada, puede que no haya ningún dato disponible o que haya pocos. Para obtener más información, consulte [tiempo de recopilación de datos de registro en Azure Monitor](../platform/data-ingestion-time.md).

En función de cómo esté configurada la regla de alertas, puede producirse una activación incorrecta si no hay ningún dato en los registros, o los datos que hay son parciales, en el momento de la ejecución de la alerta. En tales casos, se recomienda cambiar la configuración o la consulta de alerta. 

Por ejemplo, si la regla de alertas de registro se configura para desencadenarse cuando el número de resultados de una consulta de Analytics sea menor que 5, la alerta se desencadenará cuando no haya datos (cero registros) o los resultados sean parciales (un registro). Sin embargo, después del retraso de la ingesta de datos, la misma consulta con todos los datos podría proporcionar un resultado de 10 registros.

### <a name="alert-query-output-misunderstood"></a>Salida de la consulta de alerta no comprendida

La lógica de las alertas de registros se proporcionan en una consulta de Analytics. La consulta de Analytics puede usar varias funciones matemáticas y macrodatos.  El servicio de generación de alertas ejecuta la consulta a los intervalos especificados con los datos del período especificado. Este servicio realiza pequeños cambios en la consulta proporcionada en función del tipo de alerta elegido. Este cambio puede verse en la sección "Para ejecutar la consulta" en *configurar lógica de señal* pantalla, tal como se muestra a continuación: ![Query to be executed](media/alert-log-troubleshoot/LogAlertPreview.png)

Lo que se muestra en el cuadro **Query to be executed** es lo que ejecuta el servicio de alertas de registros. La consulta indicada se puede ejecutar también como intervalo de tiempo a través del [portal de Analytics](../log-query/portals.md) o [Analytics API](https://docs.microsoft.com/rest/api/loganalytics/) si se desea saber cuál puede ser el resultado de la consulta de la alerta antes de crear la alerta.

## <a name="log-alert-was-disabled"></a>Se deshabilitó la alerta del registro

A continuación aparecen algunas de las razones debido a que [regla de alerta de registro en Azure Monitor](../platform/alerts-log.md) pueden deshabilitarse por Azure Monitor.

### <a name="resource-on-which-alert-was-created-no-longer-exists"></a>Recursos en el que se creó alerta ya no existe

Las reglas de alerta de registro creadas en Azure Monitor como destino un recurso específico, como un área de trabajo de Log Analytics de Azure, aplicación de Azure Application Insights y recursos de Azure. Y el servicio de alertas de registro, a continuación, ejecutará la consulta de analytics proporcionada en la regla para el destino especificado. Pero después de la creación de reglas, a menudo, vaya al eliminar de Azure o mover dentro de Azure: el destino de la regla de alerta. Como el destino de la regla de alerta de registro ya no es válido, se produce un error en la ejecución de la regla.

En tales casos, Azure Monitor deshabilitará la alerta del registro y asegúrese de que los clientes no se facturan innecesariamente, cuando la propia regla no puede ejecutar continuamente por un período considerable como una semana. Los usuarios pueden averiguar la hora exacta a la que se deshabilitó la regla de alerta de registro por el Monitor de Azure a través de [Azure Activity Log](../../azure-resource-manager/resource-group-audit.md). En el registro de actividad de Azure cuando se deshabilita la regla de alerta de registro Azure, se agrega un evento en el registro de actividad de Azure.

Un evento de ejemplo en el registro de actividad de Azure para deshabilitar la regla de alertas debido a su error continuo; se muestra a continuación.

```json
{
    "caller": "Microsoft.Insights/ScheduledQueryRules",
    "channels": "Operation",
    "claims": {
        "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/spn": "Microsoft.Insights/ScheduledQueryRules"
    },
    "correlationId": "abcdefg-4d12-1234-4256-21233554aff",
    "description": "Alert: test-bad-alerts is disabled by the System due to : Alert has been failing consistently with the same exception for the past week",
    "eventDataId": "f123e07-bf45-1234-4565-123a123455b",
    "eventName": {
        "value": "",
        "localizedValue": ""
    },
    "category": {
        "value": "Administrative",
        "localizedValue": "Administrative"
    },
    "eventTimestamp": "2019-03-22T04:18:22.8569543Z",
    "id": "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<ResourceGroup>/PROVIDERS/MICROSOFT.INSIGHTS/SCHEDULEDQUERYRULES/TEST-BAD-ALERTS",
    "level": "Informational",
    "operationId": "",
    "operationName": {
        "value": "Microsoft.Insights/ScheduledQueryRules/disable/action",
        "localizedValue": "Microsoft.Insights/ScheduledQueryRules/disable/action"
    },
    "resourceGroupName": "<Resource Group>",
    "resourceProviderName": {
        "value": "MICROSOFT.INSIGHTS",
        "localizedValue": "Microsoft Insights"
    },
    "resourceType": {
        "value": "MICROSOFT.INSIGHTS/scheduledqueryrules",
        "localizedValue": "MICROSOFT.INSIGHTS/scheduledqueryrules"
    },
    "resourceId": "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<ResourceGroup>/PROVIDERS/MICROSOFT.INSIGHTS/SCHEDULEDQUERYRULES/TEST-BAD-ALERTS",
    "status": {
        "value": "Succeeded",
        "localizedValue": "Succeeded"
    },
    "subStatus": {
        "value": "",
        "localizedValue": ""
    },
    "submissionTimestamp": "2019-03-22T04:18:22.8569543Z",
    "subscriptionId": "<SubscriptionId>",
    "properties": {
        "resourceId": "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<ResourceGroup>/PROVIDERS/MICROSOFT.INSIGHTS/SCHEDULEDQUERYRULES/TEST-BAD-ALERTS",
        "subscriptionId": "<SubscriptionId>",
        "resourceGroup": "<ResourceGroup>",
        "eventDataId": "12e12345-12dd-1234-8e3e-12345b7a1234",
        "eventTimeStamp": "03/22/2019 04:18:22",
        "issueStartTime": "03/22/2019 04:18:22",
        "operationName": "Microsoft.Insights/ScheduledQueryRules/disable/action",
        "status": "Succeeded",
        "reason": "Alert has been failing consistently with the same exception for the past week"
    },
    "relatedEvents": []
}
```

### <a name="query-used-in-log-alert-is-not-valid"></a>Consulta que se usa en la alerta del registro no es válido

Cada regla de alerta de registro creado en Azure Monitor como parte de su configuración debe especificar una consulta de analytics que va a ejecutar periódicamente el servicio de alerta. Mientras que la consulta de analytics puede tener la sintaxis correcta en el momento de creación de reglas o actualización. Algunas veces durante un período de tiempo, la consulta se proporcionan en el registro de la regla de alerta puede desarrollar problemas de sintaxis y hacer que empecemos a suspender la ejecución de la regla. Algunas razones comunes por qué consulta de analytics proporcionada en una regla de alerta de registro puede desarrollar errores son:

- Consulta que se escribe en [ejecutar en varios recursos](../log-query/cross-workspace-query.md) y uno o varios de los recursos especificados, ahora no existen.
- No ha habido ningún flujo de datos para la plataforma de análisis, debido a que el [ejecución de la consulta da error](https://dev.loganalytics.io/documentation/Using-the-API/Errors) porque no hay ningún dato para la consulta proporcionada.
- Los cambios en [lenguaje de consulta](https://docs.microsoft.com/azure/kusto/query/) han producido en los comandos y las funciones tienen un formato revisado. Por lo tanto, la consulta proporcionada anteriormente en la regla de alerta ya no es válida.

El usuario deberá advertimos de este comportamiento en primer lugar a través de [Azure Advisor](../../advisor/advisor-overview.md). Se agregaría una recomendación para la regla de alerta de registro específico en Azure Advisor, en la categoría de alta disponibilidad con repercusión media y la descripción como "Reparación en la regla de alerta de registro para asegurarse de supervisión". Si después de siete días de la recomendación que proporciona en Azure Advisor no se rectifique la consulta de alerta en la regla de alerta de registro especificado. A continuación, Azure Monitor deshabilitará la alerta del registro y asegúrese de que los clientes no se facturan innecesariamente, cuando la propia regla no puede ejecutar continuamente por un período considerable como una semana.

Los usuarios pueden averiguar la hora exacta a la que se deshabilitó la regla de alerta de registro por el Monitor de Azure a través de [Azure Activity Log](../../azure-resource-manager/resource-group-audit.md). En el registro de actividad de Azure, cuando está deshabilitada la regla de alerta de registro de Azure: se agrega un evento en el registro de actividad de Azure.

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre las [alertas de registro en las alertas de Azure](../platform/alerts-unified-log.md).
- Más información sobre [Application Insights](../../azure-monitor/app/analytics.md)
- Obtenga más información sobre [registrar las consultas](../log-query/log-query-overview.md)
