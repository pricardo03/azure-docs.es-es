---
title: Panel - Language Understanding
titleSuffix: Azure Cognitive Services
description: Corregir las intenciones con el panel de resumen de análisis, una herramienta de informes visualizado.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 05/07/2019
ms.author: diberry
ms.openlocfilehash: a518a697369ff74689a0c4ac05af96453b6a5ca4
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65072458"
---
# <a name="how-to-use-the-dashboard-to-improve-your-app"></a>Cómo usar el panel para mejorar la aplicación

Buscar y corregir problemas con las intenciones de la aplicación entrenado cuando usas grabaciones de voz de ejemplo. El panel de resumen muestra información general de la aplicación, con los aspectos destacados de intenciones que deben solucionarse. 

Revise el panel de análisis es un proceso iterativo, puede repetido cambiar y mejorar el modelo.

Esta página no tendrán análisis pertinentes para las aplicaciones que no tienen las grabaciones de voz de ejemplo en las intenciones, conocidas como _solo patrón_ aplicaciones. 

## <a name="what-issues-can-be-fixed-from-dashboard"></a>¿Qué problemas pueden solucionarse desde el panel?

Los tres problemas solucionados en el panel son:

|Problema|Color del gráfico|Explicación|
|--|--|--|
|Desequilibrio de datos|-|Esto ocurre cuando la cantidad de grabaciones de voz de ejemplo varía significativamente. Deben tener todos los intentos _aproximadamente_ el mismo número de grabaciones de voz de ejemplo - excepto la intención None. Solo debe tener 10 al 15% de la cantidad total de grabaciones de voz en la aplicación.<br><br> Si los datos están desequilibrados, pero la intención precisión está por encima de umbral determinado, este desequilibrio no se notifica como un problema.<br><br>**Iniciar con este problema: puede ser la causa raíz de los otros problemas.**|
|Predicciones claras|Naranja|Esto sucede si son lo suficientemente que puede voltear el aprendizaje de la siguiente, debido a la intención superior y las puntuaciones de la intención siguiente [muestreo negativo](luis-how-to-train.md#train-with-all-data) o más declaraciones de ejemplo se agrega a la intención. |
|Predicciones incorrectas|Rojo|Esto se produce cuando no se prevé una declaración de ejemplo para la intención etiquetada (la intención es en).|

Predicciones correctas se representan con el color azul.

El panel de resumen muestra estos problemas y le indica qué intentos se ven afectados y sugiere lo que debe hacer para mejorar la aplicación. 

## <a name="before-app-is-trained"></a>Antes de que se entrena la aplicación 

Antes de entrenar la aplicación, el panel de resumen no contiene alguna sugerencia para correcciones. Entrenar su aplicación para ver estas sugerencias.  

## <a name="check-your-publishing-status"></a>Comprobar el estado de publicación

El **estado de publicación** tarjeta contiene información sobre el activo de publicación de última versión. 

Compruebe que la versión activa es la versión que desea corregir. 

![Servicios externos de la aplicación de panel de resumen muestra, publica las regiones y agregan visitas de punto de conexión.](./media/luis-how-to-use-dashboard/analytics-card-1-shows-app-summary-and-endpoint-hits.png)

Esto también muestra los servicios externos, regiones publicadas y agrega visitas de punto de conexión. 

## <a name="review-training-evaluation"></a>Evaluación de la formación de revisión

El **evaluación entrenamiento** tarjeta contiene el resumen agregado de la precisión general de la aplicación por área. La puntuación indica la calidad de la intención. 

![La tarjeta de evaluación de entrenamiento contiene la primera área de información sobre la precisión general de la aplicación.](./media/luis-how-to-use-dashboard/analytics-card-2-shows-app-overall-accuracy.png)

El gráfico indica las intenciones predichas correctamente y las áreas problemáticas con diferentes colores. Mejorar la aplicación con las sugerencias, esto aumenta de puntuación. 

Las correcciones sugeridas se dividen por tipo de problema y son más importantes para la aplicación. Si prefiere revisar y corregir problemas por intención, use el **[intenciones con errores](#intents-with-errors)** tarjeta en la parte inferior de la página. 

Cada área de problema tiene intenciones que deben corregirse. Cuando se selecciona el nombre de la intención, el **intención** se abre la página con un filtro aplicado a las grabaciones de voz. Este filtro permite centrarse en las declaraciones que están causando el problema.

### <a name="compare-changes-across-versions"></a>Compare los cambios entre versiones

Crear una nueva versión antes de realizar cambios en la aplicación. En la nueva versión, realice los cambios sugeridos en declaraciones de la intención ejemplo y luego volver a entrenar. En la página panel **evaluación entrenamiento** tarjeta, use el **Mostrar cambios de versión entrenado** para comparar los cambios. 

![Compare los cambios entre versiones](./media/luis-how-to-use-dashboard/compare-improvement-across-versions.png)

### <a name="fix-version-by-adding-or-editing-example-utterances-and-retraining"></a>Corregir la versión agregando o grabaciones de voz de ejemplo de edición y reciclaje

Agregar o editar las grabaciones de voz de ejemplo y volver a entrenar será el principal método de corrección de la aplicación. Deben seguir las directrices para las grabaciones de voz nuevas o modificadas [variados grabaciones de voz](luis-concept-utterance.md).

Adición de grabaciones de voz de ejemplo debe realizarse por alguien que:

* tiene un alto grado de conocimiento de las grabaciones de voz que se encuentran en las distintas intenciones
* sepa cómo pueden llevar a confusión grabaciones de voz en uno intención con otro intento
* puede decidir si las dos intenciones, que se confunden a menudo entre sí, se deben contraer en un único intento y extraen los datos diferentes con entidades

### <a name="patterns-and-phrase-lists"></a>Patrones y las listas de frase

La página de análisis no indica cuándo se debe usar [patrones](luis-concept-patterns.md) o [frase listas](luis-concept-feature.md). Si se agregan, puede ayudarle con las predicciones incorrectas o confusa, pero no ayuda con desequilibrio de datos. 

### <a name="review-data-imbalance"></a>Desequilibrio de datos de revisión

Iniciar con este problema: puede ser la causa raíz de los otros problemas.

El **desequilibrio datos** intención lista muestra las intenciones que necesitan más declaraciones para corregir el desequilibrio de datos. 

**Para corregir este problema**:

* A continuación, agregar más declaraciones a la intención de volver a entrenar. 

No agregue grabaciones de voz en intención None, a menos se sugiere en el panel de resumen.

> [!Tip]
> Utilice la tercera sección de la página, **grabaciones de voz por intención** con el **grabaciones de voz (número)** establecer como una guía visual rápida de las cuales las intenciones necesitan más declaraciones.  
    ![Use 'Grabaciones de voz (número)' para buscar las intenciones con desequilibrio de datos.](./media/luis-how-to-use-dashboard/predictions-per-intent-number-of-utterances.png)

### <a name="review-incorrect-predictions"></a>Revisar las predicciones incorrectas

El **predicción incorrecta** intención lista muestra las intenciones que tienen declaraciones, que se utilizan como ejemplos para un propósito específico, sino que se predicen para diferentes intenciones. 

**Para corregir este problema**:

* Editar grabaciones de voz para volver a ser más específico a la intención y "Train".
* Combinar las intenciones si grabaciones de voz demasiado estrechamente y volver a entrenar.

### <a name="review-unclear-predictions"></a>Revisar las predicciones claras

El **predicción claro** intención lista muestra las intenciones con declaraciones con puntuaciones de predicción que no se logre forma desde su rival más cercano, que puede cambiar la intención superior para la declaración en el curso siguiente, debido a [ el muestreo negativo](luis-how-to-train.md#train-with-all-data).

**Para corregir este problema**;

* Editar grabaciones de voz para volver a ser más específico a la intención y "Train".
* Combinar las intenciones si grabaciones de voz demasiado estrechamente y volver a entrenar.

## <a name="utterances-per-intent"></a>Grabaciones de voz por intención

Esta tarjeta muestra el estado general de la aplicación entre los intentos. Como corregir las intenciones y reciclaje, siguen Eche un vistazo a esta tarjeta para los problemas.

El gráfico siguiente muestra una aplicación y equilibrada con casi sin problemas para corregirlos.

![El gráfico siguiente muestra una aplicación y equilibrada con casi sin problemas para corregirlos.](./media/luis-how-to-use-dashboard/utterance-per-intent-shows-data-balance.png)

El gráfico siguiente muestra una aplicación mal equilibrada con muchos problemas para corregirlos.

![El gráfico siguiente muestra una aplicación y equilibrada con casi sin problemas para corregirlos.](./media/luis-how-to-use-dashboard/utterance-per-intent-shows-data-imbalance.png)

Mantenga el mouse sobre la barra de cada intento para obtener información acerca de la intención. 

![El gráfico siguiente muestra una aplicación y equilibrada con casi sin problemas para corregirlos.](./media/luis-how-to-use-dashboard/utterances-per-intent-with-details-of-errors.png)

Use la **ordenar por** característica para organizar los intentos por tipo de problema para que pueda centrarse en los intentos con ese problema más problemáticos. 

## <a name="intents-with-errors"></a>Intentos con errores

Esta tarjeta permite revisar los problemas para un propósito específico. La vista predeterminada de esta tarjeta es las intenciones más problemáticas para que sepa dónde centrar sus esfuerzos.

![Los intentos con tarjeta de errores le permite revisar los problemas para un propósito específico. La tarjeta se filtra a las intenciones más problemáticas, de forma predeterminada, por lo que sabe dónde centrar sus esfuerzos.](./media/luis-how-to-use-dashboard/most-problematic-intents-with-errors.png)

El gráfico de anillos superior muestra los problemas con la intención en los tipos de problemas de tres. Si hay problemas en los tipos de problemas de tres, cada tipo tiene su propio gráfico, a continuación, junto con las intenciones rival. 

### <a name="filter-intents-by-issue-and-percentage"></a>Intenciones de filtro mediante la emisión y porcentaje

En esta sección de la tarjeta permite buscar las declaraciones de ejemplo que se están acumulando fuera de su umbral de error. Lo ideal es que desee predicciones correctas para ser significativo. Ese porcentaje es empresariales y orientadas al cliente. 

Determinar los porcentajes del umbral que se son cómodo con para su negocio. 

El filtro le permite buscar las intenciones con problema concreto:

|Filtrar|Porcentaje sugerido|Propósito|
|--|--|--|
|Intenciones más problemáticas|-|**Comience aquí** -corregir las palabras pronunciadas en esta intención mejorará la aplicación más que otras correcciones de errores.|
|A continuación de predicciones correctas|60%|Este es el porcentaje de grabaciones de voz en la intención seleccionada que son correctos pero tienen una puntuación de confianza por debajo del umbral. |
|Predicciones claras anteriores|15 %|Este es el porcentaje de grabaciones de voz en la intención seleccionada que se confunde con la intención de rival más cercana.|
|Predicciones incorrectas anteriores|15 %|Este es el porcentaje de grabaciones de voz en la intención seleccionada que se predijeron incorrectamente. |

### <a name="correct-prediction-threshold"></a>Umbral de la predicción correcta

¿Qué es una puntuación de confianza de seguros de predicción para usted? Al principio del desarrollo de aplicaciones, 60% puede ser el destino. Use la **corregir predicciones siguiente** con el porcentaje del 60% para buscar las grabaciones de voz en la intención seleccionada que deben corregirse.

### <a name="unclear-or-incorrect-prediction-threshold"></a>Umbral de predicción es confusa o es incorrecto

Estos dos filtros permiten buscar grabaciones de voz en la intención supera el umbral seleccionada. Se puede considerar estas dos porcentajes como porcentajes de error. Si está familiarizado con una tasa de errores de 10-15% de las predicciones, establezca el umbral de filtro y el 15%, para buscar todas las grabaciones de voz por encima de este valor. 

## <a name="next-steps"></a>Pasos siguientes

* [Administrar los recursos de Azure](luis-how-to-azure-subscription.md)
