---
title: Autenticación de Azure Container Registry con una entidad de servicio
description: Proporcione acceso a las imágenes de su registro de contenedor privado mediante una entidad de servicio de Azure Active Directory.
services: container-registry
author: dlepow
manager: gwallace
ms.service: container-registry
ms.topic: article
ms.date: 12/13/2018
ms.author: danlep
ms.openlocfilehash: 97c45a009b155eea7bc61a9dd337090b9e3c1b42
ms.sourcegitcommit: f5075cffb60128360a9e2e0a538a29652b409af9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2019
ms.locfileid: "68309959"
---
# <a name="azure-container-registry-authentication-with-service-principals"></a>Autenticación de Azure Container Registry con entidades de servicio

Puede usar una entidad de servicio de Azure Active Directory (Azure AD) para proporcionar a las imágenes de contenedor `docker push` y `pull` acceso a su registro de contenedor. Al usar una entidad de servicio, podrá proporcionar acceso a aplicaciones y servicios de "equipos sin periféricos".

## <a name="what-is-a-service-principal"></a>¿Qué es una entidad de servicio?

Las *entidades de servicio* de Azure AD proporcionan acceso a los recursos de Azure de su suscripción. Las entidades de servicio son como identidades de usuario para un servicio, donde "servicio" equivale a cualquier aplicación, servicio o plataforma que necesita acceder a los recursos. Puede configurar una entidad de servicio con derechos de acceso limitados solo a aquellos recursos que especifique. A continuación, configure su aplicación o servicio para usar las credenciales de la entidad de servicio y acceder a dichos recursos.

En el contexto de Azure Container Registry, puede crear una entidad de servicio de Azure AD con permisos de extracción, inserción y extracción u otros permisos para su registro de privado de Azure. Para obtener una lista completa, consulte [Roles y permisos de Azure Container Registry](container-registry-roles.md).

## <a name="why-use-a-service-principal"></a>Razones para usar una entidad de servicio

Al usar una entidad de servicio de Azure AD, podrá proporcionar acceso limitado a su registro de contenedor privado. Puede crear distintas entidades de servicio para cada uno de sus servicios o aplicaciones, cada una con derechos de acceso personalizados a su registro. Además, puesto que es posible evitar el uso compartido de credenciales entre servicios y aplicaciones, podrá alternar las credenciales o revocar el acceso solo a la entidad principal (y, por lo tanto, a la aplicación) que elija.

Por ejemplo, su aplicación web puede usar una entidad de servicio que solo proporcione acceso a la imagen `pull`, mientras el sistema de compilación puede usar una entidad de servicio que proporcione acceso a las imágenes `push` y `pull`. Si el desarrollo de su aplicación cambia de manos, puede alternar las credenciales de entidad de servicio sin que ello afecte al sistema de compilación.

## <a name="when-to-use-a-service-principal"></a>Cuándo usar una entidad de servicio

Debe usar una entidad de servicio para proporcionar acceso al registro en **escenarios de equipos sin periféricos**. Es decir, para cualquier aplicación, servicio o script que deba insertar o extraer imágenes de contenedor de forma automática o desatendida.

Para obtener acceso individual a un registro, como, por ejemplo, al extraer manualmente una imagen de contenedor en su estación de trabajo de desarrollo, debe usar en su lugar su propia [identidad de Azure AD](container-registry-authentication.md#individual-login-with-azure-ad) para acceder al registro (por ejemplo, con [az acr login][az-acr-login]).

[!INCLUDE [container-registry-service-principal](../../includes/container-registry-service-principal.md)]

## <a name="sample-scripts"></a>Scripts de ejemplo

Puede encontrar los scripts del ejemplo anterior para la CLI de Azure en GitHub, así como versiones para Azure PowerShell:

* [CLI de Azure][acr-scripts-cli]
* [Azure PowerShell][acr-scripts-psh]

## <a name="next-steps"></a>Pasos siguientes

Cuando disponga de una entidad de servicio con acceso a su registro de contenedor, podrá usar sus credenciales en aplicaciones y servicios para interactuar con el registro. Puede usar las credenciales de la entidad de servicio desde cualquier servicio de Azure que pueda autenticarse en un registro de contenedor de Azure. Algunos ejemplos son:

* [Autenticación con Azure Container Registry desde Azure Kubernetes Service (AKS)](container-registry-auth-aks.md)
* [Autenticación con Azure Container Registry desde Azure Container Instances (ACI)](container-registry-auth-aci.md)

<!-- LINKS - External -->
[acr-scripts-cli]: https://github.com/Azure/azure-docs-cli-python-samples/tree/master/container-registry
[acr-scripts-psh]: https://github.com/Azure/azure-docs-powershell-samples/tree/master/container-registry

<!-- LINKS - Internal -->
[az-acr-login]: /cli/azure/acr#az-acr-login
