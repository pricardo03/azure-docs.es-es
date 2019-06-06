---
title: Transferir datos a Azure Storage de depósitos de Amazon S3 mediante AzCopy v10 | Microsoft Docs
description: Transferencia de datos con AzCopy y Amazon S3 depósitos
services: storage
author: normesta
ms.service: storage
ms.topic: article
ms.date: 04/23/2019
ms.author: normesta
ms.subservice: common
ms.openlocfilehash: b18c4c039b615c7c88268b6e668df9f7fec9fabf
ms.sourcegitcommit: 6932af4f4222786476fdf62e1e0bf09295d723a1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/05/2019
ms.locfileid: "66687911"
---
# <a name="copy-data-from-amazon-s3-buckets-by-using-azcopy"></a>Copiar datos desde Amazon S3 depósitos mediante AzCopy

AzCopy es una utilidad de línea de comandos que puede usar para copiar los blobs o archivos a o desde una cuenta de almacenamiento. En este artículo le ayuda a copiar los depósitos, directorios y objetos de S3 de Amazon Web Services (AWS) a Azure blob storage mediante AzCopy.

## <a name="choose-how-youll-provide-authorization-credentials"></a>Elegir cómo deberá proporcionar las credenciales de autorización

* Para autorizar con el almacenamiento de Azure, use Azure Active Directory (AD) o un token de firma de acceso compartido (SAS).

* Para autorizar con AWS S3, usar una clave de acceso AWS y una clave de acceso secreta.

### <a name="authorize-with-azure-storage"></a>Autorizar con almacenamiento de Azure

Consulte la [empezar a trabajar con AzCopy](storage-use-azcopy-v10.md) artículo para descargar AzCopy y elegir cómo deberá proporcionar las credenciales de autorización para el servicio de almacenamiento.

> [!NOTE]
> Los ejemplos en este artículo se supone que ha autenticado su identidad mediante el uso de la `AzCopy login` comando. AzCopy, a continuación, utiliza la cuenta de Azure AD para autorizar el acceso a datos en Blob storage.
>
> Si prefiere utilizar un token de SAS para autorizar el acceso a los datos blob, puede anexar ese token a la dirección URL de recursos en cada comando de AzCopy.
>
> Por ejemplo: `https://mystorageaccount.blob.core.windows.net/mycontainer?<SAS-token>`.

### <a name="authorize-with-aws-s3"></a>Autorizar con AWS S3

Recopile la clave de acceso AWS y la clave de acceso secreta y, a continuación, establezca estas variables de entorno:

| Sistema operativo | Get-Help  |
|--------|-----------|
| **Windows** | `set AWS_ACCESS_KEY_ID=<access-key>`<br>`set AWS_SECRET_ACCESS_KEY=<secret-access-key>` |
| **Linux** | `export AWS_ACCESS_KEY_ID=<access-key>`<br>`export AWS_SECRET_ACCESS_KEY=<secret-access-key>` |
| **macOS** | `export AWS_ACCESS_KEY_ID=<access-key>`<br>`export AWS_SECRET_ACCESS_KEY=<secret-access-key>`|

## <a name="copy-objects-directories-and-buckets"></a>Copiar objetos de directorios y cubos

