---
title: Creación, visualización y administración de alertas de registro mediante Azure Monitor
description: Utilice Azure Monitor para crear, ver y administrar reglas de alertas de registro en Azure.
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/15/2018
ms.author: vinagara
ms.subservice: alerts
ms.openlocfilehash: 64fb629e29de9771ca5f76d1c454ec5d14337a57
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/28/2019
ms.locfileid: "55104522"
---
# <a name="create-view-and-manage-log-alerts-using-azure-monitor"></a>Creación, visualización y administración de alertas de registro mediante Azure Monitor  

## <a name="overview"></a>Información general
En este artículo se muestra cómo configurar las alertas de registro con la interfaz de alertas en Azure Portal. La definición de una regla de alertas consta de tres partes:
- Destino: recurso de Azure específico que se va a supervisar.
- Criterios: condición o lógica específicas que, cuando se señaliza la alerta, deben desencadenar una acción.
- Acción: llamada específica enviada a un receptor de una notificación (correo electrónico, SMS, webhook, etc.).

El término **Alertas de registro** se usa para describir las alertas cuya señal es una consulta personalizada basada en [Log Analytics](../../azure-monitor/learn/tutorial-viewdata.md) o [Application Insights](../../azure-monitor/app/analytics.md). Obtenga más información acerca de la funcionalidad, la terminología y los tipos de [Alertas de registro: información general](../../azure-monitor/platform/alerts-unified-log.md).

> [!NOTE]
> Ahora los datos de registro populares de [Azure Log Analytics](../../azure-monitor/learn/tutorial-viewdata.md) también están disponibles en la plataforma de métricas de Azure Monitor. Para obtener más detalles, consulte [Metric Alert for Logs](../../azure-monitor/platform/alerts-metric-logs.md) (Alerta de métrica de los registros)

## <a name="managing-log-alerts-from-the-azure-portal"></a>Administración de las alertas de registro desde Azure Portal

A continuación, se muestra una guía paso a paso acerca de cómo usar las alertas de registro mediante la interfaz de Azure Portal.

