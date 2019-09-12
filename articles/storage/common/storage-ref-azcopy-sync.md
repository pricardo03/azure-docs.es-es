---
title: azcopy sync | Microsoft Docs
description: En este artículo se proporciona información de referencia del comando azcopy sync.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 08/26/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: fb6c3b711a89ae7e4ef403a75927c4c6172523d0
ms.sourcegitcommit: 532335f703ac7f6e1d2cc1b155c69fc258816ede
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/30/2019
ms.locfileid: "70196754"
---
# <a name="azcopy-sync"></a>azcopy sync

Replica la ubicación de origen en la ubicación de destino.

## <a name="synopsis"></a>Sinopsis

Las horas de última modificación se utilizan con fines de comparación. El archivo se omite si la hora de la última modificación del destino es más reciente.

Los pares admitidos son:

- local <-> blob de Azure (se puede emplear la autenticación con SAS u OAuth)

El comando sync difiere del comando copy de varias maneras:

  1. La marca recursiva está activada de forma predeterminada.
  2. El origen y el destino no deben contener patrones (como "*" o "?").
  3. Las marcas de inclusión y exclusión pueden ser una lista de patrones que coinciden con nombres de archivos. Consulte la sección de ejemplos para más información.
  4. Si hay archivos o blobs en el destino que no están presentes en el origen, se solicitará al usuario que los elimine.

     Este mensaje se puede silenciar mediante las marcas correspondientes para responder automáticamente a la petición de eliminación.

### <a name="advanced"></a>Avanzado

AzCopy detecta automáticamente el tipo de contenido de los archivos cuando se cargan desde el disco local, basándose en la extensión del archivo o en el contenido (si no se especifica ninguna extensión).

La tabla de búsqueda integrada es pequeña pero, en Unix, los archivos mime.types del sistema local la pueden aumentar si están disponibles en una o varias de estas ubicaciones:

- /etc/mime.types
- /etc/apache2/mime.types
- /etc/apache/mime.types

En Windows, los tipos MIME se extraen del registro.

```azcopy
azcopy sync [flags]
```

## <a name="examples"></a>Ejemplos

Sincronización de un solo archivo:

```azcopy
azcopy sync "/path/to/file.txt" "https://[account].blob.core.windows.net/[container]/[path/to/blob]"
```

Igual que antes, pero en esta ocasión también procesa el hash MD5 del contenido del archivo y lo guarda como la propiedad Content-MD5 del blob:

```azcopy
azcopy sync "/path/to/file.txt" "https://[account].blob.core.windows.net/[container]/[path/to/blob]" --put-md5
```

Sincronización de un directorio completo incluyendo los subdirectorios (tenga en cuenta que la recursividad está activada de forma predeterminada):

```azcopy
azcopy sync "/path/to/dir" "https://[account].blob.core.windows.net/[container]/[path/to/virtual/dir]"
```

o

```azcopy
azcopy sync "/path/to/dir" "https://[account].blob.core.windows.net/[container]/[path/to/virtual/dir]" --put-md5
```

Sincronización solo de los archivos principales de un directorio, pero no de sus subdirectorios:

```azcopy
azcopy sync "/path/to/dir" "https://[account].blob.core.windows.net/[container]/[path/to/virtual/dir]" --recursive=false
```

Sincronización de un subconjunto de archivos de un directorio (por ejemplo, solo los archivos jpg y pdf, o un archivo llamado "exactName"):

```azcopy
azcopy sync "/path/to/dir" "https://[account].blob.core.windows.net/[container]/[path/to/virtual/dir]" --include="*.jpg;*.pdf;exactName"
```

Sincronización de un directorio completo, pero con la exclusión de ciertos archivos del ámbito (por ejemplo: todos los archivos que comienzan por "foo" o terminan por "bar"):

```azcopy
azcopy sync "/path/to/dir" "https://[account].blob.core.windows.net/[container]/[path/to/virtual/dir]" --exclude="foo*;*bar"
```

> [!NOTE]
> Si se usan juntas las marcas de inclusión y exclusión, solo los archivos que coincidan con los patrones de inclusión se examinarán, pero los que coincidan con los patrones de exclusión se ignorarán siempre.

## <a name="options"></a>Opciones

|Opción|DESCRIPCIÓN|
|--|--|
|--block-size-mb float|Use este tamaño de bloque (especificado en MiB) al cargar o descargar de Azure Storage. El valor predeterminado se calcula automáticamente en función del tamaño del archivo. Se permiten fracciones decimales (por ejemplo: 0,25).|
|--check-md5 string|Especifica cómo de estrictamente se deben validar los hashes MD5 al descargarse. Esta opción solo está disponible al descargar. Los valores disponibles son: NoCheck, LogOnly, FailIfDifferent, FailIfDifferentOrMissing. (el valor predeterminado es "FailIfDifferent").|
|--delete-destination string|Define si se eliminan los archivos adicionales del destino que no están presentes en el origen. Se puede establecer en true, false o prompt. Si se establece en prompt, se le formulará al usuario una pregunta antes de programar archivos y blobs para su eliminación. (El valor predeterminado es false).|
|--exclude string|Excluye los archivos en los que el nombre coincide con la lista de patrones. Por ejemplo: *.jpg;* .pdf; exactName.|
|-h, --help|Muestra el contenido de la ayuda para el comando sync.|
|--include string|Incluye solo los archivos en los que el nombre coincide con la lista de patrones. Por ejemplo: *.jpg;* .pdf; exactName.|
|--log-level string|Define el nivel de detalle del registro para el archivo de registro. Niveles disponibles: INFO (todas las solicitudes y respuestas), WARNING (respuestas lentas), ERROR (solo solicitudes con error) y NONE (sin registros de salida). (Valor predeterminado: "INFO").|
|--put-md5|Crea un hash MD5 de cada archivo y lo guarda como la propiedad Content-MD5 del blob o archivo de destino. (De forma predeterminada, NO se crea el hash). Solo está disponible al cargar.|
|--recursive|"True" de forma predeterminada, busca en los subdirectorios de forma recursiva al sincronizar entre directorios. (El valor predeterminado es true).|

## <a name="options-inherited-from-parent-commands"></a>Opciones heredadas de comandos primarios

|Opción|DESCRIPCIÓN|
|---|---|
|--cap-mbps uint32|Limita la velocidad de transferencia, en megabits por segundo. El rendimiento en un momento dado puede variar ligeramente del límite. Si esta opción se establece en cero o se omite, el rendimiento no se limita.|
|--output-type string|Formato de la salida del comando. Las opciones incluyen: text, json. El valor predeterminado es "text".|

## <a name="see-also"></a>Otras referencias

- [azcopy](storage-ref-azcopy.md)
