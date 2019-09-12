---
title: azcopy list | Microsoft Docs
description: En este artículo se proporciona información de referencia del comando azcopy list.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 08/26/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: 8c40241c49917c6db6663f346aed0ec5d3b96be7
ms.sourcegitcommit: 532335f703ac7f6e1d2cc1b155c69fc258816ede
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/30/2019
ms.locfileid: "70196810"
---
# <a name="azcopy-list"></a>azcopy list

Enumera las entidades de un recurso determinado.

## <a name="synopsis"></a>Sinopsis

Solo se admiten contenedores de blobs en la versión actual.

```azcopy
azcopy list [containerURL] [flags]
```

## <a name="examples"></a>Ejemplos

```azcopy
azcopy list [containerURL]
```

## <a name="options"></a>Opciones

|Opción|DESCRIPCIÓN|
|--|--|
|-h, --help|Muestra el contenido de la ayuda para el comando list.|
|--machine-readable|Muestra los tamaños de archivo en bytes.|
|--mega-units|Muestra las unidades en grupos de 1000, no de 1024.|
|--running-tally|Cuenta el número total de archivos y sus tamaños.|

## <a name="options-inherited-from-parent-commands"></a>Opciones heredadas de comandos primarios

|Opción|DESCRIPCIÓN|
|---|---|
|--cap-mbps uint32|Limita la velocidad de transferencia, en megabits por segundo. El rendimiento en un momento dado puede variar ligeramente del límite. Si esta opción se establece en cero o se omite, el rendimiento no se limita.|
|--output-type string|Formato de la salida del comando. Las opciones incluyen: text, json. El valor predeterminado es "text".|

## <a name="see-also"></a>Otras referencias

- [azcopy](storage-ref-azcopy.md)
