---
title: Transferir datos desde o hacia Azure Blob storage mediante AzCopy v10 | Microsoft Docs
description: En este artículo contiene una colección de AzCopy ejemplo comandos que le ayudarán a crean contenedores, copie los archivos y sincronización carpetas entre sistemas de archivos locales y los contenedores.
services: storage
author: normesta
ms.service: storage
ms.topic: article
ms.date: 05/14/2019
ms.author: normesta
ms.subservice: common
ms.openlocfilehash: 98e33f838ee9b6f506bf1dc01e1dd61ad587aa05
ms.sourcegitcommit: 8c49df11910a8ed8259f377217a9ffcd892ae0ae
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66299401"
---
# <a name="transfer-data-with-azcopy-and-blob-storage"></a>Transferencia de datos con AzCopy y Blob storage

AzCopy es una utilidad de línea de comandos que puede usar para copiar datos a, desde o entre cuentas de almacenamiento. En este artículo contiene comandos de ejemplo que funcionan con almacenamiento de blobs.

## <a name="get-started"></a>Introducción

Consulte la [empezar a trabajar con AzCopy](storage-use-azcopy-v10.md) artículo descargue AzCopy y obtenga información acerca de las formas que puede proporcionar las credenciales de autorización para el servicio de almacenamiento.

> [!NOTE]
> Los ejemplos en este artículo se supone que ha autenticado su identidad mediante el uso de la `AzCopy login` comando. AzCopy, a continuación, utiliza la cuenta de Azure AD para autorizar el acceso a datos en Blob storage.
>
> Si prefiere utilizar un token de SAS para autorizar el acceso a los datos blob, puede anexar ese token a la dirección URL de recursos en cada comando de AzCopy.
>
> Por ejemplo: `https://<storage-account-name>.blob.core.windows.net/<container-name>?<SAS-token>"`.

## <a name="create-a-container"></a>Crear un contenedor

Puede usar AzCopy `make` comando para crear un contenedor. Los ejemplos en esta sección crean un contenedor denominado `mycontainer`.

|    |     |
|--------|-----------|
| **Sintaxis** | `azcopy make "https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>` |
| **Ejemplo** | `azcopy make "https://mystorageaccount.blob.core.windows.net/mycontainer"` |
| **Ejemplo** (espacio de nombres jerárquico) | `azcopy make "https://mystorageaccount.dfs.core.windows.net/mycontainer"` |

## <a name="upload-files"></a>Carga de archivos

Puede usar AzCopy `copy` comando para cargar archivos y carpetas desde su equipo local.

En esta sección se incluyen los ejemplos siguientes:

> [!div class="checklist"]
> * Cargar un archivo
> * Carga de una carpeta
> * Cargar archivos mediante caracteres comodín

> [!NOTE]
> AzCopy no calcular y almacenar el código hash del md5 del archivo automáticamente. Si desea que AzCopy para ello, anexe el `--put-md5` marca en cada comando de copia. De este modo, cuando se descarga el blob, AzCopy calcula un hash MD5 de los datos descargados y comprueba que el hash MD5 almacenado en el blob `Content-md5` propiedad coincide con el hash calculado.

### <a name="upload-a-file"></a>Cargar un archivo

|    |     |
|--------|-----------|
| **Sintaxis** | `azcopy cp "<local-file-path>" "https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>/<blob-name>"` |
| **Ejemplo** | `azcopy copy "C:\myFolder\myTextFile.txt" "https://mystorageaccount.blob.core.windows.net/mycontainer/myTextFile.txt"` |
| **Ejemplo** (espacio de nombres jerárquico) | `azcopy copy "C:\myFolder\myTextFile.txt" "https://mystorageaccount.dfs.core.windows.net/mycontainer/myTextFile.txt"` |

> [!NOTE]
> Forma predeterminada, AzCopy carga datos en blobs en bloques. Para cargar archivos como Blobs en anexos o Blobs en páginas use la marca `--blob-type=[BlockBlob|PageBlob|AppendBlob]`.

