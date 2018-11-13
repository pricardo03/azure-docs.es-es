---
title: 'Tutorial: Uso de Azure Key Vault con Azure Web App en .NET | Microsoft Docs'
description: Tutorial de configuración de una aplicación ASP.NET Core para que lea un secreto de Key Vault
services: key-vault
documentationcenter: ''
author: prashanthyv
manager: rajvijan
ms.assetid: 0e57f5c7-6f5a-46b7-a18a-043da8ca0d83
ms.service: key-vault
ms.workload: identity
ms.topic: tutorial
ms.date: 09/05/2018
ms.author: pryerram
ms.custom: mvc
ms.openlocfilehash: 9cc22e158a9473b7b60f7e8bcb57174abc1fb8cc
ms.sourcegitcommit: 1b186301dacfe6ad4aa028cfcd2975f35566d756
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2018
ms.locfileid: "51218559"
---
# <a name="tutorial-how-to-use-azure-key-vault-with-azure-web-app-in-net"></a>Tutorial: Uso de Azure Key Vault con Azure Web App en .NET

Azure Key Vault ayuda a proteger secretos como las claves de API, las cadenas de conexión de base de datos necesarias para acceder a las aplicaciones, los servicios y los recursos de TI.

En este tutorial, siga los pasos necesarios para conseguir que una aplicación web de Azure lea información de Azure Key Vault mediante identidades administradas para los recursos de Azure. Este tutorial se basa en [Azure Web Apps](../app-service/app-service-web-overview.md). A continuación aprenderá a:

> [!div class="checklist"]
> * Cree un almacén de claves.
> * Almacenar un secreto en el almacén de claves.
> * Recuperar un secreto del almacén de claves.
> * Crear una aplicación web de Azure.
> * Habilite una [identidad administrada](../active-directory/managed-identities-azure-resources/overview.md) para la aplicación web.
> * Conceder los permisos necesarios para que la aplicación web lea datos del almacén de claves.
> * Ejecutar la aplicación web en Azure.

