---
title: archivo de inclusión
description: archivo de inclusión
ms.topic: include
ms.custom: include file
services: time-series-insights
ms.service: time-series-insights
author: kingdomofends
ms.author: adgera
ms.date: 04/29/2019
ms.openlocfilehash: e3e7044148e262e6977ae3be96f7cb61218114a3
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/30/2019
ms.locfileid: "66388736"
---
## <a name="business-disaster-recovery"></a>Recuperación ante desastres de la empresa

Esta sección describen las características de Azure Time Series Insights que mantener las aplicaciones y servicios que se ejecutan aunque se produzca un desastre. Este concepto se conoce como recuperación ante desastres de negocio.

### <a name="high-availability"></a>Alta disponibilidad

Como un servicio de Azure Time Series Insights proporciona determinadas características de alta disponibilidad mediante redundancias en el nivel de región de Azure. Por ejemplo, Microsoft Azure admite funcionalidades de recuperación ante desastres a través de la característica de disponibilidad entre regiones de Azure.

Características de alta disponibilidad adicionales proporcionadas a través de Azure y que también están disponibles para cualquier instancia de Time Series Insights, incluya:

* **Conmutación por error**: Azure proporciona [equilibrio de carga y la replicación geográfica](https://docs.microsoft.com/azure/architecture/resiliency/recovery-loss-azure-region).
* **Restauración de datos** y **recuperación almacenamiento**: Azure proporciona [varias opciones para conservar y recuperar datos](https://docs.microsoft.com/azure/architecture/resiliency/recovery-data-corruption).
* **Recuperación de sitio**: Características que están disponibles a través de [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/).

Asegúrese de habilitar estas características de Azure proporcionar alta disponibilidad global entre regiones para los dispositivos y usuarios.

> [!NOTE]
> Si Azure está configurado para habilitar la disponibilidad entre regiones, ninguna configuración adicional disponibilidad entre regiones es necesaria dentro de Azure Time Series Insights.

### <a name="iot-and-event-hubs"></a>IoT y event hubs

Algunos servicios IoT de Azure también incluyen características de recuperación ante desastres de negocios integrados:

* [Recuperación de desastres de alta disponibilidad de Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-ha-dr), que incluye redundancia dentro de una región.
* [Las directivas de Azure Event Hubs](https://docs.microsoft.com/azure/event-hubs/event-hubs-geo-dr).
* [Redundancia de Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-redundancy).

Integración con estos otros servicios de Time Series Insights proporciona ante desastres más oportunidades de recuperación. Por ejemplo, los datos de telemetría enviados al centro de eventos pueden conservarse en una copia de seguridad de base de datos de almacenamiento de blobs de Azure.

### <a name="time-series-insights"></a>Time Series Insights

Hay varias maneras de mantener sus datos de Time Series Insights, aplicaciones y servicios en ejecución incluso si le interrumpe. También puede determinar que se requiere una copia de seguridad completa de su entorno Azure Time Series:

* Como una instancia de conmutación por error de Time Series Insights específica para redirigir los datos y al tráfico.
* Para fines de conservación de la auditoría y los datos.

En general, la mejor manera para duplicar un entorno de Time Series Insights es crear un segundo entorno de Time Series Insights en una región de Azure de la copia de seguridad. Los eventos también se envían a este entorno secundario del origen de eventos principal. Asegúrese de usar un grupo de consumidores dedicado en segundo lugar y seguir negocio ante desastres directrices para la recuperación de ese origen, que como ya se ha proporcionado.

En concreto, para crear un entorno duplicado:

1. Cree un entorno en una segunda región. Para obtener instrucciones, consulte [crear un nuevo entorno de Time Series Insights en Azure portal](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-get-started).
1. Cree un segundo grupo de consumidores dedicado para el origen de eventos.
1. Conecte ese origen de eventos al nuevo entorno. Asegúrese de que designar el segundo grupo de consumidores dedicado.
1. Revise el valor de la serie temporal [IoT hub](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-how-to-add-an-event-source-iothub) y [centro de eventos](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-data-access) documentación.

Por último:

1. Si la región primaria resulta afectada durante un incidente de desastre, vuelva a enrutar las operaciones al entorno de copia de Time Series Insights de respaldo.
1. Use la segunda región para realizar copias de seguridad y recuperar todos los datos de telemetría y consulta de Time Series Insights.

> [!IMPORTANT]
> * Tenga en cuenta que puede experimentarse un retraso en caso de una conmutación por error.
> * Conmutación por error también podría producir un pico momentáneo en que se vuelven a enrutar las operaciones de procesamiento de mensajes.
> * Para más información, consulte [Mitigación de la latencia en Time Series Insights](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-environment-mitigate-latency).
