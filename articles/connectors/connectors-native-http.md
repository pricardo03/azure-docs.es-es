---
title: Conexión a cualquier punto de conexión HTTP con Azure Logic Apps | Microsoft Docs
description: Automatice tareas y flujos de trabajo que se comunican con cualquier punto de conexión HTTP mediante el uso de Azure Logic Apps.
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.assetid: e11c6b4d-65a5-4d2d-8e13-38150db09c0b
ms.topic: article
tags: connectors
ms.date: 08/25/2018
ms.openlocfilehash: 95a039c990dc9b6c852a69518f0a71c51a4747b7
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2018
ms.locfileid: "44302629"
---
# <a name="call-http-or-https-endpoints-with-azure-logic-apps"></a>Llamada a puntos de conexión HTTP o HTTPS con Azure Logic Apps

Con Azure Logic Apps y el conector de Protocolo de transferencia de hipertexto (HTTP), puede automatizar los flujos de trabajo que se comunican con cualquier punto de conexión HTTP o HTTPS mediante la creación de aplicaciones lógicas. Por ejemplo, puede supervisar el punto de conexión de servicio para su sitio web. Cuando se produce un evento en ese punto de conexión, por ejemplo, que su sitio web deje de funcionar, el evento desencadena el flujo de trabajo de la aplicación lógica y ejecuta las acciones especificadas. 

Puede usar el desencadenador HTTP como primer paso en el flujo de trabajo para comprobar o *sondear* un punto de conexión según una programación periódica. En cada comprobación, el desencadenador envía una llamada o *solicitud* al punto de conexión. La respuesta del punto de conexión determina si el flujo de trabajo de la aplicación lógica se ejecuta. El desencadenador pasa a lo largo de todo el contenido desde la respuesta hasta las acciones en la aplicación lógica. 

Puede usar la acción HTTP como cualquier otro paso del flujo de trabajo para llamar al extremo cuando desee. La respuesta del punto de conexión determina cómo se ejecutan las acciones restantes de su flujo de trabajo.

Si no está familiarizado con las aplicaciones lógicas, consulte [¿Qué es Azure Logic Apps?](../logic-apps/logic-apps-overview.md)

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción de Azure. Si no tiene una suscripción de Azure, <a href="https://azure.microsoft.com/free/" target="_blank">regístrese para obtener una cuenta gratuita de Azure</a>. 

* La dirección URL del punto de conexión de destino al que desea llamar 

* Conocimientos básicos acerca de [cómo crear aplicaciones lógicas](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* La aplicación lógica desde donde desea llamar al extremo de destino Para comenzar con el desencadenador HTTP, [cree una aplicación lógica en blanco](../logic-apps/quickstart-create-first-logic-app-workflow.md). Para usar la acción HTTP, inicie la aplicación lógica con un desencadenador.

## <a name="add-http-trigger"></a>Adición de un desencadenador HTTP

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y abra la aplicación lógica en blanco en el diseñador de aplicaciones lógicas si aún no lo ha hecho.

1. En el cuadro de búsqueda, escriba "http" como filtro. En la lista de desencadenadores, seleccione el desencadenador **HTTP**. 

   ![Selección del desencadenador HTTP](./media/connectors-native-http/select-http-trigger.png)

1. Proporcione [los parámetros y valores del desencadenador HTTP](../logic-apps/logic-apps-workflow-actions-triggers.md##http-trigger) que desea incluir en la llamada al punto de conexión de destino. Establezca la periodicidad con la que desea que el desencadenador compruebe el punto de conexión de destino.

   ![Introducción de los parámetros de desencadenador HTTP](./media/connectors-native-http/http-trigger-parameters.png)

   Para obtener más información sobre el desencadenador HTTP, los parámetros y los valores, consulte [Referencia sobre los tipos de desencadenador y de acción](../logic-apps/logic-apps-workflow-actions-triggers.md##http-trigger).

1. Continúe creando el flujo de trabajo de la aplicación lógica con acciones que se ejecuten cuando se activa el desencadenador.

## <a name="add-http-action"></a>Adición de acción HTTP

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y abra la aplicación lógica en el diseñador de aplicaciones lógicas si aún no lo ha hecho.

1. En el último paso para agregar una acción HTTP, elija **Nuevo paso**. 

   En este ejemplo, la aplicación lógica se inicia con el desencadenador HTTP como primer paso.

1. En el cuadro de búsqueda, escriba "http" como filtro. En la lista de acciones, seleccione la acción **HTTP**.

   ![Selección de la acción de HTTP](./media/connectors-native-http/select-http-action.png)

   Para agregar una acción entre un paso y otro, mueva el puntero sobre la flecha entre ellos. 
   Elija el signo más (**+**) que aparece y seleccione **Agregar una acción**.

1. Proporcione [los parámetros y valores de la acción HTTP](../logic-apps/logic-apps-workflow-actions-triggers.md##http-action) que desea incluir en la llamada al punto de conexión de destino. 

   ![Introducción de los parámetros de acción de HTTP](./media/connectors-native-http/http-action-parameters.png)

1. Cuando haya terminado, asegúrese de guardar la aplicación lógica. En la barra de herramientas del diseñador, haga clic en **Guardar**. 

## <a name="authentication"></a>Autenticación

Para configurar la autenticación, seleccione **Mostrar opciones avanzadas** dentro de la acción o desencadenador. Para obtener más información acerca de los tipos de autenticación disponibles para las acciones y desencadenadores HTTP, consulte [Referencia sobre los tipos de desencadenador y de acción](../logic-apps/logic-apps-workflow-actions-triggers.md#connector-authentication).

## <a name="get-support"></a>Obtención de soporte técnico

* Si tiene alguna duda, visite el [foro de Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Para enviar ideas sobre características o votar sobre ellas, visite el [sitio de comentarios de los usuarios de Logic Apps](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre otros [conectores de Logic Apps](../connectors/apis-list.md)
