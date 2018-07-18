---
title: Variables del sistema en Azure Data Factory | Microsoft Docs
description: En este artículo se describen las variables del sistema compatibles con Azure Data Factory. Puede usar estas variables en las expresiones a la hora de definir entidades de Data Factory.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/12/2018
ms.author: shlo
ms.openlocfilehash: 43ea8703bdbfc23511c831a5f91c9461948cc254
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2018
ms.locfileid: "37055753"
---
# <a name="system-variables-supported-by-azure-data-factory"></a>Variables del sistema compatibles con Azure Data Factory
En este artículo se describen las variables del sistema compatibles con Azure Data Factory. Puede usar estas variables en las expresiones a la hora de definir entidades de Data Factory.

## <a name="pipeline-scope"></a>Ámbito de canalización
Se puede hacer referencia a estas variables del sistema en cualquier JSON de la canalización.

| Nombre de la variable | DESCRIPCIÓN |
| --- | --- |
| @pipeline().DataFactory |Nombre de la factoría de datos en la que se lleva a cabo la ejecución de la canalización |
| @pipeline().Pipeline |Nombre de la canalización |
| @pipeline().RunId | Id. de la ejecución de canalización específica |
| @pipeline().TriggerType | Tipo del desencadenador que ha invocado la canalización (Manual o Programador) |
| @pipeline().TriggerId| Identificador del desencadenador que invoca la canalización |
| @pipeline().TriggerName| Nombre del desencadenador que invoca la canalización |
| @pipeline().TriggerTime| Hora a la que el desencadenador invocó la canalización. La hora del desencadenador es la hora desencadenada real, y no la hora programada. Por ejemplo, se devuelve `13:20:08.0149599Z` en lugar de `13:20:00.00Z` |

## <a name="schedule-trigger-scope"></a>Ámbito de desencadenador de programación
Se puede hacer referencia a estas variables del sistema en cualquier parte del JSON del desencadenador si este es de tipo "ScheduleTrigger".

| Nombre de la variable | DESCRIPCIÓN |
| --- | --- |
| @trigger().scheduledTime |Hora a la que se programó el desencadenador para invocar la ejecución de la canalización. Por ejemplo, para un desencadenador que se activa cada 5 minutos, esta variable devolvería `2017-06-01T22:20:00Z`, `2017-06-01T22:25:00Z` y `2017-06-01T22:29:00Z`, respectivamente.|
| @trigger().startTime |Hora a la que **realmente** se activó el desencadenador para invocar la ejecución de la canalización. Por ejemplo, para un desencadenador que se activa cada 5 minutos, esta variable podría devolver algo parecido a `2017-06-01T22:20:00.4061448Z`, `2017-06-01T22:25:00.7958577Z` y `2017-06-01T22:29:00.9935483Z`, respectivamente.|

## <a name="tumbling-window-trigger-scope"></a>Ámbito de desencadenador de ventana de saltos de tamaño constante
Se puede hacer referencia a estas variables del sistema en cualquier parte del JSON del desencadenador si este es de tipo "TumblingWindowTrigger".

| Nombre de la variable | DESCRIPCIÓN |
| --- | --- |
| @trigger().outputs.windowStartTime |Inicio de la ventana cuando se programó el desencadenador para invocar la ejecución de la canalización. Si el desencadenador de ventana de saltos de tamaño constante tiene una frecuencia "por hora", sería al principio de la hora.|
| @trigger().outputs.windowEndTime |Fin de la ventana cuando se programó el desencadenador para invocar la ejecución de la canalización. Si el desencadenador de ventana de saltos de tamaño constante tiene una frecuencia "por hora", sería al final de la hora.|
## <a name="next-steps"></a>Pasos siguientes
Para información sobre cómo se usan estas variables en las expresiones, vea [Expression language & functions](control-flow-expression-language-functions.md) (Lenguaje de expresión y funciones).
