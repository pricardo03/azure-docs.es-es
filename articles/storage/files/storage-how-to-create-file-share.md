---
title: Creación de un recurso compartido de Azure Files | Microsoft Docs
description: Cómo crear un recurso compartido de archivos de Azure en Azure Files mediante Azure Portal, PowerShell y la CLI de Azure.
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 09/19/2017
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 3b30df7410f56c203eb600089cce130a8de23d1b
ms.sourcegitcommit: 12de9c927bc63868168056c39ccaa16d44cdc646
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2019
ms.locfileid: "72514608"
---
# <a name="create-a-file-share-in-azure-files"></a>Creación de un recurso compartido de archivos en Azure Files
Puede crear recursos compartidos de archivos de Azure mediante  [Azure Portal](https://portal.azure.com/), los cmdlets de PowerShell de Azure Storage, las bibliotecas de cliente de Azure Storage o la API de REST de Azure Storage. En este tutorial, aprenderá a:
* Crear un recurso compartido de Azure Files mediante Azure Portal
* [Crear un recurso compartido de Azure Files mediante Powershell](#create-file-share-through-powershell)
* [Crear un recurso compartido de Azure Files mediante la CLI](#create-file-share-through-command-line-interface-cli)

## <a name="prerequisites"></a>Requisitos previos
Para crear un recurso compartido de Azure Files, puede usar una cuenta de almacenamiento que ya exista o [crear una nueva cuenta de Azure Storage](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json). Para crear un recurso compartido de Azure Files con PowerShell, necesitará la clave de la cuenta y el nombre de la cuenta de almacenamiento. Necesitará una clave de la cuenta de almacenamiento si tiene previsto usar Powershell o la CLI.

> [!NOTE]
> Si desea crear recursos compartidos de archivos de más de 5 TiB, consulte nuestro artículo [Habilitación de recursos compartidos de archivos grandes](storage-files-how-to-create-large-file-share.md).

## <a name="create-a-file-share-through-the-azure-portal"></a>Creación de un recurso compartido de archivos mediante Azure Portal
1. **Vaya a la hoja de la cuenta de almacenamiento en Azure Portal**:    
    ![Hoja de la cuenta de almacenamiento](./media/storage-how-to-create-file-share/create-file-share-portal1.png)

2. **Haga clic en el botón Agregar recurso compartido de archivos**:    
    ![Haga clic en el botón Agregar recurso compartido de archivos](./media/storage-how-to-create-file-share/create-file-share-portal2.png)

3. **Proporcione el nombre y la cuota**:    
    ![Proporcione el nombre y cuota deseada para el nuevo recurso compartido de archivos](./media/storage-how-to-create-file-share/create-file-share-portal3.png)

4. **Visualice un nuevo recurso compartido de archivos**:  ![Visualización de un nuevo recurso compartido de archivos](./media/storage-how-to-create-file-share/create-file-share-portal4.png)

5. **Carga de un archivo**:  ![Carga de un archivo](./media/storage-how-to-create-file-share/create-file-share-portal5.png)

6. **Examine el recurso compartido de archivos y administre los directorios y archivos**:  ![Exploración del recurso compartido de archivos](./media/storage-how-to-create-file-share/create-file-share-portal6.png)


## <a name="create-file-share-through-powershell"></a>Creación de un recurso compartido de archivos mediante PowerShell
Para prepararse para usar PowerShell, descargue e instale los cmdlets de Azure PowerShell. Consulte  [Instalación y configuración de Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview)  para obtener instrucciones sobre el punto de instalación y la instalación.

> [!Note]  
> Es recomendable descargar e instalar el módulo más reciente de Azure PowerShell o actualizar a dicho módulo.

1. **Creación de una cuenta de almacenamiento nueva:** Una cuenta de almacenamiento es un grupo compartido de almacenamiento en el que puede implementar recursos compartidos de archivos de Azure u otros recursos de almacenamiento, como blobs o colas.

    ```PowerShell
    $resourceGroup = "myresourcegroup"
    $storAcctName = "myuniquestorageaccount"
    $region = "westus2"
    $storAcct = New-AzStorageAccount -ResourceGroupName $resourceGroup -Name $storAcctName -SkuName Standard_LRS -Location $region -Kind StorageV2
    ```

2. **Creación de un nuevo recurso compartido de archivos**:    
    
    ```powershell
    $shareName = "myshare"
    $share = New-AzStorageShare -Context $storAcct.Context -Name $shareName
    ```

> [!Note]  
> El nombre del recurso compartido de archivos debe estar en minúsculas. Para obtener detalles completos sobre cómo asignar un nombre a recursos compartidos y archivos, consulte  [Asignación de nombres y referencia a recursos compartidos, directorios, archivos y metadatos](https://msdn.microsoft.com/library/azure/dn167011.aspx).

## <a name="create-file-share-through-command-line-interface-cli"></a>Creación de un recurso compartido de archivos mediante la interfaz de línea de comandos (CLI)
1. **Para prepararse para usar la interfaz de línea de comandos (CLI), descargue e instale la CLI de Azure.**  
    Consulte  [Instalación de la CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli) e [Introducción a la CLI de Azure](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli).

2. **Creación de una cadena de conexión a la cuenta de almacenamiento donde desea crear el recurso compartido.**  
    Reemplace  ```<storage-account>```  y  ```<resource_group>```  por el nombre de la cuenta de almacenamiento y el grupo de recursos en el siguiente ejemplo:

   ```azurecli
    current_env_conn_string=$(az storage account show-connection-string -n <storage-account> -g <resource-group> --query 'connectionString' -o tsv)

    if [[ $current_env_conn_string == "" ]]; then  
        echo "Couldn't retrieve the connection string."
    fi
    ```

3. **Creación del recurso compartido de archivos**
    ```azurecli
    az storage share create --name files --quota 2048 --connection-string $current_env_conn_string > /dev/null
    ```

## <a name="next-steps"></a>Pasos siguientes
* [Conectar y montar un recurso compartido de archivos - Windows](storage-how-to-use-files-windows.md)
* [Conectar y montar un recurso compartido de archivos - Linux](../storage-how-to-use-files-linux.md)
* [Conectar y montar un recurso compartido de archivos - macOS](storage-how-to-use-files-mac.md)

Consulte los vínculos siguientes para más información acerca Azure Files.

* [P+F](../storage-files-faq.md)
* [Solución de problemas en Windows](storage-troubleshoot-windows-file-connection-problems.md)      
* [Solución de problemas en Linux](storage-troubleshoot-linux-file-connection-problems.md)   
