---
title: 'Preguntas más frecuentes de Text Analytics API: Azure Cognitive Services | Microsoft Docs'
description: Obtenga respuestas a preguntas habituales acerca de Text Analytics API de Microsoft Cognitive Services en Azure.
services: cognitive-services
author: HeidiSteen
manager: cgronlun
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: conceptual
ms.date: 3/07/2018
ms.author: heidist
ms.openlocfilehash: bf82899b4317f0f5ce0f6ca5096dccef7cddd931
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35382539"
---
# <a name="frequently-asked-questions-faq-about-the-text-analytics-api"></a>Preguntas más frecuentes (P+F) sobre Text Analytics API

 Encuentre respuestas a preguntas habituales sobre conceptos, códigos y escenarios relacionados con Text Analytics API para Microsoft Cognitive Services en Azure.

## <a name="can-text-analytics-identify-sarcasm"></a>¿Text Analytics puede identificar sarcasmo?

El análisis sirve para detectar opiniones positivas o negativas, no estados de ánimo.

Siempre hay cierto grado de imprecisión en el análisis de opiniones, pero el modelo es muy útil cuando no hay ningún significado oculto ni subtexto en el contenido. La ironía, el sarcasmo, el humor y el contenido con matices similares dependen del contexto cultural y de normas para transmitir la intención. Este tipo de contenido se encuentra entre los más difíciles de analizar. Normalmente, la mayor discrepancia entre una determinada puntuación generada por el analizador y una valoración subjetiva realizada por una persona es el contenido con significado matizado.

## <a name="can-i-add-my-own-training-data-or-models"></a>¿Puedo agregar mis propios datos o modelos de aprendizaje?

No, los modelos se entrenan previamente. Las únicas operaciones disponibles en los datos cargados son la puntuación, la extracción de frases clave y la detección de idioma. No hospedamos modelos personalizados. Si quiere crear y hospedar modelos de aprendizaje automático personalizado, considere las [funcionalidades de aprendizaje automático en Microsoft R Server](https://docs.microsoft.com/r-server/r/concept-what-is-the-microsoftml-package).

## <a name="can-i-request-additional-languages"></a>¿Puedo solicitar más idiomas?

El análisis de opiniones y la extracción de frases clave están disponibles para un [exclusivo número de idiomas](text-analytics-supported-languages.md). El procesamiento de lenguaje natural es complejo y requiere una comprobación sustancial para poder lanzar la nueva funcionalidad. Por este motivo, evitamos anunciar la compatibilidad previamente para que nadie dependa de una funcionalidad que necesita más tiempo para desarrollarse. 

Para ayudarnos a dar prioridad a que idiomas trabajar a continuación, vote por idiomas específicos en [UserVoice](https://cognitive.uservoice.com/forums/555922-text-analytics). 

## <a name="why-does-key-phrase-extraction-return-some-words-but-not-others"></a>¿Por qué la extracción de frases clave devuelve algunas palabras pero no otras?

La extracción de frases clave elimina las palabras que no son esenciales y los adjetivos independientes. Las combinaciones de adjetivo-nombre, como "vistas espectaculares" o "tiempo nublado" se devuelven juntas.

Normalmente, la salida consiste en nombres y objetos de la oración. La salida se muestra según el orden de importancia, con la primera frase como la más importante. La importancia se mide según el número de veces que se menciona un determinado concepto o según la relación de ese elemento con otros del texto.

## <a name="why-does-output-vary-given-identical-inputs"></a>¿Por qué varía la salida di si las entradas son idénticas?

Los algoritmos y las mejoras en los modelos se anuncian si el cambio es principal o se realiza una instalación integrada discretamente en el servicio si la actualización es secundaria. Con el tiempo, es posible que encuentre los mismos resultados de entrada de texto en una puntuación de opiniones o en una salida de la frase clave diferentes. Se trata de una consecuencia normal e intencionada del uso de recursos de aprendizaje automático administrados en la nube.

## <a name="next-steps"></a>Pasos siguientes

¿Es su pregunta acerca de una característica o funcionalidad que falta? Considere la posibilidad de solicitarla o votarla en nuestro [sitio web de UserVoice](https://cognitive.uservoice.com/forums/555922-text-analytics).

## <a name="see-also"></a>Otras referencias

 [StackOverflow: Text Analytics API](https://stackoverflow.com/questions/tagged/text-analytics-api)   
 [StackOverflow: Cognitive Services](http://stackoverflow.com/questions/tagged/microsoft-cognitive)
