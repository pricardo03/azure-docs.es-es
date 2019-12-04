---
title: Inicio rápido de Azure App Configuration con ASP.NET Core | Microsoft Docs
description: Un inicio rápido para el uso de Azure App Configuration con aplicaciones de ASP.NET Core
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.devlang: csharp
ms.topic: quickstart
ms.tgt_pltfrm: ASP.NET Core
ms.workload: tbd
ms.date: 10/11/2019
ms.author: yegu
ms.openlocfilehash: 91712b3f730317e65cda7b48c8f5636b2fb9ab2c
ms.sourcegitcommit: dbde4aed5a3188d6b4244ff7220f2f75fce65ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/19/2019
ms.locfileid: "74185085"
---
# <a name="quickstart-create-an-aspnet-core-app-with-azure-app-configuration"></a>Inicio rápido: Creación de una aplicación ASP.NET Core con Azure App Configuration

En este inicio rápido incorporará Azure App Configuration a una aplicación de ASP.NET Core para centralizar el almacenamiento y la administración de la configuración de la aplicación de forma independiente del código. ASP.NET Core genera un objeto de configuración basado en un par clave-valor, para lo que usa la configuración de uno o varios orígenes de datos, que una aplicación especifica. Estos orígenes de datos se conocen como *proveedores de configuración*. Dado que el cliente .NET Core de App Configuration se implementa como un proveedor, el servicio aparece igual que cualquier otro origen de datos.

## <a name="prerequisites"></a>Requisitos previos