### <a name="upload-a-folder"></a>Carga de una carpeta

En este ejemplo se copia una carpeta (y todos los archivos en esa carpeta) en un contenedor de blobs. El resultado es una carpeta en el contenedor con el mismo nombre.

|    |     |
|--------|-----------|
| **Sintaxis** | `azcopy copy "<local-folder-path>" "https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>" --recursive` |
| **Ejemplo** | `azcopy copy "C:\myFolder" "https://mystorageaccount.blob.core.windows.net/mycontainer" --recursive` |
| **Ejemplo** (espacio de nombres jerárquico) | `azcopy copy "C:\myFolder" "https://mystorageaccount.dfs.core.windows.net/mycontainer" --recursive` |

Para copiar en una carpeta dentro del contenedor, simplemente especifique el nombre de la carpeta en la cadena de comandos.

|    |     |
|--------|-----------|
| **Ejemplo** | `azcopy copy "C:\myFolder" "https://mystorageaccount.blob.core.windows.net/mycontainer/myBlobFolder" --recursive` |
| **Ejemplo** (espacio de nombres jerárquico) | `azcopy copy "C:\myFolder" "https://mystorageaccount.dfs.core.windows.net/mycontainer/myBlobFolder" --recursive` |

Si especifica el nombre de una carpeta que no existe en el contenedor, AzCopy crea una nueva carpeta con ese nombre.

### <a name="upload-the-contents-of-a-folder"></a>Cargue el contenido de una carpeta

Puede cargar el contenido de una carpeta sin copiar la carpeta del contenedor mediante el carácter comodín (*).

|    |     |
|--------|-----------|
| **Sintaxis** | `azcopy copy "<local-folder-path>\*" "https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>/<folder-path>` |
| **Ejemplo** | `azcopy copy "C:\myFolder\*" "https://mystorageaccount.blob.core.windows.net/mycontainer/myBlobFolder"` |
| **Ejemplo** (espacio de nombres jerárquico) | `azcopy copy "C:\myFolder\*" "https://mystorageaccount.dfs.core.windows.net/mycontainer/myBlobFolder"` |

> [!NOTE]
> Anexar la `--recursive` marca para cargar archivos en todas las subcarpetas.

## <a name="download-files"></a>Descarga de archivos

Puede usar AzCopy `copy` comando para descargar blobs, carpetas y contenedores en el equipo local.

En esta sección se incluyen los ejemplos siguientes:

> [!div class="checklist"]
> * Descarga de un archivo
> * Una carpeta de descarga
> * Descargar archivos mediante caracteres comodín

> [!NOTE]
> Si el `Content-md5` valor de propiedad de un blob contiene un hash, AzCopy calcula un hash MD5 para datos descargados y comprueba que el hash MD5 almacenado en el blob `Content-md5` propiedad coincide con el hash calculado. Si estos valores no coinciden, se produce un error en la descarga a menos que invalidar este comportamiento mediante la anexión de `--check-md5=NoCheck` o `--check-md5=LogOnly` en el comando de copia.

### <a name="download-a-file"></a>Descarga de un archivo

|    |     |
|--------|-----------|
| **Sintaxis** | `azcopy copy "https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>/<blob-path>" "<local-file-path>"` |
| **Ejemplo** | `azcopy copy "https://mystorageaccount.blob.core.windows.net/mycontainer/myTextFile.txt" "C:\myFolder\myTextFile.txt"` |
| **Ejemplo** (espacio de nombres jerárquico) | `azcopy copy "https://mystorageaccount.dfs.core.windows.net/mycontainer/myTextFile.txt" "C:\myFolder\myTextFile.txt"` |

### <a name="download-a-folder"></a>Una carpeta de descarga

|    |     |
|--------|-----------|
| **Sintaxis** | `azcopy copy "https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>/<folder-path>" "<local-folder-path>" --recursive` |
| **Ejemplo** | `azcopy copy "https://mystorageaccount.blob.core.windows.net/mycontainer/myBlobFolder "C:\myFolder"  --recursive` |
| **Ejemplo** (espacio de nombres jerárquico) | `azcopy copy "https://mystorageaccount.dfs.core.windows.net/mycontainer/myBlobFolder "C:\myFolder"  --recursive` |

