---
title: Recopilar registros del servicio de Azure y las métricas en el área de trabajo de Log Analytics | Microsoft Docs
description: Configurar los diagnósticos en recursos de Azure para escribir registros y métricas en el área de trabajo de Log Analytics en Azure Monitor.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: 84105740-3697-4109-bc59-2452c1131bfe
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/12/2017
ms.author: magoedte
ms.openlocfilehash: d086b6f844deb06d98edec8d8ec0f5670d84f066
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59006271"
---
# <a name="collect-azure-service-logs-and-metrics-into-log-analytics-workspace-in-azure-monitor"></a>Recopilar métricas y registros de servicio de Azure en el área de trabajo de Log Analytics en Azure Monitor

Hay cuatro maneras diferentes de recopilar registros y métricas para servicios de Azure:

1. Diagnósticos de Azure directos al área de trabajo de Log Analytics en Azure Monitor (*diagnósticos* en la tabla siguiente)
2. Diagnósticos de Azure en Azure storage al área de trabajo de Log Analytics en Azure Monitor (*almacenamiento* en la tabla siguiente)
3. Conectores para servicios de Azure (*Conectores* en la tabla siguiente)
4. Scripts para recopilar y, a continuación, publicar datos en el área de trabajo de Log Analytics en Azure Monitor (espacios en blanco en la tabla siguiente y servicios que no aparecen)


