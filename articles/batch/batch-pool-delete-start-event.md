---
title: Evento de inicio de eliminación de grupo de Azure Batch | Microsoft Docs
description: Referencia del evento de inicio de eliminación de grupo de Batch.
services: batch
author: ju-shim
manager: gwallace
ms.assetid: ''
ms.service: batch
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: jushiman
ms.openlocfilehash: d582e2b04d203484632a1781d1819f612de41fe7
ms.sourcegitcommit: dbcc4569fde1bebb9df0a3ab6d4d3ff7f806d486
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/15/2020
ms.locfileid: "76026737"
---
# <a name="pool-delete-start-event"></a>Evento de inicio de eliminación del grupo

 Este evento se genera cuando se inicia una operación de eliminación del grupo. Puesto que la eliminación de grupo es un evento asincrónico, puede esperar que se genere un evento completo de eliminación del grupo cuando se haya completado la operación de eliminación.

 En el siguiente ejemplo, se muestra el cuerpo de un evento de inicio de eliminación del grupo.

```
{
    "id": "myPool1"
}
```

|Elemento|Tipo|Notas|
|-------------|----------|-----------|
|`id`|String|El identificador del grupo.|
