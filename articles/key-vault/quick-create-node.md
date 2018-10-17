---
title: 'Guía de inicio rápido: Establecimiento y recuperación de un secreto de Azure Key Vault mediante una aplicación web de Node | Microsoft Docs'
description: 'Guía de inicio rápido: Establecimiento y recuperación de un secreto de Azure Key Vault mediante una aplicación web de Node'
services: key-vault
documentationcenter: ''
author: prashanthyv
manager: sumedhb
ms.service: key-vault
ms.workload: identity
ms.topic: quickstart
ms.date: 09/05/2018
ms.author: barclayn
ms.custom: mvc
ms.openlocfilehash: 860294ebc7fbadd3eeefc4298ec740ca7f704587
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2018
ms.locfileid: "44714401"
---
# <a name="quickstart-set-and-retrieve-a-secret-from-azure-key-vault-using-a-node-web-app"></a>Guía de inicio rápido: Establecimiento y recuperación de un secreto de Azure Key Vault mediante una aplicación web de Node 

En esta guía de inicio rápido se muestra cómo almacenar un secreto en Key Vault y cómo recuperarlo mediante una aplicación web. Para ver el valor del secreto tendría que ejecutar esto en Azure. La guía de inicio rápido usa Node.js e identidades administradas para recursos de Azure.

