---
title: 'Tutorial: Supervisión de los registros de Azure Firewall'
description: En este tutorial, aprenderá a habilitar y administrar los registros de Azure Firewall.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: tutorial
ms.workload: infrastructure-services
ms.date: 7/11/2018
ms.author: victorh
ms.openlocfilehash: a4922fda80b957138a9929090f9d3c349348185d
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/12/2018
ms.locfileid: "38991964"
---
# <a name="tutorial-monitor-azure-firewall-logs"></a>Tutorial: Supervisión de los registros de Azure Firewall

[!INCLUDE [firewall-preview-notice](../../includes/firewall-preview-notice.md)]

En los ejemplos de los artículos de Azure Firewall se supone que ya tiene habilitada la versión preliminar pública de Azure Firewall. Para más información, consulte [Enable the Azure Firewall public preview](public-preview.md) (Habilitar la versión preliminar pública de Azure Firewall).

Puede supervisar Azure Firewall mediante los registros del firewall. También puede usar los registros de actividad para auditar las operaciones de los recursos de Azure Firewall.

Se puede acceder a algunos de estos registros mediante el portal. Los registros se pueden enviar a [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md), Storage y Event Hubs, y se pueden analizar en Log Analytics o mediante distintas herramientas como Excel y Power BI.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Habilitación del registro mediante Azure Portal
> * Habilitación del registro con PowerShell
> * Visualización y análisis del registro de actividades
> * Ver y analizar los registros de la regla de red y de aplicación

## <a name="diagnostic-logs"></a>Registros de diagnóstico

 Los siguientes registros de diagnóstico están disponibles para Azure Firewall:

* **Registro de regla de aplicación**

   El registro de la regla de aplicación se guarda en una cuenta de almacenamiento, se transmite a Event Hubs o se envía a Log Analytics solo si lo habilitó para cada instancia de Azure Firewall. Cada nueva conexión que coincida con una de las reglas de la aplicación configuradas dará como resultado un registro de la conexión aceptada o rechazada. Los datos se registran en formato JSON, tal y como se muestra en el ejemplo siguiente:

   ```
   Category: access logs are either application or network rule logs.
   Time: log timestamp.
   Properties: currently contains the full message. 
   note: this field will be parsed to specific fields in the future, while maintaining backward compatibility with the existing properties field.
   ```

   ```json
   {
    "category": "AzureFirewallApplicationRule",
    "time": "2018-04-16T23:45:04.8295030Z",
    "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/AZUREFIREWALLS/{resourceName}",
    "operationName": "AzureFirewallApplicationRuleLog",
    "properties": {
        "msg": "HTTPS request from 10.1.0.5:55640 to mydestination.com:443. Action: Allow. Rule Collection: collection1000. Rule: rule1002"
    }
   }
   ```

* **Registro de regla de red**

   El registro de la regla de red se guarda en una cuenta de almacenamiento, se transmite a Event Hubs o se envía a Log Analytics solo si lo habilitó para cada instancia de Azure Firewall. Cada nueva conexión que coincida con una de las reglas de red configuradas dará como resultado un registro de la conexión aceptada o rechazada. Los datos se registran en formato JSON, tal y como se muestra en el ejemplo siguiente:

   ```
   Category: access logs are either application or network rule logs.
   Time: log timestamp.
   Properties: currently contains the full message. 
   note: this field will be parsed to specific fields in the future, while maintaining backward compatibility with the existing properties field.
   ```

   ```json
  {
    "category": "AzureFirewallNetworkRule",
    "time": "2018-06-14T23:44:11.0590400Z",
    "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/AZUREFIREWALLS/{resourceName}",
    "operationName": "AzureFirewallNetworkRuleLog",
    "properties": {
        "msg": "TCP request from 111.35.136.173:12518 to 13.78.143.217:2323. Action: Deny"
    }
   }

   ```

Tiene tres opciones para almacenar los archivos de registro:

* **Cuenta de almacenamiento**: cuentas que resultan especialmente útiles para registros cuando estos se almacenan durante mucho tiempo y se revisan cuando es necesario.
* **Event Hubs**: es una buena opción para la integración con otras herramientas de información de seguridad y administración de eventos (SEIM) para obtener alertas acerca de los recursos.
* **Log Analytics**: se usa para la supervisión general en tiempo real de la aplicación o para examinar las tendencias.

## <a name="activity-logs"></a>Registros de actividad

   Las entradas del registro de actividades se recopilan de forma predeterminada y se pueden ver en Azure Portal.

   Puede usar el [registro de actividades de Azure](../azure-resource-manager/resource-group-audit.md) (anteriormente conocido como registros operativos y registros de auditoría) para ver todas las operaciones enviadas a sus suscripciones de Azure.

## <a name="enable-diagnostic-logging-through-the-azure-portal"></a>Habilitación del registro de diagnóstico mediante Azure Portal

Puede tardar algunos minutos en que los datos aparezcan en los registros después de completar este procedimiento para activar el registro de diagnóstico. Si no ve nada a primera vista, vuelva a comprobarlo en unos minutos.

1. En Azure Portal, abra el grupo de recursos de firewall y haga clic en el firewall.
2. En **Supervisión**, haga clic en **Registros de diagnóstico**.

   En Azure Firewall hay dos registros específicos del servicio:

   * Registro de regla de aplicación
   * Registro de regla de red

