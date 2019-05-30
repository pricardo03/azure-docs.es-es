---
title: Los canales de telemetría en Azure Application Insights | Microsoft Docs
description: Cómo personalizar los canales de telemetría en el SDK de Azure Application Insights para.NET/.NET Core.
services: application-insights
documentationcenter: .net
author: cijothomas
manager: carmonm
ms.assetid: 015ab744-d514-42c0-8553-8410eef00368
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 05/14/2019
ms.reviewer: mbullwin
ms.author: cithomas
ms.openlocfilehash: fa99e29e9ec6c2bef7296a50dd381ee5fc60a1cb
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/28/2019
ms.locfileid: "66255806"
---
# <a name="telemetrychannel-in-application-insights"></a>TelemetryChannel en Application Insights

TelemetryChannel es una parte integral de [SDK de Azure Application Insights](../../azure-monitor/app/app-insights-overview.md). Administra el almacenamiento en búfer y transmisión de telemetría al servicio Application Insights. Las versiones de los SDK de .NET y .NET Core tienen dos TelemetryChannels integrados - `InMemoryChannel` y `ServerTelemetryChannel`. En este artículo se describe cada canal con detalle, incluido cómo los usuarios pueden personalizar el comportamiento de canal.

## <a name="what-is-a-telemetrychannel"></a>¿Qué es un TelemetryChannel?

