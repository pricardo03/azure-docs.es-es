---
title: azcopy jobs list | Microsoft Docs
description: En este artículo se proporciona información de referencia del comando azcopy jobs list.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 10/16/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: a37735fcfec7909e37177140116ebcf7de8a298e
ms.sourcegitcommit: 12de9c927bc63868168056c39ccaa16d44cdc646
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2019
ms.locfileid: "72513673"
---
# <a name="azcopy-jobs-list"></a>azcopy jobs list

Muestra información sobre todos los trabajos.

## <a name="synopsis"></a>Sinopsis

```azcopy
azcopy jobs list [flags]
```

## <a name="options"></a>Opciones

|Opción|DESCRIPCIÓN|
|--|--|
|-h, --help|Muestra el contenido de la ayuda para el comando list.|

## <a name="options-inherited-from-parent-commands"></a>Opciones heredadas de comandos primarios

|Opción|DESCRIPCIÓN|
|---|---|
|--cap-mbps uint32|Limita la velocidad de transferencia, en megabits por segundo. El rendimiento en un momento dado puede variar ligeramente del límite. Si esta opción se establece en cero o se omite, el rendimiento no se limita.|
|--output-type string|Formato de la salida del comando. Las opciones incluyen: text, json. El valor predeterminado es "text".|

## <a name="see-also"></a>Otras referencias

- [azcopy jobs](storage-ref-azcopy-jobs.md)
