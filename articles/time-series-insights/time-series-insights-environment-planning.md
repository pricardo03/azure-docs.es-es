---
title: Planeamiento de la escala del entorno Azure Time Series Insights | Microsoft Docs
description: En este artículo se describe cómo seguir los procedimientos recomendados cuando se planea un entorno de Azure Time Series Insights, incluida la capacidad de almacenamiento, la retención de datos, la capacidad de entrada, la supervisión y la recuperación ante desastres de la empresa (BCDR).
services: time-series-insights
ms.service: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 11/15/2017
ms.custom: seodec18
ms.openlocfilehash: f73f814321abbb75624ac18c9191c69a99cfe925
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2019
ms.locfileid: "64693571"
---
# <a name="plan-your-azure-time-series-insights-environment"></a>Planee el entorno de Azure Time Series Insights

En este artículo se describe cómo planear el entorno de Azure Time Series Insights según la velocidad de entrada esperada y los requisitos de retención de datos.

## <a name="video"></a>Vídeo

### <a name="in-this-video-we-cover-time-series-insights-data-retention-and-how-to-plan-for-itbr"></a>En este vídeo se explica la retención de datos de Time Series Insights y cómo planearla.</br>

> [!VIDEO https://www.youtube.com/embed/03x6zKDQ6DU]

## <a name="best-practices"></a>Procedimientos recomendados

Para empezar a trabajar con Time Series Insights, se recomienda saber cuántos datos espera insertar por minuto, así como también durante cuánto tiempo debe almacenar los datos.  

Para más información sobre la capacidad y la retención de ambas SKU de Time Series Insights, consulte los [precios de Time Series Insights](https://azure.microsoft.com/pricing/details/time-series-insights/).

Considere estos atributos para planear mejor el entorno para el éxito a largo plazo:

- Capacidad de almacenamiento
- Período de retención de datos
- Capacidad de entrada
- Eventos con forma
- Confirmación de la presencia de datos de referencia

## <a name="understand-storage-capacity"></a>Descripción de la capacidad de almacenamiento

De manera predeterminada, Time Series Insights conserva los datos en función de la cantidad de almacenamiento que aprovisionó (unidades por la cantidad de almacenamiento por unidad) y la entrada.

## <a name="understand-data-retention"></a>Descripción de la retención de los datos

Puede establecer la configuración del **tiempo de retención de datos** del entorno de Time Series Insights, que habilita hasta 400 días de retención.  Time Series Insights tiene dos modos, uno que optimiza el entorno para garantizar que tenga los datos más actualizados (opción que está activada de manera predeterminada) y otro que optimiza para garantizar que se cumplan los límites de retención, donde la entrada se pone en pausa si se alcanza la capacidad de almacenamiento general del entorno.  Puede ajustar la retención y alternar entre ambos modos en la página de configuración del entorno en Azure Portal.

Puede configurar un máximo de 400 días de retención de datos en el entorno de Time Series Insights.

## <a name="configure-data-retention"></a>Configuración de la retención de datos

1. En [Azure Portal](https://portal.azure.com), seleccione el entorno de Time Series Insights.

2. En la **página del entorno de Time Series Insights**, en el encabezado **Configuración**, seleccione **Configurar**. 

3. En el cuadro **Tiempo de retención de datos (en días)**, escriba un valor de 1 a 400.

   ![Configuración de la retención](media/environment-mitigate-latency/configure-retention.png)

## <a name="understand-ingress-capacity"></a>Descripción de la capacidad de entrada

La otra área de interés para el planeamiento es la capacidad de entrada, que deriva de la asignación por minuto. 

Desde la perspectiva de la limitación, un paquete de datos de entrada con un tamaño de 32 KB se trata como 32 eventos de 1 KB cada uno. El tamaño de evento máximo permitido es 32 KB. Los paquetes de datos con más de 32 KB se truncan.

En la tabla siguiente se resume la capacidad de ingreso de cada SKU:

|SKU  |Recuento de eventos por mes, por unidad  |Tamaño de eventos por mes, por unidad  |Recuento de eventos por minuto, por unidad  | Tamaño por minuto, por unidad   |
|---------|---------|---------|---------|---------|
|S1     |   30 millones     |  30 GB     |  720    |  720 KB   |
|S2     |   300 millones    |   < 300 GB   | 7200   | 7200 KB  |

Puede aumentar la capacidad de una SKU S1 o S2 a 10 unidades en un solo entorno. No puede migrar de un entorno S1 a uno S2 o desde un entorno S2 a uno S1.

Para la capacidad de entrada, primero debe determinar la entrada total que requiere de manera mensual. A continuación, determine cuáles son las necesidades por minuto, puesto que es aquí donde la limitación y la latencia desempeñan un papel.

Si tiene un incremento en la entrada de datos que dura menos de 24 horas, Time Series Insights puede "ponerse al día" con una velocidad de entrada que doble las velocidades ya mencionadas. 

Por ejemplo, si tiene una sola SKU S1 y datos de entrada a una velocidad de 720 eventos por minuto, y hay un incremento durante menos de una hora a una velocidad de 1440 eventos o menos, no habrá una latencia apreciable en el entorno. Pero si supera los 1440 eventos por minuto durante más de una hora, es probable que experimente una latencia respecto de los datos que se visualizan y están disponibles para consulta en el entorno. 

Es posible que no sepa de antemano cuántos datos espera insertar. En este caso, puede encontrar la telemetría de datos para [Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-metrics) y [Azure Event Hubs](https://blogs.msdn.microsoft.com/cloud_solution_architect/2016/05/25/using-the-azure-rest-apis-to-retrieve-event-hub-metrics/) en Azure Portal. Esta telemetría puede ayudar a determinar cómo aprovisionar el entorno. Use la página **Métricas** del origen del evento correspondiente en Azure Portal para ver su telemetría. Si comprende las métricas de origen del evento, puede planea de manera más eficaz y aprovisionar el entorno de Time Series Insights.

### <a name="calculate-ingress-requirements"></a>Cálculo de los requisitos de entrada

- Confirme que la capacidad de entrada está por encima de la velocidad promedio por minuto y que el entorno es suficientemente grande para controlar la entrada anticipada equivalente al doble de la capacidad durante menos de una hora.

- Si los incrementos de entrada se producen durante más de una hora, use la velocidad del incremento como promedio y aprovisione un entorno con la capacidad de controlar esta velocidad.

### <a name="mitigate-throttling-and-latency"></a>Mitigación de la limitación y la latencia

Para información sobre cómo prevenir la limitación y la latencia, consulte el artículo sobre la [mitigación de la limitación y la latencia](time-series-insights-environment-mitigate-latency.md).

## <a name="shaping-your-events"></a>Eventos con forma

Es importante asegurarse de que el método de envío de eventos a TSI es compatible con el tamaño del entorno que se aprovisiona (de otra manera, puede asignar el tamaño del entorno a la cantidad de eventos que TSI lea y al tamaño de cada evento).  Del mismo modo, es importante pensar en los atributos que desea segmentar y usar de filtro al consultar los datos.  Teniendo esto en cuenta, se recomienda revisar la sección sobre formas de JSON en nuestra [documentación sobre el envío de eventos](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-send-events).  Se encuentra en la parte inferior de la página.  

## <a name="ensuring-you-have-reference-data-in-place"></a>Confirmación de la presencia de datos de referencia

Un conjunto de datos de referencia es una colección de elementos que aumentan los eventos de un origen de eventos. El motor de entrada de Time Series Insights combina cada evento del origen de eventos con la fila de datos correspondiente en el conjunto de datos de referencia. A partir de ese momento, este evento aumentado está disponible para consultas. Esta combinación se basa en las columnas de clave principal definidas en el conjunto de datos de referencia.

Tenga en cuenta que los datos de referencia no se combinan de manera retroactiva. Esto significa que solo los datos de entrada actuales y futuros se combinan y unen con el conjunto de datos de referencia una vez que se configuran y cargan.  Si va a enviar una gran cantidad de datos históricos a TSI y no cargar ni crea primero datos de referencia en TSI, deberá volver a realizar el trabajo (sugerencia, esto no es agradable).  

Para más información sobre cómo crear, cargar y administrar los datos de referencia en TSI, eche un vistazo a nuestra [documentación sobre el conjunto de datos de referencia](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-add-reference-data-set).

## <a name="business-disaster-recovery"></a>Recuperación ante desastres de la empresa

Como servicio de Azure, Time Series Insights proporciona alta disponibilidad (HA) mediante redundancias en el nivel de región de Azure, sin que la solución requiera ningún trabajo adicional. La plataforma Microsoft Azure también incluye características para ayudarle a crear soluciones con funcionalidades de recuperación ante desastres o disponibilidad entre regiones. Si desea proporcionar alta disponibilidad global entre regiones para dispositivos o usuarios, aproveche estas características de recuperación ante desastres de Azure. En el artículo [Guía técnica sobre la continuidad empresarial de Azure](../resiliency/resiliency-technical-guidance.md) , se describen las características integradas en Azure para la continuidad empresarial y la recuperación ante desastres. El documento [Consideraciones sobre la alta disponibilidad y la recuperación ante desastres para las aplicaciones de Azure](https://docs.microsoft.com/azure/architecture/resiliency/index) proporciona una guía de arquitectura enfocada a estrategias para que las aplicaciones de Azure logren alta disponibilidad y recuperación ante desastres.

Azure Time Series Insights no tiene integrada la recuperación ante desastres de la empresa (BCDR).
De forma predeterminada, Azure IoT Hub y Event Hubs tienen recuperación integrada.

Para obtener más información sobre las directivas de BCDR de IoT Hub, vaya [aquí](https://docs.microsoft.com/azure/iot-hub/iot-hub-ha-dr).  

Para obtener más información sobre las directivas de BCDR de Event Hub, vaya [aquí](https://docs.microsoft.com/azure/event-hubs/event-hubs-geo-dr).

Pero los clientes que necesiten la característica BCDR pueden implementar una estrategia de recuperación con este método:
crear un segundo entorno de Time Series Insights en una copia de seguridad de la región de Azure y enviar eventos a este segundo entorno desde el origen de eventos principal, usando un segundo grupo de consumidores dedicados y las directrices de BCDR del origen de eventos.  

1. Cree un entorno en la segunda región.  Encontrará más información sobre la creación de un entorno de Time Series Insights [aquí](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-get-started).
1. Cree un segundo grupo de consumidores dedicados para el origen de eventos y conecte ese origen de eventos al nuevo entorno.  Asegúrese de designar el segundo grupo de consumidores dedicados.  Encontrará más información al respecto, en la [documentación de IoT Hub](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-how-to-add-an-event-source-iothub) o la [documentación de Event Hub](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-data-access).
1. Si la región principal queda inactivada durante un incidente de desastre, cambie las operaciones al entorno de copia de seguridad de Time Series Insights.  

Es **importante tener en cuenta** durante cualquier escenario de conmutación por error que puede haber un retraso antes de que TSI pueda empezar a procesar mensajes de nuevo: esto puede producir un pico en el procesamiento de mensajes. Para más información, eche un vistazo a [este documento](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-environment-mitigate-latency)

## <a name="next-steps"></a>Pasos siguientes

- Obtenga información sobre cómo [agregar un origen de eventos del centro de eventos](time-series-insights-how-to-add-an-event-source-eventhub.md) a Time Series Insights.

- Obtenga información sobre cómo [configurar un origen de eventos de IoT Hub](time-series-insights-how-to-add-an-event-source-iothub.md).