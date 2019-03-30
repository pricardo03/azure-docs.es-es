---
title: 'Azure Service Fabric: configurar la supervisión con los registros de Azure Monitor | Microsoft Docs'
description: Aprenda a configurar registros de Azure Monitor para visualizar y analizar eventos para supervisar los clústeres de Azure Service Fabric.
services: service-fabric
documentationcenter: .net
author: srrengar
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/20/2019
ms.author: srrengar
ms.openlocfilehash: 3523a2df413740f644151c548e403c39c9be1f03
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2019
ms.locfileid: "58670513"
---
# <a name="set-up-azure-monitor-logs-for-a-cluster"></a>Configurar registros de Azure Monitor para un clúster

Para supervisar los eventos de nivel de clúster, recomendamos los registros de Azure Monitor. Puede configurar un área de trabajo de Log Analytics mediante Azure Resource Manager, PowerShell o Azure Marketplace. Si mantiene una plantilla de Resource Manager actualizada de la implementación para un uso futuro, utilice la misma plantilla para configurar el entorno de los registros de Azure Monitor. La implementación a través de Marketplace es más fácil si ya tiene un clúster implementado con Diagnostics habilitado. Si no tiene acceso de nivel de suscripción en la cuenta en la que va a implementar, use PowerShell, realice la implementación mediante PowerShell o la plantilla de Resource Manager.

> [!NOTE]
> Para configurar los registros de Azure Monitor para supervisar el clúster, debe tener habilitado diagnostics para ver los eventos de clúster o nivel de plataforma. Consulte [cómo configurar diagnósticos en clústeres de Windows](service-fabric-diagnostics-event-aggregation-wad.md) y [cómo configurar diagnósticos en clústeres de Linux](service-fabric-diagnostics-oms-syslog.md) para más información

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="deploy-a-log-analytics-workspace-by-using-azure-marketplace"></a>Implementación de un área de trabajo de Log Analytics mediante Azure Marketplace

Si quiere agregar un área de trabajo de Log Analytics después de haber implementado un clúster, vaya a Azure Marketplace en el portal y busque **Service Fabric Analytics**. Esta es una solución personalizada para implementaciones de Service Fabric que tiene datos específicos de Service Fabric. En este proceso creará tanto la solución (el panel para ver información) como el área de trabajo (la agregación de los datos del clúster subyacentes).

1. Haga clic en **Nuevo** en el menú de navegación de la izquierda. 

2. Busque **Service Fabric Analytics**. Seleccione el recurso que aparece.

3. Seleccione **Crear**.

    ![Service Fabric Analytics en Marketplace](media/service-fabric-diagnostics-event-analysis-oms/service-fabric-analytics.png)

4. En la ventana de creación de Service Fabric Analytics, haga clic en **Seleccione un área de trabajo** para el campo **Área de trabajo de OMS** y haga clic en **Create a new workspace** (Crear un área de trabajo). Rellene las entradas necesarias. El único requisito en este caso es que la suscripción del clúster de Service Fabric sea la misma que la del área de trabajo. Después de haber validado las entradas, el área de trabajo comienza la implementación. Esta operación solo tarda unos minutos.

5. Cuando termine, seleccione **Crear** de nuevo en la parte inferior de la ventana de creación de Service Fabric Analytics. Asegúrese de que la nueva área de trabajo aparece en **Área de trabajo de OMS**. Esta acción agrega la solución al área de trabajo que ha creado.

Si usa Windows, continúe con los pasos siguientes para conectar los registros de Azure Monitor con la cuenta de almacenamiento donde se almacenan los eventos de clúster. 

>[!NOTE]
>La solución Service Fabric Analytics solo se admite para los clústeres de Windows. Para clústeres de Linux, consulte nuestro artículo sobre [cómo configurar registros de Azure Monitor para los clústeres de Linux](service-fabric-diagnostics-oms-syslog.md).  

### <a name="connect-the-log-analytics-workspace-to-your-cluster"></a>Conexión del área de trabajo de Log Analytics al clúster 

1. El área de trabajo debe conectarse a los datos de diagnóstico que proceden del clúster. Vaya al grupo de recursos donde creó la solución Service Fabric Analytics. Seleccione **ServiceFabric\<nameOfWorkspace\>** y vaya a su página de información general. Desde allí, puede cambiar la configuración de la solución y la configuración del área de trabajo, además tener acceso al área de trabajo de Log Analytics.

2. En el menú de navegación izquierdo, haga clic en **Orígenes de datos del área de trabajo**, seleccione **Registros de las cuentas de almacenamiento**.

3. En la página **Registros de las cuentas de almacenamiento**, seleccione **Agregar** en la parte superior para agregar los registros del clúster al área de trabajo.

