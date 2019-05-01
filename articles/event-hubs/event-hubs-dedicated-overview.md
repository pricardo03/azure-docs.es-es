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
ms.openlocfilehash: 52d46009464c7417d5b525154fdac09c030aabe7
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2019
ms.locfileid: "64708014"
---
# <a name="overview-of-event-hubs-dedicated"></a>Introducción a Event Hubs dedicado

*Clústeres de Event Hubs* ofrecen implementaciones de inquilino único para los clientes con las necesidades más exigentes de transmisión por secuencias. Esta oferta de inquilino único tiene un SLA del 99,99% garantizado y solo está disponible en nuestro plan de tarifa de dedicado. Un clúster de Event Hubs puede incorporar millones de eventos por segundo con capacidad garantizada y la latencia de fracciones de segundo. Espacios de nombres y event hubs creados dentro del clúster dedicado incluyen todas las características de la oferta estándar y mucho más, pero sin los límites de entrada. También incluye el popular [Event Hubs Capture](event-hubs-capture-overview.md) característica sin costo adicional, lo que le permite automáticamente por lotes y el registro de flujos de datos en Azure Storage o Azure Data Lake. 

Clústeres dedicados se aprovisionan y se factura por **unidades de capacidad (Cu)**, una cantidad de recursos de CPU y memoria previamente asignada. Puede adquirir Cu 1, 2, 4, 8, 12, 16 ó 20 para cada clúster. ¿Cuánto tiempo puede ingerir y transmitir por unidad de capacidad depende de diversos factores, como el número de productores y consumidores, la forma de carga, salida, velocidad (vea los resultados de pruebas comparativas a continuación para obtener más detalles). 

## <a name="why-dedicated"></a>¿Por qué dedicado?

Event Hubs dedicado ofrece tres beneficios atractivas para los clientes que necesitan la capacidad de nivel empresarial:

#### <a name="single-tenancy-guarantees-capacity-for-better-performance"></a>Multiniiquilino a inquilino único garantiza la capacidad para mejorar el rendimiento

Un clúster dedicado garantiza la capacidad a escala completa, y puede admitir hasta gigabytes de datos de streaming con una latencia de almacenamiento y fracciones de segundo totalmente durable para satisfacerlas ráfagas de tráfico. 

#### <a name="inclusive-and-exclusive-access-to-features"></a>Acceso inclusivo y exclusivo a las características 
La oferta dedicada incluye características como capturar en ningún costo adicional, así como acceso exclusivo a las próximas características como BYOK. El servicio también administra equilibrio de carga, OS actualizaciones, las revisiones de seguridad y creación de particiones para el cliente, por lo que puede dedicar menos tiempo en el mantenimiento de la infraestructura y más tiempo en crear características de cliente.  

#### <a name="cost-savings"></a>Ahorro de costos
En los volúmenes de entrada alto (> 100 tu), un clúster los costos mucho menor por hora que comprar una cantidad de unidades de rendimiento en la oferta estándar comparable.


## <a name="event-hubs-standard-vs-dedicated"></a>Event Hubs estándar frente a Dedicado

En la tabla siguiente se comparan niveles de servicio disponibles de Event Hubs. La oferta de Event Hubs dedicado se factura por un precio mensual fijo, frente a los precios de uso para la mayoría de las características del estándar. El nivel Dedicado ofrece todas las características del plan estándar, pero con capacidad de escala de empresa para clientes con cargas de trabajo exigentes. 

| Característica | Estándar | Dedicado |
| --- |:---:|:---:|
| Eventos de entrada | Pago por millones de eventos | Se incluye |
| Unidad de rendimiento (entrada de 1 MB/seg., salida de 2 MB/seg.) | Pago por hora | Se incluye |
| Tamaño de los mensajes | 1 MB | 1 MB |
| Particiones | 40 por espacio de nombres | 2000 por unidad de capacidad |
| Grupos de consumidores | 20 por centro de eventos | 1000 por centro de eventos |
| Máx. Ancho de banda | 20 tu (hasta 40 tu)    | 20 CUs |
| Conexiones asincrónicas | 1000 incluidos | 100 000 incluidos |
| Retención de mensajes | 1 día incluido | Hasta 7 días incluidos |
| Capture | Pago por hora | Se incluye |

## <a name="what-can-i-achieve-with-a-cluster"></a>¿Qué puedo lograr con un clúster?

Para un clúster de Event Hubs, ¿cuánto tiempo puede ingerir y transmitir depende de varios factores, como los productores, los consumidores, la velocidad a la que se ingesta y procesamiento y mucho más. 

En la tabla siguiente se muestran los resultados del banco de pruebas que hemos obtenido durante nuestras pruebas:

| Forma de la carga | Receptores | Ancho de banda de entrada| Mensajes de entrada | Ancho de banda de salida | Mensajes de salida | TU totales | TU por CU |
| ------------- | --------- | ---------------- | ------------------ | ----------------- | ------------------- | --------- | ---------- |
| Lotes de 100 x 1 KB | 2 | 400 MB/s | 400 000 mensajes/s | 800 MB/s | 800 000 mensajes/s | 400 TU | 100 TU | 
| Lotes de 10 x 10 KB | 2 | 666 MB/s | 66 600 mensajes/s | 1,33 GB/s | 133 000 mensajes/s | 666 TU | 166 TU |
| Lotes de 6 x 32 KB | 1 | 1,05 GB/s | 34 000 mensajes/s | 1,05 GB/s | 34 000 mensajes/s | 1000 TU | 250 TU |

En las pruebas, se usaron los siguientes criterios:

- Se usó un clúster de Event Hubs de nivel dedicado con cuatro unidades de capacidad (Cu). 
- El centro de eventos usado para la ingesta tenía 200 particiones. 
- Dos aplicaciones receptoras recibieron los datos ingeridos de todas las particiones.

## <a name="how-to-onboard"></a>¿Cómo incorporarlo?

Para incorporar este SKU, póngase en contacto con el [servicio de soporte técnico de facturación](https://ms.portal.azure.com/#create/Microsoft.Support) o con su representante de Microsoft. Puede aumentar o reducir verticalmente su capacidad a lo largo del mes para satisfacer sus necesidades agregando o quitando unidades de rendimiento. El plan dedicado es único en el sentido de que tendrá más ayuda del equipo del producto Event Hubs en el proceso de incorporación con el fin de obtener la implementación flexible adecuada. 

## <a name="next-steps"></a>Pasos siguientes

Póngase en contacto con su representante de ventas de Microsoft o el Servicio de soporte técnico de Microsoft para obtener detalles adicionales sobre la capacidad Event Hubs Dedicado. Para más información acerca de los planes de tarifa de Event Hubs, visite los vínculos siguientes:

- [Precios de Event Hubs dedicado](https://azure.microsoft.com/pricing/details/event-hubs/). También puede ponerse en contacto con su representante de ventas de Microsoft o el Servicio de soporte técnico de Microsoft para obtener detalles adicionales sobre la capacidad dedicada de Event Hubs.
- En [Preguntas más frecuentes sobre Event Hubs](event-hubs-faq.md) se proporciona información sobre los precios, así como respuestas a algunas preguntas frecuentes acerca de Event Hubs.
