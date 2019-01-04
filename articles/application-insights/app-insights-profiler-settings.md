---
title: Hoja de configuración de Azure Application Insights Profiler | Microsoft Docs
description: Ver estado del generador de perfiles e iniciar sesiones de generación de perfiles
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.reviewer: cawa
ms.date: 08/06/2018
ms.author: mbullwin
ms.openlocfilehash: d96b4a99b2ea66cdeff43f06c1789dd133c2c31a
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2018
ms.locfileid: "52723013"
---
# <a name="configure-application-insights-profiler"></a>Configuración de Application Insights Profiler

## <a name="profiler-settings-page"></a>Página de configuración de Profiler

La página de configuración del generador de perfiles se puede abrir desde la página Rendimiento de Application Insights presionando el botón **Profiler**.

![Entrada al panel de configuración de Profiler][configure-profiler-entry]

La página Configurar Application Insights Profiler contiene cuatro características: 
1. **Profile Now** (Generar perfil ahora): al hacer clic en este botón se inician las sesiones de generación de perfiles para todas las aplicaciones que están vinculadas a esta instancia de Application Insights
1. **Aplicaciones vinculadas**: lista de las aplicaciones que envían el generador de perfiles a este recurso de Application Insights
1. **Sessions in Progress** (Sesiones en curso): cuando se presiona **Profile Now** (Generar perfil ahora), el estado de la sesión se mostrará aquí
1. **Sesiones de generación de perfiles recientes**: muestra información sobre las sesiones de generación de perfiles anteriores.

![Profiler a petición][profiler-on-demand]

## <a name="app-service-environments-ase"></a>App Service Environment (ASE)
Según cómo esté configurada la instancia de ASE, se puede bloquear la llamada para comprobar el estado del agente. En esta página se indicará que el agente no se está ejecutando cuando en realidad sí que está. Puede comprobar el trabajo web en la aplicación para asegurarse. Pero si todos los valores de configuración de aplicación están establecidos correctamente y la extensión de sitio de App Insights está instalada en su aplicación, el generador de perfiles se ejecutará y, si hay un tráfico adecuado a la aplicación, debería ver las sesiones de generación de perfiles recientes en la lista.

## <a id="profileondemand"></a> Desencadenamiento manual de Profiler

Profiler se puede desencadenar manualmente con el clic de un botón. Supongamos que ejecuta una prueba de rendimiento web. Necesitará seguimientos que le ayuden a comprender cómo se ejecuta la aplicación web bajo condiciones de carga. Es fundamental tener control sobre cuándo se capturan los seguimientos, puesto que el usuario sabe cuándo se ejecutará la prueba de carga, pero el intervalo de muestreo aleatorio podría no saberlo.
Los pasos siguientes ilustran cómo funciona este escenario:

### <a name="optional-step-1-generate-traffic-to-your-web-app-by-starting-a-web-performance-test"></a>(Opcional) Paso 1: Generación de tráfico a la aplicación web mediante el inicio de una prueba de rendimiento web

Si la aplicación web tiene ya tráfico entrante o si simplemente desea generar tráfico manualmente, omita esta sección y continúe con el paso 2.

Vaya al portal de Application Insights, **Configure > Performance Testing** (Configurar > Prueba de rendimiento). Haga clic en el botón New (Nuevo) para iniciar una nueva prueba de rendimiento.

![creación de una prueba de rendimiento][create-performance-test]

En el panel **New performance test** (Nueva prueba de rendimiento), configure la dirección URL de destino de la prueba. Acepte toda la configuración predeterminada y empiece a ejecutar la prueba de carga.

![Configuración de la prueba de carga][configure-performance-test]

Verá que la nueva prueba se pone primero en cola, seguida de un estado de "en curso".

![La prueba de carga se ha enviado y puesto en cola][load-test-queued]

![Se está ejecutando la prueba de carga y está en curso][load-test-in-progress]

### <a name="step-2-start-profiler-on-demand"></a>Paso 2: Inicio de Profiler a petición

Una vez que se está ejecutando la prueba de carga, se puede iniciar Profiler para capturar los seguimientos de la aplicación web mientras se recibe la carga.
Vaya al panel de configuración de Profiler:


### <a name="step-3-view-traces"></a>Paso 3: Visualización de seguimientos

