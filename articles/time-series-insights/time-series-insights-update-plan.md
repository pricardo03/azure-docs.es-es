---
title: Planeamiento del entorno de versión preliminar de Azure Time Series Insights | Microsoft Docs
description: Planee el entorno de versión preliminar de Azure Time Series Insights.
author: ashannon7
ms.author: dpalled
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 09/24/2019
ms.custom: seodec18
ms.openlocfilehash: b97db5fcebeea67cc593a4d2c1fd677a55ad8559
ms.sourcegitcommit: ae461c90cada1231f496bf442ee0c4dcdb6396bc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2019
ms.locfileid: "72550179"
---
# <a name="plan-your-azure-time-series-insights-preview-environment"></a>Planeamiento del entorno de versión preliminar de Azure Time Series Insights

En este artículo se describen los procedimientos recomendados para planear y comenzar a usar rápidamente la versión preliminar de Azure Time Series Insights.

> [!NOTE]
> Para conocer los procedimientos recomendados para planear una instancia de Time Series Insights de disponibilidad general, consulte [Plan your Azure Time Series Insights general availability environment](time-series-insights-environment-planning.md) (Planear el entorno de disponibilidad general de Azure Time Series Insights).

## <a name="best-practices-for-planning-and-preparation"></a>Procedimientos recomendados para el planeamiento y la preparación

Para empezar a trabajar con Time Series Insights, es importante que comprenda:

* Lo que se obtiene cuando se [aprovisiona un entorno de versión preliminar de Time Series Insights](#the-preview-environment).
* Cuáles son las [propiedades IDs y Timestamp de Times Series](#configure-time-series-ids-and-timestamp-properties).
* Cuál es el nuevo [modelo de Times Series](#understand-the-time-series-model) y cómo crear el suyo.
* Cómo [enviar eventos de forma eficaz en JSON](#shape-your-events).
* [Opciones comerciales de recuperación ante desastres](#business-disaster-recovery) de Time Series Insights.

Azure Time Series Insights emplea un modelo comercial de pago por uso. Para más información sobre la capacidad y los cargos, consulte [Precios de Time Series Insights](https://azure.microsoft.com/pricing/details/time-series-insights/).

## <a name="the-preview-environment"></a>El entorno de versión preliminar

Al aprovisionar un entorno de versión preliminar de Time Series Insights, creará dos recursos de Azure:

* Un entorno de la versión preliminar de Azure Time Series Insights.
* Una cuenta de uso general V1 de Azure Storage.

Para empezar, necesita tres elementos adicionales:

* Un [modelo de Time Series](./time-series-insights-update-tsm.md)
* Un [origen de eventos conectado a Time Series Insights](./time-series-insights-how-to-add-an-event-source-iothub.md)
* [Eventos que fluyen hacia el origen del evento](./time-series-insights-send-events.md) que se asignan al modelo y están en formato JSON válido

## <a name="configure-time-series-ids-and-timestamp-properties"></a>Configurar las propiedades IDs y Timestamp de Time Series

Para crear un entorno de Time Series Insights, seleccione un identificador de Time Series. Al hacer esto se crea una partición lógica para los datos. Como se indicó, asegúrese de tener listos los identificadores de serie temporal.

> [!IMPORTANT]
> Los identificadores de serie temporal *no se pueden cambiar más adelante*. Compruebe cada uno de ellos antes de pasar a la selección final y el primer uso.

Puede seleccionar hasta tres claves para diferenciar los recursos de forma única. Para más información, lea [Best practices for choosing a Time Series ID](./time-series-insights-update-how-to-id.md) (Procedimientos recomendados para elegir un identificador de Time Series) y [Storage and ingress](./time-series-insights-update-storage-ingress.md) (Almacenamiento y entrada).

La propiedad Timestamp también es importante. Esta propiedad se puede designar al agregar orígenes de eventos. Cada origen de eventos tiene una propiedad Timestamp opcional que se usa para realizar el seguimiento de los orígenes de eventos con el tiempo. Los valores de Timestamp distinguen mayúsculas de minúsculas y su formato debe seguir la especificación de cada origen de evento.

> [!TIP]
> Compruebe los requisitos de formato y análisis de los orígenes de eventos.

Si se deja en blanco, la hora de puesta en cola del evento se usa como marca de tiempo del evento. Si envía datos históricos o eventos por lotes, personalizar la propiedad Timestamp es más útil que la hora predeterminada de puesta en cola del evento. Para más información, lea acerca de cómo [agregar orígenes de eventos en Azure IoT Hub](./time-series-insights-how-to-add-an-event-source-iothub.md).

## <a name="understand-the-time-series-model"></a>El modelo de serie temporal

Ahora puede configurar el modelo de serie temporal del entorno de Time Series Insights. El nuevo modelo facilita la búsqueda y el análisis de los datos de IoT. Además, permite la protección, el mantenimiento y el enriquecimiento de datos de serie temporal y ayuda a preparar los conjuntos de datos listos para el consumidor. El modelo usa identificadores de Time Series, que se asignan a una instancia que asocia el recurso único con variables, conocidas como tipos, y jerarquías. Lea sobre el nuevo [modelo de serie temporal](./time-series-insights-update-tsm.md).

El modelo es dinámico, por lo que se puede compilar en cualquier momento. Para empezar a trabajar rápidamente, compílelo y cárguelo antes de insertar datos en Time Series Insights. Para compilar el modelo, consulte [Use the Time Series Model](./time-series-insights-update-how-to-tsm.md) (Uso del modelo de serie temporal).

Para muchos clientes, el modelo de serie temporal se asigna a un modelo de recursos existente o a un sistema ERP ya implantado. Si no tiene un modelo existente, se [proporciona](https://github.com/Microsoft/tsiclient) una experiencia de usuario ya creada para ponerse en marcha rápidamente. Para saber cómo puede ayudarle un modelo, vea el [entorno de demostración de ejemplo](https://insights.timeseries.azure.com/preview/demo).

## <a name="shape-your-events"></a>Moldear los eventos

Puede comprobar el modo en que los eventos se envían a Time Series Insights. Lo ideal es que los eventos se desnormalicen bien y de manera eficaz.

Una buena regla general es la siguiente:

* Almacenar los metadatos en el modelo de Time Series.
* El modo de Time Series, los campos de instancia y los eventos incluyen únicamente la información necesaria, como los identificadores y las marcas de tiempo de Time Series.

Para más información, consulte [Moldear los eventos](./time-series-insights-send-events.md#supported-json-shapes).

[!INCLUDE [business-disaster-recover](../../includes/time-series-insights-business-recovery.md)]

## <a name="next-steps"></a>Pasos siguientes

- Revise [Azure Advisor](../advisor/advisor-overview.md) para planear las opciones de configuración de recuperación de negocio.

- Lea más sobre [el almacenamiento y la entrada](./time-series-insights-update-storage-ingress.md) en la versión preliminar de Time Series Insights.

- Lea más sobre [el modelo de datos](./time-series-insights-update-tsm.md) en la versión preliminar de Time Series Insights.