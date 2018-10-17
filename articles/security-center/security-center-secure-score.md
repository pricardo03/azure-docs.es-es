---
title: Puntuación segura de Azure Security Center | Microsoft Docs
description: " Dé prioridad a las recomendaciones de seguridad mediante la puntuación segura de Azure Security Center. "
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: c42d02e4-201d-4a95-8527-253af903a5c6
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/21/2018
ms.author: rkarlin
ms.openlocfilehash: fc521db9ad753c4162b65abfd2f9f23c318fa994
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2018
ms.locfileid: "46131065"
---
# <a name="improve-your-secure-score-in-azure-security-center"></a>Mejorar la puntuación segura de Azure Security Center


Con tantos servicios que ofrecen ventajas de seguridad, a menudo es difícil saber qué pasos dar primero para asegurar y proteger su carga de trabajo. La puntuación segura de Azure Security Center revisa las recomendaciones de seguridad y les asigna una prioridad, así podrá saber qué recomendaciones implementar primero y le permitirá conocer las vulnerabilidades de seguridad más graves para establecer prioridades en la investigación. Puntuación segura es una herramienta de medición que le ayuda a reforzar la seguridad para lograr una carga de trabajo segura.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]


![Panel de Puntuación segura](./media/security-center-secure-score/secure-score-dashboard.png)

## <a name="secure-score-calculation"></a>Cálculo de Puntuación segura

Security Center imita el trabajo de un analista de seguridad, revisando sus recomendaciones de seguridad y aplicando algoritmos avanzados para determinar cuán crucial es cada recomendación.
Azure Security Center revisa constantemente sus recomendaciones activas y calcula su puntuación segura basándose en ellas. La puntuación de una recomendación se deriva de su severidad y las mejores prácticas de seguridad que más afectarán a la seguridad de su carga de trabajo.

La **puntuación segura** es un cálculo basado en la relación entre los recursos en buen estado y los recursos totales. Si el número de recursos en buen estado es igual al número total de recursos, obtendrá la máxima puntuación segura: 50. Para intentar obtener la puntuación segura más cercana a la máxima puntuación, siga las recomendaciones para corregir los recursos en mal estado.

Security Center también proporciona una puntuación segura general. 

La **puntuación segura general** es una acumulación de todas las recomendaciones. Puede ver la puntuación segura general de sus suscripciones o grupos de administración, según lo que seleccione. La puntuación variará en función de la suscripción seleccionada y las recomendaciones activas de esas suscripciones.

 

Para comprobar qué recomendaciones afectan más a la puntuación segura, puede ver las tres recomendaciones con más impacto en el panel de Security Center o puede ordenar las recomendaciones en la hoja con la lista de recomendaciones usando la columna de **impacto en la puntuación segura**.


Para consultar su puntuación segura general:

1. En el panel de Azure, haga clic en **Security Center** y, después, en **Recomendaciones**.
2. En la parte superior verá la puntuación segura, que representa la puntuación por directivas, por la suscripción seleccionada. 
2. En la tabla siguiente se enumeran las recomendaciones, puede ver que para cada recomendación hay una columna que representa el **impacto en la puntuación segura**. Este número representa cuánto mejorará la puntuación general si sigue las recomendaciones. Por ejemplo, en la pantalla siguiente, si **corrige las vulnerabilidades de las configuraciones de seguridad del contenedor**, aumentará su puntuación segura en 35 puntos.

   ![puntuación segura](./media/security-center-secure-score/security-center-secure-score1.png)

## <a name="individual-secure-score"></a>Puntuación segura individual

Además, puede consultar las puntuaciones seguras individuales en la hoja de recomendaciones individuales.  

La **puntuación segura de recomendación** es un cálculo basado en la relación entre los recursos en buen estado y los recursos totales. Si el número de recursos en buen estado es igual al número total de recursos, obtendrá la máxima puntuación segura de la recomendación. Para intentar obtener la puntuación segura más cercana a la máxima puntuación, siga las recomendaciones para corregir los recursos en mal estado.

El **impacto de la recomendación** permite saber cuánto mejorará su puntuación segura si aplica los pasos descritos en la recomendación. Por ejemplo, si la puntuación segura es 42 y el **impacto de la recomendación** es +3, si lleva a cabo los pasos descritos en la recomendación su puntuación segura mejorará a 45.

La recomendación muestra las amenazas a las que se expone la carga de trabajo si no se realizan los pasos para la corrección.

![puntuación segura de recomendación individual](./media/security-center-secure-score/indiv-recommendation-secure-score.png)

## <a name="next-steps"></a>Pasos siguientes
En este artículo se muestra cómo mejorar su postura de seguridad mediante **Puntuación segura** de Azure Security Center. Para más información sobre el Centro de seguridad, consulte los siguientes recursos:

* [Preguntas más frecuentes sobre Azure Security Center](security-center-faq.md): encuentre las preguntas más frecuentes sobre el uso del servicio.
* [Supervisión del estado de seguridad en Azure Security Center](security-center-monitoring.md): aprenda a supervisar el estado de los recursos de Azure.


