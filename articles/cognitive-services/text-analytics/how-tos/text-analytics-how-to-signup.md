---
title: Registro en Text Analytics API
titleSuffix: Azure Cognitive Services
description: Instrucciones para registrarse y utilizar el servicio de Text Analytics.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 02/13/2019
ms.author: aahi
ms.openlocfilehash: 53532a19482a33f8727e71d44ae169225b5b1c98
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60829674"
---
# <a name="how-to-sign-up-for-the-text-analytics-api"></a>Procedimiento para registrarse en Text Analytics API

Los recursos de Text Analytics están disponibles ininterrumpidamente en la nube. Para poder cargar el contenido para su análisis, tiene que registrarse y obtener una clave de acceso. Cada llamada a la API requiere una clave de acceso en la solicitud.

+ Empiece con una suscripción de Azure. Puede crear una [cuenta gratuita ](https://azure.microsoft.com/free/) para probarlo sin costo.

+ Cree una [Cuenta de la API de Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) y elija **Text Analytics API**. La clave se genera al realizarse la suscripción.

Para Text Analytics, hay un nivel Gratis para evaluación y exploración, y los niveles facturables para cargas de trabajo de producción. Puede tener varios registros en cada suscripción: uno gratis, uno de pago, etc. Puede cambiar a un nivel que ofrece más transacciones si su volumen de solicitudes aumenta.

No hay ningún acuerdo de nivel de servicio para servicios en versión preliminar o en el nivel gratis. Para más información, consulte [Contrato de nivel de servicio para Cognitive Services](https://azure.microsoft.com/support/legal/sla/cognitive-services/v1_1/)

## <a name="how-to-change-tiers"></a>Cómo cambiar de nivel

Comience con un nivel Gratis y realice la transición a un nivel facturable para las cargas de trabajo de producción. La facturación del nivel estándar se ofrece en niveles graduados. Puede cambiar los niveles y mantener el mismo punto de conexión y las mismas claves de acceso.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y [encuentre el servicio](text-analytics-how-to-access-key.md).

2. Haga clic en **Franja de precios**.

   ![Comando de franja de precios en el menú de navegación izquierdo](../media/portal-pricing-tier.png)

3. Seleccione un nivel y haga clic en **Seleccionar**.  Los nuevos límites surten efecto en cuanto se procesa la selección. 

   ![Iconos y botón de selección en la página de selección de niveles](../media/portal-choose-tier.png)

## <a name="how-billing-works"></a>Cómo funciona la facturación

La facturación se basa en el número de transacciones. Puede comprar un bloque de transacciones en un nivel concreto en un ciclo de facturación mensual, y luego, si supera el límite, se le aplica un pequeño cargo por uso por encima del límite por cada transacción. Si supera el límite máximo de forma habitual, considere la posibilidad de cambiar a un nivel superior.

Consulte la [página de precios](https://azure.microsoft.com/pricing/details/cognitive-services/text-analytics/) para más información.

### <a name="what-constitutes-a-transaction-in-the-text-analytics-api"></a>¿Qué se considera una transacción en Text Analytics API?
Cualquier anotación en un documento cuenta como una transacción. Las llamadas de puntuación por lotes tienen también en cuenta el número de documentos que tienen que puntuarse en esa transacción. Por ejemplo, si se envían 1000 documentos para análisis de opinión en una única llamada API, contarán como 1000 transacciones.

## <a name="see-also"></a>Otras referencias 

 [Introducción a Text Analytics](../overview.md)  
 [Preguntas más frecuentes](../text-analytics-resource-faq.md)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Obtención de una clave de acceso](text-analytics-how-to-access-key.md)
