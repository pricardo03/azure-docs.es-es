---
title: Información general sobre el registro de actividad de Azure
description: Aprenda qué es el registro de actividad de Azure y cómo puede usarlo para comprender los eventos que se producen en su suscripción de Azure.
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/30/2018
ms.author: johnkem
ms.subservice: logs
ms.openlocfilehash: f5d09600829a3efa2682721bf4aedbed7e5cf969
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2019
ms.locfileid: "54439075"
---
# <a name="monitor-subscription-activity-with-the-azure-activity-log"></a>Supervise la actividad de suscripción con Azure Activity Log

**Azure Activity Log** es un registro de suscripción que proporciona información sobre los eventos de nivel de suscripción que se han producido en Azure. Esta incluye una serie de datos, desde datos operativos de Azure Resource Manager hasta actualizaciones en eventos de Estado del servicio. El Registro de actividad se conocía anteriormente como "Registros de auditoría" o "Registros operativos", ya que la categoría Administrativo notifica eventos del plano de control de las suscripciones. Con el Registro de actividades, se pueden determinar los interrogantes “qué, quién y cuándo” de las operaciones de escritura (PUT, POST, DELETE) en los recursos de la suscripción. También puede conocer el estado de la operación y otras propiedades relevantes. El registro de actividad no incluye las operaciones de lectura (GET) ni las operaciones de los recursos que usan el modelo Clásico/"RDFE".

![Comparación de los registros de actividad y otros tipos de registros ](./media/activity-logs-overview/Activity_Log_vs_other_logs_v5.png)

Figura 1: Comparación de los registros de actividad y otros tipos de registros

El registro de actividad es distinto de los [registros de diagnóstico](diagnostic-logs-overview.md). El Registro de actividad proporciona datos sobre las operaciones en un recurso desde el exterior (el "plano de control"). Los registros de diagnóstico son emitidos por un recurso y proporcionan información sobre el funcionamiento de dicho recurso (el "plano de datos").

> [!WARNING]
> El registro de actividad de Azure sirve principalmente para las actividades que se producen en Azure Resource Manager. No hace seguimiento de los recursos que usan el modelo Clásico/RDFE. Algunos tipos de recursos clásicos tienen un proveedor de recursos de servidor proxy en Azure Resource Manager (por ejemplo, Microsoft.ClassicCompute). Si interactúa con un tipo de recurso clásico a través de Azure Resource Manager con estos proveedores de recursos de servidor proxy, las operaciones aparecen en el registro de actividad. Si interactúa con un tipo de recurso clásico fuera de los servidores proxy de Azure Resource Manager, sus acciones solo se registran en el registro de operaciones. Se puede examinar el registro de operaciones en una sección independiente del portal.
>
>

Puede recuperar los eventos del registro de actividad mediante Azure Portal, la CLI, los cmdlets de PowerShell y la API de REST de Azure Monitor.

> [!NOTE]
> [Las alertas más recientes](../../azure-monitor/platform/alerts-overview.md) ofrecen una experiencia mejorada para crear y administrar las reglas de alerta del registro de actividad.  [Más información](../../azure-monitor/platform/alerts-activity-log.md).


