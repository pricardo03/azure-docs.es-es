---
title: Tutorial sobre el uso de las referencias de Key Vault de Azure App Configuration en una aplicación de ASP.NET Core | Microsoft Docs
description: En este tutorial, aprenderá a usar las referencias de Key Vault de Azure App Configuration desde una aplicación de ASP.NET Core
services: azure-app-configuration
documentationcenter: ''
author: lisaguthrie
manager: maiye
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.workload: tbd
ms.devlang: csharp
ms.topic: tutorial
ms.date: 10/07/2019
ms.author: lcozzens
ms.custom: mvc
ms.openlocfilehash: a14cb3035c159c82df44f686da7f7b78ef942943
ms.sourcegitcommit: f9e81b39693206b824e40d7657d0466246aadd6e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/08/2019
ms.locfileid: "72035820"
---
# <a name="tutorial-use-key-vault-references-in-an-aspnet-core-app"></a>Tutorial: Uso de referencias de Key Vault en una aplicación de ASP.NET Core

En este tutorial, aprenderá a usar el servicio Azure App Configuration junto con Azure Key Vault. Se trata de servicios complementarios, que se usarán de forma conjunta en la mayoría de las implementaciones de aplicaciones. Para ayudarle a usarlas juntas, App Configuration le permite crear claves que hacen referencia a valores almacenados en Key Vault. Cuando lo hace, App Configuration almacena el URI en el valor Key Vault, en lugar del propio valor. La aplicación recupera el valor de esta clave mediante el proveedor de clientes de App Configuration, igual que cualquier otra clave almacenada en App Configuration. El proveedor de clientes lo reconoce como una referencia de Key Vault y llama a este para recuperar el valor. La aplicación es responsable de autenticarse correctamente en App Configuration y en Key Vault. Los dos servicios no se comunican directamente.

Este tutorial le muestra cómo puede implementar las referencias de Key Vault en el código. Se basa en la aplicación web que se introdujo en los inicios rápidos. Antes de continuar, finalice primero el tutorial [Creación de una aplicación ASP.NET Core con Azure App Configuration](./quickstart-aspnet-core-app.md).

