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
ms.openlocfilehash: c70e2c166bee14ac58ee88bd18baf0cc61702767
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66137281"
---
[!INCLUDE [resource group intro text](resource-group.md)]

En Cloud Shell, cree un grupo de recursos con el comando [`az group create`](/cli/azure/group?view=azure-cli-latest#az-group-create). En el ejemplo siguiente, se crea un grupo de recursos denominado *myResourceGroup* en la ubicación *Europa Occidental*. Para ver todas las ubicaciones de App Service que se admiten en Linux en el nivel **Básico**, ejecute el comando [`az appservice list-locations --sku B1 --linux-workers-enabled`](/cli/azure/appservice?view=azure-cli-latest#az-appservice-list-locations).

```azurecli-interactive
az group create --name myResourceGroup --location "West Europe"
```

Generalmente se crean el grupo de recursos y los recursos en una región cercana. 

Cuando finaliza el comando, una salida de JSON muestra las propiedades del grupo de recursos.