---
title: Servicio de contenido desde Azure Storage en App Service en Linux
description: Configuración y servicio de contenido de Azure Storage en Azure App Service en Linux
author: msangapu
manager: jeconnoc
ms.service: app-service
ms.workload: web
ms.topic: article
ms.date: 2/04/2019
ms.author: msangapu
ms.openlocfilehash: 15cb31a3157b034089b1518a4e70eeb93ecc449e
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/07/2019
ms.locfileid: "67617106"
---
# <a name="serve-content-from-azure-storage-in-app-service-on-linux"></a>Servicio de contenido desde Azure Storage en App Service en Linux

En esta guía se muestra cómo servir contenido estático en App Service en Linux mediante [Azure Storage](/azure/storage/common/storage-introduction). Entre las ventajas se incluye protección y portabilidad del contenido, acceso a varias aplicaciones y varios métodos de transferencia.

## <a name="prerequisites"></a>Requisitos previos

- Una aplicación web existente (App Service en Linux o Web Apps for Containers).
- [CLI de Azure](/cli/azure/install-azure-cli) (2.0.46 o posterior).

## <a name="create-azure-storage"></a>Creación de una instancia de Azure Storage

> [!NOTE]
> Azure Storage es almacenamiento no predeterminado y se factura por separado, es decir, no se incluye con la aplicación web.
>
> Traiga su propio almacenamiento no admite el uso de la configuración del firewall de Storage debido a limitaciones de la infraestructura.
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
az webapp config storage-account list --resource-group <resource_group> --name <app_name>
```

## <a name="use-custom-storage-in-docker-compose"></a>Uso del almacenamiento personalizado en Docker Compose

Azure Storage se puede montar con aplicaciones de varios contenedores mediante custom-id. Para ver el nombre de custom-id, ejecute [`az webapp config storage-account list --name <app_name> --resource-group <resource_group>`](/cli/azure/webapp/config/storage-account?view=azure-cli-latest#az-webapp-config-storage-account-list).

En el archivo *docker-compose.yml*, asigne la opción `volumes` a `custom-id`. Por ejemplo:

```yaml
wordpress:
  image: wordpress:latest
  volumes:
  - <custom-id>:<path_in_container>
```

## <a name="next-steps"></a>Pasos siguientes

- [Configuración de aplicaciones web en Azure App Service](../configure-common.md)
