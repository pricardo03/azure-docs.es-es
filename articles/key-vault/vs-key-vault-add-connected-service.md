---
title: Adición de compatibilidad con Azure Key Vault en su proyecto de ASP.NET con Visual Studio | Microsoft Docs
description: Use este tutorial para obtener información sobre cómo agregar compatibilidad con Key Vault a una aplicación web de ASP.NET o ASP.NET Core.
services: key-vault
author: ghogen
manager: jillfra
ms.prod: visual-studio
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 03/21/2019
ms.author: ghogen
ms.openlocfilehash: d95bd114be712953b79ef5afbb0915173f6de26c
ms.sourcegitcommit: 02d17ef9aff49423bef5b322a9315f7eab86d8ff
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/21/2019
ms.locfileid: "58339285"
---
# <a name="add-key-vault-to-your-web-application-by-using-visual-studio-connected-services"></a>Adición de Key Vault a una aplicación web mediante los servicios conectados de Visual Studio

En este tutorial, aprenderá cómo agregar con facilidad todo lo que necesita para empezar a usar Azure Key Vault para administrar sus secretos para proyectos web en Visual Studio, independientemente de que use ASP.NET Core o cualquier tipo de proyecto de ASP.NET. Mediante el uso de la característica servicios conectados en Visual Studio, puede usar Visual Studio agrega automáticamente todos los paquetes de NuGet y opciones de configuración que necesita para conectarse a Key Vault en Azure. 

