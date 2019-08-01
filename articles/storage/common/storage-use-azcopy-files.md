---
title: Transferencia de datos con Azure Files como origen o destino mediante AzCopy v10 | Microsoft Docs
description: Transferencia de datos con AzCopy y File Storage.
services: storage
author: normesta
ms.service: storage
ms.topic: article
ms.date: 05/14/2019
ms.author: normesta
ms.subservice: common
ms.openlocfilehash: 69d7136396c3d989e63b8956d3e703cc7f9666c8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66687934"
---
# <a name="transfer-data-with-azcopy-and-file-storage"></a>Transferencia de datos con AzCopy y File Storage 

AzCopy es una utilidad de línea de comandos que puede usar para copiar blobs o archivos a una cuenta de almacenamiento o desde una cuenta de almacenamiento. Este artículo contiene comandos de ejemplo que funcionan con Azure Files.

Antes de comenzar, consulte el artículo [Introducción a AzCopy](storage-use-azcopy-v10.md) para descargar AzCopy y familiarizarse con la herramienta.

## <a name="create-file-shares"></a>Creación de recursos compartidos de archivos

Puede usar el comando `make` de AzCopy para crear un recurso compartido de archivos. El ejemplo de esta sección crea un recurso compartido de archivos denominado `myfileshare`.

|    |     |
|--------|-----------|
| **Sintaxis** | `azcopy make "https://<storage-account-name>.file.core.windows.net/<file-share-name>?<SAS-token>"` |
| **Ejemplo** | `azcopy make "https://mystorageaccount.file.core.windows.net/myfileshare?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D"` |

## <a name="upload-files"></a>Carga de archivos

Puede usar el comando `copy` de AzCopy para cargar archivos y directorios desde la máquina local.

En esta sección se incluyen los ejemplos siguientes:

> [!div class="checklist"]
> * Cargar un archivo
> * Subir un directorio
> * Cargar archivos mediante caracteres comodín

> [!NOTE]
> AzCopy no calcula de forma automática ni almacena el código hash md5 del archivo. Si quiere que AzCopy haga eso, anexe la marca `--put-md5` a cada comando de copia. De ese modo, cuando se descarga el archivo, AzCopy calcula un hash MD5 para los datos descargados y comprueba que el hash MD5 almacenado en la propiedad `Content-md5` del archivo coincide con el hash calculado.

### <a name="upload-a-file"></a>Cargar un archivo

|    |     |
|--------|-----------|
| **Sintaxis** | `azcopy cp "<local-file-path>" "https://<storage-account-name>.file.core.windows.net/<file-share-name>/<file-name>?<SAS-token>"` |
| **Ejemplo** | `azcopy copy "C:\myDirectory\myTextFile.txt" "https://mystorageaccount.file.core.windows.net/myfileshare/myTextFile.txt?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D"` |

### <a name="upload-a-directory"></a>Subir un directorio

En este ejemplo se copia un directorio (y todos sus archivos) en un recurso compartido de archivos. El resultado es un directorio en el recurso compartido de archivos con el mismo nombre.

|    |     |
|--------|-----------|
| **Sintaxis** | `azcopy copy "<local-directory-path>" "https://<storage-account-name>.file.core.windows.net/<file-share-name>?<SAS-token>" --recursive` |
| **Ejemplo** | `azcopy copy "C:\myDirectory" "https://mystorageaccount.file.core.windows.net/myfileshare?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D" --recursive` |

Para copiar un directorio dentro del recurso compartido de archivos, simplemente especifique el nombre de ese directorio en la cadena de comandos.

|    |     |
|--------|-----------|
| **Ejemplo** | `azcopy copy "C:\myDirectory" "https://mystorageaccount.file.core.windows.net/myfileshare/myFileShareDirectory?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D" --recursive` |

Si especifica el nombre de un directorio que no existe en el recurso compartido de archivos, AzCopy crea un directorio con ese nombre.

