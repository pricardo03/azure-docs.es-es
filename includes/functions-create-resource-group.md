---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: d44865dc3189a7f9dc05106baf9f4d120e5e8bf6
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2018
ms.locfileid: "50134104"
---
## <a name="create-a-resource-group"></a>Crear un grupo de recursos

Cree un grupo de recursos con el comando [az group create](/cli/azure/group#az_group_create). Un grupo de recursos de Azure es un contenedor lógico en el que se implementan y se administran recursos de Azure como instancias de Function App, bases de datos y cuentas de almacenamiento.

En el ejemplo siguiente se crea un grupo de recursos denominado `myResourceGroup`.  
Si no usa Cloud Shell, inicie sesión primero con `az login`.

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```
Generalmente se crean el grupo de recursos y los recursos en una región cercana. Para ver todas las ubicaciones compatibles con los planes de App Service, ejecute el comando [az appservice list-locations](/cli/azure/appservice#az-appservice-list-locations).