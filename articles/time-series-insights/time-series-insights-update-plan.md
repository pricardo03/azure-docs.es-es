---
title: 'Planeamiento de la versión preliminar de Azure Time Series Insights: planeamiento del entorno de versión preliminar de Azure Time Series Insights | Microsoft Docs'
description: Planee el entorno de versión preliminar de Azure Time Series Insights.
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 12/03/2018
ms.custom: seodec18
ms.openlocfilehash: 1df4847f20329e924352adfe782faa43d10dde98
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2018
ms.locfileid: "53277053"
---
# <a name="plan-your-azure-time-series-insights-preview-environment"></a>Planeamiento del entorno de versión preliminar de Azure Time Series Insights

En este artículo se describen los procedimientos recomendados para planear y comenzar a usar rápidamente la versión preliminar de Azure Time Series Insights.

## <a name="best-practices-for-planning-and-preparation"></a>Procedimientos recomendados para el planeamiento y la preparación

Para empezar a trabajar con Time Series Insights, es importante que comprenda:

* Lo que se obtiene cuando se aprovisiona un entorno de versión preliminar de Time Series Insights.
* Cuáles son las propiedades IDs y Timestamp de Times Series.
* Cuál es el nuevo modelo de Times Series y cómo crear el suyo propio.
* Cómo enviar eventos de forma eficaz en JSON. 
* Opciones comerciales de recuperación ante desastres de Time Series Insights.

