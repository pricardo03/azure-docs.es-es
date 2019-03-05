---
title: Tutorial sobre el uso de la configuración dinámica de Azure App Configuration en una aplicación de ASP.NET Core | Microsoft Docs
description: En este tutorial aprenderá a actualizar dinámicamente los datos de configuración de aplicaciones de ASP.NET Core
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.workload: tbd
ms.devlang: na
ms.topic: tutorial
ms.date: 02/24/2019
ms.author: yegu
ms.custom: mvc
ms.openlocfilehash: 44ae922b182874eef378d4868fb278c3c76252db
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/26/2019
ms.locfileid: "56884419"
---
# <a name="tutorial-use-dynamic-configuration-in-an-aspnet-core-app"></a>Tutorial: Uso de la configuración dinámica en una aplicación de ASP.NET Core

ASP.NET Core tiene un sistema de configuración conectable que es capaz de leer datos de configuración procedentes de una variedad de orígenes, así como administrar los cambios sobre la marcha sin hacer que la aplicación tenga que reiniciarse. Admite el enlace de valores de configuración a clases .NET fuertemente tipadas y las inserta en el código mediante diversos patrones `IOptions<T>`. Uno de estos patrones, concretamente `IOptionsSnapshot<T>`, proporciona la recarga automática de la configuración de la aplicación cuando cambian los datos subyacentes. Puede insertar `IOptionsSnapshot<T>` en controladores de la aplicación para acceder a la configuración más reciente almacenada en Azure App Configuration. Además, puede configurar la biblioteca cliente de ASP.NET Core para App Configuration para que supervise continuamente y recupere cualquier cambio de un almacén de App Configuration mediante la realización de sondeos a intervalos periódicos que se pueden definir.

Este tutorial le muestra cómo puede implementar las actualizaciones de configuración dinámica en el código. Se basa en la aplicación web que se introdujo en los inicios rápidos. Antes de continuar, complete primero [Create an ASP.NET Core app with App Configuration](./quickstart-aspnet-core-app.md) (Creación de una aplicación ASP.NET Core con App Configuration).

Puede usar cualquier editor de código para realizar los pasos de esta guía de inicio rápido. Sin embargo, [Visual Studio Code](https://code.visualstudio.com/) es una excelente opción disponible en las plataformas Windows, macOS y Linux.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Configurar la aplicación para actualizar su configuración en respuesta a los cambios de un almacén de App Configuration
> * Insertar la configuración más reciente en los controladores de la aplicación

## <a name="prerequisites"></a>Requisitos previos

Para completar esta inicio rápido, instale el [SDK de .NET Core](https://dotnet.microsoft.com/download).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="reload-data-from-app-configuration"></a>Recarga de datos de App Configuration

1. Abra *Program.cs* y actualice el método `CreateWebHostBuilder` mediante la incorporación del método `config.AddAzureAppConfiguration()`.

    ```csharp
    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                var settings = config.Build();
                config.AddAzureAppConfiguration(o => o.Connect(settings["ConnectionStrings:AppConfig"])
                    .Watch("TestApp:Settings:BackgroundColor", TimeSpan.FromSeconds(1))
                    .Watch("TestApp:Settings:FontColor", TimeSpan.FromSeconds(1))
                    .Watch("TestApp:Settings:Message", TimeSpan.FromSeconds(1)));
            })
            .UseStartup<Startup>();
    ```

    El segundo parámetro del método `.Watch` es el intervalo de sondeo en el que la biblioteca cliente de ASP.NET consulta un almacén de App Configuration para ver si ha habido algún cambio del valor de configuración específico.

2. Agregue un archivo *Settings.cs* que defina e implemente una nueva clase `Settings`.

    ```csharp
    namespace TestAppConfig
    {
        public class Settings
        {
            public string BackgroundColor { get; set; }
            public long FontSize { get; set; }
            public string FontColor { get; set; }
            public string Message { get; set; }
        }
    }
    ```

3. Abra *Startup.cs* y actualice el método `ConfigureServices` para enlazar los datos de configuración a la clase `Settings`.

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.Configure<Settings>(Configuration.GetSection("TestApp:Settings"));

        services.Configure<CookiePolicyOptions>(options =>
        {
            options.CheckConsentNeeded = context => true;
            options.MinimumSameSitePolicy = SameSiteMode.None;
        });

        services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
    }
    ```

## <a name="use-the-latest-configuration-data"></a>Uso de los datos de configuración más recientes

1. Abra *HomeController.cs* en el directorio *Controllers*, actualice la clase `HomeController` para que reciba `Settings` mediante la inyección de dependencia y use sus valores.

    ```csharp
    public class HomeController : Controller
    {
        private readonly Settings _settings;
        public HomeController(IOptionsSnapshot<Settings> settings)
        {
            _settings = settings.Value;
        }

        public IActionResult Index()
        {
            ViewData["BackgroundColor"] = _settings.BackgroundColor;
            ViewData["FontSize"] = _settings.FontSize;
            ViewData["FontColor"] = _settings.FontColor;
            ViewData["Message"] = _settings.Message;

            return View();
        }
    }
    ```

2. Abra el archivo *Index.cshtml*, que se encuentra en el directorio *Views* > *Home*, y sustituya su contenido por el siguiente:

    ```html
    <!DOCTYPE html>
    <html lang="en">
    <style>
        body {
            background-color: @ViewData["BackgroundColor"]
        }
        h1 {
            color: @ViewData["FontColor"];
            font-size: @ViewData["FontSize"];
        }
    </style>
    <head>
        <title>Index View</title>
    </head>
    <body>
        <h1>@ViewData["Message"]</h1>
    </body>
    </html>
    ```

## <a name="build-and-run-the-app-locally"></a>Compilación y ejecución de la aplicación en un entorno local

1. Para compilar la aplicación mediante la CLI de .NET Core, ejecute el siguiente comando en el shell de comandos:

        dotnet build

2. Una vez que la compilación se complete correctamente, ejecute el siguiente comando para ejecutar la aplicación web localmente:

        dotnet run

3. Inicie una ventana del explorador y vaya a `http://localhost:5000`, que es la dirección URL predeterminada de la aplicación web hospedada localmente.

    ![Inicio de la aplicación del artículo de inicio rápido en un entorno local](./media/quickstarts/aspnet-core-app-launch-local-before.png)

4. Inicie sesión en [Azure Portal](https://aka.ms/azconfig/portal), haga clic en **Todos los recursos** y en el almacén de App Configuration que creó en el inicio rápido.

5. Haga clic en **Explorador de clave-valor** y actualice los valores de las claves siguientes:

    | Clave | Valor |
    |---|---|
    | TestAppSettings:BackgroundColor | blue |
    | TestAppSettings:FontColor | lightGray |
    | TestAppSettings:Message | Datos de Azure App Configuration: ahora con actualizaciones directas |

6. Actualice la página del explorador para ver los nuevos valores de configuración.

    ![Inicio rápido de actualización de la aplicación en el entorno local](./media/quickstarts/aspnet-core-app-launch-local-after.png)

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha agregado una identidad de servicio administrada de Azure para optimizar el acceso a App Configuration y mejorar la administración de credenciales de su aplicación. Para más información sobre App Configuration, continúe con los ejemplos de la CLI de Azure.

> [!div class="nextstepaction"]
> [Ejemplos de CLI](./cli-samples.md)
