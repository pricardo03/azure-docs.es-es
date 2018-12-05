---
title: Análisis del uso de datos en Log Analytics | Microsoft Docs
description: Use el panel de uso y costo estimado en Log Analytics para evaluar cuántos datos se envían a Log Analytics e identificar los posibles motivos que podrían provocar aumentos imprevistos.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: 74d0adcb-4dc2-425e-8b62-c65537cef270
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/11/2018
ms.author: magoedte
ms.component: ''
ms.openlocfilehash: e702e1f5eb1816b007317765e4c9a9f88bb99bfd
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/30/2018
ms.locfileid: "52635431"
---
# <a name="analyze-data-usage-in-log-analytics"></a>Análisis del uso de datos en Log Analytics

> [!NOTE]
> En este artículo se describe cómo analizar el uso de datos de Log Analytics.  Consulte los artículos siguientes para obtener información relacionada.
> - En [Administración de los costos mediante el control del volumen de datos y la retención en Log Analytics](log-analytics-manage-cost-storage.md) se describe cómo controlar los costos al cambiar el período de retención de datos.
> - En [Supervisión del uso y costos estimados](../monitoring-and-diagnostics/monitoring-usage-and-estimated-costs.md) se describe cómo ver el uso y los costos estimados a través de varias características de supervisión de Azure para los distintos modelos de precios. También describe cómo cambiar el modelo de precios.

## <a name="understand-usage"></a>Descripción del uso

Utilice **Uso y costos estimados de Log Analytics** para revisar y analizar el uso de datos. Muestra la cantidad de datos que recopila cada solución, la cantidad de datos que se retienen y una estimación de los costos según la cantidad de datos ingeridos y cualquier retención adicional más allá de la cantidad incluida.

![Uso y costos estimados](media/log-analytics-usage/usage-estimated-cost-dashboard-01.png)<br>

Para explorar los datos más detalladamente, haga clic en el icono situado en el extremo superior derecho de cualquiera de los gráficos de la página **Uso y costos estimados**. Ahora puede trabajar con esta consulta para explorar más detalles sobre su uso.  

![Visualización de registros](media/log-analytics-usage/logs.png)<br>

## <a name="troubleshooting-why-usage-is-higher-than-expected"></a>Solución del problema de un uso mayor de lo esperado
Un mayor uso está provocado por una de estas causas o por ambas:
- Más datos de lo esperado enviados a Log Analytics
- Más nodos de lo esperado que envían datos a Log Analytics o algunos nodos envían más datos de lo normal

Veamos cómo podemos aprender más sobre ambas de estas causas. 

