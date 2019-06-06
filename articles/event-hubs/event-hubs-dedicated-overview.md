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
ms.openlocfilehash: 52e092e6e48f004656860cb5d078e780039584ab
ms.sourcegitcommit: 1aefdf876c95bf6c07b12eb8c5fab98e92948000
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2019
ms.locfileid: "66730234"
---
# <a name="overview-of-event-hubs-dedicated"></a>Introducción a Event Hubs dedicado

*Clústeres de Event Hubs* ofrecen implementaciones de inquilino único para los clientes con las necesidades más exigentes de transmisión por secuencias. Esta oferta de inquilino único tiene un SLA del 99,99% garantizado y solo está disponible en nuestro plan de tarifa de dedicado. Un clúster de Event Hubs puede incorporar millones de eventos por segundo con capacidad garantizada y la latencia de fracciones de segundo. Espacios de nombres y event hubs creados dentro del clúster dedicado incluyen todas las características de la oferta estándar y mucho más, pero sin los límites de entrada. También incluye el popular [Event Hubs Capture](event-hubs-capture-overview.md) característica sin costo adicional, lo que le permite automáticamente por lotes y el registro de flujos de datos en Azure Storage o Azure Data Lake. 

Los clústeres se aprovisionan y se factura por **unidades de capacidad (Cu)** , una cantidad de recursos de CPU y memoria previamente asignada. Puede adquirir 1, 2, 4, 8, 12, 16 o 20 CU para cada clúster. ¿Cuánto tiempo puede ingerir y transmitir por unidad de capacidad depende de diversos factores, como el número de productores y consumidores, la forma de carga, salida, velocidad (vea los resultados de pruebas comparativas a continuación para obtener más detalles). 

> [!NOTE]
> Todos los clústeres de Event Hubs están habilitadas para Kafka de manera predeterminada y se admiten puntos de conexión de Kafka que se pueden usar por existentes Kafka aplicaciones basadas en. Tener habilitadas para Kafka en el clúster no afecta a los casos de uso no Kafka; No hay ninguna opción o la necesidad de deshabilitar a Kafka en un clúster.

## <a name="why-dedicated"></a>¿Por qué dedicado?

Event Hubs dedicado ofrece tres beneficios atractivas para los clientes que necesitan la capacidad de nivel empresarial:

#### <a name="single-tenancy-guarantees-capacity-for-better-performance"></a>Multiniiquilino a inquilino único garantiza la capacidad para mejorar el rendimiento

Un clúster dedicado garantiza la capacidad a escala completa, y puede admitir hasta gigabytes de datos de streaming con una latencia de almacenamiento y fracciones de segundo totalmente durable para satisfacerlas ráfagas de tráfico. 

#### <a name="inclusive-and-exclusive-access-to-features"></a>Acceso inclusivo y exclusivo a las características 
La oferta dedicada incluye características como capturar en ningún costo adicional, así como acceso exclusivo a las próximas características como BYOK. El servicio también administra equilibrio de carga, OS actualizaciones, las revisiones de seguridad y creación de particiones para el cliente, por lo que puede dedicar menos tiempo en el mantenimiento de la infraestructura y más tiempo en crear características de cliente.  

#### <a name="cost-savings"></a>Ahorro de costos
En los volúmenes de entrada alto (> 100 tu), un clúster los costos mucho menor por hora que comprar una cantidad de unidades de rendimiento en la oferta estándar comparable.


## <a name="event-hubs-dedicated-quotas-and-limits"></a>Event Hubs dedicado cuotas y límites

La oferta de Event Hubs dedicado se factura a un precio mensual fijo, con un mínimo de 4 horas de uso. El nivel dedicado ofrece todas las características del plan estándar, pero con capacidad de escala de empresa y los límites para los clientes con cargas de trabajo exigentes. 

| Característica | Estándar | Dedicado |
| --- |:---:|:---:|
| Ancho de banda | 20 tu (hasta 40 tu) | 20 CUs |
| Espacios de nombres |  1 | 50 por unidad de capacidad |
| Event Hubs |  10 por espacio de nombres | 1000 por espacio de nombres |
| Eventos de entrada | Pago por millones de eventos | Se incluye |
| Tamaño de los mensajes | 1 millón de Bytes | 1 millón de Bytes |
| Particiones | 40 por espacio de nombres | 2000 por unidad de capacidad |
| Grupos de consumidores | 20 por centro de eventos | No hay límite por unidad de capacidad, 1000 por centro de eventos |
| Conexiones asincrónicas | 1.000 a 5.000 incluye, max | 100 K incluidos y max |
| Retención de mensajes | 7 días, 84 GB por unidad incluido | 90 días, 10 TB incluido por unidad de capacidad |
| Capture | Pago por hora | Se incluye |

