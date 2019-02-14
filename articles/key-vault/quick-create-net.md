---
title: 'Inicio rápido: Establecimiento y recuperación de un secreto de Azure Key Vault mediante una aplicación web de Node: Azure Key Vault | Microsoft Docs'
description: En este inicio rápido, establecerá y recuperará un secreto de Azure Key Vault mediante una aplicación web de .NET
services: key-vault
author: prashanthyv
manager: sumedhb
ms.service: key-vault
ms.topic: quickstart
ms.date: 01/02/2019
ms.author: barclayn
ms.custom: mvc
ms.openlocfilehash: 8127a7a10c117edaefc4a215366990d847a165ec
ms.sourcegitcommit: de81b3fe220562a25c1aa74ff3aa9bdc214ddd65
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2019
ms.locfileid: "56238290"
---
# <a name="quickstart-set-and-retrieve-a-secret-from-azure-key-vault-by-using-a-net-web-app"></a>Inicio rápido: Establecimiento y recuperación de un secreto de Azure Key Vault mediante una aplicación web de .NET

En este inicio rápido, siga los pasos necesarios para conseguir que una aplicación web de Azure lea información de Azure Key Vault mediante identidades administradas para recursos de Azure. El uso de Key Vault le ayuda a proteger la información. Aprenderá a:

* Cree un almacén de claves.
* Almacenar un secreto en el almacén de claves.
* Recuperar un secreto del almacén de claves.
* Crear una aplicación web de Azure.
* Habilite una [identidad de servicio administrada](../active-directory/managed-identities-azure-resources/overview.md) para la aplicación web.
* Conceder los permisos necesarios para que la aplicación web lea datos del almacén de claves.

