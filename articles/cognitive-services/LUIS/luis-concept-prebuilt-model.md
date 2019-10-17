---
title: Modelos creados previamente (LUIS)
titleSuffix: Azure Cognitive Services
description: Los modelos creados previamente proporcionan dominios, intenciones, expresiones y entidades. Puede iniciar su aplicación con un dominio creado previamente o agregar un dominio pertinente a su aplicación más adelante.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 07/29/2019
ms.author: diberry
ms.openlocfilehash: b88801ded3dea7c7514ff117361feba3e95444ed
ms.sourcegitcommit: b4665f444dcafccd74415fb6cc3d3b65746a1a31
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2019
ms.locfileid: "72264391"
---
# <a name="prebuilt-domain-intent-and-entity-models"></a>Dominio, intención y modelos de entidad creados previamente

Los modelos creados previamente proporcionan dominios, intenciones, expresiones y entidades. Puede iniciar su aplicación con un dominio creado previamente o agregar un dominio pertinente a su aplicación más adelante. 

## <a name="types-of-prebuilt-models"></a>Tipos de modelos creados previamente

LUIS proporciona 3 tipos de modelos creados previamente. Cada modelo se puede agregar a su aplicación en cualquier momento. 

|Tipo de modelo|Incluye|
|--|--|
|Domain|Intenciones, expresiones, entidades|
|Intenciones|Intenciones, expresiones|
|Entidades|Solo entidades| 

## <a name="prebuilt-domains"></a>Dominios creados previamente

En Language Understanding (LUIS) se proporcionan *dominios creados previamente*, que son conjuntos creados previamente de [intenciones](luis-how-to-add-intents.md) y [entidades](luis-concept-entity-types.md) que funcionan de manera conjunta para los dominios o las categorías comunes de las aplicaciones cliente. 

Los dominios creados previamente están capacitados y listos para agregarse a la aplicación LUIS. Las intenciones y entidades de un dominio creado previamente se pueden personalizar completamente una vez las haya agregado a la aplicación. 

Si empieza desde la personalización de un dominio creado previamente completo, elimine las intenciones y entidades que la aplicación no necesita usar. También puede agregar algunas intenciones o entidades al conjunto que ya proporciona el dominio creado previamente. Por ejemplo, si usa el dominio creado previamente **Events** (Eventos) para una aplicación de eventos deportivos, puede para agregar entidades para los equipos deportivos. Al empezar a [proporcionar expresiones](luis-how-to-add-example-utterances.md) a LUIS, incluya términos que sean específicos de la aplicación. LUIS aprende a reconocerlos y adapta las intenciones y entidades del dominio creado previamente a las necesidades de la aplicación. 

> [!TIP]
> Las intenciones y entidades de un dominio creado previamente funcionan mejor de manera conjunta. Siempre que sea posible, se recomienda combinar las intenciones y entidades del mismo dominio.
> El dominio Utilidades creado previamente tiene intenciones que se pueden personalizar para usarlas en cualquier dominio. Por ejemplo, puede agregar `Utilities.Repeat` a la aplicación y entrenarla para que reconozca todas las acciones que es posible que el usuario quiera repetir en la aplicación. 

### <a name="changing-the-behavior-of-a-prebuilt-domain-intent"></a>Cambiar el comportamiento de una intención de dominio creado previamente

Es posible que descubra que un dominio creado previamente contiene una intención que es similar a otra que quiere incluir en la aplicación de LUIS pero con otro comportamiento. Por ejemplo, el dominio creado previamente **Places** (Lugares) proporciona una intención `MakeReservation` para realizar una reserva en un restaurante, pero quiere que la aplicación use esa intención para realizar reservas de hotel. En ese caso, puede modificar el comportamiento de esa intención si proporciona expresiones a LUIS sobre cómo realizar las reservas de hotel y las etiqueta mediante la intención `MakeReservation`, para que después LUIS se pueda volver a entrenar para que reconozca la intención `MakeReservation` en una solicitud para reservar un hotel.

En [Prebuilt domains reference](./luis-reference-prebuilt-domains.md) (Referencia de dominios creados previamente) puede encontrar una lista completa de los dominios creados previamente.

## <a name="prebuilt-intents"></a>Intenciones creadas previamente

LUIS proporciona intenciones creadas previamente y sus expresiones. Las intenciones pueden agregarse sin tener que agregar todo el dominio. Agregar una intención es el proceso de agregar una intención y sus expresiones. Asimismo, puede modificar tanto el nombre de la intención como la lista de expresiones.  

## <a name="prebuilt-entities"></a>Entidades precompiladas

LUIS incluye un conjunto de entidades precompiladas para reconocer tipos comunes de información, como fechas, horas, números, medidas y moneda. La compatibilidad de entidades precompiladas varía según la referencia cultural de la aplicación LUIS. Para ver una lista completa de las entidades precompiladas que LUIS admite, incluso la compatibilidad por referencia cultural, consulte la [referencia de entidades precompiladas](./luis-reference-prebuilt-entities.md).

Cuando una entidad precompilada se incluye en la aplicación, sus predicciones se incluyen en la aplicación publicada. El comportamiento de las entidades precompiladas ya está entrenado y **no** se puede modificar. 

> [!NOTE]
> **builtin.datetime** está en desuso. Se sustituye por [**builtin.datetimeV2**](luis-reference-prebuilt-datetimev2.md), que permite reconocer los intervalos de fechas y horas, así como mejorar el reconocimiento de las fechas y horas ambiguas.

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre cómo [agregar entidades creadas previamente](luis-prebuilt-entities.md) a la aplicación.
