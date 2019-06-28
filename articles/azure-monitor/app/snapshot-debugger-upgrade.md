---
title: Actualización de Snapshot Debugger de Azure Application Insights para aplicaciones de .NET | Microsoft Docs
description: Cómo actualizar a Snapshot Debugger a la versión más reciente en Azure App Services o a través de paquetes de Nuget.
services: application-insights
author: MarioHewardt
manager: carmonm
ms.service: application-insights
ms.topic: conceptual
ms.date: 03/28/2019
ms.author: Mario.Hewardt
ms.reviewer: mbullwin
ms.openlocfilehash: 54b79897ee378cda52106fe704e25c50a4325f38
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60784223"
---
# <a name="upgrading-the-snapshot-debugger"></a>Actualización de Snapshot Debugger

Para proporcionar la máxima seguridad posible para los datos, Microsoft está dejando de utilizar TLS 1.0 y TLS 1.1, que han demostrado ser vulnerables ante determinados atacantes. Si utiliza una versión anterior de la extensión de sitio, requerirá una actualización para seguir funcionando. En este documento se describen los pasos necesarios para actualizar Snapshot Debugger a la versión más reciente. Hay dos rutas de actualización principales en función de si ha habilitado Snapshot Debugger con una extensión de sitio o si usa un SDK/Nuget que se ha agregado a la aplicación. Ambas rutas de actualización se describen a continuación. 

## <a name="upgrading-the-site-extension"></a>Actualización de la extensión de sitio

Si ha habilitado Snapshot Debugger mediante la extensión de sitio, puede actualizarlo fácilmente mediante el procedimiento siguiente:

1. Inicie sesión en el Portal de Azure.
2. Vaya al recurso que tiene habilitado Snapshot Debugger y Application Insights. Por ejemplo, para una aplicación web, navegue hasta el recurso de App Service:

   ![Captura de pantalla de recurso individual de App Service denominado DiagService01](./media/snapshot-debugger-upgrade/app-service-resource.png)

3. Una vez que ha navegado al recurso, haga clic en Application Insights en la hoja de información general:

   ![Captura de pantalla de tres botones. Se selecciona el botón central con el nombre Application Insights](./media/snapshot-debugger-upgrade/application-insights-button.png)

4. Se abrirá una hoja nueva con la configuración actual. A menos que desee aprovechar la oportunidad para cambiar la configuración, puede dejarla tal cual. El botón **Aplicar** de la parte inferior de la hoja no está habilitado de forma predeterminada, y tendrá elegir una de las opciones del botón de alternancia para activar el botón. En realidad no es necesario cambiar la configuración, basta con cambiar el parámetro y, a continuación, revertir el cambio de inmediato. Recomendamos alternar el parámetro de Profiler y luego seleccionar **Aplicar**.

   ![Captura de pantalla de la página de configuración de App Service de Application Insights con el botón Aplicar resaltado en rojo](./media/snapshot-debugger-upgrade/view-application-insights-data.png)

5. Al hacer clic en **Aplicar**, se le pedirá que confirme los cambios.

    > [!NOTE]
    > El sitio se reiniciará como parte del proceso de actualización.

   ![Captura de pantalla de la solicitud de aplicar supervisión de App Service El cuadro de texto muestra el mensaje: "Ahora vamos a aplicar cambios a la configuración de la aplicación e instalaremos nuestras herramientas para vincular el recurso de Application Insights a la aplicación web. Al hacerlo, se reiniciará el sitio. ¿Desea continuar?"](./media/snapshot-debugger-upgrade/apply-monitoring-settings.png)

6. Haga clic en **Sí** para aplicar los cambios. Durante el proceso aparecerá una notificación que muestra que se están aplicando los cambios:

   ![Captura de pantalla del mensaje Aplicar cambios - Actualizando extensiones que aparece en la esquina superior derecha](./media/snapshot-debugger-upgrade/updating-extensions.png)

Una vez completado, aparecerá la notificación **"Los cambios se han aplicado"** .

   ![Captura de pantalla de mensaje que indica que los cambios se han aplicado](./media/snapshot-debugger-upgrade/changes-are-applied.png)

El sitio ya se ha actualizado y está listo para usarse.

## <a name="upgrading-snapshot-debugger-using-sdknuget"></a>Actualización de Snapshot Debugger mediante SDK/Nuget

Si la aplicación está usando una versión de `Microsoft.ApplicationInsights.SnapshotCollector` por debajo de la versión 1.3.1, se deberá actualizar a una [versión más reciente](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) para seguir funcionando.
