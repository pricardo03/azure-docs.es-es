---
title: Monitor Azure Functions
description: Aprenda a usar Azure Application Insights con Azure Functions para supervisar la ejecución de la función.
services: functions
author: ggailey777
manager: jeconnoc
keywords: Azure funciones, funciones, procesamiento de eventos, webhooks, proceso dinámico, arquitectura sin servidor
ms.assetid: 501722c3-f2f7-4224-a220-6d59da08a320
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 11/15/2018
ms.author: glenga
ms.openlocfilehash: 43ac3e3cfe57ac7d6b8c575611bc4dbae3102dc5
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57439322"
---
# <a name="monitor-azure-functions"></a>Monitor Azure Functions

[Las funciones de Azure](functions-overview.md) ofrece integración incorporada con [Azure Application Insights](../azure-monitor/app/app-insights-overview.md) para supervisar las funciones. Este artículo muestra cómo configurar Azure Functions para enviar los archivos de registro generados por el sistema a Application Insights.

![Explorador de métricas de Application Insights](media/functions-monitoring/metrics-explorer.png)

Azure Functions también tiene [supervisión incorporada que no usa Application Insights](#monitoring-without-application-insights). Se recomienda Application Insights porque ofrece más datos y mejores maneras de analizar los datos.

## <a name="application-insights-pricing-and-limits"></a>Precios y límites de Application Insights

Puede probar la integración de Application Insights con instancias de Function App de forma gratuita. Hay un límite diario para la cantidad de datos se puede procesar de forma gratuita. Podría alcanzar este límite durante las pruebas. Azure proporciona notificaciones del portal y por correo electrónico cuando se aproxima al límite diario. Si se pierda las alertas y llega al límite, los nuevos registros no aparecerán en las consultas de Application Insights. Tenga en cuenta el límite para evitar el tiempo de solución de problemas innecesario. Para más información, consulte [Administración de precios y volúmenes de datos de Application Insights](../azure-monitor/app/pricing.md).

## <a name="enable-application-insights-integration"></a>Habilitación de la integración de Application Insights

Para que una aplicación de función envíe datos a Application Insights tiene que conocer la clave de instrumentación de un recurso de Application Insights. La clave tiene que estar en un valor de configuración de la aplicación llamado **APPINSIGHTS_INSTRUMENTATIONKEY**.

Puede configurar esta conexión en [Azure Portal](https://portal.azure.com):

* [Conectar automáticamente una nueva aplicación de función](#new-function-app)
* [Conectar manualmente el recurso de Application Insights](#manually-connect-an-app-insights-resource)

### <a name="new-function-app"></a>Nueva aplicación de función
<!-- Add a transitional sentence to introduce the procedure. -->

1. Vaya a la página **Crear** de la aplicación de función.

1. Establezca el conmutador **Application Insights** en **Habilitado**.

1. Seleccione una **ubicación de Application Insights**. Elija la región más cercana a la región de la aplicación de función y en un [ubicación geográfica de Azure](https://azure.microsoft.com/global-infrastructure/geographies/) donde desea almacenar los datos.

   ![Habilitar Application Insights al crear una aplicación de función](media/functions-monitoring/enable-ai-new-function-app.png)

1. Escriba otra información necesaria y seleccione **Crear**.

El paso siguiente consiste en [deshabilitar el registro integrado](#disable-built-in-logging).


<a id="manually-connect-an-app-insights-resource"></a>
### <a name="application-insights-resource"></a>Recurso de Application Insights 
<!-- Add a transitional sentence to introduce the procedure. -->

1. Cree el recurso de Application Insights. Establezca el tipo de aplicación en **General**.

   ![Crear un recurso de Application Insights del tipo General](media/functions-monitoring/ai-general.png)

1. Copie la clave de instrumentación de la página **Essentials** del recurso de Application Insights. Punto hasta el final del valor de clave mostrado para obtener un **haga clic para copiar** botón.

   ![Copiar la clave de instrumentación de Application Insights](media/functions-monitoring/copy-ai-key.png)

1. En la aplicación de función **configuración de la aplicación** página, [agregar una configuración de aplicación](functions-how-to-use-azure-function-app-settings.md#settings) seleccionando **agregar nueva configuración**. Nombre de la nueva configuración **APPINSIGHTS_INSTRUMENTATIONKEY** y pegue la clave de instrumentación copiada.

   ![Agregar clave de instrumentación a la configuración de la aplicación](media/functions-monitoring/add-ai-key.png)

1. Seleccione **Guardar**.

<!-- Before the next H2 heading, add transitional sentences to summarize why the procedures were necessary. -->

## <a name="disable-built-in-logging"></a>Deshabilitar el registro integrado

Cuando se habilita Application Insights, deshabilite la [registro integrado que usa Azure Storage](#logging-to-storage). El registro integrado es útil para realizar pruebas con cargas de trabajo ligeras, pero no está pensado para su uso en producción de carga alta. Para la supervisión de producción, se recomienda Application Insights. Si el registro integrado se usa en producción, el registro resultante podría ser incompleto debido a limitaciones en Azure Storage.

Para deshabilitar el registro integrado, elimine la configuración de la aplicación `AzureWebJobsDashboard`. Para obtener información acerca de cómo eliminar la configuración de la aplicación en Azure Portal, consulte la sección **Application settings** (Configuración de la aplicación) en [How to manage a function app](functions-how-to-use-azure-function-app-settings.md#settings) (Cómo administrar una aplicación de función). Antes de eliminar la configuración de aplicación, asegúrese de que ninguna función existente en la misma aplicación de función use la configuración de enlaces o desencadenadores de Azure Storage.

## <a name="view-telemetry-in-monitor-tab"></a>Visualización de datos de telemetría en la pestaña Supervisar

Después de configurar la integración de Application Insights como se muestra en las secciones anteriores, puede ver los datos de telemetría en el **Monitor** ficha.

1. En la página de aplicación de función, seleccione una función que se ha ejecutado al menos una vez después de que se ha configurado Application Insights. A continuación, seleccione el **Monitor** ficha.

   ![Selección de la pestaña Supervisar](media/functions-monitoring/monitor-tab.png)

1. Seleccione **actualizar** periódicamente, hasta que aparezca la lista de invocaciones de función.

   Puede tardar hasta cinco minutos para la lista aparezca mientras el cliente de telemetría crea lotes de datos para la transmisión al servidor. (No se aplica el retraso a la [Live Stream de métricas](../azure-monitor/app/live-stream.md). Este servicio se conecta al host de Functions cuando se carga la página, por lo que los registros se transmiten directamente a la página).

   ![Lista de invocaciones](media/functions-monitoring/monitor-tab-ai-invocations.png)

1. Para ver los registros de una invocación de función determinada, seleccione el vínculo de la columna **Fecha** para esa invocación.

   ![Vínculo de detalles de la invocación](media/functions-monitoring/invocation-details-link-ai.png)

   La salida del registro para esa invocación aparece en una nueva página.

   ![Detalles de la invocación](media/functions-monitoring/invocation-details-ai.png)

Vinculan las dos páginas (lista de invocación y detalles de la invocación) a la consulta de Analytics de Application Insights que recupera los datos:

![Ejecución en Application Insights](media/functions-monitoring/run-in-ai.png)

![Lista de invocación del análisis de Application Insights](media/functions-monitoring/ai-analytics-invocation-list.png)

En estas consultas, puede ver que la lista de invocación se limita a los últimos 30 días. La lista muestra no más de 20 filas (`where timestamp > ago(30d) | take 20`). La lista de detalles de invocación es para los últimos 30 días sin límite.

Para más información, consulte la sección [Consultar datos de telemetría](#query-telemetry-data) que se encuentra más adelante en este artículo.

## <a name="view-telemetry-in-application-insights"></a>Visualización de la telemetría en Application Insights

Para abrir Application Insights desde una aplicación de función en Azure portal, vaya a la aplicación de función **Introducción** página. En **configurado características**, seleccione **Application Insights**.

![Abrir Application Insights desde la página de información general de la aplicación de función](media/functions-monitoring/ai-link.png)

Para obtener más información acerca de Application Insights, consulte [Documentación de Application Insights](https://docs.microsoft.com/azure/application-insights/). En esta sección se muestran algunos ejemplos de cómo ver los datos de Application Insights. Si ya está familiarizado con Application Insights, puede ir directamente a [las secciones acerca de cómo configurar y personalizar los datos de telemetría](#configure-categories-and-log-levels).

En [Explorador de métricas](../azure-monitor/app/metrics-explorer.md), puede crear gráficos y las alertas que se basan en métricas. Las métricas incluyen el número de invocaciones de función, el tiempo de ejecución y las tasas de éxito.

![Explorador de métricas](media/functions-monitoring/metrics-explorer.png)

En la pestaña [Errores](../azure-monitor/app/asp-net-exceptions.md), puede crear alertas y gráficos basados en errores de funciones y excepciones del servidor. **Nombre de la operación** es el nombre de la función. Errores en las dependencias no se muestran a menos que implemente [telemetría personalizada](#custom-telemetry-in-c-functions) para las dependencias.

![Errores](media/functions-monitoring/failures.png)

En la pestaña [Rendimiento](../azure-monitor/app/performance-counters.md), puede analizar los problemas de rendimiento.

![Rendimiento](media/functions-monitoring/performance.png)

En la pestaña **Servidores** se muestra la utilización de recursos y el rendimiento por servidor. Estos datos pueden ser útiles para la depuración de escenarios en los que las funciones están dificultando los recursos subyacentes. Los servidores se conocen como **Instancias de rol en la nube**.

![Servidores](media/functions-monitoring/servers.png)

El [Live Stream de métricas](../azure-monitor/app/live-stream.md) pestaña muestra datos de métricas, como se crea en tiempo real.

![Live Stream](media/functions-monitoring/live-stream.png)

## <a name="query-telemetry-data"></a>Consultar datos de telemetría

[Análisis de Application Insights](../azure-monitor/app/analytics.md) proporciona acceso a todos los datos de telemetría en forma de tabla en una base de datos. Analytics ofrece un lenguaje de consulta para extraer, manipular y visualizar los datos.

![Seleccione Analytics](media/functions-monitoring/select-analytics.png)

![Ejemplo de Analytics](media/functions-monitoring/analytics-traces.png)

Este es un ejemplo de consulta que muestra la distribución de solicitudes por trabajador durante los últimos 30 minutos.

```
requests
| where timestamp > ago(30m) 
| summarize count() by cloud_RoleInstance, bin(timestamp, 1m)
| render timechart
```

Se muestran las tablas que están disponibles en el **esquema** ficha a la izquierda. Encontrará los datos generados por las invocaciones de función en las tablas siguientes:

* **traces**: Registros creados por el tiempo de ejecución y por código de función.
* **requests**: Una solicitud para cada invocación de función.
* **excepciones**: Las excepciones producidas por el tiempo de ejecución.
* **customMetrics**: El recuento de invocaciones correctas y erróneas, tasa de éxito y duración.
* **customEvents**: Eventos registrados por el tiempo de ejecución, por ejemplo: solicitudes HTTP que desencadenan una función.
* **performanceCounters**: Información sobre el rendimiento de los servidores que ejecutan las funciones en.

Las demás tablas son para pruebas de disponibilidad y telemetría de cliente y el explorador. Puede implementar telemetría personalizada para agregarles datos.

Dentro de cada tabla, algunos de los datos específicos de Functions están en un campo `customDimensions`.  Por ejemplo, la consulta siguiente recupera todos los seguimientos que tienen el nivel de registro `Error`.

```
traces 
| where customDimensions.LogLevel == "Error"
```

El tiempo de ejecución proporciona la `customDimensions.LogLevel` y `customDimensions.Category` campos. Puede proporcionar campos adicionales en los registros que se escribe en el código de función. Consulte [Registro estructurado](#structured-logging) más adelante en este artículo.

## <a name="configure-categories-and-log-levels"></a>Configurar categorías y niveles de registro

Puede usar Application Insights sin ninguna configuración personalizada. La configuración predeterminada puede dar lugar a grandes volúmenes de datos. Si usa una suscripción de Visual Studio Azure, es posible que alcance su límite de datos para Application Insights. Más adelante en este artículo, aprenderá a configurar y personalizar los datos que envían las funciones a Application Insights.

### <a name="categories"></a>Categorías

El registrador de Azure Functions incluye una *categoría* para cada registro. La categoría indica qué parte del código de tiempo de ejecución o del código de la función escribió el registro. 

El tiempo de ejecución de Functions crea registros con una categoría que comienzan por "Host". La "función iniciada", "función ejecutada" y los registros "función concluida" tienen la categoría "Host.Executor". 

Si escribe registros en el código de función, su categoría es "Function".

### <a name="log-levels"></a>Niveles de registro

El registrador de funciones de Azure también incluye un *nivel de registro* con cada registro. [LogLevel](/dotnet/api/microsoft.extensions.logging.loglevel) es una enumeración y el código entero indica la importancia relativa:

|LogLevel    |Código|
|------------|---|
|Seguimiento       | 0 |
|Depurar       | 1 |
|Información | 2 |
|Advertencia     | 3 |
|Error       | 4 |
|Crítico    | 5 |
|None        | 6 |

El nivel de registro `None` se explica en la sección siguiente. 

### <a name="log-configuration-in-hostjson"></a>Configuración del registro en host.json

El archivo [host.json](functions-host-json.md) configura la cantidad de registro que envía una aplicación de la función a Application Insights. Para cada categoría, debe indicar el nivel de registro mínimo para enviar. Hay dos ejemplos: el primer ejemplo se dirige la [funciones versión 2.x del runtime](functions-versions.md#version-2x) (.NET Core) y el segundo ejemplo es para la versión 1.x del runtime.

### <a name="version-2x"></a>Versión 2.x

El entorno de ejecución de v2.x emplea ahora la [jerarquía de filtro de registro de .NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#log-filtering). 

```json
{
  "logging": {
    "fileLoggingMode": "always",
    "logLevel": {
      "default": "Information",
      "Host.Results": "Error",
      "Function": "Error",
      "Host.Aggregator": "Trace"
    }
  }
}
```

### <a name="version-1x"></a>Versión 1.x

```json
{
  "logger": {
    "categoryFilter": {
      "defaultLevel": "Information",
      "categoryLevels": {
        "Host.Results": "Error",
        "Function": "Error",
        "Host.Aggregator": "Trace"
      }
    }
  }
}
```

En el ejemplo siguiente se configuran las reglas siguientes:

* Para los registros con la categoría `Host.Results` o `Function`, enviar sólo `Error` nivel y superiores a Application Insights. Los registros del nivel `Warning` e inferiores se omiten.
* Para los registros con la categoría `Host.Aggregator`, envíe todos los registros a Application Insights. El nivel de registro `Trace` es el mismo que algunos registradores llaman `Verbose`, pero usa `Trace` en el archivo [host.json](functions-host-json.md).
* Para el resto de registros, envíe solo el nivel `Information` y superior a Application Insights.

El valor de categoría de [host.json](functions-host-json.md) controla el registro de todas las categorías que comienzan con el mismo valor. `Host` en [host.json](functions-host-json.md) registro para los controles `Host.General`, `Host.Executor`, `Host.Results`, y así sucesivamente.

Si [host.json](functions-host-json.md) incluye varias categorías que comienzan con la misma cadena, las más largas se asignan primero. Suponga que desea que todo, desde el tiempo de ejecución, excepto `Host.Aggregator` para iniciar sesión en `Error` nivel, pero desea `Host.Aggregator` para iniciar sesión en el `Information` nivel:

### <a name="version-2x"></a>Versión 2.x 

```json
{
  "logging": {
    "fileLoggingMode": "always",
    "logLevel": {
      "default": "Information",
      "Host": "Error",
      "Function": "Error",
      "Host.Aggregator": "Information"
    }
  }
}
```

### <a name="version-1x"></a>Versión 1.x 

```json
{
  "logger": {
    "categoryFilter": {
      "defaultLevel": "Information",
      "categoryLevels": {
        "Host": "Error",
        "Function": "Error",
        "Host.Aggregator": "Information"
      }
    }
  }
}
```

Para eliminar todos los registros de una categoría, puede usar el nivel de registro `None`. No hay registros se escriben con esa categoría y no hay ningún nivel de registro por encima de él.

En las secciones siguientes se describen las categorías principales de registros que crea el tiempo de ejecución. 

### <a name="category-hostresults"></a>Categoría Host.Results

Estos registros se muestran como "requests" en Application Insights, e indican el éxito o fracaso de una función.

![Gráfico de solicitudes](media/functions-monitoring/requests-chart.png)

Todos estos registros se escriben en `Information` nivel. Si se filtra por `Warning` o versiones posteriores, no verá ninguno de estos datos.

### <a name="category-hostaggregator"></a>Categoría Host.Aggregator

Estos registros proporcionan los recuentos y promedios de las invocaciones de función en un período de tiempo [configurable](#configure-the-aggregator). El período predeterminado es 30 segundos o 1000 resultados, lo que ocurra primero. 

Los registros están disponibles en la tabla **customMetrics** de Application Insights. Algunos ejemplos son el número de ejecuciones, tasa de éxito y duración.

![Consulta customMetrics](media/functions-monitoring/custom-metrics-query.png)

Todos estos registros se escriben en `Information` nivel. Si se filtra por `Warning` o versiones posteriores, no verá ninguno de estos datos.

### <a name="other-categories"></a>Otras categorías

Todos los registros de categorías diferentes a las que ya se muestran están disponibles en la tabla **traces** en Application Insights.

![Consulta traces](media/functions-monitoring/analytics-traces.png)

Todos los registros con categorías que comienzan por `Host` escrito por el tiempo de ejecución de funciones. La "Función iniciada" y los registros "Función concluida" tienen la categoría `Host.Executor`. Para las ejecuciones correctas, estos registros son `Information` nivel. Las excepciones se registran en `Error` nivel. El tiempo de ejecución también crea registros del nivel `Warning`; por ejemplo, poner en cola los mensajes enviados a la cola de dudosos.

Los registros escritos por el código de función tienen la categoría `Function` y puede ser cualquier nivel de registro.

## <a name="configure-the-aggregator"></a>Configurar el agregador

Como se indicó en la sección anterior, el tiempo de ejecución agrega datos acerca de las ejecuciones de la función durante un período de tiempo. El período predeterminado es 30 segundos o 1000 ejecuciones, lo que ocurra primero. Puede configurar este valor en el archivo [host.json](functions-host-json.md).  Este es un ejemplo:

```json
{
    "aggregator": {
      "batchSize": 1000,
      "flushTimeout": "00:00:30"
    }
}
```

## <a name="configure-sampling"></a>Configurar el muestreo

Application Insights tiene una característica de [muestreo](../azure-monitor/app/sampling.md) que le puede ayudar a impedir que se recopilen demasiados datos de telemetría en los momentos de picos de carga. Cuando tasa de datos de telemetría supera un umbral especificado, Application Insights empieza a omitir aleatoriamente algunos de los elementos entrantes. La configuración predeterminada para el número máximo de elementos por segundo es cinco. Puede configurar el muestreo en [host.json](functions-host-json.md).  Este es un ejemplo:

### <a name="version-2x"></a>Versión 2.x 

```json
{
  "logging": {
    "applicationInsights": {
      "samplingSettings": {
        "isEnabled": true,
        "maxTelemetryItemsPerSecond" : 5
      }
    }
  }
}
```

### <a name="version-1x"></a>Versión 1.x 

```json
{
  "applicationInsights": {
    "sampling": {
      "isEnabled": true,
      "maxTelemetryItemsPerSecond" : 5
    }
  }
}
```

> [!NOTE]
> El [muestreo](../azure-monitor/app/sampling.md) está habilitado de forma predeterminada. Si parece que faltan datos, debe ajustar la configuración de muestreo para ajustarse a su escenario de supervisión determinado.

## <a name="write-logs-in-c-functions"></a>Escribir registros en funciones de C#

Puede escribir registros en el código de función que aparecen como seguimientos de Application Insights.

### <a name="ilogger"></a>ILogger

Use un parámetro [ILogger](https://docs.microsoft.com/dotnet/api/microsoft.extensions.logging.ilogger) en las funciones en lugar de un parámetro `TraceWriter`. Los registros creados mediante el uso de `TraceWriter` vaya a Application Insights, pero `ILogger` le permite hacer [registro estructurado](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).

Con un objeto `ILogger`, llamará al `Log<level>` [métodos de extensión en ILogger](https://docs.microsoft.com/dotnet/api/microsoft.extensions.logging.loggerextensions#methods) para crear registros. El código siguiente escribe `Information` registros con la categoría "Function".

```cs
public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, ILogger logger)
{
    logger.LogInformation("Request for item with key={itemKey}.", id);
```

### <a name="structured-logging"></a>Registro estructurado

El orden de los marcadores de posición, no sus nombres, determina qué parámetros se usan en el mensaje del registro. Supongamos que tiene el siguiente código:

```csharp
string partitionKey = "partitionKey";
string rowKey = "rowKey";
logger.LogInformation("partitionKey={partitionKey}, rowKey={rowKey}", partitionKey, rowKey);
```

Si mantiene la misma cadena de mensaje e invierte el orden de los parámetros, el texto del mensaje resultante tendrá los valores en los lugares incorrectos.

Los marcadores de posición se controlan de esta forma para que pueda hacer el registro estructurado. Application Insights almacena los pares de nombre-valor de parámetro y la cadena de mensaje. Como resultado, los argumentos del mensaje se convierten en campos que puede consultar.

Si la llamada al método de registrador tiene un aspecto similar al ejemplo anterior, puede consultar el campo `customDimensions.prop__rowKey`. El `prop__` se añade el prefijo para asegurarse de que no hay ninguna colisión entre los campos en el tiempo de ejecución agrega y campos en el código de función agrega.

También puede consultar la cadena del mensaje original haciendo referencia al campo `customDimensions.prop__{OriginalFormat}`.  

Esta es una representación de JSON de ejemplo de los datos `customDimensions`:

```json
{
  customDimensions: {
    "prop__{OriginalFormat}":"C# Queue trigger function processed: {message}",
    "Category":"Function",
    "LogLevel":"Information",
    "prop__message":"c9519cbf-b1e6-4b9b-bf24-cb7d10b1bb89"
  }
}
```

### <a name="custom-metrics-logging"></a>Registro de métricas personalizadas

En funciones de secuencia de comandos de C#, puede usar el método de extensión `LogMetric` en `ILogger` para crear métricas personalizadas en Application Insights. Este es un ejemplo de una llamada de método:

```csharp
logger.LogMetric("TestMetric", 1234);
```

Este código es una alternativa a llamar a `TrackMetric` utilizando [la API de Application Insights para .NET](#custom-telemetry-in-c-functions).

## <a name="write-logs-in-javascript-functions"></a>Escribir registros en las funciones de JavaScript

En las funciones de Node.js, use `context.log` para escribir los registros. No está habilitado el registro estructurado.

```
context.log('JavaScript HTTP trigger function processed a request.' + context.invocationId);
```

### <a name="custom-metrics-logging"></a>Registro de métricas personalizadas

Cuando se ejecutan [versión 1.x](functions-versions.md#creating-1x-apps) del tiempo de ejecución de funciones, pueden usar las funciones de Node.js el `context.log.metric` método para crear métricas personalizadas en Application Insights. Este método no se admite actualmente en la versión 2.x. Este es un ejemplo de una llamada de método:

```javascript
context.log.metric("TestMetric", 1234);
```

Este código es una alternativa a llamar a `trackMetric` utilizando [el SDK de Node.js de Application Insights](#custom-telemetry-in-javascript-functions).

## <a name="log-custom-telemetry-in-c-functions"></a>Telemetría personalizada de registro C# funciones

Puede usar el paquete NuGet [Microsoft.ApplicationInsights](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) para enviar datos de telemetría personalizada a Application Insights. En el siguiente ejemplo de C# se usa la [API de telemetría personalizada](../azure-monitor/app/api-custom-events-metrics.md). El ejemplo es para una biblioteca de clases. NET, pero el código de Application Insights es el mismo para la secuencia de comandos de C#.

### <a name="version-2x"></a>Versión 2.x

El entorno de ejecución de la versión 2.x usa características más recientes de Application Insights para correlacionar automáticamente la telemetría con la operación actual. No es necesario establecer manualmente la operación `Id`, `ParentId`, o `Name` campos.

```cs
using System;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.ApplicationInsights;
using Microsoft.ApplicationInsights.DataContracts;
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Extensions.Logging;

namespace functionapp0915
{
    public static class HttpTrigger2
    {
        // In Functions v2, TelemetryConfiguration.Active is initialized with the InstrumentationKey
        // from APPINSIGHTS_INSTRUMENTATIONKEY. Creating a default TelemetryClient like this will 
        // automatically use that key for all telemetry. It will also enable telemetry correlation
        // with the current operation.
        // If you require a custom TelemetryConfiguration, create it initially with
        // TelemetryConfiguration.CreateDefault() to include this automatic correlation.
        private static TelemetryClient telemetryClient = new TelemetryClient();

        [FunctionName("HttpTrigger2")]
        public static Task<IActionResult> Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", Route = null)]
            HttpRequest req, ExecutionContext context, ILogger log)
        {
            log.LogInformation("C# HTTP trigger function processed a request.");
            DateTime start = DateTime.UtcNow;

            // Parse query parameter
            string name = req.Query
                .FirstOrDefault(q => string.Compare(q.Key, "name", true) == 0)
                .Value;

            // Track an Event
            var evt = new EventTelemetry("Function called");
            evt.Context.User.Id = name;
            telemetryClient.TrackEvent(evt);

            // Track a Metric
            var metric = new MetricTelemetry("Test Metric", DateTime.Now.Millisecond);
            metric.Context.User.Id = name;
            telemetryClient.TrackMetric(metric);

            // Track a Dependency
            var dependency = new DependencyTelemetry
            {
                Name = "GET api/planets/1/",
                Target = "swapi.co",
                Data = "https://swapi.co/api/planets/1/",
                Timestamp = start,
                Duration = DateTime.UtcNow - start,
                Success = true
            };
            dependency.Context.User.Id = name;
            telemetryClient.TrackDependency(dependency);

            return Task.FromResult<IActionResult>(new OkResult());
        }
    }
}
```

### <a name="version-1x"></a>Versión 1.x

```cs
using System;
using System.Net;
using Microsoft.ApplicationInsights;
using Microsoft.ApplicationInsights.DataContracts;
using Microsoft.ApplicationInsights.Extensibility;
using Microsoft.Azure.WebJobs;
using System.Net.Http;
using System.Threading.Tasks;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Extensions.Logging;
using System.Linq;

namespace functionapp0915
{
    public static class HttpTrigger2
    {
        private static string key = TelemetryConfiguration.Active.InstrumentationKey = 
            System.Environment.GetEnvironmentVariable(
                "APPINSIGHTS_INSTRUMENTATIONKEY", EnvironmentVariableTarget.Process);

        private static TelemetryClient telemetryClient = 
            new TelemetryClient() { InstrumentationKey = key };

        [FunctionName("HttpTrigger2")]
        public static async Task<HttpResponseMessage> Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)]
            HttpRequestMessage req, ExecutionContext context, ILogger log)
        {
            log.LogInformation("C# HTTP trigger function processed a request.");
            DateTime start = DateTime.UtcNow;

            // Parse query parameter
            string name = req.GetQueryNameValuePairs()
                .FirstOrDefault(q => string.Compare(q.Key, "name", true) == 0)
                .Value;

            // Get request body
            dynamic data = await req.Content.ReadAsAsync<object>();

            // Set name to query string or body data
            name = name ?? data?.name;
         
            // Track an Event
            var evt = new EventTelemetry("Function called");
            UpdateTelemetryContext(evt.Context, context, name);
            telemetryClient.TrackEvent(evt);
            
            // Track a Metric
            var metric = new MetricTelemetry("Test Metric", DateTime.Now.Millisecond);
            UpdateTelemetryContext(metric.Context, context, name);
            telemetryClient.TrackMetric(metric);
            
            // Track a Dependency
            var dependency = new DependencyTelemetry
                {
                    Name = "GET api/planets/1/",
                    Target = "swapi.co",
                    Data = "https://swapi.co/api/planets/1/",
                    Timestamp = start,
                    Duration = DateTime.UtcNow - start,
                    Success = true
                };
            UpdateTelemetryContext(dependency.Context, context, name);
            telemetryClient.TrackDependency(dependency);
        }
        
        // Correlate all telemetry with the current Function invocation
        private static void UpdateTelemetryContext(TelemetryContext context, ExecutionContext functionContext, string userName)
        {
            context.Operation.Id = functionContext.InvocationId.ToString();
            context.Operation.ParentId = functionContext.InvocationId.ToString();
            context.Operation.Name = functionContext.FunctionName;
            context.User.Id = userName;
        }
    }    
}
```

No llame a `TrackRequest` o `StartOperation<RequestTelemetry>` ya que verá solicitudes duplicadas para una invocación de función.  El tiempo de ejecución de Functions realiza un seguimiento automático de las solicitudes.

No establezca `telemetryClient.Context.Operation.Id`. Esta configuración global que hace que la correlación incorrecta cuando muchas funciones se ejecutan simultáneamente. En su lugar, cree una instancia de telemetría nueva (`DependencyTelemetry`, `EventTelemetry`) y modifique su propiedad `Context`. Luego, pase la instancia de telemetría al método `Track` correspondiente en `TelemetryClient` (`TrackDependency()`, `TrackEvent()`). Este método garantiza que los datos de telemetría tengan los detalles de correlación correctos para la invocación de función actual.

## <a name="log-custom-telemetry-in-javascript-functions"></a>Telemetría personalizada de registro en las funciones de JavaScript

El [SDK de Application Insights Node.js](https://www.npmjs.com/package/applicationinsights) está actualmente en versión beta. Presentamos ejemplos de código que envía la telemetría personalizada para Application Insights:

```javascript
const appInsights = require("applicationinsights");
appInsights.setup();
const client = appInsights.defaultClient;

module.exports = function (context, req) {
    context.log('JavaScript HTTP trigger function processed a request.');

    client.trackEvent({name: "my custom event", tagOverrides:{"ai.operation.id": context.invocationId}, properties: {customProperty2: "custom property value"}});
    client.trackException({exception: new Error("handled exceptions can be logged with this method"), tagOverrides:{"ai.operation.id": context.invocationId}});
    client.trackMetric({name: "custom metric", value: 3, tagOverrides:{"ai.operation.id": context.invocationId}});
    client.trackTrace({message: "trace message", tagOverrides:{"ai.operation.id": context.invocationId}});
    client.trackDependency({target:"http://dbname", name:"select customers proc", data:"SELECT * FROM Customers", duration:231, resultCode:0, success: true, dependencyTypeName: "ZSQL", tagOverrides:{"ai.operation.id": context.invocationId}});
    client.trackRequest({name:"GET /customers", url:"http://myserver/customers", duration:309, resultCode:200, success:true, tagOverrides:{"ai.operation.id": context.invocationId}});

    context.done();
};
```

El `tagOverrides` conjuntos de parámetros la `operation_Id` al identificador de invocación de. la función Esta configuración le permite correlacionar toda la telemetría personalizada y generada automáticamente para una determinada invocación de función.

## <a name="known-issues"></a>Problemas conocidos
<!-- Add a transitional sentence to introduce the section. -->

### <a name="dependencies"></a>Dependencias

Las dependencias que la función tiene a otros servicios no se muestran automáticamente. Puede escribir código personalizado para mostrar las dependencias. Para obtener ejemplos, vea el código de ejemplo en el [ C# sección telemetría personalizada](#custom-telemetry-in-c-functions). El código de ejemplo da como resultado un *mapa de aplicación* en Application Insights, similar a la siguiente imagen:

![Mapa de aplicación](media/functions-monitoring/app-map.png)

### <a name="report-issues"></a>Informar de problemas

Para notificar un problema con la integración de Application Insights en Functions o para realizar una solicitud o sugerencia [cree un problema en GitHub](https://github.com/Azure/Azure-Functions/issues/new).

## <a name="monitor-without-application-insights"></a>Supervisión sin Application Insights

Se recomienda Application Insights para las funciones de supervisión. Ofrece más datos y mejores maneras de analizar los datos. Pero si prefiere que el sistema de registro integrado que usa el almacenamiento de Azure, puede usar ese método.

### <a name="azure-storage-account-for-logging"></a>Cuenta de Azure Storage para el registro

El registro integrado usa la cuenta de almacenamiento especificada por la cadena de conexión en la configuración de la aplicación `AzureWebJobsDashboard`. En la página de una aplicación de función, seleccione una función y, a continuación, seleccione el **Monitor** pestaña y elija mantener la **vista clásica**.

![Cambio a la vista clásica](media/functions-monitoring/switch-to-classic-view.png)

Obtendrá una lista de las ejecuciones de la función. Seleccione una ejecución de la función para revisar la duración, los datos de entrada, los errores y los archivos de registro asociados.

Si ha habilitado Application Insights, puede volver a usar el registro integrado. Deshabilitar Application Insights manualmente y, a continuación, seleccione el **Monitor** ficha. Para deshabilitar la integración de Application Insights, elimine el `APPINSIGHTS_INSTRUMENTATIONKEY` configuración de la aplicación.

Incluso si la pestaña **Supervisar** muestra datos de Application Insights, puede ver los datos de registro del sistema de archivos si no ha [deshabilitado el registro integrado](#disable-built-in-logging). En el recurso de almacenamiento, vaya a **archivos**y seleccione el servicio de archivos para la función. A continuación, vaya a **LogFiles** > **aplicación** > **funciones** > **función**  >  **your_function** para ver el archivo de registro.

### <a name="real-time-monitoring"></a>Supervisión en tiempo real

Puede transmitir archivos de registro a una sesión de línea de comandos en una estación de trabajo local. Use la [interfaz de línea de comandos (CLI) de Azure](/cli/azure/install-azure-cli) o [Azure PowerShell](/powershell/azure/overview).  

Para la CLI de Azure, use los siguientes comandos para iniciar sesión, elija su suscripción y los archivos de registro de flujo:

```azurecli
az login
az account list
az account set --subscription <subscriptionNameOrId>
az webapp log tail --resource-group <resource group name> --name <function app name>
```

Para Azure PowerShell, use los siguientes comandos para agregar su cuenta de Azure, elija su suscripción y transmita los archivos de registro:

```powershell
Add-AzAccount
Get-AzSubscription
Get-AzSubscription -SubscriptionName "<subscription name>" | Select-AzSubscription
Get-AzWebSiteLog -Name <function app name> -Tail
```

Para más información, vea [How to: Stream logs](../app-service/troubleshoot-diagnostic-logs.md#streamlogs) (Cómo: Transmitir registros).

### <a name="local-view-of-log-files"></a>Vista local de los archivos de registro

[!INCLUDE [functions-local-logs-location](../../includes/functions-local-logs-location.md)]

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información, consulte los siguientes recursos:

* [Application Insights](/azure/application-insights/)
* [Registro de ASP.NET Core](/aspnet/core/fundamentals/logging/)
