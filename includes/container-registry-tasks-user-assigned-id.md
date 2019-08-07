---
title: archivo de inclusión
description: archivo de inclusión
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: include
ms.date: 07/12/2019
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 0e2acb346fad87e0c1c7fd7de1389d8fc86206d0
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/30/2019
ms.locfileid: "68641537"
---
### <a name="create-a-user-assigned-identity"></a>Creación de una identidad asignada por el usuario

Cree una identidad denominada *myACRTasksId* en la suscripción con el comando [az identity create][az-identity-create]. Puede usar el mismo grupo de recursos que usó anteriormente para crear un registro de contenedor o uno diferente.

```azurecli-interactive
az identity create --resource-group myResourceGroup --name myACRTasksId
```

Para configurar la identidad asignada por el usuario en los pasos siguientes, use el comando [az identity show][az-identity-show] para almacenar en variables los identificadores de recurso, entidad de servicio y cliente de la identidad.

```azurecli
# Get resource ID of the user-assigned identity
resourceID=$(az identity show --resource-group myResourceGroup --name myACRTasksId --query id --output tsv)

# Get service principal ID of the user-assigned identity
principalID=$(az identity show --resource-group myResourceGroup --name myACRTasksId --query principalId --output tsv)

# Get client ID of the user-assigned identity
clientID=$(az identity show --resource-group myResourceGroup --name myACRTasksId --query clientId --output tsv)
```

<!-- LINKS - Internal -->
[az-identity-create]: /cli/azure/identity#az-identity-create
[az-identity-show]: /cli/azure/identity#az-identity-show