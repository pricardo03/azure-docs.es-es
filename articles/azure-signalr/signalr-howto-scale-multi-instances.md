---
title: Cómo escalar con varias instancias de Azure SignalR Service
description: En muchos escenarios de escalado, el cliente necesita a menudo aprovisionar varias instancias y configurar para poder utilizarlos juntos, para crear una implementación a gran escala. Por ejemplo, particionamiento requiere compatibilidad con varias instancias.
author: sffamily
ms.service: signalr
ms.topic: conceptual
ms.date: 03/27/2019
ms.author: zhshang
ms.openlocfilehash: e284a0492774e02cab79db6d9006c1718a7fcfc9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60809145"
---
# <a name="how-to-scale-signalr-service-with-multiple-instances"></a>¿Cómo escalar SignalR Service con varias instancias?
El SDK más reciente del servicio SignalR admite varios puntos de conexión para las instancias de SignalR Service. Puede usar esta característica para escalar las conexiones simultáneas, o usarlo para la mensajería entre regiones.

## <a name="for-aspnet-core"></a>Para ASP.NET Core

### <a name="how-to-add-multiple-endpoints-from-config"></a>¿Cómo agregar varios puntos de conexión desde la configuración?

La configuración con la clave `Azure:SignalR:ConnectionString` o `Azure:SignalR:ConnectionString:` para la cadena de conexión de SignalR Service.

Si la clave empieza por `Azure:SignalR:ConnectionString:`, debe estar en formato `Azure:SignalR:ConnectionString:{Name}:{EndpointType}`, donde `Name` y `EndpointType` son propiedades de la `ServiceEndpoint` de objetos y son accesibles desde el código.

Puede agregar varias cadenas de conexión de la instancia con los siguientes `dotnet` comandos:

```batch
dotnet user-secrets set Azure:SignalR:ConnectionString:east-region-a <ConnectionString1>
dotnet user-secrets set Azure:SignalR:ConnectionString:east-region-b:primary <ConnectionString2>
dotnet user-secrets set Azure:SignalR:ConnectionString:backup:secondary <ConnectionString3>
```

### <a name="how-to-add-multiple-endpoints-from-code"></a>¿Cómo agregar varios puntos de conexión desde el código?

Un `ServicEndpoint` clase se introdujo para describir las propiedades de un punto de conexión de Azure SignalR Service.
Puede configurar varios puntos de conexión de instancia cuando se usa el SDK de Azure SignalR Service a través de:
```cs
services.AddSignalR()
        .AddAzureSignalR(options => 
        {
            options.Endpoints = new ServiceEndpoint[]
            {
                // Note: this is just a demonstration of how to set options.Endpoints
                // Having ConnectionStrings explicitly set inside the code is not encouraged
                // You can fetch it from a safe place such as Azure KeyVault
                new ServiceEndpoint("<ConnectionString0>"),
                new ServiceEndpoint("<ConnectionString1>", type: EndpointType.Primary, name: "east-region-a"),
                new ServiceEndpoint("<ConnectionString2>", type: EndpointType.Primary, name: "east-region-b"),
                new ServiceEndpoint("<ConnectionString3>", type: EndpointType.Secondary, name: "backup"),
            };
        });
```

### <a name="how-to-customize-endpoint-router"></a>¿Cómo personalizar el enrutador de punto de conexión?

