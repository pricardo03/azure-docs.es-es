---
title: 'Guía de inicio rápido: Establecimiento y recuperación de un secreto de Azure Key Vault mediante una aplicación web de Node | Microsoft Docs'
description: 'Guía de inicio rápido: Establecimiento y recuperación de un secreto de Azure Key Vault mediante una aplicación web de Node'
services: key-vault
author: prashanthyv
manager: sumedhb
ms.service: key-vault
ms.topic: quickstart
ms.date: 07/24/2018
ms.author: barclayn
ms.custom: mvc
ms.openlocfilehash: 0188d06e5c58287e1040f6a15456d3ffe291b04a
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "42023442"
---
# <a name="quickstart-set-and-retrieve-a-secret-from-azure-key-vault-using-a-net-web-app"></a>Inicio rápido: Establecimiento y recuperación de un secreto de Azure Key Vault mediante una aplicación web de .NET

En esta guía de inicio rápido, encontrará los pasos necesarios para conseguir que una aplicación web de Azure lea información del almacén de claves mediante identidades de servicio administradas. Aprenderá a:

> [!div class="checklist"]
> * Crear un almacén de claves.
> * Almacenar un secreto en el almacén de claves.
> * Recuperar un secreto del almacén de claves.
> * Crear una aplicación web de Azure.
> * [Habilitar identidades de servicio administradas](../active-directory/managed-service-identity/overview.md).
> * Conceder los permisos necesarios para que la aplicación web lea datos del almacén de claves.