Antes de seguir, conozca los [conceptos básicos de Key Vault](key-vault-whatis.md#basic-concepts).

>[!NOTE]
>Key Vault es un repositorio central para almacenar secretos mediante programación. Pero para poder hacer esto, las aplicaciones y los usuarios tienen primero que autenticarse en Key Vault, es decir, presentar un secreto. Para seguir los procedimientos recomendados de seguridad, debe cambiar este secreto periódicamente. 
>
>Con [las identidades de servicio administradas para recursos de Azure](../active-directory/managed-identities-azure-resources/overview.md), las aplicaciones que se ejecutan en Azure obtienen una identidad que Azure administra automáticamente. Esto ayuda a solucionar el *problema de introducción de secretos* por el que los usuarios o aplicaciones pueden seguir procedimientos recomendados y no tener que preocuparse por el cambio del primer secreto.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prerequisites"></a>Requisitos previos

* En Windows:
  * [Visual Studio 2017, versión 15.7.3 o posterior,](https://www.microsoft.com/net/download/windows) con las siguientes cargas de trabajo:
    * ASP.NET y desarrollo web
    * Desarrollo multiplataforma de .NET Core
  * [SDK de .NET Core 2.1 o posterior](https://www.microsoft.com/net/download/windows)

* En Mac:
  * Consulte las [novedades de Visual Studio para Mac](https://visualstudio.microsoft.com/vs/mac/).

* Todas las plataformas:
  * Git ([descargar](https://git-scm.com/downloads)).
  * Una suscripción de Azure. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.
  * [CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) versión 2.0.4 o posterior. Está disponible para Windows, Mac y Linux.

## <a name="log-in-to-azure"></a>Inicio de sesión en Azure

Para iniciar sesión en Azure mediante la CLI de Azure, escriba:

```azurecli
az login
```

## <a name="create-a-resource-group"></a>Crear un grupo de recursos

Cree un grupo de recursos con el comando [az group create](/cli/azure/group#az-group-create). Un grupo de recursos de Azure es un contenedor lógico en el que se implementan y se administran los recursos de Azure.

Seleccione el nombre de un grupo de recursos y rellene el marcador de posición.
En el siguiente ejemplo, se crea un grupo de recursos en la ubicación Este de EE. UU:

```azurecli
# To list locations: az account list-locations --output table
az group create --name "<YourResourceGroupName>" --location "East US"
```

El grupo de recursos que acaba de crear se utiliza en todo este artículo.

## <a name="create-a-key-vault"></a>Creación de un Almacén de claves

A continuación, creará un almacén de claves en el grupo de recursos creado en el paso anterior. Proporcione la siguiente información:

* Nombre del almacén de claves: el nombre debe ser una cadena de entre 3 y 24 caracteres, y solo debe contener estos caracteres: 0-9, a-z, A-Z y un guion (-).
* Nombre del grupo de recursos.
* Ubicación: **Este de EE. UU**.

```azurecli
az keyvault create --name "<YourKeyVaultName>" --resource-group "<YourResourceGroupName>" --location "East US"
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

## <a name="clone-the-repo"></a>Clonación del repositorio

Clone el repositorio para hacer una copia local en la que pueda editar el origen. Ejecute el siguiente comando:

```
git clone https://github.com/Azure-Samples/key-vault-dotnet-core-quickstart.git
```

## <a name="open-and-edit-the-solution"></a>Apertura y edición de la solución

Edite el archivo program.cs para ejecutar el ejemplo con su nombre del almacén de claves específico:

1. Vaya a la carpeta key-vault-dotnet-core-quickstart.
2. Abra el archivo key-vault-dotnet-core-quickstart.sln en Visual Studio 2017.
3. Abra el archivo Program.cs y actualice el marcador de posición *YourKeyVaultName* con el nombre del almacén de claves que creó anteriormente.

Esta solución usa las bibliotecas de NuGet [AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) y [KeyVault](https://www.nuget.org/packages/Microsoft.Azure.KeyVault).

## <a name="run-the-app"></a>Ejecución de la aplicación

En el menú principal de Visual Studio 2017, elija **Depurar** > **Iniciar** sin depurar. Cuando aparezca el explorador, vaya a la página **Acerca de**. Aparecerá el valor de **AppSecret**.

## <a name="publish-the-web-application-to-azure"></a>Publicación de la aplicación web en Azure

Publique esta aplicación en Azure para verla en vivo como una aplicación web y para ver también que puede capturar el valor del secreto:

1. En Visual Studio, seleccione el proyecto **key-vault-dotnet-core-quickstart**.
2. Seleccione **Publicar** > **Iniciar**.
3. Cree una nueva instancia de **App Service** y seleccione **Publicar**.
4. Cambie el nombre de la aplicación a **keyvaultdotnetcorequickstart**.
5. Seleccione **Crear**.

>[!VIDEO https://sec.ch9.ms/ch9/e93d/a6ac417f-2e63-4125-a37a-8f34bf0fe93d/KeyVault_high.mp4]

## <a name="enable-a-managed-identity-for-the-web-app"></a>Habilitación de una identidad administrada para la aplicación web

Azure Key Vault proporciona una manera de almacenar de forma segura las credenciales y otras claves y secretos, pero el código tiene que autenticarse en Key Vault para recuperarlos. En [¿Qué es Managed Identities for Azure Resources?](../active-directory/managed-identities-azure-resources/overview.md) se facilita la resolución de este problema, al ofrecer a los servicios de Azure una identidad administrada automáticamente en Azure Active Directory (Azure AD). Puede usar esta identidad para autenticar cualquier servicio que admita la autenticación de Azure AD, incluido Key Vault, sin necesidad de tener credenciales en el código.

En la CLI de Azure, ejecute el comando assign-identity para crear la identidad de esta aplicación:

   ```azurecli
   az webapp identity assign --name "keyvaultdotnetcorequickstart" --resource-group "<YourResourceGroupName>"
   ```

>[!NOTE]
>El comando de este procedimiento equivale a ir al portal y cambiar la configuración de **Identidad o sistema asignados** a **Activada** en las propiedades de la aplicación web.

## <a name="assign-permissions-to-your-application-to-read-secrets-from-key-vault"></a>Asignación de permisos a una aplicación para leer secretos de Key Vault

Tome nota de la salida cuando publique la aplicación en Azure. Debe tener el formato:
        
        {
          "principalId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
          "tenantId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
          "type": "SystemAssigned"
        }
        
Luego, ejecute este comando con el nombre del almacén de claves y el valor de **PrincipalId**:

```azurecli

az keyvault set-policy --name '<YourKeyVaultName>' --object-id <PrincipalId> --secret-permissions get list

```

Ahora, cuando ejecute la aplicación debería ver el valor del secreto recuperado. En el comando anterior, otorga a la identidad de App Service los permisos para realizar operaciones **get** y **list** en Key Vault.

## <a name="clean-up-resources"></a>Limpieza de recursos
Elimine el grupo de recursos, la máquina virtual y todos los recursos relacionados Cuando ya no los necesite. Para ello, seleccione el grupo de recursos de la máquina virtual y seleccione **Eliminar**.

Elimine el almacén de claves mediante el comando [az keyvault delete](https://docs.microsoft.com/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-delete):

```azurecli
az keyvault delete --name
                   [--resource-group]
                   [--subscription]
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Más información acerca de Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-whatis)
