---
title: Generación de perfiles de aplicaciones activas de Azure App Service con Application Insights | Microsoft Docs
description: Cree perfiles de aplicaciones activas de Azure App Service con Application Insights Profiler.
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
ms.openlocfilehash: b8f6a2d12e1a9920421e6491432b516520ae110b
ms.sourcegitcommit: 6e32f493eb32f93f71d425497752e84763070fad
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/10/2019
ms.locfileid: "59469988"
---
# <a name="profile-live-azure-app-service-apps-with-application-insights"></a>Generación de perfiles de aplicaciones activas de Azure App Service con Application Insights

Puede ejecutar Profiler en ASP.NET y ASP.NET Core aplicaciones que se ejecutan en Azure App Service mediante el nivel de servicio básico o superior. Actualmente, la habilitación de Profiler en Linux solo es posible a través de [este método](profiler-aspnetcore-linux.md).

## <a id="installation"></a> Habilitación de Profiler en la aplicación
Para habilitar Profiler en una aplicación, siga las instrucciones que se indican a continuación. Si está ejecutando otro tipo de servicio de Azure, aquí encontrará instrucciones para habilitar Profiler en otras plataformas compatibles:
* [Cloud Services](../../azure-monitor/app/profiler-cloudservice.md?toc=/azure/azure-monitor/toc.json)
* [Aplicaciones de Service Fabric](../../azure-monitor/app/profiler-servicefabric.md?toc=/azure/azure-monitor/toc.json)
* [Virtual Machines](../../azure-monitor/app/profiler-vm.md?toc=/azure/azure-monitor/toc.json)

Application Insights Profiler se instalaron previamente como parte del tiempo de ejecución de servicios de aplicaciones. Los pasos siguientes le mostrará cómo habilitarlo para App Service. Incluso si ha incluido el SDK de App Insights en la aplicación en tiempo de compilación, siga estos pasos.

1. Habilite a la opción "Siempre activado" para el servicio de aplicación. Puede actualizar la configuración en la página de configuración del servicio en la aplicación en configuración General.
1. Vaya al panel **App Services** en Azure Portal.
1. Vaya al panel **Configuración > Application Insights**.

   ![Habilitación de App Insights en el portal de App Services](./media/profiler/AppInsights-AppServices.png)

1. Siga las instrucciones que aparecen en el panel para crear un nuevo recurso o seleccione un recurso de App Insights existente para supervisar la aplicación. También, asegúrese de que Profiler esté **activado**. Si el recurso de Application Insights está en otra suscripción de App Service, no puede usar esta página para configurar Application Insights. Todavía puede hacerlo manualmente aunque mediante la creación de la configuración de aplicación necesario manualmente. [La siguiente sección contiene instrucciones para habilitar Profiler de forma manual.](#enable-profiler-manually-or-with-azure-resource-manager) 

   ![Adición de la extensión de sitio de App Insights][Enablement UI]

1. Profiler está habilitado con una configuración de aplicación de App Services.

    ![Configuración de aplicación de Profiler][profiler-app-setting]

## <a name="enable-profiler-manually-or-with-azure-resource-manager"></a>Habilitar a Profiler manualmente o con Azure Resource Manager
Application Insights Profiler puede habilitarse mediante la creación de la configuración de la aplicación para Azure App Service. La página con las opciones mostradas anteriormente crea esta configuración de aplicación por usted. Pero es posible automatizar la creación de estas opciones mediante una plantilla u otros medios. Esta configuración también funciona si el recurso de Application Insights está en otra suscripción de Azure App Service.
Estos son los valores necesarios para habilitar el generador de perfiles:

|Configuración de aplicación    | Valor    |
|---------------|----------|
|APPINSIGHTS_INSTRUMENTATIONKEY         | clave de instrumentación para el recurso de Application Insights    |
|APPINSIGHTS_PROFILERFEATURE_VERSION | 1.0.0 |
|DiagnosticServices_EXTENSION_VERSION | ~3 |


Puede establecer estos valores con [plantillas de Azure Resource Manager](../../azure-monitor/app/azure-web-apps.md#app-service-application-settings-with-azure-resource-manager), [Azure Powershell](https://docs.microsoft.com/powershell/module/az.websites/set-azwebapp), [CLI de Azure](https://docs.microsoft.com/cli/azure/webapp/config/appsettings?view=azure-cli-latest).


## <a name="disable-profiler"></a>Deshabilitación de Profiler

Para detener o reiniciar el generador de perfiles en una determina instancia de la aplicación, en **Trabajos web**, vaya al recurso de la aplicación. Para eliminar Profiler, vaya a **Extensiones**.

![Deshabilitación de Profiler para un trabajo web][disable-profiler-webjob]

Se recomienda tener habilitado el generador de perfiles en todas las aplicaciones para detectar cualquier problema de rendimiento lo antes posible.

Archivos de Profiler pueden eliminarse cuando se usa WebDeploy para implementar los cambios en la aplicación web. Puede evitar la eliminación si se excluye de la carpeta App_Data que se eliminen durante la implementación. 


## <a name="next-steps"></a>Pasos siguientes

* [Trabajo con Application Insights en Visual Studio](https://docs.microsoft.com/azure/application-insights/app-insights-visual-studio)

[Enablement UI]: ./media/profiler/Enablement_UI.png
[profiler-app-setting]:./media/profiler/profiler-app-setting.png
[disable-profiler-webjob]: ./media/profiler/disable-profiler-webjob.png
