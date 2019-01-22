---
title: 'Tutorial: Uso de Azure Key Vault con una máquina virtual de Azure en Python | Microsoft Docs'
description: En este tutorial, configurará una aplicación Python para leer un secreto de un almacén de claves
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
ms.openlocfilehash: 8c816d17807432d75b6102190fc37d25a525d7cf
ms.sourcegitcommit: f4b78e2c9962d3139a910a4d222d02cda1474440
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/12/2019
ms.locfileid: "54244178"
---
# <a name="tutorial-use-azure-key-vault-with-an-azure-virtual-machine-in-python"></a>Tutorial: Uso de Azure Key Vault con una máquina virtual de Azure en Python

Azure Key Vault ayuda a proteger secretos como las claves de API y las cadenas de conexión de base de datos necesarias para acceder a las aplicaciones, los servicios y los recursos de TI.

En este tutorial, seguirá los pasos necesarios para conseguir que una aplicación web de Azure lea información de Azure Key Vault mediante identidades administradas para recursos de Azure. Aprenderá a:

> [!div class="checklist"]
> * Cree un almacén de claves.
> * Almacenar un secreto en el almacén de claves.
> * Cree una máquina virtual de Azure.
> * Habilitar una [identidad administrada](../active-directory/managed-identities-azure-resources/overview.md) para la máquina virtual.
> * Conceder los permisos necesarios para que la aplicación de consola lea datos del almacén de claves.
> * Recuperar un secreto del almacén de claves.