4. Seleccione **Cuenta de Storage** para agregar la cuenta correspondiente que se creó en el clúster. Si usó el nombre predeterminado, la cuenta de Storage se denomina **sfdg\<resourceGroupName\>**. Puede confirmar esto con la plantilla de Azure Resource Manager que se usa para implementar el clúster, mediante la comprobación del valor usado para **applicationDiagnosticsStorageAccountName**. Si el nombre no aparece, desplácese hacia abajo y seleccione **Cargar más**. Escriba el nombre de la cuenta de almacenamiento.

5. Especifique el tipo de datos. Establézcalo en **Eventos de Service Fabric**.

6. Asegúrese de que el origen se establece automáticamente en **WADServiceFabric\*EventTable**.

7. Haga clic en **Aceptar** para conectar el área de trabajo a los registros del clúster.

    ![Agregar registros de la cuenta de almacenamiento para registros de Azure Monitor](media/service-fabric-diagnostics-event-analysis-oms/add-storage-account.png)

La cuenta se muestra ahora como parte de los registros de la cuenta de Storage en los orígenes de datos del área de trabajo.

Ha agregado la solución Service Fabric Analytics a un área de trabajo de Log Analytics que ahora está correctamente conectada a la plataforma del clúster y a la tabla de registros de aplicaciones. Puede agregar orígenes adicionales al área de trabajo de la misma manera.


## <a name="deploy-azure-monitor-logs-with-azure-resource-manager"></a>Implementar los registros de Azure Monitor con Azure Resource Manager

Al implementar un clúster con una plantilla de Resource Manager, la plantilla crea una nueva área de trabajo de Log Analytics, agrega la solución de Service Fabric al área de trabajo y la configura para leer datos de las tablas de almacenamiento apropiadas.

Puede usar y modificar [esta plantilla de ejemplo](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Windows-OMS-UnSecure) para satisfacer sus necesidades. Esta plantilla hace lo siguiente

* Crea un clúster de 5 nodos de Service Fabric
* Crea un área de trabajo de Log Analytics y una solución de Service Fabric
* Configura el agente de Log Analytics para recopilar y enviar dos contadores de rendimiento de ejemplo al área de trabajo
* Configura WAD para recopilar datos de Service Fabric y los envía a tablas de almacenamiento de Azure (WADServiceFabric*EventTable)
* Configura el área de trabajo de Log Analytics para leer los eventos de estas tablas


Puede implementar la plantilla como actualización de Resource Manager para el clúster mediante la API `New-AzureRmResourceGroupDeployment` del módulo AzureRM PowerShell. Un comando de ejemplo sería:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName "<resourceGroupName>" -TemplateFile "<templatefile>.json" 
``` 

Azure Resource Manager detecta que este comando es una actualización para un recurso existente. Solo se procesan los cambios entre la plantilla en que se basa la implementación existente y la nueva plantilla proporcionada.

## <a name="deploy-azure-monitor-logs-with-azure-powershell"></a>Implementar los registros de Azure Monitor con Azure PowerShell

También puede implementar el recurso de log analytics a través de PowerShell mediante el `New-AzureRmOperationalInsightsWorkspace` comando. Para usar este método, asegúrese de que ha instalado [Azure PowerShell](https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps). Use este script para crear una nueva área de trabajo de Log Analytics y agregue la solución Service Fabric a esta: 

```powershell

$SubID = "<subscription ID>"
$ResourceGroup = "<Resource group name>"
$Location = "<Resource group location>"
$WorkspaceName = "<Log Analytics workspace name>"
$solution = "ServiceFabric"

# Log in to Azure and access the correct subscription
Connect-AzureRmAccount
Select-AzureRmSubscription -SubscriptionId $SubID 

# Create the resource group if needed
try {
    Get-AzureRmResourceGroup -Name $ResourceGroup -ErrorAction Stop
} catch {
    New-AzureRmResourceGroup -Name $ResourceGroup -Location $Location
}

New-AzureRmOperationalInsightsWorkspace -Location $Location -Name $WorkspaceName -Sku Standard -ResourceGroupName $ResourceGroup
Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -IntelligencePackName $solution -Enabled $true

```

Cuando haya terminado, siga los pasos descritos en la sección anterior para conectar los registros de Azure Monitor para la cuenta de almacenamiento adecuada.

También puede agregar otras soluciones o realizar otras modificaciones al área de trabajo de Log Analytics mediante PowerShell. Para obtener más información, consulte [administrar Azure Monitor registra el uso de PowerShell](../azure-monitor/platform/powershell-workspace-configuration.md).

## <a name="next-steps"></a>Pasos siguientes
* [Implemente el agente de Log Analytics](service-fabric-diagnostics-oms-agent.md) en los nodos para recopilar contadores de rendimiento, así como estadísticas y registros de Docker de sus contenedores.
* Familiarícese con la [búsqueda de registros y realizar consultas](../log-analytics/log-analytics-log-searches.md) características se ofrecen como parte de los registros de Azure Monitor
* [Use el Diseñador de vistas para crear vistas personalizadas en los registros de Azure Monitor](../azure-monitor/platform/view-designer.md)
