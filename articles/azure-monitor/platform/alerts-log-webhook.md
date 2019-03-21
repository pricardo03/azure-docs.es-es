---
title: Acciones de webhook para alertas de registro en alertas de Azure
description: En este artículo se describe cómo una regla de alertas de registro puede insertar datos como webhook de HTTP mediante Log Analytics, así como los detalles de las diferentes personalizaciones posibles.
author: msvijayn
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: vinagara
ms.subservice: alerts
ms.openlocfilehash: 908422927feabd156c5dcdc7a04d44ff8fc42094
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57442892"
---
# <a name="webhook-actions-for-log-alert-rules"></a>Acciones de webhook para reglas de alertas de registro
Cuando se [crea una alerta de registro en Azure](alerts-log.md), tiene la opción de [configurarla mediante grupos de acciones](action-groups.md), para así poder realizar una o varias acciones.  En este artículo se describen las diferentes acciones de webhook que están disponibles y los detalles sobre la configuración de los webhook personalizados basados en JSON.


## <a name="webhook-actions"></a>Acciones de webhook

Las acciones de Webhook permiten invocar un proceso externo a través de una sola solicitud HTTP POST.  El servicio que se llama debe admitir webhooks y determinar cómo usa cualquier carga que recibe.    

Las acciones de webhook requieren las propiedades de la siguiente tabla:

| Propiedad | DESCRIPCIÓN |
|:--- |:--- |
| Dirección URL de Webhook |La dirección URL del webhook. |
| Carga de JSON personalizada |Carga personalizada para enviar con el webhook, cuando esta opción se elige durante la creación de la alerta. Conozca más detalles en el artículo sobre la [administración de alertas de registro](alerts-log.md) |

> [!NOTE]
> El botón Ver webhook y la opción *Incluir la carga personalizada de JSON para webhook* de la alerta del registro mostrarán una carga de webhook de ejemplo con la personalización proporcionada. No contiene datos reales ni representativos del esquema JSON que se usen en las alertas de registro. 

Los webhooks incluyen una dirección URL y una carga en formato JSON que son los datos enviados al servicio externo.  De forma predeterminada, la carga incluye los valores de la tabla siguiente:  Puede optar por reemplazar esta carga con una personalizada de su propiedad.  En ese caso, puede utilizar las variables de la tabla para cada uno de los parámetros para incluir su valor en la carga personalizada.


| Parámetro | Variable | DESCRIPCIÓN |
|:--- |:--- |:--- |
| AlertRuleName |#alertrulename |Nombre de la regla de alerta. |
| Gravedad |#severity |Gravedad establecida en la alerta de registros activada. |
| AlertThresholdOperator |ThresholdOperator |Operador de umbral para la regla de alerta.  *Mayor que* o *menor que*. |
| AlertThresholdValue |#thresholdvalue |Valor de umbral para la regla de alerta. |
| LinkToSearchResults |#linktosearchresults |Vincular al portal de análisis que devuelve los registros de la consulta que creó la alerta. |
| ResultCount |#searchresultcount |Número de registros en los resultados de la búsqueda. |
| Search Interval End time |#searchintervalendtimeutc |Hora de finalización de la consulta en UTC, formato - mm/dd/aaaa HH:mm:ss AM/PM. |
| Search Interval |#searchinterval |Período de tiempo para la regla de alertas, formato - HH:mm:ss. |
| Search Interval StartTime |#searchintervalstarttimeutc |Hora de inicio de la consulta en UTC, formato - mm/dd/aaaa HH:mm:ss AM/PM. 
| SearchQuery |#searchquery |Consulta de búsqueda de registros utilizada por la regla de alerta. |
| SearchResults |"IncludeSearchResults": true|Son los registros que devuelve la consulta a modo de tabla de JSON y que están limitados a los primeros 1000 registros, si el elemento "IncludeSearchResults":true se agrega a una definición de webhook personalizada de JSON como propiedad de alto nivel. |
| WorkspaceID |#workspaceid |Identificador del área de trabajo de Log Analytics. |
| Identificador de aplicación |#applicationid |Identificador de la aplicación Application Insights. |
| Id. de suscripción |#subscriptionid |Identificador de la suscripción de Azure que se usa con Application Insights. 