Antes de continuar, lea los [conceptos básicos sobre Key Vault](key-vault-whatis.md#basic-concepts).

## <a name="prerequisites"></a>Requisitos previos
Para todas las plataformas, necesitará:

* Git ([descargar](https://git-scm.com/downloads)).
* Una suscripción de Azure. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.
* [CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) versión 2.0.4 o posterior. Está disponible para Windows, Mac y Linux.

### <a name="managed-service-identity-and-how-it-works"></a>Managed Service Identity y cómo funciona
En este tutorial se hace uso de Managed Service Identity (MSI).

Azure Key Vault puede almacenar credenciales de forma segura, de modo que no están en el código. Para recuperarlas, deberá autenticarse en Key Vault. Para autenticarse en Key Vault, necesita una credencial. Ese es un problema clásico de arranque. Mediante Azure y Azure Active Directory (Azure AD), MSI proporciona una "identidad de arranque" que permite comenzar fácilmente.

Cuando se habilita MSI para un servicio de Azure como Virtual Machines, App Service o Functions, Azure crea una [entidad de servicio](key-vault-whatis.md#basic-concepts) para la instancia del servicio en Azure AD. Azure inserta las credenciales de la entidad de servicio en la instancia del servicio. 

![MSI](media/MSI.png)

A continuación, el código llama a un servicio de metadatos local disponible en el recurso de Azure para obtener un token de acceso.
El código usa el token de acceso que obtiene del punto de conexión de MSI local para autenticarse en una instancia del servicio Azure Key Vault. 

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

* Nombre del almacén de claves: el nombre debe ser una cadena de entre 3 y 24 caracteres y solo debe contener estos caracteres: 0-9, a-z, A-Z y guiones (-).
* Nombre del grupo de recursos.
* Ubicación: **Oeste de EE. UU.**.

```azurecli
az keyvault create --name "<YourKeyVaultName>" --resource-group "<YourResourceGroupName>" --location "West US"
```
En este momento, su cuenta de Azure es la única autorizada para realizar operaciones en este nuevo almacén.

## <a name="add-a-secret-to-the-key-vault"></a>Incorporación de un secreto al almacén de claves

Estamos agregando un secreto para ayudar a ilustrar cómo funciona. Se puede almacenar una cadena de conexión de SQL o cualquier otra información que necesite guardar de forma segura, pero que esté disponibles para la aplicación.

Escriba los siguientes comandos para crear un secreto en el almacén de claves denominado *AppSecret*. Este secreto almacenará el valor **MySecret**.

```azurecli
az keyvault secret set --vault-name "<YourKeyVaultName>" --name "AppSecret" --value "MySecret"
```

## <a name="create-a-virtual-machine"></a>de una máquina virtual

Cree una máquina virtual con el comando [az vm create](/cli/azure/vm#az_vm_create).

En el ejemplo siguiente se crea una máquina virtual llamada *myVM* y se agrega una cuenta de usuario llamada *azureuser*. El parámetro `--generate-ssh-keys` genera automáticamente una clave SSH y la coloca en la ubicación de la clave predeterminada (*~/.ssh*). Para utilizar un conjunto de claves concreto, utilice la opción `--ssh-key-value`.

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVM \
  --image UbuntuLTS \
  --admin-username azureuser \
  --generate-ssh-keys
```

La creación de la máquina virtual y los recursos auxiliares tarda unos minutos en realizarse. En la salida de ejemplo siguiente se muestra que la creación de la máquina virtual se realizó correctamente:

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

Tenga en cuenta su propio valor de `publicIpAddress` en la salida de la máquina virtual. Esta dirección se usa para acceder a la máquina virtual en los siguientes pasos.

## <a name="assign-an-identity-to-the-virtual-machine"></a>Asignación de una identidad a la máquina virtual
En este paso, se va a crear una identidad asignada por el sistema para la máquina virtual. Ejecute el siguiente comando en la CLI de Azure:

```
az vm identity assign --name <NameOfYourVirtualMachine> --resource-group <YourResourceGroupName>
```

La salida del comando es la siguiente. Tenga en cuenta el valor de **systemAssignedIdentity**. 

```
{
  "systemAssignedIdentity": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "userAssignedIdentities": {}
}
```

## <a name="give-the-virtual-machine-identity-permission-to-the-key-vault"></a>Concesión del permiso de identidad de máquina virtual al almacén de claves
Ahora, se puede conceder el permiso de identidad al almacén de claves. Ejecute el siguiente comando:

```
az keyvault set-policy --name '<YourKeyVaultName>' --object-id <VMSystemAssignedIdentity> --secret-permissions get list
```

## <a name="log-in-to-the-virtual-machine"></a>Inicie sesión en la máquina virtual.

Inicie sesión en la máquina virtual mediante [este tutorial](https://docs.microsoft.com/azure/virtual-machines/windows/connect-logon).

## <a name="create-and-run-the-sample-python-app"></a>Creación y ejecución de la aplicación Python de ejemplo

El siguiente archivo de ejemplo se denomina *Sample.py*. Usa la biblioteca [requests](https://pypi.org/project/requests/2.7.0/) para realizar llamadas HTTP GET.

## <a name="edit-samplepy"></a>Edición de Sample.py
Después de crear Sample.py, abra el archivo y copie el código siguiente. El código es un proceso de dos pasos: 
1. Capturar un token del punto de conexión MSI local en la máquina virtual. El punto de conexión captura a continuación un token de Azure Active Directory.
2. Pase el token al almacén de claves y capture el secreto. 

```
    # importing the requests library 
    import requests 

    # Step 1: Fetch an access token from an MSI-enabled Azure resource      
    # Note that the resource here is https://vault.azure.net for the public cloud, and api-version is 2018-02-01
    MSI_ENDPOINT = "http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fvault.azure.net"
    r = requests.get(MSI_ENDPOINT, headers = {"Metadata" : "true"}) 
      
    # Extracting data in JSON format 
    # This request gets an access token from Azure Active Directory by using the local MSI endpoint
    data = r.json() 
    
    # Step 2: Pass the access token received from the previous HTTP GET call to the key vault
    KeyVaultURL = "https://prashanthwinvmvault.vault.azure.net/secrets/RandomSecret?api-version=2016-10-01"
    kvSecret = requests.get(url = KeyVaultURL, headers = {"Authorization": "Bearer " + data["access_token"]})
    
    print(kvSecret.json()["value"])
```

Al ejecutar el comando siguiente, verá el valor del secreto: 

```
python Sample.py
```

El código anterior muestra cómo realizar operaciones con Azure Key Vault en una máquina virtual Windows. 

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [API REST de Azure Key Vault](https://docs.microsoft.com/rest/api/keyvault/)