### <a name="upload-the-contents-of-a-directory"></a>Subir el contenido de un directorio

Puede cargar el contenido de un directorio sin copiar el propio directorio contenedor mediante el carácter comodín (*).

|    |     |
|--------|-----------|
| **Sintaxis** | `azcopy copy "<local-directory-path>/*" "https://<storage-account-name>.file.core.windows.net/<file-share-name>/<directory-path>?<SAS-token>` |
| **Ejemplo** | `azcopy copy "C:\myDirectory\*" "https://mystorageaccount.file.core.windows.net/myfileshare/myFileShareDirectory?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D"` |

> [!NOTE]
> Anexe la marca `--recursive` para cargar archivos en todos los subdirectorios.

## <a name="download-files"></a>Descarga de archivos

Puede usar el comando `copy` de AzCopy para descargar archivos, directorios y recursos compartidos de archivos en la máquina local.

En esta sección se incluyen los ejemplos siguientes:

> [!div class="checklist"]
> * Descarga de un archivo
> * Descargar un directorio
> * Descarga de archivos mediante caracteres comodín

> [!NOTE]
> Si el valor de la propiedad `Content-md5` de un archivo contiene un hash, AzCopy calcula un hash MD5 para los datos descargados y comprueba que el hash MD5 almacenado en la propiedad `Content-md5` del archivo coincide con el hash calculado. Si estos valores no coinciden, se produce un error en la descarga a menos que invalide este comportamiento mediante la anexión de `--check-md5=NoCheck` o `--check-md5=LogOnly` al comando de copia.

### <a name="download-a-file"></a>Descarga de un archivo

|    |     |
|--------|-----------|
| **Sintaxis** | `azcopy copy "https://<storage-account-name>.file.core.windows.net/<file-share-name>/<file-path>?<SAS-token>" "<local-file-path>"` |
| **Ejemplo** | `azcopy copy "https://mystorageaccount.file.core.windows.net/myfileshare/myTextFile.txt?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D" "C:\myDirectory\myTextFile.txt"` |

### <a name="download-a-directory"></a>Descargar un directorio

|    |     |
|--------|-----------|
| **Sintaxis** | `azcopy copy "https://<storage-account-name>.file.core.windows.net/<file-share-name>/<directory-path>?<SAS-token>" "<local-directory-path>" --recursive` |
| **Ejemplo** | `azcopy copy "https://mystorageaccount.file.core.windows.net/myfileshare/myFileShareDirectory?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D" "C:\myDirectory"  --recursive` |

En este ejemplo se crea un directorio denominado `C:\myDirectory\myFileShareDirectory` que contiene todos los archivos descargados.

### <a name="download-the-contents-of-a-directory"></a>Descargar el contenido de un directorio

Puede descargar el contenido de un directorio sin copiar el propio directorio contenedor mediante el carácter comodín (*).

|    |     |
|--------|-----------|
| **Sintaxis** | `azcopy copy "https://<storage-account-name>.file.core.windows.net/<file-share-name>/*?<SAS-token>" "<local-directory-path>/"` |
| **Ejemplo** | `azcopy copy "https://mystorageaccount.file.core.windows.net/myfileshare/myFileShareDirectory/*?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D" "C:\myDirectory"` |

> [!NOTE]
> Anexe la marca `--recursive` para descargar archivos en todos los subdirectorios.

## <a name="next-steps"></a>Pasos siguientes

Puede encontrar más ejemplos en cualquiera de estos artículos:

- [Introducción a AzCopy](storage-use-azcopy-v10.md)

- [Transferencia de datos con AzCopy y Blob Storage](storage-use-azcopy-blobs.md)

- [Transferencia de datos con AzCopy y cubos de Amazon S3](storage-use-azcopy-s3.md)

- [Configurar, optimizar y solucionar problemas de AzCopy](storage-use-azcopy-configure.md)