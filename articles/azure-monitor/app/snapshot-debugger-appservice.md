---
title: Habilitar el depurador de instantáneas para aplicaciones .NET en Azure App Service | Microsoft Docs
description: Habilitar el depurador de instantáneas para aplicaciones .NET en Azure App Service
services: application-insights
documentationcenter: ''
author: brahmnes
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.reviewer: mbullwin
ms.date: 03/07/2019
ms.author: brahmnes
ms.openlocfilehash: 7ff93dffe2dd82bdbba204b4235a297b337438f5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60784101"
---
# <a name="enable-snapshot-debugger-for-net-apps-in-azure-app-service"></a>Habilitar el depurador de instantáneas para aplicaciones .NET en Azure App Service

Depurador de instantáneas actualmente funciona para aplicaciones ASP.NET y ASP.NET Core que se ejecutan en Azure App Service en los planes de servicio de Windows.

## <a id="installation"></a> Habilitar el depurador de instantáneas
Para habilitar el depurador de instantáneas para una aplicación, siga las instrucciones siguientes. Si está ejecutando otro tipo de servicio de Azure, aquí encontrará instrucciones para habilitar el depurador de instantáneas en otras plataformas compatibles:
* [Azure Cloud Services](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)
* [Servicios de Azure Service Fabric](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)
* [Azure Virtual Machines y Virtual Machine Scale Sets](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)
* [Máquinas virtuales o físicas locales](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)

Application Insights Snapshot Debugger se instalaron previamente como parte del tiempo de ejecución de servicios de aplicaciones, pero necesita para activarla y obtener instantáneas de la aplicación de App Service. Una vez que ha implementado una aplicación, incluso si ha incluido el SDK de Application Insights en el código fuente, siga estos pasos para habilitar al depurador de instantáneas.

1. Vaya al panel **App Services** en Azure Portal.
2. Vaya al panel **Configuración > Application Insights**.

   ![Habilitación de App Insights en el portal de App Services](./media/snapshot-debugger/applicationinsights-appservices.png)

3. Siga las instrucciones que aparecen en el panel para crear un nuevo recurso o seleccione un recurso de App Insights existente para supervisar la aplicación. También asegúrese de que ambos modificadores Snapshot Debugger **en**.

   ![Adición de la extensión de sitio de App Insights][Enablement UI]

4. Depurador de instantáneas está habilitado con una configuración de aplicación de servicios de aplicación.

    ![Configuración de aplicación para el depurador de instantáneas][snapshot-debugger-app-setting]

## <a name="disable-snapshot-debugger"></a>Deshabilitar el depurador de instantáneas

Siga los mismos pasos que para **habilitar Snapshot Debugger**, pero cambie ambos modificadores Snapshot Debugger para **desactivar**.
Recomendamos que tenga habilitado en todas las aplicaciones para facilitar el diagnóstico de excepciones de la aplicación de Snapshot Debugger.

## <a name="next-steps"></a>Pasos siguientes

* [Trabajo con Application Insights en Visual Studio](https://docs.microsoft.com/azure/application-insights/app-insights-visual-studio)

[Enablement UI]: ./media/snapshot-debugger/enablement-ui.png
[snapshot-debugger-app-setting]:./media/snapshot-debugger/snapshot-debugger-app-setting.png

