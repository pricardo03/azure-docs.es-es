---
title: Planeamiento del entorno de versión preliminar de Azure Time Series Insights | Microsoft Docs
description: Planee el entorno de versión preliminar de Azure Time Series Insights.
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 04/30/2019
ms.custom: seodec18
ms.openlocfilehash: b3fab86b2b2f0ad892e02cd089dbd7c45ce601d6
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65205768"
---
# <a name="plan-your-azure-time-series-insights-preview-environment"></a>Planeamiento del entorno de versión preliminar de Azure Time Series Insights

En este artículo se describen los procedimientos recomendados para planear y comenzar a usar rápidamente la versión preliminar de Azure Time Series Insights.

> [!NOTE]
> Consulte [planear su entorno de Azure tiempo Series Insights GA](time-series-insights-environment-planning.md), los procedimientos recomendados planear una instancia TSI de la disponibilidad general.

## <a name="best-practices-for-planning-and-preparation"></a>Procedimientos recomendados para el planeamiento y la preparación

Para empezar a trabajar con Time Series Insights, es importante que comprenda:

* Lo que se obtiene cuando se [aprovisionar un entorno de tiempo de vista previa de la información de serie](#the-preview-environment).
* Lo que su [las propiedades de los identificadores de serie de tiempo y marca de tiempo son](#configure-time-series-ids-and-timestamp-properties).
* La nueva [es el modelo de serie temporal](#understand-the-time-series-model)y cómo crear las suyas propias.
* Cómo [enviar eventos de forma eficaz en JSON](#shape-your-events).
* Time Series Insights [opciones de recuperación ante desastres de negocio](#business-disaster-recovery).

Azure Time Series Insights emplea un modelo de pago por uso empresarial. Para más información sobre la capacidad y los cargos, consulte [Precios de Time Series Insights](https://azure.microsoft.com/pricing/details/time-series-insights/).

## <a name="the-preview-environment"></a>El entorno de versión preliminar

Al aprovisionar un entorno de versión preliminar de Time Series Insights, creará dos recursos de Azure:

* Un entorno de la versión preliminar de Azure Time Series Insights.
* Una cuenta de uso general V1 de Azure Storage.

Para empezar, necesita tres elementos adicionales:

* Un [modelo de serie temporal](./time-series-insights-update-tsm.md).
* Un [origen de eventos conectado a Time Series Insights](./time-series-insights-how-to-add-an-event-source-iothub.md).
* [Eventos que fluyen hacia el origen del evento](./time-series-insights-send-events.md) que se asignan al modelo y están en formato JSON válido.

## <a name="configure-time-series-ids-and-timestamp-properties"></a>Configurar las propiedades de los identificadores de serie de tiempo y marca de tiempo

Para crear un nuevo entorno de Time Series Insights, seleccione un **Id. de serie de tiempo**. Al hacer esto se crea una partición lógica para los datos. Como se indicó, asegúrese de tener listos los identificadores de serie temporal.

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
* Modo de Series de tiempo, los campos de instancia y eventos incluyen solo la información necesaria, como un **Id. de serie de tiempo** o **Timestamp**.

Para más información, consulte [Moldear los eventos](./time-series-insights-send-events.md#json).

[!INCLUDE [business-disaster-recover](../../includes/time-series-insights-business-recovery.md)]

## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre [almacenamiento y la entrada](./time-series-insights-update-storage-ingress.md) en la vista previa en tiempo Series Insights.

- Obtenga información sobre [modelado de datos](./time-series-insights-update-tsm.md) en la vista previa en tiempo Series Insights.