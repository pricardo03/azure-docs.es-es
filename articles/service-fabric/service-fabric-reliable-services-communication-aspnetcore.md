---
title: Comunicación de servicio con ASP.NET Core | Microsoft Docs
description: Aprenda a usar ASP.NET Core en Reliable Services con y sin estado.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: chackdan
editor: ''
ms.assetid: 8aa4668d-cbb6-4225-bd2d-ab5925a868f2
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 10/12/2018
ms.author: vturecek
ms.openlocfilehash: 638c06e1854504dcb7ff34b1d9df56694556c421
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2019
ms.locfileid: "64939786"
---
# <a name="aspnet-core-in-azure-service-fabric-reliable-services"></a>ASP.NET Core en Reliable Services de Azure Service Fabric

ASP.NET Core es un marco de código abierto y multiplataforma. Este marco de trabajo está diseñado para crear aplicaciones basadas en la nube, conectado a internet, como aplicaciones web, aplicaciones de IoT y back-end móviles.

En este artículo es una guía exhaustiva para hospedar servicios de ASP.NET Core en Reliable Services de Service Fabric mediante la **Microsoft.ServiceFabric.AspNetCore.** conjunto de paquetes de NuGet.

Para ver un tutorial introductorio sobre ASP.NET Core en Service Fabric e instrucciones sobre cómo configurar el entorno de desarrollo, consulte [Tutorial: Crear e implementar una aplicación con un servicio de front-end de ASP.NET Core Web API y un servicio back-end con estado](service-fabric-tutorial-create-dotnet-app.md).

El resto de este artículo se da por supuesto que ya está familiarizado con ASP.NET Core. Si no, lea el [Fundamentos de ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/index).

## <a name="aspnet-core-in-the-service-fabric-environment"></a>ASP.NET Core en el entorno de Service Fabric

Las aplicaciones de ASP.NET Core y Service Fabric pueden ejecutar en .NET Core o .NET Framework completo. Puede usar ASP.NET Core de dos maneras diferentes de Service Fabric:
 - **Implementado como archivo ejecutable invitado**. De este modo se utiliza principalmente para ejecutar aplicaciones de ASP.NET Core existentes en Service Fabric sin realizar ningún cambio en el código.
 - **Ejecutar dentro de un servicio confiable**. De esta forma permite una mejor integración con el tiempo de ejecución de Service Fabric y permite a los servicios con estado ASP.NET Core.

El resto de este artículo explica cómo usar ASP.NET Core dentro de un servicio confiable a través de los componentes de integración de ASP.NET Core que se incluyen con el SDK de Service Fabric.

## <a name="service-fabric-service-hosting"></a>Hospedaje de servicios de Service Fabric

En Service Fabric, una o varias instancias o réplicas del servicio se ejecutan en un *proceso de host del servicio*: un archivo ejecutable que se ejecuta el código del servicio. Es, como autor del servicio, el propietario del proceso de host, y Service Fabric activa y supervisa automáticamente.

Tradicionalmente, ASP.NET (hasta MVC 5) ha estado estrechamente unido a IIS a través de System.Web.dll. ASP.NET Core proporciona una separación entre el servidor web y la aplicación web. Esta separación permite que las aplicaciones web sean portátiles entre diferentes servidores web. También permite a los servidores web se *autohospedado*. Esto significa que puede iniciar un servidor web en su propio proceso, en lugar de un proceso que pertenece a software de servidor web dedicado, como IIS.

Para combinar un servicio de Service Fabric y ASP.NET, ya sea como un ejecutable invitado o en un servicio de confianza, debe ser capaz de iniciar ASP.NET dentro de su proceso de host de servicio. El autohospedaje de ASP.NET Core lo permite.

## <a name="hosting-aspnet-core-in-a-reliable-service"></a>Hospedaje de ASP.NET Core en un servicio confiable
Por lo general, las aplicaciones ASP.NET Core autohospedadas crean un elemento WebHost en el punto de entrada de una aplicación, como el método `static void Main()` en `Program.cs`. En este caso, el ciclo de vida de WebHost está enlazado al ciclo de vida del proceso.

![Hospedaje de ASP.NET Core en un proceso][0]

Pero el punto de entrada de la aplicación no es el lugar idóneo para crear un elemento WebHost en un servicio de confianza. Eso es porque el punto de entrada de la aplicación solo se usa para registrar un tipo de servicio con el tiempo de ejecución de Service Fabric, por lo que pueden crear instancias de ese tipo de servicio. El elemento WebHost debe crearse en un servicio de confianza. Dentro del proceso de host de servicio, las instancias de servicio o réplicas pueden pasar por varios ciclos de vida. 

