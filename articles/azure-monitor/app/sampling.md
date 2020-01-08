---
title: Muestreo de telemetría en Azure Application Insights | Microsoft Docs
description: Cómo mantener el volumen de telemetría bajo control.
ms.service: azure-monitor
ms.subservice: application-insights
ms.topic: conceptual
author: mrbullwinkle
ms.author: mbullwin
ms.date: 03/14/2019
ms.reviewer: vitalyg
ms.openlocfilehash: d88df0c7e17d297162a1921021b89f02077c2ac7
ms.sourcegitcommit: 5925df3bcc362c8463b76af3f57c254148ac63e3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/31/2019
ms.locfileid: "75560392"
---
# <a name="sampling-in-application-insights"></a>Muestreo en Application Insights.

El muestreo es una característica de [Azure Application Insights](../../azure-monitor/app/app-insights-overview.md). Se trata de la manera recomendada de reducir el almacenamiento y el tráfico de telemetría conservando, al mismo tiempo, un análisis estadísticamente correcto de datos de las aplicaciones. El filtro selecciona los elementos relacionado para que pueda desplazarse entre los elementos de diagnóstico cuando esté realizando investigaciones de diagnóstico.
Cuando los recuentos de métrica se presentan en el portal, se vuelven a normalizar para tener en cuenta el muestreo. De este modo, se minimizan los efectos en las estadísticas.

El muestreo reduce los costos de tráfico y datos y le ayuda a evitar la limitación.

## <a name="in-brief"></a>En resumen:

* El muestreo conserva 1 en *n* registros y descarta el resto. Por ejemplo, podría retener uno de cinco eventos, una frecuencia de muestreo del 20 %. 
* El muestreo adaptable está habilitado de manera predeterminada en todas las versiones más recientes de ASP.NET y los Kits de desarrollo de software (SDK) de ASP.NET Core.
* También puede establecer el muestreo manualmente. Esto se puede configurar en el portal en la página *Uso y costos estimados*, en el SDK de ASP.NET, en el archivo ApplicationInsights.config, en el SDK de ASP.NET Core a través del código o en el SDK de Java en el archivo ApplicationInsights.xml.
* Si registra eventos personalizados y necesita asegurarse de que un conjunto de eventos se conserve descarte juntos, los eventos deben tener el mismo valor de OperationId.
* El divisor de muestreo *n* se notifica en cada registro de la propiedad `itemCount`, que en la búsqueda aparece bajo el nombre descriptivo "recuento de solicitudes" o "recuento de eventos". `itemCount==1` cuando el muestreo no está en funcionamiento.
* Si escribe consultas de Analytics, debería [tener en cuenta el muestreo](../../azure-monitor/log-query/aggregations.md). En concreto, en lugar de simplemente contar registros, debería usar `summarize sum(itemCount)`.

## <a name="types-of-sampling"></a>Tipos de muestreo

Existen tres métodos de muestreo alternativos:

* El **muestreo adaptable** ajusta automáticamente el volumen de telemetría que se envía desde el SDK en la aplicación de ASP.NET/ASP.NET Core. Se trata del muestreo predeterminado de del SDK de ASP.NET Web v2.0.0-beta3 y versiones posteriores y el SDK de Microsoft.ApplicationInsights.AspNetCore v2.2.0-beta1 y versiones posteriores.  Actualmente, el muestreo solo está disponible para la telemetría del lado servidor de ASP.NET.

* El **muestreo de frecuencia fija** reduce el volumen de telemetría que se envía desde el servidor ASP.NET, ASP.NET Core o Java y desde los exploradores de los usuarios. El usuario establece la frecuencia. El cliente y el servidor sincronizarán su muestreo por lo que, en Búsqueda, puede desplazarse entre las solicitudes y las vistas de página relacionadas.

* El **muestreo de ingesta** funciona en Azure Portal. Lo que hace es descartar algunos de los datos de telemetría que llegan desde su aplicación según la frecuencia de muestreo establecida. Aunque no reduce el tráfico de telemetría enviado desde su aplicación, le ayuda a mantenerse dentro de su cuota mensual. La principal ventaja del muestreo de ingesta es que puede establecer la frecuencia de muestreo sin volver a implementar la aplicación. El muestreo de ingesta funciona de manera uniforme para todos los servidores y clientes.

Si el muestreo de velocidad adaptable o fija está funcionando, el muestreo de ingesta se deshabilita.


## <a name="adaptive-sampling-in-your-aspnetaspnet-core-web-applications"></a>Muestreo adaptable en las aplicaciones Web de ASP.NET/ASP.NET Core

El muestreo adaptable está disponible para el SDK de Application Insights para ASP.NET v2.0.0-beta3 y versiones posteriores, y el SDK de Microsoft.ApplicationInsights.AspNetCore v2.2.0-beta1 y versiones posteriores, y está habilitado de manera predeterminada.

