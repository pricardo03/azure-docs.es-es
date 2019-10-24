---
title: azcopy jobs remove | Microsoft Docs
description: En este artículo se proporciona información de referencia del comando azcopy jobs remove.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 10/16/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: 82c399580322334e67c0c9c2b88d1edf6f175e0c
ms.sourcegitcommit: 12de9c927bc63868168056c39ccaa16d44cdc646
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2019
ms.locfileid: "72518124"
---
# <a name="azcopy-jobs-remove"></a>azcopy jobs remove

Quita todos los archivos asociados al identificador de trabajo especificado.

> [!NOTE] 
> Puede personalizar la ubicación donde se guardan los archivos de registro y de plan. Consulte el comando [azcopy env](storage-ref-azcopy-env.md) para obtener más información.

```
azcopy jobs remove [jobID] [flags]
```

## <a name="examples"></a>Ejemplos

```
  azcopy jobs rm e52247de-0323-b14d-4cc8-76e0be2e2d44
```

## <a name="options"></a>Opciones

**-h, --help**                     Ayuda de remove.

## <a name="options-inherited-from-parent-commands"></a>Opciones heredadas de comandos primarios

**--cap-mbps uint32**      Limita la velocidad de transferencia, en megabits por segundo. El rendimiento en un momento dado puede variar ligeramente del límite. Si esta opción se establece en cero o se omite, el rendimiento no se limita.

**--output-type** string   Formato de la salida del comando. Las opciones incluyen: text, json. El valor predeterminado es "text". (Valor predeterminado: "text").

## <a name="see-also"></a>Otras referencias

- [azcopy jobs](storage-ref-azcopy-jobs.md)