- Una suscripción a Azure: [cree una cuenta gratuita](https://azure.microsoft.com/free/)
- [SDK de .NET Core](https://dotnet.microsoft.com/download)

## <a name="create-an-app-configuration-store"></a>Creación de un almacén de App Configuration

[!INCLUDE [azure-app-configuration-create](../../includes/azure-app-configuration-create.md)]

6. Seleccione **Explorador de configuración** >  **+ Crear** para agregar los siguientes pares clave-valor:

    | Clave | Valor |
    |---|---|
    | TestApp:Settings:BackgroundColor | Blanco |
    | TestApp:Settings:FontSize | 24 |
    | TestApp:Settings:FontColor | Negro |
    | TestApp:Settings:Message | Datos de Azure App Configuration |

    Deje **Etiqueta** y **Tipo de contenido** en blanco, por ahora.

## <a name="create-an-aspnet-core-web-app"></a>Cree una aplicación web ASP.NET Core

Va a utilizar la [interfaz de la línea de comandos (CLI) de .NET Core](https://docs.microsoft.com/dotnet/core/tools/) para crear un proyecto de aplicación web de ASP.NET Core MVC. La ventaja de usar la CLI de .NET Core frente a Visual Studio es que está disponible en las plataformas Windows, macOS y Linux.

1. Cree una carpeta nueva para su proyecto. En este inicio rápido, lo llamaremos *TestAppConfig*.

2. En la nueva carpeta, ejecute el siguiente comando para crear un nuevo proyecto de aplicación web de ASP.NET Core MVC:

    ```CLI
        dotnet new mvc --no-https
    ```

## <a name="add-secret-manager"></a>Incorporación de Secret Manager

Para usar Secret Manager, agregue un elemento `UserSecretsId` al archivo *.csproj*.

- Abra el archivo *.csproj*. Agregue un elemento `UserSecretsId` como se muestra aquí. Puede usar el mismo identificador único global, o bien puede reemplazar este valor por el suyo propio. Guarde el archivo.

    ```xml
    <Project Sdk="Microsoft.NET.Sdk.Web">

    <PropertyGroup>
        <TargetFramework>netcoreapp2.1</TargetFramework>
        <UserSecretsId>79a3edd0-2092-40a2-a04d-dcb46d5ca9ed</UserSecretsId>
    </PropertyGroup>

    <ItemGroup>
        <PackageReference Include="Microsoft.AspNetCore.App" />
        <PackageReference Include="Microsoft.AspNetCore.Razor.Design" Version="2.1.2" PrivateAssets="All" />
    </ItemGroup>

    </Project>
    ```

La herramienta Secret Manager almacena información confidencial para el trabajo de desarrollo fuera de su árbol de proyecto. Este enfoque ayuda a evitar el uso compartido accidental de secretos de la aplicación en el código fuente. Para más información sobre Secret Manager, consulte [Almacenamiento seguro de secretos de aplicación en el desarrollo en ASP.NET Core](https://docs.microsoft.com/aspnet/core/security/app-secrets)

## <a name="connect-to-an-app-configuration-store"></a>Conexión a un almacén de App Configuration

1. Para agregar una referencia al paquete NuGet `Microsoft.Azure.AppConfiguration.AspNetCore`, ejecute el comando siguiente:

    ```CLI
        dotnet add package Microsoft.Azure.AppConfiguration.AspNetCore --version 2.0.0-preview-010060003-1250
    ```
2. Ejecute el siguiente comando para restaurar los paquetes para el proyecto:

    ```CLI
        dotnet restore
    ```
3. Agregue un secreto llamado *ConnectionStrings:AppConfig* a Secret Manager.

    Este secreto contiene la cadena de conexión necesaria para acceder a su almacén de App Configuration. Sustituya el valor en el comando siguiente por la cadena de conexión de su almacén de App Configuration.

    Este comando debe ejecutarse en el mismo directorio que el archivo *.csproj*.

    ```CLI
        dotnet user-secrets set ConnectionStrings:AppConfig <your_connection_string>
    ```

    > [!IMPORTANT]
    > Algunos shells truncarán la cadena de conexión, salvo que esté entre comillas. Asegúrese de que la salida del comando `dotnet user-secrets` muestra toda la cadena de conexión. Si no lo hace, vuelva a ejecutar el comando y escriba la cadena de conexión entre comillas.

    Secret Manager solo se utiliza para probar la aplicación web localmente. Cuando la aplicación se implementa (por ejemplo, en [Azure App Service](https://azure.microsoft.com/services/app-service/web)), usará una configuración de la aplicación (por ejemplo, **Cadenas de conexión** en App Service), en lugar de almacenar la cadena de conexión con Secret Manager.

    Se accede a este secreto con la API de configuración. Un signo de dos puntos (:) funciona en el nombre de configuración con la API de configuración en todas las plataformas compatibles. Consulte [Configuración en ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/index?tabs=basicconfiguration&view=aspnetcore-2.0).

4. Abra *Program.cs* y agregue una referencia al proveedor de configuración de la aplicación .NET Core.

    ```csharp
    using Microsoft.Extensions.Configuration.AzureAppConfiguration;
    ```

5. Actualice el método `CreateWebHostBuilder` para usar App Configuration; para ello, llame al método `config.AddAzureAppConfiguration()`.
    
    > [!IMPORTANT]
    > `CreateHostBuilder` reemplaza a `CreateWebHostBuilder` en .NET Core 3.0.  Seleccione la sintaxis correcta en función de su entorno.

    ### <a name="update-createwebhostbuilder-for-net-core-2x"></a>Actualización `CreateWebHostBuilder` para .NET Core 2.x

    ```csharp
    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                var settings = config.Build();
                config.AddAzureAppConfiguration(settings["ConnectionStrings:AppConfig"]);
            })
            .UseStartup<Startup>();
    ```

    ### <a name="update-createhostbuilder-for-net-core-3x"></a>Actualización `CreateHostBuilder` para .NET Core 3.x

    ```csharp
    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        webBuilder.ConfigureAppConfiguration((hostingContext, config) =>
        {
            var settings = config.Build();
            config.AddAzureAppConfiguration(settings["ConnectionStrings:AppConfig"]);
        })
        .UseStartup<Startup>());
    ```

6. Abra el archivo *Index.cshtml*, que se encuentra en el directorio Views > Home, y sustituya su contenido por el siguiente código:

    ```HTML
    @using Microsoft.Extensions.Configuration
    @inject IConfiguration Configuration

    <style>
        body {
            background-color: @Configuration["TestApp:Settings:BackgroundColor"]
        }
        h1 {
            color: @Configuration["TestApp:Settings:FontColor"];
            font-size: @Configuration["TestApp:Settings:FontSize"];
        }
    </style>

    <h1>@Configuration["TestApp:Settings:Message"]</h1>
    ```

7. Abra el archivo *_Layout.cshtml*, que se encuentra en el directorio Views > Shared, y sustituya su contenido por el siguiente código:

    ```HTML
    <!DOCTYPE html>
    <html>
    <head>
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>@ViewData["Title"] - hello_world</title>

        <link rel="stylesheet" href="~/lib/bootstrap/dist/css/bootstrap.css" />
        <link rel="stylesheet" href="~/css/site.css" />
    </head>
    <body>
        <div class="container body-content">
            @RenderBody()
        </div>

        <script src="~/lib/jquery/dist/jquery.js"></script>
        <script src="~/lib/bootstrap/dist/js/bootstrap.js"></script>
        <script src="~/js/site.js" asp-append-version="true"></script>

        @RenderSection("Scripts", required: false)
    </body>
    </html>
    ```

## <a name="build-and-run-the-app-locally"></a>Compilación y ejecución de la aplicación en un entorno local

1. Para compilar la aplicación mediante la CLI de .NET Core, ejecute el siguiente comando en el shell de comandos:

    ```CLI
       dotnet build
    ```

2. Una vez que la compilación se haya realizado correctamente, ejecute el siguiente comando para ejecutar la aplicación web localmente:

    ```CLI
        dotnet run
    ```

3. Inicie una ventana del explorador y vaya a `http://localhost:5000`, que es la dirección URL predeterminada de la aplicación web hospedada localmente.

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido, ha creado un almacén de App Configuration y lo ha usado con una aplicación web de ASP.NET Core a través del [proveedor de App Configuration](https://go.microsoft.com/fwlink/?linkid=2074664). Para aprender a configurar una aplicación de ASP.NET Core para actualizar dinámicamente la configuración, vaya al siguiente tutorial.

> [!div class="nextstepaction"]
> [Habilitación de la configuración dinámica](./enable-dynamic-configuration-aspnet-core.md)
