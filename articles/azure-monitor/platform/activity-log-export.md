---
title: Exportar el registro de actividad de Azure
description: Exportar registro de actividad de Azure en el almacenamiento para archivar o Azure Event Hubs para la exportación fuera de Azure.
author: bwren
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: bwren
ms.subservice: logs
ms.openlocfilehash: acf2526e79519e610614dc5217efbfe5e327b90f
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66248150"
---
# <a name="export-azure-activity-log-to-storage-or-azure-event-hubs"></a>Exportar el registro de actividad de Azure storage o Azure Event Hubs
El [Azure Activity Log](activity-logs-overview.md) proporciona información sobre los eventos de nivel de suscripción que se han producido en su suscripción de Azure. Además de ver el registro de actividad en el portal de Azure o cópielo en un área de trabajo de Log Analytics donde se puede analizar con otros datos recopilados por Azure Monitor, puede crear un perfil de registro para archivar el registro de actividad a una cuenta de almacenamiento de Azure o transmitirlo a un  Centro de eventos.

## <a name="archive-activity-log"></a>Archivar el registro de actividad
Archivar el registro de actividad a una cuenta de almacenamiento es útil si desea conservar los datos de registro más de 90 días (con control total sobre la directiva de retención) para auditorías, análisis estático o copia de seguridad. Si solo necesita conservar los eventos durante 90 días o menos, no es necesario configurar el archivado en una cuenta de almacenamiento, ya que los eventos de registro de actividad se conservan en la plataforma de Azure durante 90 días.

## <a name="stream-activity-log-to-event-hub"></a>Registro de actividad Stream al centro de eventos
[Azure Event Hubs](/azure/event-hubs/) es un servicio de ingesta de eventos y la plataforma que puede recibir de streaming de datos y procesar millones de eventos por segundo. Los datos enviados a un centro de eventos se pueden transformar y almacenar con cualquier proveedor de análisis en tiempo real o adaptadores de procesamiento por lotes y almacenamiento. Dos formas de usar la funcionalidad de streaming para el registro de actividad son:
* **Transmisión a sistemas de registro y telemetría de terceros**: con el tiempo, el streaming de Azure Event Hubs se convertirá en el mecanismo para canalizar el registro de actividad a sistemas de información de seguridad y administración de eventos (SIEM) y soluciones de análisis de registros de terceros.
* **Creación de una plataforma personalizada de registro y telemetría**: si ya tiene una plataforma de telemetría integrada personalizada o está pensando en crear una, la gran escalabilidad en cuanto a suscripción y publicación de Event Hubs permite ingerir el registro de actividad de manera flexible. 

## <a name="prerequisites"></a>Requisitos previos

### <a name="storage-account"></a>Cuenta de almacenamiento
Si va a archivar los registros de actividad, deberá [crear una cuenta de almacenamiento](../../storage/common/storage-quickstart-create-account.md) si aún no tiene uno. No se debe usar una cuenta de almacenamiento existente que tiene datos de otros, sin supervisión almacenados en ella para que se puede controlar mejor el acceso a datos de supervisión. Si también va a archivar los registros de diagnóstico y métricas para una cuenta de almacenamiento sin embargo, puede usar esa misma cuenta de almacenamiento para mantener todos los datos de supervisión en una ubicación central.

La cuenta de almacenamiento no tiene que estar en la misma suscripción que la que emite los registros, siempre que el usuario que configura la configuración tenga acceso RBAC adecuado a ambas suscripciones.
> [!NOTE]
>  Actualmente no puede archivar datos en una cuenta de almacenamiento que está detrás de una red virtual protegida.

### <a name="event-hubs"></a>Event Hubs
Si va a enviar el registro de actividad a un centro de eventos, deberá [crear un centro de eventos](../../event-hubs/event-hubs-create.md) si aún no tiene uno. Si anteriormente se han transmitido eventos del registro de actividad para este espacio de nombres de Event Hubs, a continuación, se reutilizarán ese centro de eventos.

La directiva de acceso compartido define los permisos que tiene el mecanismo de transmisión. Streaming a Event Hubs requiere permisos de administración, envío y escucha. Puede crear o modificar las directivas de acceso compartido para el espacio de nombres de Event Hubs en Azure portal en la pestaña configurar para el espacio de nombres de Event Hubs.

