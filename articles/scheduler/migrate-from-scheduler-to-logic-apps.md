---
title: Migración de Azure Scheduler to Azure Logic Apps
description: Aprenda a pasar los trabajos de Azure Scheduler, que se va a retirar, a Azure Logic Apps
services: scheduler
ms.service: scheduler
ms.suite: infrastructure-services
author: derek1ee
ms.author: deli
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 09/20/2018
ms.openlocfilehash: c841f29adbe9911193227cced2856d953d820b08
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46997291"
---
# <a name="migrate-azure-scheduler-jobs-to-azure-logic-apps"></a>Migración de trabajos de Azure Scheduler to Azure Logic Apps

> [!IMPORTANT]
> Azure Logic Apps reemplaza a Azure Scheduler, que se va a retirar. Para programar los trabajos, siga las instrucciones de este artículo para migrar a Azure Logic Apps.

En este artículo se muestra cómo se pueden programar tanto los trabajos únicos como los periódicos mediante la creación de flujos de trabajo automatizados con Azure Logic Apps, en lugar de con Azure Scheduler. Si se crean trabajos programados con Logic Apps, se obtienen estas ventajas:

* No tiene que preocuparse por el concepto de una *colección de trabajos*, ya que cada aplicación lógica es un recurso de Azure independiente.

* Mediante una aplicación lógica individual es posible ejecutar varios trabajos únicos.

* El servicio Azure Logic Apps admite tanto la zona horaria como el horario de verano (DST).

