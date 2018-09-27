---
title: Información general sobre los registros de diagnóstico de Azure
description: Aprenda qué son los registros de diagnóstico de Azure y cómo puede usarlos para entender los eventos que se producen en un recurso de Azure.
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/07/2018
ms.author: johnkem
ms.component: logs
ms.openlocfilehash: 630c8e5f48ba475755c3c9ffe632fcba086437e4
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46983128"
---
# <a name="collect-and-consume-log-data-from-your-azure-resources"></a>Recopile y use los datos de registro provenientes de los recursos de Azure

## <a name="what-are-azure-monitor-diagnostic-logs"></a>Qué son los registros de diagnóstico de Azure Monitor

Los **registros de diagnóstico de Azure Monitor** son registros emitidos por un servicio de Azure que proporcionan datos exhaustivos y frecuentes acerca del funcionamiento de ese servicio. Azure Monitor pone a disposición dos tipos de registros de diagnóstico:
* **Registros de inquilino**: provienen de servicios de nivel de inquilino que existen fuera de una suscripción de Azure, como los registros de Azure Active Directory.
* **Registros de recursos**: proceden de servicios de Azure que implementan recursos dentro de una suscripción de Azure, como grupos de seguridad de red o cuentas de almacenamiento.

    ![Comparación de los registros de diagnóstico de recursos y otros tipos de registros ](./media/monitoring-overview-of-diagnostic-logs/Diagnostics_Logs_vs_other_logs_v5.png)

El contenido de estos registros varía según el servicio de Azure y el tipo de recurso. Por ejemplo, los contadores de regla de grupo de seguridad de red y las auditorías de Key Vault son dos tipos de registros de diagnóstico.

Estos registros de diagnóstico son distintos del [registro de actividad](monitoring-overview-activity-logs.md). El registro de actividad proporciona una visión general de las operaciones que se realizaron en los recursos de la suscripción con Resource Manager, por ejemplo, crear una máquina virtual o eliminar una aplicación de lógica. El registro de actividad es un registro de nivel de suscripción. Los registros de diagnóstico de nivel de recursos proporcionan una visión general de las operaciones realizadas dentro del mismo recurso, por ejemplo, obtener un secreto de un almacén de claves.

Estos registros también difieren de los registros de diagnóstico de nivel de sistema operativo invitado. Estos son los recopilados por un agente que se ejecuta dentro de una máquina virtual u otro tipo de recurso admitido. Los registros de diagnóstico de nivel de recursos no requieren ningún agente y capturan datos específicos de recurso de la plataforma Azure, mientras que los registros de diagnóstico de nivel de sistema operativo invitado capturan los datos desde el sistema operativo y las aplicaciones que se ejecutan en una máquina virtual.

No todos los servicios admiten los registros de diagnóstico que se describen aquí. [En este artículo se incluye una sección en la que se muestran los servicios que admiten los registros de diagnóstico](./monitoring-diagnostic-logs-schema.md).

## <a name="what-you-can-do-with-diagnostic-logs"></a>Qué se puede hacer con los registros de diagnóstico
Estas son algunas de las cosas que puede hacer con los registros de diagnóstico:

![Ubicación lógica de registros de diagnóstico](./media/monitoring-overview-of-diagnostic-logs/Diagnostics_Logs_Actions.png)

* Guardarlos en una [**cuenta de almacenamiento**](monitoring-archive-diagnostic-logs.md) para archivarlos o inspeccionarlos manualmente. Puede especificar el tiempo de retención (en días) usando la **configuración de diagnóstico de recursos**.
* [Transmitirlos a **Event Hubs**](monitoring-stream-diagnostic-logs-to-event-hubs.md) para la ingestión en un servicio de terceros o una solución de análisis personalizado como PowerBI.
* Analícelos con [Log Analytics](../log-analytics/log-analytics-azure-storage.md).

