---
title: Comprender cómo funcionan las alertas de métricas en Azure Monitor.
description: Obtenga información general acerca de lo que puede hacer con las alertas de métricas y cómo funcionan en Azure Monitor.
author: snehithm
ms.author: snmuvva
ms.date: 9/18/2018
ms.topic: conceptual
ms.service: azure-monitor
ms.component: alerts
ms.openlocfilehash: 0ac19ec18c443f677260ba22b6bce1c06237fb2e
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2018
ms.locfileid: "48816437"
---
# <a name="understand-how-metric-alerts-work-in-azure-monitor"></a>Comprender cómo funcionan las alertas de métricas en Azure Monitor

Las alertas de métricas en Azure Monitor funcionan en la parte superior de las métricas multidimensionales. Estas métricas pueden ser métricas de plataforma, [métricas personalizadas](metrics-custom-overview.md), [registros populares de Log Analytics convertidos en métricas](monitoring-metric-alerts-logs.md) y métricas estándar de Application Insights. Las alertas de métricas se evalúan a intervalos regulares para comprobar si las condiciones de una o varias series temporales para las métricas son verdaderas y recibirá una notificación cuando se cumplan las evaluaciones. Como las alertas de métricas tienen un estado, solo envían notificaciones cuando cambia ese estado.

## <a name="how-do-metric-alerts-work"></a>Cómo funcionan las alertas de métricas

Puede definir una regla de alerta de métrica especificando el recurso de destino que se supervisará, el nombre y la condición de la métrica (un operador y un umbral) y un grupo de acción que se activará cuando a su vez se active la regla de alerta.
Supongamos que ha creado una regla de alerta de métrica simple de la siguiente manera:

- Recurso de objetivo (el recurso de Azure que quiere supervisar): myVM
- Métrica: el porcentaje de la CPU
- Agregación de tiempo (estadística que se ejecuta sobre valores de métrica sin procesar. Las agregaciones de tiempo admitidas son Min, Max, Avg, Total): Average
- Período (la ventana temporal según la cual se comprueban los valores de la métrica): Over the last 5 mins
- Frecuencia (es decir, la frecuencia con la que la alerta de métricas comprueba si se cumplen las condiciones): 1 min
- Operador: Greater Than
- Umbral: 70

Desde el momento en que se crea la regla de alerta, el proceso de supervisión se ejecuta cada minuto y examina los valores métricos de los últimos 5 minutos; además, comprueba si el promedio de esos valores es superior a 70. Si se cumple la condición, es decir, si el porcentaje medio de la CPU supera el valor de 70 durante los últimos 5 minutos, la regla de alerta desencadena una notificación activada. Si configuró un correo electrónico o una acción de webhook en el grupo de acciones asociado a la regla de alerta, recibirá una notificación activada en ambos recursos.

Esta instancia particular de la activación de la regla de alerta también se puede ver en Azure Portal, en la hoja Todas las alertas.

Digamos que el uso en "myVM" continúa estando por encima del umbral en las comprobaciones posteriores; por lo tanto, la regla de alerta no volverá a activarse hasta que se resuelva la condición.

Después de algún tiempo, si el uso en "myVM" vuelve a ser normal, volverá a estar por debajo del umbral especificado. Debido a ello, la regla de alerta supervisa la condición dos veces más para enviar una notificación resuelta. La regla de alerta envía un mensaje resuelto o desactivado cuando la condición de alerta no se cumple durante tres períodos consecutivos, para así reducir el ruido en caso de que haya una oscilación en las condiciones.

Cuando la notificación resuelta se envíe a través del webhook o del correo electrónico, el estado de la instancia de alerta (llamada Estado de supervisión) de Azure Portal también se establecerá como Resuelta.

## <a name="monitoring-at-scale-using-metric-alerts-in-azure-monitor"></a>Supervisión a escala mediante alertas de métricas en Azure Monitor

### <a name="using-dimensions"></a>Usar las dimensiones

Las alertas de métricas en Azure Monitor también admiten la supervisión de las combinaciones de valores de varias dimensiones con una regla. Aquí le explicaremos por qué debería usar combinaciones de varias dimensiones con la ayuda de un ejemplo.

Supongamos que tiene un plan de App Service para su sitio web. Quiere supervisar el uso de la CPU en varias instancias de la aplicación o el sitio web. Puede hacerlo mediante una regla de alerta de métrica, tal como se indica a continuación.

- Recurso de destino: myAppServicePlan
- Métrica: el porcentaje de la CPU
- Dimensiones
  - Instance = InstanceName1, InstanceName2
- Agregación de tiempo: media
- Período: Over the last 5 mins
- Frecuencia: 1 min
- Operador: GreaterThan
- Umbral: 70

Igual que antes, esta regla supervisa si el uso medio de la CPU durante los últimos 5 minutos supera el 70 %. Sin embargo, con la misma regla puede supervisar dos instancias de su sitio web. Cada instancia será supervisada individualmente y, por consiguiente, recibirá notificaciones de forma individual.

Supongamos que tiene una aplicación web con una demanda masiva y es necesario agregar más instancias. La regla anterior únicamente supervisa dos instancias. Sin embargo, puede crear una regla de la siguiente manera.

