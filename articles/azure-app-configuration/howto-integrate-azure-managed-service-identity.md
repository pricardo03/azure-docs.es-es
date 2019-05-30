---
title: Integrar con Azure administra las identidades | Microsoft Docs
description: Aprenda a usar Azure administra las identidades para autenticarse y obtener acceso a la configuración de la aplicación de Azure
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.workload: tbd
ms.devlang: na
ms.topic: conceptual
ms.date: 02/24/2019
ms.author: yegu
ms.openlocfilehash: 3977991386dbcd07e92f21d1ac541f486b4f7f0a
ms.sourcegitcommit: 51a7669c2d12609f54509dbd78a30eeb852009ae
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/30/2019
ms.locfileid: "66393648"
---
# <a name="integrate-with-azure-managed-identities"></a>Integración con identidades administradas de Azure

Las [identidades administradas](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) de Azure Active Directory ayudan a simplificar la administración de secretos de su aplicación de nube. Con una identidad administrada, puede configurar el código para usar la entidad de servicio que se creó para el servicio de proceso de Azure en que se ejecuta. Usar una identidad administrada en lugar de una credencial diferente que se almacenan en Azure Key Vault o una cadena de conexión local. 

Azure App Configuration y sus bibliotecas cliente .NET Core, .NET y Java Spring incluyen compatibilidad integrada con Managed Service Identity (MSI). Aunque su uso no es necesario, MSI elimina la necesidad de un token de acceso que contenga los secretos. Para acceder al almacén de configuración de la aplicación, el código tiene que conocer únicamente su punto de conexión de servicio. Puede insertar esta dirección URL en el código directamente sin preocuparse de exponer ningún secreto.

En este tutorial se muestra cómo puede aprovechar MSI para acceder a App Configuration. Se basa en la aplicación web que se introdujo en los inicios rápidos. Antes de continuar, finalice primero el tutorial [Creación de una aplicación ASP.NET Core con Azure App Configuration](./quickstart-aspnet-core-app.md).

