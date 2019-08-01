---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: a2994602f857c2c8ff9f935b649a8d3e94c10dca
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/24/2019
ms.locfileid: "68444094"
---
## <a name="create-a-resource-group"></a>Crear un grupo de recursos

Cree un grupo de recursos con el comando [az group create](/cli/azure/group). Un grupo de recursos de Azure es un contenedor lógico en el que se implementan y se administran recursos de Azure como instancias de Function App, bases de datos y cuentas de almacenamiento.

En el ejemplo siguiente se crea un grupo de recursos denominado `myResourceGroup`.  
Si no usa Cloud Shell, inicie sesión primero con `az login`.

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```

Conviene crear el grupo de recursos y los recursos en una [región](https://azure.microsoft.com/global-infrastructure/regions/) cercana. 
