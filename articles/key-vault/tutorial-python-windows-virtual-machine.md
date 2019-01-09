---
title: 'Tutorial: Uso de Azure Key Vault con una máquina virtual Windows de Azure en Python | Microsoft Docs'
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
ms.openlocfilehash: cced3d363f9eb7418d6f453eccb1bf1d7ac20ead
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/02/2019
ms.locfileid: "53972352"
---
# <a name="tutorial-how-to-use-azure-key-vault-with-azure-windows-virtual-machine-in-python"></a>Tutorial: Uso de Azure Key Vault con una máquina virtual Windows en Python

Azure Key Vault ayuda a proteger secretos como las claves de API, las cadenas de conexión de base de datos necesarias para acceder a las aplicaciones, los servicios y los recursos de TI.

En este tutorial, siga los pasos necesarios para conseguir que una aplicación web de Azure lea información de Azure Key Vault mediante identidades administradas para los recursos de Azure. A continuación aprenderá a:

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

A continuación, el código llama a un servicio de metadatos local disponible en el recurso de Azure para obtener un token de acceso.
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

## <a name="give-virtual-machine-identity-permission-to-key-vault"></a>Concesión del permiso de identidad de máquina virtual a un almacén de claves
Ahora se puede conceder el permiso de identidad creado al almacén de claves mediante la ejecución del siguiente comando

```
az keyvault set-policy --name '<YourKeyVaultName>' --object-id <VMSystemAssignedIdentity> --secret-permissions get list
```

## <a name="login-to-the-virtual-machine"></a>Inicio de sesión en la máquina virtual

Puede seguir este [tutorial](https://docs.microsoft.com/azure/virtual-machines/windows/connect-logon)

## <a name="create-and-run-sample-python-app"></a>Creación y ejecución de la aplicación de Python de ejemplo

El siguiente no es más que un archivo de ejemplo llamado "Sample.py". Usa la biblioteca [requests](http://docs.python-requests.org/en/master/) para realizar llamadas HTTP GET.

## <a name="edit-samplepy"></a>Edición de Sample.py
Después de crear Sample.py ábralo y copie el siguiente código

```
The below is a 2 step process. 
1. Fetch a token from the local MSI endpoint on the VM which in turn fetches a token from Azure Active Directory
2. Pass the token to Key Vault and fetch your secret 
```
    # importing the requests library 
    import requests 

    # Step 1: Fetch an access token from a Managed Identity enabled azure resource      
    # Note that the resource here is https://vault.azure.net for public cloud and api-version is 2018-02-01
    MSI_ENDPOINT = "http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fvault.azure.net"
    r = requests.get(MSI_ENDPOINT, headers = {"Metadata" : "true"}) 
      
    # extracting data in json format 
    # This request gets a access_token from Azure Active Directory using the local MSI endpoint
    data = r.json() 
    
    # Step 2: Pass the access_token received from previous HTTP GET call to Key Vault
    KeyVaultURL = "https://prashanthwinvmvault.vault.azure.net/secrets/RandomSecret?api-version=2016-10-01"
    kvSecret = requests.get(url = KeyVaultURL, headers = {"Authorization": "Bearer " + data["access_token"]})
    
    print(kvSecret.json()["value"])
```

By running you should see the secret value 
```
python Sample.py
```

The above code shows you how to do operations with Azure Key Vault in an Azure Windows Virtual Machine. 

## Next steps

> [!div class="nextstepaction"]
> [Azure Key Vault REST API](https://docs.microsoft.com/rest/api/keyvault/)
