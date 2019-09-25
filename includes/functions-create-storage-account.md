---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: 4dd43c5bcc5a0e9a734db4ca9a4b3d7137f85250
ms.sourcegitcommit: 8a717170b04df64bd1ddd521e899ac7749627350
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/23/2019
ms.locfileid: "71203130"
---
## <a name="create-an-azure-storage-account"></a>Creación de una cuenta de Azure Storage

Functions usa una cuenta de uso general en Azure Storage para mantener el estado y otra información acerca de sus funciones. Cree una cuenta de almacenamiento de uso general en el grupo de recursos que ha creado mediante el comando [az storage account create](/cli/azure/storage/account#az-storage-account-create).

En el siguiente comando, sustituya un nombre de cuenta de almacenamiento único de manera global donde vea el marcador de posición `<storage_name>`. Los nombres de las cuentas de almacenamiento deben tener entre 3 y 24 caracteres y solo pueden incluir números y letras en minúscula.

```azurecli-interactive
az storage account create --name <storage_name> --location westeurope --resource-group myResourceGroup --sku Standard_LRS
```
