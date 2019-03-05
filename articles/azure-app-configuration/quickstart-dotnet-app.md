---
title: Inicio rápido de Azure App Configuration con .NET Framework | Microsoft Docs
description: Un inicio rápido para el uso de Azure App Configuration con aplicaciones de .NET Framework
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.devlang: csharp
ms.topic: quickstart
ms.tgt_pltfrm: .NET
ms.workload: tbd
ms.date: 02/24/2019
ms.author: yegu
ms.openlocfilehash: b5e41b1f9ee982b8ff8c86232f715d5dab705cd6
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/27/2019
ms.locfileid: "56962169"
---
# <a name="quickstart-create-a-net-framework-app-with-azure-app-configuration"></a>Inicio rápido: Creación de una aplicación de .NET Framework con Azure App Configuration

Azure App Configuration es un servicio de configuración administrado de Azure. Permite almacenar y administrar fácilmente toda la configuración de una aplicación en un solo lugar que está separado del código. En este artículo de inicio rápido se muestra cómo incorporar el servicio en una aplicación de consola de escritorio de Windows basada en .NET Framework.

![Guía de inicio rápido completo local](./media/quickstarts/dotnet-fx-app-run.png)

## <a name="prerequisites"></a>Requisitos previos

Para completar este artículo de inicio rápido, instale [Visual Studio 2017](https://visualstudio.microsoft.com/vs) y [.NET Framework 4.7.1](https://dotnet.microsoft.com/download), o cualquier versión posterior, si no lo ha hecho aún.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-an-app-configuration-store"></a>Creación de un almacén de configuración de aplicaciones

[!INCLUDE [azure-app-configuration-create](../../includes/azure-app-configuration-create.md)]

## <a name="create-a-net-console-app"></a>Creación de una aplicación de consola de .NET

1. Inicie Visual Studio y seleccione **Archivo** > **Nuevo** > **Proyecto**.

2. En el cuadro de diálogo **Nuevo proyecto**, seleccione **Instalado**, expanda **Visual C#**  > **Escritorio de Windows** , seleccione  **Aplicación de consola (.NET Framework)**, escriba el **nombre** del proyecto, elija **.NET Framework 4.7.1**, o superior, y haga clic en **Aceptar**.

## <a name="connect-to-app-configuration-store"></a>Conexión a un almacén de configuración de aplicaciones

1. Haga clic con el botón derecho en el proyecto y seleccione **Administrar paquetes NuGet...** En la pestaña **Examinar**, busque y agregue los siguientes paquetes NuGet al proyecto (seleccione la casilla **Incluir versión preliminar** si no los encuentra).
    ```
    Microsoft.Configuration.ConfigurationBuilders.AzureAppConfiguration 1.0.0 preview or later
    Microsoft.Configuration.ConfigurationBuilders.Environment 2.0.0 preview or later
    ```

2. Actualice el archivo *App.config* del proyecto como sigue:

    ```xml
    <configSections>
        <section name="configBuilders" type="System.Configuration.ConfigurationBuildersSection, System.Configuration, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a" restartOnExternalChanges="false" requirePermission="false" />
    </configSections>

    <configBuilders>
        <builders>
            <add name="MyConfigStore" mode="Greedy" connectionString="${ConnectionString}" type="Microsoft.Configuration.ConfigurationBuilders.AzureAppConfigurationBuilder, Microsoft.Configuration.ConfigurationBuilders.AzureAppConfiguration" />
            <add name="Environment" mode="Greedy" type="Microsoft.Configuration.ConfigurationBuilders.EnvironmentConfigBuilder, Microsoft.Configuration.ConfigurationBuilders.Environment" />
        </builders>
    </configBuilders>

    <appSettings configBuilders="Environment,MyConfigStore">
        <add key="AppName" value="Console App Demo" />
        <add key="ConnectionString" value ="Set via an environment variable - for example, dev, test, staging, or production connection string." />
    </appSettings>
    ```

   Tenga en cuenta que como se va a leer la cadena de conexión de su almacén de configuración de aplicaciones desde la variable de entorno `ConnectionString`, es importante agregar el generador de configuraciones `Environment` antes de `MyConfigStore` en la propiedad `configBuilders` de la sección `appSettings`.

3. Abra *Program.cs* y actualice el método `Main` para usar App Configuration mediante una llamada a `ConfigurationManager`.

    ```csharp
    static void Main(string[] args)
    {
        string message = ConfigurationManager.AppSettings["TestApp:Settings:Message"];

        Console.WriteLine(message);
    }
    ```

## <a name="build-and-run-the-app-locally"></a>Compilación y ejecución de la aplicación en un entorno local

1. Establezca una variable de entorno llamada **ConnectionString** en la cadena de conexión del almacén de configuración de aplicaciones. Si usa el símbolo del sistema de Windows, ejecute el siguiente comando:

        setx ConnectionString "connection-string-of-your-app-configuration-store"

    Si usa Windows PowerShell, ejecute el siguiente comando:

        $Env:ConnectionString = "connection-string-of-your-app-configuration-store"

2. Reinicie Visual Studio para que el cambio surta efecto y, después, presione **Ctrl + F5** en el teclado para compilar y ejecutar la aplicación de consola.

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido, ha creado un almacén de configuración de aplicaciones y lo ha usado con una aplicación de consola de .NET Framework. Para más información acerca del uso de App Configuration, continúe con el siguiente tutorial, ya que en él se muestra cómo realizar la autenticación.

> [!div class="nextstepaction"]
> [Integración de Managed Identities for Azure Resources](./integrate-azure-managed-service-identity.md)