Influye en el volumen de telemetría enviado desde su aplicación de servidor web hasta el punto de conexión del servicio de Application Insights. El volumen se ajusta automáticamente para mantenerse dentro de una frecuencia máxima especificada de tráfico y se controla mediante el valor `MaxTelemetryItemsPerSecond`. Si la aplicación genera una cantidad baja de telemetría, como al depurar o debido a un uso bajo, el procesador de muestreo no podrá descargar los elementos mientras el volumen se encuentre por debajo de `MaxTelemetryItemsPerSecond`. A medida que aumente el volumen de telemetría, la frecuencia de muestreo se ajusta con el fin de lograr el volumen objetivo.

Para lograr el volumen objetivo, se descartan algunos de los datos de telemetría generados. Pero, al igual que otros tipos de muestreo, el algoritmo conserva los elementos de telemetría relacionados. Por ejemplo, cuando se inspeccione la telemetría en Búsqueda, podrá buscar la solicitud relacionada con una excepción determinada.

Los recuentos de métrica, como la tasa de solicitudes y la tasa de excepciones, se ajustan para compensar la frecuencia de muestreo, de modo que exhiban valores aproximadamente correctos en el Explorador de métricas.

## <a name="configuring-adaptive-sampling-for-aspnet-applications"></a>Configuración del muestreo adaptable para las aplicaciones ASP.NET

