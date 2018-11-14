---
title: Generación de perfiles de aplicaciones web en ejecución en una máquina virtual de Azure con Application Insights Profiler | Microsoft Docs
description: Generación de perfiles de aplicaciones web en una máquina virtual de Azure con Application Insights Profiler.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.reviewer: cawa
ms.date: 08/06/2018
ms.author: mbullwin
ms.openlocfilehash: 152632c55fc21d2b49f6dfd8ae734833ea870898
ms.sourcegitcommit: 1fc949dab883453ac960e02d882e613806fabe6f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2018
ms.locfileid: "50978373"
---
# <a name="profile-web-apps-running-on-an-azure-virtual-machine-or-virtual-machine-scale-set-with-application-insights-profiler"></a>Generación de perfiles de aplicaciones web en ejecución en una máquina virtual o un conjunto de escalado de máquinas virtuales de Azure con Application Insights Profiler
También puede implementar Application Insights Profiler en estos servicios:
* [Azure Web Apps](app-insights-profiler.md?toc=/azure/azure-monitor/toc.json)
* [Cloud Services](app-insights-profiler-cloudservice.md?toc=/azure/azure-monitor/toc.json)
* [Service Fabric](app-insights-profiler-vm.md?toc=/azure/azure-monitor/toc.json)

## <a name="deploy-profiler-on-a-virtual-machine-or-scale-set"></a>Implementación de Profiler en una máquina virtual o un conjunto de escalado
Esta página le guiará por los pasos necesarios para que Application Insights Profiler se ejecute en una máquina virtual de Azure o un conjunto de escalado de máquinas virtuales de Azure. Application Insights Profiler se instala con la extensión de Windows Azure Diagnostics para máquinas virtuales. Deberá configurar la extensión para ejecutar el generador de perfiles y tener el SDK de App Insights integrado en la aplicación para obtener perfiles para las aplicaciones web que se ejecuten en una máquina virtual.

1. Agregue el SDK de Application Insights a su [aplicación ASP.Net](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net) o [aplicación. NET](https://docs.microsoft.com/azure/application-insights/app-insights-windows-services?toc=/azure/azure-monitor/toc.json) normal. Tiene que enviar telemetría de solicitudes para que Application Insights vea los perfiles para las solicitudes.
1. Instale la extensión de Microsoft Azure Diagnostics en la máquina virtual. Para consultar ejemplos de plantillas de Azure Resource Manager completas:  
    * [Máquina virtual](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachine.json)
    * [Conjunto de escalado de máquinas virtuales](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/WindowsVirtualMachineScaleSet.json)
1. Implemente la definición de implementación de entorno modificada.  

   Para aplicar las modificaciones, generalmente se realiza una implementación de plantilla completa o una publicación basada en el servicio en la nube mediante cmdlets de PowerShell o Visual Studio.  

   Los siguientes comandos de PowerShell se pueden usar como un enfoque alternativo para las máquinas virtuales existentes que solo afecta a la extensión de Azure Diagnostics:  

    ```powershell
    $ConfigFilePath = [IO.Path]::GetTempFileName()
    # After you export the currently deployed Diagnostics config to a file, edit it to include the ApplicationInsightsProfiler sink.
    (Get-AzureRmVMDiagnosticsExtension -ResourceGroupName "MyRG" -VMName "MyVM").PublicSettings | Out-File -Verbose $ConfigFilePath
    # Set-AzureRmVMDiagnosticsExtension might require the -StorageAccountName argument
    # If your original diagnostics configuration had the storageAccountName property in the protectedSettings section (which is not downloadable), be sure to pass the same original value you had in this cmdlet call.
    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName "MyRG" -VMName "MyVM" -DiagnosticsConfigurationPath $ConfigFilePath
    ```

1. Si la aplicación deseada se ejecuta mediante [IIS](https://www.microsoft.com/web/downloads/platform.aspx), habilite la característica `IIS Http Tracing` de Windows mediante los pasos siguientes:  

   a. Establezca acceso remoto al entorno y, después, use la ventana [Agregar características de Windows]( https://docs.microsoft.com/iis/configuration/system.webserver/tracing/) o ejecute el siguiente comando en PowerShell (como administrador):  

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

- Genere tráfico para su aplicación (por ejemplo, inicie una [prueba de disponibilidad](https://docs.microsoft.com/azure/application-insights/app-insights-monitor-web-app-availability)). A continuación, espere de 10 a 15 minutos para que se empiecen a enviar seguimientos a la instancia de Application Insights.
- Consulte [Seguimientos de Profiler](https://docs.microsoft.com/azure/application-insights/app-insights-profiler-overview?toc=/azure/azure-monitor/toc.json) en Azure Portal.
- Obtenga ayuda para solucionar problemas de Profiler en la sección [Solución de problemas](app-insights-profiler-troubleshooting.md?toc=/azure/azure-monitor/toc.json) de Profiler.
