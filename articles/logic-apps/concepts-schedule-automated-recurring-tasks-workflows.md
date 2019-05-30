---
title: Programar tareas repetitivas y flujos de trabajo en Azure Logic Apps
description: Información general sobre la programación periódicas tareas automatizadas, procesos y flujos de trabajo con Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: deli, klam, LADocs
ms.topic: conceptual
ms.date: 05/25/2019
ms.openlocfilehash: 7f15dc5b28a44dc8405e2f0524913e6012ebe380
ms.sourcegitcommit: 8e76be591034b618f5c11f4e66668f48c090ddfd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66356045"
---
# <a name="schedule-and-run-recurring-automated-tasks-processes-and-workflows-with-azure-logic-apps"></a>Programar y ejecutar tareas automatizadas periódicas, procesos y flujos de trabajo con Azure Logic Apps

Logic Apps le ayuda a crear y ejecutar tareas repetitivas automatizadas y los procesos según una programación. Mediante la creación de un flujo de trabajo de aplicación lógica que se inicia con un desencadenador periodicidad o la ventana deslizante integrado, que son los desencadenadores del tipo de programación, puede ejecutar tareas inmediatamente, en un momento posterior, o en un intervalo periódico. Puede llamar a servicios dentro y fuera de Azure, como los puntos de conexión HTTP o HTTPS, enviar mensajes a servicios de Azure como Azure Storage y Azure Service Bus u obtener los archivos cargados en un recurso compartido de archivos. Con el desencadenador de periodicidad, también puede establecer las programaciones complejas y avanzadas de periodicidad para la ejecución de tareas. Para obtener más información acerca de los desencadenadores de programación integrados y acciones, vea [programación y ejecución periódica automatizada, tareas y flujos de trabajo con Azure Logic Apps](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md).

Estos son algunos ejemplos que muestran los tipos de tareas que se pueden ejecutar:

* Obtener datos internos, como ejecutar un procedimiento almacenado SQL cada día.

* Obtener datos externos, como la extracción meteorológicos de NOAA cada 15 minutos.

* Enviar datos de informe como correo electrónico un resumen de todos los pedidos mayores que una cantidad específica de la semana pasada.

* Procesar los datos, como compress hoy en día cargado todos los días laborables durante las horas de imágenes.

* Limpiar datos, como eliminar todos los tweets de más de tres meses.

* Archivar datos, como facturas de inserción a un servicio de copia de seguridad 1:00 a.m. cada día durante los próximos 9 meses.

También puede usar las acciones integradas de programación para pausar el flujo de trabajo antes de que ejecute la siguiente acción, por ejemplo:

* Esperar hasta un día de la semana para enviar una actualización de estado por correo electrónico.

* Retrasar el flujo de trabajo hasta que una llamada HTTP tenga tiempo para completarse antes de reanudarse y recuperar el resultado.

En este artículo se describe las funcionalidades de los desencadenadores de programación integrados y acciones.

## <a name="schedule-triggers"></a>Desencadenadores de programación

Puede iniciar la aplicación lógica mediante el desencadenador de periodicidad o el desencadenador de ventana deslizante, que no estén asociados con ningún servicio específico o del sistema, por ejemplo, Office 365 Outlook o SQL Server. Estos desencadenadores se inicie y ejecutan el flujo de trabajo en función de la periodicidad especificada donde selecciona el intervalo y la frecuencia, como el número de segundos, minutos y horas para los desencadenadores, o el número de días, semanas o meses para el desencadenador de periodicidad. También puede establecer la fecha de inicio y hora, así como la zona horaria. Cada vez que se activa un desencadenador, Logic Apps crea y ejecuta una nueva instancia de flujo de trabajo para la aplicación lógica.

Estas son las diferencias entre estos desencadenadores:

* **Periodicidad**: Se ejecuta el flujo de trabajo en intervalos regulares según la programación especificada. Si se pierden las repeticiones, el desencadenador de periodicidad no procesa las repeticiones que faltan pero reinicia las repeticiones con el siguiente intervalo programado. Puede especificar una fecha de inicio y hora, así como la zona horaria. Si selecciona "Day", puede especificar horas del día y los minutos de la hora, por ejemplo, cada día a las 2:30. Si selecciona "Week", también puede seleccionar los días de la semana, por ejemplo, el miércoles y el sábado. Para obtener más información, consulte [Create, programación y ejecutadas tareas repetitivas y flujos de trabajo con el desencadenador de periodicidad](../connectors/connectors-native-recurrence.md).

