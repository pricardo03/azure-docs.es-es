---
title: Puntuación segura de Azure Security Center | Microsoft Docs
description: " Dé prioridad a las recomendaciones de seguridad mediante la puntuación segura de Azure Security Center. "
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: c42d02e4-201d-4a95-8527-253af903a5c6
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/15/2019
ms.author: memildin
ms.openlocfilehash: bd896cbee98bd61518c9a093f86e088f0b018d73
ms.sourcegitcommit: 276c1c79b814ecc9d6c1997d92a93d07aed06b84
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/16/2020
ms.locfileid: "76156665"
---
# <a name="improve-your-secure-score-in-azure-security-center"></a>Mejora de la puntuación de seguridad de Azure Security Center

> [!NOTE]
> Hay una puntuación de seguridad mejorada disponible en versión preliminar. La puntuación de seguridad mejorada reemplazará finalmente a la puntuación de seguridad ya existente, aunque durante un tiempo funcionarán ambas simultáneamente para facilitar la transición.
>
> Para más información sobre las ventajas de la puntuación de seguridad mejorada, consulte [este artículo](secure-score-security-controls.md).
>
> Para tomar parte en la versión preliminar, abra Azure Portal, inicie Azure Security Center y seleccione Puntuación de seguridad. A partir de ahí, podrá ver un banner en la parte superior de la página que ofrece la experiencia de la nueva puntuación de seguridad. O bien, haga clic [aquí](https://aka.ms/ascnewscore).

Con tantos servicios que ofrecen ventajas de seguridad, a menudo es difícil saber qué pasos dar primero para asegurar y proteger su carga de trabajo. La puntuación segura de Azure revisa las recomendaciones de seguridad y les asigna una prioridad, así sabe qué recomendaciones realizar primero. De esta manera, puede encontrar las vulnerabilidades de seguridad más graves y así dar prioridad a la investigación. La puntuación segura es una herramienta que le ayuda a evaluar la postura de seguridad de las cargas de trabajo.

## <a name="secure-score-calculation"></a>Cálculo de Puntuación segura

Security Center imita el trabajo de un analista de seguridad: revisa sus recomendaciones de seguridad y aplica algoritmos avanzados para determinar la importancia de cada recomendación.
Azure Security Center revisa constantemente las recomendaciones activas y calcula la puntuación segura basándose en ellas. La puntuación de una recomendación se deriva de su gravedad y de los procedimientos recomendados de seguridad que afectarán más a la seguridad de la carga de trabajo.

Security Center también proporciona una **puntuación segura general**. 

La **puntuación segura general** es una acumulación de todas las puntuaciones de recomendación. Puede ver la puntuación segura general de sus suscripciones o grupos de administración, según lo que seleccione. La puntuación variará en función de la suscripción seleccionada y las recomendaciones activas de esas suscripciones.

Para comprobar qué recomendaciones afectan más a la puntuación segura, puede ver las tres recomendaciones con más impacto en el panel de Security Center, o puede ordenar las recomendaciones en la hoja de la lista de recomendaciones mediante la columna **Impacto de la puntuación segura**.

Para consultar su puntuación segura general:

1. En el panel de Azure, haga clic en **Security Center** y, después, en **Puntuación de seguridad**.

2. En la parte superior puede ver los aspectos destacados de la Puntuación de seguridad:
   - La **Puntuación de seguridad general** representa la puntuación por directivas, por la suscripción seleccionada.
   - **Puntuación de seguridad por categoría** muestra los recursos que necesitan más atención.
   - **Principales recomendaciones por Impacto de la puntuación segura** le proporciona una lista de las recomendaciones que mejorarán en mayor medida su puntuación de seguridad si las implementa.
 
   ![puntuación segura](./media/security-center-secure-score/secure-score-dashboard.png)

3. En la tabla siguiente puede ver cada una de las suscripciones y la puntuación de seguridad general para cada una.

   > [!NOTE]
   > La suma de la puntuación de seguridad de cada suscripción no equivale a la puntuación de seguridad general. La puntuación de seguridad es un cálculo basado en la relación entre los recursos en buen estado y los recursos totales por recomendación, no una suma de las puntuaciones de seguridad entre las suscripciones. 
   >
4. Haga clic en **Ver recomendaciones** para ver las recomendaciones para esa suscripción, que puede corregir para mejorar su puntuación de seguridad.
4. En la lista de recomendaciones, puede ver que para cada recomendación hay una columna que representa el **impacto en la puntuación segura**. Este número representa cuánto mejorará la puntuación general si sigue las recomendaciones. Por ejemplo, en la pantalla siguiente, si **corrige las vulnerabilidades de las configuraciones de seguridad del contenedor**, aumentará su puntuación segura en 35 puntos.

   ![puntuación segura](./media/security-center-secure-score/security-center-secure-score1.png)



## <a name="individual-secure-score"></a>Puntuación segura individual

Además, puede consultar las puntuaciones seguras individuales en la hoja de recomendaciones individuales.  

La **puntuación segura de recomendación** es un cálculo basado en la relación entre los recursos en buen estado y los recursos totales. Si el número de recursos en buen estado es igual al número total de recursos, obtendrá la máxima puntuación segura de la recomendación de 50. Para intentar obtener la puntuación segura más cercana a la máxima puntuación, siga las recomendaciones para corregir los recursos en mal estado.

El **impacto de la recomendación** le permite saber cuánto mejorará su puntuación segura si aplica los pasos de la recomendación. Por ejemplo, si la puntuación segura es 42 y el **impacto de la recomendación** es +3, al realizar los pasos descritos en la recomendación mejorará su puntuación segura hasta 45.

La recomendación muestra las amenazas a las que se expone la carga de trabajo si no se realizan los pasos para la corrección.

![puntuación segura de recomendación individual](./media/security-center-secure-score/indiv-recommendation-secure-score.png)







## <a name="next-steps"></a>Pasos siguientes
En este artículo se muestra cómo mejorar su postura de seguridad mediante **Puntuación segura** de Azure Security Center. Para más información sobre Security Center, consulte:

* [Preguntas más frecuentes sobre Azure Security Center](security-center-faq.md): encuentre las preguntas más frecuentes sobre el uso del servicio.
* [Supervisión del estado de seguridad en Azure Security Center](security-center-monitoring.md): aprenda a supervisar el estado de los recursos de Azure.


