---
title: 'Introducción a los centros de eventos dedicados: Azure Event Hubs | Microsoft Docs'
description: En este artículo se proporciona una introducción a Azure Event Hubs dedicado, que ofrece implementaciones de un único inquilino de centros de eventos.
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
ms.openlocfilehash: d418715ab651721d03b67bd411eb90607391bf10
ms.sourcegitcommit: c31a2dd686ea1b0824e7e695157adbc219d9074f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/18/2019
ms.locfileid: "54402343"
---
# <a name="overview-of-event-hubs-dedicated"></a>Introducción a Event Hubs dedicado

La capacidad *dedicada de Event Hubs* ofrece implementaciones de un único inquilino para los clientes con los requisitos más exigentes. A escala completa, Azure Event Hubs puede incorporar más de dos millones de eventos por segundo o hasta 2 GB por segundo de telemetría con un almacenamiento totalmente duradero y una latencia inferior al segundo. Esto también permite soluciones integradas mediante el procesamiento en tiempo real y por lotes en el mismo sistema. Con [Event Hubs Capture](event-hubs-capture-overview.md) incluido en la oferta, un solo flujo permite canalizaciones en tiempo real y basadas en lotes, lo cual reduce la complejidad de la solución.

En la tabla siguiente se comparan niveles de servicio disponibles de Event Hubs. La oferta de Event Hubs Dedicado consiste en un precio mensual fijo, frente a los precios por uso de la mayoría de las características del nivel Estándar. El nivel Dedicado ofrece todas las características del plan estándar, pero con capacidad de escala de empresa para clientes con cargas de trabajo exigentes. 

| Característica | Estándar | Dedicado |
| --- |:---:|:---:|:---:|
| Eventos de entrada | Pago por millones de eventos | Se incluye |
| Unidad de rendimiento (entrada de 1 MB/seg., salida de 2 MB/seg.) | Pago por hora | Se incluye |
| Tamaño de los mensajes | 1 MB | 1 MB |
| Directivas de publicadores | SÍ | SÍ |   
| Grupos de consumidores | 20 | 20 |
| Redifusión de mensajes | SÍ | SÍ |
| Unidades de rendimiento máximo | 20 (flexible a 100)   | 1 unidad de capacidad (CU) ≈ 50 |
| Conexiones asincrónicas | 1000 incluidos | 100 000 incluidos |
| Conexiones desacopladas adicionales | SÍ | SÍ |
| Retención de mensajes | 1 día incluido | Hasta 7 días incluidos |
| Capture | Pago por hora | Se incluye |

## <a name="benefits-of-event-hubs-dedicated-capacity"></a>Ventajas de Event Hubs con capacidad dedicada

Dispone de las siguientes ventajas cuando utiliza Event Hubs con capacidad dedicada:

* Hospedaje de un solo inquilino sin ruido de otros inquilinos.
* Rendimiento repetible cada vez.
* Capacidad garantizada para satisfacer sus necesidades de ráfagas.
* Incluye la función [Capture](event-hubs-capture-overview.md) de Event Hubs, para proporcionar integración con retención a largo plazo y de microlotes.
* Cero mantenimiento: el servicio administra el equilibrio de carga, las actualizaciones del sistema operativo, las revisiones de seguridad y la creación de particiones.
* Precio por hora fijo.
* Retención de mensajes hasta siete días sin cargo adicional.

Event Hubs dedicado también elimina algunas de las limitaciones de rendimiento de la oferta Estándar. Las unidades de procesamiento del nivel Estándar proporcionan 1000 eventos por segundo o 1 MB por segundo de entrada por unidad de procesamiento, así como el doble de esa cantidad de salida. La oferta de escala Dedicado no tiene ninguna restricción en la cantidad de eventos de entrada y de salida. Estos límites solo se rigen por la capacidad de procesamiento de los centros de eventos adquiridos.

Este entorno reservado y dedicado proporciona otras funciones exclusivas para este nivel, como las siguientes:

* Controla el número de espacios de nombres en el clúster.
* Especifica los límites de rendimiento en cada espacio de nombres.
* Configura el número de instancias de Event Hubs de cada espacio de nombres.
* Determina el límite del número de particiones.

Este servicio está dirigido a los usuarios de telemetría más grandes y está disponible para los clientes con un contrato Enterprise.

## <a name="how-to-onboard"></a>¿Cómo incorporarlo?

Puede aumentar o reducir verticalmente su capacidad a lo largo del mes para satisfacer sus necesidades agregando o quitando unidades de rendimiento. El plan dedicado es único en el sentido de que tendrá más ayuda del equipo del producto Event Hubs en el proceso de incorporación con el fin de obtener la implementación flexible adecuada. Para incorporar este SKU, póngase en contacto con el [servicio de soporte técnico de facturación](https://ms.portal.azure.com/#create/Microsoft.Support) o con su representante de Microsoft.

## <a name="next-steps"></a>Pasos siguientes

Póngase en contacto con su representante de ventas de Microsoft o el Servicio de soporte técnico de Microsoft para obtener detalles adicionales sobre la capacidad Event Hubs Dedicado. Para más información acerca de los planes de tarifa de Event Hubs, visite los vínculos siguientes:

- [Precios de Event Hubs dedicado](https://azure.microsoft.com/pricing/details/event-hubs/). También puede ponerse en contacto con su representante de ventas de Microsoft o el Servicio de soporte técnico de Microsoft para obtener detalles adicionales sobre la capacidad dedicada de Event Hubs.
- En [Preguntas más frecuentes sobre Event Hubs](event-hubs-faq.md) se proporciona información sobre los precios, así como respuestas a algunas preguntas frecuentes acerca de Event Hubs. 