Para obtener información sobre los cambios que los servicios conectados realizan en el proyecto para habilitar Key Vault, vea [Servicio conectado de Key Vault : ¿Qué ha sucedido con mi proyecto de ASP.NET 4.7.1?](#how-your-aspnet-framework-project-is-modified) o [Servicio conectado de Key Vault: ¿Qué ha sucedido con mi proyecto de ASP.NET Core?](#how-your-aspnet-core-project-is-modified)

## <a name="prerequisites"></a>Requisitos previos

- **Una suscripción de Azure**. Si no tiene, puede registrarse para obtener una [cuenta gratuita](https://azure.microsoft.com/pricing/free-trial/).
- **Visual Studio de 2019** o **Visual Studio 2017 versión 15.7** con el **desarrollo Web** carga de trabajo instalada. [Descárguelo ahora](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs).
- Para ASP.NET (no Core) con Visual Studio 2017, necesita .NET Framework 4.7.1 o herramientas de desarrollo posteriores, que no se instalan de forma predeterminada. Para instalarlas, inicie el Instalador de Visual Studio, elija **Modificar** y luego seleccione **Componentes individuales**; después, en el lado derecho, expanda **ASP.NET y desarrollo web** y seleccione **Herramientas de desarrollo de .NET Framework 4.7.1**.
- ASP.NET 4.7.1 o versiones posteriores, o abra el proyecto web ASP.NET Core 2.0.

## <a name="add-key-vault-support-to-your-project"></a>Adición de la compatibilidad con Key Vault a un proyecto

1. En el **Explorador de soluciones**, elija **Agregar** > **Servicio conectado**.
   Aparece la página del servicio conectado con los servicios que puede agregar al proyecto.
1. En el menú de servicios disponibles, elija **Proteja los secretos con Azure Key Vault**.

   ![Selección de "Proteja los secretos con Azure Key Vault"](media/vs-key-vault-add-connected-service/KeyVaultConnectedService1.PNG)

   Si ha iniciado sesión en Visual Studio y tiene una suscripción de Azure asociada a su cuenta, aparece una página con una lista desplegable con las suscripciones. Asegúrese de que ha iniciado sesión en Visual Studio con la misma cuenta que usó para la suscripción a Azure.

1. Seleccione la suscripción que desea usar y luego elija una instancia de Key Vault nueva o existente o seleccione el vínculo Editar para modificar el nombre generado automáticamente.

   ![Seleccione su suscripción.](media/vs-key-vault-add-connected-service/KeyVaultConnectedService3.PNG)

1. Escriba el nombre que desea usar para la instancia de Key Vault.

   ![Cambiar el nombre de la instancia de Key Vault y elegir un grupo de recursos](media/vs-key-vault-add-connected-service/KeyVaultConnectedService-Edit.PNG)

1. Seleccione un grupo de recursos existente o elija crear uno nuevo con un nombre único generado automáticamente.  Si desea crear un grupo con un nombre diferente, puede usar [Azure Portal](https://portal.azure.com) y luego cerrar la página y reiniciar la recarga de la lista de grupos de recursos.
1. Elija la región en que va a crear la instancia de Key Vault. Si la aplicación web se hospeda en Azure, elija la región que hospeda la aplicación web para lograr un rendimiento óptimo.
1. Elija un modelo de precios. Para más información, vea los [precios de Key Vault ](https://azure.microsoft.com/pricing/details/key-vault/).
1. Haga clic en Aceptar para aceptar las opciones de configuración.
1. Seleccione **Agregar** para crear la instancia de Key Vault. El proceso de creación puede producir un error si elige un nombre que ya se ha utilizado.  Si esto ocurre, use el vínculo **Editar** para cambiar el nombre de la instancia de Key Vault y vuelva a intentarlo.

   ![Adición del servicio conectado al proyecto](media/vs-key-vault-add-connected-service/KeyVaultConnectedService4.PNG)

1. Ahora, agregue un secreto a Key Vault en Azure. Para localizar la ubicación correcta en Azure Portal, haga clic en el vínculo para administrar los secretos almacenados en esta instancia de Key Vault. Si cerró la página o el proyecto, puede ir a ellos en [Azure Portal](https://portal.azure.com); para ello, seleccione **Todos los servicios**, en **Seguridad** elija **Key Vault** y luego seleccione la instancia de Key Vault que ha creado.

   ![Navegación hasta Azure Portal](media/vs-key-vault-add-connected-service/manage-secrets-link.jpg)

1. En la sección Key Vault del almacén de claves creado, seleccione **Secretos** y luego **Generar/Importar**.

   ![Generar/Importar un secreto](media/vs-key-vault-add-connected-service/generate-secrets.jpg)

1. Escriba un secreto, como "MySecret", asígnele cualquier valor de cadena para probar y seleccione el botón **Crear**.

   ![Crear un secreto](media/vs-key-vault-add-connected-service/create-a-secret.jpg)

1. (Opcional) Escriba otro secreto, pero, en esta ocasión, asígnele una categoría asignándole el nombre "Secretos--MiSecreto". Esta sintaxis especifica una categoría "Secretos" que contiene un secreto "MiSecreto".
 
Ahora, puede acceder a los secretos en el código. Los pasos siguientes son distintos en función de si usa ASP.NET 4.7.1 o ASP.NET Core.

## <a name="access-your-secrets-in-code"></a>Acceso a los secretos del código

1. Instale estos dos paquetes NuGet, las bibliotecas de NuGet [AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) y [KeyVault](https://www.nuget.org/packages/Microsoft.Azure.KeyVault).

2. Abra el archivo Program.cs y sustituya el código por el siguiente: 
   ```
    public class Program
    {
        public static void Main(string[] args)
        {
            BuildWebHost(args).Run();
        }

        public static IWebHost BuildWebHost(string[] args) =>
           WebHost.CreateDefaultBuilder(args)
               .ConfigureAppConfiguration((ctx, builder) =>
               {
                   var keyVaultEndpoint = GetKeyVaultEndpoint();
                   if (!string.IsNullOrEmpty(keyVaultEndpoint))
                   {
                       var azureServiceTokenProvider = new AzureServiceTokenProvider();
                       var keyVaultClient = new KeyVaultClient(
                           new KeyVaultClient.AuthenticationCallback(
                               azureServiceTokenProvider.KeyVaultTokenCallback));
                       builder.AddAzureKeyVault(
                           keyVaultEndpoint, keyVaultClient, new DefaultKeyVaultSecretManager());
                   }
               }
            ).UseStartup<Startup>()
             .Build();

        private static string GetKeyVaultEndpoint() => "https://<YourKeyVaultName>.vault.azure.net";
    }
   ```

3. A continuación, abra el archivo de About.cshtml.cs y escribir el código siguiente:
   1. Incluir una referencia a Microsoft.Extensions.Configuration por esta instrucción using:

       ```csharp
       using Microsoft.Extensions.Configuration
       ```

   1. Agregue este constructor:

       ```csharp
       public AboutModel(IConfiguration configuration)
       {
           _configuration = configuration;
       }
       ```

   1. Actualice el método OnGet. Actualice el valor de marcador de posición que se muestra a continuación con el nombre del secreto que creó en los comandos anteriores.

       ```csharp
       public void OnGet()
       {
           //Message = "Your application description page.";
           Message = "My key val = " + _configuration["<YourSecretNameThatWasCreatedAbove>"];
       }
       ```

Ejecute la aplicación localmente, vaya a la página About. Debería ver el valor del secreto recuperado.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no necesite el grupo de recursos, elimínelo. De esta forma, se eliminan la instancia de Key Vault y los recursos relacionados. Para eliminar el grupo de recursos mediante el portal:

1. Escriba el nombre del grupo de recursos en el cuadro de búsqueda de la parte superior del portal. Cuando vea el grupo de recursos que se utiliza en esta guía de inicio rápido en los resultados de búsqueda, selecciónelo.
2. Seleccione **Eliminar grupo de recursos**.
3. En el cuadro **ESCRIBA EL NOMBRE DEL GRUPO DE RECURSOS:** escriba el nombre del grupo de recursos y seleccione **Eliminar**.

## <a name="how-your-aspnet-core-project-is-modified"></a>¿Cómo se modifica el proyecto de ASP.NET Core

Esta sección identifican los cambios exactos realizados en un proyecto de ASP.NET al agregar el almacén de claves conectados service con Visual Studio.

### <a name="added-references"></a>Referencias agregadas

Afecta a las referencias de .NET del archivo de proyecto y referencias de paquetes de NuGet.

| Type | Referencia |
| --- | --- |
| NuGet | Microsoft.AspNetCore.AzureKeyVault.HostingStartup |

### <a name="added-files"></a>Archivos agregados

- ConnectedService.json agregado, que registra información sobre el proveedor de servicio conectado, la versión y un vínculo a la documentación.

### <a name="project-file-changes"></a>Cambios del archivo de proyecto

- Agrega el archivo ConnectedServices.json y ItemGroup de los servicios conectados.

### <a name="launchsettingsjson-changes"></a>Cambios en launchsettings.json

- Agrega las siguientes entradas de la variable de entorno al perfil de IIS Express y perfil que coincide con el nombre del proyecto web:

    ```json
      "environmentVariables": {
        "ASPNETCORE_HOSTINGSTARTUP__KEYVAULT__CONFIGURATIONENABLED": "true",
        "ASPNETCORE_HOSTINGSTARTUP__KEYVAULT__CONFIGURATIONVAULT": "<your keyvault URL>"
      }
    ```

### <a name="changes-on-azure"></a>Cambios en Azure

- Se crea un grupo de recursos o se usa uno existente.
- Se crea una instancia de Key Vault en el grupo de recursos especificado.

## <a name="how-your-aspnet-framework-project-is-modified"></a>¿Cómo se modifica el proyecto de ASP.NET Framework

Esta sección identifican los cambios exactos realizados en un proyecto de ASP.NET al agregar el almacén de claves conectados service con Visual Studio.

### <a name="added-references"></a>Referencias agregadas

Afecta a las referencias de .NET del archivo de proyecto y `packages.config` (referencias de NuGet).

| Type | Referencia |
| --- | --- |
| .NET; NuGet | Microsoft.Azure.KeyVault |
| .NET; NuGet | Microsoft.Azure.KeyVault.WebKey |
| .NET; NuGet | Microsoft.Rest.ClientRuntime |
| .NET; NuGet | Microsoft.Rest.ClientRuntime.Azure |

### <a name="added-files"></a>Archivos agregados

- ConnectedService.json agregado, que registra información sobre el proveedor de servicio conectado, la versión y un vínculo a la documentación.

### <a name="project-file-changes"></a>Cambios del archivo de proyecto

- Agrega el archivo ConnectedServices.json y ItemGroup de los servicios conectados.
- Las referencias a los ensamblados de .NET se describen en la sección de [referencias agregadas](#added-references).

### <a name="webconfig-or-appconfig-changes"></a>Cambios de web.config o app.config

- Se agregaron las siguientes entradas de configuración:

    ```xml
    <configSections>
      <section
           name="configBuilders"
           type="System.Configuration.ConfigurationBuildersSection, System.Configuration, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a" 
           restartOnExternalChanges="false"
           requirePermission="false" />
    </configSections>
    <configBuilders>
      <builders>
        <add 
             name="AzureKeyVault"
             vaultName="vaultname"
             type="Microsoft.Configuration.ConfigurationBuilders.AzureKeyVaultConfigBuilder, Microsoft.Configuration.ConfigurationBuilders.Azure, Version=1.0.0.0, Culture=neutral" 
             vaultUri="https://vaultname.vault.azure.net" />
      </builders>
    </configBuilders>
    ```

### <a name="changes-on-azure"></a>Cambios en Azure

- Se crea un grupo de recursos o se usa uno existente.
- Se crea una instancia de Key Vault en el grupo de recursos especificado.

## <a name="next-steps"></a>Pasos siguientes

Puede obtener más información sobre el desarrollo con Key Vault en la [Guía para desarrolladores de Key Vault](key-vault-developers-guide.md).