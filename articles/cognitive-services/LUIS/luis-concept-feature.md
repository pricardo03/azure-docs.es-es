---
title: Descripción de las características de las aplicaciones de LUIS en Azure | Microsoft Docs
description: Obtenga información sobre las características, que ayudan a mejorar el rendimiento de la aplicación de LUIS. Las características incluyen listas de frases y patrones para el reconocimiento de expresiones regulares.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 04/18/2018
ms.author: v-geberr
ms.openlocfilehash: 416fadaf52d7a71dcaab81aab3bf6099213e5af5
ms.sourcegitcommit: 50f82f7682447245bebb229494591eb822a62038
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/08/2018
ms.locfileid: "35383487"
---
# <a name="phrase-list-features-in-luis"></a>Características de lista de frases en LUIS

En el aprendizaje automático, una *característica* es un rasgo distintivo o un atributo de datos que el sistema observa. 

Agregue características a un modelo de lenguaje para proporcionar sugerencias sobre cómo reconocer la entrada que quiera etiquetar o clasificar. Las características ayudan a LUIS a reconocer las intenciones y las entidades, pero las características no son intenciones o entidades propiamente dichas. En su lugar, es posible que las características proporcionen ejemplos de términos relacionados.  

## <a name="what-is-a-phrase-list-feature"></a>¿Qué es una característica de lista de frases?
Una lista de frases incluye un grupo de valores (palabras o frases) que pertenecen a la misma clase y que se deben tratar de forma similar (por ejemplo, nombres de ciudades o productos). Lo que LUIS aprende sobre una de ellas se aplica automáticamente al resto. No se trata de una [entidad de lista](luis-concept-entity-types.md#types-of-entities) cerrada (coincidencias de texto exactas) de palabras coincidentes.

Una lista de frases se agrega al vocabulario del dominio de aplicación como una segunda señal para LUIS sobre esas palabras.

## <a name="how-to-use-phrase-lists"></a>Cómo usar las listas de frases
En una aplicación de agencia de viajes, cree una lista de frases denominada "Ciudades" que contenga los valores Londres, París y El Cairo. Si etiqueta uno de estos valores como una entidad simple en una [expresión de ejemplo](luis-how-to-add-example-utterances.md#add-simple-entity-label) en una intención, LUIS aprende a reconocer el resto. 

Una lista de frases puede ser intercambiable o no intercambiable. Una lista de frases *intercambiable* es para los valores que son sinónimos, mientras que una lista de frases *no intercambiable* está diseñada para los valores que no son sinónimos pero que son similares de otra forma. 

## <a name="phrase-lists-help-identify-simple-exchangeable-entities"></a>Las listas de frases ayudan a identificar las entidades intercambiables simples
Las listas de frases intercambiables son una buena forma de optimizar el rendimiento de la aplicación de LUIS. Si la aplicación tiene problemas para predecir las expresiones para la intención correcta o para reconocer entidades, piense si las expresiones contienen palabras inusuales, o bien palabras que puedan tener un significado ambiguo. Estas palabras son buenas candidatas para incluirlas en una lista de frases.

## <a name="phrase-lists-help-identify-intents-by-better-understanding-context"></a>Las listas de frases ayudan identificar las intenciones mediante una mejor comprensión del contexto
Use listas de frases para palabras poco frecuentes, propietarias y extranjeras. LUIS puede ser capaz de reconocer palabras poco frecuentes y propietarias, así como palabras extranjeras (externas a la referencia cultural de la aplicación) y, por tanto, se deberían agregar a una lista de frases. Esta lista de frases debería marcarse como no intercambiable, para indicar que el conjunto de palabras poco frecuentes constituye una clase que LUIS debe aprender a reconocer, pero que no son sinónimos ni intercambiables entre sí.

Una lista de frases no es una instrucción para que LUIS realice coincidencias estrictas o siempre etiquete todos los términos de la lista de frases de la misma forma exacta. Simplemente es una sugerencia. Por ejemplo, podría tener una lista de frases en la que se indique que "Patti" y "Selma" son nombres, pero LUIS puede seguir usando la información contextual para reconocer que tienen otro significado en "Reservar cena para dos en Patti's Diner" y "Buscar indicaciones de ruta en coche para Selma, Georgia". 

Agregar una lista de frases es una alternativa a la adición de más expresiones de ejemplo a una intención. 

## <a name="when-to-use-phrase-lists-versus-list-entities"></a>Cuándo se deben usar listas de frases frente a las entidades de lista
Aunque tanto una lista de frases como las entidades de lista pueden afectar a las expresiones de todas las intenciones, en cada caso se realiza de forma diferente. Use una lista de frases para afectar a la puntuación de predicción de intención. Use una entidad de lista para afectar a la extracción de la entidad de una coincidencia de texto exacta. 

### <a name="use-a-phrase-list"></a>Usar una lista de frases
Con una lista de frases, LUIS puede tener en cuenta el contexto y generalizar para identificar los elementos que son similares, pero no una coincidencia exacta, como elementos de una lista. Si es necesario que la aplicación de LUIS sea capaz de generalizar e identificar los elementos nuevos de una categoría, use una lista de frases. 

Si quiere poder reconocer las instancias nuevas de una entidad, como un programador de reuniones que deba reconocer los nombres de los contactos nuevos, o bien una aplicación de inventario que deba reconocer los productos nuevos, use otro tipo de entidad de aprendizaje automático como una entidad simple o jerárquica. Después, cree una lista de frases de palabras y frases que permitan a LUIS buscar otras palabras similares a la entidad. Esta lista sirve de guía a LUIS para reconocer los ejemplos de la entidad mediante la adición de más importancia al valor de esas palabras. 

Las listas de frases son similares al vocabulario específico de dominio que ayuda a mejorar la calidad de comprensión de las intenciones y entidades. Un uso común de una lista de frases es para nombres propios, como los nombres de ciudades. El nombre de una ciudad puede tener varias palabras incluidos guiones o apóstrofos.
 
### <a name="dont-use-a-phrase-list"></a>No usar una lista de frases 
Una entidad de lista define de forma explícita todos los valores que una entidad puede aceptar y solo identifica los valores que coinciden exactamente. Una entidad de lista puede ser adecuada para una aplicación en las que se conocen todas las instancias de una entidad y no cambian con frecuencia, como los elementos de comida en el menú de un restaurante que cambia con poca frecuencia. Si necesita una coincidencia de texto exacta de una entidad, no use una lista de frases. 

## <a name="best-practices"></a>Procedimientos recomendados
Conozca los [procedimientos recomendados](luis-concept-best-practices.md).

## <a name="next-steps"></a>Pasos siguientes

Vea [Agregar características](luis-how-to-add-features.md) para obtener más información sobre cómo agregar características a la aplicación de LUIS.