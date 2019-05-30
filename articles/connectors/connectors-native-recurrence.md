---
title: 'Programar tareas repetitivas con desencadenador de periodicidad: Azure Logic Apps'
description: Programar y ejecutar tareas automatizadas periódicas y flujos de trabajo con el desencadenador de periodicidad en Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: deli, klam, LADocs
ms.topic: conceptual
ms.date: 05/25/2019
ms.openlocfilehash: f5fc778ee4d8f91232bc732cc276f642f748b29d
ms.sourcegitcommit: 8c49df11910a8ed8259f377217a9ffcd892ae0ae
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66297539"
---
# <a name="create-schedule-and-run-recurring-tasks-and-workflows-with-the-recurrence-trigger-in-azure-logic-apps"></a>Crear, programar y ejecutar tareas repetitivas y flujos de trabajo con el desencadenador de periodicidad en Azure Logic Apps

Para ejecutar con regularidad las tareas, procesos o trabajos en programación específica, puede iniciar la aplicación lógica con integrado **periodicidad - programación** desencadenador. Puede establecer una fecha y hora, así como una zona horaria para iniciar el flujo de trabajo y una periodicidad para ese flujo de trabajo de repetición. Si se pierden las repeticiones por cualquier motivo, este desencadenador continúa periódica en el siguiente intervalo programado. Para obtener más información acerca de los desencadenadores de programación integrados y acciones, vea [programación y ejecución periódica automatizada, tareas y flujos de trabajo con Azure Logic Apps](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md).

Aquí se presentan algunos patrones que este desencadenador es compatible con junto con las repeticiones más avanzadas y las programaciones complejas:

* Ejecutar inmediatamente y Repetir cada *n* número de segundos, minutos, horas, días, semanas o meses.

* A partir de una fecha y hora específicas, a continuación, ejecutar y Repetir cada *n* número de segundos, minutos, horas, días, semanas o meses.

* Ejecutar y repetir una o varias veces al día, por ejemplo, a las 8:00 a. m. y a las 5:00 p. m.

* Ejecutar y repetir cada semana, pero solo días específicos, como el sábado y domingo.

* Ejecutar y repetir cada semana, pero solo días específicos a horas concretas, como de lunes a viernes, a las 8:00 a. m. y a las 5:00 p. m.

Las diferencias entre este desencadenador y el desencadenador de ventana deslizante, o para obtener más información acerca de la programación periódicas de los flujos de trabajo, consulte [programación y ejecución periódica automatizar las tareas, los procesos y flujos de trabajo con Azure Logic Apps](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md).

