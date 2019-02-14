---
title: Planeamiento de la aplicación
titleSuffix: Language Understanding - Azure Cognitive Services
description: Resuma las intenciones y entidades de aplicación relevantes y cree los planes de aplicación en Language Understanding Intelligent Services (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 01/02/2019
ms.author: diberry
ms.openlocfilehash: d0c5ba41bc636479407e7580fa686fe1bba91612
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55873206"
---
# <a name="plan-your-luis-app-with-subject-domain-intents-and-entities"></a>Planificación de la aplicación LUIS con el dominio del tema, las intenciones y las entidades

Es importante planear la aplicación. Identifique el dominio, incluidas posibles intenciones y entidades que sean pertinentes para la aplicación.  

## <a name="identify-your-domain"></a>Identificar el dominio

Una aplicación de LUIS se centra en un tema específico de dominio.  Por ejemplo, podría darse el caso de que tuviera una aplicación de viajes que hace la reserva de billetes, vuelos, hoteles y alquiler de vehículos. Otra aplicación podría proporcionar contenido relacionado con el ejercicio, el seguimiento de resultados de fitness y el establecimiento de objetivos. Identificar el dominio le ayuda a buscar palabras o frases que son importantes para su dominio.

> [!TIP]
> LUIS ofrece [dominios creados previamente](luis-how-to-use-prebuilt-domains.md) para muchos escenarios comunes.
> Compruebe si puede usar un dominio creado previamente como punto de partida para su aplicación.

## <a name="identify-your-intents"></a>Identificar las intenciones

Piense en las [intenciones](luis-concept-intent.md) que son importantes para la tarea de la aplicación. Veamos el ejemplo de una aplicación de viajes, que dispone de funciones para reservar un vuelo y consultar el tiempo del destino del usuario. Puede definir las intenciones "ReservarVuelo" y "VerTiempo" para estas acciones. En una aplicación más compleja que dispone más funciones, tiene más intenciones y deberá definirlas cuidadosamente para que no sean demasiado específicas. Por ejemplo, es posible que "ReservarVuelo" y "ReservarHotel" deban ser intenciones diferentes, pero "ReservarVueloInternacional" y "ReservarVueloNacional" podrían resultar demasiado parecidas.

> [!NOTE]
> Se recomienda usar únicamente las intenciones que necesite para llevar a cabo las funciones de la aplicación. Si define demasiadas intenciones, a LUIS le resultará más difícil clasificar las expresiones correctamente. Si define demasiado pocas, podrían resultar ser tan generales que se superpondrán.

## <a name="create-example-utterances-for-each-intent"></a>Creación de expresiones de ejemplo para cada intención

Cuando haya determinado las intenciones, cree 10 o 15 expresiones de ejemplo para cada intención. Para empezar, no tenga un número menor a este ni cree muchas expresiones para cada intención. Cada expresión debe ser diferente de la expresión anterior. Una buena variedad de expresiones incluye el recuento total de palabras, la elección de palabras, el tiempo verbal y la puntuación. 

## <a name="identify-your-entities"></a>Identificar las entidades

En las expresiones de ejemplo, identifique las entidades que desea extraer. Para reservar un vuelo necesita cierta información, como el destino, la fecha, la aerolínea, el tipo de billete y la clase del viaje. Crea las entidades para estos tipos de datos y, a continuación, marca las [entidades](luis-concept-entity-types.md) en las expresiones de ejemplo, ya que son importantes para llevar a cabo una intención. 

A la hora de determinar las entidades que va a usar en la aplicación, tenga en cuenta que hay distintos tipos de entidades para capturar las relaciones entre los tipos de objetos. [Las entidades de LUIS](luis-concept-entity-types.md) proporcionan más detalles sobre los distintos tipos.

## <a name="next-steps"></a>Pasos siguientes

Después de que la aplicación esté entrenada, publicada y haya obtenido las expresiones de punto de conexión, planee la implementación de mejoras de predicción con el [aprendizaje activo](luis-how-to-review-endoint-utt.md), [listas de frases](luis-concept-feature.md) y [patrones](luis-concept-patterns.md). 


* Consulte [Create your first Language Understanding Intelligent Services (LUIS) app](luis-get-started-create-app.md) (Creación de su primera aplicación de Language Understanding Intelligent Services [LUIS]) para un tutorial rápido sobre cómo crear una aplicación de LUIS.
