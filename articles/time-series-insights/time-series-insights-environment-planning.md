---
title: Planeamiento de la escala del entorno Azure Time Series Insights | Microsoft Docs
description: En este artículo se describe cómo seguir los procedimientos recomendados al planear un entorno de Azure Time Series Insights. Las áreas cubiertas incluyen la capacidad de almacenamiento, retención de datos, capacidad de entrada, supervisión y recuperación ante desastres y continuidad empresarial (BCDR).
services: time-series-insights
ms.service: time-series-insights
author: ashannon7
ms.author: dpalled
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 04/29/2019
ms.custom: seodec18
ms.openlocfilehash: 9d9f9b7fe7aafa927f023e1d4e30e079a26f7fab
ms.sourcegitcommit: 087ee51483b7180f9e897431e83f37b08ec890ae
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66431027"
---
# <a name="plan-your-azure-time-series-insights-ga-environment"></a>Planear su entorno de Azure tiempo Series Insights GA

En este artículo se describe cómo planear su entorno de disponibilidad general (GA) de Azure Time Series Insights según la tasa de entrada esperada y los requisitos de retención de datos.

## <a name="video"></a>Vídeo

**Vea este vídeo para obtener más información sobre la retención de datos en Azure Time Series Insights y cómo planear lo**:<br /><br />

