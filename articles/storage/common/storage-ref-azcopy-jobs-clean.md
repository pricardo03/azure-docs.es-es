---
title: azcopy jobs clean | Microsoft Docs
description: En este artículo se proporciona información de referencia sobre el comando azcopy jobs clean.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 10/16/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: 31529155ee44b2bcfad90e8634053403dfe8fc8c
ms.sourcegitcommit: 12de9c927bc63868168056c39ccaa16d44cdc646
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2019
ms.locfileid: "72518192"
---
# <a name="azcopy-jobs-clean"></a>azcopy jobs clean

Eliminación de todos los archivos de registro y plan de todos los trabajos

```
azcopy jobs clean [flags]
```

## <a name="examples"></a>Ejemplos

```
  azcopy jobs clean --with-status=completed
```

## <a name="options"></a>Opciones

**-h, --help**                Ayuda de clean.

**--with-status** string   Solo quita los trabajos con este estado. Valores disponibles: Canceled, Completed, Failed, InProgress, All (el valor predeterminado es "All")

## <a name="options-inherited-from-parent-commands"></a>Opciones heredadas de comandos primarios

**--cap-mbps uint32**      Limita la velocidad de transferencia, en megabits por segundo. El rendimiento en un momento dado puede variar ligeramente del límite. Si esta opción se establece en cero o se omite, el rendimiento no se limita.

**--output-type** string   Da formato a la salida del comando. Las opciones incluyen: text, json. El valor predeterminado es "text". (el valor predeterminado es "text")

## <a name="see-also"></a>Otras referencias

- [azcopy jobs](storage-ref-azcopy-jobs.md)
