---
title: azcopy doc | Microsoft Docs
description: En este artículo se proporciona información de referencia del comando azcopy doc.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 10/16/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: b11d831be847716d4edfac4f9a0725e07a60c52a
ms.sourcegitcommit: 12de9c927bc63868168056c39ccaa16d44cdc646
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2019
ms.locfileid: "72513856"
---
# <a name="azcopy-doc"></a>azcopy doc

Genera documentación para la herramienta en formato de Markdown.

## <a name="synopsis"></a>Sinopsis

Genera documentación para la herramienta en formato Markdown y la almacena en la ubicación designada.

De forma predeterminada, los archivos se almacenan en una carpeta denominada "doc" en el directorio actual.

```azcopy
azcopy doc [flags]
```

## <a name="options"></a>Opciones

|Opción|DESCRIPCIÓN|
|--|--|
|-h, --help|Muestra el contenido de la ayuda para el comando doc.|

## <a name="options-inherited-from-parent-commands"></a>Opciones heredadas de comandos primarios

|Opción|DESCRIPCIÓN|
|---|---|
|--cap-mbps uint32|Limita la velocidad de transferencia, en megabits por segundo. El rendimiento en un momento dado puede variar ligeramente del límite. Si esta opción se establece en cero o se omite, el rendimiento no se limita.|
|--output-type string|Formato de la salida del comando. Las opciones incluyen: text, json. El valor predeterminado es "text".|

## <a name="see-also"></a>Otras referencias

- [azcopy](storage-ref-azcopy.md)
