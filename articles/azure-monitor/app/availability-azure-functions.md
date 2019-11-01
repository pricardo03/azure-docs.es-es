---
title: Creación y ejecución de pruebas de disponibilidad personalizadas mediante Azure Functions
description: En este documento se explica cómo crear una función de Azure con TrackAvailability() que se ejecutará periódicamente según la configuración especificada en la función TimerTrigger. Los resultados de esta prueba se enviarán al recurso de Application Insights, donde podrá consultar los datos de los resultados de disponibilidad y generar alertas sobre ellos. Las pruebas personalizadas le permitirán escribir pruebas de disponibilidad más complejas de las que puede crear con la interfaz de usuario del portal, supervisar una aplicación dentro de la red virtual de Azure, cambiar la dirección del punto de conexión o crear una prueba de disponibilidad si no hay ninguna disponible en su región.
ms.service: azure-monitor
ms.subservice: application-insights
ms.topic: conceptual
author: morgangrobin
ms.author: mogrobin
ms.date: 10/11/2019
ms.openlocfilehash: 900228e1f9bdf9d367fa37b9ec90a6148faec656
ms.sourcegitcommit: 7efb2a638153c22c93a5053c3c6db8b15d072949
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2019
ms.locfileid: "72880252"
---
# <a name="create-and-run-custom-availability-tests-using-azure-functions"></a>Creación y ejecución de pruebas de disponibilidad personalizadas mediante Azure Functions

En este artículo se explica cómo crear una función de Azure con TrackAvailability() que se ejecutará periódicamente según la configuración especificada en la función TimerTrigger. Los resultados de esta prueba se enviarán al recurso de Application Insights, donde podrá consultar los datos de los resultados de disponibilidad y generar alertas sobre ellos. Esto le permite crear pruebas personalizadas de forma parecida a lo que puede hacer mediante [Supervisión de la disponibilidad](../../azure-monitor/app/monitor-web-app-availability.md) en el portal. Las pruebas personalizadas le permitirán escribir pruebas de disponibilidad más complejas de las que puede crear con la interfaz de usuario del portal, supervisar una aplicación dentro de la red virtual de Azure, cambiar la dirección del punto de conexión o crear una prueba de disponibilidad incluso si esta característica no está disponible en su región.


## <a name="create-timer-triggered-function"></a>Creación de una función desencadenada por el temporizador

