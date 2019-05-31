---
title: Retrasar la acción siguiente en los flujos de trabajo - Azure Logic Apps
description: Esperar a ejecutar la siguiente acción en aplicaciones lógicas mediante el uso de las acciones de retraso o retraso hasta que en Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: deli, klam, LADocs
tags: connectors
ms.topic: conceptual
ms.date: 05/25/2019
ms.openlocfilehash: 27475fb3f086dbc5166a473e9d657d2dab723938
ms.sourcegitcommit: 8c49df11910a8ed8259f377217a9ffcd892ae0ae
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66297603"
---
# <a name="delay-running-the-next-action-in-azure-logic-apps"></a>Retraso que se ejecuta la acción siguiente en Azure Logic Apps

Para que la aplicación lógica espera una cantidad de tiempo antes de ejecutar la siguiente acción, puede agregar la integrada **retraso: programar** acción antes de realizar una acción en el flujo de trabajo de la aplicación lógica. O bien, puede agregar la integrada **retraso hasta: programar** acción espera hasta una fecha y hora específicas antes de ejecutar la siguiente acción. Para obtener más información acerca de los desencadenadores y acciones de programación integradas, consulte [programación y ejecución periódica automatizada, tareas y flujos de trabajo con Azure Logic Apps](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md).

* **retraso**: Espere a que el número especificado de unidades de tiempo, por ejemplo, segundos, minutos, horas, días, semanas o meses, antes de ejecutar en la siguiente acción.

* **Retraso hasta**: Espere hasta la fecha y hora especificadas antes de ejecutar en la siguiente acción.

Estas son algunas maneras de ejemplo para usar estas acciones:

* Esperar hasta un día de la semana para enviar una actualización de estado por correo electrónico.

* Retrasar el flujo de trabajo hasta que finalice una llamada HTTP antes de reanudarse y recuperar datos.

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción de Azure. Si no tiene una suscripción, puede [registrarse para obtener una cuenta gratuita de Azure](https://azure.microsoft.com/free/).

* Conocimientos básicos sobre [aplicaciones lógicas](../logic-apps/logic-apps-overview.md). Antes de realizar una acción, debe iniciar la aplicación lógica con un desencadenador. Puede usar cualquier desencadenador que desee y agrega otras acciones antes de agregar una acción de retraso. En este tema se usa un desencadenador de Office 365 Outlook. Si está familiarizado con las aplicaciones lógicas, aprenda [cómo crear su primera aplicación lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md).

<a name="add-delay"></a>

## <a name="add-the-delay-action"></a>Agregar la acción de retraso

1. En el Diseñador de aplicación lógica, en el paso donde desea agregar la acción de retraso, elija **nuevo paso**.

   Para agregar la acción de retraso entre los pasos, mueva el puntero sobre la flecha que conecta los pasos. Elija el signo más (+) que aparece y, a continuación, seleccione **agregar una acción**.

1. En el cuadro de búsqueda, escriba "retraso" como filtro. En la lista de acciones, seleccione esta acción: **Retraso**

   ![Agregar acción de "Retraso"](./media/connectors-native-delay/add-delay-action.png)

1. Especifique la cantidad de tiempo de espera antes de ejecutar en la siguiente acción.

   ![Establecer la cantidad de tiempo de retraso](./media/connectors-native-delay/delay-time-intervals.png)

   | Propiedad | Nombre JSON | Obligatorio | Type | DESCRIPCIÓN |
   |----------|-----------|----------|------|-------------|
   | Count | count | Sí | Entero | El número de unidades de tiempo de retraso |
   | Unidad | unit | Sí | String | La unidad de tiempo, por ejemplo: `Second`, `Minute`, `Hour`, `Day`, `Week`, o `Month` |
   ||||||

1. Agregue todas las demás acciones que desea ejecutar en el flujo de trabajo.

1. Cuando haya terminado, guarde la aplicación lógica.

<a name="add-delay-until"></a>

## <a name="add-the-delay-until-action"></a>Agregar el retraso-acción until

1. En el Diseñador de aplicación lógica, en el paso donde desea agregar la acción de retraso, elija **nuevo paso**.

   Para agregar la acción de retraso entre los pasos, mueva el puntero sobre la flecha que conecta los pasos. Elija el signo más (+) que aparece y, a continuación, seleccione **agregar una acción**.

1. En el cuadro de búsqueda, escriba "retraso" como filtro. En la lista de acciones, seleccione esta acción: **Retraso hasta**

   ![Agregar acción "Retraso hasta"](./media/connectors-native-delay/add-delay-until-action.png)

1. Proporciona la fecha de finalización y la hora con la que desea reanudar el flujo de trabajo.

   ![Especifique la marca de tiempo de cuándo se debe finalizar el retraso](./media/connectors-native-delay/delay-until-timestamp.png)

   | Propiedad | Nombre JSON | Obligatorio | Type | DESCRIPCIÓN |
   |----------|-----------|----------|------|-------------|
   | Timestamp |  timestamp | Sí | String | La fecha y hora final para reanudar el flujo de trabajo con este formato: <p>AAAA-MM-ddTHH <p>Por ejemplo, si desea el 18 de septiembre de 2017 a las 2:00 P.M., especifique "2017-09-18T14:00:00Z". <p>**Nota:** Debe seguir este formato de hora la [especificación de tiempo de fecha ISO 8601](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations) en [formato fecha UTC](https://en.wikipedia.org/wiki/Coordinated_Universal_Time), pero sin un [diferencia horaria con UTC](https://en.wikipedia.org/wiki/UTC_offset). Sin una zona horaria, debe agregar la letra "Z" al final sin espacios en blanco. Esta "Z" se refiere al equivalente de [hora náutica](https://en.wikipedia.org/wiki/Nautical_time). |
   ||||||

1. Agregue todas las demás acciones que desea ejecutar en el flujo de trabajo.

1. Cuando haya terminado, guarde la aplicación lógica.

## <a name="next-steps"></a>Pasos siguientes

* [Crear, programar y ejecutar tareas repetitivas y flujos de trabajo con el desencadenador de periodicidad](../connectors/connectors-native-recurrence.md)
* [Conectores de Logic Apps](../connectors/apis-list.md)