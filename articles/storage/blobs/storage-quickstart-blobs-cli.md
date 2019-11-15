---
title: 'Guía de inicio rápido de Azure: Creación de un blob en el almacenamiento de objetos con la CLI de Azure | Microsoft Docs'
description: En esta guía de inicio rápido, aprenderá a usar la CLI de Azure para cargar un blob en Azure Storage, descargar un blob o enumerar los blobs de un contenedor.
services: storage
author: tamram
ms.custom: mvc
ms.service: storage
ms.topic: quickstart
ms.date: 11/06/2019
ms.author: tamram
ms.openlocfilehash: 1f3143eced90f97c090c0005375ef50fe48c5f5f
ms.sourcegitcommit: 827248fa609243839aac3ff01ff40200c8c46966
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2019
ms.locfileid: "73747924"
---
# <a name="quickstart-upload-download-and-list-blobs-using-the-azure-cli"></a>Inicio rápido: Carga, descarga y enumeración de blobs mediante la CLI de Azure

La CLI de Azure es la forma de usar la línea de comandos de Azure para administrar los recursos de Azure. Puede utilizarlo en el explorador con Azure Cloud Shell. También puede instalarla en macOS, Linux o Windows y ejecutarla desde la línea de comandos. En esta guía de inicio rápido aprenderá a usar la CLI de Azure para cargar y descargar datos en Azure Blob Storage y desde este.

[!INCLUDE [storage-multi-protocol-access-preview](../../../includes/storage-multi-protocol-access-preview.md)]

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [storage-quickstart-prereq-include](../../../includes/storage-quickstart-prereq-include.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Si decide instalar y usar la CLI localmente, para esta guía de inicio rápido es preciso que ejecute la CLI de Azure versión 2.0.4 o posterior. Ejecute `az --version` para determinar la versión. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure](/cli/azure/install-azure-cli).

[!INCLUDE [storage-quickstart-tutorial-intro-include-cli](../../../includes/storage-quickstart-tutorial-intro-include-cli.md)]

## <a name="create-a-container"></a>Crear un contenedor

Los blobs siempre se cargan en un contenedor. Puede organizar los grupos de blobs de una forma similar a la que organiza los archivos en carpetas en el equipo.

Cree un contenedor para almacenar blobs con el comando [az storage container create](/cli/azure/storage/container).

```azurecli-interactive
az storage container create --name sample-container
```

## <a name="upload-a-blob"></a>Carga de un blob

Blob Storage admite blobs en bloques, blobs en anexos y blobs en páginas. Los ejemplos en esta guía de inicio rápido muestran cómo trabajar con blobs en bloques.

En primer lugar, cree un archivo para cargarlo en un blob en bloques. Si está utilizando Azure Cloud Shell, use el siguiente comando para crear un archivo:

```bash
vi helloworld
```

Cuando se abra el archivo, presione **Insertar**. Escriba *Hello World* y, a continuación, presione **Esc**. Después, escriba *:x* y, a continuación, presione **Entrar**.

En este ejemplo, se carga un blob en el contenedor que se creó en el último paso con el comando [az storage blob upload](/cli/azure/storage/blob). No es necesario especificar una ruta de acceso de archivo, ya que el archivo se creó en el directorio raíz:

```azurecli-interactive
az storage blob upload \
    --container-name sample-container \
    --name helloworld \
    --file helloworld
```

Esta operación crea el blob si todavía no existe y lo sobrescribe si ya existe. Cargue tantos archivos como desee antes de continuar.

Para cargar varios archivos al mismo tiempo, puede usar el comando [az storage blob upload-batch](/cli/azure/storage/blob).

## <a name="list-the-blobs-in-a-container"></a>Enumerar los blobs de un contenedor

Enumere los blobs del contenedor con el comando [az storage blob list](/cli/azure/storage/blob).

```azurecli-interactive
az storage blob list \
    --container-name sample-container \
    --output table
```

## <a name="download-a-blob"></a>Descarga de un blob

Use el comando [az storage blob download](/cli/azure/storage/blob) para descargar el blob que cargó anteriormente.

```azurecli-interactive
az storage blob download \
    --container-name sample-container \
    --name helloworld \
    --file ~/destination/path/for/file
```

## <a name="data-transfer-with-azcopy"></a>Transferencia de datos con AzCopy

La utilidad [AzCopy](../common/storage-use-azcopy-linux.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) es otra opción para la transferencia de datos que permite ejecutar scripts de alto rendimiento para Azure Storage. Puede utilizar AzCopy para transferir datos a y desde Blob Storage, File Storage y Table Storage.

En el ejemplo siguiente se usa AzCopy para cargar un archivo denominado *myfile.txt* al contenedor *sample-container*. No olvide reemplazar los valores del marcador de posición entre corchetes angulares por sus propios valores:

```bash
azcopy \
    --source /mnt/myfiles \
    --destination https://<account-name>.blob.core.windows.net/sample-container \
    --dest-key <account-key> \
    --include "myfile.txt"
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Si ya no necesita ninguno de los recursos del grupo de recursos, incluida la cuenta de almacenamiento que creó en esta guía de inicio rápido, elimine el grupo de recursos con el comando [az group delete](/cli/azure/group). No olvide reemplazar los valores del marcador de posición entre corchetes angulares por sus propios valores:

```azurecli-interactive
az group delete --name <resource-group-name>
```

## <a name="next-steps"></a>Pasos siguientes

En esta guía de inicio rápido, aprendió a transferir archivos entre un sistema de archivos local y un contenedor en Azure Blob Storage. Para más información sobre cómo trabajar con blobs en Azure Storage, continúe con el tutorial para trabajar con Azure Blob Storage.

> [!div class="nextstepaction"]
> [Uso de Operaciones de Blob Storage con la CLI de Azure](storage-how-to-use-blobs-cli.md)
