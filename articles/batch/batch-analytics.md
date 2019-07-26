---
title: Azure Batch Analytics | Microsoft Docs
description: Referencia de Azure Batch Analytics.
services: batch
author: laurenhughes
manager: gwallace
ms.assetid: ''
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: lahugh
ms.openlocfilehash: 0f8233846f3835a2bbccf9a7072f223d435b90af
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2019
ms.locfileid: "68323827"
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