---
title: Solución de problemas relacionados con Azure Application Insights Profiler | Microsoft Docs
description: Esta página contiene información y los pasos a seguir para la solución de problema para ayudar a los desarrolladores que tienen dificultades para habilitar o usar Application Insights Profiler.
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
ms.openlocfilehash: 28de0f8bdcaa730c5beea0c630d4e86e15642809
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/26/2018
ms.locfileid: "50142967"
---
# <a name="troubleshoot-problems-enabling-or-viewing-application-insights-profiler"></a>Solución de problemas de activación o visualización de Application Insights Profiler

## <a id="troubleshooting"></a>Solución general de problemas

### <a name="profiles-are-only-uploaded-if-there-are-requests-to-your-application-while-the-profiler-is-running"></a>Los perfiles se cargan únicamente si hay solicitudes a la aplicación mientras se ejecuta el generador de perfiles
Application Insights Profiler recopila los datos de generador de perfiles durante dos minutos cada hora, o cuando se presiona el botón [**Profile Now**](app-insights-profiler-settings.md?toc=/azure/azure-monitor/toc.json) (Generar perfil ahora) en la página **Configurar Application Insights Profiler**. Pero los datos de generación de perfiles solo se cargan cuando se pueden adjuntar a una solicitud que se produzca mientras se esté ejecutando el generador de perfiles. 

El generador de perfiles escribe mensajes de seguimiento y eventos personalizados en el recurso de Application Insights. Puede usar esos eventos para ver cómo se ejecuta el generador de perfiles. Si piensa que el generador de perfiles debe estar ejecutándose y capturando seguimientos, pero no los ve en la página Rendimiento, puede comprobar cómo se ejecuta el generador de perfiles:

1. Busque mensajes de seguimiento y eventos personalizados enviados por el generador de perfiles al recurso de Application Insights. Puede usar esta cadena de búsqueda para encontrar los datos pertinentes:

    ```
    stopprofiler OR startprofiler OR upload OR ServiceProfilerSample
    ```
    En la captura de pantalla siguiente se puede ver un ejemplo de dos búsquedas de dos recursos distintos de inteligencia artificial. 
    
    * El de la izquierda es de una aplicación que no está recibiendo solicitudes mientras se ejecuta el generador de perfiles. Puede ver el mensaje de que la carga se ha cancelado debido a la falta de actividad. 

    * En el ejemplo de la derecha, puede observar que el generador de perfiles se inició y envió eventos personalizados cuando detectó las solicitudes que se produjeron mientras se estaba ejecutando el generador de perfiles. Si ve el evento personalizado ServiceProfilerSample, esto significa que el generador de perfiles ha adjuntado un seguimiento a una solicitud y el seguimiento se puede consultar en la página de rendimiento de Application Insights.

    * Si no ve ningún dato de telemetría, es que el generador de perfiles no se está ejecutando. Es posible que tenga que leer las secciones de solución de problemas para su tipo de aplicación específico en el documento a continuación.  

     ![Buscar telemetría de Profiler][profiler-search-telemetry]

1. Si hay solicitudes durante el período de tiempo en el que se ejecutó el generador de perfiles, asegúrese de que la parte de la aplicación que tiene habilitado el generador de perfiles es la que controla las solicitudes. A veces, las aplicaciones constan de varios componentes pero Profiler está habilitado solo para algunos de los componentes, no para todos. La página Configurar Application Insights Profiler muestra los componentes que han cargado seguimientos.

### <a name="net-core-21-bug"></a>**Error .Net Core 2.1** 
Hay un error en el agente del generador de perfiles que le impide cargar los seguimientos realizados desde las aplicaciones que se ejecutan en ASP.NET Core 2.1. Estamos trabajando en una solución y estará preparada en breve. La corrección para este error se implementará a finales de octubre.

### <a name="other-things-to-check"></a>**Otras cosas que hay que comprobar:**
* Que la aplicación se ejecuta en .Net Framework 4.6.
* Si la aplicación web es una aplicación de ASP.NET Core, debe ejecutar al menos ASP.NET Core 2.0.
* Si la antigüedad de los datos que intenta ver supera el par de semanas, procure limitar el filtro de tiempo y inténtelo de nuevo. Los seguimientos se eliminan pasados siete días.
* Asegúrese de que no haya servidores proxy ni un firewall que bloqueen el acceso a https://gateway.azureserviceprofiler.net.

