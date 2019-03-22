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
ms.openlocfilehash: 4cca65e2be44d2c846cd4034f0a9d7e8c7d9af28
ms.sourcegitcommit: aa3be9ed0b92a0ac5a29c83095a7b20dd0693463
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2019
ms.locfileid: "58260054"
---
# <a name="profile-web-apps-running-on-an-azure-virtual-machine-or-a-virtual-machine-scale-set-by-using-application-insights-profiler"></a>Generación de perfiles de aplicaciones web en ejecución en una máquina virtual o un conjunto de escalado de máquinas virtuales de Azure mediante Application Insights Profiler

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

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

   Los siguientes comandos de PowerShell son un enfoque alternativo para las máquinas virtuales existentes que tocan solo en la extensión de diagnósticos de Azure. Agregue el ProfilerSink mencionado anteriormente para la configuración que es devuelto por el comando Get-AzVMDiagnosticsExtension. A continuación, pasar la configuración actualizada para el comando Set-AzVMDiagnosticsExtension.

    ```powershell
    $ConfigFilePath = [IO.Path]::GetTempFileName()
    # After you export the currently deployed Diagnostics config to a file, edit it to include the ApplicationInsightsProfiler sink.
    (Get-AzVMDiagnosticsExtension -ResourceGroupName "MyRG" -VMName "MyVM").PublicSettings | Out-File -Verbose $ConfigFilePath
    # Set-AzVMDiagnosticsExtension might require the -StorageAccountName argument
    # If your original diagnostics configuration had the storageAccountName property in the protectedSettings section (which is not downloadable), be sure to pass the same original value you had in this cmdlet call.
    Set-AzVMDiagnosticsExtension -ResourceGroupName "MyRG" -VMName "MyVM" -DiagnosticsConfigurationPath $ConfigFilePath
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

## <a name="set-profiler-sink-using-azure-resource-explorer"></a>Conjunto de Profiler receptor mediante el Explorador de recursos de Azure
Aún no tenemos una manera de configurar el receptor de Application Insights Profiler desde el portal. En lugar de usar powershell como se describió anteriormente, puede usar Azure Resource Explorer para establecer el receptor. Pero tenga en cuenta que si implementa la máquina virtual de nuevo, el receptor se perderán. Deberá actualizar la configuración que se usa al implementar la máquina virtual para mantener esta configuración.

1. Compruebe que la extensión de diagnósticos de Windows Azure está instalada mediante la visualización de las extensiones instaladas para la máquina virtual.  

    ![Comprobar si está instalada la extensión WAD][wadextension]

1. Busque la extensión de diagnósticos de máquina virtual para la máquina virtual. Expanda el grupo de recursos, virtualMachines Microsoft.Compute, nombre de máquina virtual y extensiones.  

    ![Vaya a configuración de WAD en el Explorador de recursos de Azure][azureresourceexplorer]

1. Adición del receptor de Application Insights Profiler en el nodo SinksConfig en WadCfg. Si aún no tiene una sección SinksConfig, deberá agregar uno. Asegúrese de especificar la clave de instrumentación de Application Insights adecuado en la configuración. Deberá cambiar el modo de exploradores a lectura/escritura en la esquina superior derecha y pulse el botón 'Editar' azul.

    ![Agregar receptor de Application Insights Profiler][resourceexplorersinksconfig]

1. Cuando haya terminado la edición de la configuración, presione 'Put'. Si la put se realiza correctamente, aparecerá una marca de verificación verde en el medio de la pantalla.

    ![Enviar una solicitud put para aplicar los cambios][resourceexplorerput]






## <a name="can-profiler-run-on-on-premises-servers"></a>¿Puede ejecutarse Profiler en servidores locales?
No tenemos planes para que Application Insights Profiler sea compatible con servidores locales.

## <a name="next-steps"></a>Pasos siguientes

- Genere tráfico para su aplicación (por ejemplo, inicie una [prueba de disponibilidad](monitor-web-app-availability.md)). A continuación, espere de 10 a 15 minutos para que se empiecen a enviar seguimientos a la instancia de Application Insights.
- Consulte [Seguimientos de Profiler](profiler-overview.md?toc=/azure/azure-monitor/toc.json) en Azure Portal.
- Para obtener ayuda para solucionar problemas de Profiler, consulte la sección [Solución de problemas de Profiler](profiler-troubleshooting.md?toc=/azure/azure-monitor/toc.json).

[azureresourceexplorer]: ./media/profiler-vm/azure-resource-explorer.png
[resourceexplorerput]: ./media/profiler-vm/resource-explorer-put.png
[resourceexplorersinksconfig]: ./media/profiler-vm/resource-explorer-sinks-config.png
[wadextension]: ./media/profiler-vm/wad-extension.png
