---
title: 'Inicio rápido: Establecimiento y recuperación de un secreto de Azure Key Vault mediante una aplicación web de Node | Microsoft Docs'
description: En este inicio rápido, establecerá y recuperará un secreto de Azure Key Vault mediante una aplicación web de Node.
services: key-vault
author: msmbaldwin
manager: sumedhb
ms.service: key-vault
ms.topic: quickstart
ms.date: 09/05/2018
ms.author: barclayn
ms.custom: mvc
ms.openlocfilehash: 5e8c29e033d895e24047754e686420fb4db86142
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/07/2019
ms.locfileid: "65236643"
---
# <a name="quickstart-set-and-retrieve-a-secret-from-azure-key-vault-by-using-a-node-web-app"></a>Inicio rápido: Establecimiento y recuperación de un secreto de Azure Key Vault mediante una aplicación web de Node 

En este inicio rápido se muestra cómo almacenar un secreto en Azure Key Vault y cómo recuperarlo mediante una aplicación web. El uso de Key Vault le ayuda a proteger la información. Para ver el valor del secreto, tendría que ejecutar este inicio rápido Azure. La guía de inicio rápido usa Node.js e identidades administradas para recursos de Azure. Aprenderá a:

* Cree un almacén de claves.
* Almacenar un secreto en el almacén de claves.
* Recuperar un secreto del almacén de claves.
* Crear una aplicación web de Azure.
* Habilite una [identidad administrada](https://docs.microsoft.com/azure/active-directory/managed-service-identity/overview) para la aplicación web.
* Conceder los permisos necesarios para que la aplicación web lea datos del almacén de claves.

Antes de continuar, asegúrese de que está familiarizado con los [conceptos básicos sobre Key Vault](key-vault-whatis.md#basic-concepts).

> [!NOTE]
> Key Vault es un repositorio central para almacenar secretos mediante programación. Pero para poder hacer esto, las aplicaciones y los usuarios tienen primero que autenticarse en Key Vault, es decir, presentar un secreto. Para seguir los procedimientos recomendados de seguridad, debe cambiar este secreto periódicamente. 
>
> Con [las identidades de servicio administradas para recursos de Azure](../active-directory/managed-identities-azure-resources/overview.md), las aplicaciones que se ejecutan en Azure obtienen una identidad que Azure administra automáticamente. Esto ayuda a solucionar el *problema de introducción de secretos* por el que los usuarios o aplicaciones pueden seguir procedimientos recomendados y no tener que preocuparse por el cambio del primer secreto.

## <a name="prerequisites"></a>Requisitos previos

* [Node.js](https://nodejs.org/en/)
* [Git](https://www.git-scm.com/)
* [CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) 2.0.4 o posterior. Para realizar este inicio rápido, es necesario ejecutar localmente la CLI de Azure. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure 2.0](https://review.docs.microsoft.com/en-us/cli/azure/install-azure-cli?branch=master&view=azure-cli-latest).
* Una suscripción de Azure. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="log-in-to-azure"></a>Inicio de sesión en Azure

Para iniciar sesión en Azure mediante la CLI de Azure, escriba:

```azurecli
az login
```

## <a name="create-a-resource-group"></a>Crear un grupo de recursos

Cree un grupo de recursos con el comando [az group create](/cli/azure/group#az-group-create). Un grupo de recursos de Azure es un contenedor lógico en el que se implementan y se administran los recursos de Azure.

Seleccione el nombre de un grupo de recursos y rellene el marcador de posición.
En el siguiente ejemplo, se crea un grupo de recursos en la ubicación Este de EE. UU.

```azurecli
# To list locations: az account list-locations --output table
az group create --name "<YourResourceGroupName>" --location "East US"
```

El grupo de recursos que acaba de crear se utiliza en todo este artículo.

## <a name="create-a-key-vault"></a>Creación de un Almacén de claves

A continuación, creará un almacén de claves mediante el grupo de recursos creado en el paso anterior. Aunque en este artículo se usa como nombre "ContosoKeyVault", debe usar un nombre único. Proporcione la siguiente información:

* Nombre del almacén de claves.
* Nombre del grupo de recursos. el nombre debe ser una cadena de entre 3 y 24 caracteres, y solo debe contener estos caracteres: 0-9, a-z, A-Z y un guion (-).
* Ubicación: **Este de EE. UU**.

```azurecli
az keyvault create --name "<YourKeyVaultName>" --resource-group "<YourResourceGroupName>" --location "East US"
```

En este momento, su cuenta de Azure es la única autorizada para realizar operaciones en este nuevo almacén.

## <a name="add-a-secret-to-the-key-vault"></a>Incorporación de un secreto al almacén de claves

Estamos agregando un secreto para ayudar a ilustrar cómo funciona. Se puede almacenar una cadena de conexión de SQL o cualquier otra información que necesite guardar de forma segura, pero que esté disponibles para la aplicación. En este tutorial, la contraseña se llamará **AppSecret** y almacenará el valor de **MySecret**.

Escriba los siguientes comandos para crear un secreto en el almacén de claves denominado **AppSecret**. Este secreto almacenará el valor **MySecret**.

```azurecli
az keyvault secret set --vault-name "<YourKeyVaultName>" --name "AppSecret" --value "MySecret"
```

Para ver el valor contenido en el secreto como texto sin formato:

```azurecli
az keyvault secret show --name "AppSecret" --vault-name "<YourKeyVaultName>"
```

Este comando muestra la información secreta, URI incluido. Después de completar estos pasos debe tener un identificador URI para un secreto en un almacén de claves. Tome nota de esta información. La necesitará en otro paso más adelante.

## <a name="clone-the-repo"></a>Clonación del repositorio

Clone el repositorio para hacer una copia local en la que pueda editar el origen. Ejecute el siguiente comando:

```
git clone https://github.com/Azure-Samples/key-vault-node-quickstart.git
```

## <a name="install-dependencies"></a>Instalación de dependencias

Ejecute los siguientes comandos para instalar dependencias:

```
cd key-vault-node-quickstart
npm install
```

En este proyecto se usan dos módulos de Node: [ms-rest-azure](https://www.npmjs.com/package/ms-rest-azure) y [azure-keyvault](https://www.npmjs.com/package/azure-keyvault).

## <a name="publish-the-web-app-to-azure"></a>Publicación de la aplicación web en Azure

Cree un plan de [Azure App Service](https://azure.microsoft.com/services/app-service/). Puede almacenar varias aplicaciones web en este plan.

    ```
    az appservice plan create --name myAppServicePlan --resource-group myResourceGroup
    ```
A continuación, cree una aplicación web. En el siguiente ejemplo, reemplace `<app_name>` por un nombre de aplicación único global (caracteres válidos son a-z, 0-9 y -). El runtime se establece en NODE|6.9. Para ver todos los entornos en tiempo de ejecución admitidos, ejecute `az webapp list-runtimes`.

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
Vaya a la aplicación web recién creada y verá que ya está funcionando. Reemplace `<app_name>` por un nombre de aplicación único.

    ```
    http://<app name>.azurewebsites.net
    ```
El comando anterior crea también una aplicación habilitada para Git que le permite implementarla en Azure desde el repositorio de Git local. El repositorio GIT local se configura con esta dirección URL: `https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git`.

Después de finalizar el comando anterior, puede agregar una instancia remota de Azure al repositorio de Git local. Reemplace `<url>` por la dirección URL del repositorio de Git.

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

Anote la salida del comando anterior. Debe tener este formato:
        
        {
          "principalId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
          "tenantId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
          "type": "SystemAssigned"
        }
        
A continuación, ejecute el siguiente comando con el nombre del almacén de claves y el valor de **principalId**:

```azurecli
az keyvault set-policy --name '<YourKeyVaultName>' --object-id <PrincipalId> --secret-permissions get set
```

## <a name="deploy-the-node-app-to-azure-and-retrieve-the-secret-value"></a>Implementación de la aplicación de Node en Azure y recuperación del valor del secreto

Ejecute el siguiente comando para implementar la aplicación en Azure:

```
git push azure master
```

Una vez hecho esto, cuando navegue a `https://<app_name>.azurewebsites.net`, podrá ver el valor secreto. Asegúrese de reemplazar el nombre `<YourKeyVaultName>` por el nombre de su almacén

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [SDK de Azure para Node](https://docs.microsoft.com/javascript/api/overview/azure/key-vault)
