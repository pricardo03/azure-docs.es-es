---
title: azcopy jobs resume | Microsoft Docs
description: En este artículo se proporciona información de referencia del comando azcopy jobs resume.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 08/26/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: 8786da42f000a2f13279499159a7af424aa10748
ms.sourcegitcommit: 532335f703ac7f6e1d2cc1b155c69fc258816ede
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/30/2019
ms.locfileid: "70196762"
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
