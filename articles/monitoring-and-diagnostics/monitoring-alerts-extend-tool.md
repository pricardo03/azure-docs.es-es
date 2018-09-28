---
title: Extensión de alertas de Log Analytics a Azure
description: En este artículo se describen las herramientas y API mediante las cuales puede extender alertas de Log Analytics a Alertas de Azure.
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/04/2018
ms.author: vinagara
ms.component: alerts
ms.openlocfilehash: d70eecb6a5d6bafbfa6507dbe8b1bcb1cad67191
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46990253"
---
# <a name="extend-alerts-from-log-analytics-into-azure-alerts"></a>Extensión de alertas de Log Analytics a Alertas de Azure
Alertas de Azure está reemplazando la característica de alertas de Azure Log Analytics. Como parte de esta transición, las alertas que ha configurado originalmente en Log Analytics se extenderán a Azure. Si no quiere esperar a que se trasladen automáticamente a Azure, puede iniciar el proceso:

- Manualmente desde el portal de Operations Management Suite. 
- Mediante programación usando AlertsVersion API.  

> [!NOTE]
> A partir del 14 de mayo de 2018, Microsoft extenderá automáticamente las alertas creadas en las instancias de la nube pública de Log Analytics a Alertas de Azure en una serie periódica hasta que se completen. Si tiene problemas para crear [ grupos de acciones](monitoring-action-groups.md), siga [estos pasos de corrección](monitoring-alerts-extend-tool.md#troubleshooting) para crear grupos de acciones automáticamente. Puede seguir estos pasos hasta el 5 de julio de 2018. *No es aplicable para usuarios de Azure Government y la nube soberana de Log Analytics*. 

## <a name="option-1-initiate-from-the-operations-management-suite-portal"></a>Opción 1: Inicio desde portal de Operations Management Suite
En los pasos siguientes se describen cómo ampliar las alertas para el espacio de trabajo desde el portal de Operations Management Suite.  

1. En Azure Portal, seleccione **Todos los servicios**. En la lista de recursos, escriba **Log Analytics**. Cuando comience a escribir, la lista se filtrará en función de la entrada. Seleccione **Log Analytics**.
2. En el panel de suscripciones de Log Analytics, seleccione un área de trabajo y, después, seleccione el icono de **Portal de OMS**.
![Captura de pantalla del panel de suscripción a Log Analytics, con el icono del portal de OMS resaltado](./media/monitor-alerts-extend/azure-portal-01.png) 
3. Después de redirigirse al portal de Operations Management Suite, seleccione el icono **Configuración**.
![Captura de pantalla del portal de Operations Management Suite, con el icono Configuración resaltado](./media/monitor-alerts-extend/oms-portal-settings-option.png) 
4. En la página **Configuración**, seleccione **Alertas**.  
5. Seleccione **Extender a Azure**.
![Captura de pantalla de la página de configuración de alertas del portal de Operations Management Suite, con Extender a Azure resaltado](./media/monitor-alerts-extend/ExtendInto.png)
6. Aparecerá un asistente de tres pasos en el panel **Alertas**. Lea la información general y seleccione **Siguiente**.
![Captura de pantalla del paso 1 del asistente](./media/monitor-alerts-extend/ExtendStep1.png)  
7. En el segundo paso, verá un resumen de los cambios propuestos, con los [grupos de acciones](monitoring-action-groups.md) adecuados enumerados, para las alertas. Si se ven acciones similares en más de una alerta, el asistente propondrá asociar un único grupo de acciones con todas ellas.  La convención de nomenclatura es como sigue: *WorkspaceName_AG_ #Number*. Para continuar, seleccione **Siguiente**.
![Captura de pantalla del paso 2 del asistente](./media/monitor-alerts-extend/ExtendStep2.png)  
8. En el último paso del asistente, seleccione **Finalizar** y confirme cuando se le solicite para iniciar el proceso. Opcionalmente, puede proporcionar una dirección de correo electrónico, para que se le notifique cuando el proceso se complete y todas las alertas se hayan trasladado correctamente a Alertas de Azure.
![Captura de pantalla del paso 3 del asistente](./media/monitor-alerts-extend/ExtendStep3.png)

Cuando finalice el asistente, en la página **Configuración de alertas**, se eliminará la opción de extender las alertas a Azure. En segundo plano, las alertas se trasladan a Azure, y esto puede llevar algún tiempo. Durante la operación, no puede realizar cambios en las alertas del portal de Operations Management Suite. Puede ver el estado actual en el banner en la parte superior del portal. Si ha proporcionado una dirección de correo electrónico anteriormente, recibirá un correo electrónico cuando el proceso haya finalizado correctamente.  


Las alertas siguen apareciendo en el portal de Operations Management Suite, incluso después de haber sido trasladadas correctamente a Azure.
![Captura de pantalla de la página de configuración de alertas del portal Operations Management Suite](./media/monitor-alerts-extend/PostExtendList.png)


## <a name="option-2-use-the-alertsversion-api"></a>Opción 2: Uso de AlertsVersion API
Puede utilizar AlertsVersion API de Log Analytics para extender las alertas de Log Analytics a Alertas de Azure desde cualquier cliente que pueda llamar a una API REST. Puede acceder a la API desde PowerShell mediante [ARMClient](https://github.com/projectkudu/ARMClient), una herramienta de línea de comandos de código abierto. Puede mostrar los resultados en JSON.  

Para usar la API, primero hay que crear una solicitud GET. Esto evalúa y devuelve un resumen de los cambios propuestos, antes de intentar extenderse a Azure mediante una solicitud POST. Los resultados muestran las alertas y una lista propuesta de [grupos de acciones](monitoring-action-groups.md), en formato JSON. Si se ven acciones similares en más de una alerta, el servicio propone asociarlas todas a un único grupo de acciones. La convención de nomenclatura es como sigue: *WorkspaceName_AG_ #Number*.

```
armclient GET  /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview
```

Si la solicitud GET se realiza correctamente, se devuelve un código de estado HTTP 200, junto con una lista de alertas y grupos de acción propuestos en los datos JSON. Este es un ejemplo de respuesta:

```json
{
    "version": 1,
    "migrationSummary": {
        "alertsCount": 2,
        "actionGroupsCount": 2,
        "alerts": [
            {
                "alertName": "DemoAlert_1",
                "alertId": " /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/savedSearches/<savedSearchId>/schedules/<scheduleId>/actions/<actionId>",
                "actionGroupName": "<workspaceName>_AG_1"
            },
            {
                "alertName": "DemoAlert_2",
                "alertId": " /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/savedSearches/<savedSearchId>/schedules/<scheduleId>/actions/<actionId>",
                "actionGroupName": "<workspaceName>_AG_2"
            }
        ],
        "actionGroups": [
            {
                "actionGroupName": "<workspaceName>_AG_1",
                "actionGroupResourceId": "/subscriptions/<subscriptionid>/resourceGroups/<resourceGroupName>/providers/microsoft.insights/actionGroups/<workspaceName>_AG_1",
                "actions": {
                    "emailIds": [
                        "JohnDoe@mail.com"
                    ],
                    "webhookActions": [
                        {
                            "name": "Webhook_1",
                            "serviceUri": "http://test.com"
                        }
                    ],
                    "itsmAction": {}
                }
            },
            {
                "actionGroupName": "<workspaceName>_AG_1",
                "actionGroupResourceId": "/subscriptions/<subscriptionid>/resourceGroups/<resourceGroupName>/providers/microsoft.insights/actionGroups/<workspaceName>_AG_1",
                 "actions": {
                    "emailIds": [
                        "test1@mail.com",
                          "test2@mail.com"
                    ],
                    "webhookActions": [],
                    "itsmAction": {
                        "connectionId": "<Guid>",
                        "templateInfo":"{\"PayloadRevision\":0,\"WorkItemType\":\"Incident\",\"UseTemplate\":false,\"WorkItemData\":\"{\\\"contact_type\\\":\\\"email\\\",\\\"impact\\\":\\\"3\\\",\\\"urgency\\\":\\\"2\\\",\\\"category\\\":\\\"request\\\",\\\"subcategory\\\":\\\"password\\\"}\",\"CreateOneWIPerCI\":false}"
                    }
                }
            }
        ]
    }
}

```
Si el área de trabajo especificado no tiene definidas reglas de alerta, los datos JSON devuelven lo siguiente:

```json
{
    "version": 1,
    "Message": "No Alerts found in the workspace for migration."
}
```

Si todas las reglas de alertas del área de trabajo especificado ya se han extendido a Azure, la respuesta a la solicitud GET es:

```json
{
    "version": 2
}
```

Para iniciar la migración de las alertas a Azure, inicie una respuesta POST. La respuesta POST confirma su intención, así como su aceptación, de que las alertas se extiendan de Log Analytics a Azure Alerts. La actividad se programa y las alertas se procesan como se indica, en función de los resultados obtenidos al realizar la respuesta GET con anterioridad. Puede proporcionar opcionalmente una lista de direcciones de correo electrónico a las que Log Analytics envía un informe, cuando el proceso en segundo plano programado de migrar las alertas se completa correctamente. Puede usar el siguiente ejemplo de solicitud:

```
$emailJSON = “{‘Recipients’: [‘a@b.com’, ‘b@a.com’]}”
armclient POST  /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview $emailJSON
```

> [!NOTE]
> El resultado de la migración de alertas en Alertas de Azure puede variar según el resumen proporcionado por la respuesta GET. Cuando se programan, las alertas de Log Analytics no están disponibles temporalmente para su modificación en el portal de Operations Management Suite. Sin embargo, puede crear nuevas alertas. 

Si la solicitud POST se realiza correctamente, devuelve un estado HTTP 200 OK, junto con la siguiente respuesta:

```json
{
    "version": 2
}
```

Esta respuesta indica que las alertas se han extendido correctamente a Alertas de Azure. Esta propiedad de versión es solo para comprobar si las alertas se han extendido a Azure y no tienen relación a [Search API de Log Analytics](../log-analytics/log-analytics-api-alerts.md). Cuando las alertas se extienden a Azure correctamente, se envía un informe a cualquier dirección de correo electrónico proporcionada con la solicitud POST. Si todas las alertas en el área de trabajo especificado ya están programadas para su extensión, la respuesta a la solicitud POST es que el intento se ha prohibido (un código de estado 403). Para ver los mensajes de error o para entender si el proceso está bloqueado, puede enviar una solicitud GET. Si hay un mensaje de error, este se devuelve junto con la información de resumen.

```json
{
    "version": 1,
    "message": "OMS was unable to extend your alerts into Azure, Error: The subscription is not registered to use the namespace 'microsoft.insights'. OMS will schedule extending your alerts, once remediation steps illustrated in the troubleshooting guide are done.",
    "recipients": [
       "john.doe@email.com",
       "jane.doe@email.com"
     ],
    "migrationSummary": {
        "alertsCount": 2,
        "actionGroupsCount": 2,
        "alerts": [
            {
                "alertName": "DemoAlert_1",
                "alertId": " /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/savedSearches/<savedSearchId>/schedules/<scheduleId>/actions/<actionId>",
                "actionGroupName": "<workspaceName>_AG_1"
            },
            {
                "alertName": "DemoAlert_2",
                "alertId": " /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/savedSearches/<savedSearchId>/schedules/<scheduleId>/actions/<actionId>",
                "actionGroupName": "<workspaceName>_AG_2"
            }
        ],
        "actionGroups": [
            {
                "actionGroupName": "<workspaceName>_AG_1",
                "actionGroupResourceId": "/subscriptions/<subscriptionid>/resourceGroups/<resourceGroupName>/providers/microsoft.insights/actionGroups/<workspaceName>_AG_1",
                "actions": {
                    "emailIds": [
                        "JohnDoe@mail.com"
                    ],
                    "webhookActions": [
                        {
                            "name": "Webhook_1",
                            "serviceUri": "http://test.com"
                        }
                    ],
                    "itsmAction": {}
                }
            },
            {
                "actionGroupName": "<workspaceName>_AG_1",
                "actionGroupResourceId": "/subscriptions/<subscriptionid>/resourceGroups/<resourceGroupName>/providers/microsoft.insights/actionGroups/<workspaceName>_AG_1",
                 "actions": {
                    "emailIds": [
                        "test1@mail.com",
                          "test2@mail.com"
                    ],
                    "webhookActions": [],
                    "itsmAction": {
                        "connectionId": "<Guid>",
                        "templateInfo":"{\"PayloadRevision\":0,\"WorkItemType\":\"Incident\",\"UseTemplate\":false,\"WorkItemData\":\"{\\\"contact_type\\\":\\\"email\\\",\\\"impact\\\":\\\"3\\\",\\\"urgency\\\":\\\"2\\\",\\\"category\\\":\\\"request\\\",\\\"subcategory\\\":\\\"password\\\"}\",\"CreateOneWIPerCI\":false}"
                    }
                }
            }
        ]
    }
}              

```


## <a name="option-3-use-a-custom-powershell-script"></a>Opción 3: Uso de un script personalizado de PowerShell
 Si Microsoft no ha extendido correctamente sus alertas del portal de Operations Management Suite a Azure, puede hacerlo manualmente hasta el 5 de julio de 2018. Las dos opciones para la extensión manual se tratan en las dos secciones anteriores.

A partir del 5 de julio de 2018, todas las alertas del portal de Operations Management Suite se extienden a Azure. En aquellos usuarios que no hayan tomado las [medidas de corrección necesarias sugeridas](#troubleshooting), sus alertas se seguirán ejecutando sin acciones de activación o notificaciones, debido a la falta de [grupos de acción](monitoring-action-groups.md) asociados. 

Para crear manualmente [grupos de acciones](monitoring-action-groups.md) para las alertas en Log Analytics, use el siguiente script de ejemplo:
```PowerShell
########## Input Parameters Begin ###########


$subscriptionId = ""
$resourceGroup = ""
$workspaceName = "" 


########## Input Parameters End ###########

armclient login

try
{
    $workspace = armclient get /subscriptions/$subscriptionId/resourceGroups/$resourceGroup/providers/Microsoft.OperationalInsights/workspaces/"$workspaceName"?api-version=2015-03-20 | ConvertFrom-Json
    $workspaceId = $workspace.properties.customerId
    $resourceLocation = $workspace.location
}
catch
{
    "Please enter valid input parameters i.e. Subscription Id, Resource Group and Workspace Name !!"
    exit
}

# Get Extend Summary of the Alerts
"`nGetting Extend Summary of Alerts for the workspace...`n"
try
{

    $value = armclient get /subscriptions/$subscriptionId/resourceGroups/$resourceGroup/providers/Microsoft.OperationalInsights/workspaces/$workspaceName/alertsversion?api-version=2017-04-26-preview

    "Extend preview summary"
    "=========================`n"

    $value

    $result = $value | ConvertFrom-Json
}
catch
{

    $ErrorMessage = $_.Exception.Message
    "Error occured while fetching/parsing Extend summary: $ErrorMessage"
    exit 
}

if ($result.version -eq 2)
{
    "`nThe alerts in this workspace have already been extended to Azure."
    exit
}

$in = Read-Host -Prompt "`nDo you want to continue extending the alerts to Azure? (Y/N)"

if ($in.ToLower() -ne "y")
{
    exit
} 


# Check for resource provider registration
try
{
    $val = armclient get subscriptions/$subscriptionId/providers/microsoft.insights/?api-version=2017-05-10 | ConvertFrom-Json
    if ($val.registrationState -eq "NotRegistered")
    {
        $val = armclient post subscriptions/$subscriptionId/providers/microsoft.insights/register/?api-version=2017-05-10
    }
}
catch
{
    "`nThe user does not have required access to register the resource provider. Please try with user having Contributor/Owner role in the subscription"
    exit
}

$actionGroupsMap = @{}
try
{
    "`nCreating new action groups for alerts extension...`n"
    foreach ($actionGroup in $result.migrationSummary.actionGroups)
    {
        $actionGroupName = $actionGroup.actionGroupName
        $actions = $actionGroup.actions
        if ($actionGroupsMap.ContainsKey($actionGroupName))
        {
            continue
        } 
        
        # Create action group payload
        $shortName = $actionGroupName.Substring($actionGroupName.LastIndexOf("AG_"))
        $properties = @{"groupShortName"= $shortName; "enabled" = $true}
        $emailReceivers = New-Object Object[] $actions.emailIds.Count
        $webhookReceivers = New-Object Object[] $actions.webhookActions.Count
        
        $count = 0
        foreach ($email in $actions.emailIds)
        {
            $emailReceivers[$count] = @{"name" = "Email$($count+1)"; "emailAddress" = "$email"}
            $count++
        }

        $count = 0
        foreach ($webhook in $actions.webhookActions)
        {
            $webhookReceivers[$count] = @{"name" = "$($webhook.name)"; "serviceUri" = "$($webhook.serviceUri)"}
            $count++
        }

        $itsmAction = $actions.itsmAction
        if ($itsmAction.connectionId -ne $null)
        {
            $val = @{
            "name" = "ITSM"
            "workspaceId" = "$subscriptionId|$workspaceId"
            "connectionId" = "$($itsmAction.connectionId)"
            "ticketConfiguration" = $itsmAction.templateInfo
            "region" = "$resourceLocation"
            }
            $properties["itsmReceivers"] = @($val)  
        }

        $properties["emailReceivers"] = @($emailReceivers)
        $properties["webhookReceivers"] = @($webhookReceivers)
        $armPayload = @{"properties" = $properties; "location" = "Global"} | ConvertTo-Json -Compress -Depth 4

    
        # Azure Resource Manager call to create action group
        $response = $armPayload | armclient put /subscriptions/$subscriptionId/resourceGroups/$resourceGroup/providers/Microsoft.insights/actionGroups/$actionGroupName/?api-version=2017-04-01

        "Created Action Group with name $actionGroupName" 
        $actionGroupsMap[$actionGroupName] = $actionGroup.actionGroupResourceId.ToLower()
        $index++
    }

    "`nSuccessfully created all action groups!!"
}
catch
{
    $ErrorMessage = $_.Exception.Message

    #Delete all action groups in case of failure
    "`nDeleting newly created action groups if any as some error happened..."
    
    foreach ($actionGroup in $actionGroupsMap.Keys)
    {
        $response = armclient delete /subscriptions/$subscriptionId/resourceGroups/$resourceGroup/providers/Microsoft.insights/actionGroups/$actionGroup/?api-version=2017-04-01      
    }

    "`nError: $ErrorMessage"
    "`nExiting..."
    exit
}

# Update all alerts configuration to the new version
"`nExtending OMS alerts to Azure...`n"

try
{
    $index = 1
    foreach ($alert in $result.migrationSummary.alerts)
    {
        $uri = $alert.alertId + "?api-version=2015-03-20"
        $config = armclient get $uri | ConvertFrom-Json
        $aznsNotification = @{
            "GroupIds" = @($actionGroupsMap[$alert.actionGroupName])
        }
        if ($alert.customWebhookPayload)
        {
            $aznsNotification.Add("CustomWebhookPayload", $alert.customWebhookPayload)
        }
        if ($alert.customEmailSubject)
        {
            $aznsNotification.Add("CustomEmailSubject", $alert.customEmailSubject)
        }      

        # Update alert version
        $config.properties.Version = 2

        $config.properties | Add-Member -MemberType NoteProperty -Name "AzNsNotification" -Value $aznsNotification
        $payload = $config | ConvertTo-Json -Depth 4
        $response = $payload | armclient put $uri
    
        "Extended alert with name $($alert.alertName)"
        $index++
    }
}
catch
{
    $ErrorMessage = $_.Exception.Message   
    if ($index -eq 1)
    {
        "`nDeleting all newly created action groups as no alerts got extended..."
        foreach ($actionGroup in $actionGroupsMap.Keys)
        {
            $response = armclient delete /subscriptions/$subscriptionId/resourceGroups/$resourceGroup/providers/Microsoft.insights/actionGroups/$actionGroup/?api-version=2017-04-01      
        }
        "`nDeleted all action groups."  
    }
    
    "`nError: $ErrorMessage"
    "`nPlease resolve the issue and try extending again!!"
    "`nExiting..."
    exit
}

"`nSuccessfully extended all OMS alerts to Azure!!" 

# Update version of workspace to indicate extension
"`nUpdating alert version information in OMS workspace..." 

$response = armclient post "/subscriptions/$subscriptionId/resourceGroups/$resourceGroup/providers/Microsoft.OperationalInsights/workspaces/$workspaceName/alertsversion?api-version=2017-04-26-preview&iversion=2"

"`nExtension complete!!"
```


### <a name="about-the-custom-powershell-script"></a>Acerca del script de PowerShell personalizado 
La siguiente es información importante sobre el uso del script:
- Un requisito previo es la instalación de [ARMClient](https://github.com/projectkudu/ARMClient), una herramienta de línea de comandos de código abierto que simplifica la invocación de la API de Azure Resource Manager.
- Para ejecutar el script, debes tener un rol de colaborador o propietario en la suscripción a Azure.
- Debe proporcionar los detalles siguientes:
    - $subscriptionId: el identificador de suscripción de Azure asociado con el área de trabajo de Log Analytics de Operations Management Suite.
    - $resourceGroup: el grupo de recursos de Azure para el área de trabajo de Log Analytics de Operations Management Suite.
    - $workspaceName: el nombre del área de trabajo de Log Analytics de Operations Management Suite.

### <a name="output-of-the-custom-powershell-script"></a>Salida del script de PowerShell personalizado
El script es detallado y genera los pasos mientras se ejecuta: 
- Muestra el resumen, que contiene la información acerca de las alertas de Log Analytics de Operations Management Suite existentes en el área de trabajo. El resumen también contiene información acerca de los grupos de acciones de Azure que se crean para las acciones asociadas a ellos. 
- Se le pedirá que siga adelante con la extensión o que salga después de ver el resumen.
- Si continúa con la extensión, se crean nuevos grupos de acciones de Azure, y todas las alertas existentes están asociadas con ellos. 
- El script se cierra al mostrar el mensaje "Extension complete!" (Extensión completa). En el caso de errores intermedios, el script muestra errores posteriores.

## <a name="troubleshooting"></a>solución de problemas 
Durante el proceso de la extensión de las alertas, los problemas pueden impedir que el sistema cree los [grupos de acciones](monitoring-action-groups.md) necesarios. En tales casos, verá un mensaje de error en un banner de la sección **Alerta** del portal de Operations Management Suite, o bien en la llamada GET realizada en la API.

> [!IMPORTANT]
> Si los usuarios de Log Analytics basados en la nube pública de Azure no toman las siguientes medidas de corrección antes del 5 de julio de 2018, las alertas se ejecutarán en Azure pero no activarán ninguna acción ni notificación. Para obtener notificaciones de alertas, debe editar y agregar manualmente [grupos de acciones](monitoring-action-groups.md) o usar el [script de PowerShell personalizado](#option-3---using-custom-powershell-script) anterior.

Aquí se indican los pasos para corregir cada error:
- **Error: Scope Lock is present at subscription/resource group level for write operations** (El bloqueo de ámbito está presente en el nivel de la suscripción o del grupo de recursos para las operaciones de escritura): ![captura de pantalla de la página de configuración de la alerta de Operations Management Suite con el mensaje de error del bloqueo de ámbito resaltado](./media/monitor-alerts-extend/ErrorScopeLock.png)

    Cuando se habilita el bloqueo de ámbito, la característica restringe cualquier cambio nuevo en la suscripción o en el grupo de recursos que contiene el área de trabajo de Log Analytics (Operations Management Suite). El sistema no puede ampliar las alertas en Azure y crear grupos de acciones necesarias.
    
    Para resolverlo, elimine el bloqueo *ReadOnly* en su suscripción o grupo de recursos que contiene el área de trabajo. Para ello, puede usar Azure Portal, PowerShell, CLI de Azure o la API. Para más información, consulte el artículo sobre [uso del bloqueo de recursos](../azure-resource-manager/resource-group-lock-resources.md). 
    
    Cuando resuelva el error mediante los pasos ilustrados en el artículo, Operations Management Suite extiende sus alertas a Azure dentro de la ejecución programada para el día siguiente. No es necesario realizar ninguna acción adicional ni iniciar nada.

- **Error: Policy is present at subscription/resource group level** (Error: Directiva presente en el nivel de grupo de recursos/suscripción): ![captura de pantalla de la página de configuración de alertas del portal de Operations Management Suite con un mensaje de error de directiva resaltado](./media/monitor-alerts-extend/ErrorPolicy.png)

    Cuando se aplica [Azure Policy](../azure-policy/azure-policy-introduction.md), restringe cualquier recurso nuevo en una suscripción o grupo de recursos que contiene el área de trabajo de Log Analytics (Operations Management Suite). El sistema no puede ampliar las alertas en Azure y crear grupos de acciones necesarias.
    
    Para resolver este problema, edite la directiva que provoca el error *[RequestDisallowedByPolicy](../azure-resource-manager/resource-manager-policy-requestdisallowedbypolicy-error.md)*, que impide la creación de nuevos recursos en la suscripción o el grupo de recursos que contiene el área de trabajo. Para ello, puede usar Azure Portal, PowerShell, CLI de Azure o la API. Puede auditar las acciones para encontrar la directiva adecuada que está produciendo el error. Para más información, consulte el artículo sobre la [visualización de registros de actividad para auditar las acciones](../azure-resource-manager/resource-group-audit.md). 
    
    Cuando resuelva el error mediante los pasos ilustrados en el artículo, Operations Management Suite extiende sus alertas a Azure dentro de la ejecución programada para el día siguiente. No es necesario realizar ninguna acción adicional ni iniciar nada.


## <a name="next-steps"></a>Pasos siguientes

* Más información sobre la nueva [experiencia de Alertas de Azure](monitoring-overview-unified-alerts.md).
* Más información sobre las [alertas de registro en Alertas de Azure](monitor-alerts-unified-log.md).
