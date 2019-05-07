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
ms.openlocfilehash: a5184b9980dd9f83764950445c10e8bdfea6d71a
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65203941"
---
# <a name="overview-of-event-hubs-dedicated"></a>Introducción a Event Hubs dedicado

*Clústeres de Event Hubs* ofrecen implementaciones de inquilino único para los clientes con las necesidades más exigentes de transmisión por secuencias. Esta oferta de inquilino único tiene un SLA del 99,99% garantizado y solo está disponible en nuestro plan de tarifa de dedicado. Un clúster de Event Hubs puede incorporar millones de eventos por segundo con capacidad garantizada y la latencia de fracciones de segundo. Espacios de nombres y event hubs creados dentro del clúster dedicado incluyen todas las características de la oferta estándar y mucho más, pero sin los límites de entrada. También incluye el popular [Event Hubs Capture](event-hubs-capture-overview.md) característica sin costo adicional, lo que le permite automáticamente por lotes y el registro de flujos de datos en Azure Storage o Azure Data Lake. 

Los clústeres se aprovisionan y se factura por **unidades de capacidad (Cu)**, una cantidad de recursos de CPU y memoria previamente asignada. Puede adquirir Cu 1, 2, 4, 8, 12, 16 ó 20 para cada clúster. ¿Cuánto tiempo puede ingerir y transmitir por unidad de capacidad depende de diversos factores, como el número de productores y consumidores, la forma de carga, salida, velocidad (vea los resultados de pruebas comparativas a continuación para obtener más detalles). 

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
| Event Hubs |  10 | Sin límite |
| Eventos de entrada | Pago por millones de eventos | Se incluye |
| Tamaño de los mensajes | 1 millón de Bytes | 1 millón de Bytes |
| Particiones | 40 por espacio de nombres | 2000 por unidad de capacidad, 1024 por centro de eventos |
| Grupos de consumidores | 20 por centro de eventos | No hay límite por unidad de capacidad, 1000 por centro de eventos |
| Conexiones asincrónicas | 1000 incluidos | 100 000 incluidos |
| Retención de mensajes | 7 días, 84 GB por unidad incluido | 90 días, 10 TB incluido por unidad de capacidad |
| Capture | Pago por hora | Se incluye |

## <a name="how-to-onboard"></a>¿Cómo incorporarlo?

La experiencia de autoservicio para la incorporación a dedicado se encuentra en versión preliminar, a través del cual puede crear 1 CU de clústeres en las siguientes regiones:
  - Centro de Canadá
  - Europa occidental
  - Centro de EE. UU.
  - Este de EE. UU.
  - Este de EE. UU. - 2
  - Centro y norte de EE. UU.
  - Oeste de EE. UU.

Nos activamente está agregando nuevas regiones, pero mientras tanto si su región preferida no está en la lista, envíe una solicitud de soporte técnico para el [equipo de Event Hubs](https://ms.portal.azure.com/#create/Microsoft.Support) en *técnicos > Event Hubs > cuota > solicitar para Dedicado SKU*. El plan dedicado es único en el sentido de que tendrá más ayuda del equipo del producto Event Hubs en el proceso de incorporación con el fin de obtener la implementación flexible adecuada. 

## <a name="faqs"></a>Preguntas más frecuentes

#### <a name="what-can-i-achieve-with-a-cluster"></a>¿Qué puedo lograr con un clúster?

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

#### <a name="how-do-i-create-a-cluster-larger-than-1-cu"></a>¿Cómo puedo crear un clúster mayor que 1 CU?

En la versión preliminar de la experiencia de autoservicio, puede solicitar para escalar verticalmente el clúster después de crear el clúster. Después de crear un clúster CU 1, póngase en contacto con soporte técnico de Event Hubs rellene una [solicitud de soporte técnico](https://ms.portal.azure.com/#create/Microsoft.Support) en *técnicos > cuota > solicitud para escalar verticalmente o escalar hacia abajo clúster dedicado*. En la versión de GA, podrá escalar verticalmente el clúster directamente a través del portal. 

#### <a name="can-i-scale-down-my-cluster"></a>¿Puedo reducir mi clúster?

Después de la creación, los clústeres se facturan durante un mínimo de 4 horas de uso. En la versión preliminar de la experiencia de autoservicio, puede enviar un [solicitud de soporte técnico](https://ms.portal.azure.com/#create/Microsoft.Support) al equipo de Event Hubs en *técnicos > cuota > solicitud para escalar verticalmente o escalar hacia abajo clúster dedicado*. Puede tardar hasta 7 días para completar la solicitud para escalar verticalmente el clúster. 

#### <a name="how-will-geo-dr-work-with-my-cluster"></a>¿Cómo funcionarán Geo-DR con mi clúster?

Puede que un espacio de nombres en un clúster dedicado capa con otro espacio de nombres en un clúster dedicado capa del par de replicación geográfica. No se recomienda un espacio de nombres de nivel de dedicado con un espacio de nombres en nuestro estándar que ofrece, ya que el límite de rendimiento serán incompatibles que se producirán errores de asignación. 


#### <a name="can-i-migrate-my-standard-namespaces-to-belong-to-a-dedicated-tier-cluster"></a>¿Puedo migrar mi espacios de nombres estándar para que pertenezcan a un clúster dedicado niveles?
No actualmente admitimos un proceso de migración automatizada para migrar los datos de event hubs de un espacio de nombres estándar a un dedicado uno. Para migrar a un clúster dedicado capa, se recomienda purgar los mensajes que queden en los centros de eventos de nivel estándar y reemplazando los puntos de conexión con el de su espacio de nombres dedicado.

## <a name="next-steps"></a>Pasos siguientes

Póngase en contacto con su representante de ventas de Microsoft o el Servicio de soporte técnico de Microsoft para obtener detalles adicionales sobre la capacidad Event Hubs Dedicado. Para más información acerca de los planes de tarifa de Event Hubs, visite los vínculos siguientes:

- [Precios de Event Hubs dedicado](https://azure.microsoft.com/pricing/details/event-hubs/). También puede ponerse en contacto con su representante de ventas de Microsoft o el Servicio de soporte técnico de Microsoft para obtener detalles adicionales sobre la capacidad dedicada de Event Hubs.
- En [Preguntas más frecuentes sobre Event Hubs](event-hubs-faq.md) se proporciona información sobre los precios, así como respuestas a algunas preguntas frecuentes acerca de Event Hubs.
