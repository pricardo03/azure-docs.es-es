---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 11/06/2019
ms.author: tamram
ms.openlocfilehash: e479f2376668a2fc3824e733996c94cfab04c9ec
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2019
ms.locfileid: "75467049"
---
## <a name="create-a-resource-group"></a>Crear un grupo de recursos

Cree un grupo de recursos de Azure con el comando [az group create](/cli/azure/group). Un grupo de recursos es un contenedor lógico en el que se implementan y se administran los recursos de Azure.

No olvide reemplazar los valores del marcador de posición entre corchetes angulares por sus propios valores:

```azurecli-interactive
az group create \
    --name <resource-group-name> \
    --location <location>
```

## <a name="create-a-storage-account"></a>Crear una cuenta de almacenamiento

Las cuentas de almacenamiento de uso general se crean con el comando [az storage account create](/cli/azure/storage/account). Las cuentas de almacenamiento de uso general se pueden para los cuatro servicios: blobs, archivos, tablas y colas.

No olvide reemplazar los valores del marcador de posición entre corchetes angulares por sus propios valores:

```azurecli-interactive
az storage account create \
    --name <account-name> \
    --resource-group <resource-group-name> \
    --location <location> \
    --sku Standard_ZRS \
    --encryption blob
```

## <a name="specify-storage-account-credentials"></a>Especificación de las credenciales de la cuenta de almacenamiento

La CLI de Azure necesita las credenciales de la cuenta de almacenamiento para la mayoría de los comandos en este tutorial. Aunque hay varias opciones para hacerlo, una de las formas más sencillas de especificarlas es establecer las variables de entorno `AZURE_STORAGE_ACCOUNT` y `AZURE_STORAGE_KEY`.

> [!NOTE]
> En este artículo se muestra cómo establecer variables de entorno mediante Bash. Otros entornos pueden requerir modificaciones de la sintaxis.

En primer lugar, muestre las claves de la cuenta de almacenamiento con el comando [az storage account keys list](/cli/azure/storage/account/keys). No olvide reemplazar los valores del marcador de posición entre corchetes angulares por sus propios valores:

```azurecli-interactive
az storage account keys list \
    --account-name <account-name> \
    --resource-group <resource-group-name> \
    --output table
```

Ahora, establezca las variables de entorno `AZURE_STORAGE_ACCOUNT` y `AZURE_STORAGE_KEY`. Puede hacerlo en el shell de Bash utilizando el comando `export`. No olvide reemplazar los valores del marcador de posición entre corchetes angulares por sus propios valores:

```bash
export AZURE_STORAGE_ACCOUNT="<account-name>"
export AZURE_STORAGE_KEY="<account-key>"
```

Para más información sobre cómo recuperar las claves de acceso de la cuenta mediante Azure Portal, consulte [Administración de las claves de acceso de la cuenta de almacenamiento](../articles/storage/common/storage-account-keys-manage.md).