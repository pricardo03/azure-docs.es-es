---
title: Correlación de telemetría de Azure Application Insights | Microsoft Docs
description: Correlación de Telemetría de Application Insights
services: application-insights
documentationcenter: .net
author: lgayhardt
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 02/14/2019
ms.reviewer: sergkanz
ms.author: lagayhar
ms.openlocfilehash: d3aad8f1b032960786564bbb18f99c260fd72113
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "58092725"
---
# <a name="telemetry-correlation-in-application-insights"></a>Correlación de Telemetría en Application Insights

En el mundo de los microservicios, todas las operaciones lógicas requieren que el trabajo se realice en diversos componentes del servicio. Cada uno de estos componentes puede supervisarse por separado mediante [Azure Application Insights](../../azure-monitor/app/app-insights-overview.md). El componente de la aplicación web se comunica con el componente del proveedor de autenticación para validar las credenciales de usuario y con el componente de API para obtener datos y poder verlos. El componente de API puede consultar datos de otros servicios y usar los componentes del proveedor de caché para informar al componente de facturación sobre esta llamada. Application Insights admite la correlación de telemetría distribuida, que se usa para detectar el componente que es responsable de los errores o de una degradación del rendimiento.

Este artículo explica el modelo de datos utilizado por Application Insights para poner en correlación la telemetría enviada por varios componentes. También se explican los protocolos y las técnicas de propagación contextual, así como los conceptos de correlación en distintos lenguajes y plataformas.

## <a name="data-model-for-telemetry-correlation"></a>Modelo de datos de correlación de telemetría

Application Insights define el [modelo de datos](../../azure-monitor/app/data-model.md) para la correlación de telemetría distribuida. Para asociar la telemetría con la operación lógica, todos los elementos de telemetría tienen el campo de contexto denominado `operation_Id`. Todos los elementos de telemetría comparten este identificador en el seguimiento distribuido. De este modo, aun con la pérdida de telemetría de una sola capa, se pueden seguir asociando los datos telemétricos notificados por otros componentes.

Normalmente, una operación lógica distribuida consta de un conjunto de operaciones más pequeñas, que son solicitudes que se procesan en uno de los componentes. Estas operaciones se definen mediante [telemetría de solicitudes](../../azure-monitor/app/data-model-request-telemetry.md). Cada telemetría de solicitudes cuenta con su propio `id`, que la identifica de forma global e inequívoca. Además, todos los elementos de telemetría (como los seguimientos y las excepciones) que están asociados a dicha solicitud deben establecer `operation_parentId` en el valor de la solicitud `id`.

La [telemetría de dependencias](../../azure-monitor/app/data-model-dependency-telemetry.md) representa a todas las operaciones salientes, como una llamada HTTP a otro componente. La telemetría de dependencia también define su propio `id`, que es globalmente único. La telemetría de solicitudes, que se inicia mediante esta llamada de dependencia, utiliza este `id` como su `operation_parentId`.

Puede crear una vista de la operación lógica distribuida usando `operation_Id`, `operation_parentId` y `request.id` con `dependency.id`. Estos campos también definen el orden de causalidad de las llamadas de telemetría.

En el entorno de los microservicios, los seguimientos de componentes pueden ir a distintos elementos de almacenamiento. Cada componente puede tener su propia clave de instrumentación en Application Insights. Para obtener la telemetría de la operación lógica, es necesario consultar los datos de cada elemento de almacenamiento. Si el número de elementos de almacenamiento es enorme, necesitará una pista para saber dónde debe mirar a continuación. El modelo de datos de Application Insights define dos campos, `request.source` y `dependency.target`, para solucionar este problema. El primer campo identifica el componente que inició la solicitud de dependencia y, el segundo, el componente que devolvió la respuesta de la llamada de dependencia.

## <a name="example"></a>Ejemplo

Tomemos como ejemplo una aplicación llamada Stock Prices, en la que el precio de mercado actual de una acción se muestra utilizando una API externa denominada `Stock`. La aplicación Stock Prices tiene una página llamada `Stock page` que el explorador web del cliente abre mediante `GET /Home/Stock`. La aplicación consulta la API `GET /api/stock/value` utilizando la llamada HTTP `Stock`.

Puede analizar la telemetría resultante ejecutando una consulta:

```kusto
(requests | union dependencies | union pageViews)
| where operation_Id == "STYz"
| project timestamp, itemType, name, id, operation_ParentId, operation_Id
```

