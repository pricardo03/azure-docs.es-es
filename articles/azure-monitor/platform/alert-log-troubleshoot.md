---
title: Solucionar problemas de alertas de registro en Azure Monitor | Microsoft Docs
description: Common problemas, errores y soluciones para las reglas de alerta de registro en Azure.
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 10/29/2018
ms.author: vinagara
ms.subservice: alerts
ms.openlocfilehash: 1c7712fc2ce55a3d22995bb119a9ee485a064903
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2019
ms.locfileid: "64683389"
---
# <a name="troubleshoot-log-alerts-in-azure-monitor"></a>Solucionar problemas de alertas de registro en Azure Monitor  

Este artículo muestra cómo resolver problemas comunes que pueden surgir cuando está configurando alertas del registro en Azure Monitor. También proporciona soluciones a problemas comunes con la funcionalidad o la configuración de alertas de registro. 

El término *alertas de registro* se describen las alertas que fire se basa en una consulta de registro en un [área de trabajo de Azure Log Analytics](../learn/tutorial-viewdata.md) o en [Azure Application Insights](../../azure-monitor/app/analytics.md). Obtener información acerca de la funcionalidad, terminología y los tipos en [alertas de registro en Azure Monitor](../platform/alerts-unified-log.md).

> [!NOTE]
> En este artículo no considera que los casos donde el portal de Azure muestra una regla de alerta desencadenada y una notificación no se realiza mediante un grupo de acciones asociada. Para estos casos, vea los detalles en [crear y administrar grupos de acciones en el portal de Azure](../platform/action-groups.md).

## <a name="log-alert-didnt-fire"></a>No se activó la alerta de registro

Estas son algunas causas habituales de por qué el estado de un objeto configurado [regla de alerta de registro en Azure Monitor](../platform/alerts-log.md) no muestra [como *desencadena* cuando se esperaba](../platform/alerts-managing-alert-states.md). 

### <a name="data-ingestion-time-for-logs"></a>Tiempo de recopilación de datos para los registros

Una alerta de registro periódicamente ejecuta la consulta según [Log Analytics](../learn/tutorial-viewdata.md) o [Application Insights](../../azure-monitor/app/analytics.md). Puesto que Azure Monitor procesa muchos terabytes de datos procedentes de miles de clientes de diversos orígenes en todo el mundo, el servicio es susceptible a retrasos de tiempo diferentes. Para obtener más información, consulte [tiempo de recopilación de datos en los registros de Azure Monitor](../platform/data-ingestion-time.md).

Para mitigar los retrasos, el sistema espera volver a intentar la consulta de alerta varias veces si encuentra que aún no se ingieren los datos necesarios. El sistema tiene un tiempo de espera establecido que aumenta exponencialmente. Se desencadena la alerta del registro después de que los datos están disponibles, por lo que el retraso puede deberse a lenta ingesta de datos de registro. 

### <a name="incorrect-time-period-configured"></a>Período de tiempo configurado incorrectamente

