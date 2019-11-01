---
title: Habilitación de Snapshot Debugger para aplicaciones de .NET en Azure App Service | Microsoft Docs
description: Habilitación de Snapshot Debugger para aplicaciones de .NET en Azure App Service
ms.service: azure-monitor
ms.subservice: application-insights
ms.topic: conceptual
author: brahmnes
ms.author: bfung
ms.date: 03/07/2019
ms.reviewer: mbullwin
ms.openlocfilehash: 0f6eb6376075337edd7656e4bc83b5b7fddde479
ms.sourcegitcommit: 5acd8f33a5adce3f5ded20dff2a7a48a07be8672
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2019
ms.locfileid: "72899886"
---
# <a name="enable-snapshot-debugger-for-net-apps-in-azure-app-service"></a>Habilitación de Snapshot Debugger para aplicaciones de .NET en Azure App Service

Actualmente, Snapshot Debugger puede usarse con aplicaciones ASP.NET y ASP.NET Core que se ejecutan en Azure App Service en planes de servicio de Windows.

## <a id="installation"></a> Habilitación de Snapshot Debugger
Para habilitar Snapshot Debugger en una aplicación, siga las instrucciones que se indican a continuación. Si está ejecutando otro tipo de servicio de Azure, aquí encontrará instrucciones para habilitar Snapshot Debugger en otras plataformas compatibles:
* [Azure Cloud Services](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)
* [Servicios de Azure Service Fabric](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)
* [Azure Virtual Machines y Virtual Machine Scale Sets](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)
* [Máquinas virtuales o físicas locales](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)

Si usa una versión preliminar de .NET Core, siga las instrucciones para [Habilite Snapshot Debugger para aplicaciones .NET en Azure Service Fabric, servicio en la nube y máquinas virtuales](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json) en primer lugar para incluir el paquete NuGet [Microsoft.ApplicationInsights.SnapshotCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) con la aplicación y, luego, complete el resto de las instrucciones siguientes. 

Snapshot Debugger de Application Insights se preinstaló con el entorno de ejecución de App Services, pero es necesario activarlo si quiere obtener instantáneas para la aplicación de App Service. Después de implementar una aplicación web, aunque haya incluido el SDK de Application Insights en el código fuente, siga los pasos que se indican a continuación para habilitar el depurador de instantáneas.

1. Vaya al panel **App Services** en Azure Portal.
2. Vaya al panel **Configuración > Application Insights**.

   ![Habilitación de App Insights en el portal de App Services](./media/snapshot-debugger/applicationinsights-appservices.png)

3. Siga las instrucciones que aparecen en el panel para crear un nuevo recurso o seleccione un recurso de App Insights existente para supervisar la aplicación. También asegúrese de que los dos modificadores de Snapshot Debugger están **activados**.

   ![Adición de la extensión de sitio de App Insights][Enablement UI]

4. Snapshot Debugger está habilitado con una configuración de aplicación de App Services.

    ![Configuración de aplicación para Snapshot Debugger][snapshot-debugger-app-setting]

## <a name="disable-snapshot-debugger"></a>Deshabilitación de Snapshot Debugger

Siga los mismos pasos que para **Habilitación de Snapshot Debugger**, pero cambie los dos modificadores de Snapshot Debugger al modo **desactivado**.
Recomendamos que tenga habilitado Snapshot Debugger en todas las aplicaciones para facilitar el diagnóstico de las excepciones de la aplicación.

## <a name="next-steps"></a>Pasos siguientes

- Genere tráfico para la aplicación que pueda desencadenar una excepción. A continuación, espere de 10 a 15 minutos para que se empiecen a enviar instantáneas a la instancia de Application Insights.
- Vea las [instantáneas](snapshot-debugger.md?toc=/azure/azure-monitor/toc.json#view-snapshots-in-the-portal) en Azure Portal.
- Por ayuda para solucionar problemas de Snapshot Debugger, consulte la sección [Solución de problemas de Snapshot Debugger](snapshot-debugger-troubleshoot.md?toc=/azure/azure-monitor/toc.json).

[Enablement UI]: ./media/snapshot-debugger/enablement-ui.png
[snapshot-debugger-app-setting]:./media/snapshot-debugger/snapshot-debugger-app-setting.png