3. Para empezar a recopilar los datos, clic en **Activar diagnósticos**.
4. En la página **Configuración de diagnóstico**, se encuentran las opciones de configuración de los registros de diagnóstico. 
5. En este ejemplo, Log Analytics almacena los registros, así que escriba **Análisis de registro de firewall** como nombre.
6. Haga clic en **Enviar a Log Analytics** para configurar el área de trabajo. Se pueden utilizar también Events Hubs y la cuenta de almacenamiento para guardar los registros de diagnóstico.
7. En **Log Analytics**, haga clic en **Configurar**.
8. En la página de áreas de trabajo de OMS, haga clic en **Crear área de trabajo nueva**.
9. En la página **Área de trabajo de Log Analytics**, escriba **firewall-oms** como nombre de la nueva **Área de trabajo de OMS**.
10. Seleccione su suscripción, use el grupo de recursos de firewall existente (**Test-FW-RG**), seleccione **Este de EE. UU.** como ubicación y seleccione el plan de tarifa **gratis**.
11. Haga clic en **OK**.
   ![Inicio del proceso de configuración][1]
12. En **Registro**, haga clic en **AzureFirewallApplicationRule** y **AzureFirewallNetworkRule** para recopilar los registros de las reglas de aplicación y de red.
   ![Guardar la configuración de diagnóstico][2]
13. Haga clic en **Save**(Guardar).

## <a name="enable-logging-with-powershell"></a>Habilitación del registro con PowerShell

El registro de actividades se habilita automáticamente para todos los recursos de Resource Manager. Se debe habilitar el registro de diagnóstico para empezar a recopilar los datos disponibles mediante esos registros.

Para habilitarlo, siga estos pasos:

1. Anote el identificador de recurso de la cuenta de almacenamiento donde se almacenan los datos de registro. Este valor tiene este formato: */subscriptions/\<idSuscripción\>/resourceGroups/\<nombreDeGrupoDeRecursos\>/providers/Microsoft.Storage/storageAccounts/\<nombreDeCuentaDeAlmacenamiento\>*.

   Puede usar cualquier cuenta de almacenamiento de la suscripción. Para buscar esta información, se puede usar Azure Portal. La información se encuentra en la página de **propiedades** del recurso.

2. Observe el identificador de recurso del firewall para el que se ha habilitado el registro. Este valor tiene el siguiente formato: */subscriptions/\<idSuscripción\>/resourceGroups/\<nombreDeGrupoDeRecursos\>/providers/Microsoft.Network/azureFirewalls/\<nombreDeFirewall\>*.

   Para buscar esta información, use Azure Portal.

3. Habilite el registro de diagnósticos mediante el siguiente cmdlet de PowerShell:

    ```powershell
    Set-AzureRmDiagnosticSetting  -ResourceId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Network/azureFirewalls/<Firewall name> `
   -StorageAccountId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Storage/storageAccounts/<storage account name> `
   -Enabled $true     
    ```
    
> [!TIP] 
>Los registros de diagnóstico no requieren una cuenta de almacenamiento independiente. El uso del almacenamiento para el registro de acceso y rendimiento supondrá un costo adicional de servicio.

## <a name="view-and-analyze-the-activity-log"></a>Visualización y análisis del registro de actividades

Puede ver y analizar los datos del registro de actividades con cualquiera de los métodos siguientes:

* **Herramientas de Azure:** puede recuperar información de los registros de actividades mediante Azure PowerShell, CLI de Azure, la API de REST de Azure o Azure Portal. En el artículo [Operaciones de actividades con Resource Manager](../azure-resource-manager/resource-group-audit.md) se detallan instrucciones paso a paso de cada método.
* **Power BI:** si todavía no tiene una cuenta de [Power BI](https://powerbi.microsoft.com/pricing) , puede probarlo gratis. Con el [paquete de contenido de los registros de actividades de Azure para Power BI](https://powerbi.microsoft.com/en-us/documentation/powerbi-content-pack-azure-audit-logs/), puede analizar sus datos con los paneles preconfigurados que puede personalizar o usar tal cual.

## <a name="view-and-analyze-the-network-and-application-rule-logs"></a>Ver y analizar los registros de la regla de red y de aplicación

Azure [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md) recopila los archivos de registro de contadores y eventos. Incluye visualizaciones y eficaces funciones de búsqueda para analizar los registros.

También puede conectarse a la cuenta de almacenamiento y recuperar las entradas del registro de JSON de los registros de acceso y rendimiento. Después de descargar los archivos JSON, se pueden convertir a CSV y consultarlos en Excel, PowerBI o cualquier otra herramienta de visualización de datos.

> [!TIP]
> Si está familiarizado con Visual Studio y con los conceptos básicos de cambio de los valores de constantes y variables de C#, puede usar las [herramientas convertidoras de registros](https://github.com/Azure-Samples/networking-dotnet-log-converter) que encontrará en GitHub.


## <a name="next-steps"></a>Pasos siguientes

Ahora que ha configurado el firewall para recopilar registros, puede explorar Log Analytics para ver los datos.

> [!div class="nextstepaction"]
> [Soluciones de supervisión de redes en Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md)

[1]: ./media/tutorial-diagnostics/figure1.png
[2]: ./media/tutorial-diagnostics/figure2.png