Antes de que sigamos avanzando, conozca los [conceptos básicos](key-vault-whatis.md#basic-concepts).

## <a name="prerequisites"></a>Requisitos previos

* En Windows:
  * [SDK de .NET Core 2.1 o posterior](https://www.microsoft.com/net/download/windows)

* En Mac:
  * Consulte las [novedades de Visual Studio para Mac](https://visualstudio.microsoft.com/vs/mac/).

* Todas las plataformas:
  * Git ([descargar](https://git-scm.com/downloads)).
  * Una suscripción de Azure. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.
  * [CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) versión 2.0.4 o posterior. Está disponible para Windows, Mac y Linux.
  * [.NET Core](https://www.microsoft.com/net/download/dotnet-core/2.1)

## <a name="what-is-managed-service-identity-and-how-does-it-work"></a>¿Qué es Managed Service Identity y cómo funciona?
 Antes de continuar vamos a entender cómo funciona MSI. Azure Key Vault puede almacenar las credenciales de forma segura para que no estén en el código, pero debe autenticarse en Azure Key Vault para recuperarlas. Para autenticarse en Key Vault, necesita una credencial. Un problema clásico de arranque. Mediante la magia de Azure y Azure AD, MSI proporciona una "identidad de arranque" que facilita mucho el inicio.

Aquí le mostramos cómo funciona. Al habilitar MSI para un servicio de Azure, como Virtual Machines, App Service o Functions, Azure crea una [entidad de servicio](key-vault-whatis.md#basic-concepts) para la instancia del servicio en Azure Active Directory e inserta las credenciales de la primera en la segunda. 

![MSI](media/MSI.png)

A continuación, el código llama a un servicio de metadatos local disponible en el recurso de Azure para obtener un token de acceso.
El código usa el token de acceso que obtiene del MSI_ENDPOINT local para autenticar una instancia del servicio Azure Key Vault. 

Ahora vamos a comenzar el tutorial.

## <a name="log-in-to-azure"></a>Inicio de sesión en Azure

Para iniciar sesión en Azure mediante la CLI de Azure, escriba:

```azurecli
az login
```

## <a name="create-a-resource-group"></a>Crear un grupo de recursos

Cree un grupo de recursos con el comando [az group create](/cli/azure/group#az-group-create). Un grupo de recursos de Azure es un contenedor lógico en el que se implementan y se administran los recursos de Azure.

Seleccione el nombre de un grupo de recursos y rellene el marcador de posición.
En el siguiente ejemplo se crea un grupo de recursos en la ubicación Oeste de EE. UU:

```azurecli
# To list locations: az account list-locations --output table
az group create --name "<YourResourceGroupName>" --location "West US"
```

El grupo de recursos que acaba de crear se utiliza en todo este artículo.

## <a name="create-a-key-vault"></a>Creación de un Almacén de claves

A continuación, creará un almacén de claves en el grupo de recursos creado en el paso anterior. Proporcione la siguiente información:

* Nombre del almacén de claves: el nombre debe ser una cadena de 3 a 24 caracteres y solo puede incluir alguno de estos caracteres: 0-9, a-z, A-Z y "-".
* Nombre del grupo de recursos.
* Ubicación: **Oeste de EE. UU**.

```azurecli
az keyvault create --name "<YourKeyVaultName>" --resource-group "<YourResourceGroupName>" --location "West US"
```

En este momento, su cuenta de Azure es la única autorizada para realizar operaciones en este nuevo almacén.

## <a name="add-a-secret-to-the-key-vault"></a>Incorporación de un secreto al almacén de claves

Estamos agregando un secreto para ayudar a ilustrar cómo funciona. Se puede almacenar una cadena de conexión de SQL o cualquier otra información que necesite guardar de forma segura, pero que esté disponibles para la aplicación.

Escriba los siguientes comandos para crear un secreto en el almacén de claves denominado **AppSecret**. Este secreto almacenará el valor **MySecret**.

```azurecli
az keyvault secret set --vault-name "<YourKeyVaultName>" --name "AppSecret" --value "MySecret"
```

Para ver el valor contenido en el secreto como texto sin formato:

```azurecli
az keyvault secret show --name "AppSecret" --vault-name "<YourKeyVaultName>"
```

Este comando muestra la información secreta, URI incluido. Después de completar estos pasos debe tener un identificador URI para un secreto en un almacén de claves. Tome nota de esta información. La necesitará en otro paso más adelante.

## <a name="create-a-net-core-web-app"></a>Creación de una aplicación web .NET Core

Siga este [tutorial](../app-service/app-service-web-get-started-dotnet.md) para crear una aplicación Web .NET Core y **publicarla** en Azure **O** vea el siguiente vídeo
> [!VIDEO https://www.youtube.com/embed/EdiiEH7P-bU]

## <a name="open-and-edit-the-solution"></a>Apertura y edición de la solución

1. Vaya a Páginas > archivo About.cshtml.cs.
2. Instale estos dos paquetes de Nuget
    - [AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication)
    - [KeyVault](https://www.nuget.org/packages/Microsoft.Azure.KeyVault)
3. Importe el siguiente archivo About.cshtml.cs

    ```
    using Microsoft.Azure.KeyVault;
    using Microsoft.Azure.KeyVault.Models;
    using Microsoft.Azure.Services.AppAuthentication;
    ```
4. El código de la clase AboutModel debería ser similar al siguiente
    ```
    public class AboutModel : PageModel
    {
        public string Message { get; set; }

        public async Task OnGetAsync()
        {
            Message = "Your application description page.";
            int retries = 0;
            bool retry = false;
            try
            {
                /* The below 4 lines of code shows you how to use AppAuthentication library to fetch secrets from your Key Vault*/
                AzureServiceTokenProvider azureServiceTokenProvider = new AzureServiceTokenProvider();
                KeyVaultClient keyVaultClient = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(azureServiceTokenProvider.KeyVaultTokenCallback));
                var secret = await keyVaultClient.GetSecretAsync("https://<YourKeyVaultName>.vault.azure.net/secrets/AppSecret")
                        .ConfigureAwait(false);
                Message = secret.Value;             

                /* The below do while logic is to handle throttling errors thrown by Azure Key Vault. It shows how to do exponential backoff which is the recommended client side throttling*/
                do
                {
                    long waitTime = Math.Min(getWaitTime(retries), 2000000);
                    secret = await keyVaultClient.GetSecretAsync("https://<YourKeyVaultName>.vault.azure.net/secrets/AppSecret")
                        .ConfigureAwait(false);
                    retry = false;
                } 
                while(retry && (retries++ < 10));
            }
            /// <exception cref="KeyVaultErrorException">
            /// Thrown when the operation returned an invalid status code
            /// </exception>
            catch (KeyVaultErrorException keyVaultException)
            {
                Message = keyVaultException.Message;
                if((int)keyVaultException.Response.StatusCode == 429)
                    retry = true;
            }            
        }

        // This method implements exponential backoff incase of 429 errors from Azure Key Vault
        private static long getWaitTime(int retryCount)
        {
            long waitTime = ((long)Math.Pow(2, retryCount) * 100L);
            return waitTime;
        }

        // This method fetches a token from Azure Active Directory which can then be provided to Azure Key Vault to authenticate
        public async Task<string> GetAccessTokenAsync()
        {
            var azureServiceTokenProvider = new AzureServiceTokenProvider();
            string accessToken = await azureServiceTokenProvider.GetAccessTokenAsync("https://vault.azure.net");
            return accessToken;
        }
    }
    ```


## <a name="run-the-app"></a>Ejecución de la aplicación

En el menú principal de Visual Studio 2017, seleccione **Depurar** > **Iniciar** con/sin depurar. Cuando aparezca el explorador, vaya a la página **Acerca de**. Aparecerá el valor de **AppSecret**.

## <a name="enable-a-managed-identity-for-the-web-app"></a>Habilitación de una identidad administrada para la aplicación web

Azure Key Vault proporciona una manera de almacenar de forma segura las credenciales y otras claves y secretos, pero el código tiene que autenticarse en Key Vault para recuperarlos. En [¿Qué es Managed Identities for Azure Resources?](../active-directory/managed-identities-azure-resources/overview.md) se facilita la resolución de este problema, al ofrecer a los servicios de Azure una identidad administrada automáticamente en Azure Active Directory (Azure AD). Puede usar esta identidad para autenticar cualquier servicio que admita la autenticación de Azure AD, incluido Key Vault, sin necesidad de tener credenciales en el código.

1. Vuelva a la CLI de Azure.
2. Ejecute el comando assign-identity para crear la identidad de esta aplicación: 

   ```azurecli
   az webapp identity assign --name "<YourAppName>" --resource-group "<YourResourceGroupName>"
   ```
   Tenga en cuenta que tendrá que reemplazar <YourAppName> por el nombre de la aplicación publicada en Azure, es decir, si el nombre de la aplicación publicada era MyAwesomeapp.azurewebsites.net, reemplace <YourAppName> por MyAwesomeapp
 
 La salida del comando anterior es similar a esta. Anote el valor de PrincipalId al publicar la aplicación en Azure. Debe tener el formato:
   ```
   {
     "principalId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
     "tenantId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
     "type": "SystemAssigned"
   }
  ```
>[!NOTE]
>El comando de este procedimiento equivale a ir al [portal](https://portal.azure.com) y cambiar la configuración de **Identity /System assigned** a **On** en las propiedades de la aplicación web.

## <a name="assign-permissions-to-your-application-to-read-secrets-from-key-vault"></a>Asignación de permisos a una aplicación para leer secretos de Key Vault
        
Luego, ejecute este comando con el nombre del almacén de claves y el valor de **PrincipalId**:

```azurecli

az keyvault set-policy --name '<YourKeyVaultName>' --object-id <PrincipalId> --secret-permissions get list

```

## <a name="publish-the-web-application-to-azure"></a>Publicación de la aplicación web en Azure

Publique esta aplicación en Azure de nuevo para verla en vivo como aplicación web y también para comprobar que puede capturar el valor del secreto.

1. En Visual Studio, seleccione el proyecto **key-vault-dotnet-core-quickstart**.
2. Seleccione **Publicar** > **Iniciar**.
3. Seleccione **Crear**.

En el comando anterior, otorga a la identidad (MSI) de los permisos de App Service para realizar operaciones **get** y **list** en Key Vault. <br />
Ahora, cuando ejecute la aplicación debería ver el valor del secreto recuperado. 

Eso es todo, amigos. Ya ha creado una aplicación web en .NET que almacena y recupera los cambios de los secretos de Key Vault.
