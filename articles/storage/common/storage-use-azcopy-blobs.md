---
title: Transferencia de datos con Azure Blob Storage como origen o destino mediante AzCopy v10 | Microsoft Docs
description: Este artículo contiene una colección de comandos de ejemplo de AzCopy que le ayudarán a crear contenedores, copiar archivos y sincronizar directorios entre sistemas de archivos y contenedores locales.
services: storage
author: normesta
ms.service: storage
ms.topic: article
ms.date: 05/14/2019
ms.author: normesta
ms.subservice: common
ms.openlocfilehash: 2ab561c387467086a40aa6676af347a107c2c452
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/30/2019
ms.locfileid: "68641121"
---
# <a name="transfer-data-with-azcopy-and-blob-storage"></a>Transferencia de datos con AzCopy y Blob Storage

AzCopy es una utilidad de línea de comandos que puede usar para copiar datos entre cuentas de almacenamiento. Este artículo contiene comandos de ejemplo que funcionan con Blob Storage.

## <a name="get-started"></a>Primeros pasos

Vea el artículo [Introducción a AzCopy](storage-use-azcopy-v10.md) para descargar AzCopy y obtener información sobre las formas de proporcionar credenciales de autorización para el servicio de almacenamiento.

> [!NOTE]
> En los ejemplos de este artículo se supone que ha autenticado la identidad mediante el comando `AzCopy login`. Después, AzCopy usa la cuenta de Azure AD para autorizar el acceso a los datos en Blob Storage.
>
> Si prefiere usar un token de SAS para autorizar el acceso a los datos de blob, puede anexar ese token a la dirección URL de recursos en cada comando AzCopy.
>
> Por ejemplo: `https://<storage-account-name>.blob.core.windows.net/<container-name>?<SAS-token>"`.

## <a name="create-a-container"></a>Crear un contenedor

Puede usar el comando `make` de AzCopy para crear un contenedor. En los ejemplos de esta sección se crea un contenedor denominado `mycontainer`.

|    |     |
|--------|-----------|
| **Sintaxis** | `azcopy make "https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>` |
| **Ejemplo** | `azcopy make "https://mystorageaccount.blob.core.windows.net/mycontainer"` |
| **Ejemplo** (espacio de nombres jerárquico) | `azcopy make "https://mystorageaccount.dfs.core.windows.net/mycontainer"` |

## <a name="upload-files"></a>Carga de archivos

Puede usar el comando `copy` de AzCopy para cargar archivos y directorios desde la máquina local.

En esta sección se incluyen los ejemplos siguientes:

> [!div class="checklist"]
> * Cargar un archivo
> * Subir un directorio
> * Cargar archivos mediante caracteres comodín

> [!NOTE]
> AzCopy no calcula de forma automática ni almacena el código hash md5 del archivo. Si quiere que AzCopy haga eso, anexe la marca `--put-md5` a cada comando de copia. De ese modo, cuando se descarga el blob, AzCopy calcula un hash MD5 para los datos descargados y comprueba que el hash MD5 almacenado en la propiedad `Content-md5` del blob coincide con el hash calculado.

### <a name="upload-a-file"></a>Cargar un archivo

|    |     |
|--------|-----------|
| **Sintaxis** | `azcopy cp "<local-file-path>" "https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>/<blob-name>"` |
| **Ejemplo** | `azcopy copy "C:\myDirectory\myTextFile.txt" "https://mystorageaccount.blob.core.windows.net/mycontainer/myTextFile.txt"` |
| **Ejemplo** (espacio de nombres jerárquico) | `azcopy copy "C:\myDirectory\myTextFile.txt" "https://mystorageaccount.dfs.core.windows.net/mycontainer/myTextFile.txt"` |

> [!NOTE]
> De forma predeterminada, AzCopy carga datos en blobs en bloques. Para cargar archivos como blobs en anexos o blobs en páginas, use la marca `--blob-type=[BlockBlob|PageBlob|AppendBlob]`.

### <a name="upload-a-directory"></a>Subir un directorio

