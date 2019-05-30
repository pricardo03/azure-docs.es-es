---
title: Ver eventos del registro de actividad de Azure en Azure Monitor
description: Ver el registro de actividad de Azure en Azure Monitor y recuperar con PowerShell, CLI y API de REST.
author: bwren
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/10/2019
ms.author: johnkem
ms.subservice: logs
ms.openlocfilehash: 32578f77f2b3f30d80953bdd1099d22c945c640b
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66248120"
---
# <a name="view-and-retrieve-azure-activity-log-events"></a>Ver y recuperar los eventos de registro de actividad de Azure

El [Azure Activity Log](activity-logs-overview.md) proporciona información sobre los eventos de nivel de suscripción que se han producido en Azure. En este artículo proporciona detalles sobre los métodos diferentes para ver y recuperar los eventos de registro de actividad.

## <a name="azure-portal"></a>Azure Portal
Ver el registro de actividad para todos los recursos de la **Monitor** menú en el portal de Azure. Ver el registro de actividad para un recurso determinado desde el **registro de actividad** opción en el menú de ese recurso.

![Ver registro de actividad](./media/activity-logs-overview/view-activity-log.png)

Puede filtrar los eventos de registro de actividad por los campos siguientes:

* **Intervalo de tiempo**: La hora de inicio y finalización para los eventos.
* **Categoría**: Como se describe en la categoría de eventos [categorías en el registro de actividad](activity-logs-overview.md#categories-in-the-activity-log).
* **Suscripción**: Uno o más nombres de suscripción de Azure.
* **Grupo de recursos**: Uno o varios grupos de recursos dentro de las suscripciones seleccionadas.
* **Recurso (nombre)** :-el nombre de un recurso específico.
* **Tipo de recurso**: El tipo de recurso, por ejemplo _Microsoft.COMPUTE/virtualmachines_.
* **Nombre de la operación** -el nombre de una operación de Azure Resource Manager, por ejemplo _Microsoft.SQL/servers/Write_.
* **Gravedad**: Nivel de gravedad del evento. Los valores disponibles son _informativo_, _advertencia_, _Error_, _crítico_.
* **Evento iniciado por**: El usuario que realizó la operación.
* **Abrir búsqueda**: Abra el cuadro de búsqueda de texto que busca esa cadena en todos los campos en todos los eventos.

### <a name="view-change-history"></a>Ver historial de cambios

Al revisar el registro de actividad, puede ayudar a ver lo que se produjeron cambios durante esa hora del evento. Puede ver esta información con **historial de cambios**. Seleccione un evento del que desea ver más detalles en el registro de actividad. Seleccione el **(versión preliminar) historial de cambios** pestaña para ver cualquier asociado cambios a ese evento.

![Lista de historial de cambios para un evento](media/activity-logs-overview/change-history-event.png)

Si hay algún cambio asociado con el evento, verá una lista de cambios que se pueden seleccionar. Se abrirá el **(versión preliminar) historial de cambios** página. En esta página ve los cambios en el recurso. Como puede ver en el ejemplo siguiente, se pueden ver no solo que la máquina virtual cambiado tamaños, pero ¿cuál era el tamaño de VM anterior antes del cambio y lo que se ha cambiado a.

![Página de historial de cambios que muestra las diferencias](media/activity-logs-overview/change-history-event-details.png)

Para obtener más información sobre el historial de cambios, consulte [obtener los cambios de recursos](../../governance/resource-graph/how-to/get-resource-changes.md).


## <a name="log-analytics-workspace"></a>Área de trabajo de Log Analytics
Haga clic en **registros** en la parte superior de la **registro de actividad** página para abrir el [Activity Log Analytics, solución de supervisión](activity-log-collect.md) para la suscripción. Esto le permitirá ver análisis del registro de actividades y para ejecutar [registrar consultas](../log-query/log-query-overview.md) con el **AzureActivity** tabla. Si el registro de actividad no está conectado a un área de trabajo de Log Analytics, se le pedirá para realizar esta configuración.



## <a name="powershell"></a>PowerShell
Use la [Get AzLog](https://docs.microsoft.com/powershell/module/az.monitor/get-azlog) cmdlet para recuperar el registro de actividad de PowerShell. Estos son algunos ejemplos comunes.

> [!NOTE]
> `Get-AzLog` solo proporciona 15 días de historial. Use la **- MaxEvents** parámetro para consultar los últimos eventos N más allá de 15 días. Para tener acceso a más de 15 días de eventos, use la API de REST o SDK. Si no incluye **StartTime**, el valor predeterminado será **EndTime** menos una hora. Si no incluye **EndTime**, el valor predeterminado será la hora actual. Todas las horas se muestran en UTC.


Obtención de entradas de registro creadas después de una determinada fecha y hora:

```powershell
Get-AzLog -StartTime 2016-03-01T10:30
```

Obtención de entradas de registro entre un intervalo de tiempo de fecha:

```powershell
Get-AzLog -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

Obtención de entradas de registro de un grupo de recursos específico:

```powershell
Get-AzLog -ResourceGroup 'myrg1'
```

Obtener las entradas del registro de un proveedor de recursos específico entre un intervalo de tiempo de fecha:

```powershell
Get-AzLog -ResourceProvider 'Microsoft.Web' -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

Obtención de entradas de registro con un llamador concreto:

```powershell
Get-AzLog -Caller 'myname@company.com'
```

Obtenga los últimos 1000 eventos:

```powershell
Get-AzLog -MaxEvents 1000
```


## <a name="cli"></a>CLI
Use [registro de actividad de monitor az](cli-samples.md#view-activity-log-for-a-subscription) para recuperar el registro de actividad de la CLI. Estos son algunos ejemplos comunes.


Ver todas las opciones disponibles.

```azurecli
az monitor activity-log list -h
```

Obtención de entradas de registro de un grupo de recursos específico:

```azurecli
az monitor activity-log list --resource-group <group name>
```

Obtención de entradas de registro con un llamador concreto:

```azurecli
az monitor activity-log list --caller myname@company.com
```

Obtener registros por autor de llamada en un tipo de recurso, dentro de un intervalo de fechas:

```azurecli
az monitor activity-log list --resource-provider Microsoft.Web \
    --caller myname@company.com \
    --start-time 2016-03-08T00:00:00Z \
    --end-time 2016-03-16T00:00:00Z
```

## <a name="rest-api"></a>API DE REST
Use la [API de REST de Azure Monitor](https://docs.microsoft.com/rest/api/monitor/) para recuperar el registro de actividad de un cliente de REST. Estos son algunos ejemplos comunes.

Obtener registros de actividad con filtro:

``` HTTP
GET https://management.azure.com/subscriptions/089bd33f-d4ec-47fe-8ba5-0753aa5c5b33/providers/microsoft.insights/eventtypes/management/values?api-version=2015-04-01&$filter=eventTimestamp ge '2018-01-21T20:00:00Z' and eventTimestamp le '2018-01-23T20:00:00Z' and resourceGroupName eq 'MSSupportGroup'
```

Obtener registros de actividad con filtro y seleccione:

```HTTP
GET https://management.azure.com/subscriptions/089bd33f-d4ec-47fe-8ba5-0753aa5c5b33/providers/microsoft.insights/eventtypes/management/values?api-version=2015-04-01&$filter=eventTimestamp ge '2015-01-21T20:00:00Z' and eventTimestamp le '2015-01-23T20:00:00Z' and resourceGroupName eq 'MSSupportGroup'&$select=eventName,id,resourceGroupName,resourceProviderName,operationName,status,eventTimestamp,correlationId,submissionTimestamp,level
```

Obtener registros de actividad con select:

```HTTP
GET https://management.azure.com/subscriptions/089bd33f-d4ec-47fe-8ba5-0753aa5c5b33/providers/microsoft.insights/eventtypes/management/values?api-version=2015-04-01&$select=eventName,id,resourceGroupName,resourceProviderName,operationName,status,eventTimestamp,correlationId,submissionTimestamp,level
```

Obtener registros de actividad sin filtro o seleccione:

```HTTP
GET https://management.azure.com/subscriptions/089bd33f-d4ec-47fe-8ba5-0753aa5c5b33/providers/microsoft.insights/eventtypes/management/values?api-version=2015-04-01
```


## <a name="next-steps"></a>Pasos siguientes

* [Lea información general sobre el registro de actividad](activity-logs-overview.md)
* [Archivar el registro de actividad en el almacenamiento o transmitirlo a Event Hubs](activity-log-export.md)
* [Transmisión del registro de actividad de Azure a Event Hubs](activity-logs-stream-event-hubs.md)
* [Archivar el registro de actividad de Azure para almacenamiento](archive-activity-log.md)

