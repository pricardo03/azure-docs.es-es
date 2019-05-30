---
title: Transferir datos hacia o desde Azure Files mediante AzCopy v10 | Microsoft Docs
description: Transferencia de datos con AzCopy y file storage.
services: storage
author: normesta
ms.service: storage
ms.topic: article
ms.date: 05/14/2019
ms.author: normesta
ms.subservice: common
ms.openlocfilehash: 5d11d5c0da350b9abf2e2b06a11a1690cf3f6922
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66247115"
---
# <a name="transfer-data-with-azcopy-and-file-storage"></a>Transferencia de datos con AzCopy y file storage 

AzCopy es una utilidad de línea de comandos que puede usar para copiar los blobs o archivos a o desde una cuenta de almacenamiento. En este artículo contiene comandos de ejemplo que funcionan con Azure Files.

Antes de comenzar, consulte el [empezar a trabajar con AzCopy](storage-use-azcopy-v10.md) artículo descargue AzCopy y familiarizarse con la herramienta.

## <a name="create-file-shares"></a>Crear recursos compartidos de archivos

Puede usar AzCopy `make` comando para crear un recurso compartido de archivos. El ejemplo de esta sección crea un recurso compartido de archivos denominado `myfileshare`.

|    |     |
|--------|-----------|
| **Sintaxis** | `azcopy make "https://<storage-account-name>.file.core.windows.net/<file-share-name>?<SAS-token>"` |
| **Ejemplo** | `azcopy make "https://mystorageaccount.file.core.windows.net/myfileshare?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D"` |

## <a name="upload-files"></a>Carga de archivos

Puede usar AzCopy `copy` comando para cargar archivos y carpetas desde su equipo local.

En esta sección se incluyen los ejemplos siguientes:

> [!div class="checklist"]
> * Cargar un archivo
> * Carga de una carpeta
> * Cargar archivos mediante caracteres comodín

> [!NOTE]
> AzCopy no calcular y almacenar el código hash del md5 del archivo automáticamente. Si desea que AzCopy para ello, anexe el `--put-md5` marca en cada comando de copia. De este modo, cuando se descarga el archivo, AzCopy calcula un hash MD5 de los datos descargados y comprueba que el hash MD5 almacenado en el archivo `Content-md5` propiedad coincide con el hash calculado.

### <a name="upload-a-file"></a>Cargar un archivo

|    |     |
|--------|-----------|
| **Sintaxis** | `azcopy cp "<local-file-path>" "https://<storage-account-name>.file.core.windows.net/<file-share-name>/<file-name>?<SAS-token>"` |
| **Ejemplo** | `azcopy copy "C:\myFolder\myTextFile.txt" "https://mystorageaccount.file.core.windows.net/myfileshare/myTextFile.txt?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D"` |

### <a name="upload-a-folder"></a>Carga de una carpeta

Este ejemplo copia una carpeta (y todos los archivos en esa carpeta) en un recurso compartido de archivos. El resultado es una carpeta en el recurso compartido de archivos con el mismo nombre.

|    |     |
|--------|-----------|
| **Sintaxis** | `azcopy copy "<local-folder-path>" "https://<storage-account-name>.file.core.windows.net/<file-share-name>?<SAS-token>" --recursive` |
| **Ejemplo** | `azcopy copy "C:\myFolder" "https://mystorageaccount.file.core.windows.net/myfileshare?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D" --recursive` |

Para copiar en una carpeta en el recurso compartido de archivos, simplemente especifique el nombre de la carpeta en la cadena de comandos.

|    |     |
|--------|-----------|
| **Ejemplo** | `azcopy copy "C:\myFolder" "https://mystorageaccount.file.core.windows.net/myfileshare/myFileShareFolder?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D" --recursive` |

Si especifica el nombre de una carpeta que no existe en el recurso compartido de archivos, AzCopy crea una nueva carpeta con ese nombre.

### <a name="upload-the-contents-of-a-folder"></a>Cargue el contenido de una carpeta

Puede cargar el contenido de una carpeta sin copiar la carpeta del contenedor mediante el carácter comodín (*).

|    |     |
|--------|-----------|
| **Sintaxis** | `azcopy copy "<local-folder-path>/*" "https://<storage-account-name>.file.core.windows.net/<file-share-name>/<folder-path>?<SAS-token>` |
| **Ejemplo** | `azcopy copy "C:\myFolder\*" "https://mystorageaccount.file.core.windows.net/myfileshare/myFileShareFolder?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D"` |

> [!NOTE]
> Anexar la `--recursive` marca para cargar archivos en todas las subcarpetas.

## <a name="download-files"></a>Descarga de archivos

Puede usar AzCopy `copy` comparte el comando para descargar archivos, carpetas y archivos en el equipo local.

En esta sección se incluyen los ejemplos siguientes:

> [!div class="checklist"]
> * Descarga de un archivo
> * Una carpeta de descarga
> * Descargar archivos mediante caracteres comodín

> [!NOTE]
> Si el `Content-md5` valor de propiedad de un archivo contiene un valor hash, AzCopy calcula un hash MD5 para datos descargados y comprueba que el hash MD5 almacenado en el archivo `Content-md5` propiedad coincide con el hash calculado. Si estos valores no coinciden, se produce un error en la descarga a menos que invalidar este comportamiento mediante la anexión de `--check-md5=NoCheck` o `--check-md5=LogOnly` en el comando de copia.

### <a name="download-a-file"></a>Descarga de un archivo

|    |     |
|--------|-----------|
| **Sintaxis** | `azcopy copy "https://<storage-account-name>.file.core.windows.net/<file-share-name>/<file-path>?<SAS-token>" "<local-file-path>"` |
| **Ejemplo** | `azcopy copy "https://mystorageaccount.file.core.windows.net/myfileshare/myTextFile.txt?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D" "C:\myFolder\myTextFile.txt"` |

### <a name="download-a-folder"></a>Una carpeta de descarga

|    |     |
|--------|-----------|
| **Sintaxis** | `azcopy copy "https://<storage-account-name>.file.core.windows.net/<file-share-name>/<folder-path>?<SAS-token>" "<local-folder-path>" --recursive` |
| **Ejemplo** | `azcopy copy "https://mystorageaccount.file.core.windows.net/myfileshare/myFileShareFolder?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D" "C:\myFolder"  --recursive` |

Este ejemplo resulta en una carpeta denominada `C:\myFolder\myFileShareFolder` que contiene todos los archivos descargados.

### <a name="download-the-contents-of-a-folder"></a>Descargar el contenido de una carpeta

Puede descargar el contenido de una carpeta sin copiar la carpeta del contenedor mediante el carácter comodín (*).

|    |     |
|--------|-----------|
| **Sintaxis** | `azcopy copy "https://<storage-account-name>.file.core.windows.net/<file-share-name>/*?<SAS-token>" "<local-folder-path>/"` |
| **Ejemplo** | `azcopy copy "https://mystorageaccount.file.core.windows.net/myfileshare/myFileShareFolder/*?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D" "C:\myFolder"` |

> [!NOTE]
> Anexar la `--recursive` marca para descargar archivos en todas las subcarpetas.

## <a name="next-steps"></a>Pasos siguientes

Encontrar más ejemplos en cualquiera de estos artículos:

- [Empezar a trabajar con AzCopy](storage-use-azcopy-v10.md)

- [Transferencia de datos con AzCopy y blob storage](storage-use-azcopy-blobs.md)

- [Transferencia de datos con AzCopy y Amazon S3 depósitos](storage-use-azcopy-s3.md)

- [Configurar, optimizar y solucionar problemas de AzCopy](storage-use-azcopy-configure.md)