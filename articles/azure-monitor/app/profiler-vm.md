---
title: Generación de perfiles de aplicaciones web en ejecución en una VM de Azure mediante Application Insights Profiler | Microsoft Docs
description: Generación de perfiles de aplicaciones web en una VM de Azure mediante Application Insights Profiler.
services: application-insights
documentationcenter: ''
author: cweining
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.reviewer: mbullwin
ms.date: 08/06/2018
ms.author: cweining
ms.openlocfilehash: 01d57a10189f9281736e628a83465c96d282af70
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55860157"
---
# <a name="profile-web-apps-running-on-an-azure-virtual-machine-or-a-virtual-machine-scale-set-by-using-application-insights-profiler"></a>Generación de perfiles de aplicaciones web en ejecución en una máquina virtual o un conjunto de escalado de máquinas virtuales de Azure mediante Application Insights Profiler

También puede implementar Azure Application Insights Profiler en estos servicios:
* [Azure App Service](../../azure-monitor/app/profiler.md?toc=/azure/azure-monitor/toc.json)
* [Azure Cloud Services](profiler-cloudservice.md?toc=/azure/azure-monitor/toc.json)
* [Azure Service Fabric](profiler-vm.md?toc=/azure/azure-monitor/toc.json)

## <a name="deploy-profiler-on-a-virtual-machine-or-a-virtual-machine-scale-set"></a>Implementación de Profiler en una máquina virtual o un conjunto de escalado de máquinas virtuales
En este artículo se muestran los pasos necesarios para que Application Insights Profiler se ejecute en una máquina virtual (VM) de Azure o un conjunto de escalado de máquinas virtuales de Azure. Profiler se instala con la extensión de Azure Diagnostics para VM. Configure la extensión para ejecutar Profiler y compile el SDK de Application Insights en la aplicación.

1. Agregue el SDK de Application Insights a su [aplicación ASP.NET](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net) o [aplicación. NET](windows-services.md?toc=/azure/azure-monitor/toc.json) normal.  
  Para ver los perfiles para las solicitudes, debe enviar telemetría de solicitudes a Application Insights.

1. Instale la extensión de Azure Diagnostics en la VM. Para consultar ejemplos de plantillas de Azure Resource Manager completas:  
    * [Máquina virtual](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachine.json)
    * [Conjunto de escalado de máquinas virtuales](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachineScaleSet.json)
    
    El elemento clave es ApplicationInsightsProfilerSink en WadCfg. Para indicar a Azure Diagnostics que habilite Profiler para enviar datos a la clave iKey, agregue otro receptor a esta sección.
    
    ```json
      "SinksConfig": {
        "Sink": [
          {
            "name": "ApplicationInsightsSink",
            "ApplicationInsights": "85f73556-b1ba-46de-9534-606e08c6120f"
          },
          {
            "name": "MyApplicationInsightsProfilerSink",
            "ApplicationInsightsProfiler": "85f73556-b1ba-46de-9534-606e08c6120f"
          }
        ]
      },
    ```

1. Implemente la definición de implementación de entorno modificada.  

   Al aplicar las modificaciones, generalmente se realiza una implementación de plantilla completa o una publicación basada en el servicio en la nube mediante cmdlets de PowerShell o Visual Studio.  

   Los siguientes comandos de PowerShell se pueden usar como un enfoque alternativo para las máquinas virtuales existentes que solo afecta a la extensión de Azure Diagnostics. Agregue el receptor de Profiler mencionado anteriormente a la configuración devuelta por el comando Get-AzureRmVMDiagnosticsExtension y, a continuación, pase la configuración actualizada al comando Set-AzureRmVMDiagnosticsExtension.

    ```powershell
    $ConfigFilePath = [IO.Path]::GetTempFileName()
    # After you export the currently deployed Diagnostics config to a file, edit it to include the ApplicationInsightsProfiler sink.
    (Get-AzureRmVMDiagnosticsExtension -ResourceGroupName "MyRG" -VMName "MyVM").PublicSettings | Out-File -Verbose $ConfigFilePath
    # Set-AzureRmVMDiagnosticsExtension might require the -StorageAccountName argument
    # If your original diagnostics configuration had the storageAccountName property in the protectedSettings section (which is not downloadable), be sure to pass the same original value you had in this cmdlet call.
    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName "MyRG" -VMName "MyVM" -DiagnosticsConfigurationPath $ConfigFilePath
    ```

1. Si la aplicación deseada se ejecuta mediante [IIS](https://www.microsoft.com/web/downloads/platform.aspx), habilite la característica `IIS Http Tracing` de Windows.

    a. Establezca el acceso remoto al entorno y, a continuación, use la ventana [Agregar características de Windows]( https://docs.microsoft.com/iis/configuration/system.webserver/tracing/). También puede ejecutar el siguiente comando en PowerShell (como administrador):  

    ```powershell
    Enable-WindowsOptionalFeature -FeatureName IIS-HttpTracing -Online -All
    ```  
   b. Si el establecimiento del acceso remoto supone un problema, puede usar la [CLI de Azure](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) para ejecutar el siguiente comando:  

    ```powershell
    az vm run-command invoke -g MyResourceGroupName -n MyVirtualMachineName --command-id RunPowerShellScript --scripts "Enable-WindowsOptionalFeature -FeatureName IIS-HttpTracing -Online -All"
    ```

1. Implementación de aplicación.

## <a name="can-profiler-run-on-on-premises-servers"></a>¿Puede ejecutarse Profiler en servidores locales?
No tenemos planes para que Application Insights Profiler sea compatible con servidores locales.

## <a name="next-steps"></a>Pasos siguientes

- Genere tráfico para su aplicación (por ejemplo, inicie una [prueba de disponibilidad](monitor-web-app-availability.md)). A continuación, espere de 10 a 15 minutos para que se empiecen a enviar seguimientos a la instancia de Application Insights.
- Consulte [Seguimientos de Profiler](profiler-overview.md?toc=/azure/azure-monitor/toc.json) en Azure Portal.
- Para obtener ayuda para solucionar problemas de Profiler, consulte la sección [Solución de problemas de Profiler](profiler-troubleshooting.md?toc=/azure/azure-monitor/toc.json).