> [!VIDEO https://www.youtube.com/embed/03x6zKDQ6DU]

## <a name="best-practices"></a>Procedimientos recomendados

Para empezar a trabajar con Time Series Insights, es mejor si sabe cuántos datos espera insertar por minuto y cuánto tiempo deben almacenar los datos.  

Para más información sobre la capacidad y la retención de ambas SKU de Time Series Insights, consulte los [precios de Time Series Insights](https://azure.microsoft.com/pricing/details/time-series-insights/).

Para planear mejor el entorno de Time Series Insights para el éxito a largo plazo, tenga en cuenta los siguientes atributos:

- <a href="#storage-capacity">Capacidad de almacenamiento</a>
- <a href="#data-retention">Período de retención de datos</a>
- <a href="#ingress-capacity">Capacidad de entrada</a>
- <a href="#shape-your-events">Dar forma a sus eventos</a>
- <a href="#ensure-that-you-have-reference-data">Asegúrese de que tiene datos de referencia en su lugar</a>

## <a name="storage-capacity"></a>Capacidad de almacenamiento

De forma predeterminada, Time Series Insights conserva los datos según la cantidad de almacenamiento que se aprovisiona (unidades &#215; la cantidad de almacenamiento por unidad) y la entrada.

## <a name="data-retention"></a>Retención de datos

Puede cambiar el **tiempo de retención de datos** configuración en el entorno de Time Series Insights. Puede habilitar hasta 400 días de retención. 

Time Series Insights tiene dos modos. Un modo se optimiza para asegurarse de que su entorno tiene los datos más actualizados. Este modo está activado, de forma predeterminada. 

El otro modo se optimiza para garantizar que se cumplan los límites de retención. En el modo de la segunda entrada se pausa si se cumple la capacidad de almacenamiento total del entorno. 

Puede ajustar la retención y alternar entre los dos modos en la página de configuración del entorno en el portal de Azure.

Puede configurar un máximo de 400 días de retención de datos en el entorno de Time Series Insights.

### <a name="configure-data-retention"></a>Configuración de la retención de datos

1. En [Azure Portal](https://portal.azure.com), seleccione el entorno de Time Series Insights.

1. En el **entorno Time Series Insights** panel, en **configuración**, seleccione **configurar**.

1. En el **tiempo de retención de datos (en días)** , escriba un valor comprendido entre 1 y 400.

   [![Configurar la retención](media/environment-mitigate-latency/configure-retention.png)](media/environment-mitigate-latency/configure-retention.png#lightbox)

> [!TIP]
> Para obtener más información acerca de cómo implementar una directiva de retención de datos adecuado, vea [cómo configurar la retención](./time-series-insights-how-to-configure-retention.md).

## <a name="ingress-capacity"></a>Capacidad de entrada

La segunda área de interés para planear un entorno de Time Series Insights es la capacidad de entrada. Capacidad de entrada es un derivado de la asignación por minuto.

Desde una perspectiva de limitación, un paquete de datos de entrada que tiene un tamaño de paquete de 32 KB se trata como 32 eventos, cada 1 KB de tamaño. El tamaño máximo permitido es 32 KB. Paquetes de datos mayores que 32 KB se truncan.

En la tabla siguiente se resume la capacidad de entrada por unidad para cada SKU de Time Series Insights:

|SKU  |Recuento de eventos al mes  |Tamaño del evento al mes  |Recuento de eventos por minuto  |Tamaño de los eventos por minuto  |
|---------|---------|---------|---------|---------|
|S1     |   30 millones     |  30 GB     |  720    |  720 KB   |
|S2     |   300 millones    |   < 300 GB   | 7200   | 7200 KB  |

Puede aumentar la capacidad de una SKU S1 o S2 a 10 unidades en un solo entorno. No puede migrar desde un entorno de S1 a S2. No puede migrar desde un entorno de S2 a uno S1.

Para la capacidad de entrada, determine primero la entrada total que requiere de forma mensual. A continuación, determine lo que necesita son su por minuto. 

Limitación y la latencia desempeñan un papel en la capacidad por minuto. Si tiene un pico en la entrada de datos que dura menos de 24 horas, Time Series Insights puede "detectar" en una velocidad de entrada de dos veces según las tarifas indicadas en la tabla anterior.

Por ejemplo, si tiene una sola SKU S1, que los datos de entrada a una velocidad de 720 eventos por minuto, y los picos de la velocidad de datos de menos de una hora a una velocidad de 1440 eventos o menos, no hay ninguna latencia apreciable en su entorno. Sin embargo, si se superan 1440 eventos por minuto durante más de una hora, es probable que experimentará una latencia en los datos que se visualizan y están disponibles para consulta en su entorno.

No puede saber de antemano cuántos datos espera insertar. En este caso, puede encontrar la telemetría de datos para [Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-metrics) y [Azure Event Hubs](https://blogs.msdn.microsoft.com/cloud_solution_architect/2016/05/25/using-the-azure-rest-apis-to-retrieve-event-hub-metrics/) en su suscripción de Azure portal. Los datos de telemetría pueden ayudar a determinar cómo aprovisionar el entorno. Use la **métricas** panel en el portal de Azure para el origen del evento correspondiente ver sus datos de telemetría. Si comprende las métricas de origen del evento, puede planea de manera más eficaz y aprovisionar el entorno de Time Series Insights.

### <a name="calculate-ingress-requirements"></a>Cálculo de los requisitos de entrada

Para calcular los requisitos de entrada:

- Compruebe que la capacidad de entrada está por encima de la media por minuto y que su entorno es suficientemente grande como para controlar la entrada anticipada equivalente a dos veces la capacidad durante menos de una hora.

- Si se producen picos de entrada que última durante más de 1 hora, usan la tasa de incremento como promedio. Aprovisione un entorno con la capacidad para controlar la velocidad de pico.

### <a name="mitigate-throttling-and-latency"></a>Mitigación de la limitación y la latencia

Para información sobre cómo prevenir la limitación y la latencia, consulte el artículo sobre la [mitigación de la limitación y la latencia](time-series-insights-environment-mitigate-latency.md).

## <a name="shape-your-events"></a>Moldear los eventos

Es importante para asegurarse de que el método de que envío de eventos de Time Series Insights es compatible con el tamaño del entorno que se va a aprovisionar. (A la inversa, puede asignar el tamaño del entorno para el número de eventos de Time Series Insights lee y el tamaño de cada evento.) También es importante pensar acerca de los atributos que desea usar para segmentar y filtrar por al consultar los datos.

> [!TIP]
> Revise el JSON dar forma a la documentación en [enviar eventos](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-send-events).

## <a name="ensure-that-you-have-reference-data"></a>Asegúrese de que tiene datos de referencia

Un *conjunto de datos de referencia* es una colección de elementos que aumentan los eventos del origen de eventos. El motor de entrada de Time Series Insights combina cada evento del origen de eventos con la fila de datos correspondiente en el conjunto de datos de referencia. El evento aumentado, a continuación, está disponible para la consulta. La combinación se basa en el **Primary Key** las columnas que se definen en el conjunto de datos de referencia.

> [!NOTE]
> Datos de referencia no se combinan de manera retroactiva. Solo los datos de entrada actuales y futuros es coincidente y unirse al conjunto de datos de referencia después de configurar y cargar. Si va a enviar una gran cantidad de datos históricos a Time Series Insights y no la primera carga o crear datos de referencia en Time Series Insights, deberá rehacer el trabajo (Sugerencia: no es agradable).  

Para obtener más información sobre cómo crear, cargar y administrar los datos de referencia en Time Series Insights, consulte nuestra [documentación del conjunto de datos de referencia](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-add-reference-data-set).

[!INCLUDE [business-disaster-recover](../../includes/time-series-insights-business-recovery.md)]

## <a name="next-steps"></a>Pasos siguientes

- Comience creando [un nuevo entorno de Time Series Insights en Azure portal](time-series-insights-get-started.md).

- Obtenga información sobre cómo [agregar un origen de eventos de Event Hubs](time-series-insights-how-to-add-an-event-source-eventhub.md) a Time Series Insights.

- Obtenga información sobre cómo [configurar un origen de eventos de IoT Hub](time-series-insights-how-to-add-an-event-source-iothub.md).
