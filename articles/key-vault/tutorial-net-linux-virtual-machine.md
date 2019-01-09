---
title: 'Tutorial: Uso de Azure Key Vault con una máquina virtual Linux de Azure en .NET: Azure Key Vault | Microsoft Docs'
description: Tutorial de configuración de una aplicación ASP.NET Core para que lea un secreto de Key Vault
services: key-vault
documentationcenter: ''
author: prashanthyv
manager: rajvijan
ms.assetid: 0e57f5c7-6f5a-46b7-a18a-043da8ca0d83
ms.service: key-vault
ms.workload: key-vault
ms.topic: tutorial
ms.date: 12/21/2018
ms.author: pryerram
ms.custom: mvc
ms.openlocfilehash: 68a788205917e87469b432de435e296dcabc350c
ms.sourcegitcommit: da69285e86d23c471838b5242d4bdca512e73853
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/03/2019
ms.locfileid: "54001692"
---
# <a name="tutorial-how-to-use-azure-key-vault-with-azure-linux-virtual-machine-in-net"></a>Tutorial: Uso de Azure Key Vault con una máquina virtual Linux de Azure en .NET

Azure Key Vault ayuda a proteger secretos como las claves de API, las cadenas de conexión de base de datos necesarias para acceder a las aplicaciones, los servicios y los recursos de TI.

En este tutorial, seguirá los pasos necesarios para conseguir que una aplicación de consola lea información de Azure Key Vault mediante identidades administradas para recursos de Azure. A continuación aprenderá a:

> [!div class="checklist"]
> * Cree un almacén de claves.
> * Almacenar un secreto en el almacén de claves.
> * Recuperar un secreto del almacén de claves.
> * Crear una máquina virtual de Azure.
> * Habilitar una [identidad administrada](../active-directory/managed-identities-azure-resources/overview.md) para la máquina virtual.
> * Conceder los permisos necesarios para que la aplicación de consola lea datos del almacén de claves.
> * Recuperar secretos del almacén de claves.

Antes de avanzar, lea los [conceptos básicos](key-vault-whatis.md#basic-concepts).

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

A continuación, el código llama a un servicio de metadatos local disponible en el recurso de Azure para obtener un token de acceso.
El código usa el token de acceso que obtiene del MSI_ENDPOINT local para autenticar una instancia del servicio Azure Key Vault. 

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

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

* Nombre del almacén de claves: el nombre debe ser una cadena de entre 3 y 24 caracteres y solo debe contener alguno de estos caracteres: 0-9, a-z, A-Z y "-".
* Nombre del grupo de recursos.
* Ubicación: **Oeste de EE. UU.**.

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

Cree la máquina virtual con el comando [az vm create](/cli/azure/vm#az_vm_create).

En el ejemplo siguiente se crea una máquina virtual llamada *myVM* y se agrega una cuenta de usuario llamada *azureuser*. El parámetro `--generate-ssh-keys` se usa para generar automáticamente una clave SSH y colocarla en la ubicación de la clave predeterminada (*~/.ssh*). Para utilizar un conjunto de claves concreto, utilice la opción `--ssh-key-value`.

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVM \
  --image UbuntuLTS \
  --admin-username azureuser \
  --generate-ssh-keys
```

La creación de la máquina virtual y los recursos auxiliares tarda unos minutos en realizarse. En la salida de ejemplo siguiente se muestra que la operación de creación de la máquina virtual se realizó correctamente.

```
{
  "fqdns": "",
  "id": "/subscriptions/<guid>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "westus",
  "macAddress": "00-00-00-00-00-00",
  "powerState": "VM running",
  "privateIpAddress": "XX.XX.XX.XX",
  "publicIpAddress": "XX.XX.XXX.XXX",
  "resourceGroup": "myResourceGroup"
}
```

Tenga en cuenta sus propios valores de `publicIpAddress` en la salida de la máquina virtual. Esta dirección se usa para acceder a la máquina virtual en los siguientes pasos.

## <a name="assign-identity-to-virtual-machine"></a>Asignación de una identidad a la máquina virtual

En este paso vamos a crear una identidad asignada por el sistema para la máquina virtual mediante la ejecución del siguiente comando:

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

## <a name="sign-in-to-the-virtual-machine"></a>Inicio de sesión en la máquina virtual

Ahora inicie sesión en la máquina virtual mediante un terminal.

```
ssh azureuser@<PublicIpAddress>
```

## <a name="install-dot-net-core-on-linux"></a>Instalación del núcleo de Dot Net en Linux

### <a name="register-the-microsoft-product-key-as-trusted-run-the-following-two-commands"></a>Registre la clave del producto de Microsoft como de confianza. Ejecute los dos comandos siguientes

```
curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
sudo mv microsoft.gpg /etc/apt/trusted.gpg.d/microsoft.gpg
```

### <a name="set-up-desired-version-host-package-feed-based-on-operating-system"></a>Configure la fuente del paquete de host de la versión deseada en función del sistema operativo

```
# Ubuntu 17.10
sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-ubuntu-artful-prod artful main" > /etc/apt/sources.list.d/dotnetdev.list'
sudo apt-get update
 
# Ubuntu 17.04
sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-ubuntu-zesty-prod zesty main" > /etc/apt/sources.list.d/dotnetdev.list'
sudo apt-get update
 
# Ubuntu 16.04 / Linux Mint 18
sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-ubuntu-xenial-prod xenial main" > /etc/apt/sources.list.d/dotnetdev.list'
sudo apt-get update
 
# Ubuntu 14.04 / Linux Mint 17
sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-ubuntu-trusty-prod trusty main" > /etc/apt/sources.list.d/dotnetdev.list'
sudo apt-get update
```

### <a name="install-net-core"></a>Instalar .NET Core

Y compruebe la versión de .NET

```
sudo apt-get install dotnet-sdk-2.1.4
dotnet --version
```

## <a name="create-and-run-sample-dot-net-app"></a>Creación y ejecución de la aplicación de Dot Net de ejemplo

Mediante la ejecución de los siguientes comandos, debería ver que se imprime "Hola mundo" en la consola.

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

Luego, cambie el archivo de clase para que contenga el siguiente código. Se trata de un proceso de dos pasos:

1. capturar un token del punto de conexión de MSI local de la máquina virtual que, a su vez, captura un token de Azure Active Directory.
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