Antes de que sigamos avanzando, conozca los [conceptos básicos](key-vault-whatis.md#basic-concepts).

>[!NOTE]
Para entender por qué el siguiente tutorial es un procedimiento recomendado, es necesario comprender varios conceptos. Key Vault es un repositorio central para almacenar secretos mediante programación. Pero para poder hacer esto, las aplicaciones y los usuarios tienen primero que autenticarse en Key Vault, es decir, presentar un secreto. Para seguir los procedimientos recomendados de seguridad debe cambiar este secreto periódicamente. Pero con [Managed Service Identity](../active-directory/managed-service-identity/overview.md) a las aplicaciones que se ejecutan en Azure se les da una identidad que Azure administra automáticamente. Esto ayuda a solucionar el **problema de introducción de secretos** por el que los usuarios o aplicaciones pueden seguir procedimientos recomendados y no tener que preocuparse por el cambio del primer secreto

## <a name="prerequisites"></a>Requisitos previos

* En Windows:
  * [Visual Studio 2017, versión 15.7.3 o posterior,](https://www.microsoft.com/net/download/windows) con las siguientes cargas de trabajo:
    * ASP.NET y desarrollo web
    * Desarrollo multiplataforma de .NET Core
  * [SDK de .NET Core 2.1 o posterior](https://www.microsoft.com/net/download/windows)

* En Mac:
  * https://visualstudio.microsoft.com/vs/mac/

* Todas las plataformas:
  * Descargue GIT desde [aquí](https://git-scm.com/downloads).
  * Una suscripción de Azure. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.
  * [CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) Necesita la versión 2.0.4, o posterior, de la CLI de Azure. Está disponible para Windows, Mac y Linux

## <a name="login-to-azure"></a>Inicio de sesión en Azure

   Para iniciar sesión en Azure mediante la CLI puede escribir:

```azurecli
az login
```

## <a name="create-a-resource-group"></a>Crear un grupo de recursos

Cree un grupo de recursos con el comando [az group create](/cli/azure/group#az-group-create). Un grupo de recursos de Azure es un contenedor lógico en el que se implementan y se administran los recursos de Azure.

Seleccione el nombre del grupo de recursos y rellene el marcador de posición.
En el ejemplo siguiente se crea un grupo de recursos llamado *<YourResourceGroupName>* en la ubicación *eastus*.

```azurecli
# To list locations: az account list-locations --output table
az group create --name "<YourResourceGroupName>" --location "East US"
```

El grupo de recursos que acaba de crear se utiliza en todo este artículo.

## <a name="create-an-azure-key-vault"></a>Crear una instancia de Azure Key Vault

A continuación, creará una instancia de Key Vault en el grupo de recursos creado en el paso anterior. Proporcione la siguiente información:

* Nombre del almacén: **seleccione aquí el nombre de la instancia de Key Vault**. El nombre de un almacén de claves debe ser una cadena con una longitud de 3 a 24 caracteres que solo contenga (0-9, a-z, A-Z y -).
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

Este comando muestra la información secreta, URI incluido. Después de completar estos pasos debe tener un identificador URI para un secreto en una instancia de Azure Key Vault. Tome nota de esta información. La necesitará en otro paso más adelante.

## <a name="clone-the-repo"></a>Clonación del repositorio

Clone el repositorio con el fin de realizar una copia local para editar el origen; para ello, debe ejecutar el comando siguiente:

```
git clone https://github.com/Azure-Samples/key-vault-dotnet-core-quickstart.git
```

## <a name="open-and-edit-the-solution"></a>Apertura y edición de la solución

Edite el archivo program.cs para ejecutar el ejemplo con su nombre del almacén de claves específico.

1. Vaya a la carpeta key-vault-dotnet-core-quickstart
2. Abra el archivo key-vault-dotnet-core-quickstart.sln en Visual Studio 2017
3. Abra el archivo Program.cs y actualice el marcador de posición <YourKeyVaultName> con el nombre de su instancia de Key Vault que creó anteriormente.

Esta solución usa las bibliotecas de NuGet [AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) y [KeyVault](https://www.nuget.org/packages/Microsoft.Azure.KeyVault)

## <a name="run-the-app"></a>Ejecución de la aplicación

En el menú principal de Visual Studio 2017, elija Depurar > Iniciar sin depurar. Cuando aparezca el explorador, vaya a la página Acerca de. Aparecerá el valor de AppSecret.

## <a name="publish-the-web-application-to-azure"></a>Publicación de la aplicación web en Azure

Vamos a publicar esta aplicación en Azure tanto para verla en vivo como una aplicación web como para ver también que recuperamos el valor del secreto

1. En Visual Studio, seleccione el proyecto **key-vault-dotnet-core-quickstart**.
2. Seleccione **Publicar** y, a continuación, **Iniciar**.
3. Cree una nueva instancia de **App Service** y seleccione **Publicar**.
4. Cambie la nombre de la aplicación a "keyvaultdotnetcorequickstart"
5. Seleccione **Crear**.

>[!VIDEO https://sec.ch9.ms/ch9/e93d/a6ac417f-2e63-4125-a37a-8f34bf0fe93d/KeyVault_high.mp4]

## <a name="enable-managed-service-identities-msi"></a>Habilitación de identidades de servicio administradas (MSI)

Azure Key Vault proporciona una forma de almacenar de forma segura credenciales y otras claves y secretos, pero el código tiene que autenticarse en Azure Key Vault para recuperarlos. Managed Service Identity (MSI) facilita esta labor, ya que proporciona a los servicios de Azure una identidad administrada automáticamente en Azure Active Directory (Azure AD). Puede usar esta identidad para autenticar cualquier servicio que admita la autenticación de Azure AD, incluido Key Vault, sin necesidad de tener credenciales en el código.

1. Vuelva a la CLI de Azure.
2. Ejecute el comando assign-identity para crear la identidad de esta aplicación:

```azurecli
az webapp identity assign --name "keyvaultdotnetcorequickstart" --resource-group "<YourResourceGroupName>"
```

>[!NOTE]
>Este comando es el equivalente de ir al portal y cambiar la **Identidad de servicio administrada** a **Activada** en las propiedades de la aplicación web.

## <a name="assign-permissions-to-your-application-to-read-secrets-from-key-vault"></a>Asignación de permisos a una aplicación para leer secretos de Key Vault

Tome nota de la salida cuando [publique la aplicación en Azure][]. Debe tener el formato:
        
        {
          "principalId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
          "tenantId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
          "type": "SystemAssigned"
        }
        
Luego, ejecute este comando con el nombre de la instancia de Key Vault y el valor de PrincipalId que ha copiado de arriba:

```azurecli

az keyvault set-policy --name '<YourKeyVaultName>' --object-id <PrincipalId> --secret-permissions get

```

**Ahora, cuando ejecute la aplicación debería ver el valor del secreto recuperado**

## <a name="next-steps"></a>Pasos siguientes

* [Página principal de Azure Key Vault](https://azure.microsoft.com/services/key-vault/)
* [Documentación de Azure Key Vault](https://docs.microsoft.com/azure/key-vault/)
* [SDK de Azure para .NET](https://github.com/Azure/azure-sdk-for-net)
* [Azure REST API Reference](https://docs.microsoft.com/rest/api/keyvault/) (Referencia de API de REST en Azure)