> [!NOTE]
> Algunos de los campos del tipo de datos de uso, aunque siguen en el esquema, han quedado en desuso y ya no se rellenan sus valores. Estos son **Computer**, además de los campos relacionados con la ingesta (**TotalBatches**, **BatchesWithinSla**, **BatchesOutsideSla**, **BatchesCapped** y **AverageProcessingTimeMs**.

### <a name="data-volume"></a>Volumen de datos 
En la página **Uso y costos estimados**, el gráfico *Ingesta de datos por solución*  muestra el volumen total de los datos enviados y la cantidad que envía cada solución. Esto le permite determinar tendencias tales como si el uso global de los datos (o el uso de una solución en particular) crece, permanece constante o disminuye. La consulta que se utiliza para generar esto es:

`Usage| where TimeGenerated > startofday(ago(31d))| where IsBillable == true
| summarize TotalVolumeGB = sum(Quantity) / 1024 by bin(TimeGenerated, 1d), Solution| render barchart`

Tenga en cuenta que la cláusula "where IsBillable = true" filtra los tipos de datos de determinadas soluciones para las que no hay ningún cargo de ingesta. 

Puede profundizar más para ver las tendencias de datos para tipos de datos específicos; por ejemplo, si desea estudiar los datos debidos a los registros de IIS:

`Usage| where TimeGenerated > startofday(ago(31d))| where IsBillable == true
| where DataType == "W3CIISLog"
| summarize TotalVolumeGB = sum(Quantity) / 1024 by bin(TimeGenerated, 1d), Solution| render barchart`

### <a name="nodes-sending-data"></a>Nodos que envían datos

Para comprender el número de nodos que notifican datos en el último mes, use:

`Heartbeat | where TimeGenerated > startofday(ago(31d))
| summarize dcount(ComputerIP) by bin(TimeGenerated, 1d)    
| render timechart`

Para ver el recuento de eventos ingeridos por equipo, use:

`union withsource = tt *
| summarize count() by Computer | sort by count_ nulls last`

Utilice esta consulta con moderación, ya que su ejecución es costosa. Para ver el recuento de eventos facturables ingeridos por equipo, use: 

`union withsource = tt * 
| where _IsBillable == true 
| summarize count() by Computer  | sort by count_ nulls last`

Si quiere ver qué tipos de datos facturables envían datos a un equipo específico, use:

`union withsource = tt *
| where Computer == "*computer name*"
| where _IsBillable == true 
| summarize count() by tt | sort by count_ nulls last `

Para profundizar en el origen de datos de un tipo de datos concreto, estas son algunas consultas de ejemplo útiles:

+ Solución **Security**
  - `SecurityEvent | summarize AggregatedValue = count() by EventID`
+ Solución **Log Management**
  - `Usage | where Solution == "LogManagement" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true | summarize AggregatedValue = count() by DataType`
+ Tipo de datos **Perf**
  - `Perf | summarize AggregatedValue = count() by CounterPath`
  - `Perf | summarize AggregatedValue = count() by CounterName`
+ Tipo de datos **Event**
  - `Event | summarize AggregatedValue = count() by EventID`
  - `Event | summarize AggregatedValue = count() by EventLog, EventLevelName`
+ Tipo de datos **Syslog**
  - `Syslog | summarize AggregatedValue = count() by Facility, SeverityLevel`
  - `Syslog | summarize AggregatedValue = count() by ProcessName`
+ Tipo de datos de **AzureDiagnostics**
  - `AzureDiagnostics | summarize AggregatedValue = count() by ResourceProvider, ResourceId`

### <a name="tips-for-reducing-data-volume"></a>Sugerencias para reducir el volumen de datos

Algunas sugerencias para reducir el volumen de registros recopilados incluyen:

| Origen del mayor volumen de datos | Cómo reducir el volumen de datos |
| -------------------------- | ------------------------- |
| Eventos de seguridad            | Seleccione los [eventos de seguridad común o mínima](https://blogs.technet.microsoft.com/msoms/2016/11/08/filter-the-security-events-the-oms-security-collects/). <br> Cambie la directiva de auditoría de seguridad para recopilar únicamente los eventos necesarios. En particular, revise la necesidad de recopilar eventos para <br> - [auditar plataforma de filtrado](https://technet.microsoft.com/library/dd772749(WS.10).aspx) <br> - [auditar registro](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd941614(v%3dws.10))<br> - [auditar sistema de archivos](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd772661(v%3dws.10))<br> - [auditar objeto de kernel](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd941615(v%3dws.10))<br> - [auditar manipulación de identificadores](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd772626(v%3dws.10))<br> - auditar almacenamiento extraíble |
| contadores de rendimiento       | Cambie la [configuración de los contadores de rendimiento](log-analytics-data-sources-performance-counters.md) para: <br> - Reducir la frecuencia de la colección <br> - Reducir el número de contadores de rendimiento |
| Registros de eventos                 | Cambie la [configuración del registro de eventos](log-analytics-data-sources-windows-events.md) para: <br> - Reducir el número de registros de eventos recopilados <br> - Recopilar solo los niveles de eventos necesarios Por ejemplo, no recopile eventos de nivel de *información*. |
| syslog                     | Cambie la [configuración de syslog](log-analytics-data-sources-syslog.md) para: <br> - Reducir el número de instalaciones recopiladas <br> - Recopilar solo los niveles de eventos necesarios Por ejemplo, no recopile eventos de nivel de *información* y *depuración*. |
| AzureDiagnostics           | Cambie la colección de registros de recursos para: <br> - Reducir el número de registros de recursos enviados a Log Analytics <br> - Recopilar solo los registros necesarios |
| Datos de la solución procedentes de equipos que no necesitan la solución | Use la [selección de destino de solución](../azure-monitor/insights/solution-targeting.md) para recopilar datos solo de los grupos de equipos necesarios. |

### <a name="getting-node-counts"></a>Obtención de recuentos de nodo 

Si se encuentra en el plan de tarifa "Por nodo (OMS)", se le cobrará en función del número de nodos y soluciones que utilice y el número de nodos de Insight and Analytics por los que se le factura se mostrará en la tabla de la página **Uso y costos estimados**.  

Para ver el número de nodos de seguridad distintos, puede utilizar la consulta:

`union
(
    Heartbeat
    | where (Solutions has 'security' or Solutions has 'antimalware' or Solutions has 'securitycenter')
    | project Computer
),
(
    ProtectionStatus
    | where Computer !in~
    (
        (
            Heartbeat
            | project Computer
        )
    )
    | project Computer
)
| distinct Computer
| project lowComputer = tolower(Computer)
| distinct lowComputer
| count`

Para ver el número de nodos de Automation distintos, utilice la consulta:

` ConfigurationData 
 | where (ConfigDataType == "WindowsServices" or ConfigDataType == "Software" or ConfigDataType =="Daemons") 
 | extend lowComputer = tolower(Computer) | summarize by lowComputer 
 | join (
     Heartbeat 
       | where SCAgentChannel == "Direct"
       | extend lowComputer = tolower(Computer) | summarize by lowComputer, ComputerEnvironment
 ) on lowComputer
 | summarize count() by ComputerEnvironment | sort by ComputerEnvironment asc`

## <a name="create-an-alert-when-data-collection-is-higher-than-expected"></a>Creación de una alerta cuando la colección de datos es mayor de lo esperado
En esta sección se describe cómo crear una alerta si:
- El volumen de datos supera una cantidad especificada.
- Se prevé que el volumen de datos supere un importe especificado.

Alertas de Azure admite [alertas de registro](../monitoring-and-diagnostics/monitor-alerts-unified-log.md) que usen consultas de búsqueda. 

La consulta siguiente produce un resultado cuando hay más de 100 GB de datos recopilados en las últimas 24 horas:

`union withsource = $table Usage | where QuantityUnit == "MBytes" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true | extend Type = $table | summarize DataGB = sum((Quantity / 1024)) by Type | where DataGB > 100`

La consulta siguiente utiliza una fórmula simple para predecir cuándo se enviarán más de 100 GB de datos en un día: 

`union withsource = $table Usage | where QuantityUnit == "MBytes" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true | extend Type = $table | summarize EstimatedGB = sum(((Quantity * 8) / 1024)) by Type | where EstimatedGB > 100`

Para generar una alerta en un volumen de datos diferente, cambie el 100 de las consultas por el número de GB sobre el que desea alertar.

Siga los pasos explicados en [crear una nueva regla de alerta](../monitoring-and-diagnostics/alert-metric.md) para recibir una notificación cuando la colección de datos sea mayor de lo previsto.

Al crear la alerta en la primera consulta; cuando hay más de 100 GB de datos en 24 horas, establezca los siguientes valores:  

- **Definición de la condición de alerta**: especifique el área de trabajo de Log Analytics como el destino del recurso.
- **Criterios de alerta** especifique lo siguiente:
   - **Nombre de señal**: seleccione **Custom log search** (Búsqueda de registros personalizada)
   - **Consulta de búsqueda** en `union withsource = $table Usage | where QuantityUnit == "MBytes" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true | extend Type = $table | summarize DataGB = sum((Quantity / 1024)) by Type | where DataGB > 100`
   - **Lógica de alerta** está **basada en** *número de resultados* y **Condición** es *Mayor que* un **umbral**  de *0*
   - **Período de tiempo** de *1440* minutos y **Frecuencia de la alerta** cada *60* minutos ya que los datos de uso solo se actualizan una vez por hora.
- **Definición de los detalles de la alerta**: especifique lo siguiente:
   - **Nombre** en *Volumen de datos mayor que 100 GB en 24 horas*
   - **Gravedad** en *Advertencia*

Especifique un [grupo de acciones](../monitoring-and-diagnostics/monitoring-action-groups.md) existente o cree uno nuevo para que cuando la alerta de registro coincida con criterios, se le notifique.

Al crear la alerta para la segunda consulta; cuando se prevé que va a haber más de 100 GB de datos en 24 horas, establezca los siguientes valores:

- **Definición de la condición de alerta**: especifique el área de trabajo de Log Analytics como el destino del recurso.
- **Criterios de alerta** especifique lo siguiente:
   - **Nombre de señal**: seleccione **Custom log search** (Búsqueda de registros personalizada)
   - **Consulta de búsqueda** en `union withsource = $table Usage | where QuantityUnit == "MBytes" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true | extend Type = $table | summarize EstimatedGB = sum(((Quantity * 8) / 1024)) by Type | where EstimatedGB > 100`
   - **Lógica de alerta** está **basada en** *número de resultados* y **Condición** es *Mayor que* un **umbral**  de *0*
   - **Período de tiempo** de *180* minutos y **Frecuencia de la alerta** cada *60* minutos ya que los datos de uso solo se actualizan una vez por hora.
- **Definición de los detalles de la alerta**: especifique lo siguiente:
   - **Nombre** en *Volumen de datos que se espera que sea mayor que 100 GB en 24 horas*
   - **Gravedad** en *Advertencia*

Especifique un [grupo de acciones](../monitoring-and-diagnostics/monitoring-action-groups.md) existente o cree uno nuevo para que cuando la alerta de registro coincida con criterios, se le notifique.

Cuando se recibe una alerta, siga los pasos de la sección siguiente para solucionar el problema del uso mayor de lo esperado.

## <a name="next-steps"></a>Pasos siguientes
* Consulte [Búsquedas de registro en Log Analytics](log-analytics-queries.md) para obtener información sobre cómo usar el lenguaje de búsqueda. Puede utilizar las consultas de búsqueda para realizar análisis adicionales sobre los datos de uso.
* Siga los pasos explicados en [Crear una nueva alerta de registro](../monitoring-and-diagnostics/alert-metric.md) para recibir una notificación cuando se cumplan los criterios de búsqueda.
* Use la [selección de destino de solución](../azure-monitor/insights/solution-targeting.md) para recopilar datos solo de los grupos de equipos necesarios.
* Para configurar una directiva eficaz de recopilación de eventos de seguridad, revise la [Directiva de filtrado de Azure Security Center](../security-center/security-center-enable-data-collection.md).
* Cambie la [configuración de los contadores de rendimiento](log-analytics-data-sources-performance-counters.md).
* Para modificar la configuración de recopilación de eventos, revise la [configuración de registros de eventos](log-analytics-data-sources-windows-events.md).
* Para modificar la configuración de la recopilación de syslog, revise la [configuración de syslog](log-analytics-data-sources-syslog.md).
