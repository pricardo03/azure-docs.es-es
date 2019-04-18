---
title: Seguimiento de dependencia en Azure Application Insights | Microsoft Docs
description: Analizar el uso, disponibilidad y rendimiento de su aplicación de web de Microsoft Azure con Application Insights o en el entorno local.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: d15c4ca8-4c1a-47ab-a03d-c322b4bb2a9e
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: mbullwin
ms.openlocfilehash: c77b5810164aef7508f717a0f75d90cf6cba2089
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59273114"
---
# <a name="set-up-application-insights-dependency-tracking"></a>Configurar Application Insights: Seguimiento de dependencias
Una *dependencia* es un componente externo al que llama la aplicación. Suele ser un servicio al que se llama mediante HTTP, una base de datos o un sistema de archivos. [Application Insights](../../azure-monitor/app/app-insights-overview.md) mide cuánto tiempo espera su aplicación a las dependencias y la frecuencia con que se produce un error en una llamada de dependencia. Puede investigar llamadas específicas y relacionarlas a solicitudes y excepciones.

El monitor de dependencia listo para su uso sin configuraciones adicionales actualmente llama a estos tipos de dependencias:

* Server
  * Bases de datos SQL
  * Servicios WFC y web de ASP.NET que usan enlaces basados en HTTP
  * Llamadas HTTP locales o remotas
  * Azure Cosmos DB, Table Storage, Blob Storage y Queue Storage 
* Páginas web
  * Llamadas AJAX

