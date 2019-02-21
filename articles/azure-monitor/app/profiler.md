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
ms.openlocfilehash: f3ec10a970406cbb1bb6a1a52ffa8508e37fc516
ms.sourcegitcommit: 79038221c1d2172c0677e25a1e479e04f470c567
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2019
ms.locfileid: "56414174"
---
# <a name="profile-live-azure-app-service-apps-with-application-insights"></a>Generación de perfiles de aplicaciones activas de Azure App Service con Application Insights

Actualmente, el generador de perfiles puede usarse con aplicaciones ASP.NET y ASP.NET Core que se ejecutan en Azure App Service. Es necesario disponer del nivel de servicio Básico o superior para poder utilizar Profiler. Actualmente, la habilitación de Profiler en Linux solo es posible a través de [este método](profiler-aspnetcore-linux.md).

## <a id="installation"></a> Habilitación de Profiler en la aplicación
Para habilitar Profiler en una aplicación, siga las instrucciones que se indican a continuación. Si está ejecutando otro tipo de servicio de Azure, aquí encontrará instrucciones para habilitar Profiler en otras plataformas compatibles:
* [Cloud Services](../../azure-monitor/app/profiler-cloudservice.md?toc=/azure/azure-monitor/toc.json)
* [Aplicaciones de Service Fabric](../../azure-monitor/app/profiler-servicefabric.md?toc=/azure/azure-monitor/toc.json)
* [Máquinas virtuales](../../azure-monitor/app/profiler-vm.md?toc=/azure/azure-monitor/toc.json)

Application Insights Profiler se preinstaló con el entorno de ejecución de App Services, pero es necesario activarlo si quiere obtener perfiles para la aplicación de App Service. Después de implementar una aplicación web, aunque haya incluido el SDK de App Insights en el código fuente, siga los pasos que se indican a continuación para habilitar el generador de perfiles.

1. Vaya al panel **App Services** en Azure Portal.
2. Vaya al panel **Configuración > Application Insights**.

   ![Habilitación de App Insights en el portal de App Services](./media/profiler/AppInsights-AppServices.png)

3. Siga las instrucciones que aparecen en el panel para crear un nuevo recurso o seleccione un recurso de App Insights existente para supervisar la aplicación. También, asegúrese de que Profiler esté **activado**.

   ![Adición de la extensión de sitio de App Insights][Enablement UI]

4. Profiler está habilitado con una configuración de aplicación de App Services.

    ![Configuración de aplicación de Profiler][profiler-app-setting]

## <a name="disable-profiler"></a>Deshabilitación de Profiler

Para detener o reiniciar el generador de perfiles en una determina instancia de la aplicación, en **Trabajos web**, vaya al recurso de la aplicación. Para eliminar Profiler, vaya a **Extensiones**.

![Deshabilitación de Profiler para un trabajo web][disable-profiler-webjob]

Se recomienda tener habilitado el generador de perfiles en todas las aplicaciones para detectar cualquier problema de rendimiento lo antes posible.

Si usa WebDeploy para implementar cambios en la aplicación web, asegúrese de excluir la carpeta App_Data de la eliminación durante la implementación. De lo contrario, los archivos de la extensión de Profiler se eliminan la próxima vez que implemente la aplicación web en Azure.



## <a name="next-steps"></a>Pasos siguientes

* [Trabajo con Application Insights en Visual Studio](https://docs.microsoft.com/azure/application-insights/app-insights-visual-studio)

[Enablement UI]: ./media/profiler/Enablement_UI.png
[profiler-app-setting]:./media/profiler/profiler-app-setting.png
[disable-profiler-webjob]: ./media/profiler/disable-profiler-webjob.png
