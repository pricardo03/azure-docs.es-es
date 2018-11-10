---
title: Generación de perfiles de servicios activos en la nube de Azure con Application Insights | Microsoft Docs
description: Habilitación de Application Insights Profiler para Azure Cloud Services.
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
ms.openlocfilehash: 445e607b6b0a21f840ab633b3a5a3779f49fdd98
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/26/2018
ms.locfileid: "50142914"
---
# <a name="profile-live-azure-cloud-services-with-application-insights"></a>Generación de perfiles de servicios activos en la nube de Azure con Application Insights

También puede implementar Application Insights Profiler en estos servicios:
* [Azure Web Apps](app-insights-profiler.md?toc=/azure/azure-monitor/toc.json)
* [Aplicaciones de Service Fabric](app-insights-profiler-servicefabric.md?toc=/azure/azure-monitor/toc.json)
* [Máquinas virtuales](app-insights-profiler-vm.md?toc=/azure/azure-monitor/toc.json)

Application Insights Profiler se instala con la extensión de Windows Azure Diagnostics (WAD). Solo tiene que configurar WAD para instalar el generador de perfiles y enviar perfiles al recurso de Application Insights.

## <a name="enable-profiler-for-your-azure-cloud-service"></a>Habilitación del generador de perfiles para Azure Cloud Services
1. Compruebe que está utilizando [.NET Framework 4.6.1](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) o una versión posterior.  Es suficiente con confirmar que los archivos *ServiceConfiguration.\*.cscfg* tienen un valor de `osFamily` de "5" o posterior.
1. Agregue [el SDK de Application Insights a Cloud Services](app-insights-cloudservices.md?toc=/azure/azure-monitor/toc.json).
1. Realice un seguimiento de las solicitudes con Application Insights:

    Para los roles web de ASP.Net, Application Insights puede realizar un seguimiento de las solicitudes automáticamente.

    Para los roles de trabajo, [agregue código para realizar el seguimiento de solicitudes.](app-insights-profiler-trackrequests.md?toc=/azure/azure-monitor/toc.json)

    

1. Configure la extensión de Windows Azure Diagnostics (WAD) para habilitar Profiler.



    1. Busque el archivo *diagnostics.wadcfgx* de [Azure Diagnostics](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics) correspondiente a su rol de aplicación, como se muestra aquí:  

       ![Ubicación del archivo de configuración de diagnósticos](./media/enable-profiler-compute/cloudservice-solutionexplorer.png)  

        Si no encuentra el archivo, para aprender a habilitar la extensión de Diagnostics en el proyecto de Azure Cloud Services, consulte [Configuración de diagnósticos para Azure Cloud Services y máquinas virtuales](https://docs.microsoft.com/azure/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines#enable-diagnostics-in-cloud-service-projects-before-deploying-them).

    1. Agregue la siguiente sección `SinksConfig` como elemento secundario de `WadCfg`:  

      ```xml
      <WadCfg>
        <DiagnosticMonitorConfiguration>...</DiagnosticMonitorConfiguration>
        <SinksConfig>
          <Sink name="MyApplicationInsightsProfiler">
            <!-- Replace with your own Application Insights instrumentation key. -->
            <ApplicationInsightsProfiler>00000000-0000-0000-0000-000000000000</ApplicationInsightsProfiler>
          </Sink>
        </SinksConfig>
      </WadCfg>
      ```

    >   **NOTA:** Si el archivo *diagnostics.wadcfgx* también contiene otro receptor de tipo `ApplicationInsights`, las tres claves de instrumentación siguientes tienen que coincidir:  
    >  * La clave que usa la aplicación.  
    >  * La clave que usa el receptor `ApplicationInsights`.  
    >  * La clave que usa el receptor `ApplicationInsightsProfiler`.  
    >
    > Puede encontrar el valor real de la clave de instrumentación que usa el receptor `ApplicationInsights` en los archivos *ServiceConfiguration.\*.cscfg*.  
    > Después de la versión de Azure SDK para Visual Studio 15.5, solo es preciso que coincidan las claves de instrumentación que usan la aplicación y el receptor `ApplicationInsightsProfiler`.
1. Implemente el servicio con la nueva configuración de diagnóstico y Application Insights Profiler se configurará para ejecutarse en su servicio.
 
## <a name="next-steps"></a>Pasos siguientes

- Genere tráfico para su aplicación (por ejemplo, inicie una [prueba de disponibilidad](https://docs.microsoft.com/azure/application-insights/app-insights-monitor-web-app-availability)). A continuación, espere de 10 a 15 minutos para que se empiecen a enviar seguimientos a la instancia de Application Insights.
- Consulte [Seguimientos de Profiler](https://docs.microsoft.com/azure/application-insights/app-insights-profiler-overview?toc=/azure/azure-monitor/toc.json) en Azure Portal.
- Obtenga ayuda para solucionar problemas de Profiler en la sección [Solución de problemas](app-insights-profiler-troubleshooting.md?toc=/azure/azure-monitor/toc.json) de Profiler.