`TelemetryChannel` es responsable de almacenamiento en búfer y enviar elementos de telemetría al servicio Application Insights, donde se almacena para consultas y análisis. Es cualquier clase que implementa la interfaz [`Microsoft.ApplicationInsights.ITelemetryChannel`](https://docs.microsoft.com/dotnet/api/microsoft.applicationinsights.channel.itelemetrychannel?view=azure-dotnet)

El `Send(ITelemetry item)` se llama después de todo el método de TelemetryChannel `TelemetryInitializer`s y `TelemetryProcessor`s se denominan. Esto significa que todos los elementos quitados por `TelemetryProcessor` no alcanzarán el canal. `Send()` no suelen enviar los elementos al instante en el back-end. Son normalmente almacenado en búfer en memoria y envían en lotes para su transmisión eficaz.

[LiveMetrics](live-stream.md) también tiene un canal personalizado, que permite la transmisión por secuencias en directo de telemetría. Este canal es independiente del canal de telemetría regular y, en este documento no se aplica al canal utilizado por `LiveMetrics`.

## <a name="built-in-telemetrychannels"></a>TelemetryChannels integrados

SDK de Application Insights.NET/.NET Core se distribuye con dos canales integrados:

* **InMemoryChannel** 
 `InMemoryChannel` es un canal de peso ligero, que se almacena en búfer los elementos en la memoria hasta que se envía. Elementos que están almacenados en búfer en memoria y vaciados una vez cada 30 segundos o cada vez que hayan almacenado en búfer de 500 elementos. Este canal ofrece garantías de confiabilidad mínimo como no reintentos de envío de telemetría tras errores. Este canal no mantiene los elementos en el disco, por lo que todos los elementos no enviados se pierden permanentemente al cierre de la aplicación (correctamente o no). Hay un `Flush()` método implementado por este canal, que puede usarse para forzar vaciado de todos los elementos de telemetría en memoria sincrónicamente. Esto es ideal para aplicaciones de ejecución breve donde un vaciado sincrónico es ideal.

    Este canal se distribuye como parte de la `Microsoft.ApplicationInsights` propio paquete nuget y es el canal predeterminado que usa el SDK cuando se configura nada más.

* **ServerTelemetryChannel** 
 `ServerTelemetryChannel` es un canal más avanzado, que tiene directivas de reintento y la capacidad para almacenar datos en el disco local. Este canal reintenta el envío de telemetría, si se producen errores transitorios. Este canal también usa el almacenamiento en disco local para mantener los elementos en el disco durante las interrupciones de red o volúmenes de telemetría alta. Debido a estos mecanismos de reintento y el almacenamiento en disco local, este canal se considera más confiable y se recomienda para todos los escenarios de producción. Este canal es el valor predeterminado para [ASP.NET](https://docs.microsoft.com/azure/azure-monitor/app/asp-net) y [ASP.NET Core](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core) aplicaciones que estén configuradas según la documentación oficial vinculada. Este canal está optimizado para escenarios de servidor de procesos de larga ejecución. El [ `Flush()` ](#which-channel-should-i-use) método implementado por este canal no está sincronizado.

    Este canal se distribuye como el paquete NuGet `Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel`y se pone automáticamente cuando se usa cualquiera de los paquetes de NuGet `Microsoft.ApplicationInsights.Web` o `Microsoft.ApplicationInsights.AspNetCore`.

## <a name="configuring-telemetrychannel"></a>Configurar TelemetryChannel

Canal de telemetría se puede configurar estableciendo deseado `TelemetryChannel` en activo `TelemetryConfiguration`. Para las aplicaciones Asp.Net, la configuración implica configuración `TelemetryChannel` en `TelemetryConfiguration.Active`, o modificar `ApplicationInsights.config`. Para las aplicaciones de ASP.NET Core, configuración implica la adición del canal deseado para el contenedor de inserción de dependencia.

A continuación muestra un ejemplo donde el usuario está configurando el `StorageFolder` para el canal. `StorageFolder` es solo uno de los valores configurables. Se describe la lista completa de opciones de configuración [en la sección configuración](telemetry-channels.md#configurable-settings-in-channel).

## <a name="configuration-using-applicationinsightsconfig-for-aspnet-applications"></a>Configuración con ApplicationInsights.Config para aplicaciones de ASP.NET

La siguiente sección de [ApplicationInsights.config](configuration-with-applicationinsights-config.md) muestra ServerTelemetryChannel configurado con `StorageFolder` establecido en una ubicación personalizada.

```xml
    <TelemetrySinks>
        <Add Name="default">
            <TelemetryProcessors>
                <!--TelemetryProcessors omitted for brevity  -->
            </TelemetryProcessors>
            <TelemetryChannel Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.ServerTelemetryChannel, Microsoft.AI.ServerTelemetryChannel">
                <StorageFolder>d:\temp\applicationinsights</StorageFolder>
            </TelemetryChannel>
        </Add>
    </TelemetrySinks>
```

## <a name="configuration-in-code-for-aspnet-applications"></a>Configuración en el código para las aplicaciones ASP.NET

El código siguiente configura ServerTelemetryChannel con `StorageFolder` establecido en una ubicación personalizada. Este código debe agregarse al principio de la aplicación, normalmente en el método Application_Start) `Global.aspx.cs`

```csharp
using Microsoft.ApplicationInsights.Extensibility;
using Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel;
protected void Application_Start()
{
    var serverTelemetryChannel = new ServerTelemetryChannel();
    serverTelemetryChannel.StorageFolder = @"d:\temp\applicationinsights";
    serverTelemetryChannel.Initialize(TelemetryConfiguration.Active);
    TelemetryConfiguration.Active.TelemetryChannel = serverTelemetryChannel;
}
```

## <a name="configuration-in-code-for-aspnet-core-applications"></a>Configuración en el código para aplicaciones ASP.NET Core

Modificar `ConfigureServices` método `Startup.cs` clase tal como se muestra a continuación.

```csharp
using Microsoft.ApplicationInsights.Channel;
using Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel;

public void ConfigureServices(IServiceCollection services)
{
    // This sets up ServerTelemetryChannel with StorageFolder set to a custom location.
    services.AddSingleton(typeof(ITelemetryChannel), new ServerTelemetryChannel() {StorageFolder = @"d:\temp\applicationinsights" });

    services.AddApplicationInsightsTelemetry();
}

```

> [!NOTE]
> Es importante tener en cuenta que el canal mediante la configuración `TelemetryConfiguration.Active` no se recomienda para las aplicaciones ASP.NET Core.

## <a name="configuring-telemetrychannel-in-code-for-netnet-core-console-applications"></a>Configurar TelemetryChannel en el código para aplicaciones de consola de.NET/.NET Core

Para las aplicaciones de consola, el código es el mismo para .NET y .NET Core y, a continuación se muestra.

```csharp
var serverTelemetryChannel = new ServerTelemetryChannel();
serverTelemetryChannel.StorageFolder = @"d:\temp\applicationinsights";
serverTelemetryChannel.Initialize(TelemetryConfiguration.Active);
TelemetryConfiguration.Active.TelemetryChannel = serverTelemetryChannel;
```

## <a name="operational-details-of-servertelemetrychannel"></a>Detalles operativos de ServerTelemetryChannel

El `ServerTelemetryChannel` almacena en búfer los elementos que llegan en un búfer en memoria. Se serializa, comprimido y almacenado en `Transmission` instancia una vez cada 30 segundos o cuando 500 elementos se almacenan en búfer. Una sola `Transmission` instancia contiene hasta 500 elementos y representa un lote de datos de telemetría que se envían a través de una llamada https único al servicio Application Insights. De forma predeterminada, puede haber un máximo de 10 `Transmission`s que se envían en paralelo. Si llegan datos de telemetría a velocidades más rápidas o back-end de red/Application Insights es lenta, a continuación, `Transmission`s se almacenan en la memoria. La capacidad predeterminada de este búfer de transmisión en memoria es 5 MB. Una vez que se supera la capacidad de memoria, `Transmission`se almacenan en el disco local de hasta 50 MB. `Transmission`se almacenan en el disco local cuando hay problemas de red también. Solo los elementos almacenados en el disco local sobrevivir a un bloqueo de la aplicación, que se envían cada vez que se vuelva a iniciar la aplicación.

## <a name="configurable-settings-in-channel"></a>Valores configurables en el canal

La lista completa de opciones configurables para cada canal están aquí:

[InMemoryChannel](https://github.com/microsoft/ApplicationInsights-dotnet/blob/develop/src/Microsoft.ApplicationInsights/Channel/InMemoryChannel.cs)

[ServerTelemetryChannel](https://github.com/microsoft/ApplicationInsights-dotnet/blob/develop/src/ServerTelemetryChannel/ServerTelemetryChannel.cs)

Suele utilizarse para la configuración de `ServerTelemetryChannel` se enumeran a continuación:

1. `MaxTransmissionBufferCapacity` -Cantidad máxima de memoria, en bytes, utilizado por el canal para las transmisiones de búfer en memoria. Una vez que se alcanza esta capacidad, se almacenarán los nuevos elementos directamente en el disco local. El valor predeterminado es 5 MB. Establecer un valor mayor de clientes potenciales a menor uso de disco, pero es importante tener en cuenta que los elementos en la memoria se perderán si se bloquea la aplicación.

2. `MaxTransmissionSenderCapacity` -Cantidad máxima de `Transmission`s que se enviará a Application Insights al mismo tiempo. El valor predeterminado es 10, pero se puede configurar en un número mayor. Esto se recomienda cuando se genera un gran volumen de telemetría, por lo general, al realizar pruebas de carga o cuando se desactiva el muestreo.

3. `StorageFolder` -La carpeta para almacenar elementos en el disco según sea necesario utilizada el canal. En Windows, se usa % LocalAppData % o % Temp %, si se configura nada explícitamente. En entornos que no sean Windows, usuario **debe** configurar una ubicación válida, sin que la telemetría no se almacenará en el disco local.

## <a name="which-channel-should-i-use"></a>¿Canal que se debe usar?

`ServerTelemetryChannel` se recomienda para la mayoría de escenarios de producción de aplicaciones de larga ejecución. El `Flush()` implementación del método de `ServerTelemetryChannel` no es sincrónico, y `Flush()` no garantiza el envío de todos los elementos pendientes de memoria/disco. Si este canal se usa en escenarios donde la aplicación está a punto de apagar y, después, se recomienda realizar cierto retraso después de llamar a `Flush()` en este canal. La cantidad exacta de retraso necesario no es predecible, ya que depende de factores como el número de elementos o `Transmissions` están en memoria, cuántos se encuentran en disco, cuántos se están transmitiendo a seguridad, y si el canal está en medio de escenarios de retroceso exponenciales. Si es necesario para realizar el vaciado sincrónica, a continuación, `InMemoryChannel` se recomienda.

## <a name="frequently-asked-questions"></a>Preguntas frecuentes

### <a name="does-applicationinsights-channel-offer-guaranteed-telemetry-delivery-or-what-are-the-scenarios-where-telemetry-can-be-lost"></a>*¿Canal de Application Insights ofrece entrega garantizada de telemetría o cuáles son los escenarios donde se pueden perder datos de telemetría?*

* Respuesta corta es que ninguno de los canales integrados ofrecen garantía de tipo de transacción sobre la entrega de datos de telemetría para el back-end. Mientras `ServerTelemetryChannel` es más avanzado comparado con `InMemoryChannel` para la entrega confiable de telemetría, también realiza un intento de mejor esfuerzo para enviar telemetría y todavía se pueden perder datos de telemetría en varios escenarios. Algunos de los escenarios comunes donde se pierde la telemetría son:

1. Elementos en la memoria se pierden cada vez que la aplicación se bloquea.
1. Datos de telemetría se almacenan en el disco local durante las interrupciones de red o problemas con el back-end de Application Insights. Sin embargo, se descartan los elementos anteriores a 24 horas. Por lo que la telemetría se pierde durante un período prolongado de problemas de red.
1. Las ubicaciones predeterminadas de disco para almacenar datos de telemetría de Windows son % LocalAppData % o % Temp %. Estas ubicaciones están normalmente locales en el equipo. Si la aplicación se migra físicamente desde una ubicación a otra, se pierde cualquier telemetría almacenado en esta ubicación.
1. En Azure Web Apps (Windows), la ubicación del disco predeterminado es "D:\local\LocalAppData". Esta ubicación no se conserva y se borre de reinicios de aplicación, el escalado de trabajos impresos y así sucesivamente, dando lugar a pérdida de datos de telemetría almacenado en esas ubicaciones. Los usuarios pueden invalidar el almacenamiento en una ubicación persistente como "D:\home", pero estas ubicaciones persistentes debajo de la directiva se sirven mediante almacenamiento remoto y pueden ser lentas.

### <a name="does-servertelemetrychannel-work-in-non-windows-systems"></a>*¿Funciona en sistemas que no sean Windows ServerTelemetryChannel?*

* A pesar del nombre del paquete o espacio de nombres que se va a Windows Server, este canal se admite en sistemas que no son Windows con la siguiente excepción. En el que no son de Windows, el canal no crea una carpeta de almacenamiento local de forma predeterminada. Los usuarios deben crear una carpeta de almacenamiento local y configurar el canal para que lo utilicen. Una vez configurado el almacenamiento local, el canal funciona igual en Windows y sistemas que no sean Windows.

### <a name="does-the-sdk-create-temporary-local-storage-is-the-data-encrypted-at-storage"></a>*¿El SDK crea el almacenamiento local temporal? ¿Se cifran los datos en almacenamiento?*

* SDK almacena elementos de telemetría en el almacenamiento local durante la problemas de red o de limitación. Estos datos no se cifran localmente.
Para los sistemas Windows, el SDK automáticamente crea una carpeta temporal local en el directorio TEMP o APPDATA y restringe el acceso a los administradores y sólo el usuario actual.
Para que no son de Windows, almacenamiento local no se crea automáticamente mediante el SDK y, por tanto, no hay datos se almacenan localmente de forma predeterminada. Los usuarios pueden crear un directorio de almacenamiento por sí mismos y configurar el canal para que lo utilicen. En este caso, el usuario es responsable de garantizar que este directorio está protegido.
Obtenga más información sobre [protección de datos y privacidad](data-retention-privacy.md#does-the-sdk-create-temporary-local-storage).

## <a name="open-source-sdk"></a>SDK de código abierto
Al igual que todos los SDK de Application Insights, los canales también son código abierto. Leer y contribuir al código o notificar problemas a [el repositorio GitHub oficial](https://github.com/Microsoft/ApplicationInsights-dotnet).

## <a name="next-steps"></a>Pasos siguientes

* [Muestreo](../../azure-monitor/app/sampling.md)
* [Solución de problemas de SDK](../../azure-monitor/app/asp-net-troubleshoot-no-data.md)
