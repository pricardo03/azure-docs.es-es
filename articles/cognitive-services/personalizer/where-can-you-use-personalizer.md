---
title: 'Evaluación de escenarios: Personalizer'
titleSuffix: Azure Cognitive Services
description: Personalizer se puede aplicar en cualquier situación en la que su aplicación pueda seleccionar el artículo, acción o producto correcto que se quiere mostrar, con el fin de mejorar la experiencia, lograr mejores resultados comerciales o mejorar la productividad.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 09/19/2019
ms.author: diberry
ms.openlocfilehash: 246e76a0ab94624945723b500ef136e038ab40ec
ms.sourcegitcommit: 116bc6a75e501b7bba85e750b336f2af4ad29f5a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/20/2019
ms.locfileid: "71155225"
---
# <a name="where-can-you-use-personalizer"></a>¿Dónde se puede utilizar Personalizer?

Utilice Personalizer en cualquier situación en la que su aplicación necesite seleccionar el artículo, acción o producto correcto que se quiere mostrar, con el fin de mejorar la experiencia, lograr mejores resultados comerciales o mejorar la productividad. 

Personalizer utiliza el aprendizaje automático para seleccionar qué acción mostrar al usuario. La selección puede variar drásticamente según la cantidad, calidad y distribución de los datos enviados al servicio.

### <a name="checklist-for-applying-personalizer"></a>Lista de comprobación para aplicar Personalizer


Puede aplicar Personalizer en situaciones donde:

* Tiene un objetivo de negocio o de usabilidad para la aplicación.
* Tiene un lugar en la aplicación donde tomar una decisión contextual sobre qué mostrar a los usuarios para mejorar ese objetivo.
* La mejor opción puede y debe aprenderse del comportamiento colectivo de los usuarios y de la puntuación total de recompensa.
* El uso del aprendizaje automático para la personalización sigue las [directrices de uso responsable](ethics-responsible-use.md) y las opciones que se eligen.
* La decisión contextual puede expresarse como la clasificación de la mejor opción (acción) de un conjunto limitado de opciones.
* La eficacia de la opción de clasificación para la aplicación puede determinarse mediante la medición de algún aspecto del comportamiento del usuario y de su expresión en una _[puntuación de recompensa](concept-rewards.md)_ .
* La puntuación de recompensa no aporta demasiados factores de confusión o externos. La duración del experimento es lo suficientemente baja como para que la puntuación de recompensa pueda calcularse mientras sea pertinente.
* Puede expresar el contexto para la clasificación como una lista de al menos cinco [características](concepts-features.md) que, en su opinión, ayudarían a tomar la decisión correcta, y que no incluye información personal identificable. (PII).
* Tiene información sobre cada opción de contenido, o _acción_, como una lista de al menos cinco [características](concepts-features.md) que, en su opinión, ayudarán a Personalizer a tomar la decisión correcta.
* La aplicación puede retener datos durante el tiempo suficiente para acumular un historial de al menos 100 000 interacciones.

## <a name="machine-learning-considerations-for-applying-personalizer"></a>Consideraciones de aprendizaje automático para aplicar Personalizer

Personalizer se basa en el aprendizaje de refuerzo, un enfoque del aprendizaje automático que se enseña mediante los comentarios de ofrecen los usuarios. 

Personalizer aprenderá mejor en situaciones en las que:

* Hay suficientes eventos para estar al tanto de la personalización óptima si el problema se desvía con el tiempo (como las preferencias en las noticias o la moda). Personalizer se adaptará al cambio continuo en el mundo real, pero los resultados no serán óptimos si no hay suficientes eventos y datos de los que aprender para detectar y establecer nuevos patrones. Debe elegir un caso de uso que ocurra con suficiente frecuencia. Considere la posibilidad de buscar casos de uso que ocurran al menos 500 veces al día.
* El contexto y las acciones tienen [ características](concepts-features.md) suficientes para facilitar el aprendizaje.
* Hay menos de 50 acciones para clasificar por llamada.
* La configuración de retención de datos permite a Personalizer recopilar suficientes datos para realizar evaluaciones sin conexión y optimizar las directivas. Por lo general, se trata de al menos 50 000 puntos de datos.

## <a name="monitor-effectiveness-of-personalizer"></a>Supervisión de la eficacia de Personalizer

Para supervisar la eficacia de Personalizer periódicamente, realice las [evaluaciones sin conexión](concepts-offline-evaluation.md).

## <a name="use-personalizer-with-recommendation-engines"></a>Uso de Personalizer con los motores de recomendación

Muchas empresas utilizan motores de recomendación, herramientas de marketing y campañas, segmentación y agrupación de audiencias, filtrado colaborativo y otros medios para recomendar productos de un gran catálogo a los clientes.

El [repositorio GitHub de Microsoft Recommenders](https://github.com/Microsoft/Recommenders) proporciona ejemplos y procedimientos recomendados para crear sistemas de recomendación, proporcionados como cuadernos de Jupyter Notebook. Proporciona ejemplos prácticos para la preparación de datos, la creación de modelos, la evaluación, el ajuste y la puesta en marcha de los motores de recomendación, para muchos enfoques comunes, entre ellos xDeepFM, SAR, ALS, RBM y DKN.

Personalizer puede trabajar con un motor de recomendación cuando está presente.

* Los motores de recomendación toman grandes cantidades de elementos (por ejemplo, 500 000) y recomiendan un subconjunto (como los 20 primeros) de cientos o miles de opciones.
* Personalizer toma un pequeño número de acciones con mucha información sobre ellas y las clasifica en tiempo real para un contexto rico determinado, mientras que la mayoría de los motores de recomendación solo usan unos pocos atributos sobre los usuarios, los productos y sus interacciones.
* Personalizer está diseñado para explorar de forma autónoma las preferencias del usuario todo el tiempo, lo que dará mejores resultados donde el contenido está cambiando rápidamente, como noticias, eventos en vivo, contenido de la comunidad en vivo, contenido con actualizaciones diarias o contenido estacional.

Un uso común es tomar la salida de un motor de recomendación (por ejemplo, los 20 mejores productos para un determinado cliente) y utilizarlo como las acciones de entrada para Personalizer.

## <a name="next-steps"></a>Pasos siguientes

[Ética y uso responsable](ethics-responsible-use.md).