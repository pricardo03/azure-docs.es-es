---
title: Administración de una identidad administrada asignada por el usuario mediante la CLI de Azure
description: Instrucciones paso a paso sobre cómo crear, enumerar y eliminar una identidad administrada asignada por el usuario mediante la CLI de Azure.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/16/2018
ms.author: daveba
ms.openlocfilehash: 19963edc3742cc5f09ed02249d313728410711c8
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46973510"
---
# <a name="create-list-or-delete-a-user-assigned-managed-identity-using-the-azure-cli"></a>Creación, enumeración o eliminación de una identidad administrada asignada por el usuario mediante la CLI de Azure

[!INCLUDE [preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Las identidades administradas de los recursos de Azure proporcionan a los servicios de Azure una identidad administrada en Azure Active Directory. Puede usar esta identidad para autenticar a cualquier servicio que admita la autenticación de Azure AD, sin necesidad de tener credenciales en el código. 

En este artículo, aprenderá a crear, enumerar y eliminar una identidad administrada asignada por el usuario mediante la CLI de Azure.

## <a name="prerequisites"></a>Requisitos previos

- Si no está familiarizado con las identidades administradas de los recursos de Azure, consulte la [sección de introducción](overview.md). **No olvide revisar la [diferencia entre una identidad administrada asignada por el sistema y una identidad administrada asignada por el usuario](overview.md#how-does-it-work)**.
- Si aún no tiene una cuenta de Azure, [regístrese para una cuenta gratuita](https://azure.microsoft.com/free/) antes de continuar.
- Para llevar a cabo las operaciones de administración de este artículo, su cuenta debe tener las siguientes asignaciones de roles:
    - Rol [Colaborador de identidad administrada](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) para crear, leer (enumerar), actualizar y eliminar una identidad administrada que haya asignado el usuario.
    - Rol [Operador de identidad administrada](/azure/role-based-access-control/built-in-roles#managed-identity-operator) para leer (enumerar) las propiedades de una identidad administrada que haya asignado el usuario.
- Para ejecutar los ejemplos de script de la CLI, tiene tres opciones:
    - Usar [Azure Cloud Shell](../../cloud-shell/overview.md) desde Azure Portal (consulte la sección siguiente).
    - Usar Azure Cloud Shell integrado a través del botón "Pruébelo", situado en la esquina superior derecha de cada bloque de código.
    - [Instale la versión más reciente de la CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.13 o posterior) si prefiere usar una consola de CLI local. Inicie sesión en Azure con `az login`, usando una cuenta asociada a la suscripción de Azure en la que desearía implementar la identidad administrada asignada por el usuario.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-a-user-assigned-managed-identity"></a>Creación de una identidad administrada asignada por el usuario 

Para crear una identidad administrada asignada por el usuario, use el comando [az identity create](/cli/azure/identity#az-identity-create). El parámetro `-g` especifica el grupo de recursos donde se creará la identidad administrada asignada por el usuario, mientras que el parámetro `-n` especifica su nombre. Reemplace los valores de los parámetros `<RESOURCE GROUP>` y `<USER ASSIGNED IDENTITY NAME>` con sus propios valores:

[!INCLUDE [ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

 ```azurecli-interactive
az identity create -g <RESOURCE GROUP> -n <USER ASSIGNED IDENTITY NAME>
```
## <a name="list-user-assigned-managed-identities"></a>Enumerar identidades administradas asignadas por el usuario

Para enumerar las identidades administradas asignadas por el usuario, use el comando [az identity list](/cli/azure/identity#az-identity-list). Reemplace `<RESOURCE GROUP>` por su propio valor:

```azurecli-interactive
az identity list -g <RESOURCE GROUP>
```
En la respuesta JSON, a las identidades administradas asignadas por el usuario se devuelve el valor `"Microsoft.ManagedIdentity/userAssignedIdentities"` para la clave `type`.

`"type": "Microsoft.ManagedIdentity/userAssignedIdentities"`

## <a name="delete-a-user-assigned-managed-identity"></a>Eliminación de una identidad administrada asignada por el usuario

Para eliminar una identidad administrada asignada por el usuario, use el comando [az identity delete](/cli/azure/identity#az-identity-delete).  El parámetro -n especifica el nombre, y el parámetro -g especifica el grupo de recursos donde se creó la identidad administrada asignada por el usuario. Reemplace los valores de los parámetros `<USER ASSIGNED IDENTITY NAME>` y `<RESOURCE GROUP>` con sus propios valores:

 ```azurecli-interactive
az identity delete -n <USER ASSIGNED IDENTITY NAME> -g <RESOURCE GROUP>
```
> [!NOTE]
> El proceso de eliminación de una identidad administrada asignada por el usuario no quitará la referencia de ningún recurso al que se haya asignado. Quite esas máquinas virtuales y VMSS con el comando `az vm/vmss identity remove`.

## <a name="next-steps"></a>Pasos siguientes

Para obtener una lista completa de comandos de identidad de la CLI de Azure, consulte [az identity](/cli/azure/identity).

Para obtener información sobre cómo asignar una identidad administrada asignada por el usuario a una VM de Azure, consulte [Configure managed identities for Azure resources on an Azure VM using Azure CLI](qs-configure-cli-windows-vm.md#user-assigned-managed-identity) (Configuración de identidades administradas de recursos de Azure en una VM de Azure con la CLI de Azure).


 