### <a name="create-a-log-alert-rule-with-the-azure-portal"></a>Creación de una regla de alertas de registro en Azure Portal
1. En [Azure Portal](https://portal.azure.com/), seleccione **Supervisar** y, en la sección SUPERVISAR, elija **Alertas**.  
    ![Supervisión](media/alerts-log/AlertsPreviewMenu.png)

1. Seleccione el botón **Nueva regla de alertas** para crear una alerta en Azure.
    ![Adición de una alerta](media/alerts-log/AlertsPreviewOption.png)

1. La sección Crear alerta se muestra con tres partes, que constan de: *definición de la condición de alerta*, *definición de los detalles de la alerta* y *definición del grupo de acciones*.

    ![Creación de una regla](media/alerts-log/AlertsPreviewAdd.png)

1.  Para definir la condición de la alerta, use primero el vínculo **Seleccionar recurso** y especifique el destino mediante la selección de un recurso. Para filtrar, elija la _Suscripción_, el _Tipo de recurso_ y el _Recurso_ necesario. 

    >[!NOTE]

    > Para crear una alerta de registro, compruebe la señal de **registro** disponible para el recurso seleccionado antes de continuar.
    ![Seleccionar recurso](media/alerts-log/Alert-SelectResourceLog.png)

 
1. *Alertas de registro*: asegúrese de que la opción **Tipo de recurso** sea un origen de análisis como *Log Analytics* o *Application Insights* y el tipo de señal sea **Registro**; después, una vez elegido el **recurso** apropiado, haga clic en *Listo*. A continuación, use el botón **Agregar criterios** para ver la lista de opciones de señal disponibles para el recurso y, de la lista de señales, la opción **Búsqueda de registros personalizada** para el servicio de supervisión del registro, como *Log Analytics* o *Application Insights*.

   ![Selección de un recurso: búsqueda de recursos personalizada](media/alerts-log/AlertsPreviewResourceSelectionLog.png)

   > [!NOTE]

   > Las listas de las alertas pueden importar una consulta de análisis como tipo de señal (**Log (Saved Query)** (Registro [consulta guardada])), tal como se muestra en la ilustración anterior. Por tanto, los usuarios pueden perfeccionar la consulta en Analytics y luego guardarla para usarla en alertas en otro momento. Puede encontrar más detalles sobre el uso de consultas guardadas en [Descripción de las búsquedas de registros en Log Analytics](../../azure-monitor/log-query/log-query-overview.md) o [¿Qué es Log Analytics?](../../azure-monitor/log-query/log-query-overview.md). 

1.  *Alertas de registro*: una vez seleccionada esta opción, la consulta de alertas se puede indicar en el campo **Consulta de búsqueda**; si la sintaxis de la consulta es incorrecta, en el campo aparece el error en ROJO. Si la sintaxis de consulta es correcta, como referencia, se muestran los datos históricos de la consulta indicada en formato de gráfico con la opción de retocar la ventana de tiempo desde las últimas seis horas hasta la última semana.

 ![Configuración de la regla de alertas](media/alerts-log/AlertsPreviewAlertLog.png)

 > [!NOTE]

    > La visualización de los datos históricos solo se muestra si los resultados de la consulta tienen detalles temporales. Si la consulta genera datos resumidos o valores de columna específicos, estos se representan en un gráfico singular.

    >  Para el tipo Unidades métricas de Alertas de registro con Application Insights, puede especificar por cuál variable concreta desea agrupar los datos mediante la opción **Agregado en**, tal y como se ilustra a continuación:

    ![agregar una opción](media/alerts-log/aggregate-on.png)

1.  *Alertas de registro*: Con la visualización activada, se puede seleccionar la **Lógica de alerta** de entre las opciones mostradas de Condición, Agregación y, por último, Umbral. Por último, especifique en la lógica el tiempo para evaluar la condición especificada mediante la opción **Periodo**. Además, especifique la frecuencia con que debe ejecutarse la alerta seleccionando la opción adecuada en el campo **Frecuencia**.

Para las **Alertas de registro**, las alertas pueden basarse en lo siguiente:
   - *Número de registros*: se crea una alerta si el número de registros devueltos por la consulta es mayor o menor que el valor que proporcione.
   - *Unidades métricas*: se crea una alerta si cada *valor agregado* en los resultados excede el valor de umbral proporcionado y se *agrupa por* el valor elegido. El número de infracciones de una alerta es el número de veces que se supera el umbral en el período de tiempo seleccionado. Puede especificar Infracciones totales para cualquier combinación de infracciones en el conjunto de resultados o Infracciones consecutivas para que las infracciones deban tener lugar en muestras consecutivas. Obtenga más información sobre las [alertas de registro y sus tipos](../../azure-monitor/platform/alerts-unified-log.md).


1. En segundo lugar, asigne un nombre a la alerta en el campo **Nombre de la regla de alertas** junto con una **Descripción**, en la que debe proporcionar información específica sobre la alerta, y debe indicar también un valor de **Gravedad** entre las opciones proporcionadas. Estos detalles se reutilizan en todos los correos electrónicos, las notificaciones o las notificaciones push de alerta enviados por Azure Monitor. Además, el usuario puede elegir activar inmediatamente la regla de alertas al crearla cambiando la opción **Habilitar regla tras la creación** según corresponda.

    Solo para las **alertas de registro**, se encuentra disponible alguna funcionalidad adicional en los detalles de la alerta:

    - **Suprimir alertas**: Al activar la supresión de la regla de alerta, las acciones de la regla se deshabilitan durante un período de tiempo definido después de crear una nueva alerta. La regla se sigue ejecutando y crea registros de alerta si se cumplen los criterios. De esta forma, dispone de tiempo para corregir el problema sin ejecutar acciones duplicadas.

        ![Desactivar las alertas de registro](media/alerts-log/AlertsPreviewSuppress.png)

        > [!TIP]
        > Especifique un valor de desactivar las alertas mayor que la frecuencia de alertas para garantizar que las notificaciones se detengan sin superposición

1. Como tercer y último paso, especifique si es necesario desencadenar algún **grupo de acciones** para la regla de alertas si se cumple la condición de alerta. Puede elegir cualquier grupo de acciones existente con alerta o crear uno. Según el grupo de acciones seleccionado, cuando se desencadena la alerta, Azure: envía correos electrónicos, envía SMS, llama a webhooks, la soluciona con runbooks de Azure, envía notificaciones push a la herramienta de ITSM, etc. Obtenga más información sobre los [grupos de acciones](action-groups.md).

    > [!NOTE]
    > Consulte los [límites de servicio de suscripción de Azure](../../azure-subscription-service-limits.md) para conocer los límites relacionados con las cargas de runbook desencadenadas para las alertas de registro a través de grupos de acciones de Azure.

    Para las **alertas de registro**, se encuentra disponible alguna funcionalidad adicional para reemplazar las acciones predeterminadas:

    - **Notificación por correo electrónico**: invalida el *asunto del correo electrónico* en el correo electrónico, enviado a través del grupo de acciones, si existen una o más acciones de correo electrónico en dicho grupo de acciones. No se puede modificar el cuerpo del mensaje de correo y este campo **no** es para la dirección de correo electrónico.
    - **Incluir carga de JSON personalizada**: invalida el webhook JSON usado por los grupos de acciones si una o varias acciones de webhook existen en el grupo de acciones mencionado. El usuario puede especificar el formato JSON que se usará para todos los webhooks configurados en el grupo de acciones asociado; para más información sobre los formatos de webhook, consulte [Acciones de webhook para alertas de registro](../../azure-monitor/platform/alerts-log-webhook.md). La opción de vista de webhook se proporciona para comprobar el formato con datos JSON de ejemplo.

        ![Invalidaciones de acciones para alertas de registro](media/alerts-log/AlertsPreviewOverrideLog.png)


1. Si todos los campos son válidos y tienen una marca verde, se puede hacer clic en el botón **Crear regla de alertas** y se crea la alerta en Azure Monitor: Alertas. Todas las alertas pueden verse en el panel de las alertas.

    ![Creación de reglas](media/alerts-log/AlertsPreviewCreate.png)

    En cuestión de minutos, se activa la alerta y se desencadena tal como se describió anteriormente.

Los usuarios también pueden finalizar la consulta de análisis en la [página Log Analytics de Azure Portal](../../azure-monitor/log-query/portals.md#log-analytics-page
) e insertarla para crear una alerta a través del botón "+Nueva regla de alertas " y, después, seguir las instrucciones del paso 6 en adelante del tutorial anterior.

 ![Log Analytics: Establecer alerta](media/alerts-log/AlertsAnalyticsCreate.png)

### <a name="view--manage-log-alerts-in-azure-portal"></a>Visualización y administración de las alertas de registro en Azure Portal

1. En [Azure Portal](https://portal.azure.com/), seleccione **Supervisar** y, en la sección SUPERVISAR, elija **Alertas**.  

1. Se muestra el **panel de alertas**, de forma que aparecen todas las alertas de Azure (incluidas las alertas de registro) en un único panel, incluidas todas las instancias de cuando se ha activado la regla de la alerta de registro. Para obtener más información, consulte [Alert Management](https://aka.ms/managealertinstances).
    > [!NOTE]
    > Las reglas de la alerta de registro se componen de una lógica personalizada basada en la consulta proporcionada por los usuarios y, por lo tanto, sin un estado resuelto. Debido a esto, cada vez que se cumplen las condiciones especificadas en la regla de la alerta de registro, se activa. 


1. Seleccione el botón **Administrar reglas** situado en la barra superior para navegar hasta la sección de administración de reglas, donde se enumeran todas las reglas de alerta creadas, incluidas las alertas que se han deshabilitado.
    ![Administrar reglas de alertas](media/alerts-log/manage-alert-rules.png)

## <a name="managing-log-alerts-using-azure-resource-template"></a>Administración de alertas de registro mediante la plantilla de recursos de Azure

Las alertas del registro de Azure Monitor están asociadas con el tipo de recurso `Microsoft.Insights/scheduledQueryRules/`. Para más información sobre este tipo de recurso, vea [Azure Monitor: referencia de la API de reglas de consulta programada](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/). Las alertas de registro de Application Insights o Log Analytics se pueden crear mediante la [API de Reglas de consulta programadas](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/).

> [!NOTE]
> Las alertas de registro de Log Analytics también se puede administrar mediante [Alert API de Log Analytics](../../azure-monitor/platform/api-alerts.md) y las plantillas heredadas de [búsquedas y alertas guardadas de Log Analytics](../../azure-monitor/insights/solutions-resources-searches-alerts.md) también. Para más información acerca del uso de la nueva API de Reglas de consulta programadas detallada aquí de forma predeterminada, consulte [Switch to new API for Log Analytics Alerts](alerts-log-api-switch.md) (Cambio a una API nueva de alertas de Log Analytics).

Esta es la estructura de la plantilla de recursos basada en la [creación de reglas de consulta programada](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/createorupdate), con datos de ejemplo establecidos como variables.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0", 
    "parameters": {      
    },   
    "variables": {
    "alertLocation": "Region Name for your Application Insights App or Log Analytics Workspace",
    "alertName": "sample log alert",
    "alertDescr": "Sample log search alert",
    "alertStatus": "true",
    "alertTag": "hidden-link:/subscriptions/a123d7efg-123c-1234-5678-a12bc3defgh4/resourceGroups/contosoRG/providers/microsoft.OperationalInsights/workspaces/servicews",
    "alertSource":{
        "Query":"union workspace("servicews").Update, app('serviceapp').requests | summarize AggregatedValue = count() by bin(TimeGenerated,1h), Classification",
        "Resource1": "/subscriptions/a123d7efg-123c-1234-5678-a12bc3defgh4/resourceGroups/contosoRG/providers/microsoft.OperationalInsights/workspaces/servicews", 
        "Resource2": "/subscriptions/a123d7efg-123c-1234-5678-a12bc3defgh4/resourceGroups/contosoRG/providers/microsoft.insights/components/serviceapp",
        "SourceId": "/subscriptions/a123d7efg-123c-1234-5678-a12bc3defgh4/resourceGroups/contosoRG/providers/microsoft.OperationalInsights/workspaces/servicews",
        "Type":"ResultCount"
         },
     "alertSchedule":{
         "Frequency": 15,
         "Time": 60
         },
     "alertActions":{
         "SeverityLevel": "4",
         "SuppressTimeinMin": 20
         },
      "alertTrigger":{
        "Operator":"GreaterThan",
        "Threshold":"1"
         },
      "metricMeasurement": {
          "thresholdOperator": "Equal",
          "threshold": "1",
          "metricTriggerType": "Consecutive",
          "metricColumn": "Classification"
      },
       "actionGrp":{
        "ActionGroup": "/subscriptions/a123d7efg-123c-1234-5678-a12bc3defgh4/resourceGroups/contosoRG/providers/microsoft.insights/actiongroups/sampleAG",
        "Subject": "Customized Email Header",
        "Webhook": "{ \"alertname\":\"#alertrulename\", \"IncludeSearchResults\":true }"
        }
  },
  "resources":[ {
    "name":"[variables('alertName')]",
    "type":"Microsoft.Insights/scheduledQueryRules",
    "apiVersion": "2018-04-16",
    "location": "[variables('alertLocation')]",
    "tags":{"[variables('alertTag')]": "Resource"},
    "properties":{
       "description": "[variables('alertDescr')]",
       "enabled": "[variables('alertStatus')]",
       "source": {
           "query": "[variables('alertSource').Query]",
           "authorizedResources": "[concat(array(variables('alertSource').Resource1), array(variables('alertSource').Resource2))]",
           "dataSourceId": "[variables('alertSource').SourceId]",
           "queryType":"[variables('alertSource').Type]"
       },
      "schedule":{
           "frequencyInMinutes": "[variables('alertSchedule').Frequency]",
           "timeWindowInMinutes": "[variables('alertSchedule').Time]"
       },
      "action":{
           "odata.type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.Microsoft.AppInsights.Nexus.DataContracts.Resources.ScheduledQueryRules.AlertingAction",
           "severity":"[variables('alertActions').SeverityLevel]",
           "throttlingInMin": "[variables('alertActions').SuppressTimeinMin]",
           "aznsAction":{
               "actionGroup": "[array(variables('actionGrp').ActionGroup)]",
               "emailSubject":"[variables('actionGrp').Subject]",
               "customWebhookPayload":"[variables('actionGrp').Webhook]"
           },
       "trigger":{
               "thresholdOperator":"[variables('alertTrigger').Operator]",
               "threshold":"[variables('alertTrigger').Threshold]",
               "metricTrigger":{
                   "thresholdOperator": "[variables('metricMeasurement').thresholdOperator]",
                   "threshold": "[variables('metricMeasurement').threshold]",
                   "metricColumn": "[variables('metricMeasurement').metricColumn]",
                   "metricTriggerType": "[variables('metricMeasurement').metricTriggerType]"
               }
           }
       }
     }
   }
 ]
}
```
> [!IMPORTANT]
> El campo de etiqueta con vínculo oculto al recurso de destino es obligatorio con el uso de la plantilla de recursos o la llamada API de [reglas de consulta programada ](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/). 

El JSON del ejemplo anterior puede guardarse como (digamos) sampleScheduledQueryRule.json a efectos de este tutorial y puede implementarse mediante [Azure Resource Manager en Azure Portal](../../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template).

## <a name="managing-log-alerts-using-powershell-cli-or-api"></a>Administración de alertas de registro con PowerShell, la CLI o la API

Azure Monitor: API de Reglas de consulta programadas](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/) es una API REST totalmente compatible con Azure Resource Manager REST API. Por lo tanto, se puede utilizar a través de Powershell con el cmdlet de Resource Manager y con la CLI de Azure.

> [!NOTE]
> Las alertas de registro de Log Analytics también se puede administrar mediante [Alert API de Log Analytics](../../azure-monitor/platform/api-alerts.md) y las plantillas heredadas de [búsquedas y alertas guardadas de Log Analytics](../../azure-monitor/insights/solutions-resources-searches-alerts.md) también. Para más información acerca del uso de la nueva API de Reglas de consulta programadas detallada aquí de forma predeterminada, consulte [Switch to new API for Log Analytics Alerts](alerts-log-api-switch.md) (Cambio a una API nueva de alertas de Log Analytics).


Actualmente, las alertas de registro no tienen comandos de PowerShell o la CLI dedicados; pero como se muestra a continuación se pueden usar mediante el cmdlet de PowerShell de Azure Resource Manager para la plantilla de recursos de ejemplo que se mostró anteriormente (sampleScheduledQueryRule.json) en la [sección Plantilla de recursos](#azure-resource-template-for-application-insights):
```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName "contosoRG" -TemplateFile "D:\Azure\Templates\sampleScheduledQueryRule.json"
```

Seguidamente se muestra cómo se usa la plantilla de recursos de ejemplo que se mostró anteriormente (sampleScheduledQueryRule.json), mediante el comando de Azure Resource Manager de la CLI de Azure en la [sección Plantilla de recursos](#azure-resource-template-for-application-insights):

```azurecli
az group deployment create --resource-group contosoRG --template-file sampleScheduledQueryRule.json
```

Si la operación se realiza correctamente, se devolverá 201 para indicar que se ha creado la regla de alertas o 200 si se ha modificado una regla de alerta existente.
  
## <a name="next-steps"></a>Pasos siguientes

* Más información sobre las [alertas de registro en las alertas de Azure](../../azure-monitor/platform/alerts-unified-log.md).
* Conocer las [acciones de webhook para alertas de registro](../../azure-monitor/platform/alerts-log-webhook.md)
* Más información sobre [Application Insights](../../azure-monitor/app/analytics.md)
* Más información sobre [Log Analytics](../../azure-monitor/log-query/log-query-overview.md). 