Este ejemplo resulta en una carpeta denominada `C:\myFolder\myBlobFolder` que contiene todos los archivos descargados.

### <a name="download-the-contents-of-a-folder"></a>Descargar el contenido de una carpeta

Puede descargar el contenido de una carpeta sin copiar la carpeta del contenedor mediante el carácter comodín (*).

> [!NOTE]
> Actualmente, este escenario se admite solo para las cuentas que no tienen un espacio de nombres jerárquico.

|    |     |
|--------|-----------|
| **Sintaxis** | `azcopy copy "https://<storage-account-name>.blob.core.windows.net/<container-name>/*" "<local-folder-path>/"` |
| **Ejemplo** | `azcopy copy "https://mystorageaccount.blob.core.windows.net/mycontainer/myBlobFolder/*" "C:\myFolder"` |

> [!NOTE]
> Anexar la `--recursive` marca para descargar archivos en todas las subcarpetas.

## <a name="copy-blobs-between-storage-accounts"></a>Copiar blobs entre cuentas de almacenamiento

Puede usar AzCopy para copiar los blobs a otras cuentas de almacenamiento. La operación de copia es sincrónica, por lo que devuelve el comando, que indica que se han copiado todos los archivos.

> [!NOTE]
> Actualmente, este escenario se admite solo para las cuentas que no tienen un espacio de nombres jerárquico.

