---
title: Diagnóstico y resolución de problemas en Azure Time Series Insights | Microsoft Docs
description: En este artículo se describe cómo diagnosticar y solucionar problemas comunes que pueden surgir en el entorno de Azure Time Series Insights.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: dpalled
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.workload: big-data
ms.topic: troubleshooting
ms.date: 05/07/2019
ms.custom: seodec18
ms.openlocfilehash: 68d46b54bf066724d8f3474f421d2d1f751b75e3
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66235043"
---
# <a name="diagnose-and-solve-issues-in-your-time-series-insights-environment"></a>Diagnóstico y solución de problemas de su entorno Time Series Insights

En este artículo se describen algunos problemas que podría encontrar en el entorno de Azure Time Series Insights. Se ofrecen posibles causas y soluciones para la resolución.

## <a name="video"></a>Vídeo

### <a name="learn-about-common-time-series-insights-customer-challenges-and-mitigationsbr"></a>Obtenga información sobre las mitigaciones y dificultades de los clientes de tiempo común Series Insights.</br>

> [!VIDEO https://www.youtube.com/embed/7U0SwxAVSKw]

## <a name="problem-no-data-is-shown"></a>Problema: se muestra ningún dato

Hay varias razones por las que podría no ver ningún dato en el [explorador de Azure Time Series Insights](https://insights.timeseries.azure.com):

### <a name="cause-a-event-source-data-isnt-in-json-format"></a>Datos de origen del evento de causa r: no se están en formato JSON

Azure Time Series Insights solo admite datos JSON. Para ver ejemplos de JSON, consulte [Formas de JSON admitidas](./how-to-shape-query-json.md).

### <a name="cause-b-the-event-source-key-is-missing-a-required-permission"></a>Causa B: la clave de origen del evento no tiene un permiso necesario

* Para IoT Hub en Azure IoT Hub, debe proporcionar la clave con los permisos de **conexión de servicio**. Servirán las directivas **iothubowner** o **service**, dado que ambas tienen permisos de **conexión de servicio**.

   [![Los permisos de conexión de servicio IoT Hub](media/diagnose-and-solve-problems/iothub-serviceconnect-permissions.png)](media/diagnose-and-solve-problems/iothub-serviceconnect-permissions.png#lightbox)

* Para un centro de eventos de Azure Event Hubs, debe proporcionar la clave con permisos de **escucha**. Servirán las directivas **read** o **manage**, dado que ambas tienen permisos de **escucha**.

   [![Permisos de escucha de concentrador de eventos](media/diagnose-and-solve-problems/eventhub-listen-permissions.png)](media/diagnose-and-solve-problems/eventhub-listen-permissions.png#lightbox)

### <a name="cause-c-the-consumer-group-provided-isnt-exclusive-to-time-series-insights"></a>Causa C: el grupo de consumidores proporcionado no es exclusivo de Time Series Insights

Al registrar un centro de IoT o un centro de eventos, es importante establecer el grupo de consumidores que quiere usar para leer los datos. Este grupo de consumidores *no puede ser compartido*. Si el grupo de consumidores es compartido, el centro de IoT o el centro de eventos subyacente desconectará automáticamente uno de los lectores de forma aleatoria. Proporcione un grupo de consumidor exclusivo del que Time Series Insights pueda leer.

## <a name="problem-some-data-is-shown-but-data-is-missing"></a>Problema: se muestran algunos datos, pero los datos que faltan

Cuando los datos aparecen solo parcialmente y parece que están atrasados, debe considerar varias posibilidades.

### <a name="cause-a-your-environment-is-being-throttled"></a>Hacer que se está limitando el entorno de r:

La limitación es un problema común cuando se aprovisionan entornos después de crear un origen de eventos que tiene datos. Azure IoT Hub y Azure Events Hubs almacenan datos durante siete días como máximo. Time Series Insights siempre empieza con el evento más antiguo en el origen de eventos (primero en entrar, primero en salir o *FIFO*).

Por ejemplo, si tiene 5 millones de eventos en un origen de eventos al conectarse a un entorno S1 de Time Series Insights de una sola unidad, Time Series Insights lee aproximadamente 1 millón de eventos al día. Puede parecer que Time Series Insights está experimentando cinco días de latencia. Sin embargo, lo que sucede es que el entorno está limitado.

Si tiene los eventos antiguos en el origen de eventos, puede enfocarlo de dos maneras:

- Cambie los límites de retención del origen de eventos para ayudar a eliminar los eventos antiguos que no quiere que aparezcan en Time Series Insights.
- Aprovisione un tamaño mayor de entorno (número de unidades) para aumentar el rendimiento de los eventos antiguos. Siguiendo con el ejemplo anterior, si aumenta ese mismo entorno de S1 a cinco unidades durante un día, debería actualizarse en el plazo de un día. Si la producción de eventos de estado estable es un millón o menos eventos al día, puede reducir la capacidad del evento a una unidad después de la actualización.

La limitación se aplica según el tipo y la capacidad de la SKU del entorno. Todos los orígenes de eventos del entorno comparten esta capacidad. Si el origen de eventos de la instancia de IoT Hub o del centro de evento inserta datos por encima de los límites exigidos, experimentará limitaciones y retrasos.

En la siguiente figura se muestra un entorno de Time Series Insights con una SKU S1 y una capacidad 3. Este entorno registra 3 millones de eventos al día.

![Capacidad actual de la SKU del entorno](media/diagnose-and-solve-problems/environment-sku-current-capacity.png)](media/diagnose-and-solve-problems/environment-sku-current-capacity.png#lightbox)

Por ejemplo, supongamos que este entorno ingiere mensajes de un centro de eventos. La figura siguiente muestra la velocidad de entrada:

[![Tasa de entrada de ejemplo para un centro de eventos](media/diagnose-and-solve-problems/eventhub-ingress-rate.png)](media/diagnose-and-solve-problems/eventhub-ingress-rate.png#lightbox)

La velocidad de entrada diaria es de 67 000 mensajes aproximadamente. Esta velocidad se traduce aproximadamente en 46 mensajes por minuto. Si cada mensaje de centro de eventos se acopla a un evento de Time Series Insights, no se produce ninguna limitación. Si cada mensaje de centro de eventos se acopla a 100 eventos de Time Series Insights, se deben ingerir 4600 mensajes por minuto. Un entorno de SKU S1 que tiene una capacidad de 3 solo puede admitir la entrada de 2100 eventos por minuto (1 millón de eventos por día = 700 eventos por minuto por 3 unidades = 2100 eventos por minuto). En el caso de esta configuración, experimentará un retraso debido a la limitación.

Para comprender de una manera más profunda cómo funciona la lógica de aplanamiento, consulte [Formas JSON admitidas](./how-to-shape-query-json.md).

#### <a name="recommended-resolutions-for-excessive-throttling"></a>Resoluciones recomendadas para la limitación excesiva

Para corregir el retraso, aumente la capacidad de la SKU de su entorno. Para más información, consulte [Escalado de su entorno de Time Series Insights](time-series-insights-how-to-scale-your-environment.md).

### <a name="cause-b-initial-ingestion-of-historical-data-slows-ingress"></a>Causa B: la ingesta inicial de los datos históricos ralentiza la entrada

Si va a conectar un origen de eventos existente, es probable que su centro de eventos o instancia de IoT Hub ya contengan datos. El entorno comienza a extraer datos del principio del período de retención del mensaje de origen del evento. No se puede invalidar este procesamiento predeterminado. Puede exponerse a limitaciones. La limitación puede tardar un rato en alcanzarse ya que ingiere datos históricos.

#### <a name="recommended-resolutions-for-large-initial-ingestion"></a>Resoluciones recomendadas para una ingesta inicial grande

Para corregir el retraso:

1. Aumente la capacidad de la SKU al valor máximo permitido (10 en este caso). Después de aumentar la capacidad, se inicia el proceso de entrada para ponerse al día mucho más rápidamente. El aumento de capacidad se le cobra. Para visualizar lo rápido que se está recuperando, puede ver el gráfico de disponibilidad del [explorador de Time Series Insights](https://insights.timeseries.azure.com).

2. Cuando se actualice el retraso, reduzca la capacidad de la SKU a la velocidad de entrada normal.

## <a name="problem-my-event-sources-timestamp-property-name-setting-doesnt-work"></a>Problema: la configuración de nombre de propiedad de marca de tiempo de mi origen evento no funciona

Asegúrese de que el nombre y el valor de la propiedad timestamp se ajustan a las reglas siguientes:

* El nombre de la propiedad timestamp distingue mayúsculas de minúsculas.
* El valor de la propiedad timestamp que procede del origen del evento, como una cadena JSON, debe tener el formato _dd-MM-aaaaTHH:mm:ss.FFFFFFFK_. Un ejemplo es **2008-04-12T12:53Z**.

La manera más fácil de asegurarse de que el nombre de la propiedad timestamp se captura y funciona correctamente consiste en utilizar el explorador de Time Series Insights. En el explorador de Time Series Insights mediante el gráfico, seleccione un período de tiempo después de proporcionar el nombre de la propiedad timestamp. Haga clic con el botón derecho en la selección y, a continuación, seleccione la opción de **exploración de eventos**.

El primer encabezado de columna debe ser el nombre de propiedad timestamp. Junto a la palabra **Timestamp**, debería ver **($ts)** .

No debe ver los valores siguientes:

- *(abc)* : indica que Time Series Insights lee los valores de datos como cadenas.
- *Icono de calendario*: indica que Time Series Insights lee los valores de datos como *datetime*.
- *#* : indica que Time Series Insights lee los valores de datos como un entero.

## <a name="next-steps"></a>Pasos siguientes

- Para obtener ayuda, inicie una conversación en el [Foro de MSDN](https://social.msdn.microsoft.com/Forums/home?forum=AzureTimeSeriesInsights) o en [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-timeseries-insights).

- Para opciones de soporte técnico asistido, use [Soporte técnico de Azure](https://azure.microsoft.com/support/options/).