- Si tiene un recurso de Application Insights:
    - De forma predeterminada Azure Functions crea un recurso de Application Insights, pero si desea usar uno de los recursos que ya ha creado, deberá especificarlo durante la creación.
    - Siga las instrucciones sobre cómo [crear un recurso de Azure Functions y una función desencadenada por el temporizador](https://docs.microsoft.com/azure/azure-functions/functions-create-scheduled-function) (detener antes de la limpieza) con las siguientes opciones.
        -  Haga clic en la sección Application Insights antes de seleccionar **Crear**.

            ![ Creación de una aplicación de Azure Functions con su propio recurso de App Insights](media/availability-azure-functions/create-function-app.png)

        - Haga clic en **Seleccionar un recurso existente** y escriba el nombre del recurso. Seleccione **Aplicar**.

            ![Seleccionar recurso existente de Application Insights](media/availability-azure-functions/app-insights-resource.png)

        - Seleccione **Crear**
- Si aún no tiene un recurso de Application Insights creado para la función desencadenada por el temporizador:
    - De forma predeterminada, si crea una aplicación de Azure Functions, se creará un recurso de Application Insights.
    - Siga las instrucciones sobre cómo [crear un recurso de Azure Functions y una función desencadenada por el temporizador](https://docs.microsoft.com/azure/azure-functions/functions-create-scheduled-function) (detener antes de la limpieza).

## <a name="sample-code"></a>Código de ejemplo

Copie el código siguiente en el archivo run.csx (esto reemplazará el código ya existente). Para ello, vaya a la aplicación Azure Functions y seleccione la función de desencadenador de temporizador a la izquierda.

![Archivo run.csx de la función de Azure en Azure Portal](media/availability-azure-functions/runcsx.png)

> [!NOTE]
> Para la dirección del punto de conexión debería usar: `EndpointAddress= https://dc.services.visualstudio.com/v2/track`. A menos que el recurso se encuentre en una región como Azure Government o Azure China, en cuyo caso consulte este artículo sobre [sustitución de los puntos de conexión predeterminados](https://docs.microsoft.com/azure/azure-monitor/app/custom-endpoints#regions-that-require-endpoint-modification), seleccione el punto de conexión adecuado del canal de telemetría de su región.

```C#
using System;
using System.Diagnostics;
using System.Net.Http;
using System.Threading.Tasks;
using Microsoft.ApplicationInsights;
using Microsoft.ApplicationInsights.DataContracts;
using Microsoft.ApplicationInsights.Extensibility;
using Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Extensions.Logging;

// [CONFIGURATION_REQUIRED] configure test timeout accordingly for which your request should run
private static readonly HttpClient HttpClient = new HttpClient { Timeout = TimeSpan.FromSeconds(30) };

// The Application Insights Instrumentation Key can be changed by going to the overview page of your Function App, selecting configuration, and changing the value of the APPINSIGHTS_INSTRUMENTATIONKEY Application setting.
//DO NOT replace the code below with your instrumentation key, the key's value is pulled from the environment variable/application setting key/value pair.
private static readonly string InstrumentationKey = Environment.GetEnvironmentVariable("APPINSIGHTS_INSTRUMENTATIONKEY");

// [CONFIGURATION_REQUIRED] Configure EndpointAddress
private static readonly TelemetryConfiguration TelemetryConfiguration = new TelemetryConfiguration(InstrumentationKey, new ServerTelemetryChannel() { EndpointAddress = "<EndpointAddress>" });
private static readonly TelemetryClient TelemetryClient = new TelemetryClient(TelemetryConfiguration);

[FunctionName("Function")]
public static async void Run([TimerTrigger("0 */5 * * * *")]TimerInfo myTimer, ILogger log)
{
    log.LogInformation($"Entering Run at: {DateTime.Now}");

    if (myTimer.IsPastDue)
    {
        log.LogWarning($"[Warning]: Timer is running late! Last ran at: {myTimer.ScheduleStatus.Last}");
    }

    // [CONFIGURATION_REQUIRED] provide {testName} accordingly for your test function
    string testName = "AvailabilityTestFunction";

    // REGION_NAME is a default environment variable that comes with App Service
    string location = Environment.GetEnvironmentVariable("REGION_NAME");

    // [CONFIGURATION_REQUIRED] configure {uri} and {contentMatch} accordingly for your web app. {uri} is the website that you are testing the availability of, make sure to include http:// ot https:// in your url. If {contentMatch} is present on the page, the test will succeed, otherwise it will fail.  
    await AvailabilityTestRun(
        name: testName,
        location: location,
        uri: "<http://example.com>",
        contentMatch: "<Enter a short string of text that is present  in the body of the page your are testing>",
        log: log
    );
}

private static async Task AvailabilityTestRun(string name, string location, string uri, string contentMatch, ILogger log)
{
    log.LogInformation($"Executing availability test run for {name} at: {DateTime.Now}");

    string operationId = Guid.NewGuid().ToString("N");

    var availability = new AvailabilityTelemetry
    {
        Id = operationId,
        Name = name,
        RunLocation = location,
        Success = false
    };

    var stopwatch = new Stopwatch();
    stopwatch.Start();
    bool isMonitoringFailure = false;

    try
    {
        using (var httpResponse = await HttpClient.GetAsync(uri))
        {
            // add test results to availability telemetry property
            availability.Properties.Add("HttpResponseStatusCode", Convert.ToInt32(httpResponse.StatusCode).ToString());

            // check if response content contains specific text
            string content = httpResponse.Content != null ? await httpResponse.Content.ReadAsStringAsync() : "";
            if (httpResponse.IsSuccessStatusCode && content.Contains(contentMatch))
            {
                availability.Success = true;
                availability.Message = $"Test succeeded with response: {httpResponse.StatusCode}";
                log.LogTrace($"[Verbose]: {availability.Message}");
            }
            else if (!httpResponse.IsSuccessStatusCode)
            {
                availability.Message = $"Test failed with response: {httpResponse.StatusCode}";
                log.LogWarning($"[Warning]: {availability.Message}");
            }
            else
            {
                availability.Message = $"Test content does not contain: {contentMatch}";
                log.LogWarning($"[Warning]: {availability.Message}");
            }
        }
    }
    catch (TaskCanceledException e)
    {
        availability.Message = $"Test timed out: {e.Message}";
        log.LogWarning($"[Warning]: {availability.Message}");
    }
    catch (Exception ex)
    {
        // track exception when unable to determine the state of web app
        isMonitoringFailure = true;
        var exceptionTelemetry = new ExceptionTelemetry(ex);
        exceptionTelemetry.Context.Operation.Id = operationId;
        exceptionTelemetry.Properties.Add("TestName", name);
        exceptionTelemetry.Properties.Add("TestLocation", location);
        exceptionTelemetry.Properties.Add("TestUri", uri);
        TelemetryClient.TrackException(exceptionTelemetry);
        log.LogError($"[Error]: {ex.Message}");

        // optional - throw to fail the function
        //throw;
    }
    finally
    {
        stopwatch.Stop();
        availability.Duration = stopwatch.Elapsed;
        availability.Timestamp = DateTimeOffset.UtcNow;

        // do not make an assumption as to the state of the web app when monitoring failures occur
        if (!isMonitoringFailure)
        {
            TelemetryClient.TrackAvailability(availability);
            log.LogInformation($"Availability telemetry for {name} is sent.");
        }

        // call flush to ensure telemetries are sent
        TelemetryClient.Flush();
    }
}
```

En la derecha, debajo de Ver archivos, seleccione **Agregar**. Llame al nuevo archivo **function.proj** con la configuración siguiente.

```C#
<Project Sdk="Microsoft.NET.Sdk">
    <PropertyGroup>
        <TargetFramework>netstandard2.0</TargetFramework>
    </PropertyGroup>
    <ItemGroup>
        <PackageReference Include="Microsoft.ApplicationInsights.AspNetCore" Version="2.6.1" />
    </ItemGroup>
</Project>

```

![En la selección derecha, haga clic en Agregar. Asigne al archivo el nombre function.proj](media/availability-azure-functions/addfile.png)

## <a name="check-availability"></a>Comprobación de la disponibilidad

Para asegurarse de que todo funciona, puede examinar el gráfico de la pestaña Disponibilidad del recurso de Application Insights.

![Pestaña Disponibilidad con resultados correctos](media/availability-azure-functions/availtab.png)

Al configurar la prueba con Azure Functions observará que, a diferencia de lo que sucede con **Agregar prueba** en la pestaña Disponibilidad, el nombre de la prueba no aparecerá y no podrá interactuar con él. Los resultados se visualizan, pero se obtiene una vista de resumen en lugar de la misma vista detallada que se obtiene al crear una prueba de disponibilidad a través del portal.

Para ver los detalles de la transacción completa, seleccione **Correcto** o **Erróneo** en Aumentar detalle y, a continuación, seleccione un ejemplo. También puede obtener los detalles de la transacción de un extremo a otro seleccionando un punto de datos en el gráfico.

![Seleccionar una prueba de disponibilidad de ejemplo](media/availability-azure-functions/sample.png)

![Detalles de la transacción completa](media/availability-azure-functions/end-to-end.png)

## <a name="query-in-logs-analytics"></a>Consulta en Registros (Analytics)

Puede usar registros (Analytics) para ver los resultados de disponibilidad, dependencias, etc. Para más información acerca de los registros, visite [Introducción a las consultas de registros](../../azure-monitor/log-query/log-query-overview.md).

![Resultados de disponibilidad](media/availability-azure-functions/availabilityresults.png)

![Dependencias](media/availability-azure-functions/dependencies.png)

## <a name="next-steps"></a>Pasos siguientes

- [Mapa de aplicación](../../azure-monitor/app/app-map.md)
- [Diagnósticos de transacción](../../azure-monitor/app/transaction-diagnostics.md)