AzCopy usa el [colocar bloque From URL](https://docs.microsoft.com/rest/api/storageservices/put-block-from-url) API, por lo que se copian datos directamente entre los servidores de almacenamiento. Estas operaciones de copia no utilizan el ancho de banda de red del equipo.

En esta sección se incluyen los ejemplos siguientes:

> [!div class="checklist"]
> * Copiar un blob en otra cuenta de almacenamiento
> * Copiar una carpeta a otra cuenta de almacenamiento
> * Copiar un contenedor en otra cuenta de almacenamiento
> * Copiar todos los contenedores, carpetas y archivos en otra cuenta de almacenamiento

### <a name="copy-a-blob-to-another-storage-account"></a>Copiar un blob en otra cuenta de almacenamiento

|    |     |
|--------|-----------|
| **Sintaxis** | `azcopy cp "https://<source-storage-account-name>.blob.core.windows.net/<container-name>/<blob-path>" "https://<destination-storage-account-name>.blob.core.windows.net/<container-name>/<blob-path>"` |
| **Ejemplo** | `azcopy cp "https://mysourceaccount.blob.core.windows.net/mycontainer/myTextFile.txt" "https://mydestinationaccount.blob.core.windows.net/mycontainer/myTextFile.txt"` |

### <a name="copy-a-folder-to-another-storage-account"></a>Copiar una carpeta a otra cuenta de almacenamiento

|    |     |
|--------|-----------|
| **Sintaxis** | `azcopy cp "https://<source-storage-account-name>.blob.core.windows.net/<container-name>/<folder-path>" "https://<destination-storage-account-name>.blob.core.windows.net/<container-name>/<folder-path>" --recursive` |
| **Ejemplo** | `azcopy cp "https://mysourceaccount.blob.core.windows.net/mycontainer/myBlobFolder" "https://mydestinationaccount.blob.core.windows.net/mycontainer/myBlobFolder" --recursive` |

### <a name="copy-a-containers-to-another-storage-account"></a>Copiar un contenedor en otra cuenta de almacenamiento

|    |     |
|--------|-----------|
| **Sintaxis** | `azcopy cp "https://<source-storage-account-name>.blob.core.windows.net/<container-name>" "https://<destination-storage-account-name>.blob.core.windows.net/<container-name>" --recursive` |
| **Ejemplo** | `azcopy cp "https://mysourceaccount.blob.core.windows.net/mycontainer" "https://mydestinationaccount.blob.core.windows.net/mycontainer" --recursive` |

### <a name="copy-all-containers-folders-and-files-to-another-storage-account"></a>Copiar todos los contenedores, carpetas y archivos en otra cuenta de almacenamiento

|    |     |
|--------|-----------|
| **Sintaxis** | `azcopy cp "https://<source-storage-account-name>.blob.core.windows.net/" "https://<destination-storage-account-name>.blob.core.windows.net/" --recursive"` |
| **Ejemplo** | `azcopy cp "https://mysourceaccount.blob.core.windows.net" "https://mydestinationaccount.blob.core.windows.net" --recursive` |

## <a name="synchronize-files"></a>Sincronizar archivos

Puede sincronizar el contenido de un sistema de archivos local a un contenedor de blobs. También puede sincronizar un contenedor de blobs en un sistema de archivos local en el equipo. La sincronización es unidireccional. En otras palabras, elegir cuál de estos dos puntos de conexión es el origen y cuál es el destino.

> [!NOTE]
> La versión actual de AzCopy no sincroniza entre otros orígenes y destinos (por ejemplo: Almacenamiento de archivos o depósitos de Amazon Web Services (AWS) S3).

El `sync` comando compara nombres de archivo y se modificó por última vez las marcas de tiempo. Establecer el `--delete-destination` marca opcional en un valor de `true` o `prompt` para eliminar archivos en la carpeta de destino si esos archivos ya no existen en la carpeta de origen.

Si establece la `--delete-destination` marca `true` AzCopy elimina los archivos sin tener que proporcionar un símbolo del sistema. Si desea un símbolo del sistema aparezca antes que AzCopy elimina un archivo, establezca la `--delete-destination` marca `prompt`.

> [!NOTE]
> Para evitar eliminaciones accidentales, asegúrese de habilitar la [eliminación temporal](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete) características antes de usar el `--delete-destination=prompt|true` marca.

### <a name="synchronize-a-container-to-a-local-file-system"></a>Sincronizar un contenedor para un sistema de archivos local

En este caso, el sistema de archivos local se convierte en el origen y el contenedor es el destino.

|    |     |
|--------|-----------|
| **Sintaxis** | `azcopy sync "<local-folder-path>" "https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>" --recursive` |
| **Ejemplo** | `azcopy sync "C:\myFolder" "https://mystorageaccount.blob.core.windows.net/mycontainer" --recursive` |
| **Ejemplo** (espacio de nombres jerárquico) | `azcopy sync "C:\myFolder" "https://<storage-account-name>.dfs.core.windows.net/mycontainer" --recursive` |


### <a name="synchronize-a-local-file-system-to-a-container"></a>Sincronizar un sistema de archivos local a un contenedor

En este caso, el contenedor se convierte en el origen y el sistema de archivos local es el destino.

|    |     |
|--------|-----------|
| **Sintaxis** | `azcopy sync "https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>" "C:\myFolder" --recursive` |
| **Ejemplo** | `azcopy sync "https://mystorageaccount.blob.core.windows.net/mycontainer" "C:\myFolder" --recursive` |
| **Ejemplo** (espacio de nombres jerárquico) | `azcopy sync "https://mystorageaccount.dfs.core.windows.net/mycontainer" "C:\myFolder" --recursive` |

## <a name="next-steps"></a>Pasos siguientes

Encontrar más ejemplos en cualquiera de estos artículos:

- [Empezar a trabajar con AzCopy](storage-use-azcopy-v10.md)

- [Tutorial: Migrar datos locales para almacenamiento en la nube mediante el uso de AzCopy](storage-use-azcopy-migrate-on-premises-data.md)

- [Transferencia de datos con AzCopy y file storage](storage-use-azcopy-files.md)

- [Transferencia de datos con AzCopy y Amazon S3 depósitos](storage-use-azcopy-s3.md)

- [Configurar, optimizar y solucionar problemas de AzCopy](storage-use-azcopy-configure.md)