> [!TIP]
> Si desea desencadenar la aplicación lógica y ejecute solo una vez en el futuro, consulte [ejecución de trabajos de una sola vez](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md#run-once).

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción de Azure. Si no tiene una suscripción, puede [registrarse para obtener una cuenta gratuita de Azure](https://azure.microsoft.com/free/).

* Conocimientos básicos sobre [aplicaciones lógicas](../logic-apps/logic-apps-overview.md). Si está familiarizado con las aplicaciones lógicas, aprenda [cómo crear su primera aplicación lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="add-recurrence-trigger"></a>Agregar desencadenador de periodicidad

1. Inicie sesión en el [Azure Portal](https://portal.azure.com). Crear una aplicación lógica en blanco.

1. Cuando aparezca el Diseñador de aplicación lógica, en el cuadro de búsqueda, escriba "periodicidad" como filtro. En la lista de desencadenadores, seleccione este desencadenador como primer paso en la aplicación lógica: **Periodicidad**

   ![Seleccione el desencadenador "Periodicidad"](./media/connectors-native-recurrence/add-recurrence-trigger.png)

1. Defina el intervalo y la frecuencia de periodicidad. En este ejemplo, defina estas propiedades para ejecutar el flujo de trabajo todas las semanas.

   ![Establecer el intervalo y la frecuencia](./media/connectors-native-recurrence/recurrence-trigger-details.png)

   | Propiedad | Obligatorio | Nombre JSON | Type | DESCRIPCIÓN |
   |----------|----------|-----------|------|-------------|
   | **Intervalo** | Sí | interval | Entero | Entero positivo que describe la frecuencia con la que se ejecuta el flujo de trabajo. Estos son los intervalos mínimo y máximo: <p>- Mes: 1-16 meses </br>- Día: 1-500 días </br>- Hora: 1-12 000 horas </br>- Minuto: 1-72 000 minutos </br>- Segundo: 1-9 999 999 segundos<p>Por ejemplo, si el intervalo es 6 y la frecuencia es "month", la periodicidad es cada 6 meses. |
   | **Frecuencia** | Sí | frequency | String | Unidad de tiempo que se usa para la periodicidad: **Segundo**, **Minuto**, **Hora**, **Día**, **Semana** o **Mes** |
   ||||||

   Para obtener más opciones de programación, abra el **Agregar nuevo parámetro** lista. 
   Las opciones que selecciona aparecen en el desencadenador después de la selección.

   ![Opciones avanzadas de programación](./media/connectors-native-recurrence/recurrence-trigger-more-options-details.png)

   | Propiedad | Obligatorio | Nombre JSON | Type | DESCRIPCIÓN |
   |----------|----------|-----------|------|-------------|
   | **Zona horaria** | Sin  | timeZone | String | Solo se aplica cuando se especifica una hora de inicio porque este desencadenador no acepta [diferencia horaria con UTC](https://en.wikipedia.org/wiki/UTC_offset). Seleccione la zona horaria que desea aplicar. |
   | **Hora de inicio** | No | startTime | String | Proporcione una fecha de inicio y la hora en este formato: <p>AAAA-MM-DDThh:mm:ss si selecciona una zona horaria <p>-o bien- <p>AAAA-MM-DDThh:mm:ssZ si no selecciona una zona horaria <p>Por ejemplo, si desea que el 18 de septiembre de 2017 a las 2:00 P.M., a continuación, especifique "2017-09-18T14:00:00" y seleccione una zona horaria como hora estándar del Pacífico. O bien, especifique "2017-09-18T14:00:00Z" sin una zona horaria. <p>**Nota:** Esta hora de inicio debe seguir la [especificación de fecha y hora ISO 8601](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations) en [formato de hora y fecha UTC](https://en.wikipedia.org/wiki/Coordinated_Universal_Time), pero sin una [diferencia horaria con UTC](https://en.wikipedia.org/wiki/UTC_offset). Si no se selecciona una zona horaria, debe agregar la letra "Z" al final sin espacios. Esta "Z" se refiere al equivalente de [hora náutica](https://en.wikipedia.org/wiki/Nautical_time). <p>Para las programaciones simples, la hora de inicio es la primera aparición, mientras que para programaciones complejas, el desencadenador no se activa antes de la hora de inicio. [ *¿De qué formas puedo usar la fecha y hora de inicio?* ](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md#start-time) |
   | **En estos días** | Sin  | weekDays | Cadena o matriz de cadenas | Si selecciona "Semana", puede seleccionar uno o más días que desee ejecutar el flujo de trabajo: **Lunes**, **Martes**, **Miércoles**, **Jueves**, **Viernes**, **Sábado** y **Domingo** |
   | **A estas horas** | Sin  | hours | Entero o matriz de enteros | Si selecciona "Day" o "Week", puede seleccionar uno o varios enteros de 0 a 23 como las horas del día con la que desea ejecutar el flujo de trabajo. <p><p>Por ejemplo, si especifica "10", "12" y "14", obtendrá 10 a. M., 12 p. M. y 2 P.M. durante las horas del día, pero se calculan en función de los minutos del día cuando se inicia la periodicidad. Para establecer los minutos del día, especifique el valor de la **en estos minutos** propiedad. |
   | **En estos minutos** | No | minutes | Entero o matriz de enteros | Si selecciona "Day" o "Week", puede seleccionar uno o varios enteros de 0 a 59 como los minutos de la hora en los que desea ejecutar el flujo de trabajo. <p>Por ejemplo, puede especificar "30" como la marca de minuto y, utilizando el ejemplo anterior para las horas del día, obtendrá 10:30 a. m., 12:30 p. m. y las 2:30 p. m. |
   |||||

   Por ejemplo, supongamos que hoy es lunes, 4 de septiembre de 2017. No se activa el siguiente desencadenador de periodicidad *antes* a la fecha de inicio y hora, que es el lunes, 18 de septiembre de 2017 a las 8:00 PST. Sin embargo, la programación de periodicidad está establecida para las 10:30 a. m., las 12:30 p. m. y las 2:30 p. m., solo los lunes. Por tanto, el desencadenador inicia y crea una instancia de flujo de trabajo de aplicación lógica por primera vez a las 10:30 a. m. Para más información sobre cómo funcionan las horas de inicio, vea estos [ejemplos de hora de inicio](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md#start-time).

   Las futuras ejecuciones se realizan a las 12:30 p. m. y a las 2:30 p. m. ese mismo día. Cada periodicidad crea su propia instancia de flujo de trabajo. Después de eso, toda la programación se repite el próximo lunes. [ *¿Qué otros ejemplos se hay?* ](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md#example-recurrences)

   ![Ejemplo de programación avanzada](./media/connectors-native-recurrence/recurrence-trigger-more-options-advanced-schedule.png)

   > [!NOTE]
   > El desencadenador muestra una vista previa de la periodicidad especificada solo si selecciona la frecuencia "Day" o "Week".

1. Ahora cree el flujo de trabajo restante con otras acciones. Más acciones que se pueden agregar, consulte [conectores para Azure Logic Apps](../connectors/apis-list.md).

## <a name="workflow-definition---recurrence"></a>Definición de flujo de trabajo: periodicidad

En la definición de flujo de trabajo subyacente de la aplicación lógica, que usa JSON, puede ver el [definición del desencadenador de periodicidad](../logic-apps/logic-apps-workflow-actions-triggers.md#recurrence-trigger) con las opciones que eligió. Para ver esta definición, en la barra de herramientas del diseñador, elija **vista código**. Para volver al diseñador, elija la barra de herramientas de diseñador, **diseñador**.

En este ejemplo se muestra cómo podría ser una definición de desencadenador de periodicidad en una definición de flujo de trabajo subyacente:

``` json
"triggers": {
   "Recurrence": {
      "type": "Recurrence",
      "recurrence": {
         "frequency": "Week",
         "interval": 1,
         "schedule": {
            "hours": [
               10,
               12,
               14
            ],
            "minutes": [
               30
            ],
            "weekDays": [
               "Monday"
            ]
         },
         "startTime": "2017-09-07T14:00:00Z",
         "timeZone": "Pacific Standard Time"
      }
   }
}
```

## <a name="next-steps"></a>Pasos siguientes

* [Flujos de trabajo de pausa con acciones de retraso](../connectors/connectors-native-delay.md)
* [Conectores de Logic Apps](../connectors/apis-list.md)