| Servicio                 | Tipo de recurso                           | Registros        | Métricas     | Solución |
| --- | --- | --- | --- | --- |
| Puertas de enlace de aplicaciones    | Microsoft.Network/applicationGateways   | Diagnóstico | Diagnóstico | [Azure Application Gateway Analytics](../insights/azure-networking-analytics.md#azure-application-gateway-analytics-solution-in-azure-monitor) |
| Application Insights    |                                         | Conector   | Conector   | [Application Insights Connector](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/) (versión preliminar) |
| Cuentas de Automation     | Microsoft.Automation/AutomationAccounts | Diagnóstico |             | [Más información](../../automation/automation-manage-send-joblogs-log-analytics.md)|
| Cuentas de Batch          | Microsoft.Batch/batchAccounts           | Diagnóstico | Diagnóstico | |
| Servicios en la nube clásica  |                                         | Almacenamiento     |             | [Más información](azure-storage-iis-table.md) |
| Cognitive Services      | Microsoft.CognitiveServices/accounts    |             | Diagnóstico | |
| Data Lake Analytics     | Microsoft.DataLakeAnalytics/accounts    | Diagnóstico |             | |
| Data Lake Store         | Microsoft.DataLakeStore/accounts        | Diagnóstico |             | |
| Espacio de nombres del Centro de eventos     | Microsoft.EventHub/namespaces           | Diagnóstico | Diagnóstico | |
| IoT Hubs                | Microsoft.Devices/IotHubs               |             | Diagnóstico | |
| Key Vault               | Microsoft.KeyVault/vaults               | Diagnóstico |             | [KeyVault Analytics](../insights/azure-key-vault.md) |
| Equilibradores de carga          | Microsoft.Network/loadBalancers         | Diagnóstico |             |  |
| Logic Apps              | Microsoft.Logic/workflows <br> Microsoft.Logic/integrationAccounts | Diagnóstico | Diagnóstico | |
| Grupos de seguridad de red | Microsoft.Network/networksecuritygroups | Diagnóstico |             | [Azure Network Security Group Analytics](../insights/azure-networking-analytics.md#azure-network-security-group-analytics-solution-in-azure-monitor) |
| Almacenes de recuperación         | Microsoft.RecoveryServices/vaults       |             |             | [Azure Recovery Services Analytics (versión preliminar)](https://github.com/krnese/AzureDeploy/blob/master/OMS/MSOMS/Solutions/recoveryservices/)|
| Servicios de búsqueda         | Microsoft.Search/searchServices         | Diagnóstico | Diagnóstico | |
| Espacio de nombres de Service Bus   | Microsoft.ServiceBus/namespaces         | Diagnóstico | Diagnóstico | [Service Bus Analytics (versión preliminar)](https://github.com/Azure/azure-quickstart-templates/tree/master/oms-servicebus-solution)|
| Service Fabric          |                                         | Almacenamiento     |             | [Service Fabric Analytics (versión preliminar)](../../service-fabric/service-fabric-diagnostics-oms-setup.md) |
| SQL (v12)               | Microsoft.Sql/servers/databases <br> Microsoft.Sql/servers/elasticPools |             | Diagnóstico | [Azure SQL Analytics (versión preliminar)](../insights/azure-sql.md) |
| Almacenamiento                 |                                         |             | Script      | [Azure Storage Analytics (versión preliminar)](https://github.com/Azure/azure-quickstart-templates/tree/master/oms-azure-storage-analytics-solution) |
| Virtual Machines        | Microsoft.Compute/virtualMachines       | Extensión   | Extensión <br> Diagnóstico  | |
| Conjuntos de escalado de máquinas virtuales | Microsoft.Compute/virtualMachines <br> Microsoft.Compute/virtualMachineScaleSets/virtualMachines |             | Diagnóstico | |
| Granjas de servidores web        | Microsoft.Web/serverfarms               |             | Diagnóstico | |
| Sitios web               | Microsoft.Web/sites <br> Microsoft.Web/sites/slots |             | Diagnóstico | [Azure Web Apps Analytics (versión preliminar)](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-web-apps-analytics) |


> [!NOTE]
> Para la supervisión de máquinas virtuales de Azure (Linux y Windows), se recomienda instalar la [extensión para máquinas virtuales de Log Analytics](../../azure-monitor/learn/quick-collect-azurevm.md). El agente proporciona información recopilada desde dentro de las máquinas virtuales. También puede utilizar la extensión para conjuntos de escalado de máquinas virtuales.
>
>

## <a name="azure-diagnostics-direct-to-log-analytics"></a>Diagnósticos de Azure directos a Log Analytics
Muchos recursos de Azure son capaces de escribir registros de diagnóstico y métricas directamente a un área de trabajo de Log Analytics en Azure Monitor y esta es la manera preferida de recopilar datos de análisis. Al utilizar diagnóstico de Azure, los datos se escriben inmediatamente en el área de trabajo, y no es necesario escribir primero los datos en almacenamiento.

Recursos de Azure que admiten [Azure monitor](../../azure-monitor/overview.md) puede enviar sus registros y métricas directamente a un área de trabajo de Log Analytics.

> [!NOTE]
> Actualmente no se admite el envío de métricas multidimensionales para un área de trabajo de Log Analytics a través de la configuración de diagnóstico. Las métricas con dimensiones se exportan como métricas unidimensionales planas agregadas a través de los valores de dimensión.
>
> *Por ejemplo*: la métrica "Mensajes entrantes" de una instancia de Event Hub se puede explorar y representar gráficamente por colas. Sin embargo, cuando se exporta a través de la configuración de diagnóstico, la métrica se representa con todos los mensajes entrantes de todas las colas de Event Hub.
>
>

* Para obtener información detallada sobre las métricas disponibles, consulte las [métricas admitidas con Azure Monitor](../../azure-monitor/platform/metrics-supported.md).
* Para obtener información detallada sobre los registros disponibles, consulte los [servicios admitidos y el esquema de los registros de diagnóstico](../../azure-monitor/platform/diagnostic-logs-schema.md).

### <a name="enable-diagnostics-with-powershell"></a>Habilitación de diagnósticos con PowerShell

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

El ejemplo de PowerShell siguiente muestra cómo usar [conjunto AzDiagnosticSetting](/powershell/module/Az.Monitor/Set-AzDiagnosticSetting) para habilitar diagnósticos en un grupo de seguridad de red. El mismo enfoque funciona para todos los recursos admitidos: establezca `$resourceId` en el identificador de recurso del recurso para el que desea habilitar los diagnósticos.

```powershell
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$resourceId = "/SUBSCRIPTIONS/ec11ca60-1234-491e-5678-0ea07feae25c/RESOURCEGROUPS/DEMO/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/DEMO"

Set-AzDiagnosticSetting -ResourceId $ResourceId  -WorkspaceId $workspaceId -Enabled $true
```

### <a name="enable-diagnostics-with-resource-manager-templates"></a>Habilitación de diagnósticos con plantillas de Resource Manager

Para habilitar los diagnósticos en un recurso cuando se crea, y que los diagnósticos se envíen al área de trabajo de Log Analytics, puede usar una plantilla similar a la siguiente. Este ejemplo es para una cuenta de Automation, pero funciona con todos los tipos de recursos admitidos.

```json
        {
            "type": "Microsoft.Automation/automationAccounts/providers/diagnosticSettings",
            "name": "[concat(parameters('omsAutomationAccountName'), '/', 'Microsoft.Insights/service')]",
            "apiVersion": "2015-07-01",
            "dependsOn": [
                "[concat('Microsoft.Automation/automationAccounts/', parameters('omsAutomationAccountName'))]",
                "[concat('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspaceName'))]"
            ],
            "properties": {
                "workspaceId": "[resourceId('Microsoft.OperationalInsights/workspaces', parameters('omsWorkspaceName'))]",
                "logs": [
                    {
                        "category": "JobLogs",
                        "enabled": true
                    },
                    {
                        "category": "JobStreams",
                        "enabled": true
                    }
                ]
            }
        }
```

[!INCLUDE [log-analytics-troubleshoot-azure-diagnostics](../../../includes/log-analytics-troubleshoot-azure-diagnostics.md)]

## <a name="azure-diagnostics-to-storage-then-to-log-analytics"></a>Diagnósticos de Azure a almacenamiento y luego a Log Analytics

Para recopilar registros desde dentro de algunos recursos, es posible enviar los registros al almacenamiento de Azure y, a continuación, configurar el área de trabajo de Log Analytics para leer los registros de almacenamiento.

Azure Monitor puede usar este enfoque para recopilar diagnósticos de Azure storage para los registros y los recursos siguientes:

| Recurso | Registros |
| --- | --- |
| Service Fabric |ETWEvent <br> Evento operativo <br> Eventos de Reliable Actors <br> Evento de Reliable Services |
| Virtual Machines |Syslog de Linux <br> Evento de Windows <br> Registro de IIS <br> Windows ETWEvent |
| Roles web <br> Roles de trabajo |Syslog de Linux <br> Evento de Windows <br> Registro de IIS <br> Windows ETWEvent |

> [!NOTE]
> Se le cobrará tarifas Azure normales de almacenamiento y transacciones al enviar diagnósticos a una cuenta de almacenamiento y para cuando el área de trabajo de Log Analytics lee los datos de la cuenta de almacenamiento.
>
>

Consulte [Use blob storage para IIS y table storage para eventos](azure-storage-iis-table.md) para obtener más información sobre cómo puede recopilar estos registros de Azure Monitor.

## <a name="connectors-for-azure-services"></a>Conectores para servicios de Azure

Hay un conector para Application Insights, que permite que los datos recopilados por Application Insights se envíen a un área de trabajo de Log Analytics.

Obtenga más información sobre el [conector de Application Insights](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/).

## <a name="scripts-to-collect-and-post-data-to-log-analytics-workspace"></a>Scripts para recopilar y publicar datos en el área de trabajo de Log Analytics

Puede usar un script de automatización de Azure para recopilar el registro y métricas para servicios de Azure que no proporcionan una manera directa de enviar registros y métricas a un área de trabajo de Log Analytics. La secuencia de comandos, a continuación, puede enviar los datos en el área de trabajo mediante el [API del recopilador de datos](../../azure-monitor/platform/data-collector-api.md)

La Galería de plantillas de Azure tiene [ejemplos del uso de Azure Automation](https://azure.microsoft.com/resources/templates/?term=OMS) para recopilar datos de servicios y enviarlo a Azure Monitor.

## <a name="next-steps"></a>Pasos siguientes

* Consulte [Use blob storage for IIS and table storage for events](azure-storage-iis-table.md) (Uso de Blob Storage para IIS y de Table Storage para eventos) para aprender a leer los registros para servicios de Azure que escriben diagnósticos en Table Storage o registros ISS en Blob Storage.
* [Incorporación de soluciones de Log Analytics desde la galería de soluciones](../../azure-monitor/insights/solutions.md) para más información sobre los datos.
* [Búsquedas de registros en Log Analytics](../../azure-monitor/log-query/log-query-overview.md) para analizar los datos.