Una vez que termine de ejecutarse el generador de perfiles, siga las instrucciones de la notificación para ir a la página Rendimiento y ver los seguimientos.

## <a name="troubleshooting-on-demand-profiler"></a>Solución de problemas de Profiler a petición

En ocasiones pude que vea un mensaje de error de tiempo de expiración agotado de Profiler tras una sesión a petición:

![Error de tiempo de expiración de Profiler][profiler-timeout]

Las causas de este error pueden ser dos:

1. La sesión de Profiler a petición se realizó con éxito, pero Application Insights tardó más tiempo en procesar los datos recopilados. Si los datos no han terminado de procesarse en 15 minutos, el portal mostrará un mensaje de tiempo de expiración. Aunque al cabo de un rato, se mostrarán los seguimientos de Profiler. Si esto sucede, ignore el mensaje de error por ahora. Estamos trabajando para corregirlo.

1. La aplicación web tiene una versión anterior del agente de Profiler que no tiene la característica a petición. Si ha habilitado anteriormente el perfil de Application Insights, lo más probable es que tenga que actualizar el agente de Profiler para empezar a usar la funcionalidad a petición.
  
Siga estos pasos para comprobar e instalar la última versión de Profiler:

1. Vaya a la configuración de aplicación de App Services y compruebe si los siguientes valores están establecidos:
    * **APPINSIGHTS_INSTRUMENTATIONKEY**: reemplace este valor por la clave de instrumentación correcta de Application Insights.
    * **APPINSIGHTS_PORTALINFO**: ASP.NET
    * **APPINSIGHTS_PROFILERFEATURE_VERSION**: 1.0.0. Si alguno de estos valores de configuración no está establecido, vaya al panel de habilitación de Application Insights para instalar la extensión de sitio más reciente.

1. Vaya al panel de Application Insights en el portal de App Services.

    ![Habilitación de Application Insights desde el portal de App Services][enable-app-insights]

1. Si ve un botón para actualizar en la siguiente página, haga clic en él para actualizar la extensión de sitio de Application Insights lo que instalará al agente de Profiler más reciente.

    ![Actualizar extensión de sitio][update-site-extension]

1. A continuación, haga clic en **Change** (Cambiar) para asegurarse de que Profiler está activado y seleccione **OK** (Aceptar) para guardar los cambios.

    ![Cambiar y guardar información detallada de aplicación][change-and-save-appinsights]

1. Vuelva a la pestaña **App Settings** (Configuración de la aplicación) de App Service para asegurarse de que los elementos de configuración de la aplicación están establecidos:
    * **APPINSIGHTS_INSTRUMENTATIONKEY**: reemplace este valor por la clave de instrumentación correcta de Application Insights.
    * **APPINSIGHTS_PORTALINFO**: ASP.NET
    * **APPINSIGHTS_PROFILERFEATURE_VERSION**: 1.0.0

    ![Configuración de la aplicación para Profiler][app-settings-for-profiler]

1. Opcionalmente, compruebe la versión de la extensión y asegúrese que no haya ninguna actualización disponible.

    ![Comprobación de actualizaciones de la extensión][check-for-extension-update]

## <a name="next-steps"></a>Pasos siguientes
[Habilitar Profiler y ver seguimientos](app-insights-profiler-overview.md?toc=/azure/azure-monitor/toc.json)

[profiler-on-demand]: ./media/app-insights-profiler/Profiler-on-demand.png
[configure-profiler-entry]: ./media/app-insights-profiler/configure-profiler-entry.png
[create-performance-test]: ./media/app-insights-profiler/new-performance-test.png
[configure-performance-test]: ./media/app-insights-profiler/configure-performance-test.png
[load-test-queued]: ./media/app-insights-profiler/load-test-queued.png
[load-test-in-progress]: ./media/app-insights-profiler/load-test-inprogress.png
[enable-app-insights]: ./media/app-insights-profiler/enable-app-insights-blade-01.png
[update-site-extension]: ./media/app-insights-profiler/update-site-extension-01.png
[change-and-save-appinsights]: ./media/app-insights-profiler/change-and-save-appinsights-01.png
[app-settings-for-profiler]: ./media/app-insights-profiler/appsettings-for-profiler-01.png
[check-for-extension-update]: ./media/app-insights-profiler/check-extension-update-01.png
[profiler-timeout]: ./media/app-insights-profiler/profiler-timeout.png