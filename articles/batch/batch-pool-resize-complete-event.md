---
title: Evento completo de cambio de tamaño de grupo de Azure Batch | Microsoft Docs
description: Referencia del evento completo de cambio de tamaño de grupo de Batch.
services: batch
author: laurenhughes
manager: gwallace
ms.assetid: ''
ms.service: batch
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: lahugh
ms.openlocfilehash: 8c0843db216ff99aabfda9074ee751597b43a2a2
ms.sourcegitcommit: 267a9f62af9795698e1958a038feb7ff79e77909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/04/2019
ms.locfileid: "70258402"
---
# <a name="pool-resize-complete-event"></a>Evento de finalización de cambio de tamaño del grupo

 Este evento se genera cuando finaliza o no se puede realizar el cambio de tamaño de un grupo.

 En el siguiente ejemplo, se muestra el cuerpo de un evento de cambio de tamaño del grupo para un grupo que aumentó su tamaño y se finalizó correctamente.

```
{
    "id": "myPool",
    "nodeDeallocationOption": "invalid",
        "currentDedicatedNodes": 10,
        "targetDedicatedNodes": 10,
    "currentLowPriorityNodes": 5,
        "targetLowPriorityNodes": 5,
    "enableAutoScale": false,
    "isAutoPool": false,
    "startTime": "2016-09-09T22:13:06.573Z",
    "endTime": "2016-09-09T22:14:01.727Z",
    "resultCode": "Success",
    "resultMessage": "The operation succeeded"
}
```

|Elemento|Tipo|Notas|
|-------------|----------|-----------|
|`id`|Cadena|El identificador del grupo.|
|`nodeDeallocationOption`|Cadena|Especifica cuándo se pueden quitar los nodos del grupo, si disminuye el tamaño del grupo.<br /><br /> Los valores posibles son:<br /><br /> **requeue**: finalizar las tareas en ejecución y volver a ponerlas en cola. Las tareas volverán a ejecutarse cuando se habilite el trabajo. Elimine los nodos en cuanto finalicen las tareas.<br /><br /> **terminate**: finalizar las tareas en ejecución. Las tareas no se ejecutarán de nuevo. Elimine los nodos en cuanto finalicen las tareas.<br /><br /> **taskcompletion**: permita que finalicen las tareas actualmente en ejecución. No programe ninguna tarea nueva mientras espera. Elimine los nodos cuando se hayan completado todas las tareas.<br /><br /> **Retaineddata**: permite que finalicen las tareas actualmente en ejecución, luego espera que caduquen los períodos de retención de datos de todas las tareas. No programe ninguna tarea nueva mientras espera. Elimine los nodos cuando hayan caducado los períodos de retención de todas las tareas.<br /><br /> El valor predeterminado es requeue.<br /><br /> Si aumenta el tamaño del grupo, entonces el valor se establece en **invalid**.|
|`currentDedicatedNodes`|Int32|El número de nodos de proceso dedicados asignados actualmente al grupo.|
|`targetDedicatedNodes`|Int32|El número de nodos de proceso dedicados solicitados para el grupo.|
|`currentLowPriorityNodes`|Int32|El número de nodos de proceso de baja prioridad asignados al grupo.|
|`targetLowPriorityNodes`|Int32|El número de nodos de proceso de baja prioridad solicitados para el grupo.|
|`enableAutoScale`|Bool|Especifica si el tamaño del grupo se ajusta automáticamente con el tiempo.|
|`isAutoPool`|Bool|Especifica si se ha creado el grupo a través del mecanismo AutoPool de un trabajo.|
|`startTime`|DateTime|La hora en que se inició el cambio de tamaño del grupo.|
|`endTime`|DateTime|La hora en que finalizó el cambio de tamaño del grupo.|
|`resultCode`|Cadena|El resultado del cambio de tamaño.|
|`resultMessage`|Cadena| Mensaje detallado sobre el resultado.<br /><br /> Si el cambio de tamaño finalizó sin problemas, esto indica que la operación se completó correctamente.|