[Obtenga información](../../azure-monitor/app/sampling.md#configuring-adaptive-sampling-for-aspnet-core-applications) sobre cómo configurar el muestreo adaptable para aplicaciones ASP.NET Core. 

En [ApplicationInsights.config](../../azure-monitor/app/configuration-with-applicationinsights-config.md), puede ajustar varios parámetros en el nodo `AdaptiveSamplingTelemetryProcessor`. Las cifras que se muestran son los valores predeterminados:

* `<MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>`
  
    Velocidad objetivo que el algoritmo de adaptación intenta lograr **en cada host de servidor**. Si la aplicación web se ejecuta en varios hosts, reduzca este valor para que se mantenga dentro de la velocidad objetivo de tráfico en el portal de Application Insights.
* `<EvaluationInterval>00:00:15</EvaluationInterval>` 
  
    Intervalo en el que se vuelve a evaluar la velocidad actual de telemetría. La evaluación se realiza como una media móvil. Se recomienda acortar este intervalo si la telemetría experimenta ráfagas repentinas.
* `<SamplingPercentageDecreaseTimeout>00:02:00</SamplingPercentageDecreaseTimeout>`
  
    Cuando se cambia el valor de porcentaje de muestreo, tiempo mínimo que se tarda en permitir de nuevo que se reduzca el porcentaje de muestreo para capturar menos datos.
* `<SamplingPercentageIncreaseTimeout>00:15:00</SamplingPercentageIncreaseTimeout>`
  
    Cuando se cambia el valor de porcentaje de muestreo, tiempo mínimo que se tarda en permitir de nuevo que se aumente el porcentaje de muestreo para capturar más datos.
* `<MinSamplingPercentage>0.1</MinSamplingPercentage>`
  
    Como el porcentaje de muestreo varía, valor mínimo que se permite establecer.
* `<MaxSamplingPercentage>100.0</MaxSamplingPercentage>`
  
    Como el porcentaje de muestreo varía, valor máximo que se permite establecer.
* `<MovingAverageRatio>0.25</MovingAverageRatio>` 
  
    En el cálculo de la media móvil, peso asignado al valor más reciente. Use un valor igual o menor que 1. Los valores menores hacen que el algoritmo reaccione con menor agilidad a los cambios repentinos.
* `<InitialSamplingPercentage>100</InitialSamplingPercentage>`
  
    Valor asignado cuando se acaba de iniciar la aplicación. No lo reduzca durante la depuración.

* `<ExcludedTypes>Trace;Exception</ExcludedTypes>`
  
    Una lista delimitada por puntos y coma de tipos que no desea que se muestreen. Los tipos reconocidos son: Dependency, Event, Exception, PageView, Request y Trace. Todas las instancias de los tipos especificados se transmiten; los tipos no especificados se muestrean.

* `<IncludedTypes>Request;Dependency</IncludedTypes>`
  
    Una lista delimitada por puntos y coma de tipos que desea que se muestreen. Los tipos reconocidos son: Dependency, Event, Exception, PageView, Request y Trace. Los tipos especificados se muestrean, todas las instancias del resto de tipos siempre se transmitirán.


**Para desactivar** el muestreo adaptable, quite los nodos AdaptiveSamplingTelemetryProcessor de applicationinsights-config.

### <a name="alternative-configure-adaptive-sampling-in-code"></a>Alternativa: configure el muestreo adaptivo en el código

En lugar de establecer el parámetro de muestreo en el archivo .config, puede establecer estos valores mediante programación.

1. Quite todos los nodos `AdaptiveSamplingTelemetryProcessor` del archivo .config.
2. Use el siguiente fragmento de código para configurar el muestreo adaptable.

*C#*

```csharp

    using Microsoft.ApplicationInsights;
    using Microsoft.ApplicationInsights.Extensibility;
    using Microsoft.ApplicationInsights.WindowsServer.Channel.Implementation;
    using Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel;
    ...

    var builder = TelemetryConfiguration.Active.TelemetryProcessorChainBuilder;
    // If you are on ApplicationInsights SDK v 2.8.0-beta2 or higher, use the following line instead
    // var builder = TelemetryConfiguration.Active.DefaultTelemetrySink.TelemetryProcessorChainBuilder;

    // Enable AdaptiveSampling so as to keep overall telemetry volume to 5 items per second.
    builder.UseAdaptiveSampling(maxTelemetryItemsPerSecond:5);

    // If you have other telemetry processors:
    builder.Use((next) => new AnotherProcessor(next));

    builder.Build();

```

([Más información sobre los procesadores de telemetría](../../azure-monitor/app/api-filtering-sampling.md#filtering)).

También puede ajustar la frecuencia de muestreo para cada tipo de telemetría individualmente o incluso puede excluir determinados tipos completos del muestreo. 

*C#*

```csharp
    // The following configures adaptive sampling with 5 items per second, and also excludes DependencyTelemetry from being subject to sampling.
    builder.UseAdaptiveSampling(maxTelemetryItemsPerSecond:5, excludedTypes: "Dependency");
```

## <a name="configuring-adaptive-sampling-for-aspnet-core-applications"></a>Configuración del muestreo adaptable para las aplicaciones ASP.NET Core.

No hay ningún objeto `ApplicationInsights.Config` para las aplicaciones ASP.NET Core, por lo que todas las configuraciones se realizan a través de código.
El muestreo adaptable está habilitado de forma predeterminada para todas las aplicaciones de ASP.NET Core. Puede deshabilitar o personalizar el comportamiento de muestreo.

### <a name="turning-off-adaptive-sampling"></a>Desactivación del muestreo adaptable

La característica de muestreo predeterminada se puede deshabilitar al agregar el servicio de Application Insights, en el método ```ConfigureServices```, con ```ApplicationInsightsServiceOptions``` dentro del archivo `Startup.cs`:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // ...

    var aiOptions = new Microsoft.ApplicationInsights.AspNetCore.Extensions.ApplicationInsightsServiceOptions();
    aiOptions.EnableAdaptiveSampling = false;
    services.AddApplicationInsightsTelemetry(aiOptions);
    //...
}
```

El código anterior deshabilitará la característica de muestreo. Siga los pasos a continuación para agregar el muestreo con más opciones de personalización.

### <a name="configure-sampling-settings"></a>Configuración de las opciones de muestreo

Use los métodos de extensión de ```TelemetryProcessorChainBuilder``` como se muestra a continuación para personalizar el comportamiento de muestreo.

> [!IMPORTANT]
> Si usa este método para configurar el muestreo, asegúrese de usar la opción aiOptions.EnableAdaptiveSampling = false; con AddApplicationInsightsTelemetry().

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env, TelemetryConfiguration configuration)
{
    var builder = configuration.TelemetryProcessorChainBuilder;
    // version 2.5.0-beta2 and above should use the following line instead of above. (https://github.com/Microsoft/ApplicationInsights-aspnetcore/blob/develop/CHANGELOG.md#version-250-beta2)
    // var builder = configuration.DefaultTelemetrySink.TelemetryProcessorChainBuilder;

    // Using adaptive sampling
    builder.UseAdaptiveSampling(maxTelemetryItemsPerSecond:5);

    // Alternately, the following configures adaptive sampling with 5 items per second, and also excludes DependencyTelemetry from being subject to sampling.
    // builder.UseAdaptiveSampling(maxTelemetryItemsPerSecond:5, excludedTypes: "Dependency");

    builder.Build();

    // If you have other telemetry processors:
    builder.Use((next) => new AnotherProcessor(next));
    // ...
}

```

**Si usa el método anterior para configurar el muestreo, asegúrese de usar la configuración de ```aiOptions.EnableAdaptiveSampling = false;``` con AddApplicationInsightsTelemetry().**

## <a name="fixed-rate-sampling-for-aspnet-aspnet-core-java-websites-and-python-applications"></a>Muestreo de frecuencia fija para sitios web ASP.NET, ASP.NET Core y Java y aplicaciones de Python

El muestreo de frecuencia fija reduce el tráfico enviado desde el servidor web y los exploradores web. A diferencia del muestreo adaptable, reduce la telemetría a una tasa fija que usted decide. También sincroniza el muestreo del cliente y del servidor para que los elementos relacionados se conserven; por ejemplo, cuando mira una vista de página en Búsqueda, puede encontrar su solicitud relacionada.

Al igual que otras técnicas de muestreo, esto también conserva elementos relacionados. Para cada evento de solicitud HTTP, esta y sus eventos relacionados se descartan o transmiten conjuntamente.

En el Explorador de métricas, las tasas, como los recuentos de solicitudes y de excepciones, se multiplican por un factor para compensar la frecuencia de muestreo, de forma que sean aproximadamente correctas.

### <a name="configuring-fixed-rate-sampling-in-aspnet"></a>Configuración del muestreo de frecuencia fija en ASP.NET

1. **Deshabilite el muestreo adaptable**: en [ApplicationInsights.config](../../azure-monitor/app/configuration-with-applicationinsights-config.md), quite o convierta en comentario del nodo `AdaptiveSamplingTelemetryProcessor`.

    ```xml

    <TelemetryProcessors>

    <!-- Disabled adaptive sampling:
      <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.AdaptiveSamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">
        <MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>
      </Add>
    -->
    ```

2. **Habilite el módulo de muestreo de frecuencia fija.** Agregue este fragmento a [ApplicationInsights.config](../../azure-monitor/app/configuration-with-applicationinsights-config.md):
   
    ```XML
   
    <TelemetryProcessors>
     <Add  Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.SamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">
   
      <!-- Set a percentage close to 100/N where N is an integer. -->
     <!-- E.g. 50 (=100/2), 33.33 (=100/3), 25 (=100/4), 20, 1 (=100/100), 0.1 (=100/1000) -->
      <SamplingPercentage>10</SamplingPercentage>
      </Add>
    </TelemetryProcessors>
   
    ```
   ### <a name="alternative-enable-fixed-rate-sampling-in-your-server-code"></a>Alternativa: habilitación del muestreo de frecuencia fija en el código de servidor
    
    En lugar de establecer el parámetro de muestreo en el archivo .config, puede establecer estos valores mediante programación. 

*C#*

```csharp

    using Microsoft.ApplicationInsights.Extensibility;
    using Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel;
    ...

    var builder = TelemetryConfiguration.Active.TelemetryProcessorChainBuilder;
    // If you are on ApplicationInsights SDK v 2.8.0-beta2 or higher, use the following line instead
    // var builder = TelemetryConfiguration.Active.DefaultTelemetrySink.TelemetryProcessorChainBuilder;

    builder.UseSampling(10.0); // percentage

    // If you have other telemetry processors:
    builder.Use((next) => new AnotherProcessor(next));

    builder.Build();

```
([Más información sobre los procesadores de telemetría](../../azure-monitor/app/api-filtering-sampling.md#filtering)).

### <a name="configuring-fixed-rate-sampling-in-aspnet-core"></a>Configuración del muestreo de frecuencia fija en ASP.NET Core

1. **Deshabilite el muestreo adaptable**:  Se pueden realizar cambios en el método ```ConfigureServices``` mediante ```ApplicationInsightsServiceOptions```:

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
    // ...

        var aiOptions = new Microsoft.ApplicationInsights.AspNetCore.Extensions.ApplicationInsightsServiceOptions();
        aiOptions.EnableAdaptiveSampling = false;
        services.AddApplicationInsightsTelemetry(aiOptions);
    //...
    }
    ```

2. **Habilite el módulo de muestreo de frecuencia fija.** Se pueden realizar cambios en el método ```Configure``` tal como se muestra en el siguiente fragmento de código:

    ```csharp
    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        var configuration = app.ApplicationServices.GetService<TelemetryConfiguration>();

        var builder = configuration.TelemetryProcessorChainBuilder;
        // version 2.5.0-beta2 and above should use the following line instead of above. (https://github.com/Microsoft/ApplicationInsights-aspnetcore/blob/develop/CHANGELOG.md#version-250-beta2)
        // var builder = configuration.DefaultTelemetrySink.TelemetryProcessorChainBuilder;

        // Using fixed rate sampling   
        double fixedSamplingPercentage = 10;
        builder.UseSampling(fixedSamplingPercentage);

        builder.Build();

        // ...
    }

    ```

### <a name="configuring-fixed-rate-sampling-in-java"></a>Configuración del muestreo de frecuencia fija en JAVA ###

1. Descargue y configure la aplicación web con la versión más reciente del [SDK de Java de Application Insights](../../azure-monitor/app/java-get-started.md)

2. **Habilite el módulo de muestreo de frecuencia fija** agregando el siguiente fragmento de código al archivo ApplicationInsights.xml.

    ```XML
        <TelemetryProcessors>
            <BuiltInProcessors>
                <Processor type = "FixedRateSamplingTelemetryProcessor">
                    <!-- Set a percentage close to 100/N where N is an integer. -->
                    <!-- E.g. 50 (=100/2), 33.33 (=100/3), 25 (=100/4), 20, 1 (=100/100), 0.1 (=100/1000) -->
                    <Add name = "SamplingPercentage" value = "50" />
                </Processor>
            </BuiltInProcessors>
        </TelemetryProcessors>
    ```

3. Puede incluir o excluir determinados tipos de telemetría de muestreo con las siguientes etiquetas dentro de la etiqueta de procesador "FixedRateSamplingTelemetryProcessor".
    ```XML
        <ExcludedTypes>
            <ExcludedType>Request</ExcludedType>
        </ExcludedTypes>

        <IncludedTypes>
            <IncludedType>Exception</IncludedType>
        </IncludedTypes>
    ```

Los tipos de telemetría que se pueden incluir o excluir del muestreo son los siguientes: Dependency, Event, Exception, PageView, Request y Trace.

> [!NOTE]
> Para el porcentaje de muestreo, elija un porcentaje que esté cerca de 100/N, donde N es un número entero.  Actualmente el muestreo no es compatible con otros valores.
> 
> 

<a name="other-web-pages"></a>

### <a name="configuring-fixed-rate-sampling-in-opencensus-python"></a>Configuración del muestreo de frecuencia fija en OpenCensus para Python ###

1. Instrumente la aplicación con los [exportadores de Azure Monitor de OpenCensus](../../azure-monitor/app/opencensus-python.md) más recientes.

> [!NOTE]
> El muestreo de frecuencia fija solo está disponible con el exportador de seguimiento. Esto significa que las solicitudes entrantes y salientes son los únicos tipos de telemetría en los que se puede configurar el muestreo.
> 
> 

2. Puede especificar un elemento `sampler` como parte de la configuración de `Tracer`. Si no se proporciona ningún elemento sampler explícito, se usará de forma predeterminada ProbabilitySampler. ProbabilitySampler usaría una frecuencia de 1/10000 de forma predeterminada, lo que significa que se enviará a Application Insights una de cada 10000 solicitudes. Si desea especificar una frecuencia de muestreo, consulte a continuación.

3. Al especificar un elemento sampler, asegúrese de que el elemento `Tracer` especifica un elemento sampler con una frecuencia de muestreo entre 0,0 y 1,0, ambos inclusive. Una velocidad de muestreo de 1,0 representa el 100 %, lo que significa que todas las solicitudes se enviarán como datos de telemetría a Application Insights.

    ```python
    tracer = Tracer(
        exporter=AzureExporter(
            instrumentation_key='00000000-0000-0000-0000-000000000000',
        ),
        sampler=ProbabilitySampler(1.0),
    )
    ```

## <a name="ingestion-sampling"></a>muestreo de ingesta

Esta forma de muestreo funciona en el punto donde la telemetría de su servidor web, de sus exploradores y de sus dispositivos alcanza el punto de conexión del servicio de Application Insights. Aunque no reduce el tráfico de telemetría enviado desde su aplicación, sí reduce la cantidad procesada y retenida (y cobrada) por Application Insights.

Utilice este tipo de muestreo si con frecuencia su aplicación sobrepasa su cuota mensual y no tiene la opción de usar ninguno de los tipos de muestreo basados en el SDK. 

Establecer la frecuencia de muestreo en la página Uso y costos estimados:

![En la hoja Información general de la aplicación, haga clic en Configuración, Cuota, Ejemplos y, luego, seleccione una frecuencia de muestreo y haga clic en Actualizar.](./media/sampling/data-sampling.png)

Al igual que otros tipos de muestreo, el algoritmo conserva elementos de telemetría relacionados. Por ejemplo, cuando se inspeccione la telemetría en Búsqueda, podrá buscar la solicitud relacionada con una excepción determinada. Los recuentos de métrica, como la tasa de solicitudes y la tasa de excepciones se mantienen correctamente.

Los puntos de datos que se descartan por muestreo no están disponibles en ninguna característica de Application Insights como [exportación continua](../../azure-monitor/app/export-telemetry.md).

El muestreo de ingesta no funciona mientras el muestreo adaptativo o de frecuencia fija basado en el SDK está en funcionamiento. El muestreo adaptable está habilitado de manera predeterminada cuando el SDK de ASP.NET o ASP.NET Core está habilitado en Visual Studio o habilitado en las extensiones de aplicación web de Azure, o mediante el Monitor de estado con el muestreo de ingesta deshabilitado. Si la frecuencia de muestreo en el SDK es inferior al 100 % (por ejemplo, se está realizando el muestro de elementos), se omite la frecuencia de muestreo de ingesta que establezca.

> [!WARNING]
> El valor que se muestra en el icono indica el valor que ha establecido para el muestreo de ingesta. Si el muestreo del SDK está en funcionamiento, no representa la frecuencia real de muestreo.
>
>
## <a name="sampling-for-web-pages-with-javascript"></a>Muestreo para páginas web con JavaScript
Puede configurar páginas web para el muestreo de frecuencia fija desde cualquier servidor. 

Cuando [configure las páginas web para Application Insights](../../azure-monitor/app/javascript.md), modifique el fragmento de JavaScript que obtiene del portal de Application Insights. (En las aplicaciones ASP.NET, el fragmento normalmente está en _Layout.cshtml).  Inserte una línea como `samplingPercentage: 10,` antes de la clave de instrumentación:

    <script>
    var appInsights= ... 
    }({ 


    // Value must be 100/N where N is an integer.
    // Valid examples: 50, 25, 20, 10, 5, 1, 0.1, ...
    samplingPercentage: 10, 

    instrumentationKey:...
    }); 

    window.appInsights=appInsights; 
    appInsights.trackPageView(); 
    </script> 

Para el porcentaje de muestreo, elija un porcentaje que esté cerca de 100/N, donde N es un número entero.  Actualmente el muestreo no es compatible con otros valores.

Si también habilita el muestreo de frecuencia fija en el servidor, los clientes y el servidor se sincronizarán de modo que, en Búsqueda, puede desplazarse entre las solicitudes y las vistas de página relacionadas.

## <a name="when-to-use-sampling"></a>¿Cuándo usar un muestreo?

El muestreo adaptable está habilitado automáticamente en los SDK de .NET y .NET Core más recientes. Independientemente de la versión del SDK que use, puede habilitar el muestreo de ingesta para permitir que Application Insights tome muestras de los datos recopilados.

De manera predeterminada, no hay ningún muestreo habilitado en el SDK de Java. Actualmente, solo admite el muestreo de frecuencia fija. No se admite el muestreo adaptable en el SDK de Java.

En general, para la mayoría de las aplicaciones pequeñas y medianas no es necesario realizar muestreos. La información de diagnóstico más útil y las estadísticas más precisas se obtienen mediante la recopilación de datos en todas las actividades del usuario. 

Las principales ventajas del muestreo son:

* El servicio de Application Insights descarta ("limita") puntos de datos cuando la aplicación envía un número muy elevado de telemetría en un intervalo de tiempo corto. 
* Para mantenerse dentro de la [cuota](../../azure-monitor/app/pricing.md) de puntos de datos de su plan de tarifa. 
* Para reducir el tráfico de red de la recopilación de telemetría. 

### <a name="which-type-of-sampling-should-i-use"></a>¿Qué tipo de muestreo debo utilizar?

**Use el muestreo de ingesta si:**

* Con frecuencia sobrepasa su cuota mensual de telemetría.
* Usa una versión del SDK que no admite el muestreo; por ejemplo, las versiones de ASP.NET anteriores a la 2.
* Recibe muchos datos de telemetría de los exploradores web de los usuarios.

**Use el muestreo de frecuencia fija si:**

* Usa el SDK de Application Insights para servicios web de ASP.NET versión 2.0.0 o posterior, o bien el SDK de Java v.2.0.1 o posterior.
* Quiere un muestreo sincronizado entre cliente y servidor para que, cuando investigue eventos en [Búsqueda](../../azure-monitor/app/diagnostic-search.md), pueda desplazarse entre los eventos relacionados en el cliente y el servidor, como vistas de página y solicitudes HTTP.
* Está seguro del porcentaje de muestreo adecuado para la aplicación. Debe ser lo bastante alto como para obtener métricas precisas, pero inferior a la frecuencia que supera la cuota de precios y los valores de limitación. 

**Use el muestreo adaptable si:**

Si no se cumplen las condiciones para usar las otras formas de muestreo, le recomendamos el muestreo adaptable. Esta opción está habilitada de manera predeterminada en el SDK del servidor ASP.NET o ASP.NET Core. No reducirá el tráfico hasta que se alcance un determinado índice mínimo. Por lo tanto, los sitios con poco uso no se verán afectados.

## <a name="how-do-i-know-whether-sampling-is-in-operation"></a>¿Cómo se puede saber si el muestreo está en funcionamiento?

Para conocer la frecuencia de muestreo real independientemente de dónde se ha aplicado, use una [consulta de Analytics](../../azure-monitor/app/analytics.md) como esta:

```
union requests,dependencies,pageViews,browserTimings,exceptions,traces
| where timestamp > ago(1d)
| summarize RetainedPercentage = 100/avg(itemCount) by bin(timestamp, 1h), itemType
```

Si RetainedPercentage para cualquier tipo es menor que 100,se está realizando el muestreo de ese elemento.

**Application Insights no realiza el muestreo de los contadores de sesión, métricas ni rendimiento de los tipos de telemetría en ninguna de las técnicas de muestreo descritas anteriormente. Estos tipos siempre se excluyen del muestreo, ya que la reducción en la precisión puede ser altamente indeseable para estos tipos de telemetría**.

## <a name="how-does-sampling-work"></a>¿Cómo funciona el muestreo?

El muestreo de frecuencia fija es una característica del SDK de ASP.NET de la versión 2.0.0 y posteriores y del SDK de Java de la versión 2.0.1 y posteriores. El muestreo adaptable es una característica del SDK de ASP.NET de la versión 2.0.0 y posteriores. El muestreo de ingesta es una característica del servicio de Application Insights y puede funcionar si el SDK no realiza muestreo.

El algoritmo de muestreo decide qué elementos de telemetría quitar y cuáles conservar (ya sea en el SDK o en el servicio de Application Insights). La decisión del muestreo se basa en varias reglas que tienen como objetivo conservar intactos todos los puntos de datos interrelacionados, manteniendo una experiencia de diagnóstico en Application Insights que sea práctica y confiable, incluso con un conjunto reducido de datos. Por ejemplo, si para una solicitud errónea su aplicación envía elementos de telemetría adicionales (como la excepción y los seguimientos registrados para dicha solicitud), el muestreo no dividirá esta solicitud y otra telemetría. Escogerá mantenerlos o descartarlos todos juntos Como resultado, al examinar los detalles de solicitud en Application Insights, siempre puede ver la solicitud junto con sus elementos de telemetría asociados. 

La decisión de muestreo se basa en el identificador de operación de la solicitud, lo que significa que todos los elementos de telemetría que pertenecen a una operación determinada se conservan o se descartan. Para los elementos de telemetría que no tienen establecido un identificador de operación (por ejemplo, los elementos de telemetría notificados desde subprocesos asincrónicos sin contexto http), el muestreo simplemente captura un porcentaje de elementos de telemetría de cada tipo. Antes de la versión 2.5.0-beta2 del SDK de .NET y de la versión 2.2.0-beta3 del SDK de ASP.NET Core, la decisión de muestreo se basaba en el hash del identificador de usuario de las aplicaciones que definen al "usuario" (es decir, la mayoría de las aplicaciones web más habituales). Para los tipos de aplicaciones que no definen a los usuarios (como los servicios web), la decisión de muestreo se basaba en el identificador de operación de la solicitud.

Al presentarle la telemetría, el servicio de Application Insights ajusta las métricas con el mismo porcentaje de muestreo que se usó en el momento de la recopilación, para compensar por los puntos de datos que faltan. Por lo tanto, al examinar la telemetría en Application Insights, los usuarios ven aproximaciones estadísticamente correctas que están muy próximas a los números reales.

La precisión de la aproximación depende en gran medida del porcentaje de muestreo configurado. También, la precisión aumenta en las aplicaciones que administran un gran volumen de solicitudes básicamente similares de muchos usuarios. Por otro lado, para las aplicaciones que no funcionan con una carga significativa, el muestreo no es necesario ya que estas aplicaciones normalmente pueden enviar toda su telemetría manteniéndose dentro de la cuota, sin causar pérdida de datos por causa de la limitación. 

> [!WARNING]
> Application Insights no muestrea métricas de ejemplo ni tipos de telemetría de sesiones. La reducción en la precisión puede ser altamente indeseable para estos tipos de telemetría.
> 

### <a name="adaptive-sampling"></a>muestreo adaptable

El muestreo adaptable agrega un componente que supervisa la velocidad actual de transmisión desde el SDK y ajusta el porcentaje de muestreo para mantenerse dentro de la velocidad objetivo máxima. El ajuste se actualiza a intervalos regulares y se basa en una media móvil de la velocidad de transmisión de salida.

## <a name="sampling-and-the-javascript-sdk"></a>Muestreo y el SDK de JavaScript

El SDK del lado cliente (JavaScript) participa en el muestreo de frecuencia fija junto con el SDK del lado servidor. Las páginas instrumentadas solo enviarán telemetría de cliente desde los mismos usuarios para los que el lado del servidor decidió tomar el muestreo. Esta lógica está diseñada para mantener la integridad de la sesión de usuario a través de los lados cliente y servidor. Como resultado, a partir de cualquier elemento específico de telemetría en Application Insights, puede encontrar todos los demás elementos de telemetría para ese usuario o esa sesión. 

*Las telemetrías del lado de cliente y de servidor no muestran ejemplos coordinados tal y como se describe anteriormente.*

* Compruebe que habilitó el muestreo de frecuencia fija tanto en el cliente como en el servidor.
* Asegúrese de que la versión del SDK es 2.0 o superior.
* Compruebe que establece el mismo porcentaje de muestreo en el cliente y el servidor.

### <a name="sampling-in-azure-functions"></a>Muestro en Azure Functions

Siga las instrucciones de [este artículo](https://docs.microsoft.com/azure/azure-functions/functions-monitoring#configure-sampling) para configurar el muestreo de las aplicaciones que se ejecutan en Azure Functions.

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

*¿Cuál es el comportamiento de muestreo predeterminado en el SDK de ASP.NET y ASP.NET Core?*

* Si usa una de las versiones más recientes del SDK anterior, el muestreo adaptable está habilitado de manera predeterminada con cinco elementos de telemetría por segundo.
  Hay dos elementos AdaptiveSamplingTelemetryProcessors agregados de manera predeterminada; uno incluye el tipo de evento en el muestreo y el otro excluye el tipo de evento del muestreo. Esta configuración significa que el SDK intentará limitar los elementos de telemetría a cinco elementos de telemetría de tipos de eventos y cinco elementos de telemetría de todos los demás tipos combinados, lo que garantiza que el muestreo de los eventos se realiza por separado de otros tipos de telemetría. Los eventos se suelen usar para la telemetría empresarial y es más probable que no deban verse afectados por los volúmenes de telemetría de diagnóstico.
  
  A continuación se muestra el archivo ApplicationInsights.Config predeterminado que se genera. Tal como se describió, hay dos nodos AdaptiveSamplingTelemetryProcessor independientes agregados, uno que excluye los tipos de eventos y otro que los incluye. En ASP.NET Core, el mismo comportamiento predeterminado está habilitado en el código. Use los ejemplos de la sección anterior del documento para cambiar este comportamiento predeterminado.

    ```xml
    <TelemetryProcessors>
        <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.AdaptiveSamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">
            <MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>
            <ExcludedTypes>Event</ExcludedTypes>
        </Add>
        <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.AdaptiveSamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">
            <MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>
            <IncludedTypes>Event</IncludedTypes>
        </Add>
    </TelemetryProcessors>
    ```

*¿La telemetría se puede muestrear más de una vez?*

* No. Los elementos SamplingTelemetryProcessors omiten elementos de las consideraciones de muestreo si el elemento ya se ha muestreado. Lo mismo sucede para el muestreo de ingesta, que no aplicará el muestreo a los elementos ya muestreados en el SDK en sí.

*¿Por qué realizar un muestreo no consiste simplemente en "recopilar X por ciento de cada tipo de telemetría"?*

* Aunque este método de muestreo proporcionaría un nivel de precisión alto en las aproximaciones métricas, destruiría la capacidad para poner en correlación datos de diagnóstico por usuario, sesión y solicitud, lo cual es crítico para el diagnóstico. Por lo tanto, el muestreo funciona mejor con una lógica "recopilar todos los elementos de telemetría para X por ciento de los usuarios de la aplicación", o "recopilar toda la telemetría para X por ciento de las solicitudes de aplicación". Para los elementos de telemetría no asociados con las solicitudes (por ejemplo, el procesamiento asincrónico en segundo plano), la acción de reserva es "recopilar X por ciento de todos los elementos para cada tipo de telemetría". 

*¿Se puede cambiar el porcentaje de muestreo con el tiempo?*

* Sí, el muestreo adaptable cambia gradualmente el porcentaje de muestreo, en función del volumen de datos de telemetría observado en ese momento.

*Si uso el muestreo de frecuencia fija, ¿cómo sé cuál es el porcentaje de muestreo que funcionará mejor para mi aplicación?*

* Una manera de comenzar es con muestreo adaptable, averigüe qué velocidad elige (consulte la pregunta anterior) y luego cambie a muestreo de tasa fija con esa velocidad. 
  
    Si no, lo tendrá que adivinar. Analice su uso actual de telemetría en Application Insights, observe las limitaciones que se produzcan y estime el volumen de telemetría recopilado. Estas tres entradas, junto con el plan de tarifa seleccionado, le sugerirán cuánto debería reducir el volumen de telemetría recopilado. Sin embargo, un aumento del número de usuarios o algún otro incremento en el volumen de datos de telemetría podrían provocar que la estimación no fuese válida.

*¿Qué sucede si configuro un porcentaje de muestreo demasiado bajo?*

* Un porcentaje de muestreo excesivamente bajo (muestreo demasiado drástico) reduce la precisión de las aproximaciones cuando Application Insights intenta compensar la visualización de los datos por la reducción del volumen de datos. Además, la experiencia de diagnóstico puede verse afectada negativamente, ya que algunas de las solicitudes con poca frecuencia errores o lentas pueden quedar fuera del muestreo.

*¿Qué sucede si configuro un porcentaje de muestreo demasiado alto?*

* Configurar un porcentaje de muestreo demasiado alto (no suficientemente drástico) da como resultado una reducción insuficiente del volumen de telemetría recopilada. Se pueden seguir produciendo pérdidas de datos de telemetría relacionadas con la limitación, y el costo de usar Application Insights puede ser mayor de lo planeado, debido a los cargos debidos al uso por encima del límite.

*¿En qué plataformas puedo usar muestreo?*

* El muestreo de ingesta puede producirse automáticamente cuando cualquier dato de telemetría se sitúa por encima de un determinado volumen, si el SDK no realiza muestreo. Esta configuración funcionaría, por ejemplo, si usara una versión antigua del SDK de ASP.NET o una versión anterior del SDK de Java (1.0.10 o anterior).
* Si usa el SDK de ASP.NET 2.0.0 y versiones posteriores o el SDK de ASP.NET CORE 2.2.0 y versiones posteriores (hospedado en Azure o en su propio servidor), obtendrá el muestreo adaptable de manera predeterminada, pero puede cambiar al de frecuencia fija como se describió anteriormente. Con el muestreo de frecuencia fija, el SDK del explorador se sincroniza automáticamente con los eventos relacionados con el muestreo. 
* Si usa el SDK de Java 2.0.1 o una versión posterior, puede configurar ApplicationInsights.xml para activar el muestreo de frecuencia fija. De manera predeterminada, el muestreo está desactivado. Con el muestreo de frecuencia fija, el SDK del explorador se sincroniza automáticamente con los eventos relacionados con el muestreo.

*Hay ciertos eventos excepcionales que siempre quiero ver. ¿Cómo se consigue que el módulo de muestreo los reconozca?*

* La mejor forma de lograrlo es escribir un objeto [TelemetryInitializer](../../azure-monitor/app/api-filtering-sampling.md#addmodify-properties-itelemetryinitializer) personalizado, que establece `SamplingPercentage` en 100 en el elemento de telemetría que desee retener, tal como se muestra a continuación. Cuando se garantiza que los inicializadores se ejecutan antes que los procesadores de telemetría (incluido el muestreo), se tiene la seguridad de que todas las técnicas de muestreo omitirán este elemento de cualquier consideración de muestreo.

```csharp
     public class MyTelemetryInitializer : ITelemetryInitializer
      {
         public void Initialize(ITelemetry telemetry)
        {
            if(somecondition)
            {
                ((ISupportSampling)telemetry).SamplingPercentage = 100;
            }
        }
      }
```

## <a name="next-steps"></a>Pasos siguientes

* [filtro](../../azure-monitor/app/api-filtering-sampling.md) puede proporcionar un control más estricto de los SDK que envía.
* Lea el artículo de Developer Network [Optimize Telemetry with Application Insights](https://msdn.microsoft.com/magazine/mt808502.aspx) (Optimización de la telemetría con Application Insights).