* **Ventana deslizante**: Se ejecuta el flujo de trabajo a intervalos de tiempo regulares que administran los datos en fragmentos continua. Si se pierden las repeticiones, el desencadenador de ventana deslizante vuelve y procesa las repeticiones que faltan. Puede especificar una fecha de inicio y hora, zona horaria y una duración para retrasar cada repetición del flujo de trabajo. Este desencadenador no tiene opciones para especificar los días, semanas y meses, horas del día, los minutos de la hora y los días de la semana. Para obtener más información, consulte [Create, programación y ejecutadas tareas repetitivas y flujos de trabajo con el desencadenador de ventana deslizante](../connectors/connectors-native-sliding-window.md).

## <a name="schedule-actions"></a>Programar acciones

Después de cualquier acción en la aplicación lógica, puede usar las acciones de retraso y retraso hasta que para hacer que el flujo de trabajo espere antes de ejecutar en la siguiente acción.

* **retraso**: Espere a que se ejecuta la siguiente acción para el número especificado de unidades de tiempo, por ejemplo, segundos, minutos, horas, días, semanas o meses. Para obtener más información, consulte [retrasar la acción siguiente en los flujos de trabajo](../connectors/connectors-native-delay.md).

* **Retraso hasta**: Espere a que se ejecute la siguiente acción hasta la fecha y hora especificadas. Para obtener más información, consulte [retrasar la acción siguiente en los flujos de trabajo](../connectors/connectors-native-delay.md).

## <a name="patterns-for-start-date-and-time"></a>Patrones de fecha y hora iniciales

<a name="start-time"></a>

Estos son algunos patrones que muestran cómo puede controlar la periodicidad con la fecha de inicio y hora, y cómo los servicios de Logic Apps ejecuta estas periodicidades:

| Hora de inicio | Periodicidad sin programación | Periodicidad con programación (desencadenador de periodicidad solo) |
|------------|-----------------------------|----------------------------------------------------|
| {none} | Ejecuta la primera carga de trabajo al instante. <p>Ejecuta futuras cargas de trabajo en función de la hora de la última ejecución. | Ejecuta la primera carga de trabajo al instante. <p>Ejecuta futuras cargas de trabajo en función de la programación especificada. |
| Hora de inicio en el pasado | **Periodicidad** desencadenador: Calcula los tiempos de ejecución en función de la hora de inicio especificada y descarta las horas de ejecución anteriores. Ejecuta la primera carga de trabajo la hora de la próxima ejecución. <p>Ejecuta futuras cargas de trabajo en función de los cálculos de la hora de la última ejecución. <p><p>**Ventana deslizante** desencadenador: Calcula los tiempos de ejecución según la hora de inicio especificada y respeta últimos ejecutar veces. <p>Ejecuta futuras cargas de trabajo según los cálculos de la hora de inicio especificada. <p><p>Para más información, vea el ejemplo a continuación de esta tabla. | Ejecuta la primera carga de trabajo *no antes* de la hora de inicio, en función de la programación calculada a partir de la hora de inicio. <p>Ejecuta futuras cargas de trabajo en función de la programación especificada. <p>**Nota:** Si especifica una periodicidad con una programación, pero no especifica horas o minutos para la programación, las horas de la futura ejecución se calculan usando las horas o los minutos, respectivamente, a partir de la hora de la primera ejecución. |
| Hora de inicio en la actualidad o en el futuro | Ejecuta la primera carga de trabajo a la hora de inicio especificada. <p>Ejecuta futuras cargas de trabajo en función de los cálculos de la hora de la última ejecución. | Ejecuta la primera carga de trabajo *no antes* de la hora de inicio, en función de la programación calculada a partir de la hora de inicio. <p>Ejecuta futuras cargas de trabajo en función de la programación especificada. <p>**Nota:** Si especifica una periodicidad con una programación, pero no especifica horas o minutos para la programación, las horas de la futura ejecución se calculan usando las horas o los minutos, respectivamente, a partir de la hora de la primera ejecución. |
||||

