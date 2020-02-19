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
ms.openlocfilehash: 700dbfde3be2f24eb57acbdeb9d2841ef2bdfe44
ms.sourcegitcommit: 323c3f2e518caed5ca4dd31151e5dee95b8a1578
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2020
ms.locfileid: "77112271"
---
En la salida del comando, la sección `identity` muestra que en la tarea se establece una identidad de tipo `SystemAssigned`. `principalId` es el identificador de entidad de seguridad de la identidad de la tarea:

```console
[...]
  "identity": {
    "principalId": "xxxxxxxx-2703-42f9-97d0-xxxxxxxxxxxx",
    "tenantId": "xxxxxxxx-86f1-41af-91ab-xxxxxxxxxxxx",
    "type": "SystemAssigned",
    "userAssignedIdentities": null
  },
  "location": "eastus",
[...]
``` 
Use el comando [az acr task show][az-acr-task-show] para almacenar principalId en una variable, para su uso en comandos posteriores. Sustituya el nombre de la tarea y el registro en el siguiente comando:

```azurecli
principalID=$(az acr task show --name mytask --registry myregistry --query identity.principalId --output tsv)
```

<!-- LINKS - Internal -->
[az-acr-task-show]: /cli/azure/acr/task#az-acr-task-show