En los resultados, observe que todos los elementos de telemetría comparten la raíz `operation_Id`. Cuando se realiza una llamada Ajax desde la página, se asigna un nuevo identificador único (`qJSXU`) a la telemetría de dependencia y el identificador de pageView se usa como `operation_ParentId`. La solicitud al servidor utiliza después el identificador de Ajax como `operation_ParentId`.

| itemType   | Nombre                      | ID           | operation_ParentId | operation_Id |
|------------|---------------------------|--------------|--------------------|--------------|
| pageView   | Stock page                |              | STYz               | STYz         |
| dependency | GET /Home/Stock           | qJSXU        | STYz               | STYz         |
| request    | GET Home/Stock            | KqKwlrSt9PA= | qJSXU              | STYz         |
| dependency | GET /api/stock/value      | bBrf2L7mm2g= | KqKwlrSt9PA=       | STYz         |

Cuando se realiza la llamada `GET /api/stock/value` a un servicio externo, necesita conocer la identidad de ese servidor para poder establecer el campo `dependency.target` correctamente. Si el servicio externo no admite la supervisión, `target` se establece en el nombre de host del servicio (por ejemplo, `stock-prices-api.com`). Sin embargo, si ese servicio se identifica devolviendo un encabezado HTTP predefinido, `target` contiene la identidad de servicio que permite a Application Insights crear un seguimiento distribuido consultando la telemetría de ese servicio.

## <a name="correlation-headers"></a>Encabezados de correlación

Estamos trabajando en una propuesta de RFC para el [protocolo HTTP de correlación](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/HttpCorrelationProtocol.md). Esta propuesta define dos encabezados:

- `Request-Id`: lleva el identificador único global de la llamada.
- `Correlation-Context`: lleva la colección de pares nombre-valor de las propiedades del seguimiento distribuido.

El estándar también define dos esquemas de generación de `Request-Id`: sin formato y jerárquico. Con el esquema sin formato, se define una clave `Id` conocida para la colección `Correlation-Context`.

