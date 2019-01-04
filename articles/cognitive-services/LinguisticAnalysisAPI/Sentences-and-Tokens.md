---
title: 'Oraciones y tokens: Linguistic Analysis API'
titlesuffix: Azure Cognitive Services
description: Obtenga información sobre la separación y la tokenización de oraciones en Linguistic Analysis API.
services: cognitive-services
author: DavidLiCIG
manager: cgronlun
ms.service: cognitive-services
ms.component: linguistic-analysis
ms.topic: conceptual
ms.date: 03/21/2016
ms.author: davl
ROBOTS: NOINDEX
ms.openlocfilehash: 96c2bd1c11554481e441662a6051620f4b2e2993
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/02/2019
ms.locfileid: "53972369"
---
# <a name="sentence-separation-and-tokenization"></a>Separación y tokenización de oraciones

> [!IMPORTANT]
> El 9 de agosto de 2018 se retiró la versión preliminar de Linguistic Analysis. Para el procesamiento y el análisis de texto, se recomienda usar los [módulos de análisis de texto de Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/studio-module-reference/text-analytics).

## <a name="background-and-motivation"></a>Contexto y motivación

En un cuerpo de texto, el primer paso del análisis lingüístico es dividirlo en oraciones y tokens.

### <a name="sentence-separation"></a>Separación de oraciones

A primera vista, puede parecer sencillo dividir el texto en oraciones: basta con buscar los marcadores de fin de oración y separar por ahí las oraciones.
Sin embargo, con frecuencia estas marcas son complicadas y ambiguas.

Considere el texto de ejemplo siguiente.

> What did you say?!? I didn't hear about the director's "new proposal." It's important to Mr. and Mrs. Smith.

Este texto contiene tres oraciones:

- What did you say?!?
- I didn't hear about the director's "new proposal."
- It's important to Mr. and Mrs. Smith.

Observe cómo los finales de las oraciones se marcan de muchas maneras diferentes.
La primera finaliza en una combinación de signos de interrogación y exclamación (en ocasiones se le conoce como signo de interrogación de cierre relleno).
La segunda finaliza en un punto, pero la siguiente marca de comillas se debe insertar en la oración anterior.
En la tercera oración, puede ver cómo ese mismo carácter de punto se puede usar también para marcar las abreviaturas.
Con solo examinar la puntuación, tenemos un buen conjunto de candidatos, pero es necesario un análisis más profundo para identificar los límites verdaderos de la oración.

### <a name="tokenization"></a>Tokenización

La siguiente tarea consiste en dividir estas oraciones en tokens.
En su mayor parte, los tokens de inglés están delimitados por espacios en blanco.
(Buscar tokens o palabras es mucho más fácil en inglés que en chino, donde no es común el uso de espacios entre palabras.
La primera oración podría escribirse como "Whatdidyousay?")

Hay algunos casos difíciles.
En primer lugar, los signos de puntuación se deben separar a menudo (aunque no siempre) del contexto que los rodea.
En segundo lugar, el inglés tiene *contracciones*, como "didn't" o "it's", donde las palabras se han comprimido y abreviado en trozos más pequeños.
El objetivo del tokenizador consiste en dividir la secuencia de caracteres en palabras.

Vamos a volver a las oraciones del ejemplo anterior.
Ahora hemos colocado un "punto central" (&middot;) entre cada token distintivo.

- What &middot; did &middot; you &middot; say &middot; ?!?
- I &middot; did &middot; n't &middot; hear &middot; about &middot; the &middot; director &middot; 's &middot; " &middot; new &middot; proposal &middot; . &middot; "
- It &middot; 's &middot; important &middot; to &middot; Mr. &middot; and &middot; Mrs. &middot; Smith &middot; .

Observe cómo la mayoría de los tokens son palabras que encontraría en el diccionario (por ejemplo, *important*, *director*).
Otros constan solamente de signos de puntuación.
Por último, hay tokens más inusuales para representar contracciones como *n't* para *not* y posesivos como *'s*.
Esta tokenización nos permite administrar la palabra *didn't* y la frase *did not* de forma más coherente.

## <a name="specification"></a>Especificación

Es importante tomar decisiones coherentes sobre lo que constituye una oración y un token.
Nos basamos en las especificaciones del [Treebank Penn](https://catalog.ldc.upenn.edu/LDC99T42) (hay algunos detalles adicionales disponibles en ftp://ftp.cis.upenn.edu/pub/treebank/public_html/tokenization.html).
