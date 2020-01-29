---
title: Opciones de autenticación de registro
description: Opciones de autenticación para una instancia de Azure Container Registry, incluido el inicio de sesión con una identidad de Azure Active Directory, mediante entidades de servicio y con credenciales de administrador opcionales.
ms.topic: article
ms.date: 12/21/2018
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: fbe77dee4104e3c654aad58db82765733b2c3e1d
ms.sourcegitcommit: 2a2af81e79a47510e7dea2efb9a8efb616da41f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/17/2020
ms.locfileid: "76264516"
---
# <a name="authenticate-with-a-private-docker-container-registry"></a>Autenticación con un registro de contenedor privado de Docker

Hay varias maneras de autenticar con un Azure Container Registry que se pueden aplicar a uno o más escenarios de uso de registros.

Entre los modos recomendados se incluyen la autenticación en un registro directamente mediante el [inicio de sesión individual](#individual-login-with-azure-ad) o los organizadores de contenedores y aplicaciones pueden realizar una autenticación desatendida, mediante la [entidad de servicio](#service-principal) de Azure Active Directory (Azure AD).

## <a name="individual-login-with-azure-ad"></a>Inicio de sesión individual con Azure AD

Cuando trabaje con el registro directamente (por ejemplo, para realizar la extracción e inserción de imágenes en la estación de trabajo de desarrollo), autentíquese mediante el comando [az acr login](/cli/azure/acr?view=azure-cli-latest#az-acr-login) en la [CLI de Azure](/cli/azure/install-azure-cli):

```azurecli
az acr login --name <acrName>
```

Cuando inicie sesión con `az acr login`, la CLI utiliza el token creado cuando ejecuta [az login](/cli/azure/reference-index#az-login) para autenticar sin problemas la sesión con su Registro. Una vez que haya iniciado sesión de este modo, las credenciales se almacenan en caché y los comandos `docker` posteriores de la sesión no requieren un nombre de usuario o una contraseña. 

Para acceder al registro, el token utilizado por `az acr login` es válido durante **3 horas**, por lo que se recomienda que inicie siempre la sesión en el registro antes de ejecutar un comando `docker`. Si el token expira, puede actualizarlo con el comando `az acr login` de nuevo para volver a autenticar. 

El uso de `az acr login` con identidades de Azure proporciona [acceso basado en roles](../role-based-access-control/role-assignments-portal.md). En algunos escenarios, puede que quiera iniciar sesión en un registro con su propia identidad individual de Azure AD. También puede iniciar sesión con una [instancia de Managed Identities for Azure Resources](container-registry-authentication-managed-identity.md) en los escenarios entre servicios o para satisfacer las necesidades de un grupo de trabajo donde no quiere administrar el acceso individual.

## <a name="service-principal"></a>Entidad de servicio

Si asigna una [entidad de servicio](../active-directory/develop/app-objects-and-service-principals.md) en el Registro, la aplicación o el servicio pueden usarla para la autenticación desatendida. Las entidades de servicio permiten el [acceso basado en roles](../role-based-access-control/role-assignments-portal.md) a un registro, y puede asignar varias entidades de seguridad de servicio a un registro. Las distintas entidades de servicio le permiten definir un acceso diferente para distintas aplicaciones.

Los roles disponibles para un Registro de contenedor incluyen:

* **AcrPull**: incorporación de cambios

* **AcrPush**: incorporación y envío de cambios

* **Propietario**: extracción, inserción y asignación de roles a otros usuarios

Para obtener una lista completa de roles, consulte [Roles y permisos de Azure Container Registry](container-registry-roles.md).

Para que los scripts de la CLI creen una entidad de servicio para autenticarse con Azure Container Registry y ver instrucciones sobre el uso de una entidad de servicio existente, consulte [Autenticación de Azure Container Registry con entidades de servicio](container-registry-auth-service-principal.md).

## <a name="admin-account"></a>Cuenta de administrador

Cada registro de contenedor incluye una cuenta de usuario administrador, que está deshabilitada de forma predeterminada. Puede habilitar el usuario administrador y administrar sus credenciales en Azure Portal, mediante la CLI de Azure o con otras herramientas de Azure.

> [!IMPORTANT]
> La cuenta de administrador está diseñada para que un solo usuario acceda al registro, principalmente con fines de prueba. No se recomienda compartir las credenciales de cuenta de administrador entre varios usuarios. Todos los usuarios que se autentican con la cuenta de administrador aparecen como un único usuario con acceso de inserción y extracción en el registro. Al cambiar o deshabilitar esta cuenta se deshabilita el acceso de registro para todos los usuarios que utilizan sus credenciales. Se recomienda la identidad individual para usuarios y entidades de servicio para escenarios desatendidos.
>

A la cuenta de administrador se le proporcionan dos contraseñas, y las dos se pueden regenerar. Las dos contraseñas le permiten mantener la conexión con el registro mediante una contraseña mientras se regenera la otra. Si la cuenta de administrador está habilitada, puede pasar el nombre de usuario y cualquier contraseña al comando `docker login` cuando se le solicite para la autenticación básica en el registro. Por ejemplo:

```
docker login myregistry.azurecr.io 
```

Para consultar procedimientos recomendados para administrar credenciales de inicio de sesión, consulte la referencia del comando [docker login](https://docs.docker.com/engine/reference/commandline/login/).

Para habilitar el usuario administrador para un registro existente, puede usar el parámetro `--admin-enabled` del comando [az acr update](/cli/azure/acr?view=azure-cli-latest#az-acr-update) en la CLI de Azure:

```azurecli
az acr update -n <acrName> --admin-enabled true
```

Puede habilitar el usuario administrador en Azure Portal dirigiéndose al registro, seleccionando **Claves de acceso** en **Configuración** y **Habilitar** en **Usuario administrador**.

![Activación de la interfaz de usuario administrador en Azure Portal][auth-portal-01]

## <a name="next-steps"></a>Pasos siguientes

* [Push your first image using the Azure CLI](container-registry-get-started-azure-cli.md) (Inserción de la primera imagen mediante la CLI de Azure)

<!-- IMAGES -->
[auth-portal-01]: ./media/container-registry-authentication/auth-portal-01.png
