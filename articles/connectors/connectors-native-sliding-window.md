---
title: 'Programar tareas repetitivas con desencadenador de ventana deslizante: Azure Logic Apps'
description: Programar y ejecutar tareas automatizadas periódicas y flujos de trabajo con el desencadenador de ventana deslizante en Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: deli, klam, LADocs
ms.topic: conceptual
ms.date: 05/25/2019
ms.openlocfilehash: 44944955019fcf81fb0d296592577e2b00a15928
ms.sourcegitcommit: 8c49df11910a8ed8259f377217a9ffcd892ae0ae
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66299508"
---
# <a name="create-schedule-and-run-recurring-tasks-and-workflows-with-the-sliding-window-trigger-in-azure-logic-apps"></a>Crear, programar y ejecutar tareas repetitivas y flujos de trabajo con el desencadenador de ventana deslizante en Azure Logic Apps

Para ejecutar con regularidad los trabajos que deben tratar los datos en fragmentos continua, procesos o tareas, puede iniciar el flujo de trabajo de aplicación lógica con el **ventana deslizante - programación** desencadenador. Puede establecer una fecha y hora, así como una zona horaria para iniciar el flujo de trabajo y una periodicidad para ese flujo de trabajo de repetición. Si se pierden las repeticiones por cualquier razón, este desencadenador procesa esas periodicidades que faltan. Por ejemplo, al sincronizar datos entre la base de datos y almacenamiento de copia de seguridad, use el desencadenador de ventana deslizante para que se sincronizarán los datos sin incurrir en espacios. Para obtener más información acerca de los desencadenadores de programación integrados y acciones, vea [programación y ejecución periódica automatizada, tareas y flujos de trabajo con Azure Logic Apps](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md).

Aquí se presentan algunos patrones que admite este desencadenador:

* Ejecutar inmediatamente y Repetir cada *n* número de segundos, minutos u horas.

* A partir de una fecha y hora específicas, a continuación, ejecutar y Repetir cada *n* número de segundos, minutos u horas. Con este desencadenador, puede especificar una hora de inicio en el pasado, que se ejecuta todos los anteriores las repeticiones.

* Retrasar cada repetición de una duración específica antes de ejecutar.

Las diferencias entre este desencadenador y el desencadenador de periodicidad o para obtener más información acerca de la programación periódicas de los flujos de trabajo, consulte [programación y ejecución periódica automatizar las tareas, los procesos y flujos de trabajo con Azure Logic Apps](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md).

