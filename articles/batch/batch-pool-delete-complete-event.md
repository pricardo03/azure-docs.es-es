---
title: Evento completo de eliminación de grupo de Azure Batch
description: Referencia del evento completo de eliminación de grupo de Batch. Este evento se genera cuando se finaliza una operación de eliminación del grupo.
services: batch
author: LauraBrenner
manager: evansma
ms.assetid: ''
ms.service: batch
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: labrenne
ms.openlocfilehash: d317d7395a8246c109073a72338b55960cb50954
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2020
ms.locfileid: "77023623"
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
|`id`|String|El identificador del grupo.|
|`startTime`|DateTime|La hora en que se inició la eliminación del grupo.|
|`endTime`|DateTime|La hora en que finalizó la eliminación del grupo.|

## <a name="remarks"></a>Observaciones
Para más información sobre los estados y códigos de error para la operación de cambio de tamaño del grupo, consulte [Eliminar un grupo de una cuenta](https://docs.microsoft.com/rest/api/batchservice/delete-a-pool-from-an-account).
