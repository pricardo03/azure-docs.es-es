---
title: Planificación de las aplicaciones de LUIS | Microsoft Docs
description: Resuma las intenciones y entidades de aplicación relevantes y cree los planes de aplicación en Language Understanding Intelligent Services (LUIS).
services: cognitive-services
author: DeniseMak
manager: hsalama
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2017
ms.author: v-geberr
ms.openlocfilehash: c67a2c16077c9033b52a909360b21cb7f88a5a9d
ms.sourcegitcommit: 756f866be058a8223332d91c86139eb7edea80cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2018
ms.locfileid: "37344995"
---
# <a name="plan-your-luis-app"></a>Planear la aplicación de LUIS

Es importante planear la aplicación antes de empezar a crearla en LUIS. Prepare un esquema o resumen de las posibles intenciones y entidades relevantes para el tema específico de dominio de la aplicación.  

## <a name="identify-your-domain"></a>Identificar el dominio
Una aplicación de LUIS se centra en un tema específico de dominio.  Por ejemplo, podría darse el caso de que tuviera una aplicación de viajes que hace la reserva de billetes, vuelos, hoteles y alquiler de vehículos. Otra aplicación podría proporcionar contenido relacionado con el ejercicio, el seguimiento de resultados de fitness y el establecimiento de objetivos. 

> [!TIP]
> LUIS ofrece [dominios creados previamente](luis-how-to-use-prebuilt-domains.md) para muchos escenarios comunes.
> Compruebe si puede usar un dominio creado previamente como punto de partida para su aplicación.

## <a name="identify-your-intents"></a>Identificar las intenciones
Piense en las [intenciones](luis-concept-intent.md) que son importantes para la tarea de la aplicación. Veamos el ejemplo de una aplicación de viajes, que dispone de funciones para reservar un vuelo y consultar el tiempo del destino del usuario. Puede definir las intenciones "ReservarVuelo" y "VerTiempo" para estas acciones. En una aplicación más compleja que dispone más funciones, tiene más intenciones y deberá definirlas cuidadosamente para que no sean demasiado específicas. Por ejemplo, es posible que "ReservarVuelo" y "ReservarHotel" deban ser intenciones diferentes, pero "ReservarVueloInternacional" y "ReservarVueloNacional" podrían resultar demasiado parecidas.

> [!NOTE]
> Se recomienda usar únicamente las intenciones que necesite para llevar a cabo las funciones de la aplicación. Si define demasiadas intenciones, a LUIS le resultará más difícil clasificar las expresiones correctamente. Si define demasiado pocas, podrían resultar ser tan generales que se superpondrán.


## <a name="identify-your-entities"></a>Identificar las entidades
Para reservar un vuelo necesita cierta información, como el destino, la fecha, la aerolínea, el tipo de billete y la clase del viaje. Puede agregar estos elementos como [entidades](luis-concept-entity-types.md) porque son importantes para llevar a cabo una intención. 

A la hora de determinar las entidades que va a usar en la aplicación, tenga en cuenta que hay distintos tipos de entidades para capturar las relaciones entre los tipos de objetos. [Las entidades de LUIS](luis-concept-entity-types.md) proporcionan más detalles sobre los distintos tipos.

### <a name="simple-entity"></a>Entidad simple
Una entidad simple describe un concepto único.

![entidad simple](./media/luis-plan-your-app/simple-entity.png)

