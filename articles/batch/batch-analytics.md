---
title: Azure Batch Analytics | Microsoft Docs
description: Referencia de Azure Batch Analytics.
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
ms.openlocfilehash: 03cb6231a42e27c474e20f4c6ae91095156bb766
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2019
ms.locfileid: "70095513"
---
# <a name="batch-analytics"></a>Análisis por lotes
Los temas de análisis por lotes contienen información de referencia para los eventos y las alertas disponibles para los recursos del servicio de Batch.

Vea [Registro de diagnóstico de Azure Batch](https://azure.microsoft.com/documentation/articles/batch-diagnostics/) para obtener más información sobre cómo habilitar y consumir los registros de diagnóstico por lotes.

## <a name="diagnostic-logs"></a>Registros de diagnóstico

El servicio de Azure Batch genera los siguientes eventos de registro de diagnóstico mientras duren determinados recursos de Batch.

**Eventos del registro del servicio**
* [Creación del grupo](batch-pool-create-event.md)
* [Inicio de eliminación del grupo](batch-pool-delete-start-event.md)
* [Finalización de eliminación del grupo](batch-pool-delete-complete-event.md)
* [Inicio de cambio de tamaño del grupo](batch-pool-resize-start-event.md)
* [Finalización de cambio de tamaño del grupo](batch-pool-resize-complete-event.md)
* [Inicio de tarea](batch-task-start-event.md)
* [Finalización de tarea](batch-task-complete-event.md)
* [Error en la tarea](batch-task-fail-event.md)