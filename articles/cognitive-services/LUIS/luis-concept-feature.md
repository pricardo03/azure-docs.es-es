---
title: 'Características: LUIS'
titleSuffix: Azure Cognitive Services
description: Agregue características a un modelo de lenguaje para proporcionar sugerencias sobre cómo reconocer la entrada que quiera etiquetar o clasificar.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 07/29/2019
ms.author: diberry
ms.openlocfilehash: dab4b4c6f41a95623a40e5d3fd859f9613afac27
ms.sourcegitcommit: 4f7dce56b6e3e3c901ce91115e0c8b7aab26fb72
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/04/2019
ms.locfileid: "71949606"
---
# <a name="phrase-list-features-in-your-luis-app"></a>Características de lista de frases en la aplicación de LUIS

En el aprendizaje automático, una *característica* es un rasgo distintivo o un atributo de datos que el sistema observa. 

Agregue características a un modelo de lenguaje para proporcionar sugerencias sobre cómo reconocer la entrada que quiera etiquetar o clasificar. Las características ayudan a LUIS a reconocer las intenciones y las entidades, pero las características no son intenciones o entidades propiamente dichas. En su lugar, es posible que las características proporcionen ejemplos de términos relacionados.  

## <a name="what-is-a-phrase-list-feature"></a>¿Qué es una característica de lista de frases?
Una lista de frases es una lista de palabras o frases que son importantes para la aplicación, mucho más que otras palabras de las expresiones. Una lista de frases se agrega al vocabulario del dominio de aplicación como una señal adicional para LUIS sobre esas palabras. Lo que LUIS aprende sobre una de ellas se aplica automáticamente al resto. Esta lista no es una [entidad de lista](luis-concept-entity-types.md#types-of-entities) cerrada de coincidencias de texto exactas.

Las listas de frases no ayudan a la lematización, por lo que deberá agregar expresiones de ejemplo que usen una variedad de lematizaciones para todas las palabras y frases importantes del vocabulario.

## <a name="phrase-lists-help-all-models"></a>Listas de frases que ayudan a todos los modelos

Las listas de frases no están vinculadas a una entidad ni a una intención específicas, pero se agregan como una mejora importante para todas las entidades e intenciones. Su objetivo es mejorar la detección de intenciones y la clasificación de entidades.

## <a name="how-to-use-phrase-lists"></a>Cómo usar las listas de frases

[Cree una lista de frases](luis-how-to-add-features.md) cuando su aplicación tenga palabras o frases que son importantes para su funcionamiento, como:

* términos del sector
* jerga
* abreviaturas
* lenguaje específico de la empresa
* lenguaje que pertenece a otro idioma, pero que se utiliza con frecuencia en la aplicación
* palabras y frases clave en las expresiones de ejemplo

Una vez que haya escrito algunas palabras o frases, use la función **Recomendar** para buscar valores relacionados. Revise los valores relacionados antes de agregarlos a su lista de frases.

Una lista de frases contiene los valores que son sinónimos. Por ejemplo, si quiere obtener todos los cuerpos de agua encontrados y tiene expresiones de ejemplo tales como: 

* ¿Qué ciudades están cerca de los Grandes Lagos? 
* ¿Qué carretera pasa por Lake Havasu?
* ¿Dónde empieza y termina el Nilo? 

Cada expresión se debe determinar según la intención y las entidades, independientemente del cuerpo de agua: 

* ¿Qué ciudades están cerca de [cuerpoDeAgua]?
* ¿Qué carretera pasa por [cuerpoDeAgua]?
* ¿Dónde empieza y termina [cuerpoDeAgua]? 

Porque las palabras o frases del cuerpo de agua son sinónimas y se pueden utilizar indistintamente en las expresiones. 

<a name="phrase-lists-help-identify-simple-exchangeable-entities"></a>

## <a name="phrase-lists-help-identify-simple-interchangeable-entities"></a>Las listas de frases ayudan a identificar las entidades intercambiables simples
Las listas de frases intercambiables son una buena manera de optimizar el rendimiento de la aplicación de LUIS. Si la aplicación tiene problemas para predecir las expresiones para la intención correcta o para reconocer entidades, piense si las expresiones contienen palabras inusuales, o bien palabras que puedan tener un significado ambiguo. Estas palabras son buenas candidatas para incluirlas en una lista de frases.

## <a name="phrase-lists-help-identify-intents-by-better-understanding-context"></a>Las listas de frases ayudan identificar las intenciones mediante una mejor comprensión del contexto
Una lista de frases no es una instrucción para que LUIS realice coincidencias estrictas o siempre etiquete todos los términos de la lista de frases de la misma forma exacta. Simplemente es una sugerencia. Por ejemplo, podría tener una lista de frases en la que se indique que "Patti" y "Selma" son nombres, pero LUIS puede seguir usando la información contextual para reconocer que tienen otro significado en "Reservar cena para dos en Patti's Diner" y "Buscar indicaciones de ruta en coche para Selma, Georgia". 

Agregar una lista de frases es una alternativa a la adición de más expresiones de ejemplo a una intención. 

## <a name="when-to-use-phrase-lists-versus-list-entities"></a>Cuándo se deben usar listas de frases frente a las entidades de lista
Aunque tanto una lista de frases como las [entidades de lista](reference-entity-list.md) pueden afectar a las expresiones de todas las intenciones, en cada caso se realiza de forma diferente. Use una lista de frases para afectar a la puntuación de predicción de intención. Use una entidad de lista para afectar a la extracción de la entidad de una coincidencia de texto exacta. 

### <a name="use-a-phrase-list"></a>Usar una lista de frases
Con una lista de frases, LUIS puede tener en cuenta el contexto y generalizar para identificar los elementos que son similares, pero no una coincidencia exacta, como elementos de una lista. Si es necesario que la aplicación de LUIS sea capaz de generalizar e identificar los elementos nuevos de una categoría, use una lista de frases. 

Si quiere poder reconocer las instancias nuevas de una entidad, como un programador de reuniones que deba reconocer los nombres de los contactos nuevos, o bien una aplicación de inventario que deba reconocer los productos nuevos, use otro tipo de entidad de aprendizaje automático como una entidad simple o jerárquica. Después, cree una lista de frases de palabras y frases que permitan a LUIS buscar otras palabras similares a la entidad. Esta lista sirve de guía a LUIS para reconocer los ejemplos de la entidad mediante la adición de más importancia al valor de esas palabras. 

Las listas de frases son similares al vocabulario específico de dominio que ayuda a mejorar la calidad de comprensión de las intenciones y entidades. Un uso común de una lista de frases es para nombres propios, como los nombres de ciudades. El nombre de una ciudad puede tener varias palabras incluidos guiones o apóstrofos.
 
### <a name="dont-use-a-phrase-list"></a>No usar una lista de frases 
Una entidad de lista define de forma explícita todos los valores que una entidad puede aceptar y solo identifica los valores que coinciden exactamente. Una entidad de lista puede ser adecuada para una aplicación en la que todas las instancias de una entidad son conocidas y no cambian con frecuencia. Algunos ejemplos son los elementos de comida en el menú de un restaurante que cambia con poca frecuencia. Si necesita una coincidencia de texto exacta de una entidad, no use una lista de frases. 

## <a name="best-practices"></a>Procedimientos recomendados
Obtenga información sobre los [procedimientos recomendados](luis-concept-best-practices.md).

## <a name="next-steps"></a>Pasos siguientes

Vea [Agregar características](luis-how-to-add-features.md) para obtener más información sobre cómo agregar características a la aplicación de LUIS.