En este ejemplo se copia un directorio (y todos sus archivos) en un contenedor de blobs. El resultado es un directorio en el contenedor con el mismo nombre.

|    |     |
|--------|-----------|
| **Sintaxis** | `azcopy copy "<local-directory-path>" "https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>" --recursive` |
| **Ejemplo** | `azcopy copy "C:\myDirectory" "https://mystorageaccount.blob.core.windows.net/mycontainer" --recursive` |
| **Ejemplo** (espacio de nombres jerárquico) | `azcopy copy "C:\myDirectory" "https://mystorageaccount.dfs.core.windows.net/mycontainer" --recursive` |

Para copiar un directorio dentro el contenedor, simplemente especifique el nombre de ese directorio en la cadena de comandos.

|    |     |
|--------|-----------|
| **Ejemplo** | `azcopy copy "C:\myDirectory" "https://mystorageaccount.blob.core.windows.net/mycontainer/myBlobDirectory" --recursive` |
| **Ejemplo** (espacio de nombres jerárquico) | `azcopy copy "C:\myDirectory" "https://mystorageaccount.dfs.core.windows.net/mycontainer/myBlobDirectory" --recursive` |

Si especifica el nombre de un directorio que no existe en el contenedor, AzCopy crea un directorio con ese nombre.

### <a name="upload-the-contents-of-a-directory"></a>Carga del contenido de un directorio

Puede cargar el contenido de un directorio sin copiar el propio directorio contenedor mediante el carácter comodín (*).

|    |     |
|--------|-----------|
| **Sintaxis** | `azcopy copy "<local-directory-path>\*" "https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>/<directory-path>` |
| **Ejemplo** | `azcopy copy "C:\myDirectory\*" "https://mystorageaccount.blob.core.windows.net/mycontainer/myBlobDirectory"` |
| **Ejemplo** (espacio de nombres jerárquico) | `azcopy copy "C:\myDirectory\*" "https://mystorageaccount.dfs.core.windows.net/mycontainer/myBlobDirectory"` |

> [!NOTE]
> Anexe la marca `--recursive` para cargar archivos en todos los subdirectorios.

## <a name="download-files"></a>Descarga de archivos

Puede usar el comando `copy` de AzCopy para descargar blobs, directorios y contenedores en el equipo local.

En esta sección se incluyen los ejemplos siguientes:

> [!div class="checklist"]
> * Descarga de un archivo
> * Descargar un directorio
> * Descarga de archivos mediante caracteres comodín

> [!NOTE]
> Si el valor de la propiedad `Content-md5` de un blob contiene un hash, AzCopy calcula un hash MD5 para los datos descargados y comprueba que el hash MD5 almacenado en la propiedad `Content-md5` del blob coincide con el hash calculado. Si estos valores no coinciden, se produce un error en la descarga a menos que invalide este comportamiento mediante la anexión de `--check-md5=NoCheck` o `--check-md5=LogOnly` al comando de copia.

### <a name="download-a-file"></a>Descarga de un archivo

|    |     |
|--------|-----------|
| **Sintaxis** | `azcopy copy "https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>/<blob-path>" "<local-file-path>"` |
| **Ejemplo** | `azcopy copy "https://mystorageaccount.blob.core.windows.net/mycontainer/myTextFile.txt" "C:\myDirectory\myTextFile.txt"` |
| **Ejemplo** (espacio de nombres jerárquico) | `azcopy copy "https://mystorageaccount.dfs.core.windows.net/mycontainer/myTextFile.txt" "C:\myDirectory\myTextFile.txt"` |

### <a name="download-a-directory"></a>Descarga de un directorio

|    |     |
|--------|-----------|
| **Sintaxis** | `azcopy copy "https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>/<directory-path>" "<local-directory-path>" --recursive` |
| **Ejemplo** | `azcopy copy "https://mystorageaccount.blob.core.windows.net/mycontainer/myBlobDirectory "C:\myDirectory"  --recursive` |
| **Ejemplo** (espacio de nombres jerárquico) | `azcopy copy "https://mystorageaccount.dfs.core.windows.net/mycontainer/myBlobDirectory "C:\myDirectory"  --recursive` |

