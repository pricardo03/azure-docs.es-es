---
title: 'Planificación de la aplicación: LUIS'
titleSuffix: Azure Cognitive Services
description: Resuma las intenciones y entidades de aplicación relevantes y cree los planes de aplicación en Language Understanding Intelligent Services (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 10/25/2019
ms.author: diberry
ms.openlocfilehash: b5e5df111b81cb60b6d194be190421bdb5ce2683
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73467701"
---
# <a name="plan-your-luis-app-schema-with-subject-domain-and-data-extraction"></a>Planeamiento del esquema de aplicación de LUIS con el dominio de sujeto y la extracción de datos

Un esquema de aplicación de LUIS contiene intenciones y entidades de interés para el dominio de sujeto. Las intenciones clasifican las expresiones de usuario y las entidades extraen los datos de esas expresiones. 

## <a name="identify-your-domain"></a>Identificar el dominio

Una aplicación de LUIS se centra en un tema específico de dominio.  Por ejemplo, podría darse el caso de que tuviera una aplicación de viajes que hace la reserva de billetes, vuelos, hoteles y alquiler de vehículos. Otra aplicación podría proporcionar contenido relacionado con el ejercicio, el seguimiento de resultados de fitness y el establecimiento de objetivos. Identificar el dominio le ayuda a buscar palabras o frases que son importantes para su dominio.

> [!TIP]
> LUIS ofrece [dominios creados previamente](luis-how-to-use-prebuilt-domains.md) para muchos escenarios comunes.
> Compruebe si puede usar un dominio creado previamente como punto de partida para su aplicación.

## <a name="identify-your-intents"></a>Identificar las intenciones

Piense en las [intenciones](luis-concept-intent.md) que son importantes para la tarea de la aplicación. 

Veamos el ejemplo de una aplicación de viajes, que dispone de funciones para reservar un vuelo y consultar el tiempo del destino del usuario. Puede definir las intenciones `BookFlight` y `GetWeather` de estas acciones. 

En una aplicación más compleja con más funciones, tiene más intenciones, así que debe definirlas cuidadosamente para que no sean demasiado específicas. Por ejemplo, es posible que `BookFlight` y `BookHotel` deban ser intenciones independientes, pero que `BookInternationalFlight` y `BookDomesticFlight` sean demasiado parecidas.

> [!NOTE]
> Se recomienda usar únicamente las intenciones que necesite para llevar a cabo las funciones de la aplicación. Si define demasiadas intenciones, a LUIS le resultará más difícil clasificar las expresiones correctamente. Si define demasiado pocas, podrían ser tan generales que se superpongan.

Si no necesita identificar la intención general del usuario, agregue todas las expresiones de usuario de ejemplo a la intención None. Si la aplicación necesita más intenciones, puede crearlas más adelante. 

## <a name="create-example-utterances-for-each-intent"></a>Creación de expresiones de ejemplo para cada intención

Cuando haya determinado las intenciones, cree 15 o 30 expresiones de ejemplo para cada intención. Para empezar, no tenga un número menor a este ni cree demasiadas expresiones para cada intención. Cada expresión debe ser diferente de la expresión anterior. Una buena variedad de expresiones incluye el recuento total de palabras, la elección de palabras, el tiempo verbal y la puntuación. 

Revise las [expresiones](luis-concept-utterance.md) para obtener más información.

## <a name="identify-your-entities"></a>Identificar las entidades

En las expresiones de ejemplo, identifique las entidades que desea extraer. Para reservar un vuelo necesita información como el destino, la fecha, la aerolínea, el tipo de billete y la clase del viaje. Cree las entidades para estos tipos de datos y, a continuación, marque las [entidades](luis-concept-entity-types.md) en las expresiones de ejemplo, ya que son importantes para llevar a cabo una intención. 

A la hora de determinar las entidades que va a usar en la aplicación, tenga en cuenta que hay distintos tipos de entidades para capturar las relaciones entre los tipos de objetos. [Las entidades de LUIS](luis-concept-entity-types.md) proporcionan más detalles sobre los distintos tipos.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre el [ciclo de desarrollo](luis-concept-app-iteration.md) habitual.  