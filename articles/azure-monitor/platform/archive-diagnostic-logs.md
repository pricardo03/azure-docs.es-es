---
title: Archivo de registros de diagnóstico de Azure
description: Aprenda a archivar los registros de diagnóstico de Azure para su retención a largo plazo en una cuenta de almacenamiento.
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 07/18/2018
ms.author: johnkem
ms.subservice: logs
ms.openlocfilehash: bc1804e547bb1a29fc0dc680b948f1bb31af8307
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66244929"
---
# <a name="archive-azure-diagnostic-logs"></a>Archivo de registros de diagnóstico de Azure

En este artículo, le mostraremos cómo puede usar Azure Portal, los cmdlets de PowerShell, la CLI o la API de REST para archivar los [registros de diagnóstico de Azure](diagnostic-logs-overview.md) en una cuenta de almacenamiento. Esta opción es útil si desea conservar los registros de diagnóstico con una directiva de retención opcional para auditorías, análisis estáticos o copias de seguridad. La cuenta de almacenamiento no tiene que estar en la misma suscripción que la del recurso que emite los registros, siempre que el usuario que configura la configuración tenga acceso RBAC adecuado a ambas suscripciones.

> [!WARNING]
> El formato de los datos de registro de la cuenta de almacenamiento cambiará a JSON Lines el 1 de noviembre de 2018. [Consulte este artículo para obtener una descripción de la repercusión y del modo de actualizar las herramientas para administrar el nuevo formato.](./../../azure-monitor/platform/diagnostic-logs-append-blobs.md) 
>
> 

## <a name="prerequisites"></a>Requisitos previos

Antes de comenzar, necesita [crear una cuenta de almacenamiento](../../storage/common/storage-quickstart-create-account.md) en la que poder archivar los registros de diagnóstico. Le recomendamos encarecidamente que no utilice una cuenta de almacenamiento existente que tenga otros datos sin supervisión almacenados en ella, para que pueda controlar mejor el acceso a los datos de supervisión. Sin embargo, si también va a archivar el registro de actividad y las métricas de diagnóstico para una cuenta de almacenamiento, es posible que sentido utilizar esa cuenta de almacenamiento para los registros de diagnóstico para mantener todos los datos de supervisión en una ubicación central.

> [!NOTE]
>  Actualmente no puede archivar datos en una cuenta de almacenamiento que está detrás de una red virtual protegida.

## <a name="diagnostic-settings"></a>Configuración de diagnóstico

