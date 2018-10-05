---
title: Uso de dominios creados previamente para crear aplicaciones con mayor rapidez en aplicaciones de LUIS
titleSuffix: Azure Cognitive Services
description: En Language Understanding (LUIS) se proporcionan dominios creados previamente, que son conjuntos creados previamente de intenciones y entidades que funcionan de manera conjunta para los dominios o las categorías comunes de las aplicaciones cliente. Los dominios creados previamente se han entrenado de forma previa y están listos para agregarlos a la aplicación de LUIS.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/06/2018
ms.author: diberry
ms.openlocfilehash: 1aa7e1bf9c1a584803a60a5061b4ae4cc8664ff4
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "47037378"
---
# <a name="use-prebuilt-domains-in-luis-apps"></a>Uso de dominios creados previamente en aplicaciones de LUIS  

En Language Understanding (LUIS) se proporcionan *dominios creados previamente*, que son conjuntos creados previamente de [intenciones](luis-how-to-add-intents.md) y [entidades](luis-concept-entity-types.md) que funcionan de manera conjunta para los dominios o las categorías comunes de las aplicaciones cliente. Los dominios creados previamente se han entrenado de forma previa y están listos para agregarlos a la aplicación de LUIS. Las intenciones y entidades de un dominio creado previamente son totalmente personalizables una vez que se han agregado a la aplicación; se pueden entrenar con expresiones del sistema para que funcionen para los usuarios. Se puede usar un dominio creado previamente completo como punto de partida para la personalización, o bien simplemente pedir prestadas varias intenciones o entidades a un dominio creado previamente. 

Examine la pestaña **Prebuilt domains** (Dominios creados previamente) para ver otros dominios creados previamente que se pueden usar en la aplicación. Haga clic en el icono de un dominio para agregarlo a la aplicación, o bien haga clic en **Learn more** (Más información) en su mosaico para obtener información sobre sus intenciones y entidades.

> [!TIP]
> En [Prebuilt domains reference](./luis-reference-prebuilt-domains.md) (Referencia de dominios creados previamente) puede encontrar una lista completa de los dominios creados previamente.

![Incorporación de un dominio creado previamente](./media/luis-how-to-prebuilt-domain-entities/add-prebuilt-domain.png)


## <a name="add-a-prebuilt-domain"></a>Incorporación de un dominio creado previamente
En la pestaña **Prebuilt domains** (Dominios creados previamente), busque el dominio RestaurantReservation y haga clic en **Add domain** (Agregar dominio). Una vez que el dominio creado previamente se ha agregado a la aplicación de LUIS, abra **Intents** (Intenciones) y haga clic en la intención RestaurantReservation.Reserve. Puede ver que ya se han proporcionado muchas expresiones de ejemplo y que se han etiquetado con entidades.

![Intención RestaurantReservation.Reserve](./media/luis-how-to-prebuilt-domain-entities/prebuilt-domain-restaurant-reservation.png)


## <a name="designing-luis-apps-from-prebuilt-domains"></a>Diseño de aplicaciones de LUIS a partir de dominios creados previamente
Cuando se usan dominios creados previamente en la aplicación de LUIS, se puede personalizar un dominio creado previamente completo o, simplemente empezar con algunas de sus intenciones y entidades.

## <a name="customizing-an-entire-prebuilt-domain"></a>Personalización de un dominio creado previamente completo
Los dominios creados previamente están diseñados para ser generales. Contienen muchas intenciones y entidades, entre las que puede elegir para personalizar una aplicación en función de sus necesidades. Si empieza desde la personalización de un dominio creado previamente completo, elimine las intenciones y entidades que la aplicación no necesita usar. También puede agregar algunas intenciones o entidades al conjunto que ya proporciona el dominio creado previamente. Por ejemplo, si usa el dominio creado previamente **Events** (Eventos) para una aplicación de eventos deportivos, puede para agregar entidades para los equipos deportivos. Al empezar a [proporcionar expresiones](luis-how-to-add-example-utterances.md) a LUIS, incluya términos que sean específicos de la aplicación. LUIS aprende a reconocerlos y adapta las intenciones y entidades del dominio creado previamente a las necesidades de la aplicación. 

> [!TIP]
> Las intenciones y entidades de un dominio creado previamente funcionan mejor de manera conjunta. Siempre que sea posible, se recomienda combinar las intenciones y entidades del mismo dominio.
> * Un procedimiento recomendado consiste en usar intenciones relacionadas del mismo dominio. Por ejemplo, si va a personalizar la intención `MakeReservation` del dominio **Places** (Lugares), seleccione la intención `Cancel` del dominio **Places** en lugar de la intención Cancel (Cancelar) de los dominios **Events** (Eventos) o **Utilities** (Sectores públicos).

## <a name="changing-the-behavior-of-a-prebuilt-domain-intent"></a>Cambiar el comportamiento de una intención de dominio creado previamente
Es posible que descubra que un dominio creado previamente contiene una intención que es similar a otra que quiere incluir en la aplicación de LUIS pero con otro comportamiento. Por ejemplo, el dominio creado previamente **Places** (Lugares) proporciona una intención `MakeReservation` para realizar una reserva en un restaurante, pero quiere que la aplicación use esa intención para realizar reservas de hotel. En ese caso, puede modificar el comportamiento de esa intención si proporciona expresiones a LUIS sobre cómo realizar las reservas de hotel y las etiqueta mediante la intención `MakeReservation`, para que después LUIS se pueda volver a entrenar para que reconozca la intención `MakeReservation` en una solicitud para reservar un hotel.

> [!TIP]
> Consulte el dominio de sectores públicos para obtener intenciones creadas previamente que se pueden personalizar para su uso en cualquier dominio. Por ejemplo, puede agregar `Utilities.Repeat` a la aplicación y entrenarla para que reconozca todas las acciones que es posible que el usuario quiera repetir en la aplicación.


## <a name="next-step"></a>Paso siguiente

Personalice un dominio creado previamente mediante la adición de más expresiones de ejemplo.

> [!div class="nextstepaction"]
> [Add example utterances](./luis-how-to-add-example-utterances.md) (Adición de expresiones de ejemplo)

## <a name="additional-resources"></a>Recursos adicionales

Vea [Prebuilt domains reference](./luis-reference-prebuilt-domains.md) (Referencia de dominios creados previamente) para obtener más detalles sobre los dominios creados previamente.