Una instancia de Reliable Services se representa mediante la clase de servicio derivada de `StatelessService` o `StatefulService`. La pila de comunicación de un servicio se encuentra en una implementación `ICommunicationListener` en la clase de servicio. El `Microsoft.ServiceFabric.AspNetCore.*` paquetes de NuGet contienen implementaciones de `ICommunicationListener` que inician y administran el elemento WebHost de ASP.NET Core para Kestrel o HTTP.sys en un servicio de confianza.

![Diagrama de hospedaje de ASP.NET Core en un servicio confiable][1]

## <a name="aspnet-core-icommunicationlisteners"></a>ICommunicationListeners en ASP.NET Core
El `ICommunicationListener` implementaciones para Kestrel y HTTP.sys en la `Microsoft.ServiceFabric.AspNetCore.*` paquetes de NuGet tienen patrones de uso similares. Pero realizan acciones ligeramente diferentes, específicas de cada servidor web. 

Ambos agentes de escucha de comunicación proporcionan un constructor que acepta los argumentos siguientes:
 - **`ServiceContext serviceContext`**: Se trata de la `ServiceContext` objeto que contiene información sobre el servicio en ejecución.
 - **`string endpointName`**: Éste es el nombre de un `Endpoint` configuración en ServiceManifest.xml. Es principalmente en qué se diferencian los agentes de escucha de comunicación de dos. HTTP.sys *requiere* un `Endpoint` configuración, mientras que Kestrel no.
 - **`Func<string, AspNetCoreCommunicationListener, IWebHost> build`**: Se trata de una expresión lambda que implementa en que crea y devuelve un `IWebHost`. Permite configurar `IWebHost` tal como haría normalmente en una aplicación ASP.NET Core. La expresión lambda proporciona una dirección URL que se genera automáticamente, según las opciones de integración de Service Fabric utilizas y `Endpoint` configuración que proporcione. A continuación, puede modificar o use esa dirección URL para iniciar el servidor web.

## <a name="service-fabric-integration-middleware"></a>Middleware de integración de Service Fabric
El `Microsoft.ServiceFabric.AspNetCore` paquete NuGet incluye el `UseServiceFabricIntegration` método de extensión en `IWebHostBuilder` que agrega middleware compatible con Service Fabric. Este middleware configura la Kestrel o HTTP.sys `ICommunicationListener` para registrar una dirección URL de servicio única con el servicio de nomenclatura de Service Fabric. A continuación, valida las solicitudes de cliente para asegurarse de que los clientes se conectan al servicio adecuado. 

Este paso es necesario para impedir que los clientes se conecten al servicio incorrecto. Eso es porque, en un entorno de host compartido como Service Fabric, varias aplicaciones web, pueden ejecutar en la misma máquina física o virtual pero no utilice nombres de host único. Este escenario se describe con más detalle en la sección siguiente.

### <a name="a-case-of-mistaken-identity"></a>Un caso de identidad equivocada
Las réplicas de servicio, con independencia del protocolo, escuchan en una combinación IP:puerto única. Una vez que una réplica de servicio ha comenzado a escuchar en un punto de conexión IP: Port, notifica esa dirección de punto de conexión para el servicio de nomenclatura de Service Fabric. Allí, los clientes u otros servicios pueden detectarlo. Si los servicios utilizan puertos de aplicación asignado dinámicamente, una réplica de servicio podría casualmente utilizar el mismo punto de conexión IP: Port de otro servicio previamente en el mismo físico o máquina virtual. Esto puede hacer que un cliente se conecte por error al servicio incorrecto. Este escenario puede producir si se produce la siguiente secuencia de eventos:

 1. Un servicio A escucha en 10.0.0.1:30000 a través de HTTP. 
 2. Cliente resuelve el servicio A y obtiene la dirección 10.0.0.1:30000.
 3. El servicio A se mueve a otro nodo.
 4. El servicio B se coloca en 10.0.0.1 y casualmente usa el mismo puerto 30000.
 5. El cliente intenta conectarse al servicio A con la dirección 10.0.0.1:30000 en caché.
 6. Cliente ahora está conectado correctamente al servicio B, sin darse cuenta de está conectado al servicio incorrecto.

Esto puede causar errores aleatoriamente que son difíciles de diagnosticar.

### <a name="using-unique-service-urls"></a>Uso de direcciones URL de servicio únicas
Para evitar estos errores, los servicios pueden exponer un punto de conexión en el servicio de nomenclatura con un identificador único y, a continuación, validar ese identificador único durante las solicitudes de cliente. Se trata de una acción cooperativa entre servicios en un entorno de confianza sin inquilinos hostiles. No proporciona autenticación de servicio segura en un entorno con inquilinos hostiles.

