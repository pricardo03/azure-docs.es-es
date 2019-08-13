---
title: Uso de una identidad administrada con Azure Container Registry Tasks
description: Habilite una identidad administrada para los recursos de Azure en una tarea de Azure Container Registry para permitir que esta acceda a otros recursos de Azure, entre los que se incluyen otros registros de contenedor privado.
services: container-registry
author: dlepow
manager: gwallace
ms.service: container-registry
ms.topic: article
ms.date: 07/11/2019
ms.author: danlep
ms.openlocfilehash: 9f7c083a079e42172a9e2865f90293fa4d6813d8
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/30/2019
ms.locfileid: "68640416"
---
# <a name="use-an-azure-managed-identity-in-acr-tasks"></a>Uso de una identidad administrada de Azure en ACR Tasks 

Habilite una [identidad administrada para recursos de Azure](../active-directory/managed-identities-azure-resources/overview.md) en una [tarea de ACR](container-registry-tasks-overview.md), con el fin de que la tarea pueda acceder a otros recursos de Azure sin necesidad de proporcionar o administrar credenciales. Por ejemplo, use una identidad administrada para habilitar un paso de una tarea para extraer o insertar imágenes en otro registro.

En este artículo, aprenderá a usar el CLI de Azure para habilitar una identidad administrada asignada por el usuario o asignada por el sistema en una tarea de ACR. Puede usar Azure Cloud Shell o una instalación local de la CLI de Azure. Si desea usarlo de forma local, se requiere la versión 2.0.68 o cualquier versión posterior. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, vea [Instalación de la CLI de Azure][azure-cli-install].

Para ver escenarios en los que se accede a recursos protegidos desde una tarea de ACR mediante una identidad administrada, consulte:

* [Autenticación entre registros](container-registry-tasks-cross-registry-authentication.md)
* [Acceso a recursos externos con secretos almacenados en Azure Key Vault](container-registry-tasks-authentication-key-vault.md)

## <a name="why-use-a-managed-identity"></a>¿Por qué usar una identidad administrada?

Una identidad administrada para recursos de Azure proporciona a determinados servicios de Azure una identidad administrada automáticamente en Azure Active Directory (Azure AD). Puede configurar una tarea de ACR con una identidad administrada, con el fin de que la tarea pueda acceder a otros recursos de Azure protegidos, sin pasar credenciales en los pasos de la tarea.

Las identidades administradas son de dos tipos:

* Las *identidades asignadas por el usuario*, que puede asignar a varios recursos y conservar durante tanto tiempo como necesite. Las identidades asignadas por el usuario están actualmente en versión preliminar.

* Una *identidad administrada por el sistema*, que es única para un recurso especifico, como una tarea de ACR, y dura toda la vigencia del recurso.

Puede habilitar uno de los dos tipos de identidad, o ambos, en una tarea de ACR. Conceda a la identidad acceso a otro recurso, igual que cualquier entidad de seguridad. Cuando la tarea se ejecuta, usa la identidad para acceder al recurso en los pasos de la tarea que requieran acceso.

## <a name="steps-to-use-a-managed-identity"></a>Pasos para usar una identidad administrada

Siga estos pasos de alto nivel para usar una identidad administrada con una tarea de ACR.

### <a name="1-optional-create-a-user-assigned-identity"></a>1. (Opcional) Creación de una identidad asignada por el usuario

Si planea usar una identidad asignada por el usuario, puede usar una identidad existente. O bien, puede crear la identidad mediante la CLI de Azure u otras herramientas de Azure. Por ejemplo, use el comando [az identity create][az-identity-create]. 

Si planea usar solo una identidad asignada por el sistema, omita este paso. Al crear la tarea de ACR, puede crear también una identidad asignada por el sistema.

### <a name="2-enable-identity-on-an-acr-task"></a>2. Habilitación de la identidad en una tarea de ACR

Al crear una tarea de ACR, tiene la opción de habilitar una identidad asignada por el usuario o una identidad asignada por el sistema, o ambas. Por ejemplo, pase el parámetro `--assign-identity` al ejecutar el comando [az acr task create][az-acr-task-create] en la CLI de Azure.

Para habilitar una identidad asignada por el sistema, pase `--assign-identity` sin ningún o `assign-identity [system]`. El siguiente comando crea una tarea de Linux desde un repositorio de GitHub público que compila la imagen `hello-world` con un desencadenador de confirmación de Git y con una identidad administrada asignada por el sistema:

```azurecli
az acr task create \
    --image hello-world:{{.Run.ID}} \
    --name hello-world --registry MyRegistry \
    --context https://github.com/Azure-Samples/acr-build-helloworld-node.git \
    --file Dockerfile \
    --assign-identity
```

Para habilitar una identidad asignada por el usuario, pase `--assign-identity` con un valor del *Id. de recurso*  de la identidad. El siguiente comando crea una tarea de Linux desde un repositorio de GitHub público que compila la imagen `hello-world` con un desencadenador de confirmación de Git y con una identidad administrada asignada por el usuario:

```azurecli
az acr task create \
    --image hello-world:{{.Run.ID}} \
    --name hello-world --registry MyRegistry \
    --context https://github.com/Azure-Samples/acr-build-helloworld-node.git \
    --file Dockerfile \
    --assign-identity <resourceID>
```

Para obtener el Id. de recurso de la identidad, ejecute el comando [az Identity show][az-identity-show]. El Id. de recurso del identificador *myUserAssignedIdentity* del grupo de recursos *myResourceGroup* tiene el formato. 

```
"/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/myUserAssignedIdentity"
```

### <a name="3-grant-the-identity-permissions-to-access-other-azure-resources"></a>3. Concesión de los permisos de identidad para acceder a otros recursos de Azure

En función de los requisitos de la tarea, conceda los permisos de identidad necesarios para acceder a otros recursos de Azure. Algunos ejemplos son:

* Asigne a la identidad administrada un rol con permisos de extracción, de inserción y extracción o de otro tipo en un registro de contenedor destino de Azure. Para obtener una lista completa de los roles de registros, consulte [Roles y permisos de Azure Container Registry](container-registry-roles.md). 
* Asigne a la identidad administrada un rol para leer secretos en Azure Key Vault.

Use la [CLI de Azure](../role-based-access-control/role-assignments-cli.md) o cualquier otra herramientas de Azure para administrar el acceso basado en roles a los recursos. Por ejemplo, ejecute el comando [az role assignment create][az-role-assignment-create] para asignar un rol a la identidad. 

En el ejemplo siguiente se asignan a una identidad administrada los permisos necesarios para extraer de un registro de contenedor. El comando especifica el *id. de la entidad de servicio* de la identidad y el *identificador de recurso* del registro de destino.


```azurecli
az role assignment create --assignee <servicePrincipalID> --scope <registryID> --role acrpull
```

### <a name="4-optional-add-credentials-to-the-task"></a>4. (Opcional) Incorporación de credenciales a la tarea

Si una tarea extrae o inserta imágenes en otra instancia de Azure Container Registry, agregue credenciales a la tarea para que se autentique la identidad. Ejecute el comando[az acr task credential add][az-acr-task-credential-add] y pase el parámetro `--use-identity` para agregar las credenciales de la identidad a la tarea. 

Por ejemplo, para agregar credenciales para que una identidad asignada por el sistema se autentique en el registro *targetregistry* , pase `use-identity [system]`:

```azurecli
az acr task credential add \
    --name helloworld \
    --registry myregistry \
    --login-server targetregistry.azurecr.io \
    --use-identity [system]
```

Para agregar credenciales para que una identidad asignada por el usuario se autentique en el registro *targetregistry* pase `use-identity` con un valor del *Id. de cliente* de la identidad. Por ejemplo:

```azurecli
az acr task credential add \
    --name helloworld \
    --registry myregistry \
    --login-server targetregistry.azurecr.io \
    --use-identity <clientID>
```

Para obtener el Id. de cliente de la identidad, ejecute el comando [az identity show][az-identity-show]. El Id. de cliente es un GUID con el formato`xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx`.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aprendido a habilitar y usar una identidad administrada asignada por el usuario o asignada por el sistema en una tarea de ACR. Para ver escenarios en los que se accede a recursos protegidos desde una tarea de ACR mediante una identidad administrada, consulte:

* [Autenticación entre registros](container-registry-tasks-cross-registry-authentication.md)
* [Acceso a recursos externos con secretos almacenados en Azure Key Vault](container-registry-tasks-authentication-key-vault.md)


<!-- LINKS - Internal -->
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
[az-identity-create]: /cli/azure/identity#az-identity-create
[az-identity-show]: /cli/azure/identity#az-identity-show
[az-acr-task-create]: /cli/azure/acr/task#az-acr-task-create
[az-acr-task-credential-add]: /cli/azure/acr/task/credential#az-acr-task-credential-add
[azure-cli-install]: /cli/azure/install-azure-cli
