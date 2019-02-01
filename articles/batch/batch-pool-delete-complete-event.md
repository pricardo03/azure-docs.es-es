---
title: Evento completo de eliminación de grupo de Azure Batch | Microsoft Docs
description: Referencia del evento completo de eliminación de grupo de Batch.
services: batch
author: laurenhughes
manager: jeconnoc
ms.assetid: ''
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: lahugh
ms.openlocfilehash: e715ccd0f5e79f9c640a3c060b0252b798748b4d
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2019
ms.locfileid: "55474555"
---
# <a name="pool-delete-complete-event"></a>Evento de finalización de eliminación del grupo

 Este evento se genera cuando se finaliza una operación de eliminación del grupo.

 En el siguiente ejemplo, se muestra el cuerpo de un evento de finalización de eliminación del grupo.

```
{
    "id": "myPool1",
    "startTime": "2016-09-09T22:13:48.579Z",
    "endTime": "2016-09-09T22:14:08.836Z"
}
```

|Elemento|Tipo|Notas|
|-------------|----------|-----------|
|id|string|El identificador del grupo.|
|startTime|DateTime|La hora en que se inició la eliminación del grupo.|
|endTime|DateTime|La hora en que finalizó la eliminación del grupo.|

## <a name="remarks"></a>Comentarios
Para más información sobre los estados y códigos de error para la operación de cambio de tamaño del grupo, consulte [Eliminar un grupo de una cuenta](https://docs.microsoft.com/rest/api/batchservice/delete-a-pool-from-an-account).