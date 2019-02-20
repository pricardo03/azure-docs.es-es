---
title: 'Copiar datos de Blob Storage con AzCopy: proceso de ciencia de datos en equipo'
description: Copiar datos de y en Azure Blob Storage con AzCopy
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 11/04/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 6c0951eb6ad3b7651da97e1a49c5edf5ab55a199
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2019
ms.locfileid: "56209581"
---
# <a name="copy-data-to-and-from-azure-blob-storage-using-azcopy"></a>Copiar datos de y en Azure Blob Storage con AzCopy
AzCopy es una utilidad de línea de comandos diseñada para realizar operaciones de carga, descarga y copia de datos a y desde los servicios Table Storage, File Storage y Blob Storage de Microsoft Azure.

Para obtener instrucciones sobre la instalación de AzCopy y de información adicional sobre su uso con la plataforma de Azure, consulte [Introducción a la utilidad de línea de comandos AzCopy](../../storage/common/storage-use-azcopy.md).

[!INCLUDE [blob-storage-tool-selector](../../../includes/machine-learning-blob-storage-tool-selector.md)]

> [!NOTE]
> Si está usando la máquina virtual que se configuró con los scripts ofrecidos por [Máquinas virtuales de ciencia de datos en Azure](virtual-machines.md), AzCopy ya está instalado en la máquina virtual.
> 
> [!NOTE]
> Para ver una introducción completa a Azure Blob Storage, consulte [Aspectos básicos de Azure Blob](../../storage/blobs/storage-dotnet-how-to-use-blobs.md) y [Azure Blob Service](https://msdn.microsoft.com/library/azure/dd179376.aspx).
> 
> 

## <a name="prerequisites"></a>Requisitos previos
En este documento se supone que tiene una suscripción de Azure y una cuenta de almacenamiento y la clave de almacenamiento correspondiente para dicha cuenta. Antes de cargar o descargar datos, debe conocer su nombre de cuenta de almacenamiento de Azure y la clave de cuenta.

* Para configurar una suscripción a Azure, consulte [Prueba gratuita de un mes](https://azure.microsoft.com/pricing/free-trial/).
* Para obtener instrucciones sobre la creación de una cuenta de almacenamiento y para obtener información de cuentas y claves, vea [Acerca de las cuentas de almacenamiento de Azure](../../storage/common/storage-create-storage-account.md).

## <a name="run-azcopy-commands"></a>Ejecución de comandos de AzCopy
Para ejecutar comandos de AzCopy, abra una ventana de comandos y navegue al directorio de instalación de AzCopy en el equipo, donde se encuentra el ejecutable AzCopy.exe. 

La sintaxis básica del comando AzCopy es:

    AzCopy /Source:<source> /Dest:<destination> [Options]

> [!NOTE]
> Puede agregar la ubicación de instalación de AzCopy a la ruta del sistema y, a continuación, ejecutar los comandos desde cualquier directorio. De forma predeterminada, AzCopy se instala en *%ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy* o *%ProgramFiles%\Microsoft SDKs\Azure\AzCopy*.
> 
> 

## <a name="upload-files-to-an-azure-blob"></a>Carga de archivos en un blob de Azure
Para descargar un archivo, use el comando siguiente:

    # Upload from local file system
    AzCopy /Source:<your_local_directory> /Dest: https://<your_account_name>.blob.core.windows.net/<your_container_name> /DestKey:<your_account_key> /S


## <a name="download-files-from-an-azure-blob"></a>Descarga de archivos de un blob de Azure
Para descargar un archivo desde un blob de Azure, use el comando siguiente:

    # Downloading blobs to local file system
    AzCopy /Source:https://<your_account_name>.blob.core.windows.net/<your_container_name>/<your_sub_directory_at_blob>  /Dest:<your_local_directory> /SourceKey:<your_account_key> /Pattern:<file_pattern> /S


## <a name="copy-blobs-between-azure-containers"></a>Copiar blobs entre contenedores de Azure
Para copiar blobs entre contenedores de Azure, use el siguiente comando:

    # Copying blobs between Azure containers
    AzCopy /Source:https://<your_account_name1>.blob.core.windows.net/<your_container_name1>/<your_sub_directory_at_blob1> /Dest:https://<your_account_name2>.blob.core.windows.net/<your_container_name2>/<your_sub_directory_at_blob2> /SourceKey:<your_account_key1> /DestKey:<your_account_key2> /Pattern:<file_pattern> /S

    <your_account_name>: your storage account name
    <your_account_key>: your storage account key
    <your_container_name>: your container name
    <your_sub_directory_at_blob>: the sub directory in the container
    <your_local_directory>: directory of local file system where files to be uploaded from or the directory of local file system files to be downloaded to
    <file_pattern>: pattern of file names to be copied. The standard wildcards are supported


## <a name="tips-for-using-azcopy"></a>Sugerencias de uso de AzCopy
> [!TIP]
> 1. Al **cargar** archivos, */S* lo hace de forma recursiva. Sin este parámetro, no se cargan los archivos de los subdirectorios.  
> 2. Al **descargar** el archivo, */S* busca el contenedor de manera recursiva hasta que se descarguen todos los archivos del directorio especificado y sus subdirectorios, o todos los archivos que coincidan con el patrón especificado en el directorio especificado y sus subdirectorios.  
> 3. No se puede especificar un **archivo de blob específico** para descargar mediante el parámetro */Source* . Para descargar un archivo específico, especifique el nombre del archivo blob que se va a descargar mediante el parámetro */Pattern* . **/S** se puede usar para que AzCopy busque de forma recursiva un patrón de nombre de archivo. Sin el parámetro de patrón, AzCopy descargará todos los archivos en ese directorio.
> 
> 

