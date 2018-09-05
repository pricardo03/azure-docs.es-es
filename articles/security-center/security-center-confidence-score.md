---
title: Puntuación de confianza en Azure Security Center | Microsoft Docs
description: " Aprenda a trabajar con la puntuación de confianza de Azure Security Center. "
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: e88198f8-2e16-409d-a0b0-a62e68c2f999
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/23/2018
ms.author: rkarlin
ms.openlocfilehash: d2998e9fe264d7919c8f74a98aaf8da6d701c0da
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2018
ms.locfileid: "43132527"
---
# <a name="alert-confidence-score"></a>Puntuación de confianza de la alerta 

Azure Security Center le proporciona visibilidad sobre los recursos que se ejecutan en Azure y le avisa cuando detecta posibles problemas. El volumen de alertas puede suponer un reto para que un equipo de operaciones de seguridad las aborde de manera individual, por lo que es necesario dar prioridad a las alertas que debe investigar. La investigación de alertas puede resultar compleja y lenta y, como consecuencia, algunas alertas se ignoran.

La puntuación de confianza de Security Center puede ayudar a su equipo a evaluar las prioridades y clasificar las alertas. Security Center aplica automáticamente procedimientos recomendados del sector, algoritmos inteligentes y procesos usados por analistas para determinar si una amenaza es legítima y le proporciona conclusiones significativas en forma de una puntuación de confianza.

## <a name="how-the-confidence-score-is-triggered"></a>¿Cómo se desencadena la puntuación de confianza?

Se generan alertas cuando se detectan procesos sospechosos que se ejecutan en las máquinas virtuales. Security Center revisa y analiza estas alertas en las máquinas virtuales Windows que se ejecutan en Azure. Realiza comprobaciones automatizadas y correlaciones mediante algoritmos avanzados en varias entidades y orígenes de datos de la organización, y en todos los recursos de Azure, y presenta una puntuación de confianza que es una medida del grado de confianza que tiene Security Center en que una alerta es auténtica y que debe investigarse.

## <a name="understanding-the-confidence-score"></a>Descripción de la puntuación de confianza

La puntuación de confianza va de 1 a 100 y representa el grado de confianza de Security Center en que una alerta se tiene que investigar. Cuanto mayor sea la puntuación, más seguro está Security Center de que la alerta denota una actividad malintencionada auténtica. La puntuación de confianza incluye una lista de las principales razones de por qué la alerta ha recibido esa puntuación de confianza. La puntuación de confianza facilita a los analistas de seguridad la clasificación por orden de prioridad de su respuesta a las alertas, abordando en primer lugar los ataques más acuciantes y reduciendo en última instancia la cantidad de tiempo que se tarda en responder a los ataques y vulneraciones de seguridad.

Para ver la puntuación de confianza:
- En el portal de Security Center, abra la hoja Alertas de seguridad.
-  Las alertas e incidentes se organizan de mayor a menor, lo cual significa que cuanto mayor es el grado de confianza de Security Center en que una alerta representa una amenaza, más cerca está la alerta de la parte superior de la página. 


 ![Puntuación de confianza][1]

Para ver los datos que han contribuido a la confianza de Security Center en una alerta:
- En la hoja de Alertas de seguridad, en **Confianza**, consulte las observaciones que contribuyeron a aumentar la puntuación de confianza y obtenga más información relacionada con la alerta. Esto le brinda más información sobre la naturaleza de las actividades que provocaron la alerta.

 ![Puntuación de confianza sospechosa][2]

Use la puntuación de confianza de Security Center para clasificar por orden de prioridad las alertas del entorno. La puntuación de confianza le permite ahorrar tiempo y esfuerzos mediante la investigación automática de las alertas, la aplicación de procedimientos recomendados del sector y algoritmos inteligentes y la actuación como analista virtual para determinar qué amenazas son reales y en cuáles debe centrar su atención.


## <a name="next-steps"></a>Pasos siguientes
En este artículo se explica cómo usar la puntuación de confianza para clasificar por orden de prioridad la investigación de las alertas. Para más información sobre el Centro de seguridad, consulte los siguientes recursos:

* [Preguntas más frecuentes sobre Azure Security Center](security-center-faq.md): encuentre las preguntas más frecuentes sobre el uso del servicio.
* [Supervisión del estado de seguridad en Azure Security Center](security-center-monitoring.md): aprenda a supervisar el estado de los recursos de Azure.



<!--Image references-->
[1]: ./media/security-center-confidence-score/confidence-score.png
[2]: ./media/security-center-confidence-score/suspicious-confidence-score.png
