---
title: Cómo iniciar un trabajo de Azure Stream Analytics
description: En este artículo se describe cómo iniciar un trabajo de Stream Analytics.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/03/2019
ms.openlocfilehash: 9bc3e4132919e5fc5baadc78841e66efd3c34bcd
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61362272"
---
# <a name="how-to-start-an-azure-stream-analytics-job"></a>Cómo iniciar un trabajo de Azure Stream Analytics

Puede iniciar el trabajo de Azure Stream Analytics mediante el portal de Azure, Visual Studio y PowerShell. Cuando se inicia un trabajo, seleccione una hora de empezar a crear la salida del trabajo. Azure portal, Visual Studio y PowerShell tienen diferentes métodos para establecer la hora de inicio. Esos métodos se describen a continuación.

## <a name="start-options"></a>Opciones de inicio
Las tres opciones siguientes están disponibles para iniciar un trabajo. Tenga en cuenta que todas las veces que se mencionan a continuación son los especificados en [TIMESTAMP BY](https://docs.microsoft.com/stream-analytics-query/timestamp-by-azure-stream-analytics). Si no se especifica mediante la marca de tiempo, se usará la hora de llegada.
* **Ahora**: Hace que el punto de partida del evento de salida a transmitir el mismo que cuando se inicia el trabajo. Si se usa un operador temporal (por ejemplo, ventana de tiempo, retraso o combinación), Azure Stream Analytics buscará automáticamente volver en los datos en el origen de entrada. Por ejemplo, si inicia un trabajo de "Ahora" y la consulta utiliza una ventana de saltos de tamaño constante de 5 minutos, Azure Stream Analytics se buscan datos desde hace en la entrada de 5 minutos.
El primer evento de salida posibles tendría una marca de tiempo igual o mayor que la hora actual y ASA garantiza que todos los eventos de entrada que lógicamente pueden contribuir a la salida se ha sido responsable. Por ejemplo, no se generan agregados en ventanas parciales. Siempre es el valor agregado completando.

* **Personalizado**: Puede elegir el punto de partida de la salida. Del mismo modo que el **ahora** opción, Azure Stream Analytics leerá automáticamente los datos antes de este momento si se usa un operador temporal 

* **Última detención**. Esta opción está disponible cuando el trabajo se inició previamente, pero se detuvo manualmente o no. Al elegir esta opción, Azure Stream Analytics usará la última hora de salida para reiniciar el trabajo, por lo que no se pierden datos. De forma similar a las opciones anteriores, Azure Stream Analytics leerá automáticamente los datos antes de este momento si se usa un operador temporal. Dado que varias particiones de entrada pueden tener otro momento, se usa la primera hora de detención de todas las particiones, como resultado algunas duplicados pueden verse en la salida. Para obtener más información sobre exactamente-procesamiento una vez que están disponibles en la página [garantías de entrega de evento](https://docs.microsoft.com/stream-analytics-query/event-delivery-guarantees-azure-stream-analytics).


## <a name="azure-portal"></a>Azure Portal

Vaya a su trabajo en Azure portal y seleccione **iniciar** en la página información general. Seleccione un **hora de inicio de salida del trabajo** y, a continuación, seleccione **iniciar**.

Elija una de las opciones para **hora de inicio de salida del trabajo**. Las opciones son *ahora*, *personalizado*, y, si previamente se ejecutó el trabajo, *última detención*. Consulte más arriba para obtener más información acerca de estas opciones.

## <a name="visual-studio"></a>Visual Studio

En la vista de trabajos, seleccione el botón de flecha verde para iniciar el trabajo. Establecer el **iniciar modo de salida de trabajo** y seleccione **iniciar**. El estado del trabajo cambiará a **ejecutando**.

Hay tres opciones para **iniciar modo de salida de trabajo**: *JobStartTime*, *CustomTime*, y *LastOutputEventTime*. Si esta propiedad no está presente, el valor predeterminado es *JobStartTime*. Consulte más arriba para obtener más información acerca de estas opciones.


## <a name="powershell"></a>PowerShell

Use el siguiente cmdlet para iniciar el trabajo con PowerShell:

```powershell
Start-AzStreamAnalyticsJob `
  -ResourceGroupName $resourceGroup `
  -Name $jobName `
  -OutputStartMode 'JobStartTime'
```

Hay tres opciones para **OutputStartMode**: *JobStartTime*, *CustomTime*, y *LastOutputEventTime*. Si esta propiedad no está presente, el valor predeterminado es *JobStartTime*. Consulte más arriba para obtener más información acerca de estas opciones.

Para obtener más información sobre la `Start-AzStreamAnalyitcsJob` cmdlet, ver el [inicio AzStreamAnalyticsJob referencia](/powershell/module/az.streamanalytics/start-azstreamanalyticsjob).

## <a name="next-steps"></a>Pasos siguientes

* [Inicio rápido: Creación de un trabajo de Stream Analytics mediante Azure Portal](stream-analytics-quick-create-portal.md)
* [Inicio rápido: Crear un trabajo de Stream Analytics mediante Azure PowerShell](stream-analytics-quick-create-powershell.md)
* [Inicio rápido: Creación de un trabajo de Stream Analytics mediante las herramientas de Azure Stream Analytics para Visual Studio](stream-analytics-quick-create-vs.md)