Puede usar una cuenta de almacenamiento o un espacio de nombres de Event Hubs que no esté en la misma suscripción que el que emite los registros. El usuario que configura los ajustes debe tener el acceso de RBAC adecuado a ambas suscripciones.

> [!NOTE]
>  Actualmente no puede archivar registros de flujo de red en una cuenta de almacenamiento que está detrás de una red virtual protegida.

> [!WARNING]
> El formato de los datos de registro de la cuenta de almacenamiento cambiará a JSON Lines el 1 de noviembre de 2018. [Consulte este artículo para obtener una descripción de la repercusión y del modo de actualizar las herramientas para administrar el nuevo formato.](./monitor-diagnostic-logs-append-blobs.md) 
>
> 

## <a name="diagnostic-settings"></a>Configuración de diagnóstico

Los registros de diagnóstico de recursos se configuran mediante la configuración de diagnóstico de recursos. Los registros de diagnóstico de inquilino se configuran mediante la configuración de diagnóstico de inquilino. **Configuración de diagnóstico** para un control de servicio:

* Dónde se envían los registros de diagnóstico y las métricas (cuenta de almacenamiento, Event Hubs o Log Analytics).
* Qué categorías de registro se envían y si se envían también datos de métrica.
* Cuánto tiempo se debe conservar cada categoría de registro en una cuenta de almacenamiento
    - Una retención de cero días significa que los registros se conservan de forma indefinida. De lo contrario, el valor puede ser cualquier número de días comprendido entre 1 y 2147483647.
    - Si se establecen directivas de retención, pero el almacenamiento de registros en una cuenta de almacenamiento está deshabilitado (por ejemplo, si solo se han seleccionado las opciones Event Hubs o Log Analytics), las directivas de retención no surten ningún efecto.
    - Las directivas de retención se aplican a diario, por lo que al final de un día (UTC) se eliminan los registros del día que quede fuera de la directiva de retención. Por ejemplo, si tuviera una directiva de retención de un día, se eliminarían los registros de anteayer al principio del día de hoy. El proceso de eliminación empieza a medianoche (UTC), pero tenga en cuenta que eliminar los registros de la cuenta de almacenamiento puede tardar hasta 24 horas.