## <a name="categories-in-the-activity-log"></a>Categorías del Registro de actividad
El Registro de actividad contiene varias categorías de datos. Para obtener todos los detalles sobre el esquema de datos de estas categorías, [vea este artículo](../../azure-monitor/platform/activity-log-schema.md). Entre ellas se incluyen las siguientes:
* **Administrativo**: esta categoría contiene el registro de todas las operaciones de creación, actualización, eliminación y acción realizadas a través de Resource Manager. Los ejemplos de los tipos de eventos que aparecen en esta categoría incluyen "crear máquina virtual" y "eliminar grupo de seguridad de red". Cada acción realizada por un usuario o una aplicación mediante Resource Manager se modela como una operación en un tipo de recurso determinado. Si el tipo de operación es Write, Delete o Action, los registros de inicio y corrección o error de esa operación se registran en la categoría Administrativo. La categoría Administrativo también incluye los cambios realizados en el control de acceso basado en roles de una suscripción.
* **Estado del servicio**: esta categoría contiene el registro de los incidentes de estado del servicio que se han producido en Azure. Un ejemplo del tipo de evento que aparece en esta categoría es "SQL Azure en el este de EE. UU. está experimentando un tiempo de inactividad". Los eventos de estado del servicio pueden encuadrarse dentro de cinco variedades: Acción requerida, Recuperación asistida, Incidente, Mantenimiento, Información o Seguridad, y solo aparecen si tiene un recurso en la suscripción que se podría ver afectado por el evento.
* **Resource Health**: esta categoría contiene el registro de los eventos de estado del servicio que se han producido en los recursos de Azure. Un ejemplo del tipo de evento que aparece en esta categoría es "El estado de mantenimiento de la máquina virtual se cambió a No disponible". Los eventos de mantenimiento de recursos pueden representar uno de los cuatro estados de mantenimiento siguientes: Disponible, No disponible, Degradado y Desconocido. Además, los eventos de mantenimiento de recursos se pueden clasificar como iniciados por la plataforma o por el usuario.
* **Alerta**: esta categoría contiene el registro de todas las activaciones de alertas de Azure. Un ejemplo del tipo de evento que aparece en esta categoría es "el % de CPU en myVM ha estado por encima de 80 durante los últimos 5 minutos". Varios sistemas de Azure tienen un concepto de alerta: puede definir una regla de algún tipo y recibir una notificación cuando las condiciones coincidan con esa regla. Cada vez que un tipo de alerta de Azure compatible "se activa" o se cumplen las condiciones para generar una notificación, también se inserta un registro de la activación en esta categoría del Registro de actividad.
* **Escalado automático**: esta categoría contiene el registro de los eventos relacionados con el funcionamiento del motor de escalado automático en función de cualquier configuración de escalado automático que haya definido en la suscripción. Un ejemplo del tipo de evento que aparece en esta categoría es "Error de acción de escalado automático". Con el escalado automático puede escalar horizontalmente o reducir horizontalmente de forma automática el número de instancias de un tipo de recurso compatible en función de la hora del día o cargar datos (métricas) mediante una configuración de escalado automático. Cuando se cumplen las condiciones para escalar o reducir verticalmente, se registran los eventos de inicio y corrección o error en esta categoría.
* **Recomendación**: esta categoría contiene eventos de recomendación de Azure Advisor.
* **Seguridad**: esta categoría contiene el registro de todas las alertas que generó Azure Security Center. Un ejemplo del tipo de evento que vería en esta categoría es "Se ejecutó un archivo de extensión doble sospechoso".
* **Directiva**: esta categoría no contiene ningún evento; está reservada para uso futuro. 

## <a name="event-schema-per-category"></a>Esquema de eventos por categoría
[Vea este artículo para comprender el esquema de eventos de Activity Log por categoría.](../../azure-monitor/platform/activity-log-schema.md)

## <a name="what-you-can-do-with-the-activity-log"></a>Qué se puede hacer con el registro de actividad
Estas son algunas de las cosas que puede hacer con el registro de actividad:

![Registro de actividad de Azure](./media/activity-logs-overview/Activity_Log_Overview_v3.png)


* Verlo y realizar consultas en él en **Azure Portal**.
* [Crear una alerta basada en un evento de Activity Log.](../../azure-monitor/platform/activity-log-alerts.md)
* [Transmitirlo al **Centro de eventos**](../../azure-monitor/platform/activity-logs-stream-event-hubs.md) para la ingestión en un servicio de terceros o una solución de análisis personalizado como PowerBI.
* Analizarlo en PowerBI con el [**paquete de contenido de PowerBI**](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs/).
* [Guardarlo en una **cuenta de almacenamiento** para archivarlo o inspeccionarlo manualmente](../../azure-monitor/platform/archive-activity-log.md). Puede especificar el tiempo de retención (en días) mediante el **perfil de registro**.
* Consultarlo mediante un cmdlet de PowerShell, la CLI o la API de REST.

## <a name="query-the-activity-log-in-the-azure-portal"></a>Consulta de Activity Log en Azure Portal
En Azure Portal puede ver el registro de actividad en varios lugares:
* El **Registro de actividad** al que tiene acceso buscando Registro de actividad en **Todos los servicios** en el panel de navegación izquierdo.
* **Monitor** aparece de forma predeterminada en el panel de navegación izquierdo. El Registro de actividad es una de las secciones de Azure Monitor.
* La mayoría de los **recursos**, por ejemplo, la hoja de configuración de una máquina virtual. El registro de actividad es una sección que hay en la mayoría de las hojas de recursos y, al hacer clic en ella, se filtran automáticamente los eventos relacionados con ese recurso concreto.