Vea [Extracción de datos](luis-concept-data-extraction.md#simple-entity-data) para obtener más información sobre cómo extraer la entidad simple a partir de la respuesta a la consulta JSON del punto de conexión. Pruebe el [inicio rápido](luis-quickstart-primary-and-secondary-data.md) de la entidad simple para obtener más información sobre cómo usar una entidad simple.

### <a name="hierarchical-entity"></a>Entidad jerárquica
Una entidad jerárquica es un tipo especial de entidad **simple**, donde se define una categoría y sus miembros como una relación entre elemento primario y secundario.

![entidad jerárquica](./media/luis-plan-your-app/hierarchical-entity.png)

Vea [Extracción de datos](luis-concept-data-extraction.md#hierarchical-entity-data) para obtener más información sobre cómo extraer la entidad jerárquica a partir de la respuesta a la consulta JSON del punto de conexión. Pruebe el [inicio rápido](luis-quickstart-intent-and-hier-entity.md) de la entidad jerárquica para obtener más información sobre cómo usar una entidad jerárquica.

### <a name="composite-entity"></a>Entidad compuesta
Una entidad compuesta está formada por otras entidades que forman las partes de un todo. 

![entidad compuesta](./media/luis-plan-your-app/composite-entity.png)

Vea [Extracción de datos](luis-concept-data-extraction.md#composite-entity-data) para obtener más información sobre cómo extraer la entidad compuesta a partir de la respuesta a la consulta JSON del punto de conexión. Pruebe el [tutorial](luis-tutorial-composite-entity.md) de entidades compuestas para obtener más información sobre cómo usar una entidad de compuesta.

### <a name="prebuilt-entity"></a>Entidad creada previamente
LUIS proporciona [entidades creadas previamente](luis-prebuilt-entities.md) para los tipos comunes como `Number`, que se pueden usar para el número de billetes de un pedido.

![Entidad creada previamente de número](./media/luis-plan-your-app/number-entity.png)

Vea [Extracción de datos](luis-concept-data-extraction.md#prebuilt-entity-data) para obtener más información sobre cómo extraer entidades de expresiones regulares de la respuesta a la consulta JSON del punto de conexión. 

### <a name="list-entity"></a>Entidad de lista 
Una entidad de lista es una lista de valores especificada explícitamente. Cada valor consta de uno o varios sinónimos. En una aplicación de viajes es posible que pueda crear una entidad de lista que represente los nombres de los aeropuertos.

![entidad de lista](./media/luis-plan-your-app/list-entity.png)

Vea [Extracción de datos](luis-concept-data-extraction.md#list-entity-data) para obtener más información sobre cómo extraer entidades de la lista de la respuesta a la consulta JSON del punto de conexión. Pruebe el [inicio rápido](luis-quickstart-intent-and-list-entity.md) para obtener más información sobre cómo usar una entidad de la lista.

### <a name="regular-expression-entity"></a>Entidad de expresión regular
Una entidad de expresión regular permite a LUIS extraer datos de una expresión en función de una expresión regular.

![Entidad de expresión regular](./media/luis-plan-your-app/regex-entity.png)

Vea [Extracción de datos](luis-concept-data-extraction.md#regular-expression-entity-data) para obtener más información sobre cómo extraer entidades de expresiones regulares de la respuesta a la consulta JSON del punto de conexión. Pruebe el [inicio rápido](luis-quickstart-intents-regex-entity.md) para obtener más información sobre cómo usar una entidad de expresión regular.

## <a name="after-getting-endpoint-utterances"></a>Después de obtener las expresiones de punto de conexión
En cuanto la aplicación haya obtenido las expresiones de punto de conexión, planee la implementación de mejoras de predicción con el [aprendizaje activo](label-suggested-utterances.md), [listas de frases](luis-concept-feature.md) y [patrones](luis-concept-patterns.md). 

### <a name="patternany-entity"></a>Entidad Pattern.any
Patterns.any es un marcador de posición de longitud variable que solo se usa en la expresión de plantilla de un [patrón](luis-concept-patterns.md) para marcar dónde empieza y acaba la entidad. Las expresiones de plantilla se ajustan a una [sintaxis correcta](luis-concept-patterns.md#pattern-syntax) para identificar entidades y texto que se puede ignorar.


## <a name="next-steps"></a>Pasos siguientes
* Vea [Create your first Language Understanding Intelligent Services (LUIS) app][luis-get-started-create-app] (Cree su primera aplicación de Language Understanding Intelligent Services (LUIS)) para obtener un tutorial rápido sobre cómo crear una aplicación de LUIS.

[luis-get-started-create-app]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-get-started-create-app