Para realizar los pasos de este tutorial, puede usar cualquier editor de código. [Visual Studio Code](https://code.visualstudio.com/) es una excelente opción que está disponible en las plataformas Windows, macOS y Linux.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear una clave de App Configuration que hace referencia a un valor almacenado en Key Vault
> * Acceder al valor de esta clave desde una aplicación web de ASP.NET Core

## <a name="prerequisites"></a>Requisitos previos

Para realizar este tutorial, instale el [SDK de .NET Core](https://dotnet.microsoft.com/download).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-vault"></a>Creación de un almacén

1. Seleccione la opción **Crear un recurso** de la esquina superior izquierda de Azure Portal.

    ![Salida tras completarse la creación de Key Vault](./media/quickstarts/search-services.png)
2. En el cuadro de búsqueda, escriba **Key Vault**.
3. En la lista de resultados, elija **Key Vault**.
4. En la sección Key Vault, elija **Crear**.
5. En la sección **Crear Key Vault**, proporcione la siguiente información:
    - **Nombre**: se requiere un nombre único. En esta guía de inicio rápido se usará **Contoso-vault2**. 
    - **Suscripción**: Elija una suscripción.
    - En **Grupo de recursos** elija **Crear nuevo** y escriba un nombre para el grupo de recursos.
    - En el menú desplegable **Ubicación**, elija una ubicación.
    - Deje las restantes opciones con sus valores predeterminados.
6. Después de proporcionar la información descrita anteriormente, seleccione **Crear**.

En este momento, su cuenta de Azure es la única autorizada para acceder a este nuevo almacén.

![Salida tras completarse la creación de Key Vault](./media/quickstarts/vault-properties.png)

## <a name="add-a-secret-to-key-vault"></a>Incorporación de un secreto a Key Vault

Para agregar un secreto al almacén, simplemente debe realizar un par de pasos adicionales. En este caso, vamos a agregar un mensaje que se puede usar para probar la recuperación de Key Vault. El mensaje se denomina **Message** y vamos a almacenar el valor **Hello from Key Vault** en él.

1. En las páginas de propiedades de Key Vault, seleccione **Secretos**.
1. Haga clic en **Generar o Importar**.
1. En la pantalla **Crear un secreto**, elija los siguientes valores:
    - **Opciones de carga**: Manual.
    - **Nombre**: Message
    - **Valor**: Hello from Key Vault
    - Deje las restantes opciones con sus valores predeterminados. Haga clic en **Create**(Crear).

## <a name="add-a-key-vault-reference-to-app-config"></a>Incorporación de una referencia de Key Vault a App Configuration

1. Inicie sesión en el [Azure Portal](https://portal.azure.com). Seleccione **Todos los recursos**y seleccione la instancia de almacén de App Configuration que creó en el inicio rápido.

1. Haga clic en el **Explorador de Configuration**.

1. Haga clic en **+ Crear** > **Key vault reference** (Referencia de Key Vault) y elija los siguientes valores:
    - **Clave**: TestApp:Settings:KeyVaultMessage
    - **Etiqueta**: Déjelo en blanco
    - **Suscripción**, **Grupo de recursos**, **Key Vault**: Elija las opciones correspondientes a la instancia de Key Vault que creó en la sección anterior.
    - **Secreto**: Seleccione el secreto llamado **Message** que creó en la sección anterior.

## <a name="connect-to-key-vault"></a>Conexión a Key Vault

1. En este tutorial, usará una entidad de servicio para la autenticación en KeyVault. Para crear esta entidad de servicio, use el comando [az ad sp create-for-rbac](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac) de la CLI de Azure:

    ```azurecli
    az ad sp create-for-rbac -n "http://mySP" --sdk-auth
    ```

    Esta operación devolverá una serie de pares clave-valor. 

    ```console
    {
    "clientId": "7da18cae-779c-41fc-992e-0527854c6583",
    "clientSecret": "b421b443-1669-4cd7-b5b1-394d5c945002",
    "subscriptionId": "443e30da-feca-47c4-b68f-1636b75e16b3",
    "tenantId": "35ad10f1-7799-4766-9acf-f2d946161b77",
    "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
    "resourceManagerEndpointUrl": "https://management.azure.com/",
    "activeDirectoryGraphResourceId": "https://graph.windows.net/",
    "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
    "galleryEndpointUrl": "https://gallery.azure.com/",
    "managementEndpointUrl": "https://management.core.windows.net/"
    }
    ```

1. Ejecute el siguiente comando para permitir que la entidad de servicio acceda al almacén de claves:

        az keyvault set-policy -n <your-unique-keyvault-name> --spn <clientId-of-your-service-principal> --secret-permissions delete get list set --key-permissions create decrypt delete encrypt get list unwrapKey wrapKey

1. Agregue secretos para *clientId* y *clientSecret* en el administrador de secretos. Estos comandos deben ejecutarse en el mismo directorio que el archivo *.csproj*.

        dotnet user-secrets set ConnectionStrings:KeyVaultClientId <clientId-of-your-service-principal>        
        dotnet user-secrets set ConnectionStrings:KeyVaultClientSecret <clientSecret-of-your-service-principal>

> [!NOTE]
> Estas credenciales de Key Vault se usan solo dentro de la aplicación. La aplicación se autentica directamente en Key Vault con estas credenciales. Nunca se pasan al servicio App Configuration.

## <a name="update-your-code-to-use-a-key-vault-reference"></a>Actualización del código para usar una referencia de Key Vault

1. Abra *Program.cs* y agregue referencias a los paquetes necesarios.

    ```csharp
    using Microsoft.Azure.KeyVault;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    ```

1. Actualice el método `CreateWebHostBuilder` para usar App Configuration; para ello, llame al método `config.AddAzureAppConfiguration()`. Incluya la opción `UseAzureKeyVault`, pasando una nueva referencia `KeyVaultClient` a su instancia de Key Vault.

    ```csharp
    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                var settings = config.Build();

                KeyVaultClient kvClient = new KeyVaultClient(async (authority, resource, scope) =>
                {
                    var adCredential = new ClientCredential(settings["ConnectionStrings:KeyVaultClientId"], settings["ConnectionStrings:KeyVaultClientSecret"]);
                    var authenticationContext = new AuthenticationContext(authority, null);
                    return (await authenticationContext.AcquireTokenAsync(resource, adCredential)).AccessToken;
                });

                config.AddAzureAppConfiguration(options => {
                    options.Connect(settings["ConnectionStrings:AppConfig"])
                            .UseAzureKeyVault(kvClient); });
            })
            .UseStartup<Startup>();
    ```

1. Una vez que haya pasado la referencia *KeyVaultClient* al método `UseAzureKeyVault` al inicializar la conexión en App Configuration, puede acceder a los valores de las referencias de Key Vault de la misma manera que accede a los valores de las claves normales de App Configuration. Para ver este proceso en acción, abra *Index.cshtml* en el directorio Views > Home. Sustituya su contenido por el código siguiente:

    ```html
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

    <h1>@Configuration["TestApp:Settings:Message"]
        and @Configuration["TestApp:Settings:KeyVaultMessage"]</h1>
    ```

    Puede acceder al valor de la referencia de Key Vault *TestApp:Settings:KeyVaultMessage* de la misma forma que al valor de configuración *TestApp:Settings:Message*

## <a name="build-and-run-the-app-locally"></a>Compilación y ejecución de la aplicación en un entorno local

1. Para compilar la aplicación mediante la CLI de .NET Core, ejecute el siguiente comando en el shell de comandos:

        dotnet build

2. Una vez que la compilación se haya realizado correctamente, ejecute el siguiente comando para ejecutar la aplicación web localmente:

        dotnet run

3. Inicie una ventana del explorador y vaya a `http://localhost:5000`, que es la dirección URL predeterminada de la aplicación web hospedada localmente.

    ![Inicio de la aplicación del artículo de inicio rápido en un entorno local](./media/key-vault-reference-launch-local.png)


## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha agregado una identidad de servicio administrada de Azure para optimizar el acceso a App Configuration y mejorar la administración de credenciales de su aplicación. Para más información sobre App Configuration, continúe con los ejemplos de la CLI de Azure.

> [!div class="nextstepaction"]
> [Ejemplos de CLI](./cli-samples.md)