En Azure Portal, puede filtrar Activity Log por estos campos:
* Intervalo de tiempo: la hora de inicio y finalización para los eventos.
* Categoría: la categoría de los eventos, como se describió anteriormente.
* Suscripción: uno o más nombres de suscripción de Azure.
* Grupo de recursos: uno o más grupos de recursos dentro de esas suscripciones.
* Recurso (nombre): el nombre de un recurso específico.
* Tipo de recurso: el tipo de recurso, por ejemplo, Microsoft.Compute/virtualmachines.
* Nombre de la operación: el nombre de una operación de Azure Resource Manager, por ejemplo, Microsoft.SQL/servers/Write.
* Gravedad: el nivel de gravedad del evento (informativo, advertencia, error, crítico).
* Evento iniciado por: el 'llamador' o el usuario que realizó la operación.
* Abrir búsqueda: se trata de un cuadro de búsqueda de texto abierto que busca esa cadena en todos los campos de todos los eventos.

Una vez que haya definido un conjunto de filtros, puede anclar una consulta en el panel de Azure para estar siempre al tanto de eventos específicos.

Para ampliar aún más las posibilidades, puede hacer clic en el icono **Registros**, que muestra los datos del registro de actividad en la [solución Activity Log Analytics de Log Analytics](../../azure-monitor/platform/collect-activity-logs.md). La hoja Log Analytics ofrece una experiencia básica de exploración/filtro en registros, pero Log Analytics le permite dinamizar, consultar y visualizar sus datos de otras formas más eficaces.

## <a name="export-the-activity-log-with-a-log-profile"></a>Exportación del registro de actividad con perfiles de registro
Un **perfil de registro** controla cómo se exporta el registro de actividad. Con un perfil de registro, puede configurar:

* Dónde se debería enviar el registro de actividad (cuenta de almacenamiento o Event Hubs)
* Qué categorías de eventos (Escritura, Eliminación, Acción) se deberían enviar. *El significado de "categoría" en los eventos de registro de actividad y los perfiles de registro es distinto. En el perfil de registro, "Categoría" representa el tipo de operación (escritura, eliminación, acción). En un evento de registro de actividad, la propiedad de "categoría" representa el origen o el tipo de un evento (por ejemplo, Administration, ServiceHealth, Alert, etc.).*
* Qué regiones (ubicaciones) se deben exportar. Asegúrese de que incluye "global", ya que muchos eventos en Activity Log son eventos globales.
* El tiempo durante el que se debe conservar un registro de actividad en una cuenta de almacenamiento.
    - Una retención de cero días significa que los registros se conservan de forma indefinida. De lo contrario, el valor puede ser cualquier número de días comprendido entre 1 y 2147483647.
    - Si se establecen directivas de retención, pero el almacenamiento de registros en una cuenta de almacenamiento está deshabilitado (por ejemplo, si solo se han seleccionado las opciones Event Hubs o Log Analytics), las directivas de retención no surten ningún efecto.
    - Las directivas de retención se aplican a diario, por lo que al final de un día (UTC) se eliminan los registros del día que quede fuera de la directiva de retención. Por ejemplo, si tuviera una directiva de retención de un día, se eliminarían los registros de anteayer al principio del día de hoy. El proceso de eliminación empieza a medianoche (UTC), pero tenga en cuenta que eliminar los registros de la cuenta de almacenamiento puede tardar hasta 24 horas.

Puede usar una cuenta de almacenamiento o un espacio de nombres de centro de eventos que no esté en la misma suscripción que el que emite los registros. El usuario que configura los ajustes debe tener el acceso de RBAC adecuado a ambas suscripciones.

> [!NOTE]
>  Actualmente no puede archivar datos en una cuenta de almacenamiento que está detrás de una red virtual protegida.

> [!WARNING]
> El formato de los datos de registro de la cuenta de almacenamiento ha cambiado a JSON Lines el 1 de noviembre de 2018. [Consulte este artículo para obtener una descripción de la repercusión y del modo de actualizar las herramientas para administrar el nuevo formato.](./../../azure-monitor/platform/diagnostic-logs-append-blobs.md) 
>
> 

