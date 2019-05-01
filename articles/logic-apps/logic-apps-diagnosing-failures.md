---
title: 'Solución de problemas y diagnóstico de errores: Azure Logic Apps | Microsoft Docs'
description: Más información sobre cómo solucionar problemas y diagnosticar errores de flujo de trabajo en Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, jehollan, LADocs
ms.topic: article
ms.assetid: a6727ebd-39bd-4298-9e68-2ae98738576e
ms.date: 10/15/2017
ms.openlocfilehash: 62a74364939fffb6e06f51f1c0cabb6cce8c10e1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60999807"
---
# <a name="troubleshoot-and-diagnose-workflow-failures-in-azure-logic-apps"></a>Solución de problemas y diagnóstico de errores de flujo de trabajo en Azure Logic Apps

La aplicación lógica genera información que puede ayudarle a diagnosticar y depurar los problemas de la aplicación. Puede diagnosticar una aplicación lógica revisando cada paso del flujo de trabajo a través de Azure Portal. O bien, puede agregar algunos pasos a un flujo de trabajo para la depuración en tiempo de ejecución.

## <a name="review-trigger-history"></a>Revisión del historial de desencadenadores

Cada aplicación lógica se inicia con un desencadenador. Si no se activa el desencadenador, compruebe primero el historial de desencadenadores. Este historial muestra todos los intentos del desencadenador realizados por la aplicación lógica y describe las entradas y salidas de cada intento del desencadenador.

1. Para comprobar si se activa el desencadenador, en el menú de aplicación lógica, elija **Introducción**. En **Historial de desencadenadores**, seleccione el evento del desencadenador.

   > [!TIP]
   > Si no ve el menú de la aplicación lógica, intente volver al panel de Azure y vuelva a abrir la aplicación lógica.

   ![Revisión del historial de desencadenadores](./media/logic-apps-diagnosing-failures/logic-app-trigger-history-overview.png)

   > [!TIP]
   > * Si no encuentra los datos previstos, elija **Actualizar** en la barra de herramientas.
   > * Si la lista muestra muchos intentos del desencadenador, y no se puede encontrar la entrada deseada, pruebe a filtrar la lista.

   Estos son los estados posibles de un intento de desencadenador:

   | Status | DESCRIPCIÓN | 
   | ------ | ----------- | 
   | **Correcto** | El desencadenador comprueba el punto de conexión y encuentra datos disponibles. Por lo general, un estado "activado" también aparece junto a este estado. Si no es así, la definición del desencadenador puede tener una condición o un comando `SplitOn` que no se ha cumplido. <p>Este estado puede aplicarse a un desencadenador manual, uno de periodicidad o uno de sondeo. Un desencadenador se puede ejecutar correctamente, pero la misma ejecución podría fallar cuando las acciones generan errores no controlados. | 
   | **Omitido** | El desencadenador comprueba el punto de conexión, pero no ha encontrado datos. | 
   | **Erróneo** | Se produjo un error. Para revisar los mensajes de error generado para un desencadenador con error, seleccione el intento de desencadenador y elija **Salidas**. Por ejemplo, podría encontrar entradas que no son válidas. | 
   ||| 

   Es posible que tenga varias entradas de desencadenador con la misma fecha y hora, lo que sucede cuando la aplicación lógica busca varios elementos. 
   Cada vez que el desencadenador se activa, el motor de Logic Apps crea una instancia de aplicación lógica para ejecutar el flujo de trabajo. De forma predeterminada, cada instancia se ejecuta en paralelo para que ningún flujo de trabajo tenga que esperar antes de iniciar una ejecución.

   > [!TIP]
   > Puede volver a comprobar el desencadenador sin esperar a la siguiente repetición. En la barra de herramientas de introducción, elija **Ejecutar desencadenador** y seleccione el desencadenador, lo que fuerza una comprobación. O bien, seleccione **Ejecutar** en la barra de herramientas del Diseñador de aplicaciones lógicas.

3. Para examinar los detalles de un intento de desencadenador, en **Historial de desencadenadores**, seleccione dicho intento del desencadenador. 

   ![Selección de un intento de desencadenador](./media/logic-apps-diagnosing-failures/logic-app-trigger-history.png)

