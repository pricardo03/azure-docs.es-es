---
title: Uso del panel de configuración de Azure Application Insights Profiler | Microsoft Docs
description: Consulte el estado de Profiler e inicie sesiones de generación de perfiles
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
ms.openlocfilehash: 9603c45443c6339a127f977600eeff2ba57a283f
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2019
ms.locfileid: "58884166"
---
# <a name="configure-application-insights-profiler"></a>Configuración de Application Insights Profiler

## <a name="profiler-settings-pane"></a>Panel de configuración de Profiler

Para abrir el panel de configuración de Azure Application Insights Profiler, vaya al panel de rendimiento de Application Insights y seleccione el botón **Profiler**.

![Configurar el panel de Profiler][configure-profiler-entry]

El panel de **configuración de Application Insights Profiler** contiene cuatro características: 
* **Generar perfiles ahora**: inicia sesiones de generación en todas las aplicaciones vinculadas a esta instancia de Application Insights.
* **Aplicaciones vinculadas**: enumera las aplicaciones que envían datos de generación de perfiles a este recurso de Application Insights.
* **Sesiones en curso**: muestra el estado de la sesión cuando se selecciona **Generar perfiles ahora**. 
* **Sesiones de generación de perfiles recientes**: muestra información acerca de las anteriores sesiones de generación de perfiles.

![Profiler a petición][profiler-on-demand]

## <a name="app-service-environment"></a>Entorno de App Service
En función de cómo esté configurado Azure App Service Environment, se puede bloquear la llamada para comprobar el estado del agente. El panel puede mostrar un mensaje que indique que no se ejecuta el agente, aunque se esté ejecutando. Para asegurarse de que es así, compruebe el WebJob en la aplicación. Si todos los valores de la configuración de aplicación son correctos y se instala la extensión de sitio de Application Insights en la aplicación, Profiler está en ejecución. Si la aplicación recibe suficiente tráfico, las últimas sesiones de generación de perfiles deberían mostrarse en una lista.

## <a id="profileondemand"></a> Desencadenamiento manual de Profiler