- Recurso de destino: myAppServicePlan
- Métrica: el porcentaje de la CPU
- Dimensiones
  - Instancia: *
- Agregación de tiempo: media
- Período: Over the last 5 mins
- Frecuencia: 1 min
- Operador: GreaterThan
- Umbral: 70

Esta regla supervisará automáticamente todos los valores de la instancia, es decir, puede supervisar sus instancias a medida que aparecen sin necesidad de modificar su regla de alerta de métrica nuevamente.

### <a name="monitoring-multiple-resources-using-metric-alerts"></a>Supervisión de varios recursos mediante alertas de métricas

Como ya ha visto en la sección anterior, es posible tener una sola regla de alerta de métrica que supervisa cada combinación de dimensión individual (es decir, una serie de tiempo para las métricas). Sin embargo, antes existía la limitación de que había que hacerlo recurso a recurso. Azure Monitor también admite la supervisión de varios recursos con una sola regla de alerta de métrica. Esta característica está actualmente versión preliminar y solo se admite en máquinas virtuales. Además, una alerta de métrica individual puede supervisar los recursos de una sola región de Azure.

Puede especificar el ámbito de la supervisión por una única alerta de métrica de cualquiera de estas tres formas:

- como una lista de máquinas virtuales de una región de Azure en una suscripción
- todas las máquinas virtuales (de una región de Azure) en uno o varios grupos de recursos de una suscripción
- todas las máquinas virtuales (de una región de Azure) en una suscripción

Actualmente, no se admite la creación de reglas de alerta de métricas que supervisen varios recursos a través de Azure Portal. Estas reglas se pueden crear mediante las [plantillas de Azure Resource Manager](monitoring-create-metric-alerts-with-templates.md#resource-manager-template-for-metric-alert-that-monitors-multiple-resources). Recibirá notificaciones individuales para cada máquina virtual. 

## <a name="typical-latency"></a>Latencia típica

En cuanto a las alertas de métricas, normalmente recibirá una notificación en menos de 5 minutos si configura la frecuencia de la regla de alerta en "1 min". En caso de que haya cargas pesadas en los sistemas de notificaciones, es posible que vea una latencia más larga.

## <a name="supported-resource-types-for-metric-alerts"></a>Tipos de recursos admitidos en las alertas métricas

En este [artículo](monitoring-near-real-time-metric-alerts.md#metrics-and-dimensions-supported) puede encontrar la lista completa de tipos de recursos admitidos.

Si ya utiliza alertas de métricas clásicas y desea ver si las alertas de métricas admiten todos los tipos de recursos que usa, en la tabla siguiente se muestran los tipos de recursos compatibles con alertas de métricas clásicas y si son compatibles con las alertas de métricas actuales o no.

|Tipo de recurso compatible con alertas de métricas clásicas | Compatible con alertas de métricas |
|-------------------------------------------------|----------------------------|
| Microsoft.ApiManagement/service | SÍ |
| Microsoft.Batch/batchAccounts| SÍ|
|Microsoft.Cache/redis| SÍ
|Microsoft.ClassicCompute/virtualMachines | Sin  |
|Microsoft.ClassicCompute/domainNames/slots/roles | Sin |
|Microsoft.CognitiveServices/accounts | Sin  |
|Microsoft.Compute/virtualMachines | SÍ|
|Microsoft.Compute/virtualMachineScaleSets| SÍ|
|Microsoft.ClassicStorage/storageAccounts| Sin  |
|Microsoft.DataFactory/datafactories | SÍ|
|Microsoft.DBforMySQL/servers| SÍ|
|Microsoft.DBforPostgreSQL/servers| SÍ|
|Microsoft.Devices/IotHubs | Sin |
|Microsoft.DocumentDB/databaseAccounts| Sin |
|Microsoft.EventHub/namespaces | SÍ|
|Microsoft.Logic/workflows | SÍ|
|Microsoft.Network/loadBalancers |SÍ|
|Microsoft.Network/publicIPAddresses| SÍ|
|Microsoft.Network/applicationGateways| SÍ|
|Microsoft.Network/expressRouteCircuits| SÍ|
|Microsoft.Network/trafficManagerProfiles | SÍ|
|Microsoft.Search/searchServices | Sin |
|Microsoft.ServiceBus/namespaces| Sin |
|Microsoft.Storage/storageAccounts | SÍ|
|Microsoft.StreamAnalytics/streamingjobs| SÍ|
|Microsoft.TimeSeriesInsights/environments | SÍ|
|Microsoft. Web/serverfarms | SÍ |
|Microsoft. Web/sites (se excluyen funciones) | SÍ|
|Microsoft. Web/hostingEnvironments/multiRolePools | Sin |
|Microsoft. Web/hostingEnvironments/workerPools| Sin 
|Microsoft.SQL/Servers | Sin |

## <a name="next-steps"></a>Pasos siguientes

- [Aprenda a crear, ver y administrar las alertas de métricas en Azure](alert-metric.md)
- [Aprenda a implementar alertas de métricas con plantillas de Azure Resource Manager](monitoring-create-metric-alerts-with-templates.md)
- [Obtenga más información sobre los grupos de acciones](monitoring-action-groups.md)
