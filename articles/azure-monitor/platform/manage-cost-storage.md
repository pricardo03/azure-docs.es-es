---
title: Administración del uso y los costos de Azure Log Analytics | Microsoft Docs
description: Aprenda cómo cambiar el plan de precios y cómo administrar el volumen de datos y la directiva de retención para el área de trabajo de Log Analytics en Azure.
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 03/29/2018
ms.author: magoedte
ms.subservice: ''
ms.openlocfilehash: a2f90c52823664df5fdc71c55220cc660c2f68e3
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "58878152"
---
# <a name="manage-usage-and-costs-for-log-analytics-in-azure-monitor"></a>Administrar el uso y costos de Log Analytics en Azure Monitor

> [!NOTE]
> En este artículo se describe cómo controlar los costos en Log Analytics al establecer el período de retención de datos.  Consulte los artículos siguientes para obtener información relacionada.
> - En [Análisis del uso de datos en Log Analytics](manage-cost-storage.md) se describe cómo analizar y alertar sobre el uso de datos.
> - En [Supervisión del uso y costos estimados](usage-estimated-costs.md) se describe cómo ver el uso y los costos estimados a través de varias características de supervisión de Azure para los distintos modelos de precios. También describe cómo cambiar el modelo de precios.

Log Analytics en Azure Monitor está diseñado para escalar y admitir la recopilación, indexación y almacenar grandes cantidades de datos al día de cualquier origen de su empresa o implementados en Azure.  Aunque esto puede ser un factor clave principal para su organización, la rentabilidad es en última instancia el factor clave subyacente. Con este fin, es importante comprender que el costo de un área de trabajo de Log Analytics no se basa simplemente en el volumen de datos recopilados, también depende del plan seleccionado y de cuánto tiempo se decida almacenar los datos generados a partir de los orígenes conectados.  

En este artículo revisamos cómo puede supervisar de forma proactiva el crecimiento del volumen y del almacenamiento de datos, y definir los límites para controlar esos costos asociados. 

El costo de datos puede ser considerable dependiendo de los factores siguientes: 

- Volumen de datos generado e ingerido en el área de trabajo 
    - Número de soluciones de administración habilitadas
    - Número de sistemas supervisados
    - Tipo de datos recopilados de cada recurso supervisado 
- El período de tiempo durante el que decide retener los datos 

## <a name="understand-your-workspaces-usage-and-estimated-cost"></a>Comprenda el uso y los costos estimados de su área de trabajo
Log Analytics permite entender fácilmente lo que los costos probablemente vayan a ser, en base a los patrones de uso reciente.  Para ello, utilice **Uso y costos estimados de Log Analytics** a fin de revisar y analizar el uso de datos. Muestra la cantidad de datos que recopila cada solución, la cantidad de datos que se retienen y una estimación de los costos según la cantidad de datos ingeridos y cualquier retención adicional más allá de la cantidad incluida.

![Uso y costos estimados](media/manage-cost-storage/usage-estimated-cost-dashboard-01.png)

Para explorar los datos más detalladamente, haga clic en el icono situado en el extremo superior derecho de cualquiera de los gráficos de la página **Uso y costos estimados**. Ahora puede trabajar con esta consulta para explorar más detalles sobre su uso.  

![Visualización de registros](media/manage-cost-storage/logs.png)

En la página **Uso y costos estimados** puede revisar el volumen de datos del mes. Esto incluye todos los datos recibidos y retenidos en el área de trabajo de Log Analytics.  Haga clic en **Detalles de uso** en la parte superior de la página para ver el panel de uso con información sobre las tendencias de volumen de datos por origen, equipos y oferta. Para ver y establecer un límite diario o para modificar el período de retención, haga clic en **Administración del volumen de datos**.
 