*Ejemplo de los últimos hora de inicio y periodicidad pero sin programación*

Imagine que la fecha y hora actual es el 8 de septiembre de 2017 a las 13:00. Especifique la fecha de inicio y la hora como el 7 de septiembre de 2017 a las 2:00 P.M., que se encuentra en el pasado y una periodicidad que se ejecuta cada 2 días.

| Hora de inicio | Hora actual | Periodicidad | Schedule |
|------------|--------------|------------|----------|
| 2017-09-**07**T14:00:00Z <br>(2017-09 -**07** a las 2:00 P.M.) | 2017-09-**08**T13:00:00Z <br>(2017-09 -**08** a la 1:00 P.M.) | Cada 2 días | {none} |
|||||

Para el desencadenador de periodicidad, descarta el Logic Apps calcula motor ejecutarse según la hora de inicio, más allá de los tiempos de ejecución, usa la próxima hora de inicio para la primera ejecución y calcula futuro se ejecuta según la hora de última ejecución.

Este es el aspecto de esta periocidad:

| Hora de inicio | Hora de la primera ejecución | Hora de las futuras ejecuciones |
|------------|----------------|------------------|
| **07**-09-2017 a las 2:00 p. m. | **09**-09-2017 a las 2:00 p. m. | **11**-09-2017 a las 2:00 p. m. </br>**13**-09-2017 a las 2:00 p. m. </br>**15**-09-2017 a las 2:00 p. m. </br>y así sucesivamente. |
||||

Por lo tanto, sin importar cuánto en el pasado especifique la hora de inicio, por ejemplo, 2017-09 -**05** a las 2:00 o 2017-09 -**01** a las 2:00 P.M., la primera ejecución siempre usa la próxima hora de inicio.

Para el desencadenador de ventana deslizante, las aplicaciones lógicas motor calcula ejecutarse según la hora de inicio, respeta últimos tiempos de ejecución, usa la hora de inicio para la primera ejecución y calcula las futuras ejecuciones según la hora de inicio.

Este es el aspecto de esta periocidad:

| Hora de inicio | Hora de la primera ejecución | Hora de las futuras ejecuciones |
|------------|----------------|------------------|
| **07**-09-2017 a las 2:00 p. m. | **07**-09-2017 a las 2:00 p. m. | **09**-09-2017 a las 2:00 p. m. </br>**11**-09-2017 a las 2:00 p. m. </br>**13**-09-2017 a las 2:00 p. m. </br>**15**-09-2017 a las 2:00 p. m. </br>y así sucesivamente. |
||||

Por lo tanto, sin importar cuánto en el pasado especifique la hora de inicio, por ejemplo, 2017-09 -**05** a las 2:00 o 2017-09 -**01** a las 2:00 P.M., la primera ejecución siempre usa especificado hora de inicio.

<a name="example-recurrences"></a>

## <a name="example-recurrences"></a>Repeticiones de ejemplo

Existen varias repeticiones de ejemplo que puede configurar para los desencadenadores que admiten las opciones:

| Desencadenador | Periodicidad | Interval | Frecuencia | Hora de inicio | En estos días | A estas horas | En estos minutos | Nota: |
|---------|------------|----------|-----------|------------|---------------|----------------|------------------|------|
| Periodicidad, <br>Ventana deslizante | Ejecutar cada 15 minutos (ninguna fecha y hora de inicio) | 15 | Minute | {none} | {unavailable} | {none} | {none} | Esta programación se inicia inmediatamente y después calcula las futuras periodicidades en función de la hora de la última ejecución. |
| Periodicidad, <br>Ventana deslizante | Ejecutar cada 15 minutos (con fecha y hora de inicio) | 15 | Minute | *startDate*T*startTime*Z | {unavailable} | {none} | {none} | Esta programación no se inicia *antes* de la fecha y hora de inicio especificadas y después calcula las futuras periodicidades en función de la hora de la última ejecución. |
| Periodicidad, <br>Ventana deslizante | Ejecutar cada hora, a la hora (con fecha y hora de inicio) | 1 | Hour | *startDate*Thh:00:00Z | {unavailable} | {none} | {none} | Esta programación no se inicia *antes* de la fecha y hora de inicio especificadas. Las futuras periodicidades ejecutan cada hora en la marca de minuto "00", que se calcula a partir de la hora de inicio. <p>Si la frecuencia es "Week" o "Month", esta programación se ejecuta respectivamente solo un día de la semana o un día del mes. |
| Periodicidad, <br>Ventana deslizante | Ejecutar cada hora todos los días (sin fecha y hora de inicio) | 1 | Hour | {none} | {unavailable} | {none} | {none} | Esta programación se inicia inmediatamente y calcula las futuras periodicidades en función de la hora de la última ejecución. <p>Si la frecuencia es "Week" o "Month", esta programación se ejecuta respectivamente solo un día de la semana o un día del mes. |
| Periodicidad, <br>Ventana deslizante | Ejecutar cada hora todos los días (con fecha y hora de inicio) | 1 | Hour | *startDate*T*startTime*Z | {unavailable} | {none} | {none} | Esta programación no se inicia *antes* de la fecha y hora de inicio especificadas y después calcula las futuras periodicidades en función de la hora de la última ejecución. <p>Si la frecuencia es "Week" o "Month", esta programación se ejecuta respectivamente solo un día de la semana o un día del mes. |
| Periodicidad, <br>Ventana deslizante | Ejecutar cada 15 minutos después de la hora, cada hora (con fecha y hora de inicio) | 1 | Hour | *startDate*T00:15:00Z | {unavailable} | {none} | {none} | Esta programación no se inicia *antes* de la fecha y hora de inicio especificadas. Las futuras periodicidades se ejecutan en la marca de minuto "15", que se calcula desde el inicio de tiempo, lo que a las 00:15 A.M., 1:15 A.M., 2:15 A.M. y así sucesivamente. |
| Periodicidad | Ejecutar cada 15 minutos después de la hora, cada hora (sin fecha y hora de inicio) | 1 | Día | {none} | {unavailable} | 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23 | 15 | Esta programación se ejecuta a las 00:15 a. m., 1:15 a. m., 2:15 a. m., etc. Además, esta programación tiene una frecuencia equivalente de "Hour" y una hora de inicio de "15" minutos. |
| Periodicidad | Ejecutar cada 15 minutos en las marcas especificadas por minuto (ninguna fecha de inicio y hora). | 1 | Día | {none} | {unavailable} | 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23 | 0, 15, 30, 45 | Esta programación no se inicia hasta la siguiente marca especificada de 15 minutos. |
| Periodicidad | Ejecutar a las 8:00 a. m. todos los días (sin fecha y hora de inicio) | 1 | Día | {none} | {unavailable} | 8 | {none} | Esta programación se ejecuta a las 8:00 a. m. todos los días, según la programación especificada. |
| Periodicidad | Ejecutar a las 8:00 a. m. todos los días (con fecha y hora de inicio) | 1 | Día | *startDate*T08:00:00Z | {unavailable} | {none} | {none} | Esta programación se ejecuta a las 8:00 a. m. todos los días, según la hora de inicio especificada. | 
| Periodicidad | Ejecutar a las 8:30 a. m. todos los días (sin fecha y hora de inicio) | 1 | Día | {none} | {unavailable} | 8 | 30 | Esta programación se ejecuta a las 8:30 a. m. todos los días, según la programación especificada. |
| Periodicidad | Ejecutar a las 8:30 a. m. todos los días (con fecha y hora de inicio) | 1 | Día | *startDate*T08:30:00Z | {unavailable} | {none} | {none} | Esta programación se inicia en la fecha de inicio especificada a las 8:30 a. m. |
| Periodicidad | Ejecutar a las 8:30 a. m. y 4:30 p. m. todos los días | 1 | Día | {none} | {unavailable} | 8, 16 | 30 | |
| Periodicidad | Ejecutar a las 8:30 a. m., 8:45 a. m., 4:30 p. m. y 4:45 p. m. todos los días | 1 | Día | {none} | {unavailable} | 8, 16 | 30, 45 | |
| Periodicidad | Ejecutar cada sábado a las 5 p. m. (ninguna fecha y hora de inicio) | 1 | Semana | {none} | "Saturday" | 17 | 00 | Esta programación se ejecuta cada sábado a las 5:00 p. m. |
| Periodicidad | Ejecutar cada sábado a las 5 p. m. (con fecha y hora de inicio) | 1 | Semana | *startDate*T17:00:00Z | "Saturday" | {none} | {none} | Esta programación no se inicia *antes* de la fecha y hora de inicio especificadas; en este caso, el 9 de septiembre de 2017 a las 5:00 p. m. Las futuras periodicidades se ejecutan todos los sábados a las 5:00 p. m. |
| Periodicidad | Ejecutar todos los martes y jueves a las 5:00 p. m. | 1 | Semana | {none} | "Tuesday", "Thursday" | 17 | {none} | Esta programación ejecuta todos los martes y jueves a las 5:00 p. m. |
| Periodicidad | Ejecutar cada hora durante el horario laboral | 1 | Semana | {none} | Seleccione todos los días excepto el sábado y el domingo. | Seleccione las horas del día que desee. | Seleccione los minutos de la hora que desee. | Por ejemplo, si su horario laboral es de 8:00 a. m. a las 5:00 p. m., entonces, seleccione "8, 9, 10, 11, 12, 13, 14, 15, 16, 17" como las horas del día. <p>Si su horario laboral es de 8:30 a. m. a las 5:30 p. m., seleccione las horas del día más "30" como los minutos de la hora. |
| Periodicidad | Ejecutar una vez al día los fines de semana | 1 | Semana | {none} | "Saturday", "Sunday" | Seleccione las horas del día que desee. | Seleccione los minutos de la hora que correspondan. | Esta programación se ejecuta todos los sábados y domingos según la programación especificada. |
| Periodicidad | Ejecutar cada 15 minutos quincenalmente y solo los lunes | 2 | Semana | {none} | "Monday" | 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23 | 0, 15, 30, 45 | Esta programación se ejecuta cada dos lunes en cada marca de 15 minutos. |
| Periodicidad | Ejecutar cada mes | 1 | Mes | *startDate*T*startTime*Z | {unavailable} | {unavailable} | {unavailable} | Esta programación no se inicia *antes* de una fecha y hora de inicio y calcula las futuras periodicidades en la fecha de inicio y hora especificado. Si no se especifica una fecha y hora de inicio, esta programación usa la fecha y hora de creación. |
| Periodicidad | Ejecutar cada hora durante un día al mes | 1 | Mes | {see note} | {unavailable} | 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23 | {see note} | Si no se especifica una fecha y hora de inicio, esta programación usa la fecha y hora de creación. Para controlar los minutos de la programación de periodicidad, especifique los minutos de la hora, una hora de inicio o use la hora de creación. Por ejemplo, si la hora de inicio u hora de creación es 8:25 a. m., esta programación se ejecuta a las 8:25 a. m., 9:25 a. m., 10:25 a. m., y así sucesivamente. |
|||||||||

