---
title: azcopy make | Microsoft Docs
description: En este artículo se proporciona información de referencia del comando azcopy make.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 08/26/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: 9afcd8de1af42424649dd8e44fc07f7bfd881257
ms.sourcegitcommit: 532335f703ac7f6e1d2cc1b155c69fc258816ede
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/30/2019
ms.locfileid: "70196738"
---
# <a name="azcopy-make"></a>azcopy make

Crea un contenedor o un recurso compartido de archivos.

## <a name="synopsis"></a>Sinopsis

Crea un contenedor o recurso compartido de archivos representado por la dirección URL del recurso especificado.

```azcopy
azcopy make [resourceURL] [flags]
```

## <a name="examples"></a>Ejemplos

```azcopy
azcopy make "https://[account-name].[blob,file,dfs].core.windows.net/[top-level-resource-name]"
```

## <a name="options"></a>Opciones

|Opción|DESCRIPCIÓN|
|--|--|
|-h, --help|Muestra el contenido de la ayuda para el comando make. |
|--quota-gb uint32|Especifica el tamaño máximo del recurso compartido en gigabytes (GiB), 0 significa que acepta la cuota predeterminada del servicio de archivos.|

## <a name="options-inherited-from-parent-commands"></a>Opciones heredadas de comandos primarios

|Opción|DESCRIPCIÓN|
|---|---|
|--cap-mbps uint32|Limita la velocidad de transferencia, en megabits por segundo. El rendimiento en un momento dado puede variar ligeramente del límite. Si esta opción se establece en cero o se omite, el rendimiento no se limita.|
|--output-type string|Formato de la salida del comando. Las opciones incluyen: text, json. El valor predeterminado es "text".|

## <a name="see-also"></a>Otras referencias

- [azcopy](storage-ref-azcopy.md)
