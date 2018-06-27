---
title: Uso de PowerShell para crear alertas clásicas de los servicios de Azure | Microsoft Docs
description: Desencadene correos electrónicos o notificaciones o llamadas a direcciones URL de sitios web (webhooks) o a la automatización cuando se cumplen las condiciones especificadas.
author: rboucher
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 03/28/2018
ms.author: robb
ms.component: alerts
ms.openlocfilehash: b08a8f66add45d64085ac05605fe3c7d7f91b705
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36286206"
---
# <a name="use-powershell-to-create-alerts-for-azure-services"></a>Uso de PowerShell para crear alertas de los servicios de Azure
> [!div class="op_single_selector"]
> * [Portal](insights-alerts-portal.md)
> * [PowerShell](insights-alerts-powershell.md)
> * [CLI](insights-alerts-command-line-interface.md)
>
>

> [!NOTE]
> En este artículo se describe cómo crear alertas de métrica clásicas más antiguas. Azure Monitor admite ahora [nuevas y mejores alertas de métricas](monitoring-near-real-time-metric-alerts.md). Estas alertas pueden supervisar varias métricas y permiten alertas relacionadas con métricas de dimensiones. Próximamente estará disponible la compatibilidad de PowerShell con las alertas de métricas más recientes.
>
>

En este artículo se muestra cómo configurar alertas de métricas clásicas de Azure con PowerShell.  

Puede recibir alertas basadas en métricas de los servicios de Azure, o puede recibir alertas para eventos que se producen en Azure.

* **Valores de métrica**: la alerta se desencadena cuando el valor de una métrica específica cruza un umbral asignado en cualquier dirección. Es decir, se desencadena tanto la primera vez que se cumple la condición como después, cuando esa condición ya deja de cumplirse.    
* **Eventos de registro de actividades**: una alerta puede desencadenarse con *cada* evento o cuando se producen ciertos eventos concretos. Para más información acerca de las alertas del registro de actividad, consulte [Creación de alertas del registro de actividad (clásica)](monitoring-activity-log-alerts.md).

Puede configurar una alerta de métrica clásica para realizar las siguientes tareas cuando se desencadena:

* Enviar notificaciones de correo electrónico al administrador y los coadministradores del servicio.
* Enviar un correo electrónico a direcciones de correo electrónico adicionales que especifique.
* Llamar a un webhook.
* Iniciar la ejecución de un runbook de Azure (solo desde Azure Portal).

Puede obtener información sobre las reglas de alerta y configurarlas en las siguientes ubicaciones: 

