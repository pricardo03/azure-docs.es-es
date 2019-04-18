---
title: 'Creación de flujos de trabajo o acciones basados en eventos: Azure Logic Apps | Microsoft Docs'
description: Automatización de flujos de trabajo o acciones basados en eventos mediante webhooks y Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, jehollan, LADocs
ms.assetid: 71775384-6c3a-482c-a484-6624cbe4fcc7
ms.topic: article
tags: connectors
ms.date: 07/21/2016
ms.openlocfilehash: c3047000843e054e71ec1a80313118a25e7c4905
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "58895586"
---
# <a name="create-event-based-workflows-or-actions-by-using-webhooks-and-azure-logic-apps"></a>Creación de flujos de trabajo o acciones basados en eventos mediante webhooks y Azure Logic Apps

Con el desencadenador y la acción de webhook se pueden iniciar, pausar y reanudar flujos para realizar estas tareas:

* Realizar una desencadenación desde un [Centro de eventos de Azure cuando se reciba un elemento](https://github.com/logicappsio/EventHubAPI)
* Esperar una aprobación antes de continuar con un flujo de trabajo

Más información sobre [cómo crear API personalizadas compatibles con webhook](../logic-apps/logic-apps-create-api-app.md).

## <a name="use-the-webhook-trigger"></a>Uso del desencadenador de webhook

Un [ *desencadenador* ](../connectors/apis-list.md) es un evento que inicia un flujo de trabajo de aplicación lógica. El desencadenador de webhook es basado en eventos, que no dependen de sondeo para los nuevos elementos. Cuando se guarda la aplicación lógica con un desencadenador de webhook, o cuando cambie la aplicación lógica de deshabilitado a habilitado, el desencadenador de webhook *se suscribe* para el servicio o extremo especificado mediante el registro de un *deladirecciónURLdedevolucióndellamada* con ese servicio o un punto de conexión. El desencadenador, a continuación, usa esa dirección URL para ejecutar la aplicación lógica según sea necesario. Al igual que el [desencadenador de solicitud](connectors-native-reqres.md), la aplicación lógica se activa inmediatamente cuando se produce el evento esperado. El desencadenador *cancela la suscripción* si quita el desencadenador y guarde la aplicación lógica, o al cambiar la aplicación lógica desde habilitado a deshabilitado.

Este ejemplo muestra cómo configurar un desencadenador HTTP en el Diseñador de aplicación lógica. En este paso se da por hecho que ya ha implementado una API que sigue [el patrón de suscripción y cancelación de suscripción de webhook que se utiliza en las aplicaciones lógicas](../logic-apps/logic-apps-create-api-app.md#webhook-triggers), o bien que ya ha accedido a una API de este tipo. 

**Para agregar el desencadenador de webhook**

1. Como primer paso, agregue el desencadenador de **Webhook HTTP** en una aplicación lógica.
2. Rellene los parámetros de las llamadas de suscripción y cancelación de suscripción de webhook.

   En este paso se sigue el mismo patrón del formato de la [acción HTTP](connectors-native-http.md).

     ![Desencadenador HTTP](./media/connectors-native-webhook/using-trigger.png)

3. Agregue una acción como mínimo.
4. Haga clic en **Guardar** para publicar la aplicación lógica. En este paso se llamará al punto de conexión de suscripción con la URL de devolución de llamada que se necesita para desencadenar la aplicación lógica.
5. Cada vez que el servicio realiza una solicitud `HTTP POST` en la URL de devolución de llamada, la aplicación lógica se activa (e incluye todos los datos transmitidos en la solicitud).

## <a name="use-the-webhook-action"></a>Uso de la acción de webhook

Un [ *acción* ](../connectors/apis-list.md) es una operación que se define y ejecutar el flujo de trabajo de la aplicación lógica. Cuando una aplicación lógica ejecuta una acción de webhook, esa acción *se suscribe* para el servicio o extremo especificado mediante el registro de un *dirección URL de devolución de llamada* con ese servicio o un punto de conexión. La acción de webhook, a continuación, espera hasta que la dirección URL antes de la aplicación lógica se reanuda la ejecución de llamadas de servicio. Elimina la suscripción de la aplicación lógica desde el servicio o punto de conexión en estos casos: 

* Cuando finalice correctamente la acción de webhook
* Si se cancela la ejecución de la aplicación lógica mientras espera una respuesta
* Antes de la lógica de aplicación agota el tiempo

Por ejemplo, el [ **enviar correo electrónico de aprobación** ](connectors-create-api-office365-outlook.md) acción es un ejemplo de acción de webhook que sigue este patrón. Puede extender este patrón a cualquier servicio a través de la acción de webhook. 

Este ejemplo muestra cómo configurar una acción de webhook en el Diseñador de aplicación lógica. En estos pasos se da por hecho que ya ha implementado una API que sigue [el patrón de suscripción y cancelación de suscripción de webhook que se utiliza en las aplicaciones lógicas](../logic-apps/logic-apps-create-api-app.md#webhook-actions), o bien que ya ha accedido a una API de este tipo. 

**Para agregar una acción de webhook**

1. Elija **Nuevo paso** > **Agregar una acción**.

2. En el cuadro de búsqueda, escriba "webhook" para buscar la acción **Webhook HTTP**.

    ![Acción de consulta de selección](./media/connectors-native-webhook/using-action-1.png)

3. Rellene los parámetros de las llamadas de suscripción y cancelación de suscripción de webhook.

   En este paso se sigue el mismo patrón del formato de la [acción HTTP](connectors-native-http.md).

     ![Acción de consulta de finalización](./media/connectors-native-webhook/using-action-2.png)
   
   En tiempo de ejecución, la aplicación lógica llama al punto de conexión de suscripción una vez alcanzado este paso.

4. Haga clic en **Guardar** para publicar la aplicación lógica.

## <a name="technical-details"></a>Detalles técnicos

A continuación encontrará más detalles sobre los desencadenadores y las acciones compatibles con webhook.

## <a name="webhook-triggers"></a>Desencadenadores de webhook

| . | DESCRIPCIÓN |
| --- | --- |
| Webhook HTTP |Suscribe una URL de devolución de llamada a un servicio que puede llamar a la URL para activar la aplicación de lógica según sea necesario. |

### <a name="trigger-details"></a>Detalles del desencadenador

#### <a name="http-webhook"></a>Webhook HTTP

Suscribe una URL de devolución de llamada a un servicio que puede llamar a la URL para activar la aplicación de lógica según sea necesario.
Un asterisco (*) indica un campo obligatorio.

| Display Name (Nombre para mostrar) | Nombre de propiedad | DESCRIPCIÓN |
| --- | --- | --- |
| Método de suscripción* |estático |Método HTTP que va a utilizarse para la solicitud de suscripción. |
| URI de suscripción* |uri |URI HTTP que va a utilizarse para la solicitud de suscripción. |
| Método de cancelación de suscripción* |estático |Método HTTP que va a utilizarse para la solicitud de cancelación de suscripción. |
| URI de cancelación de suscripción* |uri |URI HTTP que va a utilizarse para la solicitud de cancelación de suscripción. |
| Cuerpo de suscripción |body |Cuerpo de la solicitud HTTP para realizar la suscripción. |
| Encabezados de suscripción |encabezados |Encabezados de la solicitud HTTP para realizar la suscripción. |
| Autenticación de suscripción |Autenticación |Autenticación HTTP que se utiliza para realizar la suscripción. Para más información consulte el [conector HTTP](connectors-native-http.md#authentication) |
| Cuerpo de cancelación de suscripción |Cuerpo |Cuerpo de la solicitud HTTP para cancelar la suscripción. |
| Encabezados de cancelación de suscripción |encabezados |Encabezados de la solicitud HTTP para cancelar la suscripción. |
| Autenticación de cancelación de suscripción |authentication |Autenticación HTTP que se utiliza para cancelar la suscripción. Para más información consulte el [conector HTTP](connectors-native-http.md#authentication) |

**Detalles de salida**

Solicitud de webhook

| Nombre de propiedad | Tipo de datos | DESCRIPCIÓN |
| --- | --- | --- |
| encabezados |objeto |Encabezados de la solicitud de webhook |
| Cuerpo |objeto |Objeto de la solicitud de webhook |
| Código de estado |int |Código de estado de la solicitud de webhook |

## <a name="webhook-actions"></a>Acciones de webhook

| . | DESCRIPCIÓN |
| --- | --- |
| Webhook HTTP |Suscribe una URL de devolución de llamada a un servicio que puede llamar a la URL para reanudar un paso de flujo de trabajo según sea necesario. |

### <a name="action-details"></a>Detalles de la acción

#### <a name="http-webhook"></a>Webhook HTTP

Suscribe una URL de devolución de llamada a un servicio que puede llamar a la URL para reanudar un paso de flujo de trabajo según sea necesario.
Un asterisco (*) indica un campo obligatorio.

| Display Name (Nombre para mostrar) | Nombre de propiedad | DESCRIPCIÓN |
| --- | --- | --- |
| Método de suscripción* |estático |Método HTTP que va a utilizarse para la solicitud de suscripción. |
| URI de suscripción* |uri |URI HTTP que va a utilizarse para la solicitud de suscripción. |
| Método de cancelación de suscripción* |estático |Método HTTP que va a utilizarse para la solicitud de cancelación de suscripción. |
| URI de cancelación de suscripción* |uri |URI HTTP que va a utilizarse para la solicitud de cancelación de suscripción. |
| Cuerpo de suscripción |body |Cuerpo de la solicitud HTTP para realizar la suscripción. |
| Encabezados de suscripción |encabezados |Encabezados de la solicitud HTTP para realizar la suscripción. |
| Autenticación de suscripción |Autenticación |Autenticación HTTP que se utiliza para realizar la suscripción. Para más información consulte el [conector HTTP](connectors-native-http.md#authentication) |
| Cuerpo de cancelación de suscripción |Cuerpo |Cuerpo de la solicitud HTTP para cancelar la suscripción. |
| Encabezados de cancelación de suscripción |encabezados |Encabezados de la solicitud HTTP para cancelar la suscripción. |
| Autenticación de cancelación de suscripción |authentication |Autenticación HTTP que se utiliza para cancelar la suscripción. Para más información consulte el [conector HTTP](connectors-native-http.md#authentication) |

**Detalles de salida**

Solicitud de webhook

| Nombre de propiedad | Tipo de datos | DESCRIPCIÓN |
| --- | --- | --- |
| encabezados |objeto |Encabezados de la solicitud de webhook |
| Cuerpo |objeto |Objeto de la solicitud de webhook |
| Código de estado |int |Código de estado de la solicitud de webhook |

## <a name="next-steps"></a>Pasos siguientes

* [Creación de una aplicación lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md)
* [Buscar otros conectores](apis-list.md)
