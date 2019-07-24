---
title: Configuración de Storage mediante Azure Files
description: Configuración y conexión a Azure Files en un contenedor de Windows en App Service.
author: msangapu-msft
manager: gwallace
ms.service: app-service
ms.workload: web
ms.topic: article
ms.date: 7/01/2019
ms.author: msangapu
ms.openlocfilehash: 2c12bf45c033fea185d976f1e9d644183407b5ac
ms.sourcegitcommit: a8b638322d494739f7463db4f0ea465496c689c6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/17/2019
ms.locfileid: "68297225"
---
# <a name="configure-azure-files-in-a-windows-container-on-app-service"></a>Configuración de Azure Files en un contenedor de Windows en App Service

> [!NOTE]
> Este artículo se aplica a contenedores de Windows personalizados. Para implementar App Service en _Linux_, consulte [Servicio de contenido desde Azure Storage](./containers/how-to-serve-content-from-azure-storage.md).
>

Esta guía muestra cómo acceder a Azure Storage en contenedores de Windows. Solo se admiten [recursos compartidos de archivos de Azure Files](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-cli) y [recursos compartidos de archivos Premium](https://docs.microsoft.com/azure/storage/files/storage-how-to-create-premium-fileshare). Puede usar recursos compartidos de archivos de Azure Files en esta guía. Entre las ventajas se incluye protección y portabilidad del contenido, acceso a varias aplicaciones y varios métodos de transferencia.

## <a name="prerequisites"></a>Requisitos previos

- [CLI de Azure](/cli/azure/install-azure-cli) (2.0.46 o posterior).
- [Una aplicación de contenedor de Windows existente en Azure App Service](https://docs.microsoft.com/azure/app-service/app-service-web-get-started-windows-container)
- [Creación de un recurso compartido de Azure Files](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-cli)
- [Carga de archivos en un recurso compartido de Azure Files](https://docs.microsoft.com/azure/storage/files/storage-files-deployment-guide)

> [!NOTE]
> Azure Files es almacenamiento no predeterminado y se factura por separado, es decir, no se incluye con la aplicación web. No admite el uso de una configuración de firewall debido a limitaciones de infraestructura.
>

## <a name="link-storage-to-your-web-app-preview"></a>Vinculación del almacenamiento a la aplicación web (versión preliminar)

 Para montar un recurso compartido de Azure Files en un directorio en la aplicación de App Service, use el comando [`az webapp config storage-account add`](https://docs.microsoft.com/cli/azure/webapp/config/storage-account?view=azure-cli-latest#az-webapp-config-storage-account-add). El tipo de almacenamiento debe ser AzureFiles.

```azurecli
az webapp config storage-account add --resource-group <group_name> --name <app_name> --custom-id <custom_id> --storage-type AzureFiles --share-name <share_name> --account-name <storage_account_name> --access-key "<access_key>" --mount-path <mount_path_directory of form c:<directory name> >
```

Este paso debe realizarlo con cualquier otro directorio que quiera vincular a un recurso compartido de Azure Files.

## <a name="verify"></a>Verify

Una vez que un recurso compartido de Azure Files se vincula a una aplicación web, puede ejecutar el siguiente comando para comprobarlo:

```azurecli
az webapp config storage-account list --resource-group <resource_group> --name <app_name>
```


## <a name="next-steps"></a>Pasos siguientes

- [Migración de una aplicación de ASP.NET a Azure App Service con un contenedor de Windows (versión preliminar)](app-service-web-tutorial-windows-containers-custom-fonts.md).
