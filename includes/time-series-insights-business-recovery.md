---
ms.topic: include
ms.service: time-series-insights
author: kingdomofends
ms.author: adgera
ms.date: 04/29/2019
ms.openlocfilehash: 8a3c630b54ff95a9b1200e2421c787a514a0aa52
ms.sourcegitcommit: 087ee51483b7180f9e897431e83f37b08ec890ae
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66431036"
---
## <a name="business-disaster-recovery"></a>Recuperación ante desastres de la empresa

Esta sección describen las características de Azure Time Series Insights que mantener las aplicaciones y servicios que se ejecutan, incluso si se produce un desastre (conocido como *negocio recuperación ante desastres*).

### <a name="high-availability"></a>Alta disponibilidad

Como un servicio de Azure Time Series Insights proporciona ciertos *alta disponibilidad* características con redundancias en el nivel de región de Azure. Por ejemplo, Azure admite funcionalidades de recuperación ante desastres a través de Azure *disponibilidad entre regiones* característica.

Características de alta disponibilidad adicionales proporcionadas a través de Azure (y también está disponible en cualquier instancia de Time Series Insights) incluyen:

- **Conmutación por error**: Azure proporciona [equilibrio de carga y la replicación geográfica](https://docs.microsoft.com/azure/architecture/resiliency/recovery-loss-azure-region).
- **Restauración de datos** y **recuperación almacenamiento**: Azure proporciona [varias opciones para conservar y recuperar datos](https://docs.microsoft.com/azure/architecture/resiliency/recovery-data-corruption).
- **Recuperación de sitio**: Azure proporciona características de recuperación de sitios a través de [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/).

Asegúrese de que habilitar las características de Azure relevantes proporcionar alta disponibilidad global entre regiones para los dispositivos y usuarios.

> [!NOTE]
> Si Azure está configurado para habilitar la disponibilidad entre regiones, no se requiere ninguna configuración adicional disponibilidad entre regiones en Azure Time Series Insights.

### <a name="iot-and-event-hubs"></a>IoT y event hubs

Algunos servicios IoT de Azure también incluyen características de recuperación ante desastres de negocios integrados:

- [Recuperación ante desastres de alta disponibilidad de IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-ha-dr), incluida la redundancia entre regiones
- [Directivas de Event Hubs](https://docs.microsoft.com/azure/event-hubs/event-hubs-geo-dr)
- [Redundancia de Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-redundancy)

La integración de Time Series Insights con los demás servicios proporciona ante desastres más oportunidades de recuperación. Por ejemplo, los datos de telemetría enviados al centro de eventos pueden conservarse en una copia de seguridad de base de datos de almacenamiento de blobs de Azure.

### <a name="time-series-insights"></a>Time Series Insights

Hay varias maneras de mantener los datos de Time Series Insights, aplicaciones y servicios que se ejecutan, incluso si le interrumpe. 

Sin embargo, puede determinar que una copia de seguridad completa de su entorno Azure Time Series también es necesaria, para lo siguiente:

- Como un *instancia de conmutación por error* específicamente para Time Series Insights redirigir los datos y al tráfico
- Para conservar los datos e información de auditoría

En general, la mejor manera para duplicar un entorno de Time Series Insights es crear un segundo entorno de Time Series Insights en una región de Azure de la copia de seguridad. Los eventos también se envían a este entorno secundario del origen de eventos principal. Asegúrese de que usar un grupo de consumidores en segundo lugar, dedicado. Siga las pautas de recuperación ante desastres del negocio de ese origen, como se describió anteriormente.

Para crear un entorno duplicado:

1. Cree un entorno en una segunda región. Para obtener más información, consulte [crear un nuevo entorno de Time Series Insights en Azure portal](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-get-started).
1. Cree un segundo grupo de consumidores dedicado para el origen de eventos.
1. Conecte ese origen de eventos al nuevo entorno. Asegúrese de que se designa el grupo de consumidores dedicado, segundo.
1. Revise el valor de la serie temporal [IoT Hub](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-how-to-add-an-event-source-iothub) y [Event Hubs](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-data-access) documentación.

Si se produce un evento:

1. Si la región primaria resulta afectada durante un incidente de desastre, vuelva a enrutar las operaciones al entorno de copia de Time Series Insights de respaldo.
1. Use la segunda región para realizar copias de seguridad y recuperar todos los datos de telemetría y consulta de Time Series Insights.

> [!IMPORTANT]
> Si se produce una conmutación por error:
> 
> * También puede producirse un retraso.
> * Podría producirse un pico momentáneo de procesamiento de mensajes, como las operaciones se vuelven a enrutar.
> 
> Para más información, consulte [Mitigación de la latencia en Time Series Insights](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-environment-mitigate-latency).