Como se describe en el artículo en [terminología para alertas de registro](../platform/alerts-unified-log.md#log-search-alert-rule---definition-and-types), el período de tiempo se indica en la configuración especifica el intervalo de tiempo para la consulta. La consulta devuelve sólo los registros que se crearon dentro de este intervalo. 

El período de tiempo restringe los datos capturados para que una consulta de registro evitar abusos y evita cualquier comando de tiempo (como **hace**) utilizados en una consulta de registro. Por ejemplo, si el período se establece en 60 minutos, y la consulta se ejecuta a la 1:15 p. m., los registros creados entre las 12:15 p. m. y la 1:15 p. m. son los únicos que se usan para la consulta de registros. Si la consulta de registro usa un comando de tiempo como **hace (1 día)**, la consulta usa solo datos entre las 12:15 p. M. y 1:15 P.M. porque el período de tiempo está establecido en ese intervalo.

Compruebe que el período de tiempo en la configuración coincide con la consulta. El ejemplo mostrado anteriormente, si usa la consulta de registro **hace (1 día)** con el marcador de color verde, el período de tiempo debe establecerse en 24 horas o 1440 minutos (indicados en rojo). Esta configuración garantiza que la consulta se ejecuta según lo previsto.

![Período de tiempo](media/alert-log-troubleshoot/LogAlertTimePeriod.png)

### <a name="suppress-alerts-option-is-set"></a>Se ha establecido la opción Suprimir alertas

Como se describe en el paso 8 del artículo en [crear una regla de alerta de registro en el portal de Azure](../platform/alerts-log.md#managing-log-alerts-from-the-azure-portal), las alertas de registro proporcionan una **Suprimir alertas** opción para suprimir la notificación y desencadenar acciones para una cantidad configurada de tiempo. Como resultado, podría pensar que no se activa una alerta. De hecho, se han activado, pero se ha suprimido.  

![Suprimir alertas](media/alert-log-troubleshoot/LogAlertSuppress.png)

### <a name="metric-measurement-alert-rule-is-incorrect"></a>Regla de alertas de unidades métricas

*Las alertas del registro de unidades métricas* constituyen un subtipo de alertas del registro que tienen capacidades especiales y una sintaxis de consulta de alerta restringido. Una regla para una alerta de registro de unidades métricas requiere que el resultado sea una serie de métricas de tiempo de la consulta. Es decir, el resultado es una tabla con períodos de tiempo distintos, de igual tamaño junto con los valores agregados correspondientes. 

Puede elegir tener variables adicionales en la tabla junto con **AggregatedValue**. Estas variables se pueden usar para ordenar la tabla. 

Por ejemplo, supongamos que se ha configurado una regla para una alerta de registro de unidades métricas como:

- Consulta de `search *| summarize AggregatedValue = count() by $table, bin(timestamp, 1h)`  
- período de tiempo de 6 horas
- umbral de 50
- lógica de alerta de tres infracciones consecutivas
- **Agregado al** elegido como **$table**

Dado que el comando incluye **resumen... por** y proporciona dos variables (**timestamp** y **$table**), el sistema elegirá **$table** para **agregado tras** . El sistema ordena la tabla de resultados por la **$table** campo, como se muestra en la captura de pantalla siguiente. A continuación, examina el múltiplo **AggregatedValue** instancias para cada tipo de tabla (como **availabilityResults**) para ver si se han producido tres o más infracciones consecutivas.

![Ejecución de consultas de unidades métricas con varios valores](media/alert-log-troubleshoot/LogMMQuery.png)

Dado que **agregado tras** se define en **$table**, se ordenan los datos en un **$table** columna (indicado en rojo). A continuación, nos agrupar y buscar los tipos de la **agregado tras** campo. 

Por ejemplo, para **$table**, los valores de **availabilityResults** se considerará como un trazado o entidad (indicada en naranja). En este gráfico de valor o entidad, el servicio de alerta busca tres infracciones consecutivas (indicadas en verde). Las infracciones de desencadenan una alerta para el valor de la tabla **availabilityResults**. 

De forma similar, si se producen tres infracciones consecutivas para cualquier otro valor de **$table**, otra notificación de alerta se desencadena para lo mismo. El servicio de alertas automáticamente ordena los valores en un trazado o entidad (indicada en naranja) por hora.

Ahora suponga que se modificó la regla de la alerta de registro de unidades métricas y la consulta era `search *| summarize AggregatedValue = count() by bin(timestamp, 1h)`. El resto de la configuración continúan siendo el mismo que antes, incluida la lógica de alerta para tres infracciones consecutivas. El **agregado tras** en este caso es la opción **timestamp** de forma predeterminada. Se proporciona solo un valor en la consulta para **resumen... por** (es decir, **timestamp**). Al igual que el ejemplo anterior, la salida al final de la ejecución sería como se muestra como se indica a continuación.

   ![Ejecución de la consulta con el valor singular unidades métricas](media/alert-log-troubleshoot/LogMMtimestamp.png)

Porque **agregado tras** se define en **timestamp**, se ordenan los datos en el **marca de tiempo** columna (indicado en rojo). A continuación, agrupamos por **timestamp**. Por ejemplo, los valores de `2018-10-17T06:00:00Z` se considerará como un trazado o entidad (indicada en naranja). En este gráfico de valor o entidad, el servicio de alerta no encontrará ningún infracciones consecutivas (dado que cada **timestamp** valor tiene sólo una entrada). Por lo que nunca se desencadene la alerta. En tal caso, el usuario debe:

- Agregar una variable existente o una variable ficticia (como **$table**) para ordenar correctamente mediante el **agregado tras** campo.
- Volver a configurar la regla de alerta para usar la lógica de alerta según **total infracción** en su lugar.

## <a name="log-alert-fired-unnecessarily"></a>Alerta de registro activada innecesariamente

Un objeto configurado [regla de alerta de registro en Azure Monitor](../platform/alerts-log.md) podría desencadenarse inesperadamente al verla en [alertas de Azure](../platform/alerts-managing-alert-states.md). Las secciones siguientes describen algunas de las razones comunes.

### <a name="alert-triggered-by-partial-data"></a>Alerta desencadenada por datos parciales

Log Analytics y Application Insights están sujetos a procesamiento y retrasos de ingesta. Cuando se ejecuta una consulta de alerta de registro, es posible que encuentre que no hay datos disponibles o solo algunos datos están disponibles. Para obtener más información, consulte [tiempo de recopilación de datos de registro en Azure Monitor](../platform/data-ingestion-time.md).

Según cómo haya configurado la regla de alerta, hay puede ocurrir si no hay ningún datos o datos parciales en los registros en el momento de ejecución de alertas. En tales casos, le recomendamos que cambie la configuración o la consulta de alerta. 

Por ejemplo, si configura la regla de alerta de registro que se desencadene cuando el número de resultados de una consulta de analytics es menor que 5, la alerta se desencadena cuando no hay resultados parciales (un registro) o datos (cero registro). Pero después del retraso de ingesta de datos, la misma consulta con los datos completos podría proporcionar un resultado de 10 registros.

### <a name="alert-query-output-is-misunderstood"></a>No se entiende bien la salida de la consulta de alerta

La lógica de las alertas de registros se proporcionan en una consulta de Analytics. La consulta de analytics puede usar varias funciones matemáticas y big data. La consulta ejecuta el servicio de alertas en los intervalos especificados con los datos durante un período de tiempo especificado. El servicio de alertas realiza cambios sutiles en la consulta según el tipo de alerta. Puede ver este cambio en el **consulta que se ejecutará** sección en la **configurar lógica de señal** pantalla:

![Consulta que se ejecutará](media/alert-log-troubleshoot/LogAlertPreview.png)

El **consulta que se ejecutará** cuadro es lo que se ejecuta el servicio de alertas de registro. Si desea comprender lo que la consulta de alerta como resultado podría ser antes de crear la alerta, puede ejecutar la consulta indicada y el intervalo de tiempo a través de la [portal de Analytics](../log-query/portals.md) o [Analytics API](https://docs.microsoft.com/rest/api/loganalytics/).

## <a name="log-alert-was-disabled"></a>Se deshabilitó la alerta del registro

Las siguientes secciones enumeran algunos de los motivos por qué podría deshabilitar Azure Monitor el [regla de alerta de registro](../platform/alerts-log.md).

### <a name="resource-where-the-alert-was-created-no-longer-exists"></a>Recurso donde se creó la alerta ya no existe

Las reglas de alerta de registro creadas en Azure Monitor como destino un recurso específico, como un área de trabajo de Azure Log Analytics, una aplicación de Azure Application Insights y un recurso de Azure. El servicio de alertas de registro, a continuación, ejecutará una consulta de analytics proporcionada en la regla para el destino especificado. Pero después de la creación de reglas, los usuarios entran a menudo para eliminar de Azure, o mover dentro de Azure: el destino de la regla de alerta de registro. Dado que el destino de la regla de alerta ya no es válido, se produce un error en la ejecución de la regla.

En tales casos, Azure Monitor se deshabilita la alerta del registro y garantiza que no le cobra innecesariamente cuando la regla no se puede ejecutar continuamente durante un período considerable (por ejemplo, una semana). Puede averiguar la hora exacta cuando Azure Monitor deshabilitado a través de la alerta del registro [Azure Activity Log](../../azure-resource-manager/resource-group-audit.md). En el registro de actividad de Azure, se agrega un evento cuando Azure Monitor se deshabilita la regla de alerta de registro.

El siguiente evento de ejemplo en el registro de actividad de Azure es para una regla de alerta que se ha deshabilitado debido a un error continuo.

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

### <a name="query-used-in-a-log-alert-is-not-valid"></a>Consulta que se usa en una alerta de registro no es válido

Cada regla de alerta de registro creado en Azure Monitor como parte de su configuración debe especificar una consulta de analytics que se ejecutará periódicamente el servicio de alerta. La consulta de análisis podría tener la sintaxis correcta en el momento de creación de reglas o actualización. Pero a veces, durante un período de tiempo, la consulta proporcionada en la regla de alerta de registro puede desarrollar problemas de sintaxis y provocar un error en la ejecución de reglas. Algunas razones comunes por qué una consulta de analytics proporcionada en una regla de alerta de registro puede desarrollar errores son:

- La consulta se escribe en [ejecutar en varios recursos](../log-query/cross-workspace-query.md). Y ya no existen uno o varios de los recursos especificados.
- No ha habido ningún flujo de datos para la plataforma de análisis. El [ejecución de la consulta produce un error](https://dev.loganalytics.io/documentation/Using-the-API/Errors) porque no hay ningún dato para la consulta proporcionada.
- Los cambios en [lenguaje de consulta](https://docs.microsoft.com/azure/kusto/query/) incluyen un formato revisado para comandos y funciones. Por lo tanto, la consulta proporcionada anteriormente en una regla de alerta ya no es válida.

[Azure Advisor](../../advisor/advisor-overview.md) le advierte acerca de este comportamiento. Se agrega una recomendación para la regla de alerta de registro específico en Azure Advisor, bajo la categoría de alta disponibilidad con repercusión media y una descripción de "Reparar la regla de alerta de registro para asegurarse de supervisión". Si una consulta de alerta en la regla de alerta de registro no se rectifique después de que ha proporcionado Azure Advisor una recomendación durante siete días, Azure Monitor se deshabilita la alerta del registro y asegúrese de que no le cobra innecesariamente cuando la regla no se puede ejecutar continuamente para una considerable (período al igual que una semana).

Puede encontrar el tiempo exacto cuando Azure Monitor deshabilita la regla de alerta de registro, busque un evento en [Azure Activity Log](../../azure-resource-manager/resource-group-audit.md).

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre las [alertas de registro en Azure](../platform/alerts-unified-log.md).
- Más información sobre [Application Insights](../../azure-monitor/app/analytics.md).
- Obtenga más información sobre [registrar consultas](../log-query/log-query-overview.md).
