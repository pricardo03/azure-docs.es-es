---
title: Servicio de contenido desde Azure Storage en App Service en Linux
description: Configuración y servicio de contenido de Azure Storage en Azure App Service en Linux
author: msangapu
manager: jeconnoc
ms.service: app-service
ms.workload: web
ms.topic: article
ms.date: 2/04/2019
ms.author: msangapu-msft
ms.openlocfilehash: 40aa032654d81c947e2c31e9d93954ee050f78a2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60849955"
---
# <a name="serve-content-from-azure-storage-in-app-service-on-linux"></a>Servicio de contenido desde Azure Storage en App Service en Linux

En esta guía se muestra cómo servir contenido estático en App Service en Linux mediante [Azure Storage](/azure/storage/common/storage-introduction). Entre las ventajas se incluye protección y portabilidad del contenido, acceso a varias aplicaciones y varios métodos de transferencia. En esta guía, aprenderá a servir el contenido en el almacenamiento de Azure mediante la configuración de almacenamiento personalizado.

## <a name="prerequisites"></a>Requisitos previos

- Una aplicación web existente (App Service en Linux o Web Apps for Containers).
- [CLI de Azure](/cli/azure/install-azure-cli) (2.0.46 o posterior).

## <a name="create-azure-storage"></a>Creación de una instancia de Azure Storage

> [!NOTE]
> Azure Storage es almacenamiento no predeterminado y se factura por separado, es decir, no se incluye con la aplicación web.
>

Cree una [cuenta de almacenamiento de Azure](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=azure-cli).

```azurecli
#Create Storage Account
az storage account create --name <storage_account_name> --resource-group myResourceGroup

#Create Storage Container
az storage container create --name <storage_container_name> --account-name <storage_account_name>
```

## <a name="upload-files-to-azure-storage"></a>Carga de archivos en Azure Storage

Para cargar un directorio local en la cuenta de almacenamiento, use el comando [ `az storage blob upload-batch`](https://docs.microsoft.com/cli/azure/storage/blob?view=azure-cli-latest#az-storage-blob-upload-batch), como en el ejemplo siguiente:

```azurecli
az storage blob upload-batch -d <full_path_to_local_directory> --account-name <storage_account_name> --account-key "<access_key>" -s <source_location_name>
```

## <a name="link-storage-to-your-web-app-preview"></a>Vinculación del almacenamiento a la aplicación web (versión preliminar)

> [!CAUTION]
> Al vincular un directorio existente en una aplicación web a una cuenta de almacenamiento, se eliminará el contenido del directorio. Si va a migrar archivos de una aplicación existente, realice una copia de seguridad de la aplicación y de su contenido antes de comenzar.
>

Para montar una cuenta de almacenamiento en un directorio en la aplicación de App Service, use el comando [`az webapp config storage-account add`](https://docs.microsoft.com/cli/azure/webapp/config/storage-account?view=azure-cli-latest#az-webapp-config-storage-account-add). El tipo de almacenamiento puede ser AzureBlob o AzureFiles. Usará AzureBlob con este contenedor.

```azurecli
az webapp config storage-account add --resource-group <group_name> --name <app_name> --custom-id <custom_id> --storage-type AzureBlob --share-name <share_name> --account-name <storage_account_name> --access-key "<access_key>" --mount-path <mount_path_directory>
```

Este paso debe realizarlo con cualquier otro directorio que quiera vincular a una cuenta de almacenamiento.

## <a name="verify"></a>Verify

Una vez que un contenedor de almacenamiento está vinculado a una aplicación web, puede ejecutar el siguiente comando para comprobarlo:

```azurecli
az webapp config storage-account list --resource-group <group_name> --name <app_name>
```

## <a name="next-steps"></a>Pasos siguientes

- [Configuración de aplicaciones web en Azure App Service](https://docs.microsoft.com/azure/app-service/web-sites-configure)
