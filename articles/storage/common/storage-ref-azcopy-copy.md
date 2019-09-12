---
title: azcopy copy | Microsoft Docs
description: En este artículo se proporciona información de referencia del comando azcopy copy.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 08/26/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: c15d188e333bea5e74fa65d2bbdf38ae7fadc246
ms.sourcegitcommit: 532335f703ac7f6e1d2cc1b155c69fc258816ede
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/30/2019
ms.locfileid: "70196746"
---
# <a name="azcopy-copy"></a>azcopy copy

Copia los datos de origen en una ubicación de destino.

## <a name="synopsis"></a>Sinopsis

Estas son las direcciones que se admiten:

- local <-> blob de Azure (autenticación con SAS u OAuth)
- local <-> archivo de Azure (autenticación de recursos compartidos o directorios con SAS)
- local <-> ADLS Gen 2 (autenticación con SAS, OAuth o SharedKey)
- Blob de Azure (SAS o público) <-> blob de Azure (autenticación con SAS u OAuth)
- Archivo de Azure (SAS) -> blob en bloques de Azure (autenticación con SAS u OAuth)
- AWS S3 (clave de acceso) -> blob en bloques de Azure (autenticación con SAS u OAuth)

Consulte los ejemplos para más información.

### <a name="advanced"></a>Avanzado

AzCopy detecta automáticamente el tipo de contenido de los archivos cuando se cargan desde el disco local, basándose en la extensión del archivo o en el contenido (si no se especifica ninguna extensión).

La tabla de búsqueda integrada es pequeña pero, en Unix, los archivos mime.types del sistema local la pueden aumentar si están disponibles en una o varias de estas ubicaciones:

- /etc/mime.types
- /etc/apache2/mime.types
- /etc/apache/mime.types

En Windows, los tipos MIME se extraen del registro. Esta característica se puede desactivar con la ayuda de una marca. Consulte la sección sobre marcas.

> [!IMPORTANT]
> Si establece una variable de entorno mediante la línea de comandos, esa variable se podrá leer en el historial de la línea de comandos. Considere la posibilidad de borrar las variables que contengan credenciales del historial de la línea de comandos. Para evitar que aparezcan variables en el historial, puede usar un script para pedir al usuario sus credenciales y establecer la variable de entorno.

```azcopy
azcopy copy [source] [destination] [flags]
```

## <a name="examples"></a>Ejemplos

Carga de un solo archivo mediante la autenticación de OAuth.

Si aún no ha iniciado sesión en AzCopy, use el comando `azcopy login` antes de ejecutar el comando siguiente.

```azcopy
azcopy cp "/path/to/file.txt" "https://[account].blob.core.windows.net/[container]/[path/to/blob]"
```

Igual que antes, pero en esta ocasión también procesa el hash MD5 del contenido del archivo y lo guarda como la propiedad Content-MD5 del blob:

```azcopy
azcopy cp "/path/to/file.txt" "https://[account].blob.core.windows.net/[container]/[path/to/blob]" --put-md5
```

Carga de un solo archivo con una firma de acceso compartido:

```azcopy
azcopy cp "/path/to/file.txt" "https://[account].blob.core.windows.net/[container]/[path/to/blob]?[SAS]"
```

Carga de un solo archivo con una firma de acceso compartido mediante canalización (solo blobs en bloques):

```azcopy
cat "/path/to/file.txt" | azcopy cp "https://[account].blob.core.windows.net/[container]/[path/to/blob]?[SAS]"
```

Carga de un directorio completo con una firma de acceso compartido:

```azcopy
azcopy cp "/path/to/dir" "https://[account].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" --recursive=true
```

o

```azcopy
azcopy cp "/path/to/dir" "https://[account].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" --recursive=true --put-md5
```

Carga de un conjunto de archivos con una firma de acceso compartido mediante caracteres comodín:

```azcopy
azcopy cp "/path/*foo/*bar/*.pdf" "https://[account].blob.core.windows.net/[container]/[path/to/directory]?[SAS]"
```

Carga de archivos y directorios con una firma de acceso compartido mediante caracteres comodín:

```azcopy
azcopy cp "/path/*foo/*bar*" "https://[account].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" --recursive=true
```

Descarga de un solo archivo mediante la autenticación de OAuth.

Si aún no ha iniciado sesión en AzCopy, use el comando `azcopy login` antes de ejecutar el comando siguiente.

```azcopy
azcopy cp "https://[account].blob.core.windows.net/[container]/[path/to/blob]" "/path/to/file.txt"
```

Descarga de un solo archivo con una firma de acceso compartido:

```azcopy
azcopy cp "https://[account].blob.core.windows.net/[container]/[path/to/blob]?[SAS]" "/path/to/file.txt"
```

Descarga de un solo archivo con una firma de acceso compartido mediante canalización (solo blobs en bloques):

```azcopy
azcopy cp "https://[account].blob.core.windows.net/[container]/[path/to/blob]?[SAS]" > "/path/to/file.txt"
```

Descarga de un directorio completo con una firma de acceso compartido:

```azcopy
azcopy cp "https://[account].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" "/path/to/dir" --recursive=true
```

Descarga de un conjunto de archivos con una firma de acceso compartido mediante caracteres comodín:

```azcopy
azcopy cp "https://[account].blob.core.windows.net/[container]/foo*?[SAS]" "/path/to/dir"
```

Descarga de archivos y directorios con una firma de acceso compartido mediante caracteres comodín:

```azcopy
azcopy cp "https://[account].blob.core.windows.net/[container]/foo*?[SAS]" "/path/to/dir" --recursive=true
```

Copia de un solo blob con SAS en otro blob con SAS:

```azcopy
azcopy cp "https://[srcaccount].blob.core.windows.net/[container]/[path/to/blob]?[SAS]" "https://[destaccount].blob.core.windows.net/[container]/[path/to/blob]?[SAS]"
```

Copia de un solo blob con SAS en otro blob con un token de OAuth.

Si aún no ha iniciado sesión en AzCopy, use el comando `azcopy login` antes de ejecutar el comando siguiente. El token de OAuth se usa para acceder a la cuenta de almacenamiento de destino.

```azcopy
azcopy cp "https://[srcaccount].blob.core.windows.net/[container]/[path/to/blob]?[SAS]" "https://[destaccount].blob.core.windows.net/[container]/[path/to/blob]"
```

Copia de un directorio completo del directorio virtual de blobs con SAS en otro directorio virtual de blobs con SAS:

```azcopy
azcopy cp "https://[srcaccount].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" "https://[destaccount].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" --recursive=true
```

Copia de todos los datos de cuenta de un blob con SAS en otra cuenta de blob con SAS:

```azcopy
azcopy cp "https://[srcaccount].blob.core.windows.net?[SAS]" "https://[destaccount].blob.core.windows.net?[SAS]" --recursive=true
```

Copia de un solo objeto desde S3 con clave de acceso a un blob con SAS:

Establezca la variable de entorno AWS_ACCESS_KEY_ID y AWS_SECRET_ACCESS_KEY para el origen S3.

```azcopy
azcopy cp "https://s3.amazonaws.com/[bucket]/[object]" "https://[destaccount].blob.core.windows.net/[container]/[path/to/blob]?[SAS]"
```

Copia de un directorio completo de S3 con clave de acceso en un directorio virtual de blobs con SAS:

```azcopy
azcopy cp "https://s3.amazonaws.com/[bucket]/[folder]" "https://[destaccount].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" --recursive=true
```

Consulte https://docs.aws.amazon.com/AmazonS3/latest/user-guide/using-folders.html para más información sobre lo que significa [folder] para S3. Establezca la variable de entorno AWS_ACCESS_KEY_ID y AWS_SECRET_ACCESS_KEY para el origen S3.

Copia de todos los cubos de un servicio S3 con clave de acceso en una cuenta de blobs con SAS:

Establezca la variable de entorno AWS_ACCESS_KEY_ID y AWS_SECRET_ACCESS_KEY para el origen S3.

```azcopy
azcopy cp "https://s3.amazonaws.com/" "https://[destaccount].blob.core.windows.net?[SAS]" --recursive=true
```

Copia de todos los cubos de una región S3 con clave de acceso en una cuenta de blobs con SAS:

```azcopy
azcopy cp "https://s3-[region].amazonaws.com/" "https://[destaccount].blob.core.windows.net?[SAS]" --recursive=true
```

Establezca la variable de entorno AWS_ACCESS_KEY_ID y AWS_SECRET_ACCESS_KEY para el origen S3.

## <a name="options"></a>Opciones

