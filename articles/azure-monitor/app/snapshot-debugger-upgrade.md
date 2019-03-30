---
title: Actualización de Azure Application Insights Snapshot Debugger para aplicaciones de .NET | Microsoft Docs
description: Cómo actualizar a Snapshot Debugger a la versión más reciente en Azure App Services o a través de paquetes de Nuget
services: application-insights
author: MarioHewardt
manager: carmonm
ms.service: application-insights
ms.topic: conceptual
ms.date: 03/28/2019
ms.author: Mario.Hewardt
ms.reviewer: mbullwin
ms.openlocfilehash: 54b79897ee378cda52106fe704e25c50a4325f38
ms.sourcegitcommit: 956749f17569a55bcafba95aef9abcbb345eb929
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2019
ms.locfileid: "58632630"
---
# <a name="upgrading-the-snapshot-debugger"></a>Actualizar Snapshot Debugger

Para proporcionar la máxima seguridad posible para los datos, Microsoft está avanzando lejos de TLS 1.0 y TLS 1.1, que se ha demostrado ser vulnerable a los atacantes determinados. Si utiliza una versión anterior de la extensión de sitio, requerirán una actualización para seguir trabajando. Este documento describen los pasos necesarios para actualizar al depurador de instantáneas a la versión más reciente. Hay dos rutas de actualización principales en función de si ha habilitado al depurador de instantáneas con una extensión de sitio o si usa un SDK/Nuget agregado a la aplicación. Ambas rutas de actualización se describen a continuación. 

## <a name="upgrading-the-site-extension"></a>Actualización de la extensión de sitio

Si ha habilitado al depurador de instantáneas mediante la extensión de sitio, puede actualizar fácilmente mediante el procedimiento siguiente:

1. Inicie sesión en el Portal de Azure.
2. Vaya al recurso que tiene habilitado el depurador de instantáneas y de Application Insights. Por ejemplo, para una aplicación Web, navegue hasta el recurso de App Service:

   ![Captura de pantalla de recurso individual de App Service denominado DiagService01](./media/snapshot-debugger-upgrade/app-service-resource.png)

3. Una vez que ha navegado al recurso, haga clic en Application Insights en la hoja de información general:

   ![Captura de pantalla de tres botones. Se selecciona el botón central con nombre Application Insights](./media/snapshot-debugger-upgrade/application-insights-button.png)

4. Se abrirá una hoja nueva con la configuración actual. A menos que desee aprovechar la oportunidad de cambiar la configuración, puede dejarlos tal cual. El **aplicar** botón en la parte inferior de la hoja no está habilitado de forma predeterminada y tendrá que activar o desactivar una de las opciones para activar el botón. No es necesario cambiar la configuración real, en su lugar puede cambiar la configuración y, a continuación, inmediatamente revertir el cambio. Se recomienda si activa o desactiva el Profiler, configuración y, a continuación, seleccione **aplicar**.

   ![Página de captura de pantalla de Application Insights configuración de App Service con el botón Aplicar resaltado en rojo](./media/snapshot-debugger-upgrade/view-application-insights-data.png)

5. Al hacer clic en **aplicar**, se le pedirá que confirme los cambios.

    > [!NOTE]
    > El sitio se reiniciará como parte del proceso de actualización.

   ![Captura de pantalla de App Service aplicar el símbolo del sistema de supervisión. Cuadro de texto muestra el mensaje: "Ahora se aplican cambios a la configuración de la aplicación e instalarán nuestras herramientas para vincular el recurso de Application Insights a la aplicación web. Esto reiniciará el sitio. ¿Desea continuar?"](./media/snapshot-debugger-upgrade/apply-monitoring-settings.png)

6. Haga clic en **Sí** para aplicar los cambios. Durante el proceso de aparecerá una notificación que muestra que se están aplicando los cambios:

   ![Captura de pantalla de aplicar los cambios - actualizar el mensaje de las extensiones que aparece en la esquina superior derecha](./media/snapshot-debugger-upgrade/updating-extensions.png)

Una vez completado, una **"Los cambios se aplican"** aparecerá la notificación.

   ![Captura de pantalla de mensaje que indica que los cambios se aplican](./media/snapshot-debugger-upgrade/changes-are-applied.png)

El sitio se ha actualizado ahora y está listo para usarse.

## <a name="upgrading-snapshot-debugger-using-sdknuget"></a>Actualizar Snapshot Debugger mediante Nuget del SDK

Si la aplicación está usando una versión de `Microsoft.ApplicationInsights.SnapshotCollector` por debajo de la versión 1.3.1, se deberá actualizarse a una [versión más reciente](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) para seguir trabajando.
