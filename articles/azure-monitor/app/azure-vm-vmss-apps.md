---
title: Supervisión del rendimiento de las aplicaciones hospedadas en máquinas virtuales de Azure y conjuntos de escalado de máquinas virtuales de Azure | Microsoft Docs
description: Supervisión del rendimiento de aplicaciones para máquinas virtuales de Azure y conjuntos de escalado de máquinas virtuales de Azure. Carga y tiempo de respuesta de gráfico, información de dependencia y establecer alertas en el rendimiento.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.topic: conceptual
ms.date: 06/27/2019
ms.author: mbullwin
ms.openlocfilehash: f2c6b98fd0be2061e9d8cab5c063cafadf71476a
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/29/2019
ms.locfileid: "68597453"
---
# <a name="monitor-application-performance-hosted-on-azure-vm-and-azure-virtual-machine-scale-sets"></a>Supervisión del rendimiento de las aplicaciones hospedadas en máquinas virtuales de Azure y conjuntos de escalado de máquinas virtuales de Azure

La habilitación de la supervisión en las aplicaciones web basadas en .NET que se ejecutan en [máquinas virtuales de Azure](https://azure.microsoft.com/services/virtual-machines/) y [conjuntos de escalado de máquinas virtuales](https://docs.microsoft.com/azure/virtual-machine-scale-sets/) ahora es más fácil que nunca. Obtenga todas las ventajas de usar Application Insights sin modificar el código.

En este artículo se le guía a través de la habilitación de la supervisión de Application Insights con la extensión ApplicationMonitoringWindows y se proporcionan instrucciones preliminares para automatizar el proceso para implementaciones a gran escala.

> [!IMPORTANT]
> La extensión ApplicationMonitoringWindows de Azure se encuentra actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin un Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="enable-application-insights"></a>Habilitación de Application Insights

Hay dos maneras de habilitar la supervisión de aplicaciones para las aplicaciones hospedadas en máquinas virtuales y conjuntos de escalado de máquinas virtuales de Azure:

* **Supervisión de aplicaciones basada en agentes** (extensión ApplicationMonitoringWindows).
    * Este método es el más fácil de habilitar y no requiere ninguna configuración avanzada. A menudo se conoce como supervisión de "entorno en tiempo de ejecución". En el caso de las máquinas virtuales y los conjuntos de escalado de máquinas virtuales de Azure, se recomienda como mínimo habilitar este nivel de supervisión. Después, en función del escenario, puede evaluar si la instrumentación manual es necesaria.
    * En la actualidad, solo se admiten las aplicaciones .NET hospedadas en IIS.

* **Instrumentación manual de la aplicación mediante código** mediante la instalación del SDK de Application Insights.

    * Este enfoque es mucho más personalizable, pero requiere la [incorporación de una dependencia en los paquetes NuGet del SDK de Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/asp-net). Este método también implica que el usuario tiene que administrar las actualizaciones a la versión más reciente de los paquetes.

    * Si necesita realizar llamadas de API personalizadas para supervisar eventos o dependencias no capturados de manera predeterminada con la supervisión basada en agentes, deberá usar este método. Consulte el [artículo API de Application Insights para eventos y métricas personalizados](https://docs.microsoft.com/azure/azure-monitor/app/api-custom-events-metrics) para obtener más información.

> [!NOTE]
> Si se detecta tanto la supervisión basada en agentes como la instrumentación manual basada en SDK, solo se respetará la configuración de la instrumentación manual. Esto es para evitar que se envíen datos duplicados. Para más información sobre este tema, consulte la [sección Solución de problemas](#troubleshooting) a continuación.

## <a name="manage-agent-based-monitoring-for-net-applications-on-vm-using-powershell"></a>Administración de la supervisión basada en agentes para aplicaciones .NET en máquinas virtuales mediante PowerShell

Instalación o actualización de la extensión de supervisión de aplicaciones para la máquina virtual
```powershell
$publicCfgJsonString = '
{
  "RedfieldConfiguration": {
    "InstrumentationKeyMap": {
      "Filters": [
        {
          "AppFilter": ".*",
          "MachineFilter": ".*",
          "InstrumentationSettings" : {
            "InstrumentationKey": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
          }
        }
      ]
    }
  }
}
';
$privateCfgJsonString = '{}';

Set-AzVMExtension -ResourceGroupName "<myVmResourceGroup>" -VMName "<myVmName>" -Location "South Central US" -Name "ApplicationMonitoring" -Publisher "Microsoft.Azure.Diagnostics" -Type "ApplicationMonitoringWindows" -Version "2.8" -SettingString $publicCfgJsonString -ProtectedSettingString $privateCfgJsonString
```

Desinstalación de la extensión de supervisión de aplicaciones para la máquina virtual
```powershell
Remove-AzVMExtension -ResourceGroupName "<myVmResourceGroup>" -VMName "<myVmName>" -Name "ApplicationMonitoring"
```

Estado de la extensión de supervisión de aplicaciones de consulta para la máquina virtual
```powershell
Get-AzVMExtension -ResourceGroupName "<myVmResourceGroup>" -VMName "<myVmName>" -Name ApplicationMonitoring -Status
```

Obtención de la lista de extensiones instaladas para la máquina virtual
```powershell
Get-AzResource -ResourceId "/subscriptions/<mySubscriptionId>/resourceGroups/<myVmResourceGroup>/providers/Microsoft.Compute/virtualMachines/<myVmName>/extensions"

# Name              : ApplicationMonitoring
# ResourceGroupName : <myVmResourceGroup>
# ResourceType      : Microsoft.Compute/virtualMachines/extensions
# Location          : southcentralus
# ResourceId        : /subscriptions/<mySubscriptionId>/resourceGroups/<myVmResourceGroup>/providers/Microsoft.Compute/virtualMachines/<myVmName>/extensions/ApplicationMonitoring
```

## <a name="manage-agent-based-monitoring-for-net-applications-on-azure-virtual-machine-scale-set-using-powershell"></a>Administración de la supervisión basada en agentes para aplicaciones .NET en conjuntos de escalado de máquinas virtuales de Azure mediante PowerShell

Instalación o actualización de la extensión de supervisión de aplicaciones para el conjunto de escalado de máquinas virtuales de Azure
```powershell
$publicCfgHashtable =
@{
  "RedfieldConfiguration"= @{
    "InstrumentationKeyMap"= @{
      "Filters"= @(
        @{
          "AppFilter"= ".*";
          "MachineFilter"= ".*";
          "InstrumentationSettings"= @{
            "InstrumentationKey"= "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"; # Application Insights Instrumentation Key, create new Application Insights resource if you don't have one. https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/microsoft.insights%2Fcomponents
          }
        }
      )
    }
  }
};
$privateCfgHashtable = @{};

$vmss = Get-AzVmss -ResourceGroupName "<myResourceGroup>" -VMScaleSetName "<myVmssName>"

Add-AzVmssExtension -VirtualMachineScaleSet $vmss -Name "ApplicationMonitoring" -Publisher "Microsoft.Azure.Diagnostics" -Type "ApplicationMonitoringWindows" -TypeHandlerVersion "2.8" -Setting $publicCfgHashtable -ProtectedSetting $privateCfgHashtable

Update-AzVmss -ResourceGroupName $vmss.ResourceGroupName -Name $vmss.Name -VirtualMachineScaleSet $vmss

# Note: depending on your update policy, you might need to run Update-AzVmssInstance for each instance
```

Desinstalación de la extensión de supervisión de aplicaciones para el conjunto de escalado de máquinas virtuales de Azure
```powershell
$vmss = Get-AzVmss -ResourceGroupName "<myResourceGroup>" -VMScaleSetName "<myVmssName>"

Remove-AzVmssExtension -VirtualMachineScaleSet $vmss -Name "ApplicationMonitoring"

Update-AzVmss -ResourceGroupName $vmss.ResourceGroupName -Name $vmss.Name -VirtualMachineScaleSet $vmss

# Note: depending on your update policy, you might need to run Update-AzVmssInstance for each instance
```

Estado de la extensión de supervisión de aplicaciones de consulta para el conjunto de escalado de máquinas virtuales de Azure
```powershell
# Not supported by extensions framework
```

Obtención de la lista de extensiones instaladas para el conjunto de escalado de máquinas virtuales de Azure
```powershell
Get-AzResource -ResourceId /subscriptions/<mySubscriptionId>/resourceGroups/<myResourceGroup>/providers/Microsoft.Compute/virtualMachineScaleSets/<myVmssName>/extensions

# Name              : ApplicationMonitoringWindows
# ResourceGroupName : <myResourceGroup>
# ResourceType      : Microsoft.Compute/virtualMachineScaleSets/extensions
# Location          :
# ResourceId        : /subscriptions/<mySubscriptionId>/resourceGroups/<myResourceGroup>/providers/Microsoft.Compute/virtualMachineScaleSets/<myVmssName>/extensions/ApplicationMonitoringWindows
```

## <a name="troubleshooting"></a>solución de problemas

A continuación se muestra nuestra guía paso a paso de solución de problemas para la supervisión basada en extensiones para aplicaciones basadas en .NET que se ejecutan en máquinas virtuales y conjuntos de escalado de máquinas virtuales de Azure.

> [!NOTE]
> Las aplicaciones .NET Core, Java y Node.js solo se admiten en máquinas virtuales y conjuntos de escalado de máquinas virtuales de Azure a través de la instrumentación manual basada en SDK y, por tanto, los pasos siguientes no se aplican a estos escenarios.

El resultado de la ejecución de las extensiones se registra en los archivos que se encuentran en los siguientes directorios:
```Windows
C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.ApplicationMonitoringWindows\<version>\
```

## <a name="next-steps"></a>Pasos siguientes
* Obtenga información sobre cómo [implementar una aplicación en un conjunto de escalado de máquinas virtuales de Azure](../../virtual-machine-scale-sets/virtual-machine-scale-sets-deploy-app.md).
* [Configure pruebas web de disponibilidad](monitor-web-app-availability.md) para recibir una alerta si el punto de conexión deja de estar activo.
