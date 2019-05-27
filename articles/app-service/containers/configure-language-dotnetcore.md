---
title: 'Configurar aplicaciones de ASP.NET Core: Azure App Service | Microsoft Docs'
description: Obtenga información sobre cómo configurar aplicaciones de ASP.NET Core para que funcione en Azure App Service
services: app-service
documentationcenter: ''
author: cephalin
manager: jpconnock
editor: ''
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/28/2019
ms.author: cephalin
ms.openlocfilehash: f2781e3cc2433f73ba7ff33e5c452e29de746adf
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2019
ms.locfileid: "65956207"
---
# <a name="configure-a-linux-aspnet-core-app-for-azure-app-service"></a>Configurar una Linux aplicación ASP.NET Core para Azure App Service

Aplicaciones de ASP.NET Core deben implementarse como archivos binarios compilados. La herramienta de publicación de Visual Studio compila la solución y, a continuación, implementa los archivos binarios compilados directamente, mientras que el motor de implementación de App Service implementa el repositorio de código en primer lugar y, a continuación, compila los archivos binarios.

Esta guía proporciona instrucciones para ASP.NET Core y conceptos clave los desarrolladores que usan un contenedor de Linux integrado en App Service. Si nunca ha usado Azure App Service, siga el [inicio rápido de ASP.NET Core](quickstart-dotnetcore.md) y [ASP.NET Core con el tutorial de base de datos SQL](tutorial-dotnetcore-sqldb-app.md) primero.

## <a name="show-net-core-version"></a>Mostrar la versión de .NET Core

Para mostrar la versión actual de .NET Core, ejecute el siguiente comando el [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp config show --resource-group <resource-group-name> --name <app-name> --query linuxFxVersion
```

Para mostrar todas las versiones de .NET Core, ejecute el siguiente comando el [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp list-runtimes --linux | grep DOTNETCORE
```

## <a name="set-net-core-version"></a>Establezca la versión de .NET Core

Ejecute el siguiente comando el [Cloud Shell](https://shell.azure.com) para establecer la versión de .NET Core 2.1:

```azurecli-interactive
az webapp config set --name <app-name> --resource-group <resource-group-name> --linux-fx-version "DOTNETCORE|2.1"
```

## <a name="access-environment-variables"></a>Acceso a variables de entorno

En App Service, puede [establecer la configuración de la aplicación](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings) fuera del código de la aplicación. A continuación, puede acceder a ellos mediante el patrón estándar de ASP.NET:

```csharp
include Microsoft.Extensions.Configuration;
// retrieve App Service app setting
System.Configuration.ConfigurationManager.AppSettings["MySetting"]
// retrieve App Service connection string
Configuration.GetConnectionString("MyDbConnection")
```

Si configura una configuración de aplicación con el mismo nombre en App Service y en *Web.config*, el valor de App Service tiene prioridad sobre el valor de Web.config. El valor de Web.config permite depurar la aplicación localmente, pero el valor de App Service permite la ejecución de la aplicación en el producto con la configuración de producción. Las cadenas de conexión funcionan de la misma manera. De este modo, puede conservar los secretos de aplicación fuera de su repositorio de código y tener acceso a los valores adecuados sin cambiar el código.

## <a name="get-detailed-exceptions-page"></a>Obtener la página de excepciones detalladas

Cuando la aplicación de ASP.NET genera una excepción en el depurador de Visual Studio, el explorador muestra una página detallada de la excepción, pero en App Service, esa página se sustituirá por un tipo genérico **HTTP 500** error o **un error al Procesando la solicitud.** de la directiva). Para mostrar la página detallada de la excepción en App Service, agregue el `ASPNETCORE_ENVIRONMENT` configuración de la aplicación a la aplicación ejecutando el siguiente comando en el <a target="_blank" href="https://shell.azure.com" >Cloud Shell</a>.

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings ASPNETCORE_ENVIRONMENT="Development"
```

## <a name="detect-https-session"></a>Detección de sesión de HTTPS

En App Service, la [terminación de SSL](https://wikipedia.org/wiki/TLS_termination_proxy) se produce en los equilibradores de carga de red, por lo que todas las solicitudes HTTPS llegan a su aplicación en forma de solicitudes HTTP sin cifrar. Si necesita saber si el usuario solicita están cifrados o no la lógica de aplicación, configure el Middleware de encabezados reenviados en *Startup.cs*:

- Configure el middleware con [ForwardedHeadersOptions](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) para reenviar el `X-Forwarded-For` y `X-Forwarded-Proto` encabezados en `Startup.ConfigureServices`.
- Agregue los intervalos de direcciones IP privadas a las redes conocidas, por lo que el middleware puede confiar en el equilibrador de carga de App Service.
- Invocar el [UseForwardedHeaders](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) método `Startup.Configure` antes de llamar a otro middleware.

Reunir los tres elementos, el código es similar al ejemplo siguiente:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.Configure<ForwardedHeadersOptions>(options =>
    {
        options.ForwardedHeaders =
            ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto;
        options.KnownNetworks.Add(new IPNetwork(IPAddress.Parse("::ffff:10.0.0.0"), 104));
        options.KnownNetworks.Add(new IPNetwork(IPAddress.Parse("::ffff:192.168.0.0"), 112));
        options.KnownNetworks.Add(new IPNetwork(IPAddress.Parse("::ffff:172.16.0.0"), 108));
    });
}

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseForwardedHeaders();

    ...

    app.UseMvc();
}
```

Para obtener más información, consulte [configurar ASP.NET Core para trabajar con servidores proxy y equilibradores de carga](https://docs.microsoft.com/aspnet/core/host-and-deploy/proxy-load-balancer).

## <a name="deploy-multi-project-solutions"></a>Implementar soluciones de varios proyectos

Al implementar un repositorio ASP.NET en el motor de implementación con un *.csproj* archivo en el directorio raíz, el motor implementa el proyecto. Cuando se implementa un repositorio ASP.NET con un *.sln* archivo en el directorio raíz, el motor elige el primer sitio Web o proyecto de aplicación Web que encuentra que la aplicación de App Service. Es posible que el motor no elegir el proyecto que desee.

Para implementar una solución de varios proyectos, puede especificar el proyecto para usar en App Service de dos maneras diferentes:

### <a name="using-deployment-file"></a>Mediante el archivo de implementación.

Agregar un *.deployment* del archivo a la raíz del repositorio y agregue el código siguiente:

```
[config]
project = <project-name>/<project-name>.csproj
```

### <a name="using-app-settings"></a>Mediante la configuración de aplicación

En el <a target="_blank" href="https://shell.azure.com">Azure Cloud Shell</a>, agregar una configuración de aplicación a la aplicación de App Service mediante la ejecución del siguiente comando de CLI. Reemplace  *\<app-name >*,  *\<resource-group-name >*, y  *\<nombre del proyecto >* con los valores adecuados .

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings PROJECT="<project-name>/<project-name>.csproj"
```

## <a name="access-diagnostic-logs"></a>Acceso a los registros de diagnóstico

[!INCLUDE [Access diagnostic logs](../../../includes/app-service-web-logs-access-no-h.md)]

## <a name="open-ssh-session-in-browser"></a>Abrir sesión SSH en el explorador

[!INCLUDE [Open SSH session in browser](../../../includes/app-service-web-ssh-connect-builtin-no-h.md)]

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Tutorial: Aplicación de ASP.NET Core con SQL Database](tutorial-dotnetcore-sqldb-app.md)

> [!div class="nextstepaction"]
> [P+F sobre App Service en Linux](app-service-linux-faq.md)