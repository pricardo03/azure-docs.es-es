---
title: Realización de la evaluación sin conexión - Personalizer
titleSuffix: Azure Cognitive Services
description: Aprenda a analizar el bucle de aprendizaje con una evaluación sin conexión.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 09/10/2019
ms.author: diberry
ms.openlocfilehash: bd57880b11f56b13b4225652071593d29dcc6280
ms.sourcegitcommit: 12de9c927bc63868168056c39ccaa16d44cdc646
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2019
ms.locfileid: "72515217"
---
# <a name="analyze-your-learning-loop-with-an-offline-evaluation"></a>Análisis del bucle de aprendizaje con una evaluación sin conexión

Aprenda a realizar una evaluación sin conexión y entender los resultados.

Las evaluaciones sin conexión le permiten medir la eficacia de Personalizer en comparación con el comportamiento predeterminado de la aplicación, conocer las características que más contribuyen a la personalización y descubrir automáticamente las nuevas configuraciones de aprendizaje automático.

Consulte [Evaluaciones sin conexión](concepts-offline-evaluation.md) para más información.


## <a name="prerequisites"></a>Requisitos previos

* Un bucle de Personalizer configurado.
* El bucle de Personalizer debe tener una cantidad de datos representativa: como valor aproximado, recomendamos al menos 50 000 eventos en sus registros para obtener resultados de evaluación significativos. Opcionalmente, también puede haber exportado previamente archivos de la  _directiva de aprendizaje_ que puede comparar y probar en la misma evaluación.

## <a name="steps-to-start-a-new-offline-evaluation"></a>Pasos para iniciar una nueva evaluación sin conexión

1. En [Azure Portal](https://azure.microsoft.com/free/), busque el recurso de personalización.
1. En Azure Portal, vaya a la sección **Evaluaciones** y seleccione **Crear evaluación**.
    ![En Azure Portal, vaya a la sección **Evaluaciones** y seleccione **Crear evaluación**.](./media/offline-evaluation/create-new-offline-evaluation.png)
1. Seleccione la siguiente configuración:

    * Un nombre de evaluación.
    * Fecha de inicio y finalización: son fechas en el pasado, que especifican el intervalo de datos que se va a utilizar en la evaluación. Estos datos deben estar presentes en los registros, como se especifica en la configuración [Retención de datos](how-to-settings.md).
    * Detección de optimización establecida en **sí**.

    ![Elección de la configuración de evaluación sin conexión](./media/offline-evaluation/create-an-evaluation-form.png)

1. Seleccione **Aceptar** para iniciar la evaluación. 

## <a name="results"></a>Results

Las evaluaciones pueden tardar mucho tiempo en ejecutarse, según la cantidad de datos que se vayan a procesar, el número de directivas de aprendizaje que se vayan a comparar y si se ha solicitado una optimización.

Una vez completado, puede seleccionar la evaluación en la lista de evaluaciones. 

Las comparaciones de directivas de aprendizaje incluyen:

* **Directiva en línea**: la directiva de aprendizaje actual utilizada en Personalizer.
* **Línea de base**: el valor predeterminado de la aplicación (según lo determinado por la primera acción que se envía en las llamadas de Rank).
* **Directiva aleatoria**: un comportamiento de Rank imaginario que siempre devuelve la selección aleatoria de las acciones de las proporcionadas.
* **Directivas personalizadas**: directivas de aprendizaje adicionales que se cargan al iniciar la evaluación.
* **Directiva optimizada**: Si la evaluación se inició con la opción de detectar una directiva optimizada, también se comparará, y podrá descargarla o convertirla en la directiva de aprendizaje en línea, por el reemplazo de la actual.

![Gráfico de resultados de configuración de evaluación sin conexión](./media/offline-evaluation/evaluation-results.png)

Eficacia de las [características](concepts-features.md) para las acciones y el contexto.

## <a name="next-steps"></a>Pasos siguientes

* Aprenda cómo [trabajan las evaluaciones sin conexión](concepts-offline-evaluation.md).
