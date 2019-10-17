---
title: azcopy jobs resume | Microsoft Docs
description: En este artículo se proporciona información de referencia del comando azcopy jobs resume.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 10/16/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: 9e8dbbd40259c7a71f252d0d6e93dd6f135973de
ms.sourcegitcommit: 12de9c927bc63868168056c39ccaa16d44cdc646
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2019
ms.locfileid: "72513545"
---
# <a name="azcopy-jobs-resume"></a>azcopy jobs resume

Reanuda el trabajo existente con el identificador de trabajo especificado.

## <a name="synopsis"></a>Sinopsis

```azcopy
azcopy jobs resume [jobID] [flags]
```

## <a name="options"></a>Opciones

|Opción|DESCRIPCIÓN|
|--|--|
|--destination-sas string|SAS de destino del destino para un identificador de trabajo determinado.|
|--exclude string|Filtro: excluye estas transferencias con error al reanudar el trabajo. Los archivos deben estar separados por ";".|
|-h, --help|Muestra el contenido de la ayuda para el comando resume.|
|--include string|Filtro: solo excluye estas transferencias con error al reanudar el trabajo. Los archivos deben estar separados por ";".|
|--source-sas string |SAS de origen del origen para un identificador de trabajo determinado.|

## <a name="options-inherited-from-parent-commands"></a>Opciones heredadas de comandos primarios

|Opción|DESCRIPCIÓN|
|---|---|
|--cap-mbps uint32|Limita la velocidad de transferencia, en megabits por segundo. El rendimiento en un momento dado puede variar ligeramente del límite. Si esta opción se establece en cero o se omite, el rendimiento no se limita.|
|--output-type string|Formato de la salida del comando. Las opciones incluyen: text, json. El valor predeterminado es "text".|

## <a name="see-also"></a>Otras referencias

- [azcopy jobs](storage-ref-azcopy-jobs.md)