### <a name="minimum-requirements"></a>Requisitos mínimos 
Para que un usuario desencadenar manualmente una sesión del generador de perfiles requieren como mínimo "acceso de escritura" en su rol para el componente de Application Insights. En la mayoría de los casos para obtener este acceso automáticamente y no es necesario realizar ningún trabajo adicional. Si tiene problemas, el rol de ámbito de suscripción para agregar sería el rol "Colaborador de componente de Application Insights". [Obtenga más información sobre el control de acceso de rol con Azure Monitoring](https://docs.microsoft.com/en-us/azure/azure-monitor/app/resources-roles-access-control).

Profiler se puede desencadenar manualmente con un solo clic. Imagine que está ejecutando una prueba de rendimiento web. Necesitará seguimientos que le ayuden a saber si la aplicación web funciona correctamente en condiciones de carga. Poder control el momento en que se capturan los seguimientos es fundamental, ya que sabe cuándo se va a ejecutar la prueba de carga. Sin embargo, con el intervalo de muestreo aleatorio es posible que se lo pierda.

En las secciones siguientes se ilustra cómo funciona este escenario:

### <a name="step-1-optional-generate-traffic-to-your-web-app-by-starting-a-web-performance-test"></a>Paso 1: (Opcional) Generación de tráfico hacia la aplicación web mediante el inicio de una prueba de rendimiento web

Si la aplicación web tiene ya tráfico entrante o si simplemente desea generar tráfico manualmente, omita esta sección y continúe con el paso 2.

1. En el portal de Application Insights, seleccione **Configure** > **Performance Testing** (Configurar > Prueba de rendimiento). 

1. Para iniciar una nueva prueba de rendimiento, seleccione el botón **New** (Nuevo).

   ![creación de una prueba de rendimiento][create-performance-test]

1. En el panel **New performance test** (Nueva prueba de rendimiento), configure la dirección URL de destino de la prueba. Acepte la configuración predeterminada y seleccione **Run test** (Ejecutar prueba) para empezar a ejecutar la prueba de carga.

    ![Configuración de la prueba de carga][configure-performance-test]

    En primer lugar, la nueva prueba se pone en cola y,después, pasa al estado *en curso*.

    ![La prueba de carga se ha enviado y está en cola][load-test-queued]

    ![Se está ejecutando la prueba de carga y está en curso][load-test-in-progress]

### <a name="step-2-start-a-profiler-on-demand-session"></a>Paso 2: Inicio de una sesión a petición de Profiler

1. Cuando la prueba de carga esté en ejecución, inicie Profiler para capturar los seguimientos de la aplicación web mientras se recibe la carga.

1. Vaya al panel de **configuración de Profiler**.


### <a name="step-3-view-traces"></a>Paso 3: Visualización de seguimientos

Cuando Profiler deje de ejecutarse, siga las instrucciones de la notificación para ir al panel de rendimiento y ver los seguimientos.

## <a name="troubleshoot-the-profiler-on-demand-session"></a>Solución de problemas de la sesión a petición de Profiler

Después de una sesión a petición es posible que reciba un mensaje de error de tiempo de expiración de Profiler:

![Error de tiempo de expiración de Profiler][profiler-timeout]

Dicho error puede aparecer por cualquiera de los siguientes motivos:

* La sesión de Profiler a petición se realizó correctamente, pero Application Insights tardó más tiempo de lo esperado en procesar los datos recopilados.  

  Si los datos no se procesan en 15 minutos, el portal muestra un mensaje de tiempo de expiración. Sin embargo, al cabo de un rato, se mostrarán los seguimientos de Profiler. Si recibe un mensaje de error, ignórelo. Estamos trabajando para corregirlo.

* La aplicación web tiene una versión anterior del agente de Profiler que no tiene la característica a petición.  

  Si Application Insights Profiler se ha habilitado con anterioridad, es probable que haya que actualizar el agente de Profiler para que empiece a usar dicha característica.
  
Vaya al panel **Configuración de la aplicación** de App Services y compruebe los siguientes valores:
* **APPINSIGHTS_INSTRUMENTATIONKEY**: reemplace este valor por la clave de instrumentación correcta de Application Insights.
* **APPINSIGHTS_PORTALINFO**: ASP.NET
* **APPINSIGHTS_PROFILERFEATURE_VERSION**: 1.0.0

Si alguno de los valores anteriores no se ha establecido, instale la extensión de sitio más reciente, para lo que debe seguir estos pasos:

1. Vaya al panel **Application Insights** en el portal de App Services.

    ![Habilitar Application Insights desde el portal de App Services][enable-app-insights]

1. Si el panel **Application Insights** tiene un botón **Update** (Actualizar), selecciónelo para actualizar la extensión del sitio de Application Insights que instalará al agente de Profiler más reciente.

    ![Actualizar extensión de sitio][update-site-extension]

1. Para asegurarse de que Profiler esté activado, seleccione **Change** (Cambiar) y, después, seleccione **Aceptar** para guardar los cambios.

    ![Cambiar y guardar información detallada de aplicación][change-and-save-appinsights]

1. Vuelva al panel **Configuración de la aplicación** de App Services para asegurarse de que se han establecido los valores siguientes:
   * **APPINSIGHTS_INSTRUMENTATIONKEY**: reemplace este valor por la clave de instrumentación correcta de Application Insights.
   * **APPINSIGHTS_PORTALINFO**: ASP.NET 
   * **APPINSIGHTS_PROFILERFEATURE_VERSION**: 1.0.0

     ![Configuración de la aplicación para Profiler][app-settings-for-profiler]

1. Si lo desea, seleccione **Extensiones** y, después, compruebe la versión de extensión y determine si hay alguna actualización disponible.

    ![Comprobación de la actualización de la extensión][check-for-extension-update]

## <a name="next-steps"></a>Pasos siguientes
[Habilitar a Profiler y ver los seguimientos](profiler-overview.md?toc=/azure/azure-monitor/toc.json)

[profiler-on-demand]: ./media/profiler-settings/Profiler-on-demand.png
[configure-profiler-entry]: ./media/profiler-settings/configure-profiler-entry.png
[create-performance-test]: ./media/profiler-settings/new-performance-test.png
[configure-performance-test]: ./media/profiler-settings/configure-performance-test.png
[load-test-queued]: ./media/profiler-settings/load-test-queued.png
[load-test-in-progress]: ./media/profiler-settings/load-test-inprogress.png
[enable-app-insights]: ./media/profiler-settings/enable-app-insights-blade-01.png
[update-site-extension]: ./media/profiler-settings/update-site-extension-01.png
[change-and-save-appinsights]: ./media/profiler-settings/change-and-save-appinsights-01.png
[app-settings-for-profiler]: ./media/profiler-settings/appsettings-for-profiler-01.png
[check-for-extension-update]: ./media/profiler-settings/check-extension-update-01.png
[profiler-timeout]: ./media/profiler-settings/profiler-timeout.png