* [Azure Portal](insights-alerts-portal.md)
* [PowerShell](insights-alerts-powershell.md)
* [Interfaz de la línea de comandos (CLI) de Azure](insights-alerts-command-line-interface.md)
* [API de REST de Azure Monitor](https://msdn.microsoft.com/library/azure/dn931945.aspx)

Para información adicional, siempre puede escribir ```Get-Help``` y, luego, el comando de PowerShell sobre el que desea obtener ayuda.

## <a name="create-alert-rules-in-powershell"></a>Creación de reglas de alerta en PowerShell
1. Inicie sesión en Azure.   

    ```PowerShell
    Connect-AzureRmAccount

    ```
2. Obtenga una lista de las suscripciones disponibles. Compruebe que trabaja con la suscripción adecuada. Si no es así, obtenga la suscripción adecuada usando la salida de `Get-AzureRmSubscription`.

    ```PowerShell
    Get-AzureRmSubscription
    Get-AzureRmContext
    Set-AzureRmContext -SubscriptionId <subscriptionid>
    ```
3. Para ver las reglas existentes en un grupo de recursos, use el comando siguiente:

   ```PowerShell
   Get-AzureRmAlertRule -ResourceGroup <myresourcegroup> -DetailedOutput
   ```
4. Para crear una regla, primero debe contar con varios datos importantes.

    - El **identificador del recurso** para el que quiere establecer una alerta.
    - Las **definiciones de métricas** disponibles para ese recurso.

     Una forma de obtener el identificador de recurso es usar Azure Portal. Seleccione el recurso en el portal, suponiendo que ya se ha creado. En la sección **Configuración** de la hoja siguiente, seleccione **Propiedades**. El campo **RESOURCE ID** está en la hoja siguiente. 
     
     También puede obtener el identificador de recurso mediante [Azure Resource Explorer](https://resources.azure.com/).

     A continuación se muestra un ejemplo de identificador de recurso de una aplicación web:

     ```
     /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename
     ```

     Puede usar `Get-AzureRmMetricDefinition` para ver la lista de todas las definiciones de métricas de un recurso específico:

     ```PowerShell
     Get-AzureRmMetricDefinition -ResourceId <resource_id>
     ```

     En el ejemplo siguiente se genera una tabla con el nombre y la unidad de esa métrica:

     ```PowerShell
     Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit

     ```
     Ejecute `Get-Help Get-AzureRmMetricDefinition -Detailed` para obtener una lista completa de todas las opciones disponibles para Get-AzureRmMetricDefinition.
5. En el ejemplo siguiente se configura una alerta en un recurso de sitio web. La alerta se desencadena cada vez que se recibe constantemente cualquier tráfico durante 5 minutos y nuevamente cuando no se recibe tráfico durante 5 minutos.

    ```PowerShell
    Add-AzureRmMetricAlertRule -Name myMetricRuleWithWebhookAndEmail -Location "East US" -ResourceGroup myresourcegroup -TargetResourceId /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename -MetricName "BytesReceived" -Operator GreaterThan -Threshold 2 -WindowSize 00:05:00 -TimeAggregationOperator Total -Description "alert on any website activity"

    ```
6. Para crear un webhook o enviar un correo electrónico cuando se desencadene una alerta, debe crear antes el correo electrónico o el webhook. Cree la regla inmediatamente después con la etiqueta -Actions, tal como se muestra en el ejemplo siguiente. No se pueden asociar webhooks o mensajes de correo electrónico con reglas que ya haya creado.

    ```PowerShell
    $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
    $actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://www.contoso.com?token=mytoken

    Add-AzureRmMetricAlertRule -Name myMetricRuleWithWebhookAndEmail -Location "East US" -ResourceGroup myresourcegroup -TargetResourceId /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename -MetricName "BytesReceived" -Operator GreaterThan -Threshold 2 -WindowSize 00:05:00 -TimeAggregationOperator Total -Actions $actionEmail, $actionWebhook -Description "alert on any website activity"
    ```

7. Observe las reglas individuales para comprobar que las alertas se crearon correctamente.

    ```PowerShell
    Get-AzureRmAlertRule -Name myMetricRuleWithWebhookAndEmail -ResourceGroup myresourcegroup -DetailedOutput

    Get-AzureRmAlertRule -Name myLogAlertRule -ResourceGroup myresourcegroup -DetailedOutput
    ```
8. Elimine las alertas. Estos comandos eliminan las reglas que se crearon anteriormente en este artículo.

    ```PowerShell
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myrule
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myMetricRuleWithWebhookAndEmail
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myLogAlertRule
    ```

## <a name="next-steps"></a>Pasos siguientes
* [Obtenga información general sobre la supervisión de Azure](monitoring-overview.md), incluidos los tipos de información que puede recopilar y supervisar.
* Aprenda a [configurar webhooks en las alertas](insights-webhooks-alerts.md).
* Aprenda a [configurar alertas para un evento del registro de actividad](monitoring-activity-log-alerts.md).
* Obtenga más información sobre los [runbooks de Azure Automation](../automation/automation-starting-a-runbook.md).
* Obtenga [información general sobre la colección de registros de diagnóstico](monitoring-overview-of-diagnostic-logs.md) para recopilar métricas detalladas de alta frecuencia sobre el servicio.
* Obtenga [información general sobre la colección de métricas](insights-how-to-customize-monitoring.md) para garantizar que el servicio está disponible y que responder adecuadamente.
