---
title: Adición de compatibilidad con Azure Key Vault en su proyecto de ASP.NET con Visual Studio | Microsoft Docs
description: Use este tutorial para obtener información sobre cómo agregar compatibilidad con Key Vault a una aplicación web de ASP.NET o ASP.NET Core.
services: key-vault
author: ghogen
manager: jillfra
ms.service: key-vault
ms.custom: vs-azure
ms.topic: conceptual
ms.date: 08/07/2019
ms.author: ghogen
ms.openlocfilehash: 4cbc4044b5d1270cecd1a271d2a1db02801650dd
ms.sourcegitcommit: ae8b23ab3488a2bbbf4c7ad49e285352f2d67a68
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2019
ms.locfileid: "74012766"
---
# <a name="add-key-vault-to-your-web-application-by-using-visual-studio-connected-services"></a>Adición de Key Vault a una aplicación web mediante los servicios conectados de Visual Studio

En este tutorial, aprenderá cómo agregar con facilidad todo lo que necesita para empezar a usar Azure Key Vault para administrar sus secretos para proyectos web en Visual Studio, independientemente de que use ASP.NET Core o cualquier tipo de proyecto de ASP.NET. Mediante la característica de servicios conectados de Visual Studio, Visual Studio puede agregar automáticamente todas las opciones de configuración y los paquetes de NuGet que necesita para conectarse a Key Vault en Azure.

