---
title: azcopy jobs show | Microsoft Docs
description: En este artículo se proporciona información de referencia del comando azcopy jobs show.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 08/26/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: d4369bd0c986ee20a0796436fea47509a711de4f
ms.sourcegitcommit: 532335f703ac7f6e1d2cc1b155c69fc258816ede
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/30/2019
ms.locfileid: "70196814"
---
# <a name="azcopy-jobs-show"></a>azcopy jobs show

Muestra información detallada del identificador de trabajo especificado.

## <a name="synopsis"></a>Sinopsis

Si solo se proporciona el identificador de trabajo sin ninguna marca, se devolverá el resumen de progreso del trabajo.

Si se establece la marca `with-status`, se mostrará la lista de transferencias del trabajo con el valor especificado.

```azcopy
azcopy jobs show [jobID] [flags]
```

## <a name="options"></a>Opciones

|Opción|DESCRIPCIÓN|
|--|--|
|-h, --help|Muestra el contenido de la ayuda para el comando show.|
|--with-status string|Solo muestra la lista de las transferencias de un trabajo con este estado. Valores disponibles: Started (Iniciado), Success (Correcto), Failed (Con errores)|

## <a name="options-inherited-from-parent-commands"></a>Opciones heredadas de comandos primarios

|Opción|DESCRIPCIÓN|
|---|---|
|--cap-mbps uint32|Limita la velocidad de transferencia, en megabits por segundo. El rendimiento en un momento dado puede variar ligeramente del límite. Si esta opción se establece en cero o se omite, el rendimiento no se limita.|
|--output-type string|Formato de la salida del comando. Las opciones incluyen: text, json. El valor predeterminado es "text".|

## <a name="see-also"></a>Otras referencias

- [azcopy jobs](storage-ref-azcopy-jobs.md)
