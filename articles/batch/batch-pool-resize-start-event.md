---
title: Evento de inicio de cambio de tamaño de grupo de Azure Batch | Microsoft Docs
description: Referencia del evento de inicio de cambio de tamaño de grupo de Batch.
services: batch
author: dlepow
manager: jeconnoc
ms.assetid: ''
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: danlep
ms.openlocfilehash: 3cabf18cfd771151e62d64dc1d2b47b250ac5471
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2019
ms.locfileid: "54258278"
---
# <a name="pool-resize-start-event"></a>Evento de inicio de cambio de tamaño de grupo

 Este evento se genera se inicia el cambio de tamaño de un grupo. Puesto que el cambio de tamaño de grupo es un evento asincrónico, puede esperar que se genere un evento completo de cambio de tamaño de grupo cuando se haya completado la operación de cambio de tamaño.

 En el ejemplo siguiente se muestra el cuerpo de un evento de inicio de cambio de tamaño de grupo de 0 a 2 nodos con un cambio de tamaño manual.

```
{
    "poolId": "myPool1",
    "nodeDeallocationOption": "invalid",
    "currentDedicated": 0,
    "targetDedicated": 2,
    "enableAutoScale": false,
    "isAutoPool": false
}
```

|Elemento|Tipo|Notas|
|-------------|----------|-----------|
|poolId|string|El identificador del grupo.|
|nodeDeallocationOption|string|Especifica cuándo se pueden quitar los nodos del grupo, si disminuye el tamaño del grupo.<br /><br /> Los valores posibles son:<br /><br /> **requeue**: finalizar las tareas en ejecución y volver a ponerlas en cola. Las tareas volverán a ejecutarse cuando se habilite el trabajo. Elimine los nodos en cuanto finalicen las tareas.<br /><br /> **terminate**: finalizar las tareas en ejecución. Las tareas no se ejecutarán de nuevo. Elimine los nodos en cuanto finalicen las tareas.<br /><br /> **taskcompletion**: permita que finalicen las tareas actualmente en ejecución. No programe ninguna tarea nueva mientras espera. Elimine los nodos cuando se hayan completado todas las tareas.<br /><br /> **Retaineddata**: permite que finalicen las tareas actualmente en ejecución, luego espera que caduquen los períodos de retención de datos de todas las tareas. No programe ninguna tarea nueva mientras espera. Elimine los nodos cuando hayan caducado los períodos de retención de todas las tareas.<br /><br /> El valor predeterminado es requeue.<br /><br /> Si aumenta el tamaño del grupo, entonces el valor se establece en **invalid**.|
|currentDedicated|Int32|El número de nodos de proceso actualmente asignados al grupo.|
|targetDedicated|Int32|El número de nodos de proceso solicitados para el grupo.|
|enableAutoScale|Booleano|Especifica si el tamaño del grupo se ajusta automáticamente con el tiempo.|
|isAutoPool|Booleano|Especifica si se ha creado el grupo a través del mecanismo AutoPool de un trabajo.|
