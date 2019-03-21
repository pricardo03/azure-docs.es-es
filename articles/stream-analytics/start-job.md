---
title: Cómo iniciar un trabajo de Azure Stream Analytics
description: En este artículo se describe cómo iniciar un trabajo de Stream Analytics.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/12/2019
ms.openlocfilehash: fb1d724907c09e2eb77930f5a235336ca8cd3a25
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "57886854"
---
# <a name="how-to-start-an-azure-stream-analytics-job"></a>Cómo iniciar un trabajo de Azure Stream Analytics

Puede iniciar el trabajo de Azure Stream Analytics mediante el portal de Azure, Visual Studio y PowerShell. Cuando se inicia un trabajo, seleccione una hora de empezar a crear la salida del trabajo. Azure portal, Visual Studio y PowerShell tienen diferentes métodos para establecer la hora de inicio. Esos métodos se describen a continuación.

## <a name="azure-portal"></a>Azure Portal

Vaya a su trabajo en Azure portal y seleccione **iniciar** en la página información general. Seleccione un **hora de inicio de salida del trabajo** y, a continuación, seleccione **iniciar**.

Hay tres opciones para **hora de inicio de salida del trabajo**: *Ahora*, *personalizado*, y *última detención*. Seleccionar *ahora* inicia el trabajo en el momento actual. Seleccionar *personalizado* le permite establecer un tiempo personalizado en el pasado o en el futuro para el trabajo comenzar. Para reanudar un trabajo detenido sin perder datos, elija *última detención*.

## <a name="visual-studio"></a>Visual Studio

En la vista de trabajos, seleccione el botón de flecha verde para iniciar el trabajo. Establecer el **iniciar modo de salida de trabajo** y seleccione **iniciar**. El estado del trabajo cambiará a **ejecutando**.

Hay tres opciones para **iniciar modo de salida de trabajo**: *JobStartTime*, *CustomTime*, y *LastOutputEventTime*. Si esta propiedad no está presente, el valor predeterminado es *JobStartTime*.

*JobStartTime* hace que el punto de partida del evento de salida sea el mismo que cuando se inicia el trabajo de stream.

*CustomTime* comienza la salida en una hora personalizada que se especifica en el *OutputStartTime* parámetro.

*LastOutputEventTime* hace que el punto inicial de la secuencia de eventos de salida igual que el último evento de tiempo de salida.

## <a name="powershell"></a>PowerShell

Use el siguiente cmdlet para iniciar el trabajo con PowerShell:

```powershell
Start-AzStreamAnalyticsJob `
  -ResourceGroupName $resourceGroup `
  -Name $jobName `
  -OutputStartMode 'JobStartTime'
```

Hay tres opciones para **OutputStartMode**: *JobStartTime*, *CustomTime*, y *LastOutputEventTime*. Si esta propiedad no está presente, el valor predeterminado es *JobStartTime*.

*JobStartTime* hace que el punto de partida del evento de salida sea el mismo que cuando se inicia el trabajo de stream.

*CustomTime* comienza la salida en una hora personalizada que se especifica en el *OutputStartTime* parámetro.

*LastOutputEventTime* hace que el punto inicial de la secuencia de eventos de salida igual que el último evento de tiempo de salida.

Para obtener más información sobre la `Start-AzStreamAnalyitcsJob` cmdlet, ver el [inicio AzStreamAnalyticsJob referencia](/powershell/module/az.streamanalytics/start-azstreamanalyticsjob).

## <a name="next-steps"></a>Pasos siguientes

* [Inicio rápido: Creación de un trabajo de Stream Analytics mediante Azure Portal](stream-analytics-quick-create-portal.md)
* [Inicio rápido: Crear un trabajo de Stream Analytics mediante Azure PowerShell](stream-analytics-quick-create-powershell.md)
* [Inicio rápido: Creación de un trabajo de Stream Analytics mediante las herramientas de Azure Stream Analytics para Visual Studio](stream-analytics-quick-create-vs.md)
