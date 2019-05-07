---
title: 'Introducción a los centros de eventos dedicados: Azure Event Hubs | Microsoft Docs'
description: En este artículo se proporciona información general de Azure Event Hubs dedicado, lo que ofrece implementaciones de un único inquilino de event hubs.
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: e8a2d8321a42e8b3d090c1ce1fdb3fd9a7ee3714
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65138654"
---
# <a name="overview-of-event-hubs-dedicated"></a>Introducción a Event Hubs dedicado

Los clústeres de Azure Event Hubs ofrecen implementaciones de inquilino único para los clientes con las necesidades más exigentes de transmisión por secuencias. Esta oferta de inquilino único tiene un SLA del 99,99% garantizado. Está disponible solo en el plan de tarifa de dedicado.

Un clúster de Event Hubs puede incorporar millones de eventos por segundo con capacidad garantizada y la latencia de subsegundo. Espacios de nombres y event hubs creados dentro del clúster dedicado incluyen todas las características de la oferta estándar y mucho más, pero sin los límites de entrada. También incluye el [Event Hubs Capture](event-hubs-capture-overview.md) característica sin costo adicional. Puede usarlo para automáticamente por lotes y el registro de flujos de datos en Azure Storage o Azure Data Lake.

Clústeres dedicados se aprovisionan y se factura por unidades de capacidad (Cu). Cu es una cantidad de recursos de CPU y memoria asignado previamente. Puede comprar 1, 2, 4, 8, 12, 16 o 20 CUs para cada clúster. ¿Cuánto puede ingerir y secuencia por unidad de capacidad depende de factores como el número de productores y consumidores, la forma de carga y la tasa de salida.

Para obtener más información, vea la tabla con los resultados de pruebas comparativas.

## <a name="why-use-event-hubs-dedicated"></a>¿Por qué usar Event Hubs dedicado?

Event Hubs dedicado ofrece tres ventajas para clientes que necesitan la capacidad de nivel empresarial.

#### <a name="single-tenancy-guarantees-capacity-for-better-performance"></a>Multiniiquilino a inquilino único garantiza la capacidad para mejorar el rendimiento

Un clúster dedicado garantiza la capacidad a escala completa. Puede incorporar hasta gigabytes de datos de streaming con una latencia de almacenamiento y subsegundo totalmente durable para dar cabida a picos de tráfico.

#### <a name="inclusive-and-exclusive-access-to-features"></a>Acceso inclusivo y exclusivo a las características 
La oferta dedicada incluye características como captura sin costo adicional. También ofrece acceso exclusivo a las próximas características como BYOK. El servicio también administra el equilibrio de carga del sistema operativo actualizaciones, las revisiones de seguridad y creación de particiones. Con estas capacidades, puede dedicar menos tiempo en el mantenimiento de la infraestructura y más tiempo en crear características de cliente.

#### <a name="cost-savings"></a>Ahorros en costos
Volúmenes de alta de entrada, un clúster los costos mucho menor por hora que if adquiere una cantidad de unidades de procesamiento (tu) comparable en la oferta estándar. Es un gran volumen > 100 tu.


## <a name="event-hubs-standard-vs-dedicated"></a>Event Hubs estándar frente a Dedicado

En la tabla siguiente se comparan niveles de servicio disponibles de Event Hubs. La oferta de Event Hubs dedicado se factura por un precio mensual fijo frente a los precios de uso para la mayoría de las características del estándar. El nivel dedicado ofrece todas las características del plan estándar, pero con capacidad de escala empresarial para los clientes con cargas de trabajo exigentes.

| Característica | Estándar | Dedicado |
| --- |:---:|:---:|
| Eventos de entrada | Pago por millones de eventos | Se incluye |
| Unidad de rendimiento (entrada de 1 MB/seg., salida de 2 MB/seg.) | Pago por hora | Se incluye |
| Tamaño del mensaje | 1 MB | 1 MB |
| Particiones | 40 por espacio de nombres | 2.000 por unidad de capacidad |
| Grupos de consumidores | 20 por centro de eventos | 1.000 por centro de eventos |
| Ancho de banda máximo | 20 tu (hasta 40 tu) | 20 CUs |
| Conexiones asincrónicas | 1000 incluidos | 100 000 incluidos |
| Retención de mensajes | Un día incluido | Incluidos hasta siete días |
| Capture | Pago por hora | Se incluye |

## <a name="what-can-i-achieve-with-a-cluster"></a>¿Qué puedo lograr con un clúster?

Para un clúster de Event Hubs, ¿cuánto tiempo puede ingerir y transmitir depende de los productores, los consumidores, la velocidad a la que introducen y procesan y mucho más.

La siguiente tabla muestra los resultados de pruebas comparativas que se han logrado durante las pruebas.

| Forma de la carga | Receptores | Ancho de banda de entrada| Mensajes de entrada | Ancho de banda de salida | Mensajes de salida | TU totales | TU por CU |
| ------------- | --------- | ---------------- | ------------------ | ----------------- | ------------------- | --------- | ---------- |
| Lotes de 100 x 1 KB | 2 | 400 MB/s | 400 000 mensajes/seg. | 800 MB/s | más de 800.000 mensajes/seg. | 400 TU | 100 TU | 
| Lotes de 10 x 10 KB | 2 | 666 MB/s | 66,600 mensajes/seg. | 1,33 GB/s | 133,000 mensajes/seg. | 666 TU | 166 TU |
| Lotes de 6 x 32 KB | 1 | 1,05 GB/s | 34.000 mensajes/seg. | 1,05 GB/s | 34.000 mensajes/seg. | 1000 tu | 250 TU |

En las pruebas, se utilizaron los siguientes criterios:

- Se usó un clúster dedicado nivel Event Hubs con cuatro unidades de capacidad.
- El centro de eventos usado para la ingesta tenía 200 particiones.
- Se recibieron los datos que se ingieren por dos aplicaciones de receptor. Que reciben datos de todas las particiones.

## <a name="use-event-hubs-dedicated"></a>Uso de Event Hubs dedicados

Para usar Event Hubs dedicado, [póngase en contacto con soporte de facturación](https://ms.portal.azure.com/#create/Microsoft.Support) o con su representante de Microsoft. Puede aumentar o reducir verticalmente su capacidad a lo largo del mes para satisfacer sus necesidades agregando o quitando unidades de rendimiento. El equipo de producto de Event Hubs le ayuda a obtener la implementación flexible adecuada para usted.

## <a name="next-steps"></a>Pasos siguientes

Póngase en contacto con su representante de ventas de Microsoft o Microsoft Support para obtener más información acerca de la capacidad dedicada de Event Hubs. Para obtener más información acerca de los niveles de precios de Event Hubs, use los vínculos siguientes:

- [Precios de Event Hubs dedicado](https://azure.microsoft.com/pricing/details/event-hubs/). También puede establecer contacto con su representante de ventas de Microsoft o Microsoft Support para obtener más información acerca de la capacidad dedicada de Event Hubs.
- En [Preguntas más frecuentes sobre Event Hubs](event-hubs-faq.md) se proporciona información sobre los precios, así como respuestas a algunas preguntas frecuentes acerca de Event Hubs.