Estas opciones se pueden configurar a través de la opción de exportación en la hoja de registro de actividad en el portal. También pueden configurarse mediante programación [con la API de REST de Azure Monitor](https://msdn.microsoft.com/library/azure/dn931927.aspx), los cmdlets de PowerShell o con la CLI. Una suscripción solo puede tener un perfil de registro.

### <a name="configure-log-profiles-using-the-azure-portal"></a>Configuración de perfiles de registro mediante Azure Portal
Puede transmitir el registro de actividad a un centro de eventos o almacenarlo en una cuenta de almacenamiento mediante la opción "Exportar al Centro de eventos" de Azure Portal.

1. Vaya a la hoja **Registro de actividad** mediante el menú en el lado izquierdo del portal.

    ![Ir al registro de actividad en el portal](./media/activity-logs-overview/activity-logs-portal-navigate-v2.png)
2. Haga clic en el botón **Exportar al Centro de eventos** en la parte superior de la hoja.

    ![Botón Exportar en el portal](./media/activity-logs-overview/activity-logs-portal-export-v2.png)
3. En la hoja que aparece, puede seleccionar:  
  * Las regiones para las que le gustaría exportar eventos
  * La cuenta de almacenamiento en la que desea guardar los eventos
  * El número de días que desea conservar estos eventos en el almacenamiento. Un valor de 0 días conserva los registros para siempre.
  * El espacio de nombres de Service Bus en el que quiere que se cree un centro de eventos para transmitir estos eventos.

     ![Exportar en hoja de registro de actividad](./media/activity-logs-overview/activity-logs-portal-export-blade.png)
4. Haga clic en **Guardar** para guardar la configuración. La configuración se aplica inmediatamente a la suscripción.

### <a name="configure-log-profiles-using-the-azure-powershell-cmdlets"></a>Configuración de perfiles de registro mediante cmdlets de Azure PowerShell

#### <a name="get-existing-log-profile"></a>Obtención del perfil de registro existente

```
Get-AzureRmLogProfile
```

#### <a name="add-a-log-profile"></a>Incorporación de un perfil de registro

```
Add-AzureRmLogProfile -Name my_log_profile -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Location global,westus,eastus -RetentionInDays 90 -Category Write,Delete,Action
```

| Propiedad | Obligatorio | DESCRIPCIÓN |
| --- | --- | --- |
| NOMBRE |SÍ |Nombre de su perfil de registro. |
| StorageAccountId |Sin  |Identificador de recurso de la cuenta de almacenamiento donde se debe guardar el registro de actividad. |
| serviceBusRuleId |Sin  |Identificador de regla de Service Bus para el espacio de nombres de Service Bus donde desea que se creen los centros de eventos. Es una cadena con este formato: `{service bus resource ID}/authorizationrules/{key name}`. |
| Ubicación |SÍ |Lista separada por comas de las regiones para las que desea recopilar eventos del registro de actividad. |
| RetentionInDays |SÍ |Número de días que deben retenerse los eventos, entre 1 y 2147483647. Con el valor cero, se almacenan los registros indefinidamente. |
| Categoría |Sin  |Lista separada por comas de las categorías de eventos que deben recopilarse. Los valores posibles son Write, Delete y Action. |

#### <a name="remove-a-log-profile"></a>Eliminación de un perfil de registro
```
Remove-AzureRmLogProfile -name my_log_profile
```

### <a name="configure-log-profiles-using-the-azure-cli"></a>Configuración de perfiles de registro mediante la CLI de Azure

#### <a name="get-existing-log-profile"></a>Obtención del perfil de registro existente

```azurecli
az monitor log-profiles list
az monitor log-profiles show --name <profile name>
```

La propiedad `name` debe ser el nombre del perfil de registro.

#### <a name="add-a-log-profile"></a>Incorporación de un perfil de registro

```azurecli
az monitor log-profiles create --name <profile name> \
    --locations <location1 location2 ...> \
    --location <location> \
    --categories <category1 category2 ...>
```

Para obtener la documentación completa para crear un perfil de monitor con la CLI, consulte la [referencia de comandos de la CLI](/cli/azure/monitor/log-profiles#az-monitor-log-profiles-create).

#### <a name="remove-a-log-profile"></a>Eliminación de un perfil de registro

```azurecli
az monitor log-profiles delete --name <profile name>
```

## <a name="next-steps"></a>Pasos siguientes
* [Más información sobre el registro de actividad (antes, Registros de auditoría)](../../azure-resource-manager/resource-group-audit.md)
* [Transmisión del registro de actividad de Azure a Event Hubs](../../azure-monitor/platform/activity-logs-stream-event-hubs.md)