> [!NOTE]
> LinkToSearchResults pasa parámetros como SearchQuery, Search Interval StartTime y Search Interval End time en la dirección URL a Azure Portal para su visualización en la sección de Análisis. Portal de Azure tiene el URI de límite de aproximadamente 2000 caracteres de tamaño y le *no* abrir el vínculo proporcionado en las alertas, si los valores de parámetros superan el límite mencionado. Los usuarios pueden escribir manualmente los detalles para ver los resultados en el portal de Analytics o usar [Analytics REST API](https://dev.applicationinsights.io/documentation/Using-the-API) o [Log Analytics REST API](https://dev.loganalytics.io/reference) de Application Insights para recuperar los resultados mediante programación 

Por ejemplo, podría especificar la siguiente carga personalizada que incluye un único parámetro denominado *text*.  El servicio al que llama este webhook estaría esperando este parámetro.

```json

    {
        "text":"#alertrulename fired with #searchresultcount over threshold of #thresholdvalue."
    }
```
Esta carga de ejemplo se resolvería en algo similar a lo siguiente al enviarse al webhook.

```json
    {
        "text":"My Alert Rule fired with 18 records over threshold of 10 ."
    }
```
Como todas las variables de un webhook personalizado están especificadas dentro de un contenedor JSON como "#searchinterval", el webhook resultante también tendrá datos de variable dentro de un contenedor como "00:05:00".

Para incluir resultados de búsqueda en una carga personalizada, asegúrese de que **IncudeSearchResults** se establece como una propiedad de nivel superior en la carga de JSON. 

## <a name="sample-payloads"></a>Cargas de ejemplo
En esta sección se muestra la carga de ejemplo de webhook para las alertas del registro, incluso cuando la carga es estándar y se ha personalizado.

### <a name="standard-webhook-for-log-alerts"></a>Webhook estándar de las alertas de registro 
En ambos ejemplos se indica una carga ficticia con solo dos columnas y dos filas.

#### <a name="log-alert-for-azure-log-analytics"></a>Alerta de registro de Azure Log-Analytics
A continuación, se muestra una carga de ejemplo de una acción de webhook estándar *sin la opción JSON personalizada* cuando se usa en las alertas basadas en Log Analytics.

```json
{
    "WorkspaceId":"12345a-1234b-123c-123d-12345678e",
    "AlertRuleName":"AcmeRule","SearchQuery":"search *",
    "SearchResult":
        {
        "tables":[
                    {"name":"PrimaryResult","columns":
                        [
                        {"name":"$table","type":"string"},
                        {"name":"Id","type":"string"},
                        {"name":"TimeGenerated","type":"datetime"}
                        ],
                    "rows":
                        [
                            ["Fabrikam","33446677a","2018-02-02T15:03:12.18Z"],
                            ["Contoso","33445566b","2018-02-02T15:16:53.932Z"]
                        ]
                    }
                ]
        },
    "SearchIntervalStartTimeUtc": "2018-03-26T08:10:40Z",
    "SearchIntervalEndtimeUtc": "2018-03-26T09:10:40Z",
    "AlertThresholdOperator": "Greater Than",
    "AlertThresholdValue": 0,
    "ResultCount": 2,
    "SearchIntervalInSeconds": 3600,
    "LinkToSearchResults": "https://workspaceID.portal.mms.microsoft.com/#Workspace/search/index?_timeInterval.intervalEnd=2018-03-26T09%3a10%3a40.0000000Z&_timeInterval.intervalDuration=3600&q=Usage",
    "Description": null,
    "Severity": "Warning"
 }
 ```

> [!NOTE]
> Puede cambiar el valor del campo de gravedad si tienes [cambiar sus preferencias de la API](alerts-log-api-switch.md) para alertas de registro en Log Analytics.


#### <a name="log-alert-for-azure-application-insights"></a>Alerta de registro de Azure Application Insights
A continuación, se muestra una carga de ejemplo de un webhook estándar *sin la opción JSON personalizada* cuando se usa en las alertas de registro basadas en Application Insights.
    
```json
{
    "schemaId":"Microsoft.Insights/LogAlert","data":
    { 
    "SubscriptionId":"12345a-1234b-123c-123d-12345678e",
    "AlertRuleName":"AcmeRule","SearchQuery":"search *",
    "SearchResult":
        {
        "tables":[
                    {"name":"PrimaryResult","columns":
                        [
                        {"name":"$table","type":"string"},
                        {"name":"Id","type":"string"},
                        {"name":"TimeGenerated","type":"datetime"}
                        ],
                    "rows":
                        [
                            ["Fabrikam","33446677a","2018-02-02T15:03:12.18Z"],
                            ["Contoso","33445566b","2018-02-02T15:16:53.932Z"]
                        ]
                    }
                ]
        },
    "SearchIntervalStartTimeUtc": "2018-03-26T08:10:40Z",
    "SearchIntervalEndtimeUtc": "2018-03-26T09:10:40Z",
    "AlertThresholdOperator": "Greater Than",
    "AlertThresholdValue": 0,
    "ResultCount": 2,
    "SearchIntervalInSeconds": 3600,
    "LinkToSearchResults": "https://analytics.applicationinsights.io/subscriptions/12345a-1234b-123c-123d-12345678e/?query=search+*+&timeInterval.intervalEnd=2018-03-26T09%3a10%3a40.0000000Z&_timeInterval.intervalDuration=3600&q=Usage",
    "Description": null,
    "Severity": "3",
    "ApplicationId": "123123f0-01d3-12ab-123f-abc1ab01c0a1"
    }
}
```

#### <a name="log-alert-with-custom-json-payload"></a>Alerta de registro con carga de JSON personalizada
Por ejemplo, para crear una carga personalizada que incluya solo el nombre de la alerta y los resultados de la búsqueda, puede usar lo siguiente: 

```json
    {
       "alertname":"#alertrulename",
       "IncludeSearchResults":true
    }
```

A continuación se muestra una carga de ejemplo para una acción de webhook personalizada para cualquier alerta de registro.
    
```json
    {
    "alertname":"AcmeRule","IncludeSearchResults":true,
    "SearchResult":
        {
        "tables":[
                    {"name":"PrimaryResult","columns":
                        [
                        {"name":"$table","type":"string"},
                        {"name":"Id","type":"string"},
                        {"name":"TimeGenerated","type":"datetime"}
                        ],
                    "rows":
                        [
                            ["Fabrikam","33446677a","2018-02-02T15:03:12.18Z"],
                            ["Contoso","33445566b","2018-02-02T15:16:53.932Z"]
                        ]
                    }
                ]
        }
    }
```


## <a name="next-steps"></a>Pasos siguientes
- Más información sobre las [alertas de registro en las alertas de Azure](alerts-unified-log.md).
- Información sobre la [administración de alertas de registro en Azure](alerts-log.md)
- Crear y administrar [grupos de acciones en Azure](action-groups.md)
- Más información sobre [Application Insights](../../azure-monitor/app/analytics.md)
- Más información sobre [Log Analytics](../../azure-monitor/log-query/log-query-overview.md). 