Para obtener información sobre los cambios que los servicios conectados realizan en el proyecto para habilitar Key Vault, vea [Servicio conectado de Key Vault : ¿Qué ha sucedido con mi proyecto de ASP.NET 4.7.1?](#how-your-aspnet-framework-project-is-modified) o [Servicio conectado de Key Vault: ¿Qué ha sucedido con mi proyecto de ASP.NET Core?](#how-your-aspnet-core-project-is-modified)

## <a name="prerequisites"></a>Requisitos previos

- **Una suscripción de Azure**. Si aún no tiene una suscripción, regístrese para obtener una [cuenta gratuita](https://azure.microsoft.com/pricing/free-trial/).
- **Visual Studio 2019 versión 16.3 Preview 1** o **Visual Studio 2017 versión 15.7** con la carga de trabajo de **Desarrollo Web** instalada. [Descárguelo ahora](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs).
- Para ASP.NET (no Core) con Visual Studio 2017,necesita las herramientas de desarrollo de .NET Framework 4.7.1 o posteriores, que no se instalan de forma predeterminada. Para instalarlas, inicie el Instalador de Visual Studio, elija **Modificar** y luego seleccione **Componentes individuales**; después, en el lado derecho, expanda **ASP.NET y desarrollo web** y seleccione **Herramientas de desarrollo de .NET Framework 4.7.1**.
- Abra un proyecto web de ASP.NET 4.7.1 o posterior, o de ASP.NET Core 2.0.

## <a name="add-key-vault-support-to-your-project"></a>Adición de la compatibilidad con Key Vault a un proyecto

Antes de empezar, asegúrese de que ha iniciado sesión en Visual Studio. Inicie sesión con la misma cuenta que usa para su suscripción de Azure. A continuación, abra un proyecto web de ASP.NET 4.7.1 o una versión posterior, o ASP.NET Core 2.0, y realice los pasos siguientes:

1. En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto al que quiere agregar la compatibilidad con el almacén de claves y elija **Agregar** > **Servicio conectado**.
   Aparece la página del servicio conectado con los servicios que puede agregar al proyecto.
1. En el menú de servicios disponibles, elija **Proteja los secretos con Azure Key Vault**.

   ![Selección de "Proteja los secretos con Azure Key Vault"](media/vs-key-vault-add-connected-service/KeyVaultConnectedService1.PNG)

1. Seleccione la suscripción que desee utilizar y después elija una instancia nueva o existente de Key Vault. Si elige una instancia nueva de Key Vault, aparece un vínculo **Editar**. Selecciónelo para configurar la nueva instancia de Key Vault.

   ![Seleccione su suscripción.](media/vs-key-vault-add-connected-service/key-vault-connected-service-select-vault.png)

1. En **Editar Azure Key Vault**, escriba el nombre que desea usar para la instancia de Key Vault.

1. Seleccione un **grupo de recursos** existente o elija crear uno con un nombre único generado automáticamente.  Si desea crear un grupo con un nombre diferente, puede usar [Azure Portal](https://portal.azure.com) y luego cerrar la página y reiniciar la recarga de la lista de grupos de recursos.
1. Elija la **ubicación** en la que va a crear la instancia de Key Vault. Si la aplicación web se hospeda en Azure, elija la región que hospeda la aplicación web para lograr un rendimiento óptimo.
1. Elija un **plan de tarifa**. Para más información, vea los [precios de Key Vault ](https://azure.microsoft.com/pricing/details/key-vault/).
1. Elija **Aceptar** para aceptar las opciones de configuración.
1. Después de seleccionar una instancia de Key Vault existente o configurar una nueva, en la pestaña **Azure Key Vault** de Visual Studio, seleccione **Agregar** para agregar Servicios conectados.
1. Seleccione el vínculo **Administrar el secreto almacenado en esta instancia de Key Vault** para abrir la página **Secretos** de su instancia de Key Vault. Si cerró la página o el proyecto, puede ir a ellos en [Azure Portal](https://portal.azure.com); para ello, elija **Todos los servicios**, en **Seguridad**, elija **Key Vault** y luego seleccione la instancia de Key Vault.
1. En la sección Key Vault del almacén de claves creado, seleccione **Secretos** y luego **Generar/Importar**.

   ![Generar/Importar un secreto](media/vs-key-vault-add-connected-service/azure-generate-secrets.png)

1. Escriba un secreto, como *MiSecreto* y asígnele cualquier valor de cadena para probar; después, seleccione el botón **Crear**.

   ![Crear un secreto](media/vs-key-vault-add-connected-service/azure-create-a-secret.png)

1. (Opcional) Escriba otro secreto, pero, en esta ocasión, asígnele una categoría asignándole el nombre *Secretos--MiSecreto*. Esta sintaxis especifica una categoría "Secretos" que contiene un secreto "MiSecreto".

Ahora, puede acceder a los secretos en el código. Los pasos siguientes son distintos en función de si usa ASP.NET 4.7.1 o ASP.NET Core.

## <a name="access-your-secrets-in-code"></a>Acceso a los secretos del código

1. En el Explorador de soluciones, haga clic con el botón derecho en el proyecto y seleccione **Administrar paquetes NuGet**. En la pestaña **Examinar**, busque e instale estos dos paquetes NuGet: [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) y [Microsoft.Azure.KeyVault](https://www.nuget.org/packages/Microsoft.Azure.KeyVault).

1. Seleccione la pestaña `Program.cs` y reemplace la clase Program por el código siguiente:

   ```csharp
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

1. Luego, abra el archivo `About.cshtml.cs` y escriba el código siguiente:
   1. Incluya una referencia a `Microsoft.Extensions.Configuration` mediante esta instrucción using:

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

   1. Actualice el método `OnGet`. Actualice el valor del marcador que se muestra aquí con el nombre del secreto que creó en los comandos anteriores.

       ```csharp
       public void OnGet()
       {
           //Message = "Your application description page.";
           Message = "My key val = " + _configuration["<YourSecretNameThatWasCreatedAbove>"];
       }
       ```

Ejecute la aplicación localmente, para lo que debe desplazarse a la página Acerca de. Debería recuperar el valor del secreto.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no necesite el grupo de recursos, elimínelo. De esta forma, se eliminan la instancia de Key Vault y los recursos relacionados. Para eliminar el grupo de recursos mediante el portal:

1. Escriba el nombre del grupo de recursos en el cuadro de búsqueda de la parte superior del portal. Cuando vea el grupo de recursos que se utiliza en esta guía de inicio rápido en los resultados de búsqueda, selecciónelo.
2. Seleccione **Eliminar grupo de recursos**.
3. En el cuadro **ESCRIBA EL NOMBRE DEL GRUPO DE RECURSOS**, escriba el nombre del grupo de recursos y seleccione **Eliminar**.

## <a name="troubleshooting"></a>solución de problemas

Si el almacén de claves se está ejecutando en una cuenta Microsoft diferente de aquella con la cual ha iniciado sesión en Visual Studio (por ejemplo, el almacén de claves se está ejecutando en su cuenta profesional, pero Visual Studio usa su cuenta privada), recibirá un error en el archivo Program.cs que le indica que Visual Studio no puede acceder al almacén de claves. Para corregir este problema:

1. Vaya a [Azure Portal](https://portal.azure.com) y abra el almacén de claves.

1. Elija **Directivas de acceso**, **Agregar directiva de acceso** y elija la cuenta con la que ha iniciado sesión como entidad de seguridad.

1. En Visual Studio, elija **Archivo** > **Configuración de la cuenta**.
Seleccione **Agregar una cuenta** de la sección **Todas las cuentas**. Inicie sesión con la cuenta que ha elegido como entidad de seguridad de la directiva de acceso.

1. Elija **Herramientas** > **Opciones** y busque **Azure Service Authentication** (Autenticación del servicio de Azure). Después, seleccione la cuenta que acaba de agregar a Visual Studio.

Ahora, al depurar la aplicación, Visual Studio se conectará a la cuenta en la que se encuentra el almacén de claves.

## <a name="how-your-aspnet-core-project-is-modified"></a>Cómo se modifica el proyecto de ASP.NET Core

En esta sección se identifican los cambios exactos realizados en el proyecto de ASP.NET cuando se agrega el servicio conectado de Key Vault mediante Visual Studio.

### <a name="added-references-for-aspnet-core"></a>Se agregan referencias para ASP.NET Core

Afecta a las referencias de paquetes de NuGet y a las referencias de .NET del archivo de proyecto.

| type | Referencia |
| --- | --- |
| NuGet | Microsoft.AspNetCore.AzureKeyVault.HostingStartup |

### <a name="added-files-for-aspnet-core"></a>Se agregan archivos para ASP.NET Core

- Se agrega `ConnectedService.json`, que registra información sobre el proveedor de Servicios conectados, la versión y un vínculo a la documentación.

### <a name="project-file-changes-for-aspnet-core"></a>Cambios en el archivo de proyecto para ASP.NET Core

- Se agrega el archivo `ConnectedServices.json` e ItemGroup de Servicios conectados.

### <a name="launchsettingsjson-changes-for-aspnet-core"></a>Cambios en launchsettings.json para ASP.NET Core

- Agrega las siguientes entradas de la variable de entorno al perfil de IIS Express y perfil que coincide con el nombre del proyecto web:

    ```json
      "environmentVariables": {
        "ASPNETCORE_HOSTINGSTARTUP__KEYVAULT__CONFIGURATIONENABLED": "true",
        "ASPNETCORE_HOSTINGSTARTUP__KEYVAULT__CONFIGURATIONVAULT": "<your keyvault URL>"
      }
    ```

### <a name="changes-on-azure-for-aspnet-core"></a>Cambios en Azure para ASP.NET Core

- Se crea un grupo de recursos o se usa uno existente.
- Se crea una instancia de Key Vault en el grupo de recursos especificado.

## <a name="how-your-aspnet-framework-project-is-modified"></a>Cómo se modifica el proyecto de ASP.NET Framework

En esta sección se identifican los cambios exactos realizados en el proyecto de ASP.NET cuando se agrega el servicio conectado de Key Vault mediante Visual Studio.

### <a name="added-references-for-aspnet-framework"></a>Se agregan referencias para ASP.NET Framework

Afecta a las referencias .NET y a `packages.config` (referencias de NuGet) del archivo del proyecto.

| type | Referencia |
| --- | --- |
| .NET; NuGet | Microsoft.Azure.KeyVault |
| .NET; NuGet | Microsoft.Azure.KeyVault.WebKey |
| .NET; NuGet | Microsoft.Rest.ClientRuntime |
| .NET; NuGet | Microsoft.Rest.ClientRuntime.Azure |

### <a name="added-files-for-aspnet-framework"></a>Se agregan archivos para ASP.NET Framework

- Se agrega `ConnectedService.json`, que registra información sobre el proveedor de Servicios conectados, la versión y un vínculo a la documentación.

### <a name="project-file-changes-for-aspnet-framework"></a>Cambios en el archivo de proyecto para ASP.NET Framework

- Agrega el archivo ConnectedServices.json y ItemGroup de los servicios conectados.
- Las referencias a los ensamblados de .NET se describen en la sección de [referencias agregadas](#added-references-for-aspnet-framework).

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

### <a name="changes-on-azure-for-aspnet-framework"></a>Cambios en Azure para ASP.NET Framework

- Se crea un grupo de recursos o se usa uno existente.
- Se crea una instancia de Key Vault en el grupo de recursos especificado.

## <a name="next-steps"></a>Pasos siguientes

Puede obtener más información sobre el desarrollo con Key Vault en la [Guía del desarrollador de Key Vault](key-vault-developers-guide.md).
