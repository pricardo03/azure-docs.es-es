---
title: Extensión de alertas de Log Analytics a la nube de Azure Government
description: En este artículo se describen las herramientas y API mediante las cuales puede extender alertas de Log Analytics a Alertas de Azure.
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/04/2018
ms.author: vinagara
ms.subservice: alerts
ms.openlocfilehash: 9d734f74c4e12b369e46c15dcb9d01a8185dddd6
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/28/2019
ms.locfileid: "55103384"
---
# <a name="extend-alerts-from-log-analytics-into-azure-alerts"></a>Extensión de alertas de Log Analytics a Alertas de Azure
La característica de alertas del portal de OMS se sustituye por Alertas de Azure en la nuble de Azure Government. Como parte de esta transición, las alertas que ha configurado originalmente en Log Analytics se extenderán a Azure. Si no quiere esperar a que se trasladen automáticamente a Azure, puede iniciar el proceso:

- Manualmente desde el portal de Operations Management Suite. 
- Mediante programación usando AlertsVersion API.  

> [!NOTE]
> A partir del 1 de mayo de 2019, Microsoft extenderá automáticamente las alertas creadas en las instancias del portal de OMS de Azure Government de Log Analytics a Alertas de Azure de forma sistemática. Si tiene problemas para crear [ grupos de acciones](../../azure-monitor/platform/action-groups.md), siga [estos pasos de corrección](alerts-extend-tool.md#troubleshooting) para crear grupos de acciones automáticamente. Estos pasos se puede usar hasta el 15 de marzo de 2019 en el portal de OMS de Azure Government.

## <a name="option-1-initiate-from-the-operations-management-suite-portal"></a>Opción 1: Inicio desde el portal de Operations Management Suite
En los pasos siguientes se describe cómo ampliar las alertas para el espacio de trabajo desde el portal de Operations Management Suite para la nube de Azure Government.  

1. En Azure Portal, seleccione **Todos los servicios**. En la lista de recursos, escriba **Log Analytics**. Cuando comience a escribir, la lista se filtrará en función de la entrada. Seleccione **Log Analytics**.
2. En el panel de suscripciones de Log Analytics, seleccione un área de trabajo y, después, seleccione el icono de **Portal de OMS**.
![Captura de pantalla del panel de suscripción a Log Analytics, con el icono del portal de OMS resaltado](media/alerts-extend-tool/azure-portal-01.png) 
3. Después de redirigirse al portal de Operations Management Suite, seleccione el icono **Configuración**.
![Captura de pantalla del portal de Operations Management Suite, con el icono Configuración resaltado](media/alerts-extend-tool/oms-portal-settings-option.png) 
4. En la página **Configuración**, seleccione **Alertas**.  
5. Seleccione **Extender a Azure**.
![Captura de pantalla de la página de configuración de alertas del portal de Operations Management Suite, con Extender a Azure resaltado](media/alerts-extend-tool/ExtendInto.png)
6. Aparecerá un asistente de tres pasos en el panel **Alertas**. Lea la información general y seleccione **Siguiente**.
![Captura de pantalla del paso 1 del asistente](media/alerts-extend-tool/ExtendStep1.png)  
7. En el segundo paso, verá un resumen de los cambios propuestos, con los [grupos de acciones](../../azure-monitor/platform/action-groups.md) adecuados enumerados, para las alertas. Si se ven acciones similares en más de una alerta, el asistente propondrá asociar un único grupo de acciones con todas ellas.  Esta es la convención de nomenclatura: *WorkspaceName_AG_#Number*. Para continuar, seleccione **Siguiente**.
![Captura de pantalla del paso 2 del asistente](media/alerts-extend-tool/ExtendStep2.png)  
8. En el último paso del asistente, seleccione **Finalizar** y confirme cuando se le solicite para iniciar el proceso. Opcionalmente, puede proporcionar una dirección de correo electrónico, para que se le notifique cuando el proceso se complete y todas las alertas se hayan trasladado correctamente a Alertas de Azure.
![Captura de pantalla del paso 3 del asistente](media/alerts-extend-tool/ExtendStep3.png)

Cuando finalice el asistente, en la página **Configuración de alertas**, se eliminará la opción de extender las alertas a Azure. En segundo plano, las alertas se trasladan a Azure, y esto puede llevar algún tiempo. Durante la operación, no puede realizar cambios en las alertas del portal de Operations Management Suite. Puede ver el estado actual en el banner en la parte superior del portal. Si ha proporcionado una dirección de correo electrónico anteriormente, recibirá un correo electrónico cuando el proceso haya finalizado correctamente.  


Las alertas siguen apareciendo en el portal de Operations Management Suite, incluso después de haber sido trasladadas correctamente a Azure.
![Captura de pantalla de la página de configuración de alertas del portal Operations Management Suite](media/alerts-extend-tool/PostExtendList.png)


## <a name="option-2-use-the-alertsversion-api"></a>Opción 2: Uso de AlertsVersion API
Puede utilizar AlertsVersion API de Log Analytics para extender las alertas de Log Analytics a Alertas de Azure desde cualquier cliente que pueda llamar a una API REST. Puede acceder a la API desde PowerShell mediante [ARMClient](https://github.com/projectkudu/ARMClient), una herramienta de línea de comandos de código abierto. Puede mostrar los resultados en JSON.  

Para usar la API, primero hay que crear una solicitud GET. Esto evalúa y devuelve un resumen de los cambios propuestos, antes de intentar extenderse a Azure mediante una solicitud POST. Los resultados muestran las alertas y una lista propuesta de [grupos de acciones](../../azure-monitor/platform/action-groups.md), en formato JSON. Si se ven acciones similares en más de una alerta, el servicio propone asociarlas todas a un único grupo de acciones. Esta es la convención de nomenclatura: *WorkspaceName_AG_#Number*.

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
                            "serviceUri": "https://test.com"
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

Esta respuesta indica que las alertas se han extendido correctamente a Alertas de Azure. Esta propiedad de versión es solo para comprobar si las alertas se han extendido a Azure y no tienen relación a [Search API de Log Analytics](../../azure-monitor/platform/api-alerts.md). Cuando las alertas se extienden a Azure correctamente, se envía un informe a cualquier dirección de correo electrónico proporcionada con la solicitud POST. Si todas las alertas en el área de trabajo especificado ya están programadas para su extensión, la respuesta a la solicitud POST es que el intento se ha prohibido (un código de estado 403). Para ver los mensajes de error o para entender si el proceso está bloqueado, puede enviar una solicitud GET. Si hay un mensaje de error, este se devuelve junto con la información de resumen.

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
                            "serviceUri": "https://test.com"
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

## <a name="troubleshooting"></a>solución de problemas 
Durante el proceso de la extensión de las alertas, los problemas pueden impedir que el sistema cree los [grupos de acciones](../../azure-monitor/platform/action-groups.md) necesarios. En tales casos, verá un mensaje de error en un banner de la sección **Alerta** del portal de Operations Management Suite, o bien en la llamada GET realizada en la API.

> [!IMPORTANT]
> Si los usuarios del portal de OMS basados en la nube de Azure Government no toman las siguientes medidas de corrección antes del 15 de marzo de 2019, las alertas se ejecutarán en Azure pero no activarán ninguna acción ni notificación. Para obtener notificaciones de alertas, es preciso editar manualmente sus reglas de alertas en Azure y agregar [grupos de acciones](../../azure-monitor/platform/action-groups.md)

Aquí se indican los pasos para corregir cada error:
- **Error: el bloqueo de ámbito está presente en el nivel de grupo de recursos o suscripción para las operaciones de escritura**:   ![Captura de pantalla de la página de configuración de alertas del portal de Operations Management Suite, con el mensaje de error de bloqueo de ámbito resaltado](media/alerts-extend-tool/ErrorScopeLock.png)

    Cuando se habilita el bloqueo de ámbito, la característica restringe cualquier cambio nuevo en la suscripción o en el grupo de recursos que contiene el área de trabajo de Log Analytics (Operations Management Suite). El sistema no puede ampliar las alertas en Azure y crear grupos de acciones necesarias.
    
    Para resolverlo, elimine el bloqueo *ReadOnly* en su suscripción o grupo de recursos que contiene el área de trabajo. Para ello, puede usar Azure Portal, PowerShell, CLI de Azure o la API. Para más información, consulte el artículo sobre [uso del bloqueo de recursos](../../azure-resource-manager/resource-group-lock-resources.md). 
    
    Cuando resuelva el error mediante los pasos ilustrados en el artículo, Operations Management Suite extiende sus alertas a Azure dentro de la ejecución programada para el día siguiente. No es necesario realizar ninguna acción adicional ni iniciar nada.

- **Error: la directiva está presente en el nivel de grupo de recursos o suscripción**:   ![Captura de pantalla de la página de configuración de alertas del portal de Operations Management Suite, con el mensaje de error de directiva resaltado](media/alerts-extend-tool/ErrorPolicy.png)

    Cuando se aplica [Azure Policy](../../governance/policy/overview.md), restringe cualquier recurso nuevo en una suscripción o grupo de recursos que contiene el área de trabajo de Log Analytics (Operations Management Suite). El sistema no puede ampliar las alertas en Azure y crear grupos de acciones necesarias.
    
    Para resolver este problema, edite la directiva que provoca el error *[RequestDisallowedByPolicy](../../azure-resource-manager/resource-manager-policy-requestdisallowedbypolicy-error.md)*, que impide la creación de nuevos recursos en la suscripción o el grupo de recursos que contiene el área de trabajo. Para ello, puede usar Azure Portal, PowerShell, CLI de Azure o la API. Puede auditar las acciones para encontrar la directiva adecuada que está produciendo el error. Para más información, consulte el artículo sobre la [visualización de registros de actividad para auditar las acciones](../../azure-resource-manager/resource-group-audit.md). 
    
    Cuando resuelva el error mediante los pasos ilustrados en el artículo, Operations Management Suite extiende sus alertas a Azure dentro de la ejecución programada para el día siguiente. No es necesario realizar ninguna acción adicional ni iniciar nada.


## <a name="next-steps"></a>Pasos siguientes

* Más información sobre la nueva [experiencia de Alertas de Azure](../../azure-monitor/platform/alerts-overview.md).
* Más información sobre las [alertas de registro en Alertas de Azure](alerts-unified-log.md).

