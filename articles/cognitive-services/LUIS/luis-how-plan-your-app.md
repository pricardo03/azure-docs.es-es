---
title: 'Planear la aplicaciones de Language Understanding (LUIS): Azure Cognitive Services | Microsoft Docs'
description: Resuma las intenciones y entidades de aplicación relevantes y cree los planes de aplicación en Language Understanding Intelligent Services (LUIS).
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: conceptual
ms.date: 09/06/2017
ms.author: diberry
ms.openlocfilehash: 7c8974767621ad574b243fba5c3e49d97ec142e6
ms.sourcegitcommit: 3d0295a939c07bf9f0b38ebd37ac8461af8d461f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/06/2018
ms.locfileid: "43842279"
---
# <a name="plan-your-luis-app"></a>Planear la aplicación de LUIS

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

### <a name="simple-entity"></a>Entidad simple
Una entidad simple describe un concepto único.

![entidad simple](./media/luis-plan-your-app/simple-entity.png)

Vea [Extracción de datos](luis-concept-data-extraction.md#simple-entity-data) para obtener más información sobre cómo extraer la entidad simple a partir de la respuesta a la consulta JSON del punto de conexión. Pruebe este [inicio rápido](luis-quickstart-primary-and-secondary-data.md) para obtener más información sobre cómo usar una entidad simple.

### <a name="hierarchical-entity"></a>Entidad jerárquica
Una entidad jerárquica es un tipo especial de entidad **simple**, donde se define una categoría y sus miembros como una relación entre elemento primario y secundario. La relación se determina mediante el contexto desde dentro de la expresión. Los elementos secundarios de una entidad jerárquica son también entidades simples.

![entidad jerárquica](./media/luis-plan-your-app/hierarchical-entity.png)

Vea [Extracción de datos](luis-concept-data-extraction.md#hierarchical-entity-data) para obtener más información sobre cómo extraer la entidad jerárquica a partir de la respuesta a la consulta JSON del punto de conexión. Pruebe este [inicio rápido](luis-quickstart-intent-and-hier-entity.md) para obtener más información sobre cómo usar una entidad jerárquica.

### <a name="composite-entity"></a>Entidad compuesta
Una entidad compuesta está formada por otras entidades que forman las partes de un todo. Una entidad compuesta contiene una variedad de tipos de entidad.

![entidad compuesta](./media/luis-plan-your-app/composite-entity.png)

Vea [Extracción de datos](luis-concept-data-extraction.md#composite-entity-data) para obtener más información sobre cómo extraer la entidad compuesta a partir de la respuesta a la consulta JSON del punto de conexión. Pruebe este [tutorial](luis-tutorial-composite-entity.md) para obtener más información sobre cómo usar una entidad compuesta.

### <a name="prebuilt-entity"></a>Entidad creada previamente
LUIS proporciona [entidades creadas previamente](luis-prebuilt-entities.md) para tipos de datos comunes, como dirección URL, dirección de correo electrónico, datos y número. Puede usar la entidad creada previamente de número para el número de entradas en un pedido de entradas.

![Entidad creada previamente de número](./media/luis-plan-your-app/number-entity.png)

Consulte [Extracción de datos](luis-concept-data-extraction.md#prebuilt-entity-data) para más información sobre cómo extraer la entidad creada previamente a partir de la respuesta a la consulta JSON del punto de conexión. 

### <a name="list-entity"></a>Entidad de lista 
Una entidad de lista es una lista de valores especificada explícitamente. Cada valor consta de uno o varios sinónimos. En una aplicación de viajes es posible que pueda crear una entidad de lista que represente los nombres de los aeropuertos.

![entidad de lista](./media/luis-plan-your-app/list-entity.png)

Vea [Extracción de datos](luis-concept-data-extraction.md#list-entity-data) para obtener más información sobre cómo extraer entidades de la lista de la respuesta a la consulta JSON del punto de conexión. Pruebe este [inicio rápido](luis-quickstart-intent-and-list-entity.md) para obtener más información sobre cómo usar una entidad de lista.

### <a name="regular-expression-entity"></a>Entidad de expresión regular
Una entidad de expresión regular permite a LUIS extraer datos con formato correcto de una expresión en función de una expresión regular.

![Entidad de expresión regular](./media/luis-plan-your-app/regex-entity.png)

Vea [Extracción de datos](luis-concept-data-extraction.md#regular-expression-entity-data) para obtener más información sobre cómo extraer entidades de expresiones regulares de la respuesta a la consulta JSON del punto de conexión. Pruebe el [inicio rápido](luis-quickstart-intents-regex-entity.md) para obtener más información sobre cómo usar una entidad de expresión regular.

## <a name="next-steps"></a>Pasos siguientes
Después de que la aplicación esté entrenada, publicada y haya obtenido las expresiones de punto de conexión, planee la implementación de mejoras de predicción con el [aprendizaje activo](luis-how-to-review-endoint-utt.md), [listas de frases](luis-concept-feature.md) y [patrones](luis-concept-patterns.md). 


* Consulte [Create your first Language Understanding Intelligent Services (LUIS) app](luis-get-started-create-app.md) (Creación de su primera aplicación de Language Understanding Intelligent Services [LUIS]) para un tutorial rápido sobre cómo crear una aplicación de LUIS.
