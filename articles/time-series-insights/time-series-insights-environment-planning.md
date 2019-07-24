---
title: Planeamiento de la escala del entorno Azure Time Series Insights | Microsoft Docs
description: En este artículo se describe cómo seguir los procedimientos recomendados al planear un entorno de Azure Time Series Insights. Las áreas tratadas incluyen la capacidad de almacenamiento, la retención de datos, la capacidad de entrada, la supervisión y la continuidad empresarial y recuperación ante desastres (BCDR).
services: time-series-insights
ms.service: time-series-insights
author: ashannon7
ms.author: dpalled
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 04/29/2019
ms.custom: seodec18
ms.openlocfilehash: 2c11e3f623817894cea801173239cc386c6c3313
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/17/2019
ms.locfileid: "67165834"
---
# <a name="plan-your-azure-time-series-insights-ga-environment"></a>Planificación del entorno de Azure Time Series Insights de disponibilidad general

En este artículo se describe cómo planear el entorno de disponibilidad general (GA) de Azure Time Series Insights según la velocidad de entrada esperada y los requisitos de retención de datos.

## <a name="video"></a>Vídeo

**Vea este vídeo para obtener más información sobre la retención de datos en Azure Time Series Insights y cómo planearla**:<br /><br />

> [!VIDEO https://www.youtube.com/embed/03x6zKDQ6DU]

## <a name="best-practices"></a>Procedimientos recomendados

Para empezar a trabajar con Time Series Insights, se recomienda saber cuántos datos espera insertar por minuto, así como también durante cuánto tiempo debe almacenar los datos.  

Para más información sobre la capacidad y la retención de ambas SKU de Time Series Insights, consulte los [precios de Time Series Insights](https://azure.microsoft.com/pricing/details/time-series-insights/).

Considere estos atributos para planear mejor el entorno para el éxito a largo plazo:

- <a href="#storage-capacity">Capacidad de almacenamiento</a>
- <a href="#data-retention">Período de retención de datos</a>
- <a href="#ingress-capacity">Capacidad de entrada</a>
- <a href="#shape-your-events">Eventos con forma</a>
- <a href="#ensure-that-you-have-reference-data">Confirmación de la presencia de datos de referencia</a>

## <a name="storage-capacity"></a>Capacidad de almacenamiento

De manera predeterminada, Time Series Insights conserva los datos en función de la cantidad de almacenamiento que ha aprovisionado (unidades &#215; la cantidad de almacenamiento por unidad) y la entrada.

## <a name="data-retention"></a>Retención de datos

Puede cambiar el valor de **tiempo de retención de datos** en el entorno de Time Series Insights. Puede habilitar hasta 400 días de retención. 

Time Series Insights tiene dos modos. Un modo realiza la optimización para garantizar que el entorno tiene los datos más actualizados. Este modo está activado, de forma predeterminada. 

El otro modo realiza la optimización para garantizar que se cumplen los límites de retención. En este segundo modo, la entrada se pone en pausa si se cumple la capacidad de almacenamiento total del entorno. 

Puede ajustar la retención y alternar entre ambos modos en la página de configuración del entorno en Azure Portal.

Puede configurar un máximo de 400 días de retención de datos en el entorno de Time Series Insights.

### <a name="configure-data-retention"></a>Configuración de la retención de datos

1. En [Azure Portal](https://portal.azure.com), seleccione el entorno de Time Series Insights.

1. En el panel **Entorno de Time Series Insights**, bajo **Configuración**, seleccione **Configurar**.

1. En el cuadro **Tiempo de retención de datos (en días)** , escriba un valor entre 1 y 400.

   [![Configuración de la retención](media/environment-mitigate-latency/configure-retention.png)](media/environment-mitigate-latency/configure-retention.png#lightbox)

> [!TIP]
> Para obtener más información sobre cómo implementar una directiva de retención de datos adecuada, vea [Cómo configurar la retención](./time-series-insights-how-to-configure-retention.md).

## <a name="ingress-capacity"></a>Capacidad de entrada

La segunda área de interés para planear un entorno de Time Series Insights es la capacidad de entrada. La capacidad de entrada es una derivación de la asignación por minuto.

Desde la perspectiva de la limitación, un paquete de datos de entrada con un tamaño de 32 KB se trata como 32 eventos de 1 KB cada uno. El tamaño de evento máximo permitido es 32 KB. Los paquetes de datos mayores de 32 KB se truncan.

En la tabla siguiente se resume la capacidad de entrada por unidad para cada SKU de Time Series Insights:

|SKU  |Recuento de eventos cada mes  |Tamaño de eventos cada mes  |Recuento de eventos cada minuto  |Tamaño de eventos cada minuto  |
|---------|---------|---------|---------|---------|
|S1     |   30 millones     |  30 GB     |  720    |  720 KB   |
|S2     |   300 millones    |   < 300 GB   | 7200   | 7200 KB  |

Puede aumentar la capacidad de una SKU S1 o S2 a 10 unidades en un solo entorno. No puede migrar desde un entorno de S1 a uno de S2. No puede migrar desde un entorno de S2 a uno de S1.

Para la capacidad de entrada, primero determine la entrada total que requiere de manera mensual. A continuación, determine sus necesidades por minuto. 

La limitación y la latencia desempeñan un papel en la capacidad por minuto. Si tiene un incremento en la entrada de datos que dura menos de 24 horas, Time Series Insights puede "ponerse al día" con una velocidad de entrada que doble las velocidades mencionadas en la tabla anterior.

Por ejemplo, si tiene una sola SKU S1, los datos entran a una velocidad de 720 eventos por minuto y hay un incremento de la velocidad de datos durante menos de una hora a una velocidad de 1440 eventos o menos, no habrá una latencia apreciable en el entorno. Pero si supera los 1440 eventos por minuto durante más de una hora, es probable que experimente una latencia con respecto a los datos que se visualizan y están disponibles para consulta en el entorno.

Es posible que no sepa de antemano cuántos datos espera insertar. En este caso, puede encontrar la telemetría de datos para [Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-metrics) y [Azure Event Hubs](https://blogs.msdn.microsoft.com/cloud_solution_architect/2016/05/25/using-the-azure-rest-apis-to-retrieve-event-hub-metrics/) en la suscripción de Azure Portal. La telemetría puede ayudar a determinar cómo aprovisionar el entorno. Use la página **Métricas** del origen del evento correspondiente en Azure Portal para ver su telemetría. Si comprende las métricas de origen del evento, puede planea de manera más eficaz y aprovisionar el entorno de Time Series Insights.

### <a name="calculate-ingress-requirements"></a>Cálculo de los requisitos de entrada

Para calcular los requisitos de entrada:

- Verifique que la capacidad de entrada está por encima de la velocidad promedio por minuto y que el entorno es suficientemente grande para controlar la entrada anticipada equivalente al doble de la capacidad durante menos de una hora.

- Si se producen incrementos de entrada que duran más de 1 hora, use la tasa de incremento como promedio. Aprovisione un entorno con capacidad para controlar la tasa de incremento.

### <a name="mitigate-throttling-and-latency"></a>Mitigación de la limitación y la latencia

Para información sobre cómo prevenir la limitación y la latencia, consulte el artículo sobre la [mitigación de la limitación y la latencia](time-series-insights-environment-mitigate-latency.md).

## <a name="shape-your-events"></a>Moldear los eventos

Es importante asegurarse de que el método de envío de eventos de Time Series Insights es compatible con el tamaño del entorno que se va a aprovisionar. (De forma inversa, puede asignar el tamaño del entorno respecto al número de eventos que lee Time Series Insights y el tamaño de cada evento). También es importante pensar en los atributos que quiere usar para segmentar y filtrar al consultar los datos.

> [!TIP]
> Revise la documentación de configuración de JSON en [Enviar eventos](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-send-events).

## <a name="ensure-that-you-have-reference-data"></a>Garantizar la presencia de datos de referencia

Un *conjunto de datos de referencia* es una colección de elementos que aumentan los eventos de un origen de eventos. El motor de entrada de Time Series Insights combina cada evento del origen de eventos con la fila de datos correspondiente en el conjunto de datos de referencia. A partir de ese momento, el evento aumentado está disponible para consultas. La combinación se basa en las columnas de **clave principal** definidas en el conjunto de datos de referencia.

> [!NOTE]
> Los datos de referencia no se combinan de manera retroactiva. Solo los datos de entrada actuales y futuros se combinan y unen con el conjunto de datos de referencia una vez que se han configurado y cargado. Si va a enviar una gran cantidad de datos históricos a Time Series Insights y primero no carga o crea datos de referencia en Time Series Insights, deberá rehacer el trabajo (lo que no resulta nada agradable).  

Para más información sobre cómo crear, cargar y administrar los datos de referencia en Time Series Insights, vea nuestra [documentación sobre el conjunto de datos de referencia](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-add-reference-data-set).

[!INCLUDE [business-disaster-recover](../../includes/time-series-insights-business-recovery.md)]

## <a name="next-steps"></a>Pasos siguientes

- Empiece por crear [un entorno de Time Series Insights en Azure Portal](time-series-insights-get-started.md).

- Obtenga información sobre cómo [agregar un origen de evento de Event Hubs](time-series-insights-how-to-add-an-event-source-eventhub.md) al entorno de Time Series Insights.

- Obtenga información sobre cómo [configurar un origen de eventos de IoT Hub](time-series-insights-how-to-add-an-event-source-iothub.md).