En este ejemplo se crea un directorio denominado `C:\myDirectory\myBlobDirectory` que contiene todos los archivos descargados.

### <a name="download-the-contents-of-a-directory"></a>Descargar el contenido de un directorio

Puede descargar el contenido de un directorio sin copiar el propio directorio contenedor mediante el carácter comodín (*).

> [!NOTE]
> En la actualidad, este escenario solo se admite para las cuentas que no tienen un espacio de nombres jerárquico.

|    |     |
|--------|-----------|
| **Sintaxis** | `azcopy copy "https://<storage-account-name>.blob.core.windows.net/<container-name>/*" "<local-directory-path>/"` |
| **Ejemplo** | `azcopy copy "https://mystorageaccount.blob.core.windows.net/mycontainer/myBlobDirectory/*" "C:\myDirectory"` |

> [!NOTE]
> Anexe la marca `--recursive` para descargar archivos en todos los subdirectorios.

## <a name="copy-blobs-between-storage-accounts"></a>Copia de blobs entre cuentas de almacenamiento

Puede usar AzCopy para copiar blobs a otras cuentas de almacenamiento. La operación de copia es sincrónica, por lo que cuando el comando devuelve un resultado, eso indica que se han copiado todos los archivos.

> [!NOTE]
> En la actualidad, este escenario solo se admite para las cuentas que no tienen un espacio de nombres jerárquico. 