Para archivar los registros de diagnóstico mediante cualquiera de los métodos siguientes, establezca una **configuración de diagnóstico** para un recurso determinado. Una configuración de diagnóstico para un recurso define las categorías de registros y datos de métricas enviados a un destino (cuenta de almacenamiento, espacio de nombres de Event Hubs o área de trabajo de Log Analytics). También define la directiva de retención (el número de días que deben conservarse) para los eventos de cada dato de métrica y categoría de registro almacenados en una cuenta de almacenamiento. Si se establece en cero una directiva de retención, los eventos para esa categoría de registro se almacenan indefinidamente. De lo contrario, una directiva de retención puede ser cualquier número de días comprendido entre 1 y 2147483647. [Puede leer más acerca de estas opciones de diagnóstico aquí](../../azure-monitor/platform/diagnostic-logs-overview.md#diagnostic-settings). Las directivas de retención se aplican a diario, por lo que al final de un día (UTC) se eliminan los registros del día que quede fuera de la directiva de retención. Por ejemplo, si tuviera una directiva de retención de un día, se eliminarían los registros de anteayer al principio del día de hoy. El proceso de eliminación empieza a medianoche (UTC), pero tenga en cuenta que eliminar los registros de la cuenta de almacenamiento puede tardar hasta 24 horas. 

> [!NOTE]
> Actualmente no se admite el envío de métricas de varias dimensiones a través de la configuración de diagnóstico. Las métricas con dimensiones se exportan como métricas unidimensionales planas agregadas a través de los valores de dimensión.
>
> *Por ejemplo*: la métrica "Mensajes entrantes" de una instancia de Event Hub se puede explorar y representar gráficamente por colas. Sin embargo, cuando se exporta a través de la configuración de diagnóstico, la métrica se representará con todos los mensajes entrantes de todas las colas de Event Hub.
>
>

## <a name="archive-diagnostic-logs-using-the-portal"></a>Archivo de registros de diagnóstico mediante el portal

1. En el portal, desplácese a Azure Monitor y haga clic en **Configuración de diagnóstico**.

    ![Sección de supervisión de Azure Monitor](media/archive-diagnostic-logs/diagnostic-settings-blade.png)

2. Si lo desea, filtre la lista por tipo de recurso o por grupo de recursos y, a continuación, haga clic en el recurso para el que desea establecer la configuración de diagnóstico.

3. Si no existe ninguna configuración en el recurso que ha seleccionado, se le pide que cree una. Haga clic en "Activar diagnóstico".

   ![Agregar configuración de diagnóstico: sin configuración actual](media/archive-diagnostic-logs/diagnostic-settings-none.png)

   Si hay una configuración actual en el recurso, verá una lista de opciones ya configuradas en este recurso. Haga clic en "Agregar configuración de diagnóstico".

   ![Agregar configuración de diagnóstico: configuración actual](media/archive-diagnostic-logs/diagnostic-settings-multiple.png)

3. Dé a su opcion de configuración un nombre y active la casilla para **Exportar a cuenta de almacenamiento**; después, seleccione una cuenta de almacenamiento. Además, puede establecer un número de días para conservar estos registros mediante los controles deslizantes de **Retención (días)** . Con una retención de cero días, los registros se almacenan indefinidamente.

   ![Agregar configuración de diagnóstico: configuración actual](media/archive-diagnostic-logs/diagnostic-settings-configure.png)

4. Haga clic en **Save**(Guardar).

Transcurridos unos instantes, la nueva opción de configuración aparece en la lista de opciones para este recurso y los registros de diagnóstico se archivan en esa cuenta de almacenamiento en cuanto se generan nuevos datos de eventos.

## <a name="archive-diagnostic-logs-via-azure-powershell"></a>Archivo de registros de diagnóstico mediante Azure PowerShell

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

```
Set-AzDiagnosticSetting -ResourceId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg -StorageAccountId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Categories networksecuritygroupevent,networksecuritygrouprulecounter -Enabled $true -RetentionEnabled $true -RetentionInDays 90
```

| Propiedad | Obligatorio | DESCRIPCIÓN |
| --- | --- | --- |
| resourceId |Sí |Identificador de recurso en el que desea establecer una configuración de diagnóstico. |
| StorageAccountId |Sin  |Identificador de recurso de la cuenta de almacenamiento en la que se deben guardar los registros de diagnóstico. |
| Categorías |Sin  |Lista de categorías de registro separadas por comas, que se van a habilitar. |
| Enabled |Sí |Valor booleano que indica si los diagnósticos están habilitados o deshabilitados en este recurso. |
| RetentionEnabled |Sin  |Valor booleano que indica si está habilitada una directiva de retención en este recurso. |
| RetentionInDays |Sin  |Número de días que deben retenerse los eventos, entre 1 y 2147483647. Con el valor cero, se almacenan los registros indefinidamente. |

## <a name="archive-diagnostic-logs-via-the-azure-cli"></a>Archivo de registros de diagnóstico mediante la CLI de Azure

```azurecli
az monitor diagnostic-settings create --name <diagnostic name> \
    --storage-account <name or ID of storage account> \
    --resource <target resource object ID> \
    --resource-group <storage account resource group> \
    --logs '[
    {
        "category": <category name>,
        "enabled": true,
        "retentionPolicy": {
            "days": <# days to retain>,
            "enabled": true
        }
    }]'
```

Puede agregar categorías adicionales al registro de diagnóstico mediante la incorporación de diccionarios a la matriz JSON que se pasa como el parámetro `--logs`.

El argumento `--resource-group` solo es obligatorio si `--storage-account` no es un identificador de objeto. Para obtener la documentación completa para archivar los registros de diagnóstico para almacenamiento, consulte la [referencia de comandos de la CLI](/cli/azure/monitor/diagnostic-settings#az-monitor-diagnostic-settings-create).

## <a name="archive-diagnostic-logs-via-the-rest-api"></a>Archivo de registros de diagnóstico mediante la API de REST

[Consulte este documento](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings) para obtener información sobre cómo definir una configuración de diagnóstico mediante la API de REST de Azure Monitor.

## <a name="schema-of-diagnostic-logs-in-the-storage-account"></a>Esquema de registros de diagnóstico en la cuenta de almacenamiento

Una vez que haya configurado el archivo, se crea un contenedor de almacenamiento en la cuenta de almacenamiento en cuanto se produzca un evento en una de las categorías de registro que haya habilitado. Los blobs dentro del contenedor siguen la misma convención de nomenclatura en los registros de actividad y los registros de diagnóstico, como se muestra aquí:

```
insights-logs-{log category name}/resourceId=/SUBSCRIPTIONS/{subscription ID}/RESOURCEGROUPS/{resource group name}/PROVIDERS/{resource provider name}/{resource type}/{resource name}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```

Por ejemplo, un nombre de blob podría ser:

```
insights-logs-networksecuritygrouprulecounter/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUP/TESTNSG/y=2016/m=08/d=22/h=18/m=00/PT1H.json
```

Cada blob PT1H.json contiene un blob JSON de eventos que se produjeron en la hora especificada en la dirección URL del blob (por ejemplo h=12). Durante la hora en cuestión, los eventos se anexan al archivo PT1H.json a medida que se producen. El valor de los minutos (m=00) siempre es 00, ya que los eventos de los registros de diagnóstico se dividen en blobs individuales por hora.

En el archivo PT1H.json, cada evento se almacena en la matriz de "registros" con este formato:

``` JSON
{
    "records": [
        {
            "time": "2016-07-01T00:00:37.2040000Z",
            "systemId": "46cdbb41-cb9c-4f3d-a5b4-1d458d827ff1",
            "category": "NetworkSecurityGroupRuleCounter",
            "resourceId": "/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/TESTNSG",
            "operationName": "NetworkSecurityGroupCounters",
            "properties": {
                "vnetResourceGuid": "{12345678-9012-3456-7890-123456789012}",
                "subnetPrefix": "10.3.0.0/24",
                "macAddress": "000123456789",
                "ruleName": "/subscriptions/ s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg/securityRules/default-allow-rdp",
                "direction": "In",
                "type": "allow",
                "matchedConnections": 1988
            }
        }
    ]
}
```

| Nombre del elemento | DESCRIPCIÓN |
| --- | --- |
| time |Marca de tiempo de cuándo el servicio de Azure generó el evento que procesó la solicitud correspondiente al evento. |
| resourceId |Identificador de recurso del recurso afectado. |
| operationName |Nombre de la operación. |
| category |Categoría de registro del evento. |
| properties |Conjunto de pares `<Key, Value>` (es decir, diccionario) que describe los detalles del evento. |

> [!NOTE]
> Las propiedades y el uso de estas propiedades pueden variar según el recurso.

## <a name="next-steps"></a>Pasos siguientes

* [Descargar blobs para el análisis](../../storage/blobs/storage-quickstart-blobs-dotnet.md)
* [Streaming de registros de diagnóstico a un espacio de nombres de Event Hubs](../../azure-monitor/platform/diagnostic-logs-stream-event-hubs.md)
* [Archivar registros de Azure Active Directory con Azure Monitor](../../active-directory/reports-monitoring/quickstart-azure-monitor-route-logs-to-storage-account.md)
* [Información general sobre los registros de diagnóstico de Azure](../../azure-monitor/platform/diagnostic-logs-overview.md)