La supervisión funciona mediante la [instrumentación del código de bytes](https://msdn.microsoft.com/library/z9z62c29.aspx) en función de los métodos seleccionados o de las devoluciones de llamada de DiagnosticSource (en los SDK de .NET más recientes) de .NET Framework. La sobrecarga de rendimiento es mínima.

También puede escribir sus propias llamadas de SDK para supervisar otras dependencias, en el código de cliente y servidor, que usan la [API de TrackDependency](../../azure-monitor/app/api-custom-events-metrics.md#trackdependency).

> [!NOTE]
> Azure Cosmos DB se rastrea automáticamente solo si se usa [HTTP/HTTPS](../../cosmos-db/performance-tips.md#networking). Application Insights no capturará el modo TCP.

## <a name="set-up-dependency-monitoring"></a>Configuración de la supervisión de dependencias
El [SDK de Application Insights](asp-net.md) recopila automáticamente información de dependencias parciales. Para obtener datos completos, instale al agente adecuado para el servidor host.

| Plataforma | Instalación |
| --- | --- |
| Servidor IIS |[Instale el Monitor de estado en el servidor](../../azure-monitor/app/monitor-performance-live-website-now.md) o [actualice la aplicación a .NET Framework 4.6 o superior](https://go.microsoft.com/fwlink/?LinkId=528259) e instale el [SDK de Application Insights](asp-net.md) en la aplicación. |
| Aplicación web de Azure |En el panel de control de la aplicación web, [abra la hoja Application Insights del panel de control de la aplicación web](../../azure-monitor/app/azure-web-apps.md) y seleccione Instalar, si aparece dicha opción. |
| Servicio en la nube de Azure |[Uso de tarea de inicio](../../azure-monitor/app/cloudservices.md) o [Instalación de .NET Framework 4.6 o versiones posteriores](../../cloud-services/cloud-services-dotnet-install-dotnet.md) |

## <a name="where-to-find-dependency-data"></a>Dónde encontrar los datos de dependencia
* [Asignación de aplicación](#application-map) visualiza las dependencias entre la aplicación y los componentes colindantes.
* Las hojas [Rendimiento, Exploradores y Errores](https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-performance) muestran datos de dependencia del servidor.
* La hoja [Exploradores](#ajax-calls) muestra las llamadas AJAX de los exploradores de los usuarios.
* Haga clic en las solicitudes lentas o con errores para comprobar sus llamadas de dependencia.
* [Análisis](#analytics) puede utilizarse para consultar los datos de dependencia.

## <a name="application-map"></a>Mapa de aplicación
Mapa de aplicación sirve de ayuda visual para detectar las dependencias entre los componentes de la aplicación. Se genera automáticamente a partir de la telemetría de la aplicación. En este ejemplo se muestran las llamadas AJAX de los scripts del explorador y las llamadas REST de la aplicación del servidor a dos servicios externos.

![Mapa de aplicación](./media/asp-net-dependencies/cloud-rolename.png)

* **Desplácese por los cuadros** para ir a la dependencia pertinente y a otros gráficos.
* **Ancle el mapa** al [panel](../../azure-monitor/app/app-insights-dashboards.md), donde será completamente funcional.

[Más información](../../azure-monitor/app/app-map.md).

## <a name="performance-and-failure-blades"></a>Hojas Rendimiento y Errores
La hoja Rendimiento muestra la duración de las llamadas de dependencia que realiza la aplicación del servidor.

Los **número de errores** se muestran en la hoja **Errores**. Un error es cualquier código de retorno que no está en el intervalo 200-399, o bien uno desconocido.

> [!NOTE]
> **¿El porcentaje de errores es 100?** Probablemente, esto signifique que está obteniendo datos de dependencias parciales. Tiene que [configurar la supervisión de dependencias adecuada a su plataforma](#set-up-dependency-monitoring).
>
>

## <a name="ajax-calls"></a>Llamadas AJAX
La hoja Exploradores muestra la duración y la frecuencia de errores de las llamadas AJAX de [JavaScript en las páginas web](../../azure-monitor/app/javascript.md). Se muestran como dependencias.

## <a name="diagnosis"></a> Diagnóstico de solicitudes lentas
Cada evento de solicitud se asocia con las llamadas de dependencia, excepciones y otros eventos que se realiza un seguimiento mientras la aplicación está procesando la solicitud. Por lo tanto, si algunas solicitudes no se procesan correctamente, puede averiguar si se debe a respuestas lentas de una dependencia.

### <a name="profile-your-live-site"></a>Generación de un perfil del sitio activo

¿Quiere saber en qué se invierte el tiempo? El [Application Insights profiler](../../azure-monitor/app/profiler.md) seguimientos HTTP llama a los sitios activos y muestra qué funciones del código tardaron más tiempo.

## <a name="analytics"></a>Análisis
Puede realizar un seguimiento de las dependencias en el [lenguaje de consulta de Kusto](/azure/kusto/query/). Estos son algunos ejemplos.

* Búsqueda de llamadas de dependencia con errores:

```

    dependencies | where success != "True" | take 10
```

* Búsqueda de llamadas AJAX:

```

    dependencies | where client_Type == "Browser" | take 10
```

* Búsqueda de llamadas de dependencia asociadas a solicitudes:

```

    dependencies
    | where timestamp > ago(1d) and  client_Type != "Browser"
    | join (requests | where timestamp > ago(1d))
      on operation_Id  
```


* Búsqueda de llamadas AJAX asociadas a vistas de página:

```

    dependencies
    | where timestamp > ago(1d) and  client_Type == "Browser"
    | join (browserTimings | where timestamp > ago(1d))
      on operation_Id
```



## <a name="custom-dependency-tracking"></a>Seguimiento de dependencias personalizadas
El módulo de seguimiento de dependencias estándar detecta automáticamente las dependencias externas, como bases de datos y API de REST. Pero tal vez quieras que se traten algunos componentes adicionales de la misma manera.

Puede escribir código que envíe la información de dependencia usando la misma [API de TrackDependency](../../azure-monitor/app/api-custom-events-metrics.md#trackdependency) que usan los módulos estándar.

Por ejemplo, si compila el código con un ensamblado que no escribió usted mismo, podría cronometrar todas las llamadas al ensamblado para averiguar cómo contribuye a los tiempos de respuesta. Para que estos datos se muestren en los gráficos de dependencia en Application Insights, envíelos mediante `TrackDependency`.

```csharp

            var startTime = DateTime.UtcNow;
            var timer = System.Diagnostics.Stopwatch.StartNew();
            try
            {
                success = dependency.Call();
            }
            finally
            {
                timer.Stop();
                telemetry.TrackDependency("myDependency", "myCall", startTime, timer.Elapsed, success);
                // The call above has been made obsolete in the latest SDK. The updated call follows this format:
                // TrackDependency (string dependencyTypeName, string dependencyName, string data, DateTimeOffset startTime, TimeSpan duration, bool success);
            }
```

Si desea desactivar el módulo de seguimiento de dependencia estándar, quite la referencia a DependencyTrackingTelemetryModule en [ApplicationInsights.config](../../azure-monitor/app/configuration-with-applicationinsights-config.md).

## <a name="troubleshooting"></a>solución de problemas
*La marca de éxito de dependencia siempre muestra true o false.*

*La consulta SQL no se muestra en su totalidad.*

Consulte la tabla siguiente y asegúrese de que ha elegido la configuración correcta para habilitar la supervisión de las dependencias de la aplicación.

| Plataforma | Instalación |
| --- | --- |
| Servidor IIS |Instale el [Monitor de estado en el servidor](../../azure-monitor/app/monitor-performance-live-website-now.md). O [actualice la aplicación a .NET Framework 4.6 o superior](https://go.microsoft.com/fwlink/?LinkId=528259) e instale el [SDK de Application Insights](asp-net.md) en la aplicación. |
| IIS Express |Use el servidor de IIS en su lugar. |
| Aplicación web de Azure |En el panel de control de la aplicación web, [abra la hoja Application Insights del panel de control de la aplicación web](../../azure-monitor/app/azure-web-apps.md) y seleccione Instalar, si aparece dicha opción. |
| Servicio en la nube de Azure |[Use la tarea de la startup](../../azure-monitor/app/cloudservices.md) o [instale .NET Framework 4.6 o una versión más reciente](../../cloud-services/cloud-services-dotnet-install-dotnet.md). |

## <a name="next-steps"></a>Pasos siguientes
* [Excepciones](../../azure-monitor/app/asp-net-exceptions.md)
* [Datos de página y usuario](../../azure-monitor/app/javascript.md)
* [Disponibilidad](../../azure-monitor/app/monitor-web-app-availability.md)