## <a name="how-to-onboard"></a>¿Cómo incorporarlo?

La experiencia de autoservicio [crear un clúster de Event Hubs](event-hubs-dedicated-cluster-create-portal.md) a través de la [Portal de Azure](https://aka.ms/eventhubsclusterquickstart) está ahora en versión preliminar. Si tiene alguna pregunta o necesita ayuda incorporación a Event Hubs dedicado, póngase en contacto con el [equipo de Event Hubs](mailto:askeventhubs@microsoft.com).

## <a name="faqs"></a>Preguntas más frecuentes

#### <a name="what-can-i-achieve-with-a-cluster"></a>¿Qué puedo lograr con un clúster?

Para un clúster de Event Hubs, ¿cuánto tiempo puede ingerir y transmitir depende de varios factores, como los productores, los consumidores, la velocidad a la que se ingesta y procesamiento y mucho más. 

En la tabla siguiente se muestran los resultados del banco de pruebas que hemos obtenido durante nuestras pruebas:

| Forma de la carga | Receptores | Ancho de banda de entrada| Mensajes de entrada | Ancho de banda de salida | Mensajes de salida | TU totales | TU por CU |
| ------------- | --------- | ---------------- | ------------------ | ----------------- | ------------------- | --------- | ---------- |
| Lotes de 100 x 1 KB | 2 | 400 MB/s | 400 k mensajes/s | 800 MB/s | 800 k de mensajes/seg. | 400 TU | 100 TU | 
| Lotes de 10 x 10 KB | 2 | 666 MB/s | 66,6 k de mensajes/seg. | 1,33 GB/s | 133 k de mensajes/seg. | 666 TU | 166 TU |
| Lotes de 6 x 32 KB | 1 | 1,05 GB/s | mensajes de 34 KB / seg | 1,05 GB/s | 34 KB de mensajes/seg. | 1000 TU | 250 TU |

En las pruebas, se usaron los siguientes criterios:

- Se usó un clúster de Event Hubs de nivel dedicado con cuatro unidades de capacidad (Cu). 
- El centro de eventos usado para la ingesta tenía 200 particiones. 
- Dos aplicaciones receptoras recibieron los datos ingeridos de todas las particiones.

#### <a name="can-i-scale-updown-my-cluster"></a>¿Se puede escalar verticalmente/reducir verticalmente el clúster?

Después de la creación, los clústeres se facturan durante un mínimo de 4 horas de uso. En la versión preliminar de la experiencia de autoservicio, puede enviar un [solicitud de soporte técnico](https://ms.portal.azure.com/#create/Microsoft.Support) al equipo de Event Hubs en *técnicos > cuota > solicitud para escalar verticalmente o escalar hacia abajo clúster dedicado* a escala el clúster hacia arriba o hacia abajo. Puede tardar hasta 7 días para completar la solicitud para escalar verticalmente el clúster. 

#### <a name="how-will-geo-dr-work-with-my-cluster"></a>¿Cómo funcionarán Geo-DR con mi clúster?

Puede que un espacio de nombres en un clúster dedicado capa con otro espacio de nombres en un clúster dedicado capa del par de replicación geográfica. No se recomienda un espacio de nombres de nivel de dedicado con un espacio de nombres en nuestro estándar que ofrece, ya que el límite de rendimiento serán incompatibles que se producirán errores de asignación. 

#### <a name="can-i-migrate-my-standard-namespaces-to-belong-to-a-dedicated-tier-cluster"></a>¿Puedo migrar mi espacios de nombres estándar para que pertenezcan a un clúster dedicado niveles?
No actualmente admitimos un proceso de migración automatizada para migrar los datos de event hubs de un espacio de nombres estándar a un dedicado uno. 

## <a name="next-steps"></a>Pasos siguientes

Póngase en contacto con su representante de ventas de Microsoft o Microsoft Support para obtener detalles adicionales acerca de Event Hubs dedicado. También puede crear un clúster u obtener más información acerca de Event Hubs, los planes de tarifa, visite los vínculos siguientes:

- [Crear un clúster de Event Hubs a través del Portal de Azure](https://aka.ms/eventhubsclusterquickstart) 
- [Precios de Event Hubs dedicado](https://azure.microsoft.com/pricing/details/event-hubs/). También puede ponerse en contacto con su representante de ventas de Microsoft o el Servicio de soporte técnico de Microsoft para obtener detalles adicionales sobre la capacidad dedicada de Event Hubs.
- En [Preguntas más frecuentes sobre Event Hubs](event-hubs-faq.md) se proporciona información sobre los precios, así como respuestas a algunas preguntas frecuentes acerca de Event Hubs.
