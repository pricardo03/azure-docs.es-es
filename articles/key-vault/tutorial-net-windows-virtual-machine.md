---
title: 'Tutorial: Uso de Azure Key Vault con una máquina virtual Windows de Azure en .NET | Microsoft Docs'
description: Tutorial de configuración de una aplicación ASP.NET Core para que lea un secreto de Key Vault
services: key-vault
documentationcenter: ''
author: prashanthyv
manager: rajvijan
ms.assetid: 0e57f5c7-6f5a-46b7-a18a-043da8ca0d83
ms.service: key-vault
ms.workload: key-vault
ms.topic: tutorial
ms.date: 09/05/2018
ms.author: pryerram
ms.custom: mvc
ms.openlocfilehash: d1f24c8bebc8740f47dc0f02089db1091c22f597
ms.sourcegitcommit: a4e4e0236197544569a0a7e34c1c20d071774dd6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2018
ms.locfileid: "51711334"
---
# <a name="tutorial-how-to-use-azure-key-vault-with-azure-windows-virtual-machine-in-net"></a>Tutorial: Uso de Azure Key Vault con una máquina virtual Windows en .NET

Azure Key Vault ayuda a proteger secretos como las claves de API, las cadenas de conexión de base de datos necesarias para acceder a las aplicaciones, los servicios y los recursos de TI.

En este tutorial, seguirá los pasos necesarios para conseguir que una aplicación de consola lea información de Azure Key Vault mediante Managed Identities for Azure Resources. Este tutorial se basa en [Azure Web Apps](../app-service/app-service-web-overview.md). A continuación aprenderá a:

> [!div class="checklist"]
> * Cree un almacén de claves.
> * Almacenar un secreto en el almacén de claves.
> * Recuperar un secreto del almacén de claves.
> * Crear una máquina virtual de Azure.
> * Habilitar una [identidad administrada](../active-directory/managed-identities-azure-resources/overview.md) para la máquina virtual.
> * Conceder los permisos necesarios para que la aplicación de consola lea datos del almacén de claves.
> * Recuperar secretos del almacén de claves.