Para actualizar el perfil de registro del registro de actividad para incluir el streaming, debe tener el permiso ListKey en esa regla de autorización de Event Hubs. El espacio de nombres de Event Hubs no tiene que estar necesariamente en la misma suscripción que la suscripción que emite los registros, siempre que el usuario que configure el valor tenga el acceso RBAC adecuado a ambas suscripciones y estas se encuentren en el mismo inquilino de AAD.

Stream el registro de actividad a un centro de eventos por [crear un perfil de registro](#create-a-log-profile).

## <a name="create-a-log-profile"></a>Crear un perfil de registro
Define cómo se encuentra el registro de actividad de Azure exportado mediante un **perfil de registro**. Cada suscripción de Azure solo puede tener un perfil de registro. Estas opciones se pueden configurar a través de la **exportar** opción en la hoja de registro de actividad en el portal. También pueden configurarse mediante programación [con la API de REST de Azure Monitor](https://msdn.microsoft.com/library/azure/dn931927.aspx), los cmdlets de PowerShell o con la CLI.

El perfil de registro define lo siguiente.

**Donde se debe enviar el registro de actividad.** Actualmente, las opciones disponibles son la cuenta de almacenamiento o Event Hubs.

**Qué categorías de eventos deben enviarse.** El significado de *categoría* en el registro de actividad y perfiles de registro de eventos es diferente. En el perfil de registro, *categoría* representa el tipo de operación (escritura, eliminación, acción). En un evento de registro de actividad, el *categoría*"* propiedad representa el origen o el tipo de evento (por ejemplo, Administration, ServiceHealth y alerta).

**Qué regiones (ubicaciones) se deben exportar.** Debe incluir todas las ubicaciones, puesto que muchos eventos en el registro de actividad son eventos globales.

**¿Durante cuánto tiempo deben retenerse el registro de actividad en una cuenta de almacenamiento.** Una retención de cero días significa que los registros se conservan de forma indefinida. De lo contrario, el valor puede ser cualquier número de días comprendido entre 1 y 2147483647.

Si se establecen las directivas de retención, pero está deshabilitado el almacenamiento de registros en una cuenta de almacenamiento, las directivas de retención no tienen ningún efecto. Las directivas de retención se aplican a diario, por lo que al final de un día (UTC) se eliminan los registros del día que quede fuera de la directiva de retención. Por ejemplo, si tuviera una directiva de retención de un día, se eliminarían los registros de anteayer al principio del día de hoy. El proceso de eliminación empieza a medianoche (UTC), pero tenga en cuenta que eliminar los registros de la cuenta de almacenamiento puede tardar hasta 24 horas.



> [!WARNING]
> El formato de los datos de registro de la cuenta de almacenamiento ha cambiado a JSON Lines el 1 de noviembre de 2018. [Consulte este artículo para obtener una descripción de la repercusión y del modo de actualizar las herramientas para administrar el nuevo formato.](diagnostic-logs-append-blobs.md)
>
>

### <a name="create-log-profile-using-the-azure-portal"></a>Crear perfil de registro mediante el portal de Azure

Crear o editar un perfil de registro con el **exportar a centro de eventos** opción en el portal de Azure.

1. Desde el **Monitor** menú en Azure portal, seleccione **exportar a centro de eventos**.

    ![Botón Exportar en el portal](media/activity-log-export/portal-export.png)

3. En la hoja que aparece, especifique lo siguiente:
   * Regiones con los eventos para exportar. Debe seleccionar todas las regiones para garantizar que no se pierda los eventos clave puesto que el registro de actividad es un registro (no regional) global y por lo que la mayoría de los eventos no tiene una región asociada con ellos. 
   * Si desea escribir en la cuenta de almacenamiento:
       * La cuenta de almacenamiento al que le gustaría guardar los eventos.
       * el número de días que desea conservar estos eventos en el almacenamiento. Un valor de 0 días conserva los registros para siempre.
   * Si desea escribir en el centro de eventos:
       * Namespace de Bus de servicio en la que desea que un centro de eventos que deben crearse para transmitir estos eventos.

     ![Exportar en hoja de registro de actividad](./media/activity-logs-overview/activity-logs-portal-export-blade.png)


4. Haga clic en **Guardar** para guardar la configuración. La configuración se aplica inmediatamente a la suscripción.


### <a name="configure-log-profile-using-powershell"></a>Configurar el perfil de registro con PowerShell

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Si ya existe un perfil de registro, primero deberá quitar el perfil de registro existente y, a continuación, cree uno nuevo.

1. Use `Get-AzLogProfile` para identificar si existe un perfil de registro.  Si existe un perfil de registro, tenga en cuenta la *nombre* propiedad.

1. Use `Remove-AzLogProfile` para quitar el perfil de registro mediante el valor de la propiedad *name*.

    ```powershell
    # For example, if the log profile name is 'default'
    Remove-AzLogProfile -Name "default"
    ```

3. Use `Add-AzLogProfile` para crear un nuevo perfil de registro:

    ```powershell
    Add-AzLogProfile -Name my_log_profile -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Location global,westus,eastus -RetentionInDays 90 -Category Write,Delete,Action
    ```

    | Propiedad | Obligatorio | DESCRIPCIÓN |
    | --- | --- | --- |
    | Name |Sí |Nombre de su perfil de registro. |
    | StorageAccountId |Sin  |Identificador de recurso de la cuenta de almacenamiento donde se debe guardar el registro de actividad. |
    | serviceBusRuleId |Sin  |Identificador de regla de Service Bus para el espacio de nombres de Service Bus donde desea que se creen los centros de eventos. Se trata de una cadena con el formato: `{service bus resource ID}/authorizationrules/{key name}`. |
    | Location |Sí |Lista separada por comas de las regiones para las que desea recopilar eventos del registro de actividad. |
    | RetentionInDays |Sí |Número de días que deben retenerse los eventos en la cuenta de almacenamiento, entre 1 y 2147483647. Con el valor cero, se almacenan los registros indefinidamente. |
    | Categoría |Sin  |Lista separada por comas de las categorías de eventos que deben recopilarse. Los valores posibles son _escribir_, _eliminar_, y _acción_. |

### <a name="example-script"></a>Script de ejemplo
Siguiente es un script de PowerShell de ejemplo para crear un perfil de registro que escribe el registro de actividad a ambas un centro de eventos y la cuenta de almacenamiento.

   ```powershell
   # Settings needed for the new log profile
   $logProfileName = "default"
   $locations = (Get-AzLocation).Location
   $locations += "global"
   $subscriptionId = "<your Azure subscription Id>"
   $resourceGroupName = "<resource group name your event hub belongs to>"
   $eventHubNamespace = "<event hub namespace>"

   # Build the service bus rule Id from the settings above
   $serviceBusRuleId = "/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.EventHub/namespaces/$eventHubNamespace/authorizationrules/RootManageSharedAccessKey"

   # Build the storage account Id from the settings above
   $storageAccountId = "/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.Storage/storageAccounts/$storageAccountName"

   Add-AzLogProfile -Name $logProfileName -Location $locations -ServiceBusRuleId $serviceBusRuleId
   ```


### <a name="configure-log-profile-using-azure-cli"></a>Configurar el perfil de registro mediante la CLI de Azure

Si ya existe un perfil de registro, primero debe quitar el perfil de registro existente y luego crear un nuevo perfil de registro.

1. Use `az monitor log-profiles list` para identificar si existe un perfil de registro.
2. Use `az monitor log-profiles delete --name "<log profile name>` para quitar el perfil de registro mediante el valor de la propiedad *name*.
3. Use `az monitor log-profiles create` para crear un nuevo perfil de registro:

   ```azurecli-interactive
   az monitor log-profiles create --name "default" --location null --locations "global" "eastus" "westus" --categories "Delete" "Write" "Action"  --enabled false --days 0 --service-bus-rule-id "/subscriptions/<YOUR SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventHub/namespaces/<EVENT HUB NAME SPACE>/authorizationrules/RootManageSharedAccessKey"
   ```

    | Propiedad | Obligatorio | DESCRIPCIÓN |
    | --- | --- | --- |
    | name |Sí |Nombre de su perfil de registro. |
    | storage-account-id |Sí |Identificador de recurso de la cuenta de almacenamiento donde se deben guardar los registros de actividades. |
    | locations |Sí |Lista separada por espacios de las regiones para las que desea recopilar eventos del registro de actividad. Puede ver una lista de todas las regiones para la suscripción con `az account list-locations --query [].name`. |
    | days |Sí |Número de días que deben retenerse los eventos, entre 1 y 365. Con el valor cero, se almacenarán los registros indefinidamente (de manera indefinida).  Si el valor es cero, el parámetro habilitado se debe establecer en true. |
    |enabled | Sí |True o False.  Se usa para habilitar o deshabilitar la directiva de retención.  Si el valor es True, el parámetro de días debe ser un valor mayor que 0.
    | categories |Sí |Lista separada por espacios de las categorías de eventos que deben recopilarse. Los valores posibles son Write, Delete y Action. |



## <a name="activity-log-schema"></a>Esquema de registro de actividad
Si se envía a Azure storage o Event hubs, se escribirán los datos de registro de actividad a JSON con el formato siguiente.

``` JSON
{
    "records": [
        {
            "time": "2015-01-21T22:14:26.9792776Z",
            "resourceId": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
            "operationName": "microsoft.support/supporttickets/write",
            "category": "Write",
            "resultType": "Success",
            "resultSignature": "Succeeded.Created",
            "durationMs": 2826,
            "callerIpAddress": "111.111.111.11",
            "correlationId": "c776f9f4-36e5-4e0e-809b-c9b3c3fb62a8",
            "identity": {
                "authorization": {
                    "scope": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
                    "action": "microsoft.support/supporttickets/write",
                    "evidence": {
                        "role": "Subscription Admin"
                    }
                },
                "claims": {
                    "aud": "https://management.core.windows.net/",
                    "iss": "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/",
                    "iat": "1421876371",
                    "nbf": "1421876371",
                    "exp": "1421880271",
                    "ver": "1.0",
                    "http://schemas.microsoft.com/identity/claims/tenantid": "1e8d8218-c5e7-4578-9acc-9abbd5d23315 ",
                    "http://schemas.microsoft.com/claims/authnmethodsreferences": "pwd",
                    "http://schemas.microsoft.com/identity/claims/objectidentifier": "2468adf0-8211-44e3-95xq-85137af64708",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "admin@contoso.com",
                    "puid": "20030000801A118C",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "9vckmEGF7zDKk1YzIY8k0t1_EAPaXoeHyPRn6f413zM",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "John",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "Smith",
                    "name": "John Smith",
                    "groups": "cacfe77c-e058-4712-83qw-f9b08849fd60,7f71d11d-4c41-4b23-99d2-d32ce7aa621c,31522864-0578-4ea0-9gdc-e66cc564d18c",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": " admin@contoso.com",
                    "appid": "c44b4083-3bq0-49c1-b47d-974e53cbdf3c",
                    "appidacr": "2",
                    "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
                    "http://schemas.microsoft.com/claims/authnclassreference": "1"
                }
            },
            "level": "Information",
            "location": "global",
            "properties": {
                "statusCode": "Created",
                "serviceRequestId": "50d5cddb-8ca0-47ad-9b80-6cde2207f97c"
            }
        }
    ]
}
```
Los elementos de este JSON se describen en la tabla siguiente.

| Nombre del elemento | DESCRIPCIÓN |
| --- | --- |
| time |Marca de tiempo de cuándo el servicio de Azure generó el evento que procesó la solicitud correspondiente al evento. |
| resourceId |Identificador de recurso del recurso afectado. |
| operationName |Nombre de la operación. |
| category |Categoría de la acción, p. ej. Write, Read, Action. |
| resultType |Tipo del resultado, p. ej. Success, Failure, Start |
| resultSignature |Depende del tipo de recurso. |
| durationMs |Duración de la operación en milisegundos |
| callerIpAddress |La dirección IP del usuario que ha realizado la operación, la notificación de UPN o la notificación de SPN basada en la disponibilidad. |
| correlationId |Normalmente, un GUID en formato de cadena. Los eventos que comparten correlationId pertenecen a la misma acción general. |
| identity |Blob JSON que describe la autorización y las notificaciones. |
| authorization |Blob de propiedades RBAC del evento. Normalmente incluye las propiedades "action", "role" y "scope". |
| level |Nivel del evento. Uno de los valores siguientes: _Crítica_, _Error_, _advertencia_, _informativo_, y _detallado_ |
| location |Región en la que se ha producido la ubicación (o global). |
| properties |Conjunto de pares `<Key, Value>` (es decir, diccionario) que describe los detalles del evento. |

> [!NOTE]
> Las propiedades y el uso de estas propiedades pueden variar según el recurso.



## <a name="next-steps"></a>Pasos siguientes

* [Más información sobre el registro de actividad](../../azure-resource-manager/resource-group-audit.md)
* [Recopilar el registro de actividad en los registros de Azure Monitor](activity-log-collect.md)