<a name="run-once"></a>

## <a name="run-one-time-only"></a>Ejecutar sólo una vez

Si desea ejecutar la aplicación lógica solo al mismo tiempo en el futuro, puede usar el **programador: Run once jobs** (Programador:ejecutar trabajos una vez). Después de crear una nueva aplicación lógica, pero antes de abrir el Diseñador de aplicaciones lógicas, bajo el **plantillas** sección, desde el **categoría** lista, seleccione **programación**y, a continuación, seleccione Esta plantilla:

![Seleccione la plantilla "Scheduler: Run once jobs" (Programador:ejecutar trabajos una vez)](./media/concepts-schedule-automated-recurring-tasks-workflows/choose-run-once-template.png)

O bien, si se puede iniciar la aplicación lógica con el **cuando recibe una solicitud HTTP es - solicitud** desencadenar y pase la hora de inicio como un parámetro para el desencadenador. Para la primera acción, use el **retraso hasta: programar** acción y proporcionar el tiempo para cuando la siguiente acción comienza a ejecutarse.

## <a name="next-steps"></a>Pasos siguientes

* [Crear, programar y ejecutar tareas repetitivas y flujos de trabajo con el desencadenador de periodicidad](../connectors/connectors-native-recurrence.md)
* [Crear, programar y ejecutar tareas repetitivas y flujos de trabajo con el desencadenador de ventana deslizante](../connectors/connectors-native-sliding-window.md)
* [Flujos de trabajo de pausa con acciones de retraso](../connectors/connectors-native-delay.md)