Para más información, consulte [¿Qué es Azure Logic Apps?](../logic-apps/logic-apps-overview.md) o pruebe a crear su primera aplicación lógica en esta guía de inicio rápido: [Creación de una aplicación lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción de Azure. Si no tiene una suscripción de Azure, <a href="https://azure.microsoft.com/free/" target="_blank">regístrese para obtener una cuenta gratuita de Azure</a>.

* Para desencadenar una aplicación lógica mediante el envío de solicitudes HTTP, use una herramienta como la [aplicación de escritorio de Postman](https://www.getpostman.com/apps).

## <a name="schedule-one-time-jobs"></a>Programación de trabajos únicos

La creación de una aplicación lógica individual permite ejecutar varios trabajos únicos. 

### <a name="create-your-logic-app"></a>Creación de una aplicación lógica

1. En [Azure Portal](https://portal.azure.com), cree una aplicación lógica en blanco en el Diseñador de aplicaciones lógicas. 

   Para conocer los pasos básicos, [Guía de inicio rápido: Creación del primer flujo de trabajo automatizado con Azure Logic Apps: Azure Portal](../logic-apps/quickstart-create-first-logic-app-workflow.md).

1. En el cuadro de búsqueda, escriba "when a http request" como filtro. En la lista de desencadenadores, seleccione el siguiente: **Cuando se recibe una solicitud HTTP** 

   ![Agregar un desencadenador de "solicitud"](./media/migrate-from-scheduler-to-logic-apps/request-trigger.png)

1. En el caso del desencadenador de solicitud, también puede proporcionar un esquema de JSON, que ayuda al Diseñador de aplicaciones lógicas a conocer la estructura de las entradas de la solicitud entrante y facilita la selección de salidas más adelante en el flujo de trabajo.

   Para especificar un esquema, especifique el esquema en el cuadro **Esquema JSON de cuerpo de solicitud**, por ejemplo: 

   ![Esquema de solicitud](./media/migrate-from-scheduler-to-logic-apps/request-schema.png)

   Si no tiene un esquema, pero tiene una carga de ejemplo en formato JSON, puede generar un esquema a partir de dicha carga.

   1. En el desencadenador Request, elija **Usar una carga de ejemplo para generar el esquema**.

   1. En **Enter or paste a sample JSON payload** (Escribir o copiar una carga de JSON de ejemplo), especifique la carga de ejemplo y, después, elija **Listo**, por ejemplo:

      ![Carga de ejemplo](./media/migrate-from-scheduler-to-logic-apps/sample-payload.png)

1. En el desencadenador, elija **Next step** (Paso siguiente). 

1. En el cuadro de búsqueda, escriba "retraso hasta" como filtro. En la lista de acciones, seleccione esta acción: **Retraso hasta**

   Dicha acción detiene el flujo de la aplicación lógica hasta una fecha y hora especificadas.

   ![Agregar acción "Retraso hasta"](./media/migrate-from-scheduler-to-logic-apps/delay-until.png)

1. Especifique la marca de tiempo del momento en que desea iniciar el flujo de trabajo de la aplicación lógica. 

   Al hacer clic dentro del cuadro **Marca de tiempo** aparece la lista de contenido dinámico, con el fin de que tenga la opción de seleccionar una salida del desencadenador.

   ![Especificar los detalles de "Retraso hasta"](./media/migrate-from-scheduler-to-logic-apps/delay-until-details.png)

1. Agregue todas las acciones que desee ejecutar mediante la selección de [~ más de 200 conectores](../connectors/apis-list.md). 

   Por ejemplo, puede incluir una acción de HTTP que envía una solicitud a una dirección URL o las acciones que trabajar con Colas de almacenamiento, colas de Service Bus o temas de Service Bus: 

   ![Acción HTTP](./media/migrate-from-scheduler-to-logic-apps/request-http-action.png)

1. Cuando haya terminado, guarde la aplicación lógica.

   ![Guardado de la aplicación lógica](./media/migrate-from-scheduler-to-logic-apps/save-logic-app.png)

   La primera vez que se guarda una aplicación lógica por primera vez, la dirección URL del punto de conexión del desencadenador de solicitud de la aplicación lógica aparece en el cuadro **Dirección URL de HTTP POST**. 
   Si desea llamar a la aplicación lógica y enviar las entradas a ella para su procesamiento, use esta dirección URL como destino de la llamada.

   ![Guardar la dirección URL del punto de conexión del desencadenador de solicitud](./media/migrate-from-scheduler-to-logic-apps/request-endpoint-url.png)

1. Copie y guarde la dirección URL del punto de conexión, con el fin de que posteriormente pueda enviar una solicitud manual que desencadene la aplicación lógica. 

## <a name="start-a-one-time-job"></a>Iniciar un trabajo único

Para ejecutar o desencadenar un trabajo único, envíe una llamada a la dirección URL del punto de conexión del desencadenador de solicitud de la aplicación lógica. En esta llamada, especifique la entrada o la carga que envía, que es posible que ha descrito anteriormente mediante la especificación de un esquema. 

Por ejemplo, mediante la aplicación Postman puede crear una solicitud POST con una configuración similar a la de este ejemplo y, después, elija **Send** (Enviar) para realizar la solicitud.

| Método de solicitud | URL | Cuerpo | encabezados |
|----------------|-----|------|---------| 
| **POST** | <*endpoint-URL*> | **raw** (sin formato) <p>**JSON(application/json)** <p>En el cuadro **raw** (sin formato), escriba la carga que desee enviar en la solicitud. <p>**Nota**: esta configuración automáticamente configura los valores de **Headers** (Encabezados). | **Clave**: Content-Type <br>**Valor**: application/json
 |||| 

![Enviar solicitud para desencadenar manualmente la aplicación lógica](./media/migrate-from-scheduler-to-logic-apps/postman-send-post-request.png)

Después de enviar la llamada, la respuesta de la aplicación lógica aparece bajo el cuadro **raw** (sin formato) de la pestaña **Body** (Cuerpo). 

<a name="workflow-run-id"></a>

> [!IMPORTANT]
>
> Si desea cancelar el trabajo posteriormente, elija la pestaña **Headers**. Busque y copie el valor del encabezado **x-ms-flujo de trabajo-run-id** de la respuesta. 
>
> ![Response](./media/migrate-from-scheduler-to-logic-apps/postman-response.png)

## <a name="cancel-a-one-time-job"></a>Cancelar un trabajo único

En Logic Apps, cada trabajo único se ejecuta como una instancia de la ejecución de una aplicación lógica. Para cancelar un trabajo único, puede usar la [ejecución de flujos de trabajo - Cancel](https://docs.microsoft.com/rest/api/logic/workflowruns/cancel) en la API REST de Logic Apps. Si envía una llamada al desencadenador, especifique el [identificador de la ejecución del flujo de trabajo](#workflow-run-id).

## <a name="schedule-recurring-jobs"></a>Programar trabajos repetitivos

### <a name="create-your-logic-app"></a>Creación de una aplicación lógica

1. En [Azure Portal](https://portal.azure.com), cree una aplicación lógica en blanco en el Diseñador de aplicaciones lógicas. 

   Para conocer los pasos básicos, [Guía de inicio rápido: Creación del primer flujo de trabajo automatizado con Azure Logic Apps: Azure Portal](../logic-apps/quickstart-create-first-logic-app-workflow.md).

1. En el cuadro de búsqueda, escriba "periodicidad" para el filtro. En la lista de desencadenadores, seleccione este: **Periodicidad** 

   ![Agregar desencadenador "Periodicidad"](./media/migrate-from-scheduler-to-logic-apps/recurrence-trigger.png)

1. Configure una programación más avanzada, si lo desea.

   ![Programación avanzada](./media/migrate-from-scheduler-to-logic-apps/recurrence-advanced-schedule.png)

   Para más información acerca de las opciones de programación avanzadas, consulte [Creación y ejecución de tareas y flujos de trabajo repetitivos con Azure Logic Apps](../connectors/connectors-native-recurrence.md)

1. Agregue todas las acciones que desee mediante la selección de [Más de 200 conectores](../connectors/apis-list.md). En el desencadenador, elija **Next step** (Paso siguiente). Busque y seleccione las acciones que desea.

   Por ejemplo, puede incluir una acción de HTTP que envía una solicitud a una dirección URL o las acciones que trabajar con Colas de almacenamiento, colas de Service Bus o temas de Service Bus: 

   ![Acción HTTP](./media/migrate-from-scheduler-to-logic-apps/recurrence-http-action.png)

1. Cuando haya terminado, guarde la aplicación lógica.

   ![Guardado de la aplicación lógica](./media/migrate-from-scheduler-to-logic-apps/save-logic-app.png)

## <a name="advanced-setup"></a>Configuración avanzada

Estas son otras formas de personalizar los trabajos.

### <a name="retry-policy"></a>Directiva de reintentos

Para controlar la forma en que las acciones intentan volver a ejecutarse en una aplicación lógica cuando se producen errores intermitentes, puede establecer la [directiva de reintentos](../logic-apps/logic-apps-exception-handling.md#retry-policies) en la configuración de cada acción, por ejemplo:

1. Abra el menú (**...** ) de la acción y seleccione **Configuración**.

   ![Abrir la configuración de la acción](./media/migrate-from-scheduler-to-logic-apps/action-settings.png)

1. Seleccione la directiva de reintentos que desea. Para más información acerca de cada directiva, consulte [Directivas de reintentos](../logic-apps/logic-apps-exception-handling.md#retry-policies).

   ![Seleccionar directiva de reintentos](./media/migrate-from-scheduler-to-logic-apps/retry-policy.png)

## <a name="handle-exceptions-and-errors"></a>Control de excepciones y errores

En Azure Scheduler, si la acción predeterminada no se ejecuta, puede ejecutar una acción alternativa que solucione el error. En Azure Logic Apps, también puede realizar la misma tarea.

1. En el Diseñador de aplicaciones lógicas, encima de la acción que desee administrar, mueva el puntero sobre la flecha entre los pasos y seleccione **Agregar una rama paralela**. 

   ![Incorporación de una rama paralela](./media/migrate-from-scheduler-to-logic-apps/add-parallel-branch.png)

1. Busque y seleccione la acción que desee ejecutar en su lugar como acción alternativa.

   ![Agregar acción paralela](./media/migrate-from-scheduler-to-logic-apps/add-parallel-action.png)

1. En la acción alternativa, abra el menú (**...** ) y seleccione **Configurar ejecución posterior**.

   ![Configurar ejecución posterior](./media/migrate-from-scheduler-to-logic-apps/configure-run-after.png)

1. Desactive la casilla de la propiedad **correcto**. Seleccione estas propiedades: **con errores**, **se omitió** y **se superó el tiempo de espera**

   ![Configurar las propiedades de "ejecución posterior"](./media/migrate-from-scheduler-to-logic-apps/select-run-after-properties.png)

1. Cuando haya finalizado, elija **Listo**.

Para más información acerca del control de excepciones, consulte [Detección y control de errores con la propiedad RunAfter](../logic-apps/logic-apps-exception-handling.md#catch-and-handle-failures-with-the-runafter-property).

## <a name="faq"></a>Preguntas más frecuentes

<a name="retire-date"></a> 

**P**: ¿Cuando se va a retirar Azure Scheduler? <br>
**R**: Está programado que Azure Scheduler se retire el 30 de septiembre de 2019.

**P**: ¿Qué ocurrirá con mis colecciones de trabajos y mis trabajos de Scheduler cuando se retire el servicio? <br>
**R**: Todas las colecciones de trabajos y los trabajos de Scheduler se eliminarán del sistema.

**P**: ¿Tengo que hacer una copia de seguridad o realizar cualquier otra tarea antes de migrar mis trabajos de Scheduler a Logic Apps? <br>
**R**: Como procedimiento recomendado, realice siempre una copia de seguridad de su trabajo. Compruebe que las aplicaciones lógicas que creó se ejecutan según lo esperado antes de eliminar o deshabilitar los trabajos de Scheduler. 

**P**: ¿Hay alguna herramienta que pueda ayudarme a migrar mis trabajos de Scheduler a Logic Apps? <br>
**R**: Cada trabajo de Scheduler es único, por lo que no existe una que sirva para todos los trabajos. Sin embargo, hay varios scripts disponibles que puede modificar para ajustarlos a sus necesidades. Para obtener información acerca de la disponibilidad de los scripts, realice la consulta más adelante.

**P**: ¿Dónde puedo obtener soporte técnico para migrar mis trabajos de Scheduler? <br>
**R**: Estas son algunas formas de obtener soporte técnico: 

**Azure Portal**

Si su suscripción de Azure tiene un plan de soporte técnico de pago, puede crear una solicitud de soporte en Azure Portal. En caso contrario, puede seleccionar otra opción diferente de soporte técnico.

1. En el menú principal de [Azure Portal](https://portal.azure.com), seleccione **Ayuda y soporte técnico**.

1. En **Soporte técnico**, seleccione **Nueva solicitud de soporte técnico**. Incluya estos datos en la solicitud:

   | Configuración | Valor |
   |---------|-------|
   | **Tipo de problema** | **Técnico** | 
   | **Suscripción** | <*su-suscripción-de-Azure*> | 
   | **Servicio** | En **Supervisión y administración**, seleccione **Scheduler**. | 
   ||| 

1. Seleccione la opción de soporte técnico que desea. Si tiene un plan de soporte técnico de pago, elija **Siguiente**.

**Comunidad**

* [Foro de Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps)
* [Desbordamiento de la pila](https://stackoverflow.com/questions/tagged/azure-scheduler)

## <a name="next-steps"></a>Pasos siguientes

* [Creación de tareas y flujos de trabajo que se ejecutan regularmente con Azure Logic Apps](../connectors/connectors-native-recurrence.md)
* [Tutorial: Comprobación del tráfico con una aplicación lógica basada en una programación](../logic-apps/tutorial-build-schedule-recurring-logic-app-workflow.md)