Time Series Insights emplea un modelo comercial de pago por uso. Para más información sobre la capacidad y los cargos, consulte [Precios de Time Series Insights](https://azure.microsoft.com/pricing/details/time-series-insights/).

## <a name="the-time-series-insights-preview-environment"></a>Entorno de versión preliminar de Time Series Insights

Al aprovisionar un entorno de versión preliminar de Time Series Insights, creará dos recursos de Azure:

* Entorno de versión preliminar de Time Series Insights
* Cuenta de uso general V1 de Azure Storage

Para empezar, necesita tres elementos adicionales:
 
- Un [modelo de Time Series](./time-series-insights-update-tsm.md) 
- Un [origen de eventos conectado a Time Series Insights](./time-series-insights-how-to-add-an-event-source-iothub.md) 
- [Eventos que fluyen hacia el origen del evento](./time-series-insights-send-events.md) que se asignan al modelo y están en formato JSON válido 

## <a name="configure-your-time-series-ids-and-timestamp-properties"></a>Configuración de las propiedades IDs y Timestamp de Time Series

Para crear un entorno de Time Series Insights, seleccione un identificador de Time Series. Al hacer esto se crea una partición lógica para los datos. Como se indicó, asegúrese de tener listos los identificadores de serie temporal.

> [!IMPORTANT]
> Los identificadores de serie temporal son *inmutables* y *no se pueden cambiar más adelante*. Compruebe cada uno de ellos antes de pasar a la selección final y el primer uso.

Puede seleccionar hasta tres claves (3) para diferenciar los recursos de forma única. Para más información, lea [Best practices for choosing a Time Series ID](./time-series-insights-update-how-to-id.md) (Procedimientos recomendados para elegir un identificador de Time Series) y [Storage and ingress](./time-series-insights-update-storage-ingress.md) (Almacenamiento y entrada).

La propiedad Timestamp también es importante. Esta propiedad se puede designar al agregar orígenes de eventos. Cada origen de eventos tiene una propiedad Timestamp opcional que se usa para realizar el seguimiento de los orígenes de eventos con el tiempo. Los valores de Timestamp distinguen mayúsculas de minúsculas y su formato debe seguir la especificación de cada origen de evento.

> [!TIP]
> Compruebe los requisitos de formato y análisis de los orígenes de eventos.

Si se deja en blanco, la hora de puesta en cola del evento se usa como marca de tiempo del evento. Si envía datos históricos o eventos por lotes, personalizar la propiedad Timestamp es más útil que la hora predeterminada de puesta en cola del evento. Para más información, lea acerca de [cómo agregar orígenes de eventos en IoT Hub](./time-series-insights-how-to-add-an-event-source-iothub.md). 

## <a name="understand-the-time-series-model"></a>El modelo de serie temporal

Ahora puede configurar el modelo de serie temporal del entorno de Time Series Insights. El nuevo modelo facilita la búsqueda y el análisis de los datos de IoT. Además, permite la protección, el mantenimiento y el enriquecimiento de datos de serie temporal y ayuda a preparar los conjuntos de datos listos para el consumidor. El modelo usa **identificadores de serie temporal**, que se asignan a una instancia que asocia el recurso único con variables, conocidas como tipos, y jerarquías. Lea sobre el nuevo [modelo de serie temporal](./time-series-insights-update-tsm.md).

El modelo es dinámico, por lo que se puede compilar en cualquier momento. Para empezar a trabajar rápidamente, compílelo y cárguelo antes de insertar datos en Time Series Insights. Para compilar el modelo, consulte [Use the Time Series Model](./time-series-insights-update-how-to-tsm.md) (Uso del modelo de serie temporal).

Para muchos clientes, el modelo de serie temporal se asigna a un modelo de recursos existente o a un sistema ERP ya implantado. Si no tiene un modelo existente, se [proporciona](https://github.com/Microsoft/tsiclient) una experiencia de usuario ya creada para ponerse en marcha rápidamente. Para saber cómo puede ayudarle un modelo, vea el [entorno de demostración de ejemplo](https://insights.timeseries.azure.com/preview/demo). 

## <a name="shape-your-events"></a>Moldear los eventos

Puede comprobar el modo en que los eventos se envían a Time Series Insights. Lo ideal es que los eventos se desnormalicen bien y de manera eficaz.

Una buena regla general es la siguiente:

* Almacenar los metadatos en el modelo de serie temporal
* El modo de serie temporal, los campos de instancia y los eventos incluyen únicamente la información necesaria, como:
  * El identificador de serie temporal
  * Timestamp

Para más información, consulte [Moldear los eventos](./time-series-insights-send-events.md#json).

## <a name="business-disaster-recovery"></a>Recuperación ante desastres de la empresa

Time Series Insights es un servicio de alta disponibilidad que usa redundancias en el nivel de región de Azure. No es necesario configurar nada para usar estas funcionalidades inherentes. La plataforma Microsoft Azure también incluye características para ayudarle a crear soluciones con funcionalidades de recuperación ante desastres o disponibilidad entre regiones. Para proporcionar alta disponibilidad global entre regiones para dispositivos o usuarios, aproveche estas características de recuperación ante desastres de Azure. 

Para información sobre las características integradas de continuidad empresarial y recuperación ante desastres (BCDR) de Azure, consulte la [guía técnica de la continuidad empresarial de Azure](https://docs.microsoft.com/azure/resiliency/resiliency-technical-guidance). El documento [Recuperación ante desastres y alta disponibilidad para aplicaciones de Azure](https://docs.microsoft.com/azure/architecture/resiliency/index) proporciona una guía de arquitectura enfocada a estrategias para que las aplicaciones de Azure logren alta disponibilidad y recuperación ante desastres.

> [!NOTE]

>  Time Series Insights no incorpora BCDR.
> De forma predeterminada, Azure Storage, Azure IoT Hub y Azure Event Hubs tienen recuperación integrada.

Para más información, lea sobre:

* [Redundancia de Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-redundancy)
* [Alta disponibilidad y recuperación ante desastres de IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-ha-dr)
* [Directivas del centro de eventos](https://docs.microsoft.com/azure/event-hubs/event-hubs-geo-dr)

Si necesita BCDR, puede implementar una estrategia de recuperación. Cree un segundo entorno de Time Series Insights en una región de Azure de respaldo. Envíe eventos a este entorno secundario desde el origen de eventos principal. Use un segundo grupo de consumidores dedicado y las directrices de BCDR de ese origen de eventos.

Siga estos pasos para crear y usar un entorno de Time Series Insights secundario.

1. Cree un entorno en una segunda región. Para más información, consulte [Entornos de Time Series Insights](./time-series-insights-get-started.md).
1. Cree un segundo grupo de consumidores dedicado para el origen de eventos. Conecte ese origen de eventos al nuevo entorno. Asegúrese de designar el segundo grupo de consumidores dedicado. Para más información, consulte la [documentación de IoT Hub](./time-series-insights-how-to-add-an-event-source-iothub.md) o la [documentación del centro de eventos](./time-series-insights-data-access.md).
1. Si la región primaria resulta afectada durante un incidente de desastre, vuelva a enrutar las operaciones al entorno de copia de Time Series Insights de respaldo.

> [!IMPORTANT]
> * Tenga en cuenta que puede experimentarse un retraso en caso de una conmutación por error.
> * La conmutación por error también podría provocar un pico momentáneo en el procesamiento de los mensajes dado que las operaciones se vuelven a enrutar.
> * Para más información, consulte [Mitigación de la latencia en Time Series Insights](./time-series-insights-environment-mitigate-latency.md).

## <a name="next-steps"></a>Pasos siguientes

Para más información, lea sobre:

- [Almacenamiento y entrada de datos en la versión preliminar de Azure Time Series Insights](./time-series-insights-update-storage-ingress.md)
- [Modelado de datos](./time-series-insights-update-tsm.md)