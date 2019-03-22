---
title: Integrar con una canalización de integración y entrega continua mediante la configuración de la aplicación de Azure | Microsoft Docs
description: Obtenga información sobre cómo generar un archivo de configuración mediante los datos de configuración de la aplicación de Azure durante la integración y entrega continuas
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.topic: conceptual
ms.date: 02/24/2019
ms.author: yegu
ms.custom: mvc
ms.openlocfilehash: cb9fe6dc234c317daa5eabec01812324e7c81663
ms.sourcegitcommit: 12d67f9e4956bb30e7ca55209dd15d51a692d4f6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2019
ms.locfileid: "58224335"
---
# <a name="integrate-with-a-cicd-pipeline"></a>Integración con una canalización de CI/CD

Puede mejorar la resistencia de la aplicación contra la posibilidad de no ser capaz de conectarse a la configuración de la aplicación de Azure remota. Para ello, empaquete los datos de configuración actual en un archivo que se implementa con la aplicación y cargar localmente durante su inicio. Este enfoque garantiza que la aplicación tiene al menos los valores de configuración predeterminados. Estos valores se sobrescriben con los cambios más recientes de un almacén de configuración de aplicación cuando esté disponible.

Mediante el [exportar](./howto-import-export-data.md#export-data) función de la configuración de aplicaciones de Azure, puede automatizar el proceso de recuperación de datos de configuración actual como un único archivo. A continuación, inserte este archivo en un paso de compilación o implementación en la canalización de implementación continua (CI/CD) e integración continua.

En el ejemplo siguiente se muestra cómo incluir la configuración de aplicaciones de datos como una compilación del paso de la aplicación web que se introdujo en los inicios rápidos. Antes de continuar, finalizar [crear una aplicación ASP.NET Core con configuración de la aplicación](./quickstart-aspnet-core-app.md) primero.

Puede usar cualquier editor de código para realizar los pasos descritos en esta guía de inicio rápido. [Código de Visual Studio](https://code.visualstudio.com/) es una excelente opción disponible en las plataformas de Linux, macOS y Windows.

## <a name="prerequisites"></a>Requisitos previos

Si compila localmente, descargue e instale el [CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) si no lo ha hecho ya.

Para realizar una compilación en la nube, con Azure DevOps, por ejemplo, asegúrese de que el [CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) está instalado en el sistema de compilación.

## <a name="export-an-app-configuration-store"></a>Exportar un almacén de configuración de aplicación

1. Abra su *.csproj* y agréguele el siguiente script:

    ```xml
    <Target Name="Export file" AfterTargets="Build">
        <Message Text="Export the configurations to a temp file. " />
        <Exec WorkingDirectory="$(MSBuildProjectDirectory)" Condition="$(ConnectionString) != ''" Command="az appconfig kv export -f $(OutDir)\azureappconfig.json --format json --separator : --connection-string $(ConnectionString)" />
    </Target>
    ```

    Agregar el *ConnectionString* asociado con el almacén de configuración de la aplicación como una variable de entorno.

2. Abra Program.cs y actualice el `CreateWebHostBuilder` método que se usará el archivo JSON exportado mediante una llamada a la `config.AddJsonFile()` método.

    ```csharp
    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                var directory = System.IO.Path.GetDirectoryName(Assembly.GetExecutingAssembly().Location);
                var settings = config.Build();

                config.AddJsonFile(Path.Combine(directory, "azureappconfig.json"));
                config.AddAzureAppConfiguration(settings["ConnectionStrings:AppConfig"]);
            })
            .UseStartup<Startup>();
    ```

## <a name="build-and-run-the-app-locally"></a>Compilación y ejecución de la aplicación en un entorno local

1. Establezca una variable de entorno denominada **ConnectionString**y establézcalo en la clave de acceso a su almacén de configuración de aplicación. Si usa el símbolo del sistema de Windows, ejecute el comando siguiente y reinicie el símbolo del sistema para permitir que el cambio surta efecto:

        setx ConnectionString "connection-string-of-your-app-configuration-store"

    Si usa Windows PowerShell, ejecute el siguiente comando:

        $Env:ConnectionString = "connection-string-of-your-app-configuration-store"

    Si usa macOS o Linux, ejecute el siguiente comando:

        export ConnectionString='connection-string-of-your-app-configuration-store'

2. Para compilar la aplicación mediante la CLI de .NET Core, ejecute el siguiente comando en el shell de comandos:

        dotnet build

3. Después de la compilación se complete correctamente, ejecute el siguiente comando para ejecutar la aplicación web localmente:

        dotnet run

4. Abra una ventana del explorador y vaya a `http://localhost:5000`, que es la dirección URL predeterminada para la aplicación web hospedada localmente.

    ![Inicio de la aplicación del artículo de inicio rápido en un entorno local](./media/quickstarts/aspnet-core-app-launch-local.png)

## <a name="next-steps"></a>Pasos siguientes

* [Identidades administradas para la integración de los recursos de Azure](./integrate-azure-managed-service-identity.md)
