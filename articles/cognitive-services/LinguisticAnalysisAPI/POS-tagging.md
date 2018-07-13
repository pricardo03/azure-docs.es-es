---
title: Etiquetado de categorías gramaticales en Linguistic Analysis API | Microsoft Docs
description: Conozca cómo el etiquetado de categorías gramaticales en Microsoft Cognitive Services permite identificar la categoría o función de cada palabra del texto.
services: cognitive-services
author: RichardSunMS
manager: wkwok
ms.service: cognitive-services
ms.component: linguistic-analysis
ms.topic: article
ms.date: 09/27/2016
ms.author: lesun
ms.openlocfilehash: 35d2042db5f8d64c7dbd1df6bfdebcba6d6aab28
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35380290"
---
# <a name="part-of-speech-tagging"></a>Etiquetado de categorías gramaticales

## <a name="background-and-motivation"></a>Contexto y motivación

Una vez que un texto se ha dividido en frases y elementos, el siguiente paso del análisis es identificar la categoría gramatical de cada palabra.
Puede tratarse de categorías como *sustantivo* (que por lo general representa personas, lugares, cosas, ideas, etc.) y *verbo* (que generalmente representa acciones, cambios de estado, etc). Para algunas palabras, la categoría gramatical es inequívoca (por ejemplo, *pantano* solo puede ser en realidad un nombre), pero en muchos otros casos, resulta difícil saber la categoría.
*Corro* podría ser un círculo de personas, pero puede ser también un verbo en "corro la carrera".

## <a name="list-of-part-of-speech-tags"></a>Lista de etiquetas de categorías gramaticales

| Etiqueta | DESCRIPCIÓN | Palabras de ejemplo |
|-----|-------------|---------------|
| $ | dólar | $ |
| \`\` | comillas de apertura | \` \`\` |
| '' | comillas de cierre | ' '' |
| ( | paréntesis de apertura | ( [ { |
| ) | paréntesis de cierre | ) ] } |
| . | coma | . |
| -- | guión | -- |
| . | terminador de frase | . ! ? |
| : | dos puntos o puntos suspensivos | : ; ... |
| CC | conjunción, coordinación | "y", "pero" o "todavía"|
| CD | numeral, cardinal | nueve, 20, 1980, '96 |
| DT | determinante |"un", "el", "una", "todos", "ambas", "ninguna"|
| EX | existencial | allí |
| FW | palabra extranjera | enfant terrible hoi polloi je ne sais quoi |
| IN | preposición o conjunción subordinada| "en", "dentro de", "si", "después de" |
| JJ | adjetivo o numeral, ordinal | "noveno", "bastante", "execrable", "multimodal" |
| JJR | adjetivo, comparativo | "mejor", "más rápido", "más barato" |
| JJS | adjetivo, superlativo | "el mejor", "el más barato", "el más rápido" | 
| LS | marcador de elemento de lista | (a) (b) 1 2 A B A. B. |
| MD | auxiliar modal | "puede", "podría", "debería" |
| NN | nombre común, singular o plural | "patata", "dinero", "zapato" |
| NNP | nombre propio singular | "Kennedy", "Roosevelt", "Chicago", "Weehauken" |
| NNPS | nombre propio plural | "Springfields", "Bushes" |
| NNS | nombre común plural | "piezas", "ratones", "campos" |
| PDT | predeterminante | todos, ambos, muchos, bastantes, esta |
| POS | marcador de genitivo (en inglés) | ' 's |
| PRP | pronombre personal | ella, él, ello, yo, nosotros, vosotros, ellos |
| PRP$ | pronombre posesivo | suya, suyo, mío, nuestro, vuestro |
| RB | adverbio | clínicamente, solamente |
| RBR | adverbio comparativo | más lejos, más sombrío, más grandioso, más grave, más grande, más triste, más difícil, más duro, más sano, más pesado, más alto, sin embargo, mayor, más tarde, más esbelto, más largo, menos perfectamente, menor, más solo, más alto, más bajo y muchos otros... |
| RBS | adverbio superlativo | el mejor, el más grande, el más contundente, el más temprano, el más lejos, el primero, el más difícil, el más abundante, el más alto, el más grande, el menos, el más cercano, el segundo, el más apretado, el peor |
| RP | partícula | en, sobre, hasta |
| SYM | símbolo | %, & |
| TO | "to" como preposición o marcador de infinitivo (en inglés) | to |
| UH | interjección | hurra, hey, hola |
| VB | verbo, forma básica | dar, asignar, volar |
| VBD | verbo, tiempo verbal pasado | dio, asignó, voló |
| VBG | verbo, participio presente o gerundio | dando, asignando, volando |
| VBN | verbo, participio pasado | dado, asignado, volado |
| VBP | verbo, tiempo verbal presente, no es la tercera persona del singular | dais, asignáis, voláis |
| VBZ | verbo, tiempo verbal presente, tercera persona del singular | da, asigna, vuela |
| WDT | Determinante | que, cual |
| WP | Pronombre interrogativo | qué, quién |
| WP$ | pronombre interrogativo posesivo | de quién |
| WRB | Adverbio interrogativo | cuánto, sin embargo, siempre que, dónde |

## <a name="specification"></a>Especificación

Para la tokenización, nos basamos en las especificaciones de [Treebank Penn](https://www.cis.upenn.edu/~treebank/).