Los cargos de Log Analytics se agregarán a la factura de Azure. Puede consultar los detalles de su factura de Azure en la sección de facturación de Azure Portal o en el [Portal de facturación de Azure](https://account.windowsazure.com/Subscriptions).  

## <a name="daily-cap"></a>Límite diario
Puede configurar un límite diario y limitar la ingesta diaria para el área de trabajo, pero tenga cuidado de establecer el límite diario como el objetivo al que llegar.  En caso contrario, perderá los datos para el resto del día, lo que puede afectar a otros servicios y soluciones de Azure cuya funcionalidad puede depender de la disponibilidad de datos actualizados en el área de trabajo.  Como resultado, esto afecta a la capacidad de observar y recibir alertas cuando cambian las condiciones de mantenimiento de los recursos que respaldan los servicios de TI.  El límite diario está pensado para usarse como una manera de administrar el aumento inesperado del volumen de datos de los recursos administrados y permanecer dentro de su límite, o simplemente cuando desee limitar cargos no planeados para el área de trabajo.  

Cuando se alcanza el límite diario, la recopilación de tipos de datos facturables se detiene durante el resto del día. Un mensaje emergente de advertencia aparece en la parte superior de la página del área de trabajo de Log Analytics seleccionada, y se envía un evento de operación para la tabla *Operación* en la categoría **LogManagement**. La recopilación de datos se reanuda después de que se restablezca el tiempo definido en *Daily limit will be set at* (El límite diario se establecerá en). Se recomienda definir una regla de alerta en función de este evento de operación que esté configurada para notificar cuando se ha alcanzado el límite diario de datos. 

### <a name="identify-what-daily-data-limit-to-define"></a>Identificación del límite diario de datos para definir 
Revise el [uso de Log Analytics y costos estimados](usage-estimated-costs.md) para comprender la tendencia de ingesta de datos y cuál es el límite de volumen diario para definir. Se debe considerar con cuidado, ya que no podrá supervisar los recursos una vez que se alcance el límite. 

### <a name="manage-the-maximum-daily-data-volume"></a>Administración del volumen de datos diario máximo 
Los pasos siguientes describen cómo configurar un límite para administrar el volumen de datos que Log Analytics ingiere por día.  

1. En el área de trabajo seleccione **Usage and estimated costs** (Uso y costos estimados) en el panel izquierdo.
2. En la página **Usage and estimated costs** (Uso y costos estimados) del área de trabajo seleccionada, haga clic en **Administración del volumen de datos** en la parte superior de la página. 
3. El límite diario está en **OFF** (Desactivado) de manera predeterminada, haga clic en **ON** (Activado) para habilitarlo y luego establezca el límite de volumen de datos en GB/día.<br><br> ![Configuración del límite de datos con Log Analytics](media/manage-cost-storage/set-daily-volume-cap-01.png)

### <a name="alert-when-daily-cap-reached"></a>Envía una alerta cuando se alcanza el límite diario
Aunque Azure Portal presenta una indicación visual cuando se alcanza el umbral de límite de datos, este comportamiento no tiene que alinearse necesariamente con la forma en la que el usuario administra los problemas de funcionamiento que requieren atención inmediata.  Para recibir una notificación de alerta, puede crear una nueva regla de alerta en Azure Monitor.  Para más información, consulte [cómo crear, ver y administrar alertas](alerts-metric.md).      

Para comenzar, esta es la configuración recomendada para la alerta:

* Destino: seleccione el recurso de Log Analytics
* Criterios: 
   * Nombre de señal: Búsqueda de registros personalizada
   * Consulta de búsqueda: Operación | donde Detalle tiene "OverQuota"
   * Basado en: Número de resultados
   * Condición: Mayor que
   * Umbral: 0
   * Período: 5 (minutos)
   * Frecuencia: 5 (minutos)
* Nombre de regla de alertas: Límite de datos diario alcanzado
* Gravedad: advertencia (gravedad 1)

Una vez que se define la alerta y se alcanza el límite, se desencadena una alerta que realiza la respuesta que se define en el grupo de acciones. Puede notificar a su equipo a través de mensajes de correo electrónico y de texto, o automatizar acciones mediante webhooks, runbooks de Automation o [integrar con una solución de ITSM externa](itsmc-overview.md#create-itsm-work-items-from-azure-alerts). 

## <a name="change-the-data-retention-period"></a>Cambio del período de retención de datos 
Los pasos siguientes describen cómo configurar cuánto tiempo se conservan los datos de registro en el área de trabajo.
 
1. En el área de trabajo seleccione **Usage and estimated costs** (Uso y costos estimados) en el panel izquierdo.
2. En la página **Usage and estimated costs** (Uso y costos estimados), haga clic en **Administración del volumen de datos** en la parte superior de la página.
5. En el panel, mueva el control deslizante para aumentar o disminuir el número de días y, luego, haga clic en **Aceptar**.  Si el nivel es *gratuito*, no podrá modificar el período de retención de datos y tendrá que actualizar al nivel de pago para controlar esta configuración.<br><br> ![Cambio de la configuración de retención de datos del área de trabajo](media/manage-cost-storage/manage-cost-change-retention-01.png)

## <a name="legacy-pricing-tiers"></a>Planes de tarifa heredados

Los clientes con un contrato Enterprise firmado antes del 1 de julio de 2018 o que ya crearon un área de trabajo de Log Analytics en una suscripción, seguirán teniendo acceso al plan *Gratis*. Si su suscripción no está asociado a una inscripción de EA existente, el nivel *Gratis* no está disponible cuando se crea un área de trabajo en una suscripción nueva después del 2 de abril de 2018.  En un plan *Gratis*, los datos tienen una retención limitada de siete días.  Para los planes de tarifa *independientes* o *por nodo* heredados, así como el plan de tarifa único y actual de 2018, los datos recopilados están disponibles durante los últimos 31 días. El plan *Gratis* tiene 500 MB de límite de ingesta diaria; si observa que supera constantemente el volumen permitido, puede cambiar el área de trabajo a otro plan de pago para recopilar datos más allá de este límite. 

> [!NOTE]
> Para usar los derechos que proceden de la adquisición de OMS E1 Suite, OMS E2 Suite o un complemento de OMS para System Center, elija el plan de tarifa *Por nodo* de Log Analytics.

## <a name="changing-pricing-tier"></a>Actualización del plan de tarifa

Si el área de trabajo de Log Analytics tiene acceso a los planes de tarifa heredados, siga los pasos a continuación para cambiar entre ellos:

1. En Azure Portal, en el panel de suscripciones de Log Analytics, seleccione un área de trabajo.

2. En el panel de área de trabajo, en **General**, seleccione **Plan de tarifa**.  

3. En **Plan de tarifa**, seleccione un plan y haga clic en **Seleccionar**.  
    ![Plan de precios seleccionado](media/manage-cost-storage/workspace-pricing-tier-info.png)

Si desea mover el área de trabajo al plan de tarifa actual, deberá [cambiar el modelo de precios de su suscripción en Azure Monitor](usage-estimated-costs.md#moving-to-the-new-pricing-model). Esta acción cambiará el plan de tarifa de todas las áreas de trabajo en esa suscripción.


> [!NOTE]
> Puede obtener más información acerca de [establecer el plan de tarifa a través de ARM](template-workspace-configuration.md#create-a-log-analytics-workspace) y cómo asegurarse de que la implementación de ARM se realizará correctamente, independientemente de si la suscripción está en el heredado o el nuevo modelo de precios. 


## <a name="troubleshooting-why-log-analytics-is-no-longer-collecting-data"></a>Solucionar que Log Analytics ya no recopile datos
Si tiene el plan de tarifa gratuito heredado y ha enviado más de 500 MB de datos en un día, la recopilación de datos se detiene durante el resto del día. Alcanzar el límite diario es un motivo frecuente de que Log Analytics deje de recopilar datos o de que parezca que faltan datos.  Log Analytics crea un evento de tipo Operación cuando la recopilación de datos se inicia y se detiene. Ejecute la siguiente consulta en la búsqueda para comprobar si ha alcanzado el límite diario y faltan datos: 

`Operation | where OperationCategory == 'Data Collection Status'`

Cuando se detiene la recopilación de datos, OperationStatus es Advertencia. Cuando se inicia la recopilación de datos, OperationStatus es Correcto. La tabla siguiente describe los motivos por los que se detiene la recopilación de datos y una acción recomendada para reanudarla:  

|Motivo por el que se detiene la recopilación| Solución| 
|-----------------------|---------|
|Se alcanzó el límite diario del plan de tarifa Gratis heredado |Espere hasta el día siguiente para que la recopilación se reinicie automáticamente, o cambie a un plan de tarifa de pago.|
|Se alcanzó el límite diario del área de trabajo|Espere para que se reinicie automáticamente la recopilación o aumente el límite diario de volumen de datos que se describe en Administración del volumen de datos diario máximo El tiempo de restablecimiento de límite diario se muestra en la página **Administración del volumen de datos**. |
|La suscripción de Azure está en estado suspendido debido a:<br> Prueba gratuita finalizada<br> Pase para Azure expirado<br> Se ha alcanzado el límite de gasto mensual (por ejemplo, en una suscripción de MSDN o Visual Studio)|Cambie a una suscripción de pago<br> Quite el límite o espere a que se restablezca|

Para recibir una notificación cuando la recopilación de datos se detenga, siga los pasos descritos para *Crear una alerta para el límite de datos diario* para recibir una notificación cuando se detenga la recopilación de datos, y siga los pasos descritos en la incorporación de acciones a las reglas de alerta con la configuración de una acción de correo electrónico, webhook o runbook para la regla de alerta. 

## <a name="troubleshooting-why-usage-is-higher-than-expected"></a>Solución del problema de un uso mayor de lo esperado
Un mayor uso está provocado por una de estas causas o por ambas:
- Más nodos de lo previsto que envían datos a Log Analytics
- Más datos de lo esperado enviados a Log Analytics

El siguiente Explorer.exe secciones

## <a name="understanding-nodes-sending-data"></a>Descripción de nodos que envían datos

Para comprender el número de equipos (nodos) que notificaron datos todos los días del último mes, use lo siguiente

`Heartbeat | where TimeGenerated > startofday(ago(31d))
| summarize dcount(Computer) by bin(TimeGenerated, 1d)    
| render timechart`

Para obtener una lista de equipos que envían **tipos de datos facturados** (algunos tipos de datos son gratuitos), aproveche la propiedad [_IsBillable](log-standard-properties.md#_isbillable):

`union withsource = tt * 
| where _IsBillable == true 
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| where computerName != ""
| summarize TotalVolumeBytes=sum(_BilledSize) by computerName`

Use estas consultas `union withsource = tt *` con moderación, ya que la ejecución de exámenes entre tipos de datos es costosa. Esta consulta sustituye la antigua forma de consultar información de cada equipo con el tipo de datos de uso.  

Esto puede ampliarse para devolver el número de equipos por hora que se va a enviar factura a los tipos de datos (que es cómo Log Analytics calcula nodos facturables para la heredado por nodo de plan de tarifa):

`union withsource = tt * 
| where _IsBillable == true 
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| where computerName != ""
| summarize dcount(computerName) by bin(TimeGenerated, 1h) | sort by TimeGenerated asc`

## <a name="understanding-ingested-data-volume"></a>Volumen de datos ingerido de descripción 

En la página **Uso y costos estimados**, el gráfico *Ingesta de datos por solución*  muestra el volumen total de los datos enviados y la cantidad que envía cada solución. Esto le permite determinar tendencias tales como si el uso global de los datos (o el uso de una solución en particular) crece, permanece constante o disminuye. La consulta que se utiliza para generar esto es:

`Usage | where TimeGenerated > startofday(ago(31d))| where IsBillable == true
| summarize TotalVolumeGB = sum(Quantity) / 1024 by bin(TimeGenerated, 1d), Solution| render barchart`

Tenga en cuenta que la cláusula "where IsBillable = true" filtra los tipos de datos de determinadas soluciones para las que no hay ningún cargo de ingesta. 

Puede profundizar más para ver las tendencias de datos para tipos de datos específicos; por ejemplo, si desea estudiar los datos debidos a los registros de IIS:

`Usage | where TimeGenerated > startofday(ago(31d))| where IsBillable == true
| where DataType == "W3CIISLog"
| summarize TotalVolumeGB = sum(Quantity) / 1024 by bin(TimeGenerated, 1d), Solution| render barchart`

### <a name="data-volume-by-computer"></a>Volumen de datos por equipo

Para ver el **tamaño** de eventos facturables ingeridos por equipo, use el `_BilledSize` propiedad ([propiedades de registro estándar #_billedsize.md](learn more)) que proporciona el tamaño en bytes:

```
union withsource = tt * 
| where _IsBillable == true 
| summarize Bytes=sum(_BilledSize) by  Computer | sort by Bytes nulls last
```

El `_IsBillable` propiedad especifica si los datos ingeridos incurrirá en gastos ([log-estándar-properties.md #_isbillable](Learn more).)

Para ver el **recuento** de eventos ingeridos por equipo, use lo siguiente:

`union withsource = tt *
| summarize count() by Computer | sort by count_ nulls last`

Para ver el recuento de eventos facturables ingeridos por equipo, use: 

`union withsource = tt * 
| where _IsBillable == true 
| summarize count() by Computer  | sort by count_ nulls last`

Si quiere ver recuentos de tipos de datos facturables que envían datos a un equipo específico, use:

```
union withsource = tt *
| where Computer == "computer name"
| where _IsBillable == true 
| summarize count() by tt | sort by count_ nulls last
```

### <a name="data-volume-by-azure-resource-resource-group-or-subscription"></a>Volumen de datos por suscripción, grupo de recursos o recursos de Azure

Para los datos de nodos que se hospedan en Azure puede obtener el **tamaño** de ingesta de eventos facturables __por equipo__, utilice el `_ResourceId` propiedad que proporciona la ruta de acceso completa al recurso ([ registro-estándar-properties.md #_resourceid](learn more)):

```
union withsource = tt * 
| where _IsBillable == true 
| summarize Bytes=sum(_BilledSize) by _ResourceId | sort by Bytes nulls last
```

Para los datos de nodos que se hospedan en Azure puede obtener el **tamaño** de ingesta de eventos facturables __por suscripción de Azure__, analizar el `_ResourceId` propiedad como:

```
union withsource = tt * 
| where _IsBillable == true 
| parse tolower(_ResourceId) with "/subscriptions/" subscriptionId "/resourcegroups/" 
    resourceGroup "/providers/" provider "/" resourceType "/" resourceName   
| summarize Bytes=sum(_BilledSize) by subscriptionId | sort by Bytes nulls last
```

Cambiar `subscriptionId` a `resourceGroup` mostrará el volumen de datos ingeridos facturables por grupo de Azure resouurce. 


> [!NOTE]
> Algunos de los campos del tipo de datos de uso, aunque siguen en el esquema, han quedado en desuso y ya no se rellenarán sus valores. Estos son **Computer**, además de los campos relacionados con la ingesta (**TotalBatches**, **BatchesWithinSla**, **BatchesOutsideSla**, **BatchesCapped** y **AverageProcessingTimeMs**.

### <a name="querying-for-common-data-types"></a>Consulta de los tipos de datos comunes

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
| Eventos de seguridad            | Seleccione los [eventos de seguridad común o mínima](https://docs.microsoft.com/azure/security-center/security-center-enable-data-collection#data-collection-tier). <br> Cambie la directiva de auditoría de seguridad para recopilar únicamente los eventos necesarios. En particular, revise la necesidad de recopilar eventos para <br> - [auditar plataforma de filtrado](https://technet.microsoft.com/library/dd772749(WS.10).aspx) <br> - [auditar registro](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd941614(v%3dws.10))<br> - [auditar sistema de archivos](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd772661(v%3dws.10))<br> - [auditar objeto de kernel](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd941615(v%3dws.10))<br> - [auditar manipulación de identificadores](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd772626(v%3dws.10))<br> - auditar almacenamiento extraíble |
| contadores de rendimiento       | Cambie la [configuración de los contadores de rendimiento](data-sources-performance-counters.md) para: <br> - Reducir la frecuencia de la colección <br> - Reducir el número de contadores de rendimiento |
| Registros de eventos                 | Cambie la [configuración del registro de eventos](data-sources-windows-events.md) para: <br> - Reducir el número de registros de eventos recopilados <br> - Recopilar solo los niveles de eventos necesarios Por ejemplo, no recopile eventos de nivel de *información*. |
| syslog                     | Cambie la [configuración de syslog](data-sources-syslog.md) para: <br> - Reducir el número de instalaciones recopiladas <br> - Recopilar solo los niveles de eventos necesarios Por ejemplo, no recopile eventos de nivel de *información* y *depuración*. |
| AzureDiagnostics           | Cambie la colección de registros de recursos para: <br> - Reducir el número de registros de recursos enviados a Log Analytics <br> - Recopilar solo los registros necesarios |
| Datos de la solución procedentes de equipos que no necesitan la solución | Use la [selección de destino de solución](../insights/solution-targeting.md) para recopilar datos solo de los grupos de equipos necesarios. |

### <a name="getting-security-and-automation-node-counts"></a>Obtener recuentos de nodo de seguridad y automatización 

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

```
 ConfigurationData 
 | where (ConfigDataType == "WindowsServices" or ConfigDataType == "Software" or ConfigDataType =="Daemons") 
 | extend lowComputer = tolower(Computer) | summarize by lowComputer 
 | join (
     Heartbeat 
       | where SCAgentChannel == "Direct"
       | extend lowComputer = tolower(Computer) | summarize by lowComputer, ComputerEnvironment
 ) on lowComputer
 | summarize count() by ComputerEnvironment | sort by ComputerEnvironment asc
```

## <a name="create-an-alert-when-data-collection-is-higher-than-expected"></a>Creación de una alerta cuando la colección de datos es mayor de lo esperado

En esta sección se describe cómo crear una alerta si:
- El volumen de datos supera una cantidad especificada.
- Se prevé que el volumen de datos supere un importe especificado.

Alertas de Azure admite [alertas de registro](alerts-unified-log.md) que usen consultas de búsqueda. 

La consulta siguiente produce un resultado cuando hay más de 100 GB de datos recopilados en las últimas 24 horas:

`union withsource = $table Usage | where QuantityUnit == "MBytes" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true | extend Type = $table | summarize DataGB = sum((Quantity / 1024)) by Type | where DataGB > 100`

La consulta siguiente utiliza una fórmula simple para predecir cuándo se enviarán más de 100 GB de datos en un día: 

`union withsource = $table Usage | where QuantityUnit == "MBytes" and iff(isnotnull(toint(IsBillable)), IsBillable == true, IsBillable == "true") == true | extend Type = $table | summarize EstimatedGB = sum(((Quantity * 8) / 1024)) by Type | where EstimatedGB > 100`

Para generar una alerta en un volumen de datos diferente, cambie el 100 de las consultas por el número de GB sobre el que desea alertar.

Siga los pasos explicados en [crear una nueva regla de alerta](alerts-metric.md) para recibir una notificación cuando la colección de datos sea mayor de lo previsto.

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

Especifique un [grupo de acciones](action-groups.md) existente o cree uno nuevo para que cuando la alerta de registro coincida con criterios, se le notifique.

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

Especifique un [grupo de acciones](action-groups.md) existente o cree uno nuevo para que cuando la alerta de registro coincida con criterios, se le notifique.

Cuando se recibe una alerta, siga los pasos de la sección siguiente para solucionar el problema del uso mayor de lo esperado.

## <a name="next-steps"></a>Pasos siguientes
* Consulte [Búsquedas de registro en Log Analytics](../log-query/log-query-overview.md) para obtener información sobre cómo usar el lenguaje de búsqueda. Puede utilizar las consultas de búsqueda para realizar análisis adicionales sobre los datos de uso.
* Siga los pasos explicados en [Crear una nueva alerta de registro](alerts-metric.md) para recibir una notificación cuando se cumplan los criterios de búsqueda.
* Use la [selección de destino de solución](../insights/solution-targeting.md) para recopilar datos solo de los grupos de equipos necesarios.
* Para configurar una directiva eficaz de recopilación de eventos, revise la [Directiva de filtrado de Azure Security Center](../../security-center/security-center-enable-data-collection.md).
* Cambie la [configuración de los contadores de rendimiento](data-sources-performance-counters.md).
* Para modificar la configuración de recopilación de eventos, revise la [configuración de registros de eventos](data-sources-windows-events.md).
* Para modificar la configuración de la recopilación de syslog, revise la [configuración de syslog](data-sources-syslog.md).


