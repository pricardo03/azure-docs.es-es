---
title: Expresiones en aplicaciones de LUIS en Azure | Microsoft Docs
description: Adición de expresiones en aplicaciones de Language Understanding Intelligent Service (LUIS).
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 02/13/2018
ms.author: diberry
ms.openlocfilehash: 6f962d0aaf631051c841be29d2854a89bf58ac25
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/24/2018
ms.locfileid: "39224422"
---
# <a name="utterances-in-luis"></a>Expresiones en LUIS

Las **expresiones** son datos proporcionados por el usuario que la aplicación necesita interpretar. Para entrenar a LUIS para que extraiga intenciones y entidades de ellas, es importante capturar varias entradas diferentes por cada intención. El aprendizaje activo o el proceso de entrenar continuamente en nuevas expresiones es esencial para la inteligencia de aprendizaje automático que LUIS proporciona.

Recopile frases que crea que los usuarios pueden escribir. Incluya expresiones que signifiquen lo mismo, pero que se construyan de forma diferente tanto en longitud de palabras como en el orden de las palabras. 

## <a name="how-to-choose-varied-utterances"></a>Cómo elegir expresiones variadas
Cuando empiece por primera vez a [agregar expresiones de ejemplo](luis-how-to-add-example-utterances.md) al modelo de LUIS, debe tener en cuenta algunos de los principios siguientes.

### <a name="utterances-arent-always-well-formed"></a>Las expresiones no siempre tienen el formato correcto
Puede ser una frase, como "Reservar un billete a París", o un fragmento de una frase, como "Reservar" o "Vuelo a París".  Los usuarios a menudo cometen errores ortográficos. Al planear la aplicación, tenga en cuenta si se revisará la ortografía de la entrada del usuario o no, antes de pasarla a LUIS. [Bing Spell Check API] [ BingSpellCheck] se integra con LUIS. Puede asociar la aplicación de LUIS con una clave externa para Bing Spell Check API al publicarla. Si no revisa la ortografía de las expresiones del usuario, debe entrenar a LUIS en expresiones que incluyan errores tipográficos y faltas de ortografía.

### <a name="use-the-representative-language-of-the-user"></a>Utilice el lenguaje representativo del usuario
Al elegir expresiones, tenga en cuenta que lo que cree que es un término o frase común tal vez no lo sea para el típico usuario de la aplicación cliente. Es posible que no tenga experiencia de dominio. Preste atención al utilizar términos o frases que solo las diría un experto.

### <a name="choose-varied-terminology-as-well-as-phrasing"></a>Elija terminología así como frases variadas
Observará que aunque realice esfuerzos para crear patrones de frases variadas,se seguirá repitiendo algo de vocabulario.

Considere las siguientes expresiones de ejemplo:
```
how do I get a computer?
Where do I get a computer?
I want to get a computer, how do I go about it?
When can I have a computer? 
```
En este caso, el término principal, "equipo", no varía. Se podría decir el equipo de escritorio, portátil, estación de trabajo o incluso simplemente máquina. LUIS deduce de forma inteligente los sinónimos a partir del contexto, pero cuando se crean expresiones para el entrenamiento, también es mejor variarlas.

## <a name="example-utterances-in-each-intent"></a>Expresiones de ejemplo en cada intención
Cada intención debe tener expresiones de ejemplo, al menos de 10 a 15. Si tiene una intención que no tiene ninguna expresión de ejemplo, no podrá entrenar a LUIS. Si dispone de una intención con una o muy pocas expresiones de ejemplo, LUIS no podrá predecirla con precisión. 

## <a name="add-small-groups-of-10-15-utterances-for-each-authoring-iteration"></a>Adición de grupos pequeños de 10 a 15 expresiones para cada iteración de creación
En cada iteración del modelo, no agregue una gran cantidad de expresiones. Agregue expresiones de diez en diez. [Entrene](luis-how-to-train.md), [publique](luis-how-to-publish-app.md) y vuelva a [realizar pruebas](luis-interactive-test.md).  

LUIS compila modelos efectivos con expresiones seleccionadas cuidadosamente. Agregar demasiadas expresiones no resulta útil porque genera confusión.  

Es mejor empezar con pocas expresiones y, luego, [revisar las expresiones del punto de conexión](luis-how-to-review-endoint-utt.md) para que la extracción de la entidad y la predicción de intención se realicen correctamente.

## <a name="ignoring-words-and-punctuation"></a>Omisión de palabras y puntuación
Si quiere omitir palabras específicas o signos de puntuación en la expresión de ejemplo, use un [patrón](luis-concept-patterns.md#pattern-syntax) con la sintaxis _ignore_. 

## <a name="training-utterances"></a>Expresiones de entrenamiento
El entrenamiento no es determinista: la predicción de expresiones podría variar ligeramente entre versiones o aplicaciones.

## <a name="testing-utterances"></a>Prueba de expresiones 

Los desarrolladores deben empezar a probar su aplicación de LUIS con tráfico real mediante el envío de expresiones al punto de conexión. Estas expresiones se utilizan para mejorar el rendimiento de las intenciones y las entidades con la [revisión de expresiones](luis-how-to-review-endoint-utt.md). Las pruebas enviadas mediante el panel de pruebas del sitio web de LUIS no se envían a través del punto de conexión y, por lo tanto, no contribuyen al aprendizaje activo. 

## <a name="review-utterances"></a>Revisión de las expresiones
Una vez que el modelo esté entrenado, publicado y reciba consultas del [punto de conexión](luis-glossary.md#endpoint), [revise las expresiones](luis-how-to-review-endoint-utt.md) sugeridas por LUIS. LUIS selecciona expresiones del punto de conexión que tienen puntuaciones bajas, ya sea para la intención o para la entidad. 

## <a name="best-practices"></a>Procedimientos recomendados
Revise los [procedimientos recomendados](luis-concept-best-practices.md) para obtener más información.

## <a name="next-steps"></a>Pasos siguientes
Consulte [Agregar expresiones de ejemplo](luis-how-to-add-example-utterances.md) para información sobre cómo entrenar una aplicación de LUIS para comprender las expresiones del usuario.

[BingSpellCheck]: https://docs.microsoft.com/azure/cognitive-services/bing-spell-check/proof-text