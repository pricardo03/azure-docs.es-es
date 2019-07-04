---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: c20f86fe7fdcfc7ecc940923a8c98fa1fbf4cf65
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/18/2019
ms.locfileid: "67186091"
---
## <a name="create-a-resource-group"></a>Crear un grupo de recursos

Cree un grupo de recursos con el comando [az group create](/cli/azure/group). Un grupo de recursos de Azure es un contenedor lógico en el que se implementan y se administran recursos de Azure como instancias de Function App, bases de datos y cuentas de almacenamiento.

En el ejemplo siguiente se crea un grupo de recursos denominado `myResourceGroup`.  
Si no usa Cloud Shell, inicie sesión primero con `az login`.

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```
Generalmente se crean el grupo de recursos y los recursos en una región cercana. Para ver todas las ubicaciones compatibles con los planes de App Service, ejecute el comando [az appservice list-locations](/cli/azure/appservice#az-appservice-list-locations).