4. Revise las entradas y cualquier salida generada por el desencadenador. Las salidas del desencadenador muestran los datos que se han recibido del desencadenador. Estas salidas pueden ayudarle a determinar si se devolvieron todas las propiedades según lo previsto.

   > [!NOTE]
   > Si encuentra cualquier contenido que no conozca, aprenda cómo Azure Logic Apps [administra distintos tipos de contenido](../logic-apps/logic-apps-content-type.md).

   ![Salidas del desencadenador](./media/logic-apps-diagnosing-failures/trigger-outputs.png)

## <a name="review-run-history"></a>Revisar el historial de ejecución.

Cada desencadenador activado inicia una ejecución de flujo de trabajo. Puede revisar lo que ha ocurrido durante la ejecución, incluido el estado de cada paso en el flujo de trabajo, además de las entradas y salidas de cada paso.

1. En el menú de la aplicación lógica, elija **Introducción**. En **Historial de ejecuciones**, revise la ejecución del desencadenador activado.

   > [!TIP]
   > Si no ve el menú de la aplicación lógica, intente volver al panel de Azure y vuelva a abrir la aplicación lógica.

   ![Revisión del historial de ejecuciones](./media/logic-apps-diagnosing-failures/logic-app-runs-history-overview.png)

   > [!TIP]
   > * Si no encuentra los datos previstos, elija **Actualizar** en la barra de herramientas.
   > * Si la lista muestra varias ejecuciones, y no se puede encontrar la entrada deseada, pruebe a filtrar la lista.

   Estos son los estados posibles de una ejecución:

   | Status | DESCRIPCIÓN | 
   | ------ | ----------- | 
   | **Correcto** | Todas las acciones correctas. <p>Si se han producido errores en una acción específica, la acción siguiente en el flujo de trabajo es la que controla ese error. | 
   | **Erróneo** | Error en al menos una acción y ninguna de las acciones posteriores en el flujo de trabajo se ha configurado para controlar el error. | 
   | **Cancelado** | El flujo de trabajo se estaba ejecutando pero recibió una solicitud de cancelación. | 
   | **Ejecución** | El flujo de trabajo se ejecuta actualmente. <p>Este estado puede ocurrir en flujos de trabajo con limitaciones o a causa del plan de precios actual. Para más información, consulte [los límites de acción en la página de precios](https://azure.microsoft.com/pricing/details/logic-apps/). Si se configura un [registro de diagnósticos](../logic-apps/logic-apps-monitor-your-logic-apps.md), también se puede obtener información acerca de los eventos de limitación que se producen. | 
   ||| 

2. Revise los detalles de cada paso de una ejecución específica. En **Historial de ejecuciones**, seleccione la ejecución que desea examinar.

   ![Revisión del historial de ejecuciones](./media/logic-apps-diagnosing-failures/logic-app-run-history.png)

   Si la ejecución se realizó correctamente o produjo un error, la vista de detalles de la ejecución muestra cada paso y si se ha realizado correctamente o no.

   ![Visualización de los detalles de una ejecución de aplicación lógica](./media/logic-apps-diagnosing-failures/logic-app-run-details.png)

3. Para examinar las entradas, salidas y los mensajes de error de un paso específico, seleccione ese paso para que la forma se expanda y muestre los detalles. Por ejemplo: 

   ![Visualización de los detalles del paso](./media/logic-apps-diagnosing-failures/logic-app-run-details-expanded.png)

## <a name="perform-runtime-debugging"></a>Ejecución de la depuración en tiempo de ejecución

Para ayudar con la depuración, puede agregar pasos de diagnóstico a un flujo de trabajo, además de revisar el historial de desencadenadores y ejecuciones. Por ejemplo, puede agregar pasos que utilicen el servicio [Webhook Tester](https://webhook.site/) para poder inspeccionar las solicitudes HTTP y determinar su tamaño, forma y formato exactos.

1. Visite [Webhook Tester](https://webhook.site/) y copie la dirección URL única creada

2. En la aplicación lógica, añada una acción HTTP POST junto con el contenido del cuerpo que desee probar, como una expresión, la salida de otro paso.

3. Pegue la dirección URL de Webhook Tester en la acción HTTP POST.

4. Para examinar cómo se forma una solicitud cuando se genera desde el motor de Logic Apps, ejecute la aplicación lógica y consulte Webhook Tester para más información.

## <a name="next-steps"></a>Pasos siguientes

[Supervisión de la aplicación lógica](../logic-apps/logic-apps-monitor-your-logic-apps.md)