Estas opciones se configuran con facilidad a través de la configuración de los diagnósticos en Azure Portal, mediante los comandos de Azure PowerShell y de la CLI, o mediante la [API de REST de Azure Monitor](https://docs.microsoft.com/rest/api/monitor/).

> [!NOTE]
> Actualmente no se admite el envío de métricas de varias dimensiones a través de la configuración de diagnóstico. Las métricas con dimensiones se exportan como métricas unidimensionales planas agregadas a través de los valores de dimensión.
>
> *Por ejemplo*: la métrica "Mensajes entrantes" de una instancia de Event Hub se puede explorar y representar gráficamente por colas. Sin embargo, cuando se exporta a través de la configuración de diagnóstico, la métrica se representará con todos los mensajes entrantes de todas las colas de Event Hub.
>
>

## <a name="how-to-enable-collection-of-diagnostic-logs"></a>Cómo habilitar la recopilación de registros de diagnóstico

La recopilación de registros de diagnóstico de se puede habilitar [como parte de la creación de un recurso en una plantilla de Resource Manager](./monitoring-enable-diagnostic-logs-using-template.md) o después de crear un recurso mediante la página del recurso en el portal. También puede habilitar la recolección en cualquier momento mediante comandos de Azure PowerShell o de la CLI, o con la API de REST de Azure Monitor.

> [!TIP]
> Es posible que estas instrucciones no se apliquen directamente a cada recurso. Consulte los vínculos de esquema al final de esta página para ver los pasos especiales que se pueden aplicar a determinados tipos de recursos.

### <a name="enable-collection-of-diagnostic-logs-in-the-portal"></a>Habilitación de la recopilación de registros de diagnóstico en el portal

Puede habilitar la recopilación de registros de diagnóstico de recursos en Azure Portal una vez creado un recurso yendo a un recurso concreto o a Azure Monitor. Para habilitar esta opción a través de Azure Monitor:

1. En [Azure Portal](http://portal.azure.com), desplácese a Azure Monitor y haga clic en **Configuración de diagnóstico**

    ![Sección de supervisión de Azure Monitor](media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-blade.png)

2. Si lo desea, filtre la lista por tipo de recurso o por grupo de recursos y, a continuación, haga clic en el recurso para el que desea establecer la configuración de diagnóstico.

3. Si no existe ninguna configuración en el recurso que ha seleccionado, se le pide que cree una. Haga clic en "Activar diagnóstico".

   ![Agregar configuración de diagnóstico: sin configuración actual](media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-none.png)

   Si hay una configuración actual en el recurso, verá una lista de opciones ya configuradas en este recurso. Haga clic en "Agregar configuración de diagnóstico".

   ![Agregar configuración de diagnóstico: configuración actual](media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-multiple.png)

3. Asigne un nombre a su configuración, active las casillas para cada destino al que le gustaría enviar datos y configure el recurso que se utiliza para cada uno. Además, puede establecer un número de días para conservar estos registros mediante los controles deslizantes de **Retención (días)** (solo aplicable al destino de la cuenta de almacenamiento). Con una retención de cero días, los registros se almacenan indefinidamente.

   ![Agregar configuración de diagnóstico: configuración actual](media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-configure.png)

4. Haga clic en **Save**(Guardar).

Transcurridos unos instantes, la nueva configuración aparece en la lista de opciones para este recurso y los registros de diagnóstico se envían a los destinos especificados en cuanto se generan nuevos datos de eventos.

La configuración de diagnóstico de inquilino solo puede realizarse en la hoja del portal para el servicio de inquilino; estos valores no aparecen en la hoja de configuración de diagnóstico de Azure Monitor. Por ejemplo, los registros de auditoría de Azure Active Directory se configuran en **Configuración de los datos de exportación** en la hoja Registro de auditoría.

![Configuración de diagnóstico de AAD](./media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-aad.png)

### <a name="enable-collection-of-resource-diagnostic-logs-via-powershell"></a>Habilitación de la recopilación de registros de diagnóstico de recursos mediante PowerShell

Para habilitar la recopilación de registros de diagnóstico de recursos con Azure PowerShell, use los siguientes comandos:

Para habilitar el almacenamiento de registros de diagnóstico en una cuenta de almacenamiento, use este comando:

```powershell
Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -StorageAccountId [your storage account id] -Enabled $true
```

El identificador de la cuenta de almacenamiento es el identificador de recurso para la cuenta de almacenamiento a la que desea enviar los registros.

Para habilitar el streaming de registros de diagnóstico a un centro de eventos, use este comando:

```powershell
Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -ServiceBusRuleId [your Service Bus rule id] -Enabled $true
```

El identificador de regla de Service Bus es una cadena con este formato: `{Service Bus resource ID}/authorizationrules/{key name}`.

Para habilitar el envío de registros de diagnóstico a un área de trabajo de Log Analytics, use este comando:

```powershell
Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -WorkspaceId [resource id of the log analytics workspace] -Enabled $true
```

Puede obtener el identificador de recurso de su área de trabajo de Log Analytics con el comando siguiente:

```powershell
(Get-AzureRmOperationalInsightsWorkspace).ResourceId
```

Puede combinar estos parámetros para habilitar varias opciones de salida.

Actualmente, no puede configurar opciones de diagnóstico de inquilino mediante Azure PowerShell.

### <a name="enable-collection-of-resource-diagnostic-logs-via-the-azure-cli"></a>Habilitación de la recopilación de registros de diagnóstico de recursos mediante la CLI de Azure

Para habilitar la recopilación de registros de diagnóstico de recursos a través de la CLI de Azure, utilice el comando [az monitor diagnostic-settings create](/cli/azure/monitor/diagnostic-settings#az-monitor-diagnostic-settings-create).

Para habilitar el almacenamiento de registros de diagnóstico en una cuenta de almacenamiento:

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

El argumento `--resource-group` solo es obligatorio si `--storage-account` no es un identificador de objeto.

Para habilitar el streaming de registros de diagnóstico a un centro de eventos:

```azurecli
az monitor diagnostic-settings create --name <diagnostic name> \
    --event-hub <event hub name> \
    --event-hub-rule <event hub rule ID> \
    --resource <target resource object ID> \
    --logs '[
    {
        "category": <category name>,
        "enabled": true
    }
    ]'
```

El identificador de regla es una cadena con este formato: `{Service Bus resource ID}/authorizationrules/{key name}`.

Para habilitar el envío de registros de diagnóstico a un área de trabajo de Log Analytics:

```azurecli
az monitor diagnostic-settings create --name <diagnostic name> \
    --workspace <log analytics name or object ID> \
    --resource <target resource object ID> \
    --resource-group <log analytics workspace resource group> \
    --logs '[
    {
        "category": <category name>,
        "enabled": true
    }
    ]'
```

El argumento `--resource-group` solo es obligatorio si `--workspace` no es un identificador de objeto.

Con cualquier comando, puede agregar categorías adicionales al registro de diagnóstico mediante la incorporación de diccionarios a la matriz JSON que se pasa como el parámetro `--logs`. Puede combinar los parámetros `--storage-account`, `--event-hub` y `--workspace` para habilitar varias opciones de salida.

Actualmente, no puede configurar opciones de diagnóstico de inquilino mediante la CLI.

### <a name="enable-collection-of-resource-diagnostic-logs-via-rest-api"></a>Habilitación de la recopilación de registros de diagnóstico de recursos mediante la API de REST

Para cambiar la configuración de diagnóstico con la API de REST de Azure Monitor, consulte [este documento](https://docs.microsoft.com/rest/api/monitor/).

Actualmente, no puede configurar opciones de diagnóstico de inquilino mediante la API de REST de Azure Monitor.

## <a name="manage-resource-diagnostic-settings-in-the-portal"></a>Administración de la configuración de diagnóstico de recursos en el portal

Asegúrese de que todos los recursos estén instalados con la configuración de diagnóstico. Vaya a **Monitor** en el portal y abra **Configuración de diagnóstico**.

![Hoja Registros de diagnóstico en el portal](./media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-nav.png)

Puede que tenga que hacer clic en "Todos los servicios" para encontrar la sección Supervisión.

Aquí puede ver y filtrar todos los recursos que admiten la configuración de diagnósticos y comprobar si está habilitada para los diagnósticos. También puede explorar en profundidad para comprobar si hay varias opciones de configuración establecidas en un recurso y a qué cuenta de almacenamiento, espacio de nombres de Event Hubs o área de trabajo de Log Analytics se transmiten los datos.

![Resultados de los registros de diagnóstico en el portal](./media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-blade.png)

Al agregar una opción de configuración de diagnóstico, se abre la vista Configuración de diagnóstico, donde puede habilitar, deshabilitar o modificar la configuración de diagnóstico para el recurso seleccionado.

## <a name="supported-services-categories-and-schemas-for-diagnostic-logs"></a>Servicios admitidos, categorías y esquemas para los registros de diagnóstico

[Consulte este artículo](monitoring-diagnostic-logs-schema.md) para obtener una lista completa de los servicios admitidos y las categorías de registro y los esquemas utilizados por esos servicios.

## <a name="next-steps"></a>Pasos siguientes

* [Transmisión de registros de diagnóstico a **Event Hubs**](monitoring-stream-diagnostic-logs-to-event-hubs.md)
* [Cambio de la configuración de diagnóstico de recursos con la API de REST de Azure Monitor](https://msdn.microsoft.com/library/azure/dn931931.aspx)
* [Análisis de registros desde Azure Storage con Log Analytics](../log-analytics/log-analytics-azure-storage.md)
