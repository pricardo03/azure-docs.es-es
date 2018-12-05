---
title: Suscripciones en Azure API Management | Microsoft Docs
description: Obtenga información sobre el concepto de Suscripciones en Azure API Management.
services: api-management
documentationcenter: ''
author: miaojiang
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/14/2018
ms.author: apimpm
ms.openlocfilehash: 45a65c7a94f3e6917b2882f27c9ad7d764ef97d7
ms.sourcegitcommit: eba6841a8b8c3cb78c94afe703d4f83bf0dcab13
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/29/2018
ms.locfileid: "52621763"
---
# <a name="subscriptions-in-azure-api-management"></a>Suscripciones en Azure API Management

Suscripciones es un concepto importante en Azure API Management (APIM). Es la forma más habitual de obtención de acceso de los consumidores de API a las API publicadas a través de una instancia de APIM. En este artículo se proporciona información general del concepto.

## <a name="what-is-subscriptions"></a>Qué es Suscripciones

Al publicar las API a través de Azure APIM, la forma más fácil y habitual de proteger el acceso a esas API es mediante el uso de claves de suscripción. Es decir, los desarrolladores que necesiten usar las API publicadas deberán incluir una clave de suscripción válida en las solicitudes HTTP al realizar llamadas a esas API. De lo contrario, la puerta de enlace de API rechazará las llamadas inmediatamente y no se reenviarán a los servicios de back-end.

Para obtener una clave de suscripción para tener acceso a las API, es necesaria una suscripción. La suscripción es básicamente un contenedor con nombre para un par de claves de suscripción. Suscripciones puede obtenerse por medio de desarrolladores que deben usar las API publicadas, con o sin la aprobación de los publicadores de API. Los publicadores de API también pueden crear Suscripciones directamente, en nombre de los consumidores de API.

> [!TIP]
> APIM también admite otros mecanismos para proteger el acceso a API, entre los que se incluyen [OAuth2.0](api-management-howto-protect-backend-with-aad.md), [certificados de cliente](api-management-howto-mutual-certificates-for-clients.md) y [listas blancas de direcciones IP](https://docs.microsoft.com/azure/api-management/api-management-access-restriction-policies#RestrictCallerIPs)

## <a name="scope-of-subscriptions"></a>Ámbito de Suscripciones

Suscripciones se puede asociar a diversos ámbitos: producto, todas las API o una API individual.

### <a name="subscriptions-for-a-product"></a>Suscripciones para un producto

Tradicionalmente, Suscripciones en APIM se ha asociado siempre a un solo ámbito del [producto de API](api-management-terminology.md). Los desarrolladores encontrarían la lista de productos en el Portal para desarrolladores y enviarían solicitudes de suscripción para los productos que desean usar. Una vez aprobada una solicitud de suscripción (automáticamente o por medio de publicadores de API), el desarrollador puede usar las claves en esta para tener acceso a todas las API del producto.

![Suscripciones de producto](./media/api-management-subscriptions/product-subscription.png)

> [!TIP]
> En determinadas situaciones, es posible que los publicadores de API deseen publicar un producto de API para el público sin el requisito de Suscripciones. Pueden desactivar la opción **Requerir suscripción** en la página **Configuración** del producto en Azure Portal. Como resultado, se puede tener acceso a todas las API del producto sin una clave de API.

### <a name="subscriptions-for-all-apis-or-an-individual-api"></a>Suscripciones para todas las API o una API individual

> [!NOTE]
> Actualmente, esta característica está disponible solo en el nivel de consumo de API Management.

Cuando especificamos el nivel de [consumo](https://aka.ms/apimconsumptionblog) de APIM, realizamos algunos cambios para transmitir la administración de claves. En primer lugar, agregamos dos ámbitos más de suscripción: todas las API y una sola API. El ámbito de Suscripciones ya no se limita a un producto de API. Ahora es posible crear claves que conceden acceso a una API (o a todas las API de una instancia de APIM), sin necesidad de crear un producto y agregar las API a este primero. Además, cada instancia de APIM incluye ahora una suscripción inalterable a todas las API, lo que hace que probar y depurar API dentro de la consola de prueba resulte más fácil y sencillo.

En segundo lugar, APIM permite ahora Suscripciones "independientes". Ya no es necesario que Suscripciones se asocie a una cuenta de desarrollador. Esto es útil en situaciones como, por ejemplo, cuando varios desarrolladores o equipos comparten una suscripción.

Por último, los publicadores de API ahora pueden [crear Suscripciones](api-management-howto-create-subscriptions.md) directamente en Azure Portal:

![Suscripciones flexibles](./media/api-management-subscriptions/flexible-subscription.png)

## <a name="next-steps"></a>Pasos siguientes
Para más información sobre API Management:

+ Aprenda otros [conceptos](api-management-terminology.md) en API Management
+ Siga nuestros [tutoriales](import-and-publish.md) para obtener más información sobre API Management
+ Consulte nuestra [página de preguntas más frecuentes](api-management-faq.md) para ver las preguntas habituales