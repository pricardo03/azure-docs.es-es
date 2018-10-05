---
title: Características de las aplicaciones de LUIS en Azure Cognitive Services
titleSuffix: Azure Cognitive Services
description: Agregue características a un modelo de lenguaje para proporcionar sugerencias sobre cómo reconocer la entrada que quiera etiquetar o clasificar. Las características ayudan a LUIS a reconocer intenciones y entidades.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 015679b6020e9d2a4d702f9d6e723ecd9499d8dc
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "47034474"
---
# <a name="phrase-list-features-in-luis"></a>Características de lista de frases en LUIS

En el aprendizaje automático, una *característica* es un rasgo distintivo o un atributo de datos que el sistema observa. 

Agregue características a un modelo de lenguaje para proporcionar sugerencias sobre cómo reconocer la entrada que quiera etiquetar o clasificar. Las características ayudan a LUIS a reconocer las intenciones y las entidades, pero las características no son intenciones o entidades propiamente dichas. En su lugar, es posible que las características proporcionen ejemplos de términos relacionados.  

## <a name="what-is-a-phrase-list-feature"></a>¿Qué es una característica de lista de frases?
Una lista de frases incluye un grupo de valores (palabras o frases) que pertenecen a la misma clase y que se deben tratar de forma similar (por ejemplo, nombres de ciudades o productos). Lo que LUIS aprende sobre una de ellas se aplica automáticamente al resto. Esta lista no es una [entidad de lista](luis-concept-entity-types.md#types-of-entities) cerrada (coincidencias de texto exactas) de palabras coincidentes.

Una lista de frases se agrega al vocabulario del dominio de aplicación como una segunda señal para LUIS sobre esas palabras.

## <a name="how-to-use-phrase-lists"></a>Cómo usar las listas de frases
En el [tutorial sobre entidades simples](luis-quickstart-primary-and-secondary-data.md) de la aplicación de recursos humanos, la aplicación usa una lista de frases **Job** de tipos de trabajos como programador, reparador de tejados y secretaria. Si etiqueta uno de estos valores como una entidad de aprendizaje automático, LUIS aprende a reconocer el resto. 

Una lista de frases puede ser intercambiable o no intercambiable. Una lista de frases *intercambiables* es para los valores que son sinónimos, mientras que una lista de frases *no intercambiables* está diseñada para los valores que no son sinónimos pero siguen necesitando una señal adicional en la aplicación. 

<a name="phrase-lists-help-identify-simple-exchangeable-entities"></a>
## <a name="phrase-lists-help-identify-simple-interchangeable-entities"></a>Las listas de frases ayudan a identificar las entidades intercambiables simples
Las listas de frases intercambiables son una buena manera de optimizar el rendimiento de la aplicación de LUIS. Si la aplicación tiene problemas para predecir las expresiones para la intención correcta o para reconocer entidades, piense si las expresiones contienen palabras inusuales, o bien palabras que puedan tener un significado ambiguo. Estas palabras son buenas candidatas para incluirlas en una lista de frases.

## <a name="phrase-lists-help-identify-intents-by-better-understanding-context"></a>Las listas de frases ayudan identificar las intenciones mediante una mejor comprensión del contexto
Una lista de frases no es una instrucción para que LUIS realice coincidencias estrictas o siempre etiquete todos los términos de la lista de frases de la misma forma exacta. Simplemente es una sugerencia. Por ejemplo, podría tener una lista de frases en la que se indique que "Patti" y "Selma" son nombres, pero LUIS puede seguir usando la información contextual para reconocer que tienen otro significado en "Reservar cena para dos en Patti's Diner" y "Buscar indicaciones de ruta en coche para Selma, Georgia". 

Agregar una lista de frases es una alternativa a la adición de más expresiones de ejemplo a una intención. 

## <a name="an-interchangeable-phrase-list"></a>Una lista de frases intercambiables
Use una lista de frases intercambiables cuando la lista de palabras o frases cree una clase o un grupo. Ejemplo de esto es una lista de meses como "Enero", "Febrero", "Marzo" o nombres como "John", "Mary", "Frank".  Estas listas son intercambiables, lo que significa que la expresión se podría etiquetar con la misma intención o entidad si se usara otra palabra de la lista de frases. Por ejemplo, si "Mostrar el calendario de enero" tiene la misma intención que "Mostrar el calendario de febrero", las palabras deberían estar en una lista intercambiable. 

## <a name="a-non-interchangeable-phrase-list"></a>Una lista de frases no intercambiables
Utilice una lista de frases no intercambiables para las palabras o frases que no son sinónimos que se pueden agrupar en el dominio. 

Como ejemplo, utilice una lista de frases no intercambiables para palabras poco frecuentes, exclusivas y extranjeras. Es posible que LUIS no pueda reconocer las palabras poco frecuente y exclusivas, así como las palabras extranjeras (ajenas a la referencia cultural de la aplicación). La configuración no intercambiable indica que el conjunto de palabras poco frecuentes constituye una clase que LUIS debe aprender a reconocer, pero que no son sinónimos ni intercambiables entre sí.

## <a name="when-to-use-phrase-lists-versus-list-entities"></a>Cuándo se deben usar listas de frases frente a las entidades de lista
Aunque tanto una lista de frases como las entidades de lista pueden afectar a las expresiones de todas las intenciones, en cada caso se realiza de forma diferente. Use una lista de frases para afectar a la puntuación de predicción de intención. Use una entidad de lista para afectar a la extracción de la entidad de una coincidencia de texto exacta. 

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
