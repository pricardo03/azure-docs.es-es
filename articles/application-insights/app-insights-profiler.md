---
title: Generación de perfiles de aplicaciones web activas en Azure con Application Insights Profiler | Microsoft Docs
description: Identifique la ruta de acceso activa en el código del servidor web con un generador de perfiles de bajo impacto.
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
ms.openlocfilehash: 839e462522be4f492010ca1c22631cb4dd6affe4
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2018
ms.locfileid: "47064447"
---
# <a name="profile-live-azure-web-apps-with-application-insights"></a>Generación de perfiles de aplicaciones web de Azure activas con Application Insights

Esta característica de Azure Application Insights está disponible generalmente para la característica Web Apps de Azure App Service y para los recursos de proceso de Azure. Más información relacionada con el [uso local del generador de perfiles](https://docs.microsoft.com/azure/application-insights/enable-profiler-compute#enable-profiler-on-on-premises-servers).

En este artículo se habla de la cantidad de tiempo que se emplea en cada método de su aplicación web activa al usar [Application Insights](app-insights-overview.md). La herramienta Application Insights Profiler muestra perfiles detallados de solicitudes dinámicas que atendió su aplicación. Profiler resalta la *ruta de acceso activa* que más tiempo emplea. El perfil de las solicitudes con diferentes tiempos de respuesta se genera por muestreo. Gracias al uso de diversas técnicas, puede minimizar la sobrecarga asociada a la aplicación.

Actualmente, Profiler puede usarse con aplicaciones web ASP.NET y ASP.NET Core que se ejecutan en Web Apps. Es necesario disponer del nivel de servicio Básico o superior para poder utilizar Profiler.

## <a id="installation"></a> Habilitación de Profiler para su instancia de Web Apps

Una vez que ha implementado una aplicación web, con independencia de si ha incluido el SDK de App Insights en el código fuente, haga lo siguiente:

1. Vaya al panel **App Services** en Azure Portal.
1. Vaya al panel **Configuración > Supervisión**.

   ![Habilitación de App Insights en el portal de App Services](./media/app-insights-profiler/AppInsights-AppServices.png)

1. Siga las instrucciones que aparecen en el panel para crear un recurso o seleccione un recurso de App Insights existente para supervisar la aplicación web. Acepte todas las opciones predeterminadas. La opción **Diagnósticos de nivel de código** está activada de forma predeterminada y permite Profiler.

   ![Adición de la extensión de sitio de App Insights][Enablement UI]

1. Profiler se instala ahora con la extensión de sitio de App Insights y se habilita mediante un valor de configuración de aplicación de App Services.

    ![Configuración de aplicación de Profiler][profiler-app-setting]

### <a name="enable-profiler-for-azure-compute-resources"></a>Habilitación de Profiler para recursos de proceso de Azure

Para más información, consulte la [versión de Profiler para los recursos de proceso de Azure](https://go.microsoft.com/fwlink/?linkid=848155).

## <a name="view-profiler-data"></a>Visualización de datos de Profiler

Asegúrese de que la aplicación está recibiendo tráfico. Si está realizando un experimento, puede generar solicitudes dirigidas a la aplicación web mediante [Pruebas de rendimiento de Application Insights](https://docs.microsoft.com/vsts/load-test/app-service-web-app-performance-test). Si acaba de habilitar Profiler, puede ejecutar una breve prueba de carga de unos 15 minutos, lo que debería generar seguimientos del generador de perfiles. Si tiene Profiler habilitado desde hace tiempo, tenga en cuenta que se ejecuta de forma aleatoria dos veces cada hora, con una duración de dos minutos cada vez. Se recomienda ejecutar primero la prueba de carga durante una hora para garantizar que se obtienen los seguimientos de ejemplo del generador de perfiles.

Cuando llegue tráfico a la aplicación, vaya al panel **Rendimiento**, seleccione **Take Actions** (Tomar acciones) para ver los seguimientos del generador de perfiles y, a continuación, seleccione el botón **Seguimientos de Profiler**.

![Seguimientos de Profiler de vista previa del panel Rendimiento de Application Insights][performance-blade-v2-examples]

Seleccione una muestra para mostrar un desglose de nivel de código del tiempo dedicado a ejecutar la solicitud.

![Explorador de seguimiento de Application Insights][trace-explorer]

El explorador de seguimiento muestra la siguiente información:

* **Mostrar ruta de acceso activa** abre el nodo hoja más grande o al menos algo que se aproxime. En la mayoría de los casos, este nodo estará adyacente a un cuello de botella de rendimiento.
* **Etiqueta**: nombre de la función o el evento. El árbol muestra una combinación de código y eventos que se han producido (como, por ejemplo, eventos HTTP y SQL). El evento superior representa la duración total de la solicitud.
* **Transcurrido**: el intervalo de tiempo entre el inicio y el final de la operación.
* **Cuándo**: el momento en que se ejecutó la función o el evento con relación a otras funciones.

## <a name="how-to-read-performance-data"></a>Cómo leer datos de rendimiento

Profiler de servicio de Microsoft usa una combinación de método de muestreo e instrumentación para analizar el rendimiento de la aplicación. Cuando la colección detallada está en curso, el generador de perfiles del servicio muestrea cada milisegundo el puntero de instrucción de las CPU de todas las máquinas. Cada una de las muestras captura toda la pila de llamadas del subproceso que se está ejecutando actualmente. Esto proporciona información detallada sobre lo que dicho subproceso estaba haciendo, con un grado de abstracción tanto de alto como de bajo nivel. Profiler de servicio también recopila otros eventos para realizar el seguimiento de la causalidad y la correlación de actividades, como eventos de cambio de contexto, eventos de biblioteca TPL y eventos de grupo de subprocesos.

La pila de llamadas que se muestra en la vista de escala de tiempo es el resultado del muestreo y la instrumentación. Como cada muestra captura la pila de llamadas completa del subproceso, incluye código de Microsoft .NET Framework y de otros marcos a los que se haga referencia.

### <a id="jitnewobj"></a>Asignación de objetos (clr!JIT\_New o clr!JIT\_Newarr1)

**clr!JIT\_New** y **clr!JIT\_Newarr1** son funciones auxiliares de .NET Framework que asignan memoria desde un salto administrado. **clr!JIT\_New** se invoca cuando se asigna un objeto. **clr!JIT\_Newarr1** se invoca cuando se asigna una matriz de objetos. Estas dos funciones suelen ser rápidas y tardan relativamente poco tiempo. Si observa que **clr!JIT\_New** o **clr!JIT\_Newarr1** tardan más de lo normal, significa que el código podría estar asignando muchos objetos y consumiendo una importante cantidad de memoria.

### <a id="theprestub"></a>Código de carga (clr!ThePreStub)

**clr!ThePreStub** es una función auxiliar de .NET Framework que prepara el código para ejecutarse por primera vez. Suele incluir, pero sin limitarse a ello, la compilación JIT (Just-In-Time). Para cada método de C#, se debe invocar **clr!ThePreStub** al menos una vez mientras dura un proceso.

Si **clr!ThePreStub** se lleva una cantidad considerable de tiempo con una solicitud, significa que la solicitud es la primera que ejecuta ese método. El entorno de ejecución de .NET Framework tarda un tiempo considerable en cargar el primer método. Podría plantearse la posibilidad de usar un proceso de preparación que ejecute esa parte del código antes de que los usuarios accedan a él, o bien ejecutar el Generador de imágenes nativo (ngen.exe) en sus ensamblados.

### <a id="lockcontention"></a>Contención de bloqueo (clr!JITutil\_MonContention o clr!JITutil\_MonEnterWorker)

**clr!JITutil\_MonContention** o **clr!JITutil\_MonEnterWorker** indican que el subproceso actual está a la espera de que se libere un bloqueo. Este texto se muestra normalmente al ejecutar una instrucción **LOCK** de C#, al invocar el método **Monitor.Enter** o al invocar un método con el atributo **MethodImplOptions.Synchronized**. La contención de bloqueo se produce normalmente cuando un subproceso _A_ adquiere un bloqueo y un subproceso _B_ intenta adquirir el mismo bloqueo antes de que el subproceso _A_ lo libere.

### <a id="ngencold"></a>Código de carga ([COLD])

Si el nombre del método contiene **[COLD]**, por ejemplo, **mscorlib.ni![COLD]System.Reflection.CustomAttribute.IsDefined**, significa que el entorno de tiempo de ejecución de .NET Framework es la primera vez que ejecuta código que no está optimizado mediante <a href="https://msdn.microsoft.com/library/e7k32f4k.aspx">profile-guided optimization</a>. Para cada método, debe presentarse como máximo una vez durante todo el proceso.

Si el código de carga tarda una cantidad de tiempo considerable con una solicitud, significa que la solicitud es la primera en ejecutar la parte no optimizada del método. Considere la posibilidad de usar un proceso de preparación que ejecute esa parte del código antes de que los usuarios accedan e él.

### <a id="httpclientsend"></a>Envío de una solicitud HTTP

Métodos como **HttpClient.Send** indican que el código está esperando a que finalice una solicitud HTTP.

### <a id="sqlcommand"></a>Operación de base de datos

Métodos como **SqlCommand.Execute** indican que el código está a la espera de que finalice una operación de base de datos.

### <a id="await"></a>Espera (AWAIT\_TIME)

**AWAIT\_TIME** indica que el código está a la espera de que finalice otra tarea. Esto sucede normalmente con la instrucción **AWAIT** de C#. Cuando el código efectúa una instrucción **AWAIT** de C#, el subproceso se desenreda y devuelve el control al grupo de subprocesos, y no hay ningún subproceso bloqueado a la espera de que finalice la **AWAIT**. Pero, lógicamente, el subproceso que realizó **AWAIT** está "bloqueado" a la espera de que finalice la operación. La instrucción **AWAIT\_TIME** indica el tiempo de bloqueo a la espera de que finalice la tarea.

### <a id="block"></a>Tiempo de bloqueo

**BLOCKED_TIME** indica que el código está a la espera de que otro recurso esté disponible. Por ejemplo, podría estar esperando un objeto de sincronización, a que un subproceso esté disponible o a que termine una solicitud.

### <a id="cpu"></a>Tiempo de CPU

La CPU está ocupada ejecutando las instrucciones.

### <a id="disk"></a>Tiempo de disco

La aplicación está ejecutando operaciones de disco.

### <a id="network"></a>Tiempo de red

La aplicación está ejecutando operaciones de red.

### <a id="when"></a>Columna Cuándo

La columna **Cuándo** es una visualización de cómo varían con el tiempo las muestras INCLUSIVAS recopiladas para un nodo. El intervalo total de la solicitud se divide en 32 depósitos de tiempo. Las muestras inclusivas para ese nodo se acumulan en esos 32 depósitos. Cada depósito se representa con una barra. El alto de la barra representa un valor escalado. En el caso de nodos marcados **CPU_TIME** o **BLOCKED_TIME**, o cuando existe una relación obvia de consumo de un recurso (por ejemplo, CPU, disco o subproceso), la barra representa el consumo de uno de esos recursos durante el período de tiempo de ese depósito. Para estas métricas, puede obtener un valor superior al 100 % si consume varios recursos. Por ejemplo, si por término medio usa dos CPU a lo largo de un intervalo, consigue el 200 %.

## <a name="limitations"></a>Limitaciones

El período de retención de datos predeterminado es de cinco días. El número máximo de datos ingerido al día es de 10 GB.

No hay ningún cargo por el uso del servicio Profiler. Para usar el servicio Profiler, la aplicación web se debe hospedar al menos en el nivel Básico de Web Apps.

## <a name="overhead-and-sampling-algorithm"></a>Sobrecarga y algoritmo de muestreo

Profiler se ejecuta de manera aleatoria dos minutos cada hora en cada máquina virtual que hospede la aplicación que tiene habilitado Profiler para capturar seguimientos. Al ejecutarse Profiler, agrega una sobrecarga de la CPU del 5 al 15 % al servidor.

Cuantos más servidores haya disponibles para hospedar la aplicación, menor será el impacto de Profiler en el rendimiento general de la aplicación. El motivo es que el algoritmo de muestreo da lugar a que Profiler se ejecute únicamente en un 5 % de los servidores en cualquier momento. Para compensar la sobrecarga del servidor ocasionada por la ejecución de Profiler, hay más servidores disponibles para atender las solicitudes web.

## <a name="disable-profiler"></a>Deshabilitación de Profiler

Para detener o reiniciar Profiler para una instancia de Web Apps individual, en **Trabajos web**, vaya al recurso de Web Apps. Para eliminar Profiler, vaya a **Extensiones**.

![Deshabilitación de Profiler para un trabajo web][disable-profiler-webjob]

Se recomienda tener habilitado Profiler en todas las aplicaciones web para detectar cualquier problema de rendimiento lo antes posible.

Si usa WebDeploy para implementar cambios en la aplicación web, asegúrese de excluir la carpeta App_Data de la eliminación durante la implementación. De lo contrario, los archivos de la extensión de Profiler se eliminan la próxima vez que implemente la aplicación web en Azure.


## <a id="troubleshooting"></a>Solución de problemas

### <a name="too-many-active-profiling-sessions"></a>Hay demasiadas sesiones de generación de perfiles activas

Actualmente, puede habilitar Profiler en hasta cuatro aplicaciones web de Azure y ranuras de implementación que se ejecuten en el mismo plan de servicio. Si el trabajo web de Profiler notifica demasiadas sesiones activas de generación de perfiles, mueva algunas aplicaciones web a un plan de servicio diferente.

### <a name="how-do-i-determine-whether-application-insights-profiler-is-running"></a>¿Cómo se puede determinar si Application Insights Profiler se está ejecutando?

Profiler se ejecuta como un trabajo web continuo de la aplicación web. Puede abrir el recurso de aplicación web en [Azure Portal](https://portal.azure.com). En el panel **WebJobs**, compruebe el estado de **ApplicationInsightsProfiler**. Si no se está ejecutando, abra **Registros** para obtener más información.

### <a name="why-cant-i-find-any-stack-examples-even-though-profiler-is-running"></a>¿Por qué no encuentro ningún ejemplo de pila aunque Profiler se esté ejecutando?

Estas son algunas cosas que puede comprobar:

* Asegúrese de que el plan de App Service web es de nivel Básico o superior.
* Asegúrese de que la aplicación web tenga habilitado el SDK 2.2 Beta o superior de Application Insights.
* Asegúrese de que la aplicación web tenga el valor **APPINSIGHTS_INSTRUMENTATIONKEY** configurado con la misma clave de instrumentación que usa el SDK de Application Insights.
* Asegúrese de que la aplicación web se ejecuta en .NET Framework 4.6.
* Si la aplicación web es una aplicación de ASP.NET Core, debe ejecutar al menos ASP.NET Core 2.0.

Una vez iniciado Profiler, hay un breve proceso de preparación durante el cual recopila activamente varios seguimientos de rendimiento. Después, Profiler recopila los seguimientos de rendimiento durante dos minutos cada hora.

> [!NOTE]
> Hay un error en el agente del generador de perfiles que le impide cargar los seguimientos realizados desde las aplicaciones que se ejecutan en ASP.NET Core 2.1. Estamos trabajando en una solución y estará preparada en breve.

### <a name="i-was-using-azure-service-profiler-what-happened-to-it"></a>Yo usaba Azure Service Profiler. ¿Qué ha ocurrido?

Cuando se habilita Application Insights Profiler, se deshabilita el agente Azure Service Profiler.

### <a id="double-counting"></a>Doble recuento de subprocesos paralelos

En algunos casos, la métrica de tiempo total del visor de la pila es mayor que la duración de la solicitud.

Esta situación puede suceder cuando hay dos o más subprocesos que funcionan en paralelo asociados a una solicitud. En ese caso, el tiempo de subproceso total es superior al tiempo transcurrido. Es posible que uno de los subprocesos esté a la espera de que el otro finalice. El visor intenta detectar esta situación y omite la espera carente de interés, pero prefiere mostrar demasiada información antes que omitir lo que podría ser información crítica.

Si ve subprocesos en paralelo en sus seguimientos, determine cuáles son los subprocesos que esperan para poder establecer la ruta crítica de la solicitud. En la mayoría de los casos, el subproceso que pasa rápidamente a un estado de espera tan solo espera a los demás subprocesos. Concéntrese en los demás subprocesos e ignore el tiempo de los subprocesos en espera.

### <a id="issue-loading-trace-in-viewer"></a>No hay datos de generación de perfiles

Estas son algunas cosas que puede comprobar:

* Si la antigüedad de los datos que intenta ver supera el par de semanas, procure limitar el filtro de tiempo y inténtelo de nuevo.
* Asegúrese de que no haya servidores proxy ni un firewall que bloqueen el acceso a https://gateway.azureserviceprofiler.net.
* Asegúrese de que la clave de instrumentación de Application Insights que usa en la aplicación sea igual a la del recurso de Application Insights que usó para habilitar la generación de perfiles. La clave se encuentra normalmente en el archivo ApplicationInsights.config, pero también podría estar en los archivos web.config o app.config.

### <a name="error-report-in-the-profiling-viewer"></a>Informe de errores del visor de generación de perfiles

Envíe una incidencia de soporte técnico desde el portal. Asegúrese de incluir el identificador de correlación del mensaje de error.

### <a name="deployment-error-directory-not-empty-dhomesitewwwrootappdatajobs"></a>Deployment error: Directory Not Empty (Error de implementación: el directorio no está vacío) "D:\\home\\site\\wwwroot\\App_Data\\jobs"

Si va a volver a implementar su aplicación web en un recurso de Web Apps con Profiler habilitado, puede que aparezca un mensaje similar al siguiente:

*El directorio no está vacío "D:\\home\\site\\wwwroot\\App_Data\\jobs"*

Este error se produce si Web Deploy se ejecuta desde scripts o desde la canalización de implementación de Azure DevOps. La solución consiste en agregar los siguientes parámetros de implementación adicionales a la tarea de Web Deploy:

```
-skip:Directory='.*\\App_Data\\jobs\\continuous\\ApplicationInsightsProfiler.*' -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data\\jobs\\continuous$' -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data\\jobs$'  -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data$'
```

Estos parámetros eliminan la carpeta que usa Application Insights Profiler y desbloquea el proceso de reeimplementación. No afectan a la instancia de Profiler actualmente en ejecución.

## <a name="manual-installation"></a>Instalación manual

Cuando se configura Profiler, se realizan las siguientes actualizaciones en la configuración de la aplicación web. Estas actualizaciones se pueden aplicar manualmente si su entorno lo requiere. Un ejemplo podría ser la ejecución de su aplicación en un entorno de Web Apps para PowerApps.

1. En el panel de **control de la aplicación web**, abra **Configuración**.
1. Establezca **Versión de .Net Framework** en **v4.6**.
1. Establezca **Siempre activado** en **Activado**.
1. Agregue el valor de configuración de la aplicación **APPINSIGHTS_INSTRUMENTATIONKEY** y establezca el valor en la misma clave de instrumentación que usó el SDK.
1. Abra **Herramientas avanzadas**.
1. Seleccione **Ir** para abrir el sitio web de Kudu.
1. En el sitio web de Kudu, seleccione **Site extensions** (Extensiones de sitio).
1. Instale **Application Insights** desde la galería de aplicaciones web de Azure.
1. Reinicie la aplicación web.

## <a id="profileondemand"></a> Desencadenamiento manual de Profiler

Profiler se puede desencadenar manualmente con el clic de un botón. Supongamos que ejecuta una prueba de rendimiento web. Necesitará seguimientos que le ayuden a comprender cómo se ejecuta la aplicación web bajo condiciones de carga. Es fundamental tener control sobre cuándo se capturan los seguimientos, puesto que el usuario sabe cuándo se ejecutará la prueba de carga, pero el intervalo de muestreo aleatorio podría no saberlo.
Los pasos siguientes ilustran cómo funciona este escenario:

### <a name="optional-step-1-generate-traffic-to-your-web-app-by-starting-a-web-performance-test"></a>(Opcional) Paso 1: Generación de tráfico a la aplicación web mediante el inicio de una prueba de rendimiento web

Si la aplicación web tiene ya tráfico entrante o simplemente desea generar tráfico manualmente, omita esta sección y continúe con el paso 2.

Vaya al portal de Application Insights, **Configure > Performance Testing** (Configurar > Prueba de rendimiento). Haga clic en el botón New (Nuevo) para iniciar una nueva prueba de rendimiento.
![Creación de una prueba de rendimiento][create-performance-test]

En el panel **New performance test** (Nueva prueba de rendimiento), configure la dirección URL de destino de la prueba. Acepte toda la configuración predeterminada y empiece a ejecutar la prueba de carga.

![Configuración de la prueba de carga][configure-performance-test]

Verá que la nueva prueba se pone primero en cola, seguido de un estado de en curso.

![La prueba de carga se ha enviado y puesto en cola][load-test-queued]

![Se está ejecutando la prueba de carga y está en curso][load-test-in-progress]

### <a name="step-2-start-profiler-on-demand"></a>Paso 2: Inicio de Profiler a petición

Una vez que se está ejecutando la prueba de carga, se puede iniciar Profiler para capturar los seguimientos de la aplicación web mientras se recibe la carga.
Vaya al panel de configuración de Profiler:

![Entrada al panel de configuración de Profiler][configure-profiler-entry]

En el panel **Configure Profiler** (Configurar Profiler), hay un botón **Profile Now** (Generar perfil ahora) para desencadenar Profiler en todas las instancias de las aplicaciones web vinculadas. Además, se proporciona visibilidad sobre cuándo se ejecutó Profiler en el pasado.

![Profiler a petición][profiler-on-demand]

Verá que la notificación y el estado cambian en el estado de ejecución de Profiler.

### <a name="step-3-view-traces"></a>El paso 3: Visualización de seguimientos

Una vez que termine de ejecutarse Profiler, siga las instrucciones de la notificación para ir a la hoja de rendimiento y ver los seguimientos.

### <a name="troubleshooting-on-demand-profiler"></a>Solución de problemas de Profiler a petición

En ocasiones pude que vea un mensaje de error de tiempo de expiración agotado de Profiler tras una sesión a petición:

![Error de tiempo de expiración de Profiler][profiler-timeout]

Las causas de este error pueden ser dos:

1. La sesión de Profiler a petición se realizó con éxito, pero Application Insights tardó más tiempo en procesar los datos recopilados. Si los datos no han terminado de procesarse en 15 minutos, el portal mostrará un mensaje de tiempo de expiración. Aunque al cabo de un rato, se mostrarán los seguimientos de Profiler. Si esto sucede, omita el mensaje de error por ahora. Estamos trabajando para corregirlo.

1. La aplicación web tiene una versión anterior del agente de Profiler que no tiene la característica a petición. Si ha habilitado anteriormente el perfil de Application Insights, lo más probable es que deba actualizar el agente de Profiler para empezar a usar la funcionalidad a petición.
  
Siga estos pasos para comprobar e instalar la última versión de Profiler:

1. Vaya a la configuración de aplicación de App Services y compruebe si los siguientes valores están establecidos:
    * **APPINSIGHTS_INSTRUMENTATIONKEY**: reemplace este valor por la clave de instrumentación correcta de Application Insights.
    * **APPINSIGHTS_PORTALINFO**: ASP.NET
    * **APPINSIGHTS_PROFILERFEATURE_VERSION**: 1.0.0. Si alguno de estos valores de configuración no está establecido, vaya al panel de habilitación de Application Insights para instalar la extensión de sitio más reciente.

1. Vaya al panel de Application Insights en el portal de App Services.

    ![Habilitación de Application Insights desde el portal de App Services][enable-app-insights]

1. Si ve un botón para actualizar en la siguiente página, haga clic en él para actualizar la extensión de sitio de Application Insights que instalará al agente de Profiler más reciente.
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
[create-performance-test]: ./media/app-insights-profiler/new-performance-test.png
[configure-performance-test]: ./media/app-insights-profiler/configure-performance-test.png
[load-test-queued]: ./media/app-insights-profiler/load-test-queued.png
[load-test-in-progress]: ./media/app-insights-profiler/load-test-inprogress.png
[profiler-on-demand]: ./media/app-insights-profiler/Profiler-on-demand.png
[configure-profiler-entry]: ./media/app-insights-profiler/configure-profiler-entry.png
[enable-app-insights]: ./media/app-insights-profiler/enable-app-insights-blade-01.png
[update-site-extension]: ./media/app-insights-profiler/update-site-extension-01.png
[change-and-save-appinsights]: ./media/app-insights-profiler/change-and-save-appinsights-01.png
[app-settings-for-profiler]: ./media/app-insights-profiler/appsettings-for-profiler-01.png
[check-for-extension-update]: ./media/app-insights-profiler/check-extension-update-01.png
[profiler-timeout]: ./media/app-insights-profiler/profiler-timeout.png
