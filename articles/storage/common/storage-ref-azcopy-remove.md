---
title: azcopy remove | Microsoft Docs
description: En este artículo se proporciona información de referencia del comando azcopy remove.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 08/26/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: 0cc366ab2cdad9c7258dca905d8f4a06472119fe
ms.sourcegitcommit: 532335f703ac7f6e1d2cc1b155c69fc258816ede
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/30/2019
ms.locfileid: "70196798"
---
# <a name="azcopy-remove"></a>azcopy remove

Elimina entidades de Azure Storage Blob, Azure Files y Azure Data Lake Storage Gen2.

## <a name="synopsis"></a>Sinopsis

```azcopy
azcopy remove [resourceURL] [flags]
```

## <a name="examples"></a>Ejemplos

Elimine un solo blob con SAS:

```azcopy
azcopy rm "https://[account].blob.core.windows.net/[container]/[path/to/blob]?[SAS]"
```

Elimine un directorio virtual completo con una SAS:

```azcopy
azcopy rm "https://[account].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" --recursive=true
```

Elimine solo los blobs principales de un directorio virtual, pero no sus subdirectorios:

```azcopy
azcopy rm "https://[account].blob.core.windows.net/[container]/[path/to/virtual/dir]" --recursive=false
```

Elimine un subconjunto de blobs de un directorio virtual (por ejemplo, solo los archivos jpg y pdf, o un blob llamado "exactName"):

```azcopy
azcopy rm "https://[account].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" --recursive=true --include="*.jpg;*.pdf;exactName"
```

Elimine un directorio completo, pero con la exclusión de ciertos blobs del ámbito (por ejemplo: todos los blobs que comienzan por "foo" o terminan por "bar"):

```azcopy
azcopy rm "https://[account].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" --recursive=true --exclude="foo*;*bar"
```

Elimine un solo archivo de Data Lake Storage Gen2 (no se admiten las operaciones de inclusión y exclusión):

```azcopy
azcopy rm "https://[account].dfs.core.windows.net/[container]/[path/to/file]?[SAS]"
```

Elimine un solo directorio de Data Lake Storage Gen2 (no se admiten las operaciones de inclusión y exclusión):

```azcopy
azcopy rm "https://[account].dfs.core.windows.net/[container]/[path/to/directory]?[SAS]"
```

## <a name="options"></a>Opciones

|Opción|DESCRIPCIÓN|
|--|--|
|--exclude string|Excluye los archivos en los que el nombre coincide con la lista de patrones. Por ejemplo: *.jpg;* .pdf; exactName.|
|-h, --help|Muestra el contenido de la ayuda para el comando remove.|
|--include string|Solo incluye los archivos en los que el nombre coincide con la lista de patrones. Por ejemplo: *.jpg;* .pdf; exactName.|
|--log-level string|Define el nivel de detalle del registro para el archivo de registro. Los niveles disponibles incluyen: INFO (todas las solicitudes y respuestas), WARNING (respuestas lentas), ERROR (solo solicitudes con error) y NONE (sin registros de salida). (Valor predeterminado: "INFO").|
|--recursive|Busca en los subdirectorios de forma recursiva al sincronizar entre directorios.|

## <a name="options-inherited-from-parent-commands"></a>Opciones heredadas de comandos primarios

|Opción|DESCRIPCIÓN|
|---|---|
|--cap-mbps uint32|Limita la velocidad de transferencia, en megabits por segundo. El rendimiento en un momento dado puede variar ligeramente del límite. Si esta opción se establece en cero o se omite, el rendimiento no se limita.|
|--output-type string|Formato de la salida del comando. Las opciones incluyen: text, json. El valor predeterminado es "text".|

## <a name="see-also"></a>Otras referencias

- [azcopy](storage-ref-azcopy.md)