De forma predeterminada, el SDK usa el [DefaultEndpointRouter](https://github.com/Azure/azure-signalr/blob/dev/src/Microsoft.Azure.SignalR/EndpointRouters/DefaultEndpointRouter.cs) para recoger los puntos de conexión.

#### <a name="default-behavior"></a>Comportamiento predeterminado 
1. Enrutamiento de solicitud de cliente

    Cuando cliente `/negotiate` con el servidor de aplicaciones. De forma predeterminada, el SDK **selecciona aleatoriamente** un punto de conexión del conjunto de puntos de conexión de servicio disponibles.

2. Enrutamiento de mensajes de servidor

    Cuando * enviar mensaje a un determinado ** conexión *** y la conexión de destino se enruta al servidor actual, el mensaje va directamente a ese extremo conectado. En caso contrario, los mensajes se difunden a cada punto de conexión de Azure SignalR.

#### <a name="customize-routing-algorithm"></a>Personalizar el algoritmo de enrutamiento
Puede crear su propio enrutador cuando tenga un conocimiento especial para identificar los mensajes deben ir a qué puntos de conexión.

Un enrutador personalizado se define a continuación, por ejemplo cuando grupos que empiecen por `east-` siempre vaya al punto de conexión denominado `east`:

```cs
private class CustomRouter : EndpointRouterDecorator
{
    public override IEnumerable<ServiceEndpoint> GetEndpointsForGroup(string groupName, IEnumerable<ServiceEndpoint> endpoints)
    {
        // Override the group broadcast behavior, if the group name starts with "east-", only send messages to endpoints inside east
        if (groupName.StartsWith("east-"))
        {
            return endpoints.Where(e => e.Name.StartsWith("east-"));
        }

        return base.GetEndpointsForGroup(groupName, endpoints);
    }
}
```

A continuación, otro ejemplo que reemplaza el valor predeterminado negociar el comportamiento seleccionar los puntos de conexión depende de dónde se encuentra el servidor de aplicaciones.

```cs
private class CustomRouter : EndpointRouterDecorator
{
    public override ServiceEndpoint GetNegotiateEndpoint(HttpContext context, IEnumerable<ServiceEndpoint> endpoints)
    {
        // Override the negotiate behavior to get the endpoint from query string
        var endpointName = context.Request.Query["endpoint"];
        if (endpointName.Count == 0)
        {
            context.Response.StatusCode = 400;
            var response = Encoding.UTF8.GetBytes("Invalid request");
            context.Response.Body.Write(response, 0, response.Length);
            return null;
        }

        return endpoints.FirstOrDefault(s => s.Name == endpointName && s.Online) // Get the endpoint with name matching the incoming request
               ?? base.GetNegotiateEndpoint(context, endpoints); // Or fallback to the default behavior to randomly select one from primary endpoints, or fallback to secondary when no primary ones are online
    }
}
```

No olvide registrar el enrutador para el contenedor de DI mediante:

```cs
services.AddSingleton(typeof(IEndpointRouter), typeof(CustomRouter));
services.AddSignalR()
        .AddAzureSignalR(
            options => 
            {
                options.Endpoints = new ServiceEndpoint[]
                {
                    new ServiceEndpoint(name: "east", connectionString: "<connectionString1>"),
                    new ServiceEndpoint(name: "west", connectionString: "<connectionString2>"),
                    new ServiceEndpoint("<connectionString3>")
                };
            });
```

## <a name="for-aspnet"></a>Para ASP.NET

### <a name="how-to-add-multiple-endpoints-from-config"></a>¿Cómo agregar varios puntos de conexión desde la configuración?

La configuración con la clave `Azure:SignalR:ConnectionString` o `Azure:SignalR:ConnectionString:` para la cadena de conexión de SignalR Service.

Si la clave empieza por `Azure:SignalR:ConnectionString:`, debe estar en formato `Azure:SignalR:ConnectionString:{Name}:{EndpointType}`, donde `Name` y `EndpointType` son propiedades de la `ServiceEndpoint` de objetos y son accesibles desde el código.

Puede agregar varias cadenas de conexión de instancia a `web.config`:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <connectionStrings>
    <add name="Azure:SignalR:ConnectionString" connectionString="<ConnectionString1>"/>
    <add name="Azure:SignalR:ConnectionString:en-us" connectionString="<ConnectionString2>"/>
    <add name="Azure:SignalR:ConnectionString:zh-cn:secondary" connectionString="<ConnectionString3>"/>
    <add name="Azure:SignalR:ConnectionString:Backup:secondary" connectionString="<ConnectionString4>"/>
  </connectionStrings>
  ...
</configuration>
```

### <a name="how-to-add-multiple-endpoints-from-code"></a>¿Cómo agregar varios puntos de conexión desde el código?

Un `ServicEndpoint` clase se introdujo para describir las propiedades de un punto de conexión de Azure SignalR Service.
Puede configurar varios puntos de conexión de instancia cuando se usa el SDK de Azure SignalR Service a través de:

```cs
app.MapAzureSignalR(
    this.GetType().FullName, 
    options => {
            options.Endpoints = new ServiceEndpoint[]
            {
                // Note: this is just a demonstration of how to set options.Endpoints
                // Having ConnectionStrings explicitly set inside the code is not encouraged
                // You can fetch it from a safe place such as Azure KeyVault
                new ServiceEndpoint("<ConnectionString1>"),
                new ServiceEndpoint("<ConnectionString2>"),
                new ServiceEndpoint("<ConnectionString3>"),
            }
        });
```

### <a name="how-to-customize-router"></a>¿Cómo personalizar el enrutador?

La única diferencia entre ASP.NET SignalR y ASP.NET Core SignalR es el tipo de contexto http para `GetNegotiateEndpoint`. Para SignalR de ASP.NET, es de [IOwinContext](https://github.com/Azure/azure-signalr/blob/dev/src/Microsoft.Azure.SignalR.AspNet/EndpointRouters/DefaultEndpointRouter.cs#L19) tipo.

A continuación es personalizado negociar ejemplo para ASP.NET SignalR:

```cs
private class CustomRouter : EndpointRouterDecorator
{
    public override ServiceEndpoint GetNegotiateEndpoint(IOwinContext context, IEnumerable<ServiceEndpoint> endpoints)
    {
        // Override the negotiate behavior to get the endpoint from query string
        var endpointName = context.Request.Query["endpoint"];
        if (string.IsNullOrEmpty(endpointName))
        {
            context.Response.StatusCode = 400;
            context.Response.Write("Invalid request.");
            return null;
        }

        return endpoints.FirstOrDefault(s => s.Name == endpointName && s.Online) // Get the endpoint with name matching the incoming request
               ?? base.GetNegotiateEndpoint(context, endpoints); // Or fallback to the default behavior to randomly select one from primary endpoints, or fallback to secondary when no primary ones are online
    }
}
```

No olvide registrar el enrutador para el contenedor de DI mediante:

```cs
var hub = new HubConfiguration();
var router = new CustomRouter();
hub.Resolver.Register(typeof(IEndpointRouter), () => router);
app.MapAzureSignalR(GetType().FullName, hub, options => {
    options.Endpoints = new ServiceEndpoint[]
                {
                    new ServiceEndpoint(name: "east", connectionString: "<connectionString1>"),
                    new ServiceEndpoint(name: "west", connectionString: "<connectionString2>"),
                    new ServiceEndpoint("<connectionString3>")
                };
});
```

## <a name="configuration-in-cross-region-scenarios"></a>Configuración en escenarios entre regiones

El `ServiceEndpoint` objeto tiene un `EndpointType` propiedad con valor `primary` o `secondary`.

`primary` los puntos de conexión son puntos de conexión preferidos para recibir el tráfico de cliente y se consideran que tienen conexiones de red más confiables; `secondary` puntos de conexión se considera que tienen menos confiables conexiones de red y se usan solo para el servidor de la toma de al tráfico del cliente, por ejemplo, difundir mensajes, pero no para la toma de cliente para el tráfico del servidor.

En los casos entre regiones, la red puede ser inestable. Para el servidor de una aplicación que se encuentra en *East US*, el punto de conexión de SignalR Service ubicada en el mismo *East US* región puede configurarse como `primary` y puntos de conexión en otras regiones marcan como `secondary`. En esta configuración, los puntos de conexión de servicio en otras regiones pueden **recibir** mensajes desde este *East US* del servidor de aplicaciones, pero habrá ninguna **entre regiones** enrutan los clientes a Este servidor de aplicaciones. La arquitectura se muestra en el diagrama siguiente:

![Geográfica cruzada Infra](./media/signalr-howto-scale-multi-instances/cross_geo_infra.png)

Cuando un cliente intenta `/negotiate` con el servidor de aplicaciones, con el enrutador predeterminado, el SDK **selecciona aleatoriamente** un punto de conexión del conjunto de disponibilidad `primary` puntos de conexión. Cuando el punto de conexión está disponible, SDK, a continuación, **selecciona aleatoriamente** desde todos los disponibles `secondary` puntos de conexión. El punto de conexión está marcada como **disponibles** cuando la conexión entre el servidor y el punto de conexión de servicio está activa.

En el escenario entre regiones, cuando un cliente intenta `/negotiate` con el servidor de aplicaciones hospedada en *East US*, de manera predeterminada, siempre devuelve el `primary` punto de conexión que se encuentra en la misma región. Cuando todos los *East US* puntos de conexión no están disponibles, el cliente se redirige a puntos de conexión en otras regiones. Sección de conmutación por error a continuación describe detalladamente el escenario.

![Negociar normal](./media/signalr-howto-scale-multi-instances/normal_negotiate.png)

## <a name="fail-over"></a>Conmutación por error

Cuando todos los `primary` no están disponibles, los puntos de conexión del cliente `/negotiate` elige uno de los contadores `secondary` puntos de conexión. Este mecanismo de conmutación por error requiere que cada punto de conexión debe actuar como `primary` punto de conexión al servidor de al menos una aplicación.

![Conmutación por error](./media/signalr-howto-scale-multi-instances/failover_negotiate.png)

## <a name="next-steps"></a>Pasos siguientes

En esta guía, ha aprendido acerca de cómo configurar varias instancias en la misma aplicación para escenarios entre regiones, particionamiento y escalado.

Admite varios de los puntos de conexión también puede usarse en escenarios de recuperación ante desastres y disponibilidad alta.

> [!div class="nextstepaction"]
> [El programa de instalación de SignalR Service para la recuperación ante desastres y alta disponibilidad](./signalr-concept-disaster-recovery.md)
