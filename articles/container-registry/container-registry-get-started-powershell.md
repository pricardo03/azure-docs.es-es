---
title: 'Guía de inicio rápido: Creación de un registro privado de Docker en Azure: PowerShell'
description: Aprenda rápidamente a crear un registro de contenedor privado de Docker en Azure con PowerShell.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: quickstart
ms.date: 01/22/2019
ms.author: danlep
ms.custom: seodec18, mvc
ms.openlocfilehash: b8ff8e671d51a148177e66b30225dd7536a48028
ms.sourcegitcommit: a7331d0cc53805a7d3170c4368862cad0d4f3144
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2019
ms.locfileid: "55299750"
---
# <a name="quickstart-create-a-private-container-registry-using-azure-powershell"></a>Guía de inicio rápido: Creación de un registro de contenedor privado con Azure PowerShell

Azure Container Registry es un servicio privado y administrado de registro de contenedores de Docker, que se usa para compilar, almacenar y proporcionar imágenes de contenedor de Docker. Gracias a esta guía de inicio rápido, aprenderá a crear un registro de contenedor de Azure mediante PowerShell. A continuación, utilice los comandos de Docker para insertar una imagen de contenedor en el registro y, finalmente, extraiga y ejecute la imagen desde el registro.

## <a name="prerequisites"></a>Requisitos previos

Para realizar los pasos de esta guía, se requiere la versión 5.7.0 o posterior del módulo Azure PowerShell. Ejecute `Get-Module -ListAvailable AzureRM` para determinar la versión instalada. Si necesita instalarla o actualizarla, consulte el artículo sobre [cómo instalar el módulo de Azure PowerShell](/powershell/azure/azurerm/install-azurerm-ps).

También debe tener instalado Docker localmente. Docker ofrece paquetes para los sistemas [macOS][docker-mac], [Windows][docker-windows] y [Linux][docker-linux].

Dado que Azure Cloud Shell no incluye todos los componentes necesarios de Docker (como por ejemplo el demonio `dockerd`), no se puede usar Cloud Shell en este tutorial de inicio rápido.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en la suscripción de Azure con el comando [Connect-AzureRmAccount][Connect-AzureRmAccount] y siga las instrucciones de la pantalla.

```powershell
Connect-AzureRmAccount
```

## <a name="create-resource-group"></a>Creación de un grupo de recursos

Una vez se haya autenticado con Azure, debe crear un grupo de recursos con [New-AzureRmResourceGroup][New-AzureRmResourceGroup]. Un grupo de recursos es un contenedor lógico en el que se implementan y se administran los recursos de Azure.

```powershell
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

## <a name="create-container-registry"></a>Crear un registro de contenedor

A continuación, cree un registro de contenedor en el nuevo grupo de recursos mediante el comando [New-AzureRMContainerRegistry][New-AzureRMContainerRegistry].

El nombre del registro debe ser único dentro de Azure y contener entre 5 y 50 caracteres alfanuméricos. En el ejemplo siguiente se crea un registro con el nombre "myContainerRegistry007". Reemplace *myContainerRegistry007* en el siguiente comando y, a continuación, ejecútelo para crear el registro:

```powershell
$registry = New-AzureRMContainerRegistry -ResourceGroupName "myResourceGroup" -Name "myContainerRegistry007" -EnableAdminUser -Sku Basic
```

En este inicio rápido se crea un registro *Básico*, que es una opción rentable para los desarrolladores que aprenden sobre Azure Container Registry. Para más información sobre los niveles de servicio disponibles, consulte [SKU de Azure Container Registry][container-registry-skus].

## <a name="log-in-to-registry"></a>Iniciar sesión en el registro

Antes de insertar y extraer imágenes de contenedor, debe iniciar sesión en el registro. En escenarios de producción se debe usar una entidad individual o entidad de servicio para acceder al registro de contenedores, pero para abreviar en este inicio rápido, habilite al usuario administrador en el registro con el comando [Get-AzureRmContainerRegistryCredential][Get-AzureRmContainerRegistryCredential]:

```powershell
$creds = Get-AzureRmContainerRegistryCredential -Registry $registry
```

A continuación, ejecute el [inicio de sesión de Docker][docker-login] para iniciar sesión:

```powershell
$creds.Password | docker login $registry.LoginServer -u $creds.Username --password-stdin
```

El comando devolverá `Login Succeeded` una vez completado.

[!INCLUDE [container-registry-quickstart-docker-push](../../includes/container-registry-quickstart-docker-push.md)]

[!INCLUDE [container-registry-quickstart-docker-pull](../../includes/container-registry-quickstart-docker-pull.md)]

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando haya terminado de trabajar con los recursos que creó gracias a este tutorial, use el comando [Remove-AzureRmResourceGroup][Remove-AzureRmResourceGroup] para quitar el grupo de recursos, el registro de contenedor y las imágenes de contenedor almacenadas aquí:

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido, ha creado una instancia de Azure Container Registry con Azure PowerShell, ha insertado una imagen de contenedor y ha extraído y ejecutado la imagen del registro. Siga los tutoriales de Azure Container Registry para información más detallada de ACR.

> [!div class="nextstepaction"]
> [Tutoriales de Azure Container Registry][container-registry-tutorial-quick-task]

<!-- LINKS - external -->
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- Links - internal -->
[Connect-AzureRmAccount]: /powershell/module/azurerm.profile/connect-azurermaccount
[Get-AzureRmContainerRegistryCredential]: /powershell/module/azurerm.containerregistry/get-azurermcontainerregistrycredential
[Get-Module]: /powershell/module/microsoft.powershell.core/get-module
[New-AzureRMContainerRegistry]: /powershell/module/azurerm.containerregistry/New-AzureRMContainerRegistry
[New-AzureRmResourceGroup]: /powershell/module/azurerm.resources/new-azurermresourcegroup
[Remove-AzureRmResourceGroup]: /powershell/module/azurerm.resources/remove-azurermresourcegroup
[container-registry-tutorial-quick-task]: container-registry-tutorial-quick-task.md
[container-registry-skus]: container-registry-skus.md