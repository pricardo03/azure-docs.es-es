---
title: Programación y ejecución de tareas y flujos de trabajo automatizados con Azure Logic Apps | Microsoft Docs
description: Automatice tareas programadas y periódicas con el conector de periodicidad en Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.assetid: 51dd4f22-7dc5-41af-a0a9-e7148378cd50
tags: connectors
ms.topic: article
ms.date: 01/08/2019
ms.openlocfilehash: eb22539d1f433e396935f82e4cb3786d5699d21a
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "58083961"
---
# <a name="create-and-run-recurring-tasks-and-workflows-with-azure-logic-apps"></a>Creación y ejecución de tareas y flujos de trabajo repetitivos con Azure Logic Apps

Para programar acciones, cargas de trabajo o procesos que se ejecutan con regularidad, cree un flujo de trabajo de aplicación lógica que se inicie con el [desencadenador](../logic-apps/logic-apps-overview.md#logic-app-concepts) **Programación: Periodicidad**. Puede establecer una fecha y hora para iniciar el flujo de trabajo y una programación de periodicidad para realizar tareas, como estos ejemplos, y mucho más:

* Obtener datos internos: [ejecutar un procedimiento almacenado de SQL](../connectors/connectors-create-api-sqlazure.md) cada día.
* Obtener datos externos: extraer informes meteorológicos de NOAA cada 15 minutos.
* Datos de informe: enviar un correo electrónico con un resumen de todos los pedidos de la semana pasada que superen una cantidad específica.
* Procesar datos: comprimir las imágenes cargadas de hoy todos los días laborables fuera de horas punta.
* Limpiar datos: eliminar todos los tweets que tengan más de tres meses.
* Archivar datos: insertar facturas en un servicio de copia de seguridad cada mes.

Este desencadenador es compatible con muchos patrones, por ejemplo:

* Ejecutar inmediatamente y repetir cada *n* segundos, minutos, horas, días, semanas o meses.
* Empezar a una hora específica y después ejecutar y repetir cada *n* segundos, minutos, horas, días, semanas o meses.
* Ejecutar y repetir una o varias veces al día, por ejemplo, a las 8:00 a. m. y a las 5:00 p. m.
* Ejecutar y repetir cada semana, pero solo días específicos, como el sábado y domingo.
* Ejecutar y repetir cada semana, pero solo días específicos a horas concretas, como de lunes a viernes, a las 8:00 a. m. y a las 5:00 p. m.

Cada vez que el desencadenador de periodicidad se activa, Logic Apps crea y ejecuta una instancia nueva del flujo de trabajo de aplicación lógica. 

Para desencadenar la aplicación lógica y ejecutarla solo una vez en el futuro, consulte [Ejecución de trabajos una sola vez](#run-once) más adelante en este tema.

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción de Azure. Si no tiene una suscripción, puede [comenzar con una cuenta de Azure gratuita](https://azure.microsoft.com/free/). También puede [registrarse para obtener una suscripción de pago por uso](https://azure.microsoft.com/pricing/purchase-options/).

* Conocimientos básicos acerca de [cómo crear aplicaciones lógicas](../logic-apps/quickstart-create-first-logic-app-workflow.md) 

## <a name="add-a-recurrence-trigger-to-your-logic-app"></a>Adición de un desencadenador de periodicidad a la aplicación lógica

1. Inicie sesión en el [Azure Portal](https://portal.azure.com). Cree una aplicación lógica en blanco u obtenga información [sobre cómo crear una aplicación lógica en blanco](../logic-apps/quickstart-create-first-logic-app-workflow.md).

2. Cuando aparezca el Diseñador de aplicaciones lógicas, elija **Todas**. En el cuadro de búsqueda, escriba "periodicidad" para el filtro. En la lista de desencadenadores, seleccione este desencadenador: **Periodicidad: Programación** 

   ![Selección del desencadenador "Periodicidad: Programación"](./media/connectors-native-recurrence/add-recurrence-trigger.png)

   Este desencadenador ahora es el primer paso de la aplicación lógica.

3. Defina el intervalo y la frecuencia de periodicidad. En este ejemplo, defina estas propiedades para ejecutar el flujo de trabajo todas las semanas. 

   ![Establecer el intervalo y la frecuencia](./media/connectors-native-recurrence/recurrence-trigger-details.png)

4. Para obtener más opciones de programación, elija **Mostrar opciones avanzadas**. 

   ![Más opciones](./media/connectors-native-recurrence/recurrence-trigger-more-options.png)

5. Ahora puede establecer estas opciones: 

   * Defina una fecha y hora de inicio para activar el desencadenador. 
   Si especifica una fecha y hora de inicio, también puede aplicar una zona horaria. 

   * Si selecciona "Day" o "Week" para la frecuencia, puede elegir horas específicas para la periodicidad. 

   * Si selecciona "Week", también puede elegir días concretos de la semana.
   
   ![Opciones avanzadas de programación](./media/connectors-native-recurrence/recurrence-trigger-more-options-details.png)

   Por ejemplo, supongamos que hoy es lunes, 4 de septiembre de 2017. 
   El siguiente desencadenador de periodicidad no se inicia *antes* de la fecha y hora de inicio, establecida el lunes, 18 de septiembre de 2017, a las 8:00 a. m. PST. 
   Sin embargo, la programación de periodicidad está establecida para las 10:30 a. m., las 12:30 p. m. y las 2:30 p. m., solo los lunes. Por tanto, el desencadenador inicia y crea una instancia de flujo de trabajo de aplicación lógica por primera vez a las 10:30 a. m. 
   Para más información sobre cómo funcionan las horas de inicio, vea estos [ejemplos de hora de inicio](#start-time).
   Las futuras ejecuciones se realizan a las 12:30 p. m. y a las 2:30 p. m. ese mismo día. 
   Cada periodicidad crea su propia instancia de flujo de trabajo. Después de eso, toda la programación se repite el próximo lunes. 
   [*¿Qué otros ejemplos se hay?*](#example-recurrences)

   ![Ejemplo de programación avanzada](./media/connectors-native-recurrence/recurrence-trigger-more-options-advanced-schedule.png)

   > [!NOTE]
   > El desencadenador muestra una vista previa de la periodicidad especificada solo si selecciona la frecuencia "Day" o "Week".
   
6. Ahora, cree el flujo de trabajo restante con acciones o instrucciones de control del flujo. Para ver más acciones que se puedan agregar, vea los [conectores](../connectors/apis-list.md). 

## <a name="trigger-details"></a>Detalles del desencadenador

Puede configurar estas propiedades en el desencadenador de periodicidad.

| NOMBRE | Obligatorio | Nombre de propiedad | Type | DESCRIPCIÓN | 
|----- | -------- | ------------- | ---- | ----------- | 
| **Frecuencia** | Sí | frequency | string | Unidad de tiempo que se usa para la periodicidad: **Segundo**, **Minuto**, **Hora**, **Día**, **Semana** o **Mes** | 
| **Intervalo** | Sí | interval | Entero | Entero positivo que describe la frecuencia con la que se ejecuta el flujo de trabajo. <p>El intervalo predeterminado es 1. Estos son los intervalos mínimo y máximo: <p>- Mes: 1-16 meses </br>- Día: 1-500 días </br>- Hora: 1-12 000 horas </br>- Minuto: 1-72 000 minutos </br>- Segundo: 1-9 999 999 segundos<p>Por ejemplo, si el intervalo es 6 y la frecuencia es "month", la periodicidad es cada 6 meses. | 
| **Zona horaria** | Sin  | timeZone | string | Solo se aplica cuando se especifica una hora de inicio porque este desencadenador no acepta [diferencia horaria con UTC](https://en.wikipedia.org/wiki/UTC_offset). Seleccione la zona horaria que desea aplicar. | 
| **Hora de inicio** | Sin  | startTime | string | Proporcione una hora de inicio con este formato: <p>AAAA-MM-DDThh:mm:ss si selecciona una zona horaria <p>O bien <p>AAAA-MM-DDThh:mm:ssZ si no selecciona una zona horaria <p>Por ejemplo, si desea la fecha del 18 de septiembre de 2017 a las 2:00 p. m., especifique entonces "2017-09-18T14:00:00" y selecciona una zona horaria como Hora estándar del Pacífico. O bien, especifique "2017-09-18T14:00:00Z" sin una zona horaria. <p>**Nota:** Esta hora de inicio debe seguir la [especificación de fecha y hora ISO 8601](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations) en [formato de hora y fecha UTC](https://en.wikipedia.org/wiki/Coordinated_Universal_Time), pero sin una [diferencia horaria con UTC](https://en.wikipedia.org/wiki/UTC_offset). Si no se selecciona una zona horaria, debe agregar la letra "Z" al final sin espacios. Esta "Z" se refiere al equivalente de [hora náutica](https://en.wikipedia.org/wiki/Nautical_time). <p>Para las programaciones simples, la hora de inicio es la primera aparición, mientras que para programaciones complejas, el desencadenador no se activa antes de la hora de inicio. [*¿De qué formas puedo usar la fecha y hora de inicio?*](#start-time) | 
| **En estos días** | Sin  | weekDays | Cadena o matriz de cadenas | Si selecciona "Semana", puede seleccionar uno o más días que desee ejecutar el flujo de trabajo: **Lunes**, **Martes**, **Miércoles**, **Jueves**, **Viernes**, **Sábado** y **Domingo** | 
| **A estas horas** | Sin  | hours | Entero o matriz de enteros | Si selecciona "Day" o "Week", puede especificar uno o varios enteros de 0 a 23 como las horas del día en las que desea ejecutar el flujo de trabajo. <p>Por ejemplo, si especifica "10", "12" y "14", obtendrá 10 a. m., 12 p. m. y 2 p. m. como las marcas de hora. | 
| **En estos minutos** | Sin  | minutes | Entero o matriz de enteros | Si selecciona "Day" o "Week", puede seleccionar uno o varios enteros de 0 a 59 como los minutos de la hora en los que desea ejecutar el flujo de trabajo. <p>Por ejemplo, puede especificar "30" como la marca de minuto y, utilizando el ejemplo anterior para las horas del día, obtendrá 10:30 a. m., 12:30 p. m. y las 2:30 p. m. | 
||||| 

## <a name="json-example"></a>Ejemplo JSON

Este es un ejemplo de [definición de desencadenador de periodicidad](../logic-apps/logic-apps-workflow-actions-triggers.md#recurrence-trigger):

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
         "startTime": "2017-09-07T14:00:00",
         "timeZone": "Pacific Standard Time"
      }
   }
}
```

## <a name="faq"></a>Preguntas más frecuentes

<a name="run-once"></a>

**P:** ¿Qué pasa si deseo ejecutar una aplicación lógica una vez solo en el futuro? </br>
**R:** Para desencadenar la aplicación lógica y ejecutarla una vez sin periodicidad, puede usar la plantilla **Scheduler: Run once jobs** (Programador:ejecutar trabajos una vez). Después de crear una nueva aplicación lógica, pero antes de abrir el Diseñador de aplicaciones lógicas, en la sección **Plantillas**, en la lista **Categoría**, seleccione **Programación** y, a continuación, seleccione la plantilla:

![Seleccione la plantilla "Scheduler: Run once jobs" (Programador:ejecutar trabajos una vez)](./media/connectors-native-recurrence/choose-run-once-template.png)

O bien, si usa una aplicación lógica en blanco, inicie la aplicación lógica con el desencadenador **Cuando se recibe una solicitud HTTP - Solicitar**. Pase la hora de inicio del desencadenador como un parámetro. Para el paso siguiente, agregue la acción **Retraso hasta - Programación** y especifique la hora en que empieza a ejecutarse la acción siguiente.

<a name="example-recurrences"></a>

**P:** ¿Qué otros ejemplos hay de programas de periodicidad? </br>
**R:** Estos son otros ejemplos:

| Periodicidad | Intervalo | Frecuencia | Hora de inicio | En estos días | A estas horas | En estos minutos | Nota: |
| ---------- | -------- | --------- | ---------- | ------------- | -------------- | ---------------- | ---- |
| Ejecutar cada 15 minutos (ninguna fecha y hora de inicio) | 15 | Minuto | {none} | {unavailable} | {none} | {none} | Esta programación se inicia inmediatamente y después calcula las futuras periodicidades en función de la hora de la última ejecución. | 
| Ejecutar cada 15 minutos (con fecha y hora de inicio) | 15 | Minuto | *startDate*T*startTime*Z | {unavailable} | {none} | {none} | Esta programación no se inicia *antes* de la fecha y hora de inicio especificadas y después calcula las futuras periodicidades en función de la hora de la última ejecución. | 
| Ejecutar cada hora, a la hora (con fecha y hora de inicio) | 1 | Hora | *startDate*Thh:00:00Z | {unavailable} | {none} | {none} | Esta programación no se inicia *antes* de la fecha y hora de inicio especificadas. Las futuras periodicidades se ejecutan cada hora en la marca de minuto "00". <p>Si la frecuencia es "Week" o "Month", esta programación se ejecuta respectivamente solo un día de la semana o un día del mes. | 
| Ejecutar cada hora todos los días (sin fecha y hora de inicio) | 1 | Hora | {none} | {unavailable} | {none} | {none} | Esta programación se inicia inmediatamente y calcula las futuras periodicidades en función de la hora de la última ejecución. <p>Si la frecuencia es "Week" o "Month", esta programación se ejecuta respectivamente solo un día de la semana o un día del mes. | 
| Ejecutar cada hora todos los días (con fecha y hora de inicio) | 1 | Hora | *startDate*T*startTime*Z | {unavailable} | {none} | {none} | Esta programación no se inicia *antes* de la fecha y hora de inicio especificadas y después calcula las futuras periodicidades en función de la hora de la última ejecución. <p>Si la frecuencia es "Week" o "Month", esta programación se ejecuta respectivamente solo un día de la semana o un día del mes. | 
| Ejecutar cada 15 minutos después de la hora, cada hora (con fecha y hora de inicio) | 1 | Hora | *startDate*T00:15:00Z | {unavailable} | {none} | {none} | Esta programación no se inicia *antes* de la fecha y hora de inicio especificadas; se ejecuta a las 00:15 a. m., 1:15 a. m., 2:15 a. m., etc. | 
| Ejecutar cada 15 minutos después de la hora, cada hora (sin fecha y hora de inicio) | 1 | Día | {none} | {unavailable} | 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23 | 15 | Esta programación se ejecuta a las 00:15 a. m., 1:15 a. m., 2:15 a. m., etc. Además, esta programación tiene una frecuencia equivalente de "Hour" y una hora de inicio de "15" minutos. | 
| Ejecutar cada 15 minutos en la marca de 15 minutos (sin fecha y hora de inicio) | 1 | Día | {none} | {unavailable} | 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23 | 0, 15, 30, 45 | Esta programación no se inicia hasta la siguiente marca especificada de 15 minutos. | 
| Ejecutar a las 8:00 a. m. todos los días (sin fecha y hora de inicio) | 1 | Día | {none} | {unavailable} | 8 | {none} | Esta programación se ejecuta a las 8:00 a. m. todos los días, según la programación especificada. | 
| Ejecutar a las 8:00 a. m. todos los días (con fecha y hora de inicio) | 1 | Día | *startDate*T08:00:00Z | {unavailable} | {none} | {none} | Esta programación se ejecuta a las 8:00 a. m. todos los días, según la hora de inicio especificada. | 
| Ejecutar a las 8:30 a. m. todos los días (sin fecha y hora de inicio) | 1 | Día | {none} | {unavailable} | 8 | 30 | Esta programación se ejecuta a las 8:30 a. m. todos los días, según la programación especificada. | 
| Ejecutar a las 8:30 a. m. todos los días (con fecha y hora de inicio) | 1 | Día | *startDate*T08:30:00Z | {unavailable} | {none} | {none} | Esta programación se inicia en la fecha de inicio especificada a las 8:30 a. m. | 
| Ejecutar a las 8:30 a. m. y 4:30 p. m. todos los días | 1 | Día | {none} | {unavailable} | 8, 16 | 30 | | 
| Ejecutar a las 8:30 a. m., 8:45 a. m., 4:30 p. m. y 4:45 p. m. todos los días | 1 | Día | {none} | {unavailable} | 8, 16 | 30, 45 | | 
| Ejecutar cada sábado a las 5 p. m. (ninguna fecha y hora de inicio) | 1 | Semana | {none} | "Saturday" | 17 | 00 | Esta programación se ejecuta cada sábado a las 5:00 p. m. | 
| Ejecutar cada sábado a las 5 p. m. (con fecha y hora de inicio) | 1 | Semana | *startDate*T17:00:00Z | "Saturday" | {none} | {none} | Esta programación no se inicia *antes* de la fecha y hora de inicio especificadas; en este caso, el 9 de septiembre de 2017 a las 5:00 p. m. Las futuras periodicidades se ejecutan todos los sábados a las 5:00 p. m. | 
| Ejecutar todos los martes y jueves a las 5:00 p. m. | 1 | Semana | {none} | "Tuesday", "Thursday" | 17 | {none} | Esta programación ejecuta todos los martes y jueves a las 5:00 p. m. | 
| Ejecutar cada hora durante el horario laboral | 1 | Semana | {none} | Seleccione todos los días excepto el sábado y el domingo. | Seleccione las horas del día que desee. | Seleccione los minutos de la hora que desee. | Por ejemplo, si su horario laboral es de 8:00 a. m. a las 5:00 p. m., entonces, seleccione "8, 9, 10, 11, 12, 13, 14, 15, 16, 17" como las horas del día. <p>Si su horario laboral es de 8:30 a. m. a las 5:30 p. m., seleccione las horas del día más "30" como los minutos de la hora. | 
| Ejecutar una vez al día los fines de semana | 1 | Semana | {none} | "Saturday", "Sunday" | Seleccione las horas del día que desee. | Seleccione los minutos de la hora que correspondan. | Esta programación se ejecuta todos los sábados y domingos según la programación especificada. | 
| Ejecutar cada 15 minutos quincenalmente y solo los lunes | 2 | Semana | {none} | "Monday" | 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23 | 0, 15, 30, 45 | Esta programación se ejecuta cada dos lunes en cada marca de 15 minutos. | 
| Ejecutar cada hora durante un día al mes | 1 | Mes | {see note} | {unavailable} | 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23 | {see note} | Si no se especifica una fecha y hora de inicio, esta programación usa la fecha y hora de creación. Para controlar los minutos de la programación de periodicidad, especifique los minutos de la hora, una hora de inicio o use la hora de creación. Por ejemplo, si la hora de inicio u hora de creación es 8:25 a. m., esta programación se ejecuta a las 8:25 a. m., 9:25 a. m., 10:25 a. m., y así sucesivamente. | 
||||||||| 

<a name="start-time"></a>

**P:** ¿De qué formas puedo usar la fecha y hora de inicio? </br>
**R:** A continuación, se presentan algunos patrones que muestran cómo se puede controlar la periodicidad con la fecha y hora de inicio, además de cómo Logic Apps ejecuta estas periodicidades:

| Hora de inicio | Periodicidad sin programación | Periodicidad con programación | 
| ---------- | --------------------------- | ------------------------ | 
| {none} | Ejecuta la primera carga de trabajo al instante. <p>Ejecuta futuras cargas de trabajo en función de la hora de la última ejecución. | Ejecuta la primera carga de trabajo al instante. <p>Ejecuta futuras cargas de trabajo en función de la programación especificada. | 
| Hora de inicio en el pasado | Calcula los tiempos de ejecución en función de la hora de inicio especificada y descarta las horas de ejecución anteriores. Ejecuta la primera carga de trabajo la hora de la próxima ejecución. <p>Ejecuta futuras cargas de trabajo en función de los cálculos de la hora de la última ejecución. <p>Para más información, vea el ejemplo a continuación de esta tabla. | Ejecuta la primera carga de trabajo *no antes* de la hora de inicio, en función de la programación calculada a partir de la hora de inicio. <p>Ejecuta futuras cargas de trabajo en función de la programación especificada. <p>**Nota:** Si especifica una periodicidad con una programación, pero no especifica horas o minutos para la programación, las horas de la futura ejecución se calculan usando las horas o los minutos, respectivamente, a partir de la hora de la primera ejecución. | 
| Hora de inicio en la actualidad o en el futuro | Ejecuta la primera carga de trabajo a la hora de inicio especificada. <p>Ejecuta futuras cargas de trabajo en función de los cálculos de la hora de la última ejecución. | Ejecuta la primera carga de trabajo *no antes* de la hora de inicio, en función de la programación calculada a partir de la hora de inicio. <p>Ejecuta futuras cargas de trabajo en función de la programación especificada. <p>**Nota:** Si especifica una periodicidad con una programación, pero no especifica horas o minutos para la programación, las horas de la futura ejecución se calculan usando las horas o los minutos, respectivamente, a partir de la hora de la primera ejecución. | 
||||

**Ejemplo de una hora de inicio anterior con periodicidad pero sin programación** 

| Hora de inicio | Hora actual | Periodicidad | Schedule |
| ---------- | ------------ | ---------- | -------- | 
| 2017-09-**07**T14:00:00Z | 2017-09-**08**T13:00:00Z | Cada 2 días | {none} | 
||||| 

En este escenario, el motor de Logic Apps calcula las horas de ejecución en función de la hora de inicio, descarta las horas de ejecución anteriores y usa la hora de inicio de la próxima ejecución para la primera ejecución. Después de la primera ejecución, las futuras ejecuciones se basan en la programación calculada a partir de la hora de inicio. Este es el aspecto de esta periocidad:

| Hora de inicio | Hora de la primera ejecución | Hora de las futuras ejecuciones | 
| ---------- | ------------ | ---------- | 
| **07**-09-2017 a las 2:00 p. m. | **09**-09-2017 a las 2:00 p. m. | **11**-09-2017 a las 2:00 p. m. </br>**13**-09-2017 a las 2:00 p. m. </br>**15**-09-2017 a las 2:00 p. m. </br>y así sucesivamente.
||||

Por tanto, en este escenario, no importa el tiempo que haya transcurrido desde que especificó la hora de inicio; por ejemplo, **05**-09-2017 a las 2:00 p. m. o **01**-09-2017 a las 2:00 p. m., ya que la hora de la primera ejecución es la misma.

## <a name="next-steps"></a>Pasos siguientes

* [Acciones y desencadenadores de flujo de trabajo](../logic-apps/logic-apps-workflow-actions-triggers.md#recurrence-trigger)
* [Conectores](../connectors/apis-list.md)