AzCopy usa [API](https://docs.microsoft.com/en-us/rest/api/storageservices/put-page-from-url) [de servidor a servidor](https://docs.microsoft.com/rest/api/storageservices/put-block-from-url), por lo que los datos se copian directamente entre servidores de almacenamiento. En estas operaciones de copia no se usa el ancho de banda de red del equipo.

En esta sección se incluyen los ejemplos siguientes:

> [!div class="checklist"]
> * Copia de un blob a otra cuenta de almacenamiento
> * Copia de un directorio a otra cuenta de almacenamiento
> * Copia de un contenedor a otra cuenta de almacenamiento
> * Copia de todos los contenedores, directorios y archivos a otra cuenta de almacenamiento

> [!NOTE]
> En la versión actual, tiene que anexar un token de SAS en cada dirección URL de origen. Si proporciona credenciales de autorización mediante Azure Active Directory (AD), puede omitir el token de SAS solo de la dirección URL de destino. 

### <a name="copy-a-blob-to-another-storage-account"></a>Copia de un blob a otra cuenta de almacenamiento

|    |     |
|--------|-----------|
| **Sintaxis** | `azcopy cp "https://<source-storage-account-name>.blob.core.windows.net/<container-name>/<blob-path>?<SAS-token>" "https://<destination-storage-account-name>.blob.core.windows.net/<container-name>/<blob-path>"` |
| **Ejemplo** | `azcopy cp "https://mysourceaccount.blob.core.windows.net/mycontainer/myTextFile.txt?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D" "https://mydestinationaccount.blob.core.windows.net/mycontainer/myTextFile.txt"` |

### <a name="copy-a-directory-to-another-storage-account"></a>Copia de un directorio a otra cuenta de almacenamiento

|    |     |
|--------|-----------|
| **Sintaxis** | `azcopy cp "https://<source-storage-account-name>.blob.core.windows.net/<container-name>/<directory-path>?<SAS-token>" "https://<destination-storage-account-name>.blob.core.windows.net/<container-name>" --recursive` |
| **Ejemplo** | `azcopy cp "https://mysourceaccount.blob.core.windows.net/mycontainer/myBlobDirectory?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D" "https://mydestinationaccount.blob.core.windows.net/mycontainer" --recursive` |

### <a name="copy-a-containers-to-another-storage-account"></a>Copia de un contenedor a otra cuenta de almacenamiento

|    |     |
|--------|-----------|
| **Sintaxis** | `azcopy cp "https://<source-storage-account-name>.blob.core.windows.net/<container-name>?<SAS-token>" "https://<destination-storage-account-name>.blob.core.windows.net/<container-name>" --recursive` |
| **Ejemplo** | `azcopy cp "https://mysourceaccount.blob.core.windows.net/mycontainer?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D" "https://mydestinationaccount.blob.core.windows.net/mycontainer" --recursive` |

### <a name="copy-all-containers-directories-and-files-to-another-storage-account"></a>Copia de todos los contenedores, directorios y archivos a otra cuenta de almacenamiento

|    |     |
|--------|-----------|
| **Sintaxis** | `azcopy cp "https://<source-storage-account-name>.blob.core.windows.net/?<SAS-token>" "https://<destination-storage-account-name>.blob.core.windows.net/" --recursive"` |
| **Ejemplo** | `azcopy cp "https://mysourceaccount.blob.core.windows.net?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D" "https://mydestinationaccount.blob.core.windows.net" --recursive` |

## <a name="synchronize-files"></a>Sincronización de archivos

Puede sincronizar el contenido de un sistema de archivos local con un contenedor de blobs. La sincronización es unidireccional. En otras palabras, tendrá que elegir cuál de estos dos puntos de conexión es el origen y cuál es el destino.

> [!NOTE]
> En la actualidad, este escenario solo se admite para las cuentas que no tienen un espacio de nombres jerárquico. La versión actual de AzCopy no sincroniza entre otros orígenes y destinos (por ejemplo: Almacenamiento de archivos o cubos de Amazon Web Services (AWS) S3).

El comando `sync` compara nombres de archivo y las marcas de tiempo de la última modificación. Establezca la marca opcional `--delete-destination` en un valor de `true` o `prompt` para eliminar archivos en el directorio de destino si esos archivos ya no existen en el directorio de origen.

Si establece la marca `--delete-destination` en `true`, AzCopy elimina los archivos sin proporcionar un mensaje. Si quiere que aparezca un mensaje antes de que AzCopy elimine un archivo, establezca la marca `--delete-destination` en `prompt`.

> [!NOTE]
> Para evitar eliminaciones accidentales, asegúrese de habilitar la característica de [eliminación temporal](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete) antes de usar la marca `--delete-destination=prompt|true`.

### <a name="update-a-container-with-changes-to-a-local-file-system"></a>Actualización de un contenedor con los cambios realizados en un sistema de archivos local

En este caso, el contenedor es el destino y el sistema de archivos local es el origen.

|    |     |
|--------|-----------|
| **Sintaxis** | `azcopy sync "<local-directory-path>" "https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>" --recursive` |
| **Ejemplo** | `azcopy sync "C:\myDirectory" "https://mystorageaccount.blob.core.windows.net/mycontainer" --recursive` |

### <a name="update-a-local-file-system-with-changes-to-a-container"></a>Actualización de un sistema de archivos local con los cambios realizados en un contenedor

En este caso, el sistema de archivos local es el destino y el contenedor es el origen.

|    |     |
|--------|-----------|
| **Sintaxis** | `azcopy sync "https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>" "C:\myDirectory" --recursive` |
| **Ejemplo** | `azcopy sync "https://mystorageaccount.blob.core.windows.net/mycontainer" "C:\myDirectory" --recursive` |
|

## <a name="next-steps"></a>Pasos siguientes

Puede encontrar más ejemplos en cualquiera de estos artículos:

- [Introducción a AzCopy](storage-use-azcopy-v10.md)

- [Tutorial: Migración de datos locales al almacenamiento en la nube mediante AzCopy](storage-use-azcopy-migrate-on-premises-data.md)

- [Transferencia de datos con AzCopy y File Storage](storage-use-azcopy-files.md)

- [Transferencia de datos con AzCopy y cubos de Amazon S3](storage-use-azcopy-s3.md)

- [Configurar, optimizar y solucionar problemas de AzCopy](storage-use-azcopy-configure.md)