Application Insights define la [extensión](https://github.com/lmolkova/correlation/blob/master/http_protocol_proposal_v2.md) del protocolo HTTP de correlación. Usa pares nombre-valor `Request-Context` para propagar la colección de propiedades utilizadas por el autor o el destinatario de la llamada. El SDK de Application Insights usa este encabezado para establecer los campos `dependency.target` y `request.source`.

### <a name="w3c-distributed-tracing"></a>Seguimiento distribuido de W3C

Estamos realizando una transición al [formato de seguimiento distribuido de W3C](https://w3c.github.io/trace-context/). Define:

- `traceparent`: lleva el identificador único global de la operación y un identificador único de la llamada.
- `tracestate`: lleva un contexto específico del sistema de seguimiento.

#### <a name="enable-w3c-distributed-tracing-support-for-classic-aspnet-apps"></a>Habilitación de la compatibilidad con el seguimiento distribuido de W3C en aplicaciones clásicas de ASP.NET

Esta característica está disponible en los paquetes `Microsoft.ApplicationInsights.Web` y `Microsoft.ApplicationInsights.DependencyCollector` a partir de la versión 2.8.0-beta1.
De forma predeterminada, está deshabilitada. Para habilitarla, cambie `ApplicationInsights.config`:

- En `RequestTrackingTelemetryModule` agregue el elemento `EnableW3CHeadersExtraction` con el valor establecido en `true`.
- En `DependencyTrackingTelemetryModule` agregue el elemento `EnableW3CHeadersInjection` con el valor establecido en `true`.

#### <a name="enable-w3c-distributed-tracing-support-for-aspnet-core-apps"></a>Habilitar la compatibilidad con el seguimiento distribuido de W3C para aplicaciones de ASP.NET Core

Esta característica está en la versión 2.5.0-beta1 de `Microsoft.ApplicationInsights.AspNetCore` y en la versión 2.8.0-beta1 de `Microsoft.ApplicationInsights.DependencyCollector`.
De forma predeterminada, está deshabilitada. Para habilitarla, cambie `ApplicationInsightsServiceOptions.RequestCollectionOptions.EnableW3CDistributedTracing` a `true`:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddApplicationInsightsTelemetry(o => 
        o.RequestCollectionOptions.EnableW3CDistributedTracing = true );
    // ....
}
```

#### <a name="enable-w3c-distributed-tracing-support-for-java-apps"></a>Habilitar la compatibilidad con el seguimiento distribuido de W3C para aplicaciones de Java

- **Configuración de entrada**

  - En el caso de las aplicaciones de Java EE, agregue lo siguiente a la etiqueta `<TelemetryModules>` en el archivo ApplicationInsights.xml:

    ```xml
    <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebRequestTrackingTelemetryModule>
       <Param name = "W3CEnabled" value ="true"/>
       <Param name ="enableW3CBackCompat" value = "true" />
    </Add>
    ```
  - En el caso de las aplicaciones de Spring Boot, agregue las siguientes propiedades:

    - `azure.application-insights.web.enable-W3C=true`
    - `azure.application-insights.web.enable-W3C-backcompat-mode=true`

- **Configuración de salida**

  Agregue lo siguiente al archivo AI-Agent.xml:

  ```xml
  <Instrumentation>
    <BuiltIn enabled="true">
      <HTTP enabled="true" W3C="true" enableW3CBackCompat="true"/>
    </BuiltIn>
  </Instrumentation>
  ```

  > [!NOTE]
  > El modo de compatibilidad con versiones anteriores está habilitado de forma predeterminada, por lo que el `enableW3CBackCompat` parámetro es opcional. Solo debe utilizarlo cuando desee desactivar la compatibilidad con versiones anteriores.
  >
  > Lo ideal sería desactivar la compatibilidad cuando todos los servicios se hayan actualizado a la versión más reciente de los SDK compatibles con el protocolo W3C. Se recomienda encarecidamente que cambie a estos SDK más recientes lo antes posible.

> [!IMPORTANT]
> Asegúrese de que las configuraciones de entrada y salida sean exactamente iguales.

## <a name="opentracing-and-application-insights"></a>OpenTracing y Application Insights

La [especificación del modelo de datos de OpenTracing](https://opentracing.io/) y los modelos de datos de Application Insights se asignan de la siguiente manera:

| Application Insights                  | OpenTracing                                       |
|------------------------------------   |-------------------------------------------------  |
| `Request`, `PageView`                 | `Span` con `span.kind = server`                  |
| `Dependency`                          | `Span` con `span.kind = client`                  |
| `Id` de `Request` y `Dependency`    | `SpanId`                                          |
| `Operation_Id`                        | `TraceId`                                         |
| `Operation_ParentId`                  | `Reference` de tipo `ChildOf` (el intervalo primario)   |

Para más información, consulte [Modelo de datos de telemetría de Application Insights](../../azure-monitor/app/data-model.md). 

Para ver definiciones de los conceptos de OpenTracing, consulte [specification](https://github.com/opentracing/specification/blob/master/specification.md) y [semantic_conventions](https://github.com/opentracing/specification/blob/master/semantic_conventions.md) de OpenTracing.

## <a name="telemetry-correlation-in-net"></a>Correlación de telemetría en .NET

Con el tiempo, .NET ha establecido diversas formas para establecer correspondencias entre la telemetría y los registros de diagnósticos:

- `System.Diagnostics.CorrelationManager` permite realizar un seguimiento de [LogicalOperationStack y ActivityId](https://msdn.microsoft.com/library/system.diagnostics.correlationmanager.aspx). 
- `System.Diagnostics.Tracing.EventSource` y Seguimiento de eventos para Windows (ETW) definen el método [SetCurrentThreadActivityId](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.setcurrentthreadactivityid.aspx).
- `ILogger` usa [ámbitos de registro](https://docs.microsoft.com/aspnet/core/fundamentals/logging#log-scopes). 
- Windows Communication Foundation (WCF) y HTTP conectan la propagación del contexto "actual".

Sin embargo, esos métodos no proporcionaban compatibilidad con el seguimiento distribuido automático. `DiagnosticSource` es un mecanismo para admitir la correlación automática entre máquinas. Las bibliotecas de .NET admiten "DiagnosticSource" y permiten la propagación automática entre máquinas del contexto de correlación mediante el transporte correspondiente; por ejemplo, HTTP.

En la [guía de Activity](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/ActivityUserGuide.md) de `DiagnosticSource`, se explican los conceptos básicos de las actividades de seguimiento.

ASP.NET Core 2.0 admite la extracción de encabezados HTTP e inicia una nueva actividad.

A partir de la versión 4.1.0, `System.Net.HttpClient` permite insertar automáticamente los encabezados HTTP de correlación y realizar un seguimiento de la llamada HTTP como actividad.

Hay un nuevo módulo HTTP, [Microsoft.AspNet.TelemetryCorrelation](https://www.nuget.org/packages/Microsoft.AspNet.TelemetryCorrelation/) para ASP.NET clásico. Este módulo implementa la correlación de telemetría mediante `DiagnosticSource`. Las actividades se inician en función de los encabezados de solicitud entrantes. También establece correlación entre los datos de telemetría de las distintas fases del procesamiento de solicitudes, incluso en los casos en los que cada fase de procesamiento de Internet Information Services (IIS) se ejecuta en un subproceso administrado diferente.

A partir de la versión 2.4.0-beta1, el SDK de Application Insights utiliza `DiagnosticSource` y `Activity` para recopilar datos telemétricos y asociarlos con la actividad actual.

<a name="java-correlation"></a>
## <a name="telemetry-correlation-in-the-java-sdk"></a>Correlación de telemetría en el SDK de Java

El [SDK de Application Insights para Java](../../azure-monitor/app/java-get-started.md) permite la correlación automática de datos de telemetría desde la versión 2.0.0. Rellena automáticamente el valor de `operation_id` en todos los elementos de telemetría (seguimientos, excepciones y eventos personalizados) que se emiten en el ámbito de una solicitud. También se encarga de propagar los encabezados de correlación (descritos anteriormente) de las llamadas de un servicio de a otro mediante HTTP si el [agente del SDK de Java](../../azure-monitor/app/java-agent.md) está configurado.

> [!NOTE]
> La característica de correlación solamente admite las llamadas realizadas mediante Apache HTTPClient. Si usa Spring RestTemplate o Feign, los dos pueden utilizarse con Apache HTTPClient en segundo plano.

En la actualidad, no se admite la propagación automática de contextos entre tecnologías de mensajería (como Kafka, RabbitMQ o Azure Service Bus). Sin embargo, es posible codificar estos escenarios manualmente utilizando las API `trackDependency` y `trackRequest`. En estas API, la telemetría de dependencias representa un mensaje agregado a una cola por un productor, mientras que la solicitud representa un mensaje procesado por el consumidor. En este caso, tanto `operation_id` como `operation_parentId` deben propagarse en las propiedades del mensaje.

<a name="java-role-name"></a>
## <a name="role-name"></a>Nombre de rol

Es posible que, en ocasiones, quiera personalizar el modo en que los nombres de los componentes aparecen en el [mapa de aplicación](../../azure-monitor/app/app-map.md). Para ello, puede establecer manualmente el valor de `cloud_RoleName` a través de uno de los siguientes mecanismos:

- Si usa Spring Boot con el iniciador de Spring Boot de Application Insights, el único cambio necesario es establecer el nombre personalizado para la aplicación en el archivo application.properties.

  `spring.application.name=<name-of-app>`

  El código de inicio de Spring Boot asigna automáticamente `cloudRoleName` al valor que se especifica para la propiedad `spring.application.name`.

- Si usa `WebRequestTrackingFilter`, `WebAppNameContextInitializer` establecerá automáticamente el nombre de la aplicación. Agregue lo siguiente al archivo de configuración (ApplicationInsights.xml):

  ```XML
  <ContextInitializers>
    <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebAppNameContextInitializer" />
  </ContextInitializers>
  ```

- Si utiliza la clase de contexto en la nube:

  ```Java
  telemetryClient.getContext().getCloud().setRole("My Component Name");
  ```

## <a name="next-steps"></a>Pasos siguientes

- Escriba una [telemetría personalizada](../../azure-monitor/app/api-custom-events-metrics.md).
- Obtenga más información sobre la [opción cloud_RoleName](../../azure-monitor/app/app-map.md#set-cloud_rolename) en otros SDK.
- Incorpore todos los componentes de un microservicio en Application Insights. Consulte las [plataformas compatibles](../../azure-monitor/app/platforms.md).
- Consulte en el [modelo de datos](../../azure-monitor/app/data-model.md) los tipos de Application Insights.
- Obtenga información sobre cómo [ampliar y filtrar la telemetría](../../azure-monitor/app/api-filtering-sampling.md).
- Consulte la [referencia de configuración de Application Insights](configuration-with-applicationinsights-config.md).
