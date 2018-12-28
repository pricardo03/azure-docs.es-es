---
title: Generación de perfiles de aplicaciones de producción en Azure con Application Insights Profiler | Microsoft Docs
description: Identifique la ruta de acceso activa en el código del servidor web con un generador de perfiles de bajo impacto.
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
ms.openlocfilehash: 7780c10233a0ce256ee6e9015f40ea789516c25b
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2018
ms.locfileid: "52726906"
---
# <a name="profile-production-applications-in-azure-with-application-insights"></a>Generación de perfiles de aplicaciones de producción en Azure con Application Insights
## <a name="enable-profiler-for-your-application"></a>Habilitación de Profiler para la aplicación

Application Insights Profiler proporciona un seguimiento del rendimiento de las aplicaciones que se ejecutan en producción en Azure. Captura los datos automáticamente a escala sin que afecte de forma negativa a los usuarios finales. Profiler le ayuda a identificar la ruta de acceso "activa" al código que tarda más tiempo cuando se atiende una solicitud web determinada. 

El generador de perfiles funciona con aplicaciones .Net implementadas en los siguientes servicios de Azure. En los vínculos siguientes se pueden encontrar instrucciones específicas para habilitar el generador de perfiles para cada tipo de servicio.

* [App Services](app-insights-profiler.md?toc=/azure/azure-monitor/toc.json)
* [Cloud Services](app-insights-profiler-cloudservice.md?toc=/azure/azure-monitor/toc.json)
* [Aplicaciones de Service Fabric](app-insights-profiler-servicefabric.md?toc=/azure/azure-monitor/toc.json)
* [Virtual Machines y Virtual Machine Scale Sets](app-insights-profiler-vm.md?toc=/azure/azure-monitor/toc.json)

Si ha habilitado Profiler, pero no ve los seguimientos, consulte nuestra [Guía de solución de problemas.](app-insights-profiler-troubleshooting.md?toc=/azure/azure-monitor/toc.json)

La ejecución de Profiler en el entorno local no se admite oficialmente, pero hay algunas [instrucciones que puede intentar.](https://docs.microsoft.com/azure/application-insights/enable-profiler-compute#enable-profiler-on-on-premises-servers)

## <a name="view-profiler-data"></a>Visualización de datos de Profiler

Para que el generador de perfiles cargue los seguimientos, la aplicación tiene que administrar las solicitudes de forma activa. Si está realizando un experimento, puede generar solicitudes dirigidas a la aplicación web mediante [Pruebas de rendimiento de Application Insights](https://docs.microsoft.com/vsts/load-test/app-service-web-app-performance-test). Si acaba de habilitar Profiler, puede ejecutar una breve prueba de carga. Mientras se ejecuta la prueba de carga, presione el botón **Profile Now** (Generar perfil ahora) en la página [**Profiler Settings**](app-insights-profiler-settings.md#profiler-settings-page) (Configuración de Profiler). Una vez que el generador de perfiles está en ejecución, genera perfiles aleatoriamente una vez cada hora aproximadamente y durante dos minutos. Si la aplicación está controlando un flujo constante de solicitudes, Profiler carga los seguimientos cada hora.

Después de que la aplicación recibe tráfico y el generador de perfiles ha tenido tiempo de cargar los seguimientos, podrá verlos. Este proceso puede tardar entre 5 y 10 minutos. Para ver los seguimientos, vaya al panel **Performance** (Rendimiento), seleccione **Take Actions** (Realizar acciones) para ver los seguimientos del generador de perfiles y, luego, seleccione el botón **Profiler Traces** (Seguimientos de Profiler).

![Seguimientos de Profiler de vista previa del panel Rendimiento de Application Insights][performance-blade]

Seleccione una muestra para mostrar un desglose de nivel de código del tiempo dedicado a ejecutar la solicitud.

![Explorador de seguimiento de Application Insights][trace-explorer]

El explorador de seguimiento muestra la siguiente información:

* **Mostrar ruta de acceso activa**: abre el nodo hoja más grande o al menos algo que se aproxime. En la mayoría de los casos, este nodo está cerca de un cuello de botella de rendimiento.
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

Si **clr!ThePreStub** tarda mucho tiempo con una solicitud, significa que la solicitud es la primera que ejecuta ese método. El entorno de ejecución de .NET Framework tarda un tiempo considerable en cargar el primer método. Podría plantearse la posibilidad de usar un proceso de preparación que ejecute esa parte del código antes de que los usuarios accedan a él, o bien ejecutar el Generador de imágenes nativo (ngen.exe) en sus ensamblados.

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

**AWAIT\_TIME** indica que el código está a la espera de que finalice otra tarea. Esto sucede normalmente con la instrucción **AWAIT** de C#. Cuando el código efectúa una instrucción **AWAIT** de C#, el subproceso se desenreda y devuelve el control al grupo de subprocesos, y no hay ningún subproceso bloqueado a la espera de que finalice **AWAIT**. Pero, lógicamente, el subproceso que realizó **AWAIT** está "bloqueado" a la espera de que finalice la operación. La instrucción **AWAIT\_TIME** indica el tiempo de bloqueo a la espera de que finalice la tarea.

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

## <a name="next-steps"></a>Pasos siguientes
Habilitación de Application Insights Profiler para la aplicación de Azure
* [App Services](app-insights-profiler.md?toc=/azure/azure-monitor/toc.json)
* [Cloud Services](app-insights-profiler-cloudservice.md?toc=/azure/azure-monitor/toc.json)
* [Aplicaciones de Service Fabric](app-insights-profiler-servicefabric.md?toc=/azure/azure-monitor/toc.json)
* [Virtual Machines y Virtual Machine Scale Sets](app-insights-profiler-vm.md?toc=/azure/azure-monitor/toc.json)


[performance-blade]: ./media/app-insights-profiler/performance-blade-v2-examples.png
[trace-explorer]: ./media/app-insights-profiler/trace-explorer.png
