---
title: archivo de inclusión
description: archivo de inclusión
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 02/02/2018
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 6fab0258b0c0e2f9b31358075a8f7a5be0228a5e
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/07/2018
ms.locfileid: "44095718"
---
[!INCLUDE [resource group intro text](resource-group.md)]

En Cloud Shell, cree un grupo de recursos con el comando [`az group create`](/cli/azure/group?view=azure-cli-latest#az_group_create). En el ejemplo siguiente, se crea un grupo de recursos denominado *myResourceGroup* en la ubicación *Europa Occidental*. Para ver todas las ubicaciones de App Service que se admiten en Linux en el nivel **Básico**, ejecute el comando [`az appservice list-locations --sku B1 --linux-workers-enabled`](/cli/azure/appservice?view=azure-cli-latest#az_appservice_list_locations).

```azurecli-interactive
az group create --name myResourceGroup --location "West Europe"
```

Generalmente se crean el grupo de recursos y los recursos en una región cercana. 

Cuando finaliza el comando, una salida de JSON muestra las propiedades del grupo de recursos.