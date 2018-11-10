---
title: Generación de perfiles de aplicaciones web activas de Azure con Application Insights | Microsoft Docs
description: Generación de perfiles de aplicaciones web activas en Azure con Application Insights Profiler.
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
ms.openlocfilehash: 9aae08aa5906f341a890ac15e30d2863109d83a2
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/26/2018
ms.locfileid: "50140015"
---
# <a name="profile-live-azure-web-apps-with-application-insights"></a>Generación de perfiles de aplicaciones web de Azure activas con Application Insights

Actualmente, Profiler puede usarse con aplicaciones web ASP.NET y ASP.NET Core que se ejecutan en Web Apps. Es necesario disponer del nivel de servicio Básico o superior para poder utilizar Profiler.

## <a id="installation"></a> Habilitación de Profiler para su instancia de Web Apps
Para habilitar Profiler para una aplicación web, siga las instrucciones a continuación. Si está ejecutando otro tipo de servicio de Azure, aquí encontrará instrucciones para habilitar Profiler en otras plataformas compatibles:
* [Cloud Services](app-insights-profiler-cloudservice.md?toc=/azure/azure-monitor/toc.json)
* [Aplicaciones de Service Fabric](app-insights-profiler-servicefabric.md?toc=/azure/azure-monitor/toc.json)
* [Máquinas virtuales](app-insights-profiler-vm.md?toc=/azure/azure-monitor/toc.json)


Application Insights Profiler se instala con la extensión de sitio de Application Insights. Tiene que instalar la extensión de sitio y configurarlo para obtener perfiles para Azure Web Apps. Una vez que ha implementado una aplicación web, incluso si ha incluido el SDK de App Insights en el código fuente, siga los pasos a continuación para habilitar el generador de perfiles.

1. Vaya al panel **App Services** en Azure Portal.
1. Vaya al panel **Configuración > Supervisión**.

   ![Habilitación de App Insights en el portal de App Services](./media/app-insights-profiler/AppInsights-AppServices.png)

1. Siga las instrucciones que aparecen en el panel para crear un recurso o seleccione un recurso de App Insights existente para supervisar la aplicación web. Acepte todas las opciones predeterminadas. La opción **Diagnósticos de nivel de código** está activada de forma predeterminada y permite Profiler.

   ![Adición de la extensión de sitio de App Insights][Enablement UI]

1. Profiler se instala ahora con la extensión de sitio de App Insights y se habilita mediante un valor de configuración de aplicación de App Services.

    ![Configuración de aplicación de Profiler][profiler-app-setting]

## <a name="disable-profiler"></a>Deshabilitación de Profiler

Para detener o reiniciar Profiler para una instancia de Web Apps individual, en **Trabajos web**, vaya al recurso de Web Apps. Para eliminar Profiler, vaya a **Extensiones**.

![Deshabilitación de Profiler para un trabajo web][disable-profiler-webjob]

Se recomienda tener habilitado Profiler en todas las aplicaciones web para detectar cualquier problema de rendimiento lo antes posible.

Si usa WebDeploy para implementar cambios en la aplicación web, asegúrese de excluir la carpeta App_Data de la eliminación durante la implementación. De lo contrario, los archivos de la extensión de Profiler se eliminan la próxima vez que implemente la aplicación web en Azure.



## <a name="next-steps"></a>Pasos siguientes

* [Trabajo con Application Insights en Visual Studio](https://docs.microsoft.com/azure/application-insights/app-insights-visual-studio)

[appinsights-in-appservices]:./media/app-insights-profiler/AppInsights-AppServices.png
[Enablement UI]: ./media/app-insights-profiler/Enablement_UI.png
[profiler-app-setting]:./media/app-insights-profiler/profiler-app-setting.png
[performance-blade]: ./media/app-insights-profiler/performance-blade.png
[performance-blade-examples]: ./media/app-insights-profiler/performance-blade-examples.png
[performance-blade-v2-examples]:./media/app-insights-profiler/performance-blade-v2-examples.png
[trace-explorer]: ./media/app-insights-profiler/trace-explorer.png
[trace-explorer-toolbar]: ./media/app-insights-profiler/trace-explorer-toolbar.png
[trace-explorer-hint-tip]: ./media/app-insights-profiler/trace-explorer-hint-tip.png
[trace-explorer-hot-path]: ./media/app-insights-profiler/trace-explorer-hot-path.png
[enable-profiler-banner]: ./media/app-insights-profiler/enable-profiler-banner.png
[disable-profiler-webjob]: ./media/app-insights-profiler/disable-profiler-webjob.png
[linked app services]: ./media/app-insights-profiler/linked-app-services.png