> [!div class="checklist"]
> * Crear un almacén de claves.
> * Almacenar un secreto en el almacén de claves.
> * Recuperar un secreto del almacén de claves.
> * Crear una aplicación web de Azure.
> * Habilite una [identidad administrada](https://docs.microsoft.com/azure/active-directory/managed-service-identity/overview) para la aplicación web.
> * Conceder los permisos necesarios para que la aplicación web lea datos del almacén de claves.

Antes de continuar, asegúrese de que está familiarizado con los [conceptos básicos](key-vault-whatis.md#basic-concepts).

>[!NOTE]
Para entender por qué el siguiente tutorial es un procedimiento recomendado, es necesario comprender varios conceptos. Key Vault es un repositorio central para almacenar secretos mediante programación. Pero para poder hacer esto, las aplicaciones y los usuarios tienen primero que autenticarse en Key Vault, es decir, presentar un secreto. Para seguir los procedimientos recomendados de seguridad debe cambiar este secreto periódicamente. Pero con [identidades administradas para recursos de Azure](../active-directory/managed-identities-azure-resources/overview.md), a las aplicaciones que se ejecutan en Azure se les asigna una identidad que Azure administra automáticamente. Esto ayuda a solucionar el **problema de introducción de secretos** por el que los usuarios o aplicaciones pueden seguir procedimientos recomendados y no tener que preocuparse por el cambio del primer secreto

## <a name="prerequisites"></a>Requisitos previos

* [Node.js](https://nodejs.org/en/)
* [Git](https://www.git-scm.com/)
* [CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) 2.0.4 o posterior
* Una suscripción de Azure. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="login-to-azure"></a>Inicio de sesión en Azure

Para iniciar sesión en Azure mediante la CLI puede escribir:

```azurecli
az login
```

## <a name="create-resource-group"></a>Creación de un grupo de recursos

Cree un grupo de recursos con el comando [az group create](/cli/azure/group#az-group-create). Un grupo de recursos de Azure es un contenedor lógico en el que se implementan y se administran los recursos de Azure.

Seleccione el nombre del grupo de recursos y rellene el marcador de posición.
En el ejemplo siguiente se crea un grupo de recursos llamado *<YourResourceGroupName>* en la ubicación *eastus*.

```azurecli
# To list locations: az account list-locations --output table
az group create --name "<YourResourceGroupName>" --location "East US"
```

El grupo de recursos que acaba de crear se utiliza a lo largo de este tutorial.

## <a name="create-an-azure-key-vault"></a>Crear una instancia de Azure Key Vault

A continuación, creará una instancia de Key Vault en el grupo de recursos creado en el paso anterior. Aunque "ContosoKeyVault" se utiliza como nombre de la instancia de Key Vault a lo largo de este artículo, tiene que usar un nombre único. Proporcione la siguiente información:

* Nombre del almacén: **seleccione aquí el nombre de la instancia de Key Vault**.
* Nombre del grupo de recursos: **seleccione aquí el nombre del grupo de recursos**.
* Ubicación: **Este de EE. UU**.

```azurecli
az keyvault create --name "<YourKeyVaultName>" --resource-group "<YourResourceGroupName>" --location "East US"
```

En este momento, su cuenta de Azure es la única autorizada para realizar operaciones en este nuevo almacén.

## <a name="add-a-secret-to-key-vault"></a>Incorporación de un secreto a Key Vault

Estamos agregando un secreto para ayudar a ilustrar cómo funciona. Se puede almacenar una cadena de conexión de SQL o cualquier otra información que necesite guardar de forma segura, pero que esté disponibles para la aplicación. En este tutorial, la contraseña se llamará **AppSecret** y almacenará el valor de **MySecret**.

Escriba los siguientes comandos para crear un secreto en Key Vault denominado **AppSecret** que almacene el valor **MySecret**:

```azurecli
az keyvault secret set --vault-name "<YourKeyVaultName>" --name "AppSecret" --value "MySecret"
```

Para ver el valor contenido en el secreto como texto sin formato:

```azurecli
az keyvault secret show --name "AppSecret" --vault-name "<YourKeyVaultName>"
```

Este comando muestra la información secreta, URI incluido. Después de completar estos pasos debe tener un identificador URI para un secreto en una instancia de Azure Key Vault. Anote esta información. La necesitará en otro paso más adelante.

## <a name="clone-the-repo"></a>Clonación del repositorio

Clone el repositorio con el fin de realizar una copia local para editar el origen; para ello, debe ejecutar el comando siguiente:

```
git clone https://github.com/Azure-Samples/key-vault-node-quickstart.git
```

## <a name="install-dependencies"></a>Instalación de dependencias

Aquí se instalan las dependencias. Ejecute los siguientes comandos cd key-vault-node-quickstart npm install

Este proyecto utiliza 2 módulos de Node:

* [ms-rest-azure](https://www.npmjs.com/package/ms-rest-azure) 
* [azure-keyvault](https://www.npmjs.com/package/azure-keyvault)

## <a name="publish-the-web-application-to-azure"></a>Publicación de la aplicación web en Azure

A continuación se muestran los pasos que deberá hacer

- El primer paso consiste en crear un plan de [Azure App Service](https://azure.microsoft.com/services/app-service/). Puede almacenar varias aplicaciones web en este plan.

    ```
    az appservice plan create --name myAppServicePlan --resource-group myResourceGroup
    ```
- A continuación, se crea una aplicación web. En el siguiente ejemplo, reemplace <app_name> por un nombre único global de aplicación (los caracteres válidos son a-z, 0-9 y -). El runtime se establece en NODE|6.9. Para ver todos los runtime admitidos, ejecute az webapp list-runtimes.
    ```
    # Bash
    az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app_name> --runtime "NODE|6.9" --deployment-local-git
    ```
    Cuando se haya creado la aplicación web, la CLI de Azure mostrará información similar a la del ejemplo siguiente:
    ```
    {
      "availabilityState": "Normal",
      "clientAffinityEnabled": true,
      "clientCertEnabled": false,
      "cloningInfo": null,
      "containerSize": 0,
      "dailyMemoryTimeQuota": 0,
      "defaultHostName": "<app_name>.azurewebsites.net",
      "enabled": true,
      "deploymentLocalGitUrl": "https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git"
      < JSON data removed for brevity. >
    }
    ```
    Vaya a la aplicación web recién creada y ya debería ver una aplicación web en funcionamiento. Reemplace <app_name> por un nombre de aplicación único.

    ```
    http://<app name>.azurewebsites.net
    ```
    El comando anterior crea también una aplicación habilitada para Git que le permite implementar en Azure desde el repositorio de Git local. 
    El repositorio de Git local está configurado con la dirección url "https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git"

- Cree un usuario de implementación. Una vez completado el comando anterior puede agregar una instancia remota de Azure en el repositorio de Git local. Reemplace <url> con la dirección URL del Git remoto que obtuvo en Habilitación de Git para la aplicación.

    ```
    git remote add azure <url>
    ```

## <a name="enable-a-managed-identity-for-the-web-app"></a>Habilitación de una identidad administrada para la aplicación web

Azure Key Vault proporciona una manera de almacenar de forma segura las credenciales y otras claves y secretos, pero el código tiene que autenticarse en Key Vault para recuperarlos. En [¿Qué es Managed Identities for Azure Resources?](../active-directory/managed-identities-azure-resources/overview.md) se facilita la resolución de este problema, al ofrecer a los servicios de Azure una identidad administrada automáticamente en Azure Active Directory (Azure AD). Puede usar esta identidad para autenticar cualquier servicio que admita la autenticación de Azure AD, incluido Key Vault, sin necesidad de tener credenciales en el código.

Ejecute el comando assign-identity para crear la identidad de esta aplicación:

```azurecli
az webapp identity assign --name <app_name> --resource-group "<YourResourceGroupName>"
```

Este comando equivale a ir al portal y cambiar la configuración de **Identidad o sistema asignados** a **Activada** en las propiedades de la aplicación web.

### <a name="assign-permissions-to-your-application-to-read-secrets-from-key-vault"></a>Asignación de permisos a una aplicación para leer secretos de Key Vault

Escriba o copie la salida del comando anterior. Debe tener este formato:
        
        {
          "principalId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
          "tenantId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
          "type": "SystemAssigned"
        }
        
Luego, ejecute este comando con el nombre de la instancia de Key Vault y el valor de PrincipalId que ha copiado de arriba:

```azurecli
az keyvault set-policy --name '<YourKeyVaultName>' --object-id <PrincipalId> --secret-permissions get
```

## <a name="deploy-the-node-app-to-azure-and-retrieve-the-secret-value"></a>Implementación de la aplicación de Node en Azure y recuperación del valor del secreto

Ahora que ya está todo establecido, ejecute el siguiente comando para implementar la aplicación en Azure.

```
git push azure master
```

Después de esto, si navega a https://<app_name>.azurewebsites.net podrá ver el valor del secreto.
Asegúrese de reemplazar el nombre <YourKeyVaultName> por el nombre de su almacén

## <a name="next-steps"></a>Pasos siguientes

* [Página principal de Azure Key Vault](https://azure.microsoft.com/services/key-vault/)
* [Documentación de Azure Key Vault](https://docs.microsoft.com/azure/key-vault/)
* [Azure SDK para Node](https://docs.microsoft.com/javascript/api/overview/azure/key-vault)
* [Azure REST API Reference](https://docs.microsoft.com/rest/api/keyvault/) (Referencia de API de REST en Azure)
