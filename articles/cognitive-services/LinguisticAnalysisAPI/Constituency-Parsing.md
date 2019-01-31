---
title: 'Análisis de elementos de la oración: Linguistic Analysis API'
titlesuffix: Azure Cognitive Services
description: Más información sobre cómo el análisis de elementos de la oración, también conocido como "análisis de la estructura de sintagmas", identifica sintagmas del texto.
services: cognitive-services
author: RichardSunMS
manager: cgronlun
ms.service: cognitive-services
ms.subservice: linguistic-analysis
ms.topic: conceptual
ms.date: 03/21/2016
ms.author: lesun
ROBOTS: NOINDEX
ms.openlocfilehash: 778005ee321c981fec1d1271cee54229bb3a522f
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55214184"
---
# <a name="constituency-parsing"></a>Análisis de elementos de la oración

> [!IMPORTANT]
> El 9 de agosto de 2018 se retiró la versión preliminar de Linguistic Analysis. Para el procesamiento y el análisis de texto, se recomienda usar los [módulos de análisis de texto de Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/studio-module-reference/text-analytics).

El objetivo del análisis de elementos de la oración, también conocido como "análisis de la estructura de sintagmas", es identificar sintagmas del texto.
Esto puede ser útil al extraer información de un texto.
Puede que los clientes quieran encontrar nombres de características o sintagmas importantes en un texto grande y ver los modificadores y acciones que rodean a ese sintagma.

## <a name="phrases"></a>Sintagma

Para un lingüista, un *sintagma* es algo más que una secuencia de palabras.
Para que sea un sintagma, un grupo de palabras se debe agrupar para desempeñar un papel específico en la frase.
Ese grupo de palabras puede moverse conjuntamente o reemplazarse como un conjunto y la frase debería conservar la fluidez y ser correcta gramaticalmente.

Observe la oración

> "Yo quiero encontrar un nuevo automóvil híbrido con Bluetooth".

Esta frase contiene el sintagma nominal: "un nuevo automóvil híbrido con Bluetooth".
¿Cómo sabemos que se trata de un sintagma?
Podemos reescribir la frase (de forma un tanto poética) moviendo ese sintagma entero a la parte delantera:

> "Un nuevo automóvil híbrido con Bluetooth yo quiero encontrar".

O bien, se podría reemplazar ese sintagma por otro sintagma de función y significado similares como "un nuevo coche sofisticado":

> "Yo quiero encontrar un nuevo coche sofisticado".

Si en lugar de eso hubiéramos elegido un subconjunto diferente de palabras, las tareas de sustitución habrían generado frases extrañas o ininteligibles.
Observe lo que ocurre cuando se mueve "encontrar un nuevo" a la parte delantera:

> "Encontrar un nuevo yo quiero automóvil híbrido con Bluetooth".

Es muy difícil de leer y entender el resultado.

El objetivo de un analizador es encontrar todos estos sintagmas.
Curiosamente, en el lenguaje natural, los sintagmas tienden a estar anidados unos dentro de otros.
Una representación natural de estos sintagmas en un árbol como el siguiente:

![Árbol](./Images/tree.png)

En este árbol, las bifurcaciones marcadas con "NP" son sintagmas nominales.
Hay varios de estos sintagmas: *yo*, *un nuevo automóvil híbrido*, *Bluetooth* y *un nuevo automóvil híbrido con Bluetooth*.

## <a name="phrase-types"></a>Tipos de sintagmas

| Etiqueta | DESCRIPCIÓN | Ejemplo |
|-------|-------------|---------|
|ADJP   | Sintagma adjetival | "tan maleducado" |
|ADVP   | Sintagma adverbial | "suficientemente claro" |
|CONJP  | Sintagma conjuntivo | "así como " |
|FRAG   | Fragmento, que se utiliza para las entradas incompletas o fragmentadas | "Muy recomendable..." |
|INTJ   | Interjección | "Hurra" |
|LST    | Marcador de lista, incluidos los signos de puntuación | "#4)" |
|NAC    | "No elemento de la oración", se utiliza para indicar el ámbito de un sintagma que no es un elemento de la oración |  "y a un buen precio" en "consigues todas las cosas y a un buen precio" |
|NP | Sintagma nominal | "un delicioso crepe de patata" |
|NX | Se usa dentro de determinados sintagmas nominales complejos para marcar el elemento principal| |
|PP | Sintagma preposicional| "en el grupo" |
|PRN    | Inciso| "(se llama así)" |
|PRT    | Partícula| "out" en "ripped out" |
|QP | Sintagma cuantificador (es decir, un importe o medida complejos) dentro de un sintagma nominal| "alrededor de 75 EUR" |
|RRC    | Oraciones de relativo reducidas.| "aún sin resolver" en "muchos problemas aún sin resolver" |
|S  | Frase u oración. | "Esta es una frase".
|SBAR   | Oración subordinada, introducida normalmente por una conjunción de subordinación | "cuando salí" en "Miré alrededor cuando salí".|
|SBARQ  | Pregunta directa introducida por un pronombre o término interrogativo | "¿Qué es lo más importante?" |
|SINV   | Oración enunciativa negativa | "En ningún momento fueron ellos conscientes". (observe cómo el sujeto normal "ellos" se ha desplazado detrás del verbo "fueron") |
|SQ | Preguntas dicotómicas u oración principal de una pregunta con interrogativo | "¿Consiguieron el coche?" |
|UCP    | Sintagma coordinado copulativo| "pequeño y con bichos" (observe cómo un adjetivo y un sintagma preposicional se unen mediante "y")|
|VP | Sintagma verbal | "corrió al bosque" |
|WHADJP | Sintagma adjetival exclamativo | "qué incómodo" |
|WHADVP | Sintagma adverbial interrogativo| "cuándo" |
|WHNP   | Sintagma nominal interrogativo| "qué patata", "cuánta sopa"|
|WHPP   | Sintagma preposicional interrogativo| "en qué país"|
|X  | Desconocido, incierto o sin paréntesis.| el primer "la" en "la... la sopa" |


## <a name="specification"></a>Especificación

Aquí los árboles utilizan las expresiones S del [Treebank Penn](https://catalog.ldc.upenn.edu/LDC99T42).
