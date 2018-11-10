---
title: Generación de perfiles de aplicaciones de Azure Service Fabric en vivo con Application Insights | Microsoft Docs
description: Habilitación del generador de perfiles para una aplicación de Service Fabric
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
ms.openlocfilehash: 071d1acb592220f7da83a47fd7dffa757ef81a95
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/26/2018
ms.locfileid: "50142974"
---
# <a name="profile-live-azure-service-fabric-applications-with-application-insights"></a>Generación de perfiles de aplicaciones de Azure Service Fabric en vivo con Application Insights

También puede implementar Application Insights Profiler en estos servicios:
* [Azure Web Apps](app-insights-profiler.md?toc=/azure/azure-monitor/toc.json)
* [Cloud Services](app-insights-profiler-cloudservice.md?toc=/azure/azure-monitor/toc.json)
* [Máquinas virtuales](app-insights-profiler-vm.md?toc=/azure/azure-monitor/toc.json)


## <a name="set-up-the-environment-deployment-definition"></a>Configuración de la definición de implementación del entorno

Application Insights Profiler se incluye con Windows Azure Diagnostics (WAD). La extensión WAD puede instalarse mediante una plantilla de Azure RM para el clúster de Service Fabric. Hay una plantilla de ejemplo aquí: [**Plantilla que instala WAD en un clúster de Service Fabric.**](https://github.com/Azure/azure-docs-json-samples/blob/master/application-insights/ServiceFabricCluster.json)

Para configurar su entorno, realice las siguientes acciones:
1. Para asegurarse de que usa [.NET Framework 4.6.1](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) o una versión más reciente, es suficiente con confirmar que el sistema operativo implementado sea `Windows Server 2012 R2` u otro posterior.

1. Busque la extensión [Azure Diagnostics](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics) en el archivo de plantilla de implementación y agregue la siguiente sección `SinksConfig` como elemento secundario de `WadCfg`. Reemplace el valor de la propiedad `ApplicationInsightsProfiler` por su propia clave de instrumentación de Application Insights:  

      ```json
      "SinksConfig": {
        "Sink": [
          {
            "name": "MyApplicationInsightsProfilerSink",
            "ApplicationInsightsProfiler": "00000000-0000-0000-0000-000000000000"
          }
        ]
      }
      ```

      Para obtener información acerca de cómo agregar la extensión de Diagnostics a una plantilla de implementación, consulte [Uso de la supervisión y el diagnóstico con una máquina virtual Windows y plantillas de Azure Resource Manager](https://docs.microsoft.com/azure/virtual-machines/windows/extensions-diagnostics-template?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
1. Implemente el clúster de Service Fabric mediante la plantilla de Azure Resource Manager. Si la configuración es correcta, Application Insights Profiler se instala y se habilita cuando está instalada la extensión WAD. 
1. Agregue Application Insights a la aplicación de Service Fabric. La aplicación debe estar enviando los datos de solicitud a Application Insights para que el generador de perfiles pueda recopilar los perfiles de las solicitudes. Puede encontrar las instrucciones [aquí.](https://github.com/Microsoft/ApplicationInsights-ServiceFabric)
1. Vuelva a implementar la aplicación.

> [SUGERENCIA] Para Virtual Machines, una alternativa a los pasos basados en JSON anteriores es ir a **Virtual Machines**>**Configuración de diagnóstico**>**Receptores** en Azure Portal, establecer el envío de datos de diagnóstico a Application Insights en **Habilitado** y seleccionar una cuenta de Application Insights o una ikey específica.

## <a name="next-steps"></a>Pasos siguientes

- Genere tráfico para su aplicación (por ejemplo, inicie una [prueba de disponibilidad](https://docs.microsoft.com/azure/application-insights/app-insights-monitor-web-app-availability)). A continuación, espere de 10 a 15 minutos para que se empiecen a enviar seguimientos a la instancia de Application Insights.
- Consulte [Seguimientos de Profiler](https://docs.microsoft.com/azure/application-insights/app-insights-profiler-overview?toc=/azure/azure-monitor/toc.json) en Azure Portal.
- Obtenga ayuda para solucionar problemas de Profiler en la sección [Solución de problemas](app-insights-profiler-troubleshooting.md?toc=/azure/azure-monitor/toc.json) de Profiler.