En un entorno de confianza, el middleware que se agrega mediante la `UseServiceFabricIntegration` método anexa automáticamente un identificador único para la dirección que se registran en el servicio de nomenclatura. Valida ese identificador en cada solicitud. Si no coincide con el identificador, el middleware devuelve inmediatamente una respuesta HTTP 410 ya no existe.

Los servicios que utilizan un puerto asignado de forma dinámica deberían usar este middleware.

Los servicios que utilizan un puerto fijo único no tienen este problema en un entorno cooperativo. Un puerto fijo único se utiliza normalmente para servicios orientados hacia el exterior que necesitan usar un puerto conocido al que las aplicaciones cliente puedan conectarse. Por ejemplo, mayoría de las aplicaciones web accesibles desde internet usará el puerto 80 o 443 para las conexiones de explorador web. En este caso, no debe habilitarse el identificador único.

En el siguiente diagrama se muestra el flujo de la solicitud con el middleware habilitado:

![Integración de ASP.NET Core de Service Fabric][2]

Kestrel y HTTP.sys `ICommunicationListener` implementaciones utilizan este mecanismo exactamente del mismo modo. Aunque HTTP.sys puede diferenciar internamente las solicitudes basadas en rutas de acceso de dirección URL únicas mediante el uso subyacente **HTTP.sys** característica, esa funcionalidad de uso compartido de puerto *no* utilizado por el HTTP.sys `ICommunicationListener`implementación. Eso es porque resulta en códigos de estado de error HTTP 503 y HTTP 404 en el escenario descrito anteriormente. Que a su vez hace difícil para los clientes determinen la intención del error, como HTTP 503 y HTTP 404 se usan normalmente para indicar otros errores. 

Por lo tanto, Kestrel y HTTP.sys `ICommunicationListener` estandarizan las implementaciones en middleware proporcionado por el `UseServiceFabricIntegration` método de extensión. Por lo tanto, los clientes solo necesiten realizar una acción de volver a resolver de punto de conexión de servicio en las respuestas HTTP 410.

## <a name="httpsys-in-reliable-services"></a>HTTP.sys en Reliable Services
Puede usar HTTP.sys en Reliable Services importando el **Microsoft.ServiceFabric.AspNetCore.HttpSys** paquete NuGet. Este paquete contiene `HttpSysCommunicationListener`, una implementación de `ICommunicationListener`. `HttpSysCommunicationListener` le permite crear un elemento WebHost de ASP.NET Core dentro de un servicio confiable con HTTP.sys como el servidor web.

HTTP.sys se basa en el [Windows HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510(v=vs.85).aspx). Esta API usa el **HTTP.sys** controlador de kernel para procesar las solicitudes HTTP y enrutarlas a procesos que se ejecutan las aplicaciones web. Esto permite que varios procesos en la misma máquina física o virtual para aplicaciones de web de host en el mismo puerto, eliminando la ambigüedad con cualquier una única dirección URL ruta de acceso o nombre de host. Estas características son útiles en Service Fabric para hospedar varios sitios web en el mismo clúster.

>[!NOTE]
>Implementación de HTTP.sys funciona sólo en la plataforma Windows.

El siguiente diagrama ilustra cómo utiliza HTTP.sys el **HTTP.sys** controlador de kernel en Windows para compartir puertos:

![Diagrama de HTTP.sys][3]

### <a name="httpsys-in-a-stateless-service"></a>HTTP.sys en un servicio sin estado
Para usar `HttpSys` en un servicio sin estado, invalide el método `CreateServiceInstanceListeners` y devuelva una instancia de `HttpSysCommunicationListener`:

```csharp
protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    return new ServiceInstanceListener[]
    {
        new ServiceInstanceListener(serviceContext =>
            new HttpSysCommunicationListener(serviceContext, "ServiceEndpoint", (url, listener) =>
                new WebHostBuilder()
                    .UseHttpSys()
                    .ConfigureServices(
                        services => services
                            .AddSingleton<StatelessServiceContext>(serviceContext))
                    .UseContentRoot(Directory.GetCurrentDirectory())
                    .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.None)
                    .UseStartup<Startup>()
                    .UseUrls(url)
                    .Build()))
    };
}
```

### <a name="httpsys-in-a-stateful-service"></a>HTTP.sys en un servicio con estado

`HttpSysCommunicationListener` actualmente no está diseñada para su uso en los servicios con estado debido a las complicaciones con subyacente **HTTP.sys** característica de uso compartido de puerto. Para obtener más información, consulte la sección siguiente sobre la asignación de puerto dinámico con HTTP.sys. Para los servicios con estado, Kestrel es el servidor web sugerido.

### <a name="endpoint-configuration"></a>Configuración del punto de conexión

