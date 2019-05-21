---
title: 'Estructura de nomenclatura de los analizadores: Linguistic Analysis API'
titlesuffix: Azure Cognitive Services
description: Obtenga información sobre la manera en que la estructura de nomenclatura del analizador de Linguistic Analysis API permite flexibilidad y precisión.
services: cognitive-services
author: RichardSunMS
manager: nitinme
ms.service: cognitive-services
ms.subservice: linguistic-analysis
ms.topic: conceptual
ms.date: 03/23/2016
ms.author: lesun
ROBOTS: NOINDEX
ms.openlocfilehash: c989f1115bc5a85bf09270c553ac1cb51bb4f170
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2019
ms.locfileid: "65954706"
---
# <a name="analyzer-names"></a>Nombres de analizador

> [!IMPORTANT]
> El 9 de agosto de 2018 se retiró la versión preliminar de Linguistic Analysis. Para el procesamiento y el análisis de texto, se recomienda usar los [módulos de análisis de texto de Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/studio-module-reference/text-analytics).

La estructura de nomenclatura que se usa con los analizadores es algo complicada a fin de permitir flexibilidad en los analizadores y precisión a la hora de comprender lo que significa un nombre.
Los nombres de analizador constan de cuatro partes: un identificador, un tipo, una especificación y una implementación.
El rol de cada componente se define a continuación.

## <a name="id"></a>ID
En primer lugar, un analizador tiene un identificador único; un GUID.
Estos GUID deben cambiar con relativa poca frecuencia, pero son la única manera de describir de forma exclusiva un analizador determinado.

## <a name="kind"></a>Clase
A continuación, cada analizador es de una **clase**.
La clase define en términos muy amplios el tipo de análisis devuelto, y debe definir de forma exclusiva la estructura de datos usada para representar ese análisis.
Actualmente, hay tres clases distintas:
 - [Tokens](Sentences-and-Tokens.md)
 - [Etiquetas de categoría gramatical](Pos-Tagging.md)
 - [Árbol de área](constituency-parsing.md)

## <a name="specification"></a>Especificación
Sin embargo, dentro de una clase dada, distintos expertos podrían discrepar en cómo se debe analizar un fenómeno determinado.
A diferencia de los lenguajes de programación, no hay ninguna definición clara y exacta de cómo debe hacerse.

Por ejemplo, imagine que estábamos intentando encontrar los tokens en la frase en inglés "No vayan."
En concreto, considere la cadena "didn't".
Una posible interpretación es que esta cadena debería dividirse en dos tokens: "did" y "not".
A continuación, la frase alternativa "que no se ha pasado" tendría el mismo conjunto de tokens.
Otra posibilidad consiste en indicar que se debe dividir en los tokens "did" y "n't".
Este último token no se consideraría normalmente una palabra, pero este enfoque conserva más información sobre la cadena de superficie, lo que a veces puede resultar útil.
O quizás, que la contracción se debe considerar una sola palabra.

Sin importar la elección que se haga, esa elección se debe realizar de forma coherente.
Este es precisamente el rol de una **especificación**: decidir lo que debe ser una representación correcta.

Las salidas del analizador solo pueden compararse razonablemente con los datos que se ajustan a la misma especificación.

## <a name="implementation"></a>Implementación

Con frecuencia, hay muchos modelos que intentan conseguir los mismos resultados, pero con diferentes características de rendimiento.
Un modelo podría ser más rápido pero menos preciso; otro podría realizar una concesión diferente.

La parte de **implementación** de un nombre de analizador se usa para identificar este tipo de información, de modo que los usuarios puedan elegir el analizador más adecuado para sus necesidades.