AzCopy usa el [colocar bloque From URL](https://docs.microsoft.com/rest/api/storageservices/put-block-from-url) API, por lo que se copian datos directamente entre AWS S3 y los servidores de almacenamiento. Estas operaciones de copia no utilizan el ancho de banda de red del equipo.

### <a name="copy-an-object"></a>Copiar un objeto

|    |     |
|--------|-----------|
| **Sintaxis** | `azcopy cp "https://s3.amazonaws.com/<bucket-name>/<object-name>" "https://<storage-account-name>.blob.core.windows.net/<container-name>/<blob-name>"` |
| **Ejemplo** | `azcopy cp "https://s3.amazonaws.com/mybucket/myobject" "https://mystorageaccount.blob.core.windows.net/mycontainer/myblob"` |

> [!NOTE]
> Ejemplos de este artículo utilizan direcciones URL de estilo de ruta de acceso de los depósitos de AWS S3 (por ejemplo: `http://s3.amazonaws.com/<bucket-name>`). 
>
> También puede usar virtuales direcciones URL de estilo hospedados (por ejemplo: `http://bucket.s3.amazonaws.com`). 
>
> Para obtener más información acerca de hospedaje virtual de depósitos, consulte [Virtual hospeda de cubos]] (https://docs.aws.amazon.com/AmazonS3/latest/dev/VirtualHosting.html).

### <a name="copy-a-directory"></a>Copiar un directorio

|    |     |
|--------|-----------|
| **Sintaxis** | `azcopy cp "https://s3.amazonaws.com/<bucket-name>/<directory-name>" "https://<storage-account-name>.blob.core.windows.net/<container-name>/<directory-name>" --recursive=true` |
| **Ejemplo** | `azcopy cp "https://s3.amazonaws.com/mybucket/mydirectory" "https://mystorageaccount.blob.core.windows.net/mycontainer/mydirectory" --recursive=true` |

### <a name="copy-a-bucket"></a>Copiar un depósito

|    |     |
|--------|-----------|
| **Sintaxis** | `azcopy cp "https://s3.amazonaws.com/<bucket-name>" "https://<storage-account-name>.blob.core.windows.net/<container-name>" --recursive=true` |
| **Ejemplo** | `azcopy cp "https://s3.amazonaws.com/mybucket" "https://mystorageaccount.blob.core.windows.net/mycontainer" --recursive=true` |

### <a name="copy-all-buckets-in-all-regions"></a>Copiar todos los cubos en todas las regiones

|    |     |
|--------|-----------|
| **Sintaxis** | `azcopy cp "https://s3.amazonaws.com/" "https://<storage-account-name>.blob.core.windows.net" --recursive=true` |
| **Ejemplo** | `azcopy cp "https://s3.amazonaws.com" "https://mystorageaccount.blob.core.windows.net" --recursive=true` |

### <a name="copy-all-buckets-in-a-specific-s3-region"></a>Copiar todos los cubos en una región específica de S3

|    |     |
|--------|-----------|
| **Sintaxis** | `azcopy cp "https://s3-<region-name>.amazonaws.com/" "https://<storage-account-name>.blob.core.windows.net" --recursive=true` |
| **Ejemplo** | `azcopy cp "https://s3-rds.eu-north-1.amazonaws.com" "https://mystorageaccount.blob.core.windows.net" --recursive=true` |

## <a name="handle-differences-in-object-naming-rules"></a>Controlar las diferencias en las reglas de nomenclatura de objetos

AWS S3 tiene un conjunto diferente de convenciones de nomenclatura para los nombres de depósito en comparación con los contenedores de blobs de Azure. Puede leer sobre ellos [aquí](https://docs.aws.amazon.com/AmazonS3/latest/dev/BucketRestrictions.html#bucketnamingrules). Si elige copiar un grupo de depósitos a una cuenta de almacenamiento de Azure, la operación de copia puede producir un error debido a diferencias de nombre.

AzCopy controla dos de los problemas más comunes que pueden surgir; cubos que contienen puntos y cubos que puede contener guiones consecutivos. AWS S3 bucket nombres pueden contener puntos y guiones consecutivos, pero no de un contenedor en Azure. AzCopy reemplaza a los puntos con guiones y guiones consecutivos con un número que representa el número de guiones consecutivos (por ejemplo: un depósito denominado `my----bucket` se convierte en `my-4-bucket`. 

Además, como AzCopy copia archivos, comprueba si hay conflictos de nombres e intenta resolverlos. Por ejemplo, si hay depósitos con el nombre `bucket-name` y `bucket.name`, AzCopy se resuelve como un cubo denominado `bucket.name` primero `bucket-name` y, a continuación, en `bucket-name-2`.

## <a name="handle-differences-in-object-metadata"></a>Controlar las diferencias en los metadatos del objeto

AWS S3 y Azure permiten distintos conjuntos de caracteres en los nombres de las claves de objeto. Puede leer acerca de los caracteres que usa AWS S3 [aquí](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingMetadata.html#object-keys). En el lado de Azure, las claves de objeto de blob se adhieren a las reglas de nomenclatura [ C# identificadores](https://docs.microsoft.com/dotnet/csharp/language-reference/).

Como parte de un AzCopy `copy` de comandos, puede proporcionar un valor para opcional el `s2s-invalid-metadata-handle` marca que especifica cómo desea controlar los archivos donde los metadatos del archivo contienen los nombres de clave no compatibles. La tabla siguiente describe cada valor de la marca.

| Valor de marca | DESCRIPCIÓN  |
|--------|-----------|
| **ExcludeIfInvalid** | (Opción predeterminada) Los metadatos no se incluyen en el objeto transferido. AzCopy registra una advertencia. |
| **FailIfInvalid** | No se copian los objetos. AzCopy registra un error e incluye ese error en el número de errores que aparece en el resumen de la transferencia.  |
| **RenameIfInvalid**  | AzCopy se resuelve como la clave de metadatos no válidos y copia el objeto en Azure mediante el par clave-valor de metadatos resuelto. Para saber exactamente qué pasos AzCopy que tarda en cambiar el nombre de las claves de objeto, consulte el [AzCopy cómo cambia el nombre de las claves de objeto](#rename-logic) sección más adelante. Si AzCopy no se puede cambiar el nombre de la clave, el objeto no se copiarán. |

<a id="rename-logic" />

### <a name="how-azcopy-renames-object-keys"></a>Cómo AzCopy cambia el nombre de las claves de objeto

AzCopy lleva a cabo estos pasos:

1. Reemplaza los caracteres no válidos con '_'.

2. Agrega la cadena `rename_` al principio de una nueva clave válida.

   Esta clave se utilizará para guardar los metadatos originales **valor**.

3. Agrega la cadena `rename_key_` al principio de una nueva clave válida.
   Esta clave se utilizará para guardar los metadatos originales no válido **clave**.
   Puede usar esta clave para intentar recuperar los metadatos en el lado de Azure, ya que la clave de metadatos se conserva como un valor en el servicio de almacenamiento de blobs.

## <a name="next-steps"></a>Pasos siguientes

Encontrar más ejemplos en cualquiera de estos artículos:

- [Introducción a AzCopy](storage-use-azcopy-v10.md)

- [Transferencia de datos con AzCopy y Blob Storage](storage-use-azcopy-blobs.md)

- [Transferencia de datos con AzCopy y File Storage](storage-use-azcopy-files.md)

- [Configurar, optimizar y solucionar problemas de AzCopy](storage-use-azcopy-configure.md)