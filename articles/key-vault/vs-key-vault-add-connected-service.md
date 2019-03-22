---
title: Adición de compatibilidad con Azure Key Vault en su proyecto de ASP.NET con Visual Studio | Microsoft Docs
description: Use este tutorial para obtener información sobre cómo agregar compatibilidad con Key Vault a una aplicación web de ASP.NET o ASP.NET Core.
services: key-vault
author: ghogen
manager: douge
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 01/02/2019
ms.author: ghogen
ms.openlocfilehash: de849ae290228826ee500ae1c7e623210e585d34
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "58113255"
---
# <a name="add-key-vault-to-your-web-application-by-using-visual-studio-connected-services"></a>Adición de Key Vault a una aplicación web mediante los servicios conectados de Visual Studio

En este tutorial, aprenderá cómo agregar con facilidad todo lo que necesita para empezar a usar Azure Key Vault para administrar sus secretos para proyectos web en Visual Studio, independientemente de que use ASP.NET Core o cualquier tipo de proyecto de ASP.NET. Mediante la característica de servicios conectados de Visual Studio 2017, Visual Studio puede agregar automáticamente todas las opciones de configuración y los paquetes de NuGet que necesita para conectarse a Key Vault en Azure. 

Para obtener información sobre los cambios que los servicios conectados realizan en el proyecto para habilitar Key Vault, vea [Servicio conectado de Key Vault : ¿Qué ha sucedido con mi proyecto de ASP.NET 4.7.1?](vs-key-vault-aspnet-what-happened.md) o [Servicio conectado de Key Vault: ¿Qué ha sucedido con mi proyecto de ASP.NET Core?](vs-key-vault-aspnet-core-what-happened.md)

## <a name="prerequisites"></a>Requisitos previos

- **Una suscripción de Azure**. Si no tiene, puede registrarse para obtener una [cuenta gratuita](https://azure.microsoft.com/pricing/free-trial/).
- **Visual Studio 2017 15.7** con la carga de trabajo de **Desarrollo web** instalado. [Descárguelo ahora](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs).
- Para ASP.NET (no Core),necesita las herramientas de desarrollo de .NET Framework 4.7.1, que no se instalan de forma predeterminada. Para instalarlas, inicie el Instalador de Visual Studio, elija **Modificar** y luego seleccione **Componentes individuales**; después, en el lado derecho, expanda **ASP.NET y desarrollo web** y seleccione **Herramientas de desarrollo de .NET Framework 4.7.1**.
- Abra el proyecto web de ASP.NET 4.7.1 o ASP.NET Core 2.0.

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
3. Luego, abra el archivo de About.cshtml.cs y escriba el código siguiente
   1. Incluya una referencia a Microsoft.Extensions.Configuration mediante esta instrucción using    
       ```
       using Microsoft.Extensions.Configuration
       ```
   2. Agregue este constructor
       ```
       public AboutModel(IConfiguration configuration)
       {
           _configuration = configuration;
       }
       ```
   3. Actualice el método OnGet. Actualice el valor del marcador que se muestra aquí con el nombre del secreto que creó en los comandos anteriores
       ```
       public void OnGet()
       {
           //Message = "Your application description page.";
           Message = "My key val = " + _configuration["<YourSecretNameThatWasCreatedAbove>"];
       }
       ```

Ejecute la aplicación localmente, para lo que debe desplazarse a la página Acerca de. Debería recuperar el valor del secreto

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no necesite el grupo de recursos, elimínelo. De esta forma, se eliminan la instancia de Key Vault y los recursos relacionados. Para eliminar el grupo de recursos mediante el portal:

1. Escriba el nombre del grupo de recursos en el cuadro de búsqueda de la parte superior del portal. Cuando vea el grupo de recursos que se utiliza en esta guía de inicio rápido en los resultados de búsqueda, selecciónelo.
2. Seleccione **Eliminar grupo de recursos**.
3. En el cuadro **ESCRIBA EL NOMBRE DEL GRUPO DE RECURSOS:** escriba el nombre del grupo de recursos y seleccione **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

Puede obtener más información sobre el desarrollo con Key Vault en la [Guía para desarrolladores de Key Vault](key-vault-developers-guide.md).