Un `Endpoint` configuración es necesaria para los servidores web que utilizan la API de servidor de HTTP de Windows, incluidas HTTP.sys. Servidores Web que utilizan la API de servidor HTTP de Windows deben reservar en primer lugar su dirección URL con HTTP.sys (normalmente, esto se logra con la [netsh](https://msdn.microsoft.com/library/windows/desktop/cc307236(v=vs.85).aspx) herramienta). 

Esta acción requiere privilegios elevados que los servicios no tienen de forma predeterminada. Las opciones "http" o "https" para el `Protocol` propiedad de la `Endpoint` configuración en ServiceManifest.xml se utilizan específicamente para indicar al runtime de Service Fabric que registre una dirección URL con HTTP.sys en su nombre. Para ello, utiliza el [ *carácter comodín seguro* ](https://msdn.microsoft.com/library/windows/desktop/aa364698(v=vs.85).aspx) prefijo de dirección URL.

Por ejemplo, para reservar `http://+:80` para un servicio, use la siguiente configuración en ServiceManifest.xml:

```xml
<ServiceManifest ... >
    ...
    <Resources>
        <Endpoints>
            <Endpoint Name="ServiceEndpoint" Protocol="http" Port="80" />
        </Endpoints>
    </Resources>

</ServiceManifest>
```

Y se debe pasar el nombre del punto de conexión al constructor `HttpSysCommunicationListener`:

```csharp
 new HttpSysCommunicationListener(serviceContext, "ServiceEndpoint", (url, listener) =>
 {
     return new WebHostBuilder()
         .UseHttpSys()
         .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.None)
         .UseUrls(url)
         .Build();
 })
```

#### <a name="use-httpsys-with-a-static-port"></a>Usar HTTP.sys con un puerto estático
Para usar un puerto estático con HTTP.sys, proporcione el número de puerto en el `Endpoint` configuración:

```xml
  <Resources>
    <Endpoints>
      <Endpoint Protocol="http" Name="ServiceEndpoint" Port="80" />
    </Endpoints>
  </Resources>
```

#### <a name="use-httpsys-with-a-dynamic-port"></a>Usar HTTP.sys con un puerto dinámico
Para usar un puerto asignado dinámicamente con HTTP.sys, omita el `Port` propiedad en el `Endpoint` configuración:

```xml
  <Resources>
    <Endpoints>
      <Endpoint Protocol="http" Name="ServiceEndpoint" />
    </Endpoints>
  </Resources>
```

Un puerto dinámico asignado por una `Endpoint` configuración proporciona un único puerto *por el proceso de host*. El modelo de hospedaje actual de Service Fabric permite varias instancias de servicio o réplicas se hospeden en el mismo proceso. Esto significa que cada uno de ellos compartirán el mismo puerto cuando se asignan a través de la `Endpoint` configuración. Varios **HTTP.sys** instancias pueden compartir un puerto mediante el uso subyacente **HTTP.sys** característica de uso compartido de puerto. Pero no es compatible con `HttpSysCommunicationListener` debido a las complicaciones que presenta las solicitudes de cliente. Para el uso de puertos dinámicos, Kestrel es el servidor web sugerido.

## <a name="kestrel-in-reliable-services"></a>Kestrel en Reliable Services
Puede usar Kestrel en Reliable Services importando el **Microsoft.ServiceFabric.AspNetCore.Kestrel** paquete NuGet. Este paquete contiene `KestrelCommunicationListener`, una implementación de `ICommunicationListener`. `KestrelCommunicationListener` permite crear un elemento WebHost de ASP.NET Core dentro de un servicio confiable con Kestrel como servidor web.

Kestrel es un servidor web multiplataforma para ASP.NET Core basado en libuv, una biblioteca de E/S asincrónica multiplataforma. A diferencia de HTTP.sys, Kestrel no utiliza un administrador centralizado de punto de conexión. También a diferencia de HTTP.sys, Kestrel no admite el uso compartido de puerto entre varios procesos. Cada instancia de Kestrel debe usar un puerto único.

![Diagrama de kestrel][4]

### <a name="kestrel-in-a-stateless-service"></a>Kestrel en un servicio sin estado
Para usar `Kestrel` en un servicio sin estado, invalide el método `CreateServiceInstanceListeners` y devuelva una instancia de `KestrelCommunicationListener`:

```csharp
protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    return new ServiceInstanceListener[]
    {
        new ServiceInstanceListener(serviceContext =>
            new KestrelCommunicationListener(serviceContext, "ServiceEndpoint", (url, listener) =>
                new WebHostBuilder()
                    .UseKestrel()
                    .ConfigureServices(
                        services => services
                            .AddSingleton<StatelessServiceContext>(serviceContext))
                    .UseContentRoot(Directory.GetCurrentDirectory())
                    .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.UseUniqueServiceUrl)
                    .UseStartup<Startup>()
                    .UseUrls(url)
                    .Build();
            ))
    };
}
```

### <a name="kestrel-in-a-stateful-service"></a>Kestrel en un servicio con estado
Para usar `Kestrel` en un servicio con estado, invalide el método `CreateServiceReplicaListeners` y devuelva una instancia de `KestrelCommunicationListener`:

```csharp
protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new ServiceReplicaListener[]
    {
        new ServiceReplicaListener(serviceContext =>
            new KestrelCommunicationListener(serviceContext, (url, listener) =>
                new WebHostBuilder()
                    .UseKestrel()
                    .ConfigureServices(
                         services => services
                             .AddSingleton<StatefulServiceContext>(serviceContext)
                             .AddSingleton<IReliableStateManager>(this.StateManager))
                    .UseContentRoot(Directory.GetCurrentDirectory())
                    .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.UseUniqueServiceUrl)
                    .UseStartup<Startup>()
                    .UseUrls(url)
                    .Build();
            ))
    };
}
```

En este ejemplo, se proporciona una instancia singleton de `IReliableStateManager` en el contenedor de inyección de dependencias de WebHost. Esto no es estrictamente necesario, pero permite usar `IReliableStateManager` y Reliable Collections en los métodos de acción de controlador MVC.

*No* se proporciona un nombre de configuración `Endpoint` a `KestrelCommunicationListener` en un servicio con estado. Esto se explica con más detalle en la sección siguiente.

### <a name="configure-kestrel-to-use-https"></a>Configurar Kestrel para que use HTTPS
Al habilitar HTTPS con Kestrel en el servicio, deberá establecer varias opciones de escuchas. Actualización de la `ServiceInstanceListener` para usar un *EndpointHttps* punto de conexión y escuche en un puerto específico (por ejemplo, el puerto 443). Al configurar el host de web para usar el servidor web Kestrel, debe configurar Kestrel para escuchar las direcciones IPv6 en todas las interfaces de red: 

```csharp
new ServiceInstanceListener(
serviceContext =>
    new KestrelCommunicationListener(
        serviceContext,
        "EndpointHttps",
        (url, listener) =>
        {
            ServiceEventSource.Current.ServiceMessage(serviceContext, $"Starting Kestrel on {url}");

            return new WebHostBuilder()
                .UseKestrel(opt =>
                {
                    int port = serviceContext.CodePackageActivationContext.GetEndpoint("EndpointHttps").Port;
                    opt.Listen(IPAddress.IPv6Any, port, listenOptions =>
                    {
                        listenOptions.UseHttps(GetCertificateFromStore());
                        listenOptions.NoDelay = true;
                    });
                })
                .ConfigureAppConfiguration((builderContext, config) =>
                {
                    config.AddJsonFile("appsettings.json", optional: false, reloadOnChange: true);
                })

                .ConfigureServices(
                    services => services
                        .AddSingleton<HttpClient>(new HttpClient())
                        .AddSingleton<FabricClient>(new FabricClient())
                        .AddSingleton<StatelessServiceContext>(serviceContext))
                .UseContentRoot(Directory.GetCurrentDirectory())
                .UseStartup<Startup>()
                .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.None)
                .UseUrls(url)
                .Build();
        }))
```

Para obtener un ejemplo completo en un tutorial, consulte [configurar Kestrel para que use HTTPS](service-fabric-tutorial-dotnet-app-enable-https-endpoint.md#configure-kestrel-to-use-https).


### <a name="endpoint-configuration"></a>Configuración del punto de conexión
Un `Endpoint` configuración no es necesario usar Kestrel. 

Kestrel es un servidor de web independiente simple. A diferencia de HTTP.sys (o HttpListener), no es necesario un `Endpoint` configuración en ServiceManifest.xml porque no requiere el registro de dirección URL antes de iniciar. 

#### <a name="use-kestrel-with-a-static-port"></a>Uso de Kestrel con un puerto estático
Puede configurar un puerto estático en el `Endpoint` configuración de ServiceManifest.xml para usarlo con Kestrel. Aunque esto no es estrictamente necesario, ofrece dos ventajas potenciales:
 - Si el puerto no se encuentra en el intervalo de puertos de aplicación, se abre a través del firewall del sistema operativo por Service Fabric.
 - La dirección URL proporcionada a través de `KestrelCommunicationListener` usará este puerto.

```xml
  <Resources>
    <Endpoints>
      <Endpoint Protocol="http" Name="ServiceEndpoint" Port="80" />
    </Endpoints>
  </Resources>
```

Si se configura una instancia de `Endpoint`, se debe pasar su nombre al constructor `KestrelCommunicationListener`: 

```csharp
new KestrelCommunicationListener(serviceContext, "ServiceEndpoint", (url, listener) => ...
```

Si no usa ServiceManifest.xml una `Endpoint` configuración, omita el nombre de la `KestrelCommunicationListener` constructor. En este caso, usará un puerto dinámico. Consulte la sección siguiente para obtener más información acerca de este.

#### <a name="use-kestrel-with-a-dynamic-port"></a>Uso de Kestrel con un puerto dinámico
Kestrel no puede usar la asignación automática de puertos desde el `Endpoint` configuración en ServiceManifest.xml. Eso es automática porque el puerto de asignación de un `Endpoint` configuración asigna un puerto único por *el proceso de host*, y un proceso de host solo puede contener varias instancias de Kestrel. Esto no funciona con Kestrel porque no admite el uso compartido de puertos. Por lo tanto, cada instancia de Kestrel debe abrirse en un único puerto.

Para usar la asignación dinámica de puertos con Kestrel, omita el `Endpoint` configuración en ServiceManifest.xml por completo y no pase un nombre de punto de conexión para el `KestrelCommunicationListener` constructor, como se indica a continuación:

```csharp
new KestrelCommunicationListener(serviceContext, (url, listener) => ...
```

En esta configuración, `KestrelCommunicationListener` seleccionará automáticamente un puerto no utilizado en el intervalo de puertos de la aplicación.

## <a name="service-fabric-configuration-provider"></a>Proveedor de configuración de Service Fabric
Configuración de la aplicación en ASP.NET Core se basa en pares de clave-valor establecidos por el proveedor de configuración. Lectura [configuración en ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/) para conocer más sobre compatibilidad de configuración de ASP.NET Core general.

Esta sección describe cómo el proveedor de configuración de Service Fabric se integra con la configuración de ASP.NET Core mediante la importación de la `Microsoft.ServiceFabric.AspNetCore.Configuration` paquete NuGet.

### <a name="addservicefabricconfiguration-startup-extensions"></a>Extensiones de inicio AddServiceFabricConfiguration
Después de importar el `Microsoft.ServiceFabric.AspNetCore.Configuration` paquete NuGet, deberá registrar el origen de configuración de Service Fabric con API de configuración de ASP.NET Core. Hacerlo comprobando **AddServiceFabricConfiguration** extensiones en el `Microsoft.ServiceFabric.AspNetCore.Configuration` espacio de nombres en `IConfigurationBuilder`.

```csharp
using Microsoft.ServiceFabric.AspNetCore.Configuration;

public Startup(IHostingEnvironment env)
{
    var builder = new ConfigurationBuilder()
        .SetBasePath(env.ContentRootPath)
        .AddJsonFile("appsettings.json", optional: false, reloadOnChange: true)
        .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true)
        .AddServiceFabricConfiguration() // Add Service Fabric configuration settings.
        .AddEnvironmentVariables();
    Configuration = builder.Build();
}

public IConfigurationRoot Configuration { get; }
```

Ahora el servicio de ASP.NET Core puede tener acceso a los valores de configuración de Service Fabric, al igual que cualquier otra configuración de la aplicación. Por ejemplo, puede usar el patrón de opciones para cargar la configuración en objetos fuertemente tipados.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.Configure<MyOptions>(Configuration);  // Strongly typed configuration object.
    services.AddMvc();
}
```
### <a name="default-key-mapping"></a>Asignación de claves predeterminada
De forma predeterminada, el proveedor de configuración de Service Fabric incluye el nombre del paquete, el nombre de sección y el nombre de la propiedad. Juntas, forman la clave de configuración de ASP.NET Core, como sigue:
```csharp
$"{this.PackageName}{ConfigurationPath.KeyDelimiter}{section.Name}{ConfigurationPath.KeyDelimiter}{property.Name}"
```

Por ejemplo, si tiene un paquete de configuración denominado `MyConfigPackage` con el siguiente contenido, a continuación, el valor de configuración estará disponible en ASP.NET Core `IConfiguration` a través de *MyConfigPackage:MyConfigSection:MyParameter*.
```xml
<?xml version="1.0" encoding="utf-8" ?>
<Settings xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">  
  <Section Name="MyConfigSection">
    <Parameter Name="MyParameter" Value="Value1" />
  </Section>  
</Settings>
```
### <a name="service-fabric-configuration-options"></a>Opciones de configuración de Service Fabric
El proveedor de configuración de Service Fabric también admite `ServiceFabricConfigurationOptions` para cambiar el comportamiento predeterminado de asignación de claves.

#### <a name="encrypted-settings"></a>Configuración cifrada
Service Fabric admite la configuración cifrada, igual que el proveedor de configuración de Service Fabric. No se puede descifrar la configuración cifrada a ASP.NET Core `IConfiguration` de forma predeterminada. Los valores cifrados se almacenan allí en su lugar. Pero si desea descifrar el valor que se almacenará en ASP.NET Core IConfiguration, puede establecer el *DecryptValue* marca en false en el `AddServiceFabricConfiguration` extensión, como se indica a continuación:

```csharp
public Startup()
{
    ICodePackageActivationContext activationContext = FabricRuntime.GetActivationContext();
    var builder = new ConfigurationBuilder()        
        .AddServiceFabricConfiguration(activationContext, (options) => options.DecryptValue = true); // set flag to decrypt the value
    Configuration = builder.Build();
}
```
#### <a name="multiple-configuration-packages"></a>Varios paquetes de configuración
Service Fabric admite varios paquetes de configuración. De forma predeterminada, el nombre del paquete se incluye en la clave de configuración. Pero puede establecer el `IncludePackageName` marca en false, como se indica a continuación:
```csharp
public Startup()
{
    ICodePackageActivationContext activationContext = FabricRuntime.GetActivationContext();
    var builder = new ConfigurationBuilder()        
        // exclude package name from key.
        .AddServiceFabricConfiguration(activationContext, (options) => options.IncludePackageName = false); 
    Configuration = builder.Build();
}
```
#### <a name="custom-key-mapping-value-extraction-and-data-population"></a>Asignación de claves personalizado, la extracción de valor y el rellenado de datos
El proveedor de configuración de Service Fabric también admite escenarios más avanzados para personalizar la asignación de claves con `ExtractKeyFunc` y extraer los valores con custom `ExtractValueFunc`. Incluso puede cambiar todo el proceso de rellenar los datos de configuración de Service Fabric a la configuración de ASP.NET Core mediante el uso de `ConfigAction`.

Los ejemplos siguientes muestran cómo usar `ConfigAction` para personalizar el rellenado de datos:
```csharp
public Startup()
{
    ICodePackageActivationContext activationContext = FabricRuntime.GetActivationContext();
    
    this.valueCount = 0;
    this.sectionCount = 0;
    var builder = new ConfigurationBuilder();
    builder.AddServiceFabricConfiguration(activationContext, (options) =>
        {
            options.ConfigAction = (package, configData) =>
            {
                ILogger logger = new ConsoleLogger("Test", null, false);
                logger.LogInformation($"Config Update for package {package.Path} started");

                foreach (var section in package.Settings.Sections)
                {
                    this.sectionCount++;

                    foreach (var param in section.Parameters)
                    {
                        configData[options.ExtractKeyFunc(section, param)] = options.ExtractValueFunc(section, param);
                        this.valueCount++;
                    }
                }

                logger.LogInformation($"Config Update for package {package.Path} finished");
            };
        });
  Configuration = builder.Build();
}
```

### <a name="configuration-updates"></a>Actualizaciones de configuración
El proveedor de configuración de Service Fabric también admite las actualizaciones de configuración. Puede usar ASP.NET Core `IOptionsMonitor` para recibir notificaciones de cambio y, a continuación, usar `IOptionsSnapshot` para volver a cargar los datos de configuración. Para obtener más información, consulte [opciones de ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/options).

Estas opciones se admiten de forma predeterminada. Ninguna otra codificación es necesaria para habilitar las actualizaciones de configuración.

## <a name="scenarios-and-configurations"></a>Escenarios y configuraciones
Esta sección proporciona la combinación de servidor web, configuración de puerto, opciones de integración de Service Fabric y diversas configuraciones, que se recomienda para solucionar problemas de los siguientes escenarios:
 - Servicios sin estado de ASP.NET Core expuestos al exterior
 - Servicios sin estado de ASP.NET Core solo interno
 - Servicios con estado de ASP.NET Core solo interno

Un **servicio expuesto al exterior** es aquel que expone un punto de conexión que se llama desde fuera del clúster, normalmente a través de un equilibrador de carga.

Un **sólo interno** servicio es uno cuyo punto de conexión solo se llama desde dentro del clúster.

> [!NOTE]
> Los puntos de conexión de servicio con estado generalmente no deberían exponerse a internet. Clústeres detrás de equilibradores de carga que no son conscientes de resolución de servicio de Service Fabric, como Azure Load Balancer, podrá exponer los servicios con estado. Eso es porque el equilibrador de carga no podrá localizar ni enrutar el tráfico a la réplica de servicio con estado adecuada. 

### <a name="externally-exposed-aspnet-core-stateless-services"></a>Servicios sin estado de ASP.NET Core expuestos al exterior
Kestrel es el servidor web sugerido para los servicios front-end que exponen los puntos de conexión HTTP externos, orientado a internet. En Windows, HTTP.sys puede proporcionar funcionalidad de uso compartido de puerto, que le permite hospedar varios servicios web en el mismo conjunto de nodos con el mismo puerto. En este escenario, los servicios web se diferencian por nombre de host o ruta de acceso, sin tener que depender de un proxy de front-end o puerta de enlace para brindar enrutamiento HTTP.
 
Cuando se expone a internet, un servicio sin estado debe usar un punto de conexión conocido y estable que sea accesible a través de un equilibrador de carga. Deberá proporcionar esta dirección URL a los usuarios de su aplicación. Se recomienda la siguiente configuración:

|  |  | **Notas** |
| --- | --- | --- |
| Servidor web | Kestrel | Kestrel es el servidor web que prefiera, como se admite en Windows y Linux. |
| Configuración de puerto | estática | Se debe configurar un puerto estático conocido en la configuración `Endpoints` de ServiceManifest.xml, como 80 para HTTP o 443 para HTTPS. |
| ServiceFabricIntegrationOptions | None | Use el `ServiceFabricIntegrationOptions.None` opción al configurar el middleware de integración de Service Fabric, para que el servicio no intenta validar solicitudes entrantes para un identificador único. Los usuarios externos de la aplicación no saben que la información de identificación única que usa el middleware. |
| Recuento de instancias | -1 | En los casos de uso típico, se debe establecer la configuración de recuento de instancia en *-1*. Esto se hace para que una instancia está disponible en todos los nodos que reciben tráfico de un equilibrador de carga. |

Si varios servicios expuestos al exterior comparten el mismo conjunto de nodos, puede usar HTTP.sys con una ruta de acceso de dirección URL única pero estable. Esto se consigue mediante la modificación de la dirección URL proporcionada al configurar IWebHost. Tenga en cuenta que esto solo se aplica a HTTP.sys.

 ```csharp
 new HttpSysCommunicationListener(serviceContext, "ServiceEndpoint", (url, listener) =>
 {
     url += "/MyUniqueServicePath";
 
     return new WebHostBuilder()
         .UseHttpSys()
         ...
         .UseUrls(url)
         .Build();
 })
 ```

### <a name="internal-only-stateless-aspnet-core-service"></a>Servicio sin estado de ASP.NET Core solo interno
Los servicios sin estado a los que solo se llama desde dentro del clúster deben utilizar direcciones URL únicas y puertos asignados de forma dinámica para garantizar la cooperación entre varios servicios. Se recomienda la siguiente configuración:

|  |  | **Notas** |
| --- | --- | --- |
| Servidor web | Kestrel | Aunque puede usar HTTP.sys para servicios sin estado internos, Kestrel es el mejor servidor para permitir que varias instancias de servicio compartan un host.  |
| Configuración de puerto | asignado de forma dinámica | Varias réplicas de un servicio con estado pueden compartir un proceso de host o el sistema operativo host y, por tanto, necesitarán puertos únicos. |
| ServiceFabricIntegrationOptions | UseUniqueServiceUrl | Con la asignación dinámica de puertos, esta configuración evita el problema de identidad equivocada descrito antes. |
| InstanceCount | cualquiera | La configuración de recuento de instancias se puede establecer en cualquier valor necesario para hacer funcionar el servicio. |

### <a name="internal-only-stateful-aspnet-core-service"></a>Servicio con estado de ASP.NET Core solo interno
Los servicios con estado a los que solo se llama desde dentro del clúster deben utilizar puertos asignados de forma dinámica para garantizar la cooperación entre varios servicios. Se recomienda la siguiente configuración:

|  |  | **Notas** |
| --- | --- | --- |
| Servidor web | Kestrel | El `HttpSysCommunicationListener` no está diseñado para los servicios con estado en el que las réplicas comparten un proceso de host. |
| Configuración de puerto | asignado de forma dinámica | Varias réplicas de un servicio con estado pueden compartir un proceso de host o el sistema operativo host y, por tanto, necesitarán puertos únicos. |
| ServiceFabricIntegrationOptions | UseUniqueServiceUrl | Con la asignación dinámica de puertos, esta configuración evita el problema de identidad equivocada descrito antes. |

## <a name="next-steps"></a>Pasos siguientes
[Depurar la aplicación de Service Fabric con Visual Studio](service-fabric-debugging-your-application.md)


<!--Image references-->
[0]:./media/service-fabric-reliable-services-communication-aspnetcore/webhost-standalone.png
[1]:./media/service-fabric-reliable-services-communication-aspnetcore/webhost-servicefabric.png
[2]:./media/service-fabric-reliable-services-communication-aspnetcore/integration.png
[3]:./media/service-fabric-reliable-services-communication-aspnetcore/httpsys.png
[4]:./media/service-fabric-reliable-services-communication-aspnetcore/kestrel.png