### <a id="double-counting"></a>**Doble recuento en subprocesos paralelos**

En algunos casos, la métrica de tiempo total del visor de la pila es mayor que la duración de la solicitud.

Esta situación puede suceder cuando hay dos o más subprocesos que funcionan en paralelo asociados a una solicitud. En ese caso, el tiempo de subproceso total es superior al tiempo transcurrido. Es posible que uno de los subprocesos esté a la espera de que el otro finalice. El visor intenta detectar esta situación y omite la espera carente de interés, pero prefiere mostrar demasiada información antes que omitir lo que podría ser información crítica.

Si ve subprocesos en paralelo en sus seguimientos, determine cuáles son los subprocesos que esperan para poder establecer la ruta crítica de la solicitud. En la mayoría de los casos, el subproceso que pasa rápidamente a un estado de espera tan solo espera a los demás subprocesos. Concéntrese en los demás subprocesos e ignore el tiempo de los subprocesos en espera.

### <a name="error-report-in-the-profiling-viewer"></a>**Informe de errores del visor de generación de perfiles**
Envíe una incidencia de soporte técnico desde el portal. Asegúrese de incluir el identificador de correlación del mensaje de error.

## <a name="troubleshooting-profiler-on-app-services"></a>Solución de problemas de Profiler en App Services
### <a name="for-the-profiler-to-work-properly"></a>**Para que el generador de perfiles funcione correctamente:**
* El plan de servicio de aplicación web tiene que ser de nivel Básico o superior.
* La aplicación web tiene que tener instalada la extensión de Application Insights para App Services (2.6.5).
* La aplicación web tiene que tener el valor **APPINSIGHTS_INSTRUMENTATIONKEY** configurado con la misma clave de instrumentación que usa el SDK de Application Insights.
* La aplicación web tiene que tener el valor de aplicación **APPINSIGHTS_PROFILERFEATURE_VERSION** definido y configurado en 1.0.0.
* El trabajo web **ApplicationInsightsProfiler2** tiene que estar ejecutándose. Para comprobar el trabajo web acuda a [Kudu](https://blogs.msdn.microsoft.com/cdndevs/2015/04/01/the-kudu-debug-console-azure-websites-best-kept-secret/)y abra el **panel de WebJobs** en el menú Herramientas. Como puede observar en las capturas de pantalla a continuación, al hacer clic en el vínculo ApplicationInsightsProfiler2, verá los detalles del trabajo web, incluido el registro.

    Este es el vínculo en el que tiene que hacer clic para ver los detalles del trabajo web: 

    ![profiler-webjob]

    Esta es la página que muestra los detalles. Si no puede averiguar por qué el generador de perfiles no le funciona, puede descargar el registro y enviarlo a nuestro equipo.
    
    ![profiler-webjob-log]

### <a name="manual-installation"></a>**Instalación manual**

Cuando se configura Profiler, se realizan las siguientes actualizaciones en la configuración de la aplicación web. Estas actualizaciones se pueden aplicar manualmente si su entorno lo requiere. Un ejemplo podría ser la ejecución de su aplicación en un entorno de Web Apps para PowerApps.

1. En el panel de **control de la aplicación web**, abra **Configuración**.
1. Establezca **Versión de .Net Framework** en **v4.6**.
1. Establezca **Siempre activado** en **Activado**.
1. Agregue el valor de configuración de la aplicación **APPINSIGHTS_INSTRUMENTATIONKEY** y establezca el valor en la misma clave de instrumentación que usó el SDK.
1. Agregue el valor de aplicación **APPINSIGHTS_PROFILERFEATURE_VERSION** y establezca el valor en 1.0.0.
1. Abra **Herramientas avanzadas**.
1. Seleccione **Ir** para abrir el sitio web de Kudu.
1. En el sitio web de Kudu, seleccione **Site extensions** (Extensiones de sitio).
1. Instale **Application Insights** desde la galería de aplicaciones web de Azure.
1. Reinicie la aplicación web.

### <a name="too-many-active-profiling-sessions"></a>**Hay demasiadas sesiones de generación de perfiles activas**

Actualmente, puede habilitar Profiler en hasta cuatro aplicaciones web de Azure y ranuras de implementación que se ejecuten en el mismo plan de servicio. Si el trabajo web de Profiler notifica demasiadas sesiones activas de generación de perfiles, mueva algunas aplicaciones web a un plan de servicio diferente.

### <a name="deployment-error-directory-not-empty-dhomesitewwwrootappdatajobs"></a>Deployment error: Directory Not Empty (Error de implementación: el directorio no está vacío) "D:\\home\\site\\wwwroot\\App_Data\\jobs"

Si va a volver a implementar su aplicación web en un recurso de Web Apps con Profiler habilitado, puede que aparezca un mensaje similar al siguiente:

*El directorio no está vacío "D:\\home\\site\\wwwroot\\App_Data\\jobs"*

Este error se produce si Web Deploy se ejecuta desde scripts o desde la canalización de implementación de Azure DevOps. La solución consiste en agregar los siguientes parámetros de implementación adicionales a la tarea de Web Deploy:

```
-skip:Directory='.*\\App_Data\\jobs\\continuous\\ApplicationInsightsProfiler.*' -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data\\jobs\\continuous$' -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data\\jobs$'  -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data$'
```

Estos parámetros eliminan la carpeta que usa Application Insights Profiler y desbloquea el proceso de reeimplementación. No afectan a la instancia de Profiler actualmente en ejecución.

### <a name="how-do-i-determine-whether-application-insights-profiler-is-running"></a>¿Cómo se puede determinar si Application Insights Profiler se está ejecutando?

Profiler se ejecuta como un trabajo web continuo de la aplicación web. Puede abrir el recurso de aplicación web en [Azure Portal](https://portal.azure.com). En el panel **WebJobs**, compruebe el estado de **ApplicationInsightsProfiler**. Si no se está ejecutando, abra **Registros** para obtener más información.

## <a name="troubleshooting-problems-with-profiler-and-wad"></a>Solucionar problemas de Profiler y WAD

Hay tres cosas que hay que comprobar para ver si el generador de perfiles está configurado correctamente por WAD. En primer lugar, tiene que comprobar que los contenidos de la configuración de WAD que están implementados son los esperados. En segundo lugar, tiene que comprobar que WAD pasa la clave de instrumentación (iKey) adecuada en la línea de comandos del generador de perfiles. En tercer lugar, puede comprobar el archivo de registro del generador de perfiles para ver si el generador de perfiles se ejecutó pero hay un error. 

Para comprobar los valores que se usaron para configurar WAD, tiene que iniciar sesión en la máquina virtual y abrir el archivo de registro en esta ubicación: 
```
c:\logs\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\1.11.3.12\DiagnosticsPlugin.logs  
```
En ese archivo, busque la cadena "WadCfg" y encontrará los valores que se pasaron a la máquina virtual para configurar WAD. Compruebe que el valor de iKey utilizado por el receptor del generador de perfiles es correcto.

En segundo lugar, compruebe la línea de comandos que se usa para iniciar el generador de perfiles. El siguiente archivo contiene los argumentos utilizados para iniciar el generador de perfiles.
```
D:\ProgramData\ApplicationInsightsProfiler\config.json
```
Compruebe que el valor de ikey en la línea de comandos del generador de perfiles es correcto. 

En tercer lugar, use la ruta de acceso que se encuentra en el archivo config.json anterior, compruebe el archivo de registro del generador de perfiles. Se mostrará información de depuración que indica los valores que está usando el generador de perfiles y los mensajes de error y de estado del generador de perfiles. Si el generador de perfiles se está ejecutando mientras la aplicación está recibiendo solicitudes, verá este mensaje: Activity detected from iKey (Se ha detectado actividad en la clave de instrumentación). Cuando se está cargando el seguimiento, verá este mensaje: Start to upload trace (Inicio de carga del seguimiento). 


[profiler-search-telemetry]:./media/app-insights-profiler/Profiler-Search-Telemetry.png
[profiler-webjob]:./media/app-insights-profiler/Profiler-webjob.png
[profiler-webjob-log]:./media/app-insights-profiler/Profiler-webjob-log.png








