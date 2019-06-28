---
title: Investigar casos con Azure Sentinel, versión preliminar | Microsoft Docs
description: Use este tutorial para aprender a investigar casos con Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: a493cd67-dc70-4163-81b8-04a9bc0232ac
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/6/2019
ms.author: rkarlin
ms.openlocfilehash: d1da180d3b30b57ca2b69985bf3d0261b8d70c39
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65205520"
---
# <a name="tutorial-investigate-cases-with-azure-sentinel-preview"></a>Tutorial: Investigar casos con Azure Sentinel, versión preliminar

> [!IMPORTANT]
> Azure Sentinel se encuentra actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Este tutorial ayuda a detectar amenazas con Azure Sentinel.

Después de [conectar los orígenes de datos](quickstart-onboard.md) a Azure Sentinel, querrá recibir una notificación cuando suceda algo sospechoso. Para ello, Azure Sentinel le permite crear reglas de alerta avanzadas, que generan casos que se pueden asignar y usar para investigar las anomalías y las amenazas del entorno de manera exhaustiva. 

> [!div class="checklist"]
> * Creación de casos
> * Investigación de casos
> * Respuesta a amenazas

## <a name="investigate-cases"></a>Investigación de casos

Un caso puede incluir varias alertas, a modo de agregado de todas las pruebas relevantes en una investigación en concreto. Los casos se crean en función de las alertas que haya definidas en la página **Analytics** (Análisis). Las propiedades de las alertas, como la gravedad y el estado, se establecen en el nivel de caso. Después de indicar a Azure Sentinel qué tipos de amenazas está buscando y cómo detectarlas, puede supervisar las amenazas que se detecten investigando cada caso. 

1. Seleccione **Cases** (Casos). La página **Cases** (Casos) le permite saber cuántos casos tiene, cuántos están abiertos, cuántos están establecidos en **In progress** (En curso) y cuántos se han cerrado. De cada caso puede ver la hora a la que tuvo lugar y su estado. Valore la gravedad para decidir qué abordar primero. En la página **Cases** (Casos), haga clic en la pestaña **Alerts** (Alertas) para ver todas las alertas relacionadas con un caso. Las entidades que haya asignado anteriormente como parte del caso se pueden ver en la pestaña **Entities** (Entidades).  Puede filtrar los casos según sea necesario, por ejemplo, por estado o gravedad. Cuando observe la pestaña **Cases** (Casos), verá los casos abiertos que contienen alertas desencadenadas por las reglas de detección definidas en **Analytics** (Análisis). En la parte superior verá los casos activos, los casos nuevos y los casos en curso. También puede ver un resumen de todos los casos por gravedad.

   ![Panel de alertas](./media/tutorial-investigate-cases/cases.png)

2. Para iniciar una investigación, haga clic en un caso específico. A la derecha, puede ver información detallada del caso, como la gravedad o un resumen del número de entidades implicadas (según la asignación). Cada caso tiene un identificador único. La gravedad del caso viene determinada según la alerta más grave incluida en el caso.  

1. Para ver más detalles sobre las alertas y las entidades del caso, haga clic en **View full details** (Ver detalles completos) en la página del caso y revise las pestañas correspondientes donde se resume la información del caso.  La vista completa del caso condensa todas las pruebas de la alerta, las alertas asociadas y las entidades.

1. En la pestaña **Alerts** (Alertas), revise la alerta en sí: cuándo se ha desencadenado y por cuánto ha superado los umbrales establecidos. Puede ver toda la información relevante sobre la alerta: la consulta que la ha desencadenado, el número de resultados devueltos por consulta y la capacidad de ejecutar cuadernos de estrategias en las alertas. Para explorar el caso más en profundidad, haga clic en el número de aciertos. Esto abre la consulta que ha generado los resultados y los resultados que han desencadenado la alerta en Log Analytics.

3. En la pestaña **Entities** (Entidades), puede ver todas las entidades que ha asignado como parte de la definición de regla de alerta. 

4. Si está investigando un caso activamente, una buena idea consiste en establecer el estado del caso en **In progress** (En curso) hasta que lo cierre. También puede cerrar el caso y, en este sentido, **Closed resolved** (Cerrado y resuelto) es el estado de los casos que indican que se ha abordado un incidente, mientras que **Closed dismissed** (Cerrado y descartado) sería el estado de los casos que no es necesario abordar. Para cerrar un caso, es necesario aportar una explicación que sustente sus motivos.

5. Los casos se pueden asignar a un usuario específico. Para asignar un propietario a un caso, hay que establecer el campo **Owner** (propietario). Todos los casos se inician sin tener un propietario asignado. Para ver todos los casos de su propiedad, puede ir a los casos y filtrarlos por su nombre. 

5. Haga clic en **Investigate** (Investigar) para ver la asignación de la investigación y el ámbito de la infracción junto con los pasos de corrección. 



## <a name="respond-to-threats"></a>Respuesta a amenazas

Azure Sentinel ofrece dos opciones principales para responder a amenazas con cuadernos de estrategias. Puede establecer un cuaderno de estrategias para que se ejecute automáticamente cuando se desencadene una alerta, o bien ejecutar un cuaderno de estrategias manualmente como respuesta a una alerta.

- Puede establecer un cuaderno de estrategias para que se ejecute automáticamente cuando se desencadene una alerta, al configurar ese cuaderno de estrategias. 

- Puede ejecutar un cuaderno de estrategias manualmente desde la propia alerta; para ello, haga clic en **View playbooks** (Ver cuadernos de estrategias) y, después, seleccione un cuaderno de estrategias para ejecutarlo.




## <a name="next-steps"></a>Pasos siguientes
En este tutorial ha aprendido cómo empezar a investigar casos mediante Azure Sentinel. Siga con el tutorial sobre [cómo responder a amenazas usando cuadernos de estrategias automatizados](tutorial-respond-threats-playbook.md).
> [!div class="nextstepaction"]
> [Responda a amenazas](tutorial-respond-threats-playbook.md) para automatizar sus respuestas a estas.