> [!TIP]
> Si desea desencadenar la aplicación lógica y ejecute solo una vez en el futuro, consulte [ejecución de trabajos de una sola vez](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md#run-once).

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción de Azure. Si no tiene una suscripción, puede [registrarse para obtener una cuenta gratuita de Azure](https://azure.microsoft.com/free/).

* Conocimientos básicos sobre [aplicaciones lógicas](../logic-apps/logic-apps-overview.md). Si está familiarizado con las aplicaciones lógicas, aprenda [cómo crear su primera aplicación lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="add-sliding-window-trigger"></a>Agregar desencadenador de ventana deslizante

1. Inicie sesión en el [Azure Portal](https://portal.azure.com). Crear una aplicación lógica en blanco.

1. Cuando aparezca el Diseñador de aplicación lógica, en el cuadro de búsqueda, escriba "ventana deslizante" como filtro. En la lista de desencadenadores, seleccione este desencadenador como primer paso en la aplicación lógica: **Ventana deslizante**

   ![Selección del desencadenador "Ventana deslizante"](./media/connectors-native-sliding-window/add-sliding-window-trigger.png)

1. Defina el intervalo y la frecuencia de periodicidad. En este ejemplo, defina estas propiedades para ejecutar el flujo de trabajo todas las semanas.

   ![Establecer el intervalo y la frecuencia](./media/connectors-native-sliding-window/sliding-window-trigger-details.png)

   | Propiedad | Obligatorio | Nombre JSON | Type | DESCRIPCIÓN |
   |----------|----------|-----------|------|-------------|
   | **Intervalo** | Sí | interval | Entero | Entero positivo que describe la frecuencia con la que se ejecuta el flujo de trabajo. Estos son los intervalos mínimo y máximo: <p>- Hora: 1-12 000 horas </br>- Minuto: 1-72 000 minutos </br>- Segundo: 1-9 999 999 segundos<p>Por ejemplo, si el intervalo es 6 y la frecuencia es "Hour", la periodicidad es cada 6 horas. |
   | **Frecuencia** | Sí | frequency | String | Unidad de tiempo que se usa para la periodicidad: **Segundo**, **minuto**, o **hora** |
   ||||||

   ![Opciones avanzadas de periodicidad](./media/connectors-native-sliding-window/sliding-window-trigger-more-options-details.png)

   Para obtener más opciones de periodicidad, abra el **Agregar nuevo parámetro** lista. 
   Las opciones que selecciona aparecen en el desencadenador después de la selección.

   | Propiedad | Obligatorio | Nombre JSON | Type | DESCRIPCIÓN |
   |----------|----------|-----------|------|-------------|
   | **Retraso** | No | delay | String | La duración del retraso cada periodicidad con la [especificación de tiempo de fecha ISO 8601](https://en.wikipedia.org/wiki/ISO_8601#Durations) |
   | **Zona horaria** | Sin  | timeZone | String | Solo se aplica cuando se especifica una hora de inicio porque este desencadenador no acepta [diferencia horaria con UTC](https://en.wikipedia.org/wiki/UTC_offset). Seleccione la zona horaria que desea aplicar. |
   | **Hora de inicio** | Sin  | startTime | String | Proporcione una fecha de inicio y la hora en este formato: <p>AAAA-MM-DDThh:mm:ss si selecciona una zona horaria <p>-o bien- <p>AAAA-MM-DDThh:mm:ssZ si no selecciona una zona horaria <p>Por ejemplo, si desea que el 18 de septiembre de 2017 a las 2:00 P.M., a continuación, especifique "2017-09-18T14:00:00" y seleccione una zona horaria como hora estándar del Pacífico. O bien, especifique "2017-09-18T14:00:00Z" sin una zona horaria. <p>**Nota:** Esta hora de inicio debe seguir la [especificación de fecha y hora ISO 8601](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations) en [formato de hora y fecha UTC](https://en.wikipedia.org/wiki/Coordinated_Universal_Time), pero sin una [diferencia horaria con UTC](https://en.wikipedia.org/wiki/UTC_offset). Si no se selecciona una zona horaria, debe agregar la letra "Z" al final sin espacios. Esta "Z" se refiere al equivalente de [hora náutica](https://en.wikipedia.org/wiki/Nautical_time). <p>Las programaciones simples, la hora de inicio es la primera aparición, mientras que para las repeticiones avanzada, el desencadenador no desencadena antes de la hora de inicio. [ *¿De qué formas puedo usar la fecha y hora de inicio?* ](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md#start-time) |
   |||||

1. Ahora cree el flujo de trabajo restante con otras acciones. Más acciones que se pueden agregar, consulte [conectores para Azure Logic Apps](../connectors/apis-list.md).

## <a name="workflow-definition---sliding-window"></a>Definición de flujo de trabajo: ventana deslizante

En la definición de flujo de trabajo subyacente de la aplicación lógica, que usa JSON, puede ver la definición del desencadenador de ventana deslizante con las opciones que eligió. Para ver esta definición, en la barra de herramientas del diseñador, elija **vista código**. Para volver al diseñador, elija la barra de herramientas de diseñador, **diseñador**.

En este ejemplo se muestra cómo podría ser una definición de desencadenador de ventana deslizante en una definición de flujo de trabajo subyacente donde es el retraso para cada repetición de cinco segundos para una periodicidad por hora:

``` json
"triggers": {
   "Recurrence": {
      "type": "SlidingWindow",
      "Sliding_Window": {
         "inputs": {
            "delay": "PT5S"
         },
         "recurrence": {
            "frequency": "Hour",
            "interval": 1,
            "startTime": "2019-05-13T14:00:00Z",
            "timeZone": "Pacific Standard Time"
         }
      }
   }
}
```

## <a name="next-steps"></a>Pasos siguientes

* [Retrasar la acción siguiente en los flujos de trabajo](../connectors/connectors-native-delay.md)
* [Conectores de Logic Apps](../connectors/apis-list.md)