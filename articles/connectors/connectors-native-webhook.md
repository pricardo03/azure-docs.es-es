---
title: Espera y respuesta a eventos
description: Automatización de flujos de trabajo que se desencadenan, pausan y reanudan en función de eventos en un punto de conexión de servicio mediante el uso de Azure Logic Apps
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 10/10/2019
tags: connectors
ms.openlocfilehash: 7ff411ae082acfe2d465ab9d3371982b0693c226
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2019
ms.locfileid: "74787053"
---
# <a name="create-and-run-automated-event-based-workflows-by-using-http-webhooks-in-azure-logic-apps"></a>Creación y ejecución de flujos de trabajo basados en eventos automatizados mediante webhooks HTTP en Azure Logic Apps

Con [Azure Logic Apps](../logic-apps/logic-apps-overview.md) y el conector de webhook HTTP integrado, puede automatizar los flujos de trabajo que esperan y se ejecutan basados en eventos específicos que se producen en un punto de conexión HTTP o HTTPS mediante la compilación de aplicaciones lógicas. Por ejemplo, puede crear una aplicación lógica que supervisa un punto de conexión de servicio al esperar un evento específico antes de desencadenar el flujo de trabajo y ejecutar las acciones especificadas, en lugar de comprobar con regularidad o *sondear* ese punto de conexión.

Estos son algunos ejemplos de flujos de trabajo basados en eventos:

* Espere a que llegue un elemento de un [Centro de eventos de Azure](https://github.com/logicappsio/EventHubAPI) antes de desencadenar una ejecución de la aplicación lógica.
* Espere a una aprobación antes de continuar con un flujo de trabajo.

## <a name="how-do-webhooks-work"></a>¿Cómo funcionan los webhooks?

Un desencadenador de webhook HTTP se basa en eventos, que no dependen de la comprobación o sondeo con regularidad de nuevos elementos. Cuando se guarda una aplicación lógica que empieza con un desencadenador de webhook, o cuando se cambia la aplicación lógica de deshabilitada a habilitada, el desencadenador de webhook *se suscribe* a un servicio o punto de conexión específico mediante el registro de una *dirección URL de devolución de llamada* con ese servicio o punto de conexión. El desencadenador, luego, espera a que ese servicio o punto de conexión llame a la dirección URL, que empieza a ejecutar la aplicación lógica. Al igual que el [desencadenador de solicitud](connectors-native-reqres.md), la aplicación lógica se activa inmediatamente cuando se produce el evento especificado. El desencadenador *cancela la suscripción* del servicio o punto de conexión si se quita y guarda la aplicación lógica, o al cambiar la aplicación lógica de habilitada a deshabilitada.

Una acción de webhook HTTP también está basada en eventos y *se suscribe* a un servicio o punto de conexión específico mediante el registro de una *dirección URL de devolución de llamada* con ese servicio o punto de conexión. La acción de webhook pausa el flujo de trabajo de la aplicación lógica y espera hasta que el servicio o punto de conexión llama a la dirección URL antes de que la aplicación lógica reanude la ejecución. La aplicación lógica de acción *cancela la suscripción* del servicio o punto de conexión en estos casos:

* Cuando finaliza correctamente la acción de webhook
* Si se cancela la ejecución de la aplicación lógica mientras espera una respuesta
* Antes de que la aplicación lógica agote el tiempo de espera

Por ejemplo, la acción [**Enviar correo de aprobación**](connectors-create-api-office365-outlook.md) del conector de Office 365 Outlook es un ejemplo de acción de webhook que sigue este patrón. Puede ampliar este patrón a cualquier servicio mediante el uso de la acción de webhook.

> [!NOTE]
> Logic Apps aplica la seguridad de la capa de transporte (TLS) 1.2 al recibir la llamada de vuelta al desencadenador o acción de webhook HTTP. Si ve errores de protocolo de enlace SSL, asegúrese de usar TLS 1.2.

Para más información, consulte los temas siguientes:

* [Parámetros de desencadenador de webhook HTTP](../logic-apps/logic-apps-workflow-actions-triggers.md#http-webhook-trigger)
* [Webhooks y suscripciones](../logic-apps/logic-apps-workflow-actions-triggers.md#webhooks-and-subscriptions)
* [Creación de API personalizadas compatibles con webhook](../logic-apps/logic-apps-create-api-app.md)

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción de Azure. Si no tiene una suscripción de Azure, [regístrese para obtener una cuenta gratuita de Azure](https://azure.microsoft.com/free/).

* La dirección URL para una API o un punto de conexión ya implementado que admita el patrón de suscripción y cancelación de suscripción de webhook para [desencadenadores de webhook en aplicaciones lógicas](../logic-apps/logic-apps-create-api-app.md#webhook-triggers) o [acciones de webhook en aplicaciones lógicas](../logic-apps/logic-apps-create-api-app.md#webhook-actions), según corresponda.

* Conocimientos básicos acerca de [cómo crear aplicaciones lógicas](../logic-apps/quickstart-create-first-logic-app-workflow.md). Si no está familiarizado con las aplicaciones lógicas, consulte [¿Qué es Azure Logic Apps?](../logic-apps/logic-apps-overview.md)

* La aplicación lógica en la que quiera esperar los eventos específicos en el punto de conexión de destino. Para comenzar con un desencadenador de webhook HTTP, [cree una aplicación lógica en blanco](../logic-apps/quickstart-create-first-logic-app-workflow.md). Para usar la acción de webhook HTTP, inicie la aplicación lógica con el desencadenador que quiera. En este ejemplo se usa el desencadenador HTTP como primer paso.

## <a name="add-an-http-webhook-trigger"></a>Adición de un desencadenador de webhook HTTP

Este desencadenador integrado registra una dirección URL de devolución de llamada con el servicio especificado y espera a que ese servicio envíe una solicitud HTTP POST a esa dirección URL. Cuando se produce este evento, el desencadenador se activa y ejecuta inmediatamente la aplicación lógica.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com). Abra la aplicación lógica en blanco en el Diseñador de aplicaciones lógicas.

1. En el diseñador, en el cuadro de búsqueda, escriba "webhook http" como filtro. En la lista de **desencadenadores**, seleccione el desencadenador **webhook HTTP**.

   ![Selección de un desencadenador de webhook HTTP](./media/connectors-native-webhook/select-http-webhook-trigger.png)

   En este ejemplo se cambia el nombre del desencadenador a "desencadenador de webhook HTTP" para que el paso tenga un nombre más descriptivo. Además, más adelante, en el ejemplo se agrega una acción de webhook HTTP, y ambos nombres deben ser únicos.

1. Proporcione los valores para los [parámetros del desencadenador de webhook HTTP](../logic-apps/logic-apps-workflow-actions-triggers.md#http-webhook-trigger) que quiera usar para las llamadas de suscripción y cancelación de suscripción, por ejemplo:

   ![Introducción de parámetros del desencadenador de webhook HTTP](./media/connectors-native-webhook/http-webhook-trigger-parameters.png)

1. Para agregar otros parámetros disponibles, abra la lista **Agregar nuevo parámetro** y seleccione los parámetros que quiera.

   Para obtener más información sobre los tipos de autenticación disponibles para webhook de HTTP, consulte [Incorporación de la autenticación en las llamadas salientes](../logic-apps/logic-apps-securing-a-logic-app.md#add-authentication-outbound).

1. Continúe creando el flujo de trabajo de la aplicación lógica con acciones que se ejecuten cuando se activa el desencadenador.

1. Cuando haya finalizado, recuerde guardar la aplicación lógica. En la barra de herramientas del diseñador, seleccione **Guardar**.

   Al guardar la aplicación lógica, esta llama al punto de conexión de suscripción y registra la dirección URL de devolución de llamada para desencadenar esta aplicación lógica.

1. Ahora, cada vez que el servicio de destino envía una solicitud `HTTP POST` a la dirección URL de devolución de llamada, se activa la aplicación de lógica e incluye los datos que se pasan a través de la solicitud.

## <a name="add-an-http-webhook-action"></a>Adición de una acción de webhook HTTP

Esta acción integrada registra una dirección URL de devolución de llamada con el servicio especificado, pausa el flujo de trabajo de la aplicación lógica y espera a que ese servicio envíe una solicitud HTTP POST a esa dirección URL. Cuando se produce este evento, la acción reanuda la ejecución de la aplicación lógica.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com). Abra la aplicación lógica en el Diseñador de aplicaciones lógicas.

   En este ejemplo se usa el desencadenador de webhook HTTP como primer paso.

1. En el paso en el que quiera agregar la acción de webhook HTTP, seleccione **Nuevo paso**.

   Para agregar una acción entre un paso y otro, mueva el puntero sobre la flecha entre ellos. Seleccione el signo más ( **+** ) que aparece y, luego, seleccione **Agregar una acción**.

1. En el diseñador, en el cuadro de búsqueda, escriba "webhook http" como filtro. En la lista **Acciones**, seleccione la acción **webhook HTTP**.

   ![Selección de una acción de webhook HTTP](./media/connectors-native-webhook/select-http-webhook-action.png)

   En este ejemplo se cambia el nombre de la acción a "acción de webhook HTTP" para que el paso tenga un nombre más descriptivo.

1. Proporcione los valores para los parámetros de la acción de webhook HTTP, que son parecidos a los [parámetros del desencadenador de webhook HTTP](../logic-apps/logic-apps-workflow-actions-triggers.md##http-webhook-trigger), que quiera usar para las llamadas de suscripción y cancelación de suscripción, por ejemplo:

   ![Introducción de los parámetros de acción de webhook HTTP](./media/connectors-native-webhook/http-webhook-action-parameters.png)

   Durante el tiempo de ejecución, la aplicación lógica llama al punto de conexión de suscripción cuando se ejecuta esta acción. Luego la aplicación lógica pausa el flujo de trabajo y espera a que el servicio de destino envíe una solicitud `HTTP POST` a la dirección URL de devolución de llamada. Si la acción se completa correctamente, la acción cancela la suscripción desde el punto de conexión y la aplicación lógica reanuda la ejecución del flujo de trabajo.

1. Para agregar otros parámetros disponibles, abra la lista **Agregar nuevo parámetro** y seleccione los parámetros que quiera.

   Para obtener más información sobre los tipos de autenticación disponibles para webhook de HTTP, consulte [Incorporación de la autenticación en las llamadas salientes](../logic-apps/logic-apps-securing-a-logic-app.md#add-authentication-outbound).

1. Cuando haya finalizado, recuerde guardar la aplicación lógica. En la barra de herramientas del diseñador, seleccione **Guardar**.

## <a name="connector-reference"></a>Referencia de conectores

Para obtener más información acerca de los parámetros del desencadenador y la acción, que son parecidos entre sí, consulte [Parámetros del webhook HTTP](../logic-apps/logic-apps-workflow-actions-triggers.md##http-webhook-trigger).

### <a name="output-details"></a>Detalles de salida

Aquí tiene más información acerca de las salidas de un desencadenador o acción de webhook HTTP, que devuelve esta información:

| Nombre de propiedad | type | DESCRIPCIÓN |
|---------------|------|-------------|
| headers | object | Encabezados de la solicitud |
| body | object | Objeto JSON | Objeto con el contenido del cuerpo de la solicitud |
| status code | int | Código de estado de la solicitud |
|||

| status code | DESCRIPCIÓN |
|-------------|-------------|
| 200 | OK |
| 202 | Accepted |
| 400 | Solicitud incorrecta |
| 401 | No autorizado |
| 403 | Prohibido |
| 404 | No encontrado |
| 500 | Error interno del servidor Error desconocido. |
|||

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre otros [conectores de Logic Apps](../connectors/apis-list.md)
