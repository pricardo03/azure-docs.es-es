---
title: Creación de un recurso compartido de archivos de Azure
titleSuffix: Azure Files
description: Creación de un recurso compartido de archivos de Azure mediante Azure Portal, PowerShell y la CLI de Azure.
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 09/19/2017
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: ca4904940c7974e83445892b9597a9d3c36b44ee
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2019
ms.locfileid: "75452886"
---
# <a name="create-a-file-share"></a>Creación de un recurso compartido de archivos
Puede crear recursos compartidos en Azure Files mediante  [Azure Portal](https://portal.azure.com/), los cmdlets de PowerShell de Azure Storage, las bibliotecas de cliente de Azure Storage o la API REST de Azure Storage. En este artículo, aprenderá a:
- Crear un recurso compartido de archivos de Azure mediante Azure Portal
- Crear un recurso compartido de archivos de Azure mediante PowerShell
- Crear un recurso compartido de archivos de Azure mediante la CLI de Azure

## <a name="prerequisites"></a>Prerequisites
Para crear un recurso compartido de Azure Files, puede usar una cuenta de almacenamiento que ya exista o [crear una nueva cuenta de Azure Storage](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json). Para crear un recurso compartido de Azure Files con PowerShell, necesitará la clave de la cuenta y el nombre de la cuenta de almacenamiento. Necesitará una clave de la cuenta de almacenamiento si tiene previsto usar PowerShell o la CLI.

> [!NOTE]
> Si desea crear recursos compartidos de archivos de más de 5 TiB, consulte nuestro artículo [Habilitación de recursos compartidos de archivos grandes](storage-files-how-to-create-large-file-share.md).

## <a name="create-a-file-share-through-the-azure-portal"></a>Creación de un recurso compartido de archivos mediante Azure Portal

1. Vaya al panel **Cuenta de almacenamiento** en Azure Portal.
    ![Panel de cuenta de almacenamiento](./media/storage-how-to-create-file-share/create-file-share-portal1.png)

1. Haga clic en el botón para **agregar un recurso compartido de archivos**.
    ![Botón para agregar recurso compartido de archivos](./media/storage-how-to-create-file-share/create-file-share-portal2.png)

1. Escriba la información de **Nombre** y **Cuota**.
    ![El nombre y la cuota del nuevo recurso compartido de archivos](./media/storage-how-to-create-file-share/create-file-share-portal3.png)

1. Vea el nuevo recurso compartido de archivos.
    ![El nuevo recurso compartido de archivos en la interfaz de usuario](./media/storage-how-to-create-file-share/create-file-share-portal4.png)

1. Cargar un archivo.
    ![Botón Cargar en la barra de navegación](./media/storage-how-to-create-file-share/create-file-share-portal5.png)

1. Examine el recurso compartido de archivos y administre los directorios y archivos.
    ![Vista de carpetas de recursos compartidos de archivos](./media/storage-how-to-create-file-share/create-file-share-portal6.png)

## <a name="create-a-file-share-through-powershell"></a>Creación de un recurso compartido de archivos mediante PowerShell

Descargue e instale los cmdlets de Azure PowerShell. Consulte  [Instalación y configuración de Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview)  para obtener instrucciones sobre el punto de instalación y la instalación.

> [!Note]  
> Es recomendable descargar e instalar el módulo más reciente de Azure PowerShell o actualizar a dicho módulo.

1. Cree una cuenta de almacenamiento nueva.
    Una cuenta de almacenamiento es un grupo compartido de almacenamiento en el que puede implementar recursos compartidos de archivos de Azure u otros recursos de almacenamiento como blobs o colas.

    ```PowerShell
    $resourceGroup = "myresourcegroup"
    $storAcctName = "myuniquestorageaccount"
    $region = "westus2"
    $storAcct = New-AzStorageAccount -ResourceGroupName $resourceGroup -Name $storAcctName -SkuName Standard_LRS -Location $region -Kind StorageV2
    ```

1. Cree un recurso compartido de archivos.        
    ```PowerShell
    $shareName = "myshare"
    $share = New-AzStorageShare -Context $storAcct.Context -Name $shareName
    ```

> [!Note]  
> El nombre del recurso compartido de archivos debe estar en minúsculas. Para obtener detalles completos sobre cómo asignar un nombre a recursos compartidos y archivos, consulte  [Asignación de nombres y referencia a recursos compartidos, directorios, archivos y metadatos](https://msdn.microsoft.com/library/azure/dn167011.aspx).

## <a name="create-a-file-share-through-the-cli"></a>Creación de un recurso compartido de archivos mediante la CLI

1. Descargue e instale la CLI de Azure.
    Consulte  [Instalación de la CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli) e [Introducción a la CLI de Azure](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli).

1. Creación de una cadena de conexión a la cuenta de almacenamiento donde desea crear el recurso compartido.
    Reemplace  ```<storage-account>```  y  ```<resource_group>```  por el nombre de la cuenta de almacenamiento y el grupo de recursos en el siguiente ejemplo:

   ```azurecli
    current_env_conn_string=$(az storage account show-connection-string -n <storage-account> -g <resource-group> --query 'connectionString' -o tsv)

    if [[ $current_env_conn_string == "" ]]; then  
        echo "Couldn't retrieve the connection string."
    fi
    ```

1. Cree el recurso compartido de archivos.
    ```azurecli
    az storage share create --name files --quota 2048 --connection-string $current_env_conn_string > /dev/null
    ```

## <a name="next-steps"></a>Pasos siguientes

* [Conexión y montaje de un recurso compartido de archivos en Windows](storage-how-to-use-files-windows.md)
* [Conecte y monte un recurso compartido de archivos en Linux](storage-how-to-use-files-linux.md)
* [Conecte y monte un recurso compartido de archivos en macOS](storage-how-to-use-files-mac.md)

Consulte los vínculos siguientes para más información sobre Azure Files:

* [Preguntas más frecuentes sobre archivos de almacenamiento](storage-files-faq.md)
* [Solución de problemas de Azure Files en Windows](storage-troubleshoot-windows-file-connection-problems.md)
* [Solución de problemas de Azure Files en Linux](storage-troubleshoot-linux-file-connection-problems.md)
