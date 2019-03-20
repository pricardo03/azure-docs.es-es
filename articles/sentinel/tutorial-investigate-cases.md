---
title: Investigar los casos con la versión preliminar de Azure Sentinel | Microsoft Docs
description: Use este tutorial para aprender a investigar los casos con Centinela de Azure.
services: sentinel
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: a493cd67-dc70-4163-81b8-04a9bc0232ac
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/6/2019
ms.author: rkarlin
ms.openlocfilehash: 6b3357ec06c89645b9c41e9efdb582a18af40672
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "58074682"
---
# <a name="tutorial-investigate-cases-with-azure-sentinel-preview"></a>Tutorial: Investigar los casos con la versión preliminar de Centinela de Azure

> [!IMPORTANT]
> Sentinel Azure está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Este tutorial le ayudará a detectar amenazas con Azure Sentinel.

Después de [los orígenes de datos conectados](quickstart-onboard.md) a Sentinel, Azure, que desea recibir una notificación cuando ocurre algo sospechoso. Para poder hacer esto, Sentinel Azure permite que crear reglas de alerta, que generan los casos que se pueden asignar y avanzadas uso profundamente investigar las anomalías y las amenazas en su entorno. 

> [!div class="checklist"]
> * Creación de casos
> * Investigación de casos
> * Respuesta a amenazas

## <a name="investigate-cases"></a>Investigación de casos

Un caso puede incluir varias alertas. Es la suma de todos los criterios para una investigación específica. Se crea un caso en función de las alertas que ha definido en el **Analytics** página. Se establecen las propiedades relacionadas con las alertas, como la gravedad y estado en el nivel de caso. Después, informar a Azure Sentinel qué tipos de amenazas que está buscando y cómo encontrarlos, puede supervisar las amenazas que se detectan mediante la investigación de los casos. 

1. Seleccione **casos**. El **casos** página le permite saber cuántos casos tiene, cuántas son abrir, cuántos se ha establecido en **en curso**, y cuántas se cierran. Para cada caso, puede ver la hora que se produjo y el estado del caso. Examine la gravedad para decidir qué controlar primero. En el **casos** página, haga clic en el **alertas** pestaña para ver todas las alertas relacionadas con un caso. Las entidades que asignó anteriormente como parte del caso de que se puede ver en el **entidades** ficha.  Puede filtrar los casos según sea necesario, por ejemplo por estado o gravedad. Cuando observa la **casos** pestaña, verá los casos abiertos que contienen las alertas activadas por las reglas de detección definidas en **Analytics**. En la parte superior, verá los casos activos, los nuevos casos y en los casos de progreso. También puede ver un resumen de todos los casos por gravedad.

   ![Panel de alertas](./media/tutorial-investigate-cases/cases.png)

2. Para empezar una investigación, haga clic en un caso específico. A la derecha, puede ver información detallada para el caso como su gravedad, resumen del número de entidades implicadas (según la asignación). Cada caso tiene un identificador único. La gravedad del caso de se determina según la alerta más grave que se incluye en el caso.  

1. Para ver más detalles sobre las alertas y las entidades en el caso, haga clic en **ver detalles completos** en el caso de página y revise las fichas relevantes que resumen la información del caso.  Vista completa de casos consolida toda la evidencia de la alerta, las alertas asociadas y las entidades.

1. En el **alertas** pestaña, revise la propia - alerta cuando se desencadena y por cuánto superan los umbrales que haya establecido. Puede ver toda la información relevante acerca de la alerta: la consulta que desencadenó la alerta, el número de resultados devueltos por la consulta y la capacidad de ejecutar playbooks en las alertas. Para explorar en reducir aún más en el caso, haga clic en el número de visitas. Se abrirá la consulta que genera los resultados y los resultados que desencadenó la alerta en Log Analytics.

3. En el **entidades** ficha, puede ver todas las entidades que asignó como parte de la definición de regla de alerta. 

4. Si está investigando activamente un caso, es una buena idea para establecer el estado del caso en **en curso** hasta que lo cierre. También puede cerrar el caso, donde **cerrado a resuelto** es el estado de los casos que indican que se ha controlado un incidente, mientras que **cerrado descartadas** es el estado de los casos que no requieren un tratamiento. Explicaciones son necesarias que explica su razonamiento para cerrar un caso.

5. Los casos pueden asignarse a un usuario específico. Para cada caso puede asignar un propietario, estableciendo así **propietario** campo. Inicio de todos los casos como sin asignar. Puede entrar en los casos y filtrar por su nombre para ver todos los casos que posee. 

5. Haga clic en **investigar** para ver el mapa de la investigación y el ámbito de la infracción con los pasos de corrección. 



## <a name="respond-to-threats"></a>Respuesta a amenazas

Sentinel Azure ofrece dos opciones principales para responder a amenazas con cuadernos de estrategias. Puede establecer un cuaderno de estrategias para ejecutar automáticamente cuando se desencadena una alerta, o puede ejecutar manualmente un guión de procedimientos como respuesta a una alerta.

- Puede establecer un cuaderno de estrategias para ejecutar automáticamente cuando se desencadena una alerta cuando se configura el cuaderno de estrategias. 

- Puede ejecutar manualmente un cuaderno de estrategias desde dentro de la alerta, haga clic en **ver playbooks** y, a continuación, seleccionando un cuaderno de estrategias de ejecución.




## <a name="next-steps"></a>Pasos siguientes
En este tutorial, ha aprendido cómo empezar a investigar casos mediante Centinela de Azure. Siga el tutorial para [cómo responder a amenazas mediante guiones de procedimientos automatizados](tutorial-respond-threats-playbook.md).
> [!div class="nextstepaction"]
> [Responder a amenazas](tutorial-respond-threats-playbook.md) para automatizar sus respuestas a las amenazas.