Para realizar los pasos de este tutorial, puede usar cualquier editor de código. [Visual Studio Code](https://code.visualstudio.com/) es una excelente opción disponible en las plataformas Windows, macOS y Linux.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Conceder a una identidad administrada acceso a App Configuration.
> * Configurar la aplicación para usar una identidad administrada al conectarse a App Configuration.

## <a name="prerequisites"></a>Requisitos previos

Para realizar este tutorial, necesitará lo siguiente:

* [SDK de .NET Core](https://www.microsoft.com/net/download/windows).
* [Azure Cloud Shell configurado](https://docs.microsoft.com/azure/cloud-shell/quickstart).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="add-a-managed-identity"></a>Agregar una identidad administrada

Para configurar una identidad administrada en el portal, primero crea una aplicación como lo hace normalmente y, a continuación, habilita la característica.

1. Cree una aplicación en [Azure Portal](https://portal.azure.com) como lo haría normalmente. Vaya a ella en el portal.

2. Desplácese hacia abajo hasta el grupo **Configuración** en el panel de navegación izquierdo y seleccione **Identidad**.

3. En la pestaña **Asignado por el sistema**, cambie **Estado** a **Activado** y seleccione **Guardar**.

    ![Configurar la identidad administrada en App Service](./media/set-managed-identity-app-service.png)

## <a name="grant-access-to-app-configuration"></a>Conceder acceso a App Configuration

1. En [Azure Portal](https://portal.azure.com), seleccione **Todos los recursos** y seleccione la instancia de almacén de App Configuration que creó en el inicio rápido.

2. Seleccione **Access Control (IAM)** .

3. En la pestaña **Comprobar el acceso**, seleccione **Agregar** en la interfaz de usuario de la tarjeta **Agregar una asignación de roles**.

4. En **Rol**, seleccione **Colaborador**. En **Asignar acceso**, seleccione **App Service** en **Identidad administrada asignada por el sistema**.

5. En **Suscripción**, seleccione su suscripción de Azure. Seleccione el recurso de App Service para la aplicación.

6. Seleccione **Guardar**.

    ![Agregar una identidad administrada](./media/add-managed-identity.png)

## <a name="use-a-managed-identity"></a>Uso de una identidad administrada

1. Abra *appsettings.json*y agregue el siguiente script. Reemplace *\<service_endpoint>* , incluidos los corchetes, por la dirección URL del almacén de configuración de aplicaciones:

    ```json
    "AppConfig": {
        "Endpoint": "<service_endpoint>"
    }
    ```

2. Abra *Program.cs* y actualice el método `CreateWebHostBuilder`; para ello, reemplace el método `config.AddAzureAppConfiguration()`.

    ```csharp
    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                var settings = config.Build();
                config.AddAzureAppConfiguration(options =>
                    options.ConnectWithManagedIdentity(settings["AppConfig:Endpoint"]));
            })
            .UseStartup<Startup>();
    ```

[!INCLUDE [Prepare repository](../../includes/app-service-deploy-prepare-repo.md)]

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

## <a name="deploy-from-local-git"></a>Implementación desde Git local

La manera más fácil de habilitar la implementación de Git local para la aplicación con el servidor de compilación Kudu es utilizar Azure Cloud Shell.

### <a name="configure-a-deployment-user"></a>Configuración de un usuario de implementación

[!INCLUDE [Configure a deployment user](../../includes/configure-deployment-user-no-h.md)]

### <a name="enable-local-git-with-kudu"></a>Habilitación de GIT local con Kudu

Para habilitar la implementación de Git local para la aplicación con el servidor de compilación Kudu, ejecute [`az webapp deployment source config-local-git`](/cli/azure/webapp/deployment/source?view=azure-cli-latest#az-webapp-deployment-source-config-local-git) en Cloud Shell.

```azurecli-interactive
az webapp deployment source config-local-git --name <app_name> --resource-group <group_name>
```

Para crear en su lugar una aplicación habilitada para Git, ejecute [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create) en Cloud Shell con el parámetro `--deployment-local-git`.

```azurecli-interactive
az webapp create --name <app_name> --resource-group <group_name> --plan <plan_name> --deployment-local-git
```

El comando `az webapp create` genera algo similar a la salida siguiente:

```json
Local git is configured with url of 'https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git'
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "deploymentLocalGitUrl": "https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

### <a name="deploy-your-project"></a>Implementación del proyecto

En la _ventana del terminal local_, agregue una instancia remota de Azure al repositorio de Git local. Reemplace _\<url>_ con la dirección URL del Git remoto que obtuvo en [Habilitación de Git para la aplicación](#enable-local-git-with-kudu).

```bash
git remote add azure <url>
```

Inserte en la instancia remota de Azure para implementar la aplicación con el comando siguiente. Cuando se le pida una contraseña, escriba la contraseña que creó en [Configuración de un usuario de implementación](#configure-a-deployment-user). No use la misma contraseña que para iniciar sesión en Azure Portal.

```bash
git push azure master
```

Es posible que vea la automatización específica para el entorno de tiempo de ejecución en la salida, como MSBuild para ASP.NET, `npm install` para Node.js y `pip install` para Python.

### <a name="browse-to-the-azure-web-app"></a>Navegación hasta la aplicación web de Azure

Vaya a la aplicación web mediante un explorador para comprobar que el contenido se ha implementado.

```bash
http://<app_name>.azurewebsites.net
```

![Aplicación que se ejecuta en App Service](../app-service/media/app-service-web-tutorial-dotnetcore-sqldb/azure-app-in-browser.png)

## <a name="use-managed-identity-in-other-languages"></a>Usar identidades administradas en otros idiomas

Los proveedores de App Configuration para .NET Framework y Java Spring también incluyen compatibilidad integrada con identidades administradas. En estos casos, al configurar un proveedor, use el punto de conexión de dirección URL del almacén de configuración de aplicaciones en lugar de su cadena de conexión completa. Por ejemplo, para la aplicación de consola de .NET Framework que se creó en el inicio rápido, especifique la siguiente configuración en el archivo *App.config*:

```xml
    <configSections>
        <section name="configBuilders" type="System.Configuration.ConfigurationBuildersSection, System.Configuration, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a" restartOnExternalChanges="false" requirePermission="false" />
    </configSections>

    <configBuilders>
        <builders>
            <add name="MyConfigStore" mode="Greedy" endpoint="${Endpoint}" type="Microsoft.Configuration.ConfigurationBuilders.AzureAppConfigurationBuilder, Microsoft.Configuration.ConfigurationBuilders.AzureAppConfiguration" />
            <add name="Environment" mode="Greedy" type="Microsoft.Configuration.ConfigurationBuilders.EnvironmentConfigBuilder, Microsoft.Configuration.ConfigurationBuilders.Environment" />
        </builders>
    </configBuilders>

    <appSettings configBuilders="Environment,MyConfigStore">
        <add key="AppName" value="Console App Demo" />
        <add key="Endpoint" value ="Set via an environment variable - for example, dev, test, staging, or production endpoint." />
    </appSettings>
```

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Ejemplos de CLI](./cli-samples.md)