Antes de que sigamos avanzando, conozca los [conceptos básicos](key-vault-whatis.md#basic-concepts).

## <a name="prerequisites"></a>Requisitos previos
* Todas las plataformas:
  * Git ([descargar](https://git-scm.com/downloads)).
  * Una suscripción de Azure. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.
  * [CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) versión 2.0.4 o posterior. Está disponible para Windows, Mac y Linux.

En este tutorial se hace uso de Managed Service Identity

## <a name="what-is-managed-service-identity-and-how-does-it-work"></a>¿Qué es Managed Service Identity y cómo funciona?
Antes de continuar vamos a entender cómo funciona MSI. Azure Key Vault puede almacenar las credenciales de forma segura para que no estén en el código, pero debe autenticarse en Azure Key Vault para recuperarlas. Para autenticarse en Key Vault, necesita una credencial. Un problema clásico de arranque. Mediante la magia de Azure y Azure AD, MSI proporciona una "identidad de arranque" que facilita mucho el inicio.

Aquí le mostramos cómo funciona. Al habilitar MSI para un servicio de Azure, como Virtual Machines, App Service o Functions, Azure crea una [entidad de servicio](key-vault-whatis.md#basic-concepts) para la instancia del servicio en Azure Active Directory e inserta las credenciales de la primera en la segunda. 

![MSI](media/MSI.png)

A continuación, el código llama a un servicio local de metadatos disponible en el recurso de Azure para obtener un token de acceso.
El código usa el token de acceso que obtiene del MSI_ENDPOINT local para autenticar una instancia del servicio Azure Key Vault. 

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

## <a name="create-a-virtual-machine"></a>Creación de una máquina virtual
Siga los vínculos de abajo para crear una máquina virtual Windows

[CLI de Azure](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-cli) 

[PowerShell](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-powershell)

[Portal](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal)

## <a name="assign-identity-to-virtual-machine"></a>Asignación de una identidad a la máquina virtual
En este paso vamos a crear una identidad asignada al sistema para la máquina virtual mediante la ejecución del siguiente comando en la CLI de Azure

```
az vm identity assign --name <NameOfYourVirtualMachine> --resource-group <YourResourceGroupName>
```

Fíjese en systemAssignedIdentity, que se muestra a continuación. La salida del comando anterior sería 

```
{
  "systemAssignedIdentity": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "userAssignedIdentities": {}
}
```

## <a name="give-vm-identity-permission-to-key-vault"></a>Concesión del permiso de identidad de máquina virtual al almacén de claves
Ahora se puede conceder el permiso de identidad creado al almacén de claves mediante la ejecución del siguiente comando

```
az keyvault set-policy --name '<YourKeyVaultName>' --object-id <VMSystemAssignedIdentity> --secret-permissions get list
```

## <a name="login-to-the-virtual-machine"></a>Inicio de sesión en la máquina virtual

Puede seguir este [tutorial](https://docs.microsoft.com/azure/virtual-machines/windows/connect-logon)

## <a name="install-net-core"></a>Instalar .NET Core

Para instalar .NET Core, siga los pasos de este [artículo](https://www.microsoft.com/net/download)

## <a name="create-and-run-sample-dot-net-app"></a>Creación y ejecución de la aplicación de Dot Net de ejemplo

Abra un símbolo del sistema.

Mediante la ejecución de los siguientes comandos, debería ver que se imprime "Hola mundo" en la consola

```
dotnet new console -o helloworldapp
cd helloworldapp
dotnet run
```

## <a name="edit-console-app"></a>Edición de la aplicación de consola
Abra el archivo Program.cs y agregue estos paquetes
```
using System;
using System.IO;
using System.Net;
using System.Text;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
```
Luego, cambie el archivo de clase para que contenga el siguiente código. Este es un proceso de 2 pasos. 
1. Capture un token del punto de conexión del MSI local de la máquina virtual que, a su vez, captura un token de Azure Active Directory
2. Pase el token a Key Vault y capture el secreto 

```
 class Program
    {
        static void Main(string[] args)
        {
            // Step 1: Get a token from local (URI) Managed Service Identity endpoint which in turn fetches it from Azure Active Directory
            var token = GetToken();

            // Step 2: Fetch the secret value from Key Vault
            System.Console.WriteLine(FetchSecretValueFromKeyVault(token));
        }

        static string GetToken()
        {
            WebRequest request = WebRequest.Create("http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fvault.azure.net");
            request.Headers.Add("Metadata", "true");
            WebResponse response = request.GetResponse();
            return ParseWebResponse(response, "access_token");
        }

        static string FetchSecretValueFromKeyVault(string token)
        {
            WebRequest kvRequest = WebRequest.Create("https://prashanthwinvmvault.vault.azure.net/secrets/RandomSecret?api-version=2016-10-01");
            kvRequest.Headers.Add("Authorization", "Bearer "+  token);
            WebResponse kvResponse = kvRequest.GetResponse();
            return ParseWebResponse(kvResponse, "value");
        }

        private static string ParseWebResponse(WebResponse response, string tokenName)
        {
            string token = String.Empty;
            using (Stream stream = response.GetResponseStream())
            {
                StreamReader reader = new StreamReader(stream, Encoding.UTF8);
                String responseString = reader.ReadToEnd();

                JObject joResponse = JObject.Parse(responseString);    
                JValue ojObject = (JValue)joResponse[tokenName];             
                token = ojObject.Value.ToString();
            }
            return token;
        }
    }
```


El código anterior muestra cómo realizar operaciones con Azure Key Vault en una máquina virtual Linux de Azure. 



## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [API REST de Azure Key Vault](https://docs.microsoft.com/rest/api/keyvault/)