|Opción|DESCRIPCIÓN|
|--|--|
|--blob-type string|Define el tipo de blob en el destino. Se usa para cargar blobs y al copiar entre cuentas (el valor predeterminado es "none").|
|--block-blob-tier string|Cargue un blob en bloques en Azure Storage con este nivel de blobs. (valor predeterminado: "None").|
|--block-size-mb float |Use este tamaño de bloque (especificado en MiB) al cargar y descargar de Azure Storage. El valor predeterminado se calcula automáticamente en función del tamaño del archivo. Se permiten fracciones decimales (por ejemplo: 0,25).|
|--cache-control string|Establece el encabezado cache-control. Devuelto al descargar.|
|--check-md5 string|Especifica cómo de estrictamente se deben validar los hashes MD5 al descargarse. Solo está disponible al descargar. Opciones disponibles: NoCheck, LogOnly, FailIfDifferent, FailIfDifferentOrMissing. (el valor predeterminado es "FailIfDifferent")|
|--content-disposition string|Establece el encabezado content-disposition. Devuelto al descargar.|
|--content-encoding string|Establece el encabezado content-encoding. Devuelto al descargar.|
|--content-language string|Establece el encabezado content-language. Devuelto al descargar.|
|--content-type string |Especifica el tipo de contenido del archivo. Implica el uso de no-guess-mime-type. Devuelto al descargar.|
|--exclude string|Excluye estos archivos al copiar. Admite el uso de *.|
|--exclude-blob-type string|Opcionalmente, especifica el tipo de blob (BlockBlob/PageBlob/AppendBlob) que se va a excluir al copiar blobs desde el contenedor o la cuenta. El uso de esta marca no es aplicable para copiar datos desde un servicio que no es de Azure a otro que sí lo es. Si hay más de un blob deberían separarse mediante ";".|
|--follow-symlinks|Sigue los vínculos simbólicos al cargar desde el sistema de archivos local.|
|--from-to string|Opcionalmente, especifica la combinación de destino de origen. Por ejemplo: LocalBlob, BlobLocal, LocalBlobFS.|
|-h, --help|Muestra el contenido de la ayuda para el comando copy. |
|--log-level string|Define el nivel de detalle del registro para el archivo de registro. Niveles disponibles: INFO (todas las solicitudes y respuestas), WARNING (respuestas lentas), ERROR (solo solicitudes con error) y NONE (sin registros de salida). (Valor predeterminado: "INFO").|
|--metadata string|Carga en Azure Storage con estos pares clave-valor como metadatos.|
|--no-guess-mime-type|Impide que AzCopy detecte el tipo de contenido en función de la extensión o el contenido del archivo.|
|--overwrite|Sobrescribe los archivos o blobs en conflicto en el destino si esta marca está establecida en true. (El valor predeterminado es true).|
|--page-blob-tier string |Carga un blob en páginas en Azure Storage con este nivel de blobs. (valor predeterminado: "None").|
|--preserve-last-modified-time|Solo está disponible si el destino es un sistema de archivos.|
|--put-md5|Crea un hash MD5 de cada archivo y lo guarda como la propiedad Content-MD5 del blob o archivo de destino. (De forma predeterminada, NO se crea el hash). Solo está disponible al cargar.|
|--recursive|Busca en subdirectorios de forma recursiva al cargar desde el sistema de archivos local.|
|--s2s-detect-source-changed|Comprueba si el origen ha cambiado después de la enumeración. Para la copia S2S, como el origen es un recurso remoto, la validación de si el origen ha cambiado necesita costos de solicitud adicionales.|
|--s2s-handle-invalid-metadata string |Especifica cómo se administran las claves de metadatos no válidas. Opciones disponibles: ExcludeIfInvalid, FailIfInvalid, RenameIfInvalid. (El valor predeterminado es "ExcludeIfInvalid").|
|--s2s-preserve-access-tier|Conserva el nivel de acceso durante la copia de servicio a servicio. Consulte [Azure Blob Storage: niveles de acceso frecuente, esporádico y de archivo](../blobs/storage-blob-storage-tiers.md) para asegurarse de que la cuenta de almacenamiento de destino admite la configuración del nivel de acceso. En los casos en los que no se admite el establecimiento del nivel de acceso, use s2sPreserveAccessTier=false para omitir la copia del nivel de acceso.  (El valor predeterminado es true).|
|--s2s-preserve-properties|Conserva las propiedades completas durante la copia de servicio a servicio. En el caso de un origen de archivo no único de archivos S3 y de Azure, como la operación de lista no devuelve propiedades completas de objetos y archivos, para conservar las propiedades completas AzCopy debe enviar una solicitud adicional por objeto y archivo. (El valor predeterminado es true).|

## <a name="options-inherited-from-parent-commands"></a>Opciones heredadas de comandos primarios

|Opción|DESCRIPCIÓN|
|---|---|
|--cap-mbps uint32|Limita la velocidad de transferencia, en megabits por segundo. El rendimiento en un momento dado puede variar ligeramente del límite. Si esta opción se establece en cero o se omite, el rendimiento no se limita.|
|--output-type string|Formato de la salida del comando. Las opciones incluyen: text, json. El valor predeterminado es "text".|

## <a name="see-also"></a>Otras referencias

- [azcopy](storage-ref-azcopy.md)
