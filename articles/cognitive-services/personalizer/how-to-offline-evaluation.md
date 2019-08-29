---
title: Evaluación sin conexión de Personalizer
titleSuffix: Azure Cognitive Services
description: Aprenda a analizar el bucle de aprendizaje con una evaluación sin conexión.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 05/07/2019
ms.author: diberry
ms.openlocfilehash: a8a75601daf36ca21ea56a5930219d7d467f0c85
ms.sourcegitcommit: a6888fba33fc20cc6a850e436f8f1d300d03771f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2019
ms.locfileid: "69557825"
---
# <a name="how-to-analyze-your-learning-loop-with-an-offline-evaluation"></a>Análisis del bucle de aprendizaje con una evaluación sin conexión


Aprenda a realizar una evaluación sin conexión y entender los resultados.

Las evaluaciones sin conexión le permiten medir la eficacia de Personalizer en comparación con el comportamiento predeterminado de la aplicación, conocer las características que más contribuyen a la personalización y descubrir automáticamente las nuevas configuraciones de aprendizaje automático.

Consulte [Evaluaciones sin conexión](concepts-offline-evaluation.md) para más información.


## <a name="prerequisites"></a>Requisitos previos

1. Debe tener un bucle Personalizer configurado.
1. El bucle de Personalizer debe tener una cantidad de datos representativa: como valor aproximado, recomendamos al menos 50 000 eventos en sus registros para obtener resultados de evaluación significativos.

Opcionalmente, también puede haber exportado previamente archivos de la  _directiva de aprendizaje_ que puede comparar y probar en la misma evaluación.

## <a name="steps-to-start-a-new-offline-evaluation"></a>Pasos para iniciar una nueva evaluación sin conexión

1. Busque el recurso de bucle Personalizer en Azure Portal.
1. Vaya a la sección "Evaluación".
1. Haga clic en Nueva evaluación.
1. Seleccione una fecha de inicio y finalización para la evaluación sin conexión. Estas son fechas en el pasado, que especifican el intervalo de datos que se va a utilizar en la evaluación. Estos datos deben estar presentes en los registros, como se especifica en la configuración [Retención de datos](how-to-settings.md).
1. Si lo desea, puede cargar su propia directiva de aprendizaje. 
1. Especifique si Personalizer debe crear una directiva de aprendizaje optimizada basada en el comportamiento del usuario observado en este período.
1. Inicio de la evaluación

## <a name="results"></a>Results

Las evaluaciones pueden tardar mucho tiempo en ejecutarse, según la cantidad de datos que se vayan a procesar, el número de directivas de aprendizaje que se vayan a comparar y si se ha solicitado una optimización.

Una vez completado, puede ver los siguientes resultados:

1. Comparaciones de directivas de aprendizaje, incluidas:
    * **Directiva en línea**: la directiva de aprendizaje actual utilizada en Personalizer.
    * **Línea de base**: el valor predeterminado de la aplicación (según lo determinado por la primera acción que se envía en las llamadas de Rank).
    * **Directiva aleatoria**: un comportamiento de Rank imaginario que siempre devuelve la selección aleatoria de las acciones de las proporcionadas.
    * **Directivas personalizadas**: directivas de aprendizaje adicionales que se cargan al iniciar la evaluación.
    * **Directiva optimizada**: Si la evaluación se inició con la opción de detectar una directiva optimizada, también se comparará, y podrá descargarla o convertirla en la directiva de aprendizaje en línea, por el reemplazo de la actual.

1. Eficacia de las [características](concepts-features.md) para las acciones y el contexto.


## <a name="more-information"></a>Más información

* Aprenda cómo [trabajan las evaluaciones sin conexión](concepts-offline-evaluation.md).
