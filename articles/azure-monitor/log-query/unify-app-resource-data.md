---
title: Unificación de varios recursos de Application Insights de Azure Monitor | Microsoft Docs
description: En este artículo se proporcionan detalles sobre cómo usar una función en registros de Azure Monitor para consultar varios recursos de Application Insights y visualizar los datos.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.service: azure-monitor
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: magoedte
ms.openlocfilehash: e3b118306b5a139ba31029bc6191368690b36666
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2019
ms.locfileid: "54265216"
---
# <a name="unify-multiple-azure-monitor-application-insights-resources"></a>Unificación de varios recursos de Application Insights de Azure Monitor 
En este artículo se describe cómo consultar y ver todos los datos de registro de aplicación de Application Insights en un mismo lugar, incluso cuando están en diferentes suscripciones de Azure, como un reemplazo para Application Insights Connector en desuso.  

## <a name="recommended-approach-to-query-multiple-application-insights-resources"></a>Enfoque recomendado para consultar varios recursos de Application Insights 
Enumerar varios recursos de Application Insights en una consulta puede ser pesado y difícil de mantener. En su lugar, puede aprovechar la función para separar la lógica de consulta del ámbito de las aplicaciones.  

En este ejemplo se muestra cómo puede supervisar varios recursos de Application Insights y visualizar el número de solicitudes erróneas por nombre de la aplicación. Antes de comenzar, ejecute esta consulta en el área de trabajo que está conectada a los recursos de Application Insights para obtener la lista de aplicaciones conectadas: 

```
ApplicationInsights
| summarize by ApplicationName
```

Cree una función mediante el operador de unión con la lista de aplicaciones, luego guarde la consulta como función con el alias *applicationsScoping*.  

```
union withsource=SourceApp 
app('Contoso-app1').requests,  
app('Contoso-app2').requests, 
app('Contoso-app3').requests, 
app('Contoso-app4').requests, 
app('Contoso-app5').requests 
| parse SourceApp with * "('" applicationName "')" *  
```

>[!NOTE]
>Puede modificar las aplicaciones enumeradas en cualquier momento; para ello, vaya al explorador de consultas en el portal de registros y edite la función o use el cmdlet `SavedSearch` de PowerShell. El comando `withsource= SourceApp` agrega una columna a los resultados, que designa la aplicación que envió el registro. 
>
>La consulta usa el esquema de Application Insights, aunque la consulta se ejecuta en el área de trabajo debido a que la función applicationsScoping devuelve la estructura de datos de Application Insights. 
>
>El operador de análisis es opcional en este ejemplo, extrae el nombre de la aplicación de la propiedad SourceApp. 

Ahora está listo para usar la función applicationsScoping en la consulta entre recursos. El alias de la función devuelve la unión de las solicitudes de todas las aplicaciones definidas. A continuación, la consulta filtra las solicitudes erróneas y visualiza las tendencias por aplicación. ![Ejemplo de resultados entre consultas](media/unify-app-resource-data/app-insights-query-results.png)

## <a name="query-across-application-insights-resources-and-workspace-data"></a>Consulta entre recursos de Application Insights y datos del área de trabajo 
Cuando detiene el conector y tiene que realizar consultas durante un intervalo de tiempo que se ha recortado por la retención de datos de Application Insights (90 días), deberá realizar [consultas entre recursos](../../azure-monitor/log-query/cross-workspace-query.md) en el área de trabajo y los recursos de Application Insights durante un período intermedio. Esto es hasta que los datos de las aplicaciones se acumulen según la nueva retención de datos de Application Insights mencionada anteriormente. La consulta requiere ciertas manipulaciones, ya que los esquemas de Application Insights y del área de trabajo son diferentes. Consulte la tabla más adelante en esta sección que destaca las diferencias en los esquemas. 

Por ejemplo, si el conector ha dejado de funcionar el 01/11/2018, al consultar los registros entre los recursos de Application Insights y los datos de aplicaciones en el área de trabajo, la consulta se podría construir de manera similar al ejemplo siguiente:

```
applicationsScoping //this brings data from Application Insights resources 
| where timestamp between (datetime("2018-11-01") .. now()) 
| where success == 'False' 
| where duration > 1000 
| union ( 
    ApplicationInsights //this is Application Insights data in Log Analytics workspace 
    | where TimeGenerated < (datetime("2018-12-01") 
    | where RequestSuccess == 'False' 
    | where RequestDuration > 1000 
    | extend duration = RequestDuration //align to Application Insights schema 
    | extend timestamp = TimeGenerated //align to Application Insights schema 
    | extend name = RequestName //align to Application Insights schema 
    | extend resultCode = ResponseCode //align to Application Insights schema 
    | project-away RequestDuration , RequestName , ResponseCode , TimeGenerated 
) 
| project timestamp , duration , name , resultCode 
```

## <a name="application-insights-and-log-analytics-workspace-schema-differences"></a>Diferencias de esquema del área de trabajo de Log Analytics y Application Insights
En la tabla siguiente se muestran las diferencias de esquema entre Log Analytics y Application Insights.  

| Propiedades del área de trabajo de Log Analytics| Propiedades de recursos de Application Insights|
|------------|------------| 
| AnonUserId | user_id|
| ApplicationId | appId|
| ApplicationName | appName|
| ApplicationTypeVersion | application_Version |
| AvailabilityCount | itemCount |
| AvailabilityDuration | duration |
| AvailabilityMessage | Mensaje |
| AvailabilityRunLocation | location |
| AvailabilityTestId | id |
| AvailabilityTestName | Nombre |
| AvailabilityTimestamp |  timestamp |
| Browser | client_browser |
| City | client_city |
| ClientIP | client_IP |
| Equipo | cloud_RoleInstance | 
| País | client_CountryOrRegion | 
| CustomEventCount | itemCount | 
| CustomEventDimensions | customDimensions |
| CustomEventName | Nombre | 
| DeviceModel | client_Model | 
| DeviceType | client_Type | 
| ExceptionCount | itemCount | 
| ExceptionHandledAt | handledAt |
| ExceptionMessage | Mensaje | 
| ExceptionType | Tipo |
| OperationID | operation_id |
| nombreOperación | operation_Name | 
| SO | client_OS | 
| PageViewCount | itemCount |
| PageViewDuration | duration | 
| PageViewName | Nombre | 
| ParentOperationID | operation_Id | 
| RequestCount | itemCount | 
| RequestDuration | duration | 
| RequestID | id | 
| RequestName | Nombre | 
| RequestSuccess | Correcto | 
| ResponseCode | resultCode | 
| Rol | cloud_RoleName |
| RoleInstance | cloud_RoleInstance |
| SessionId | session_Id | 
| SourceSystem | operation_SyntheticSource |
| TelemetryTYpe | Tipo |
| URL | _url |
| UserAccountId | user_AccountId |

## <a name="next-steps"></a>Pasos siguientes

Use [Búsqueda de registros](../../azure-monitor/log-query/log-query-overview.md) para ver información detallada de las aplicaciones de Application Insights.