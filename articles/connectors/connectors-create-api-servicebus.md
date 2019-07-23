---
title: 'Envío y recepción de mensajes con Azure Service Bus: Azure Logic Apps | Microsoft Docs'
description: Configuración de la mensajería empresarial en la nube con Azure Service Bus en Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.assetid: d6d14f5f-2126-4e33-808e-41de08e6721f
ms.topic: article
tags: connectors
ms.date: 08/25/2018
ms.openlocfilehash: 68378c87e18df874059579445352b8fd1b2b6c13
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "62105587"
---
# <a name="exchange-messages-in-the-cloud-with-azure-service-bus-and-azure-logic-apps"></a>Intercambio de mensajes en la nube con Azure Service Bus y Azure Logic Apps

Con Azure Logic Apps y el conector de Azure Service Bus puede crear tareas automatizadas y flujos de trabajo de transferencia de datos, por ejemplo, ventas y pedidos de compra, diarios y movimientos del inventario, en todas las aplicaciones de la organización. El conector no solo supervisa, envía y administra los mensajes, sino que además realiza acciones con las colas, las sesiones, los temas, las suscripciones, etc., por ejemplo:

* Supervisa los mensajes cuando llegan (autocompletar) o se reciben (bloqueo de información) en las colas, los temas y las suscripciones a los temas. 
* Envía mensajes.
* Crea y elimina suscripciones a temas.
* Administra los mensajes de las colas y las suscripciones a temas, por ejemplo, realiza acciones como obtener, obtener los aplazados, completar, aplazar, abandonar y administrar los mensajes fallidos.
* Renueva los bloqueos de mensajes y sesiones en las colas y suscripciones a temas.
* Cierra sesiones en las colas y los temas.

Puede usar desencadenadores que obtengan respuestas de Service Bus y hagan que la salida esté disponible para otras acciones en las aplicaciones lógicas. También puede hacer que otras acciones usen la salida de las de Service Bus. Si no está familiarizado con Service Bus y Logic Apps, revise [Qué es Azure Service Bus](../service-bus-messaging/service-bus-messaging-overview.md) y [¿Qué es Azure Logic Apps?](../logic-apps/logic-apps-overview.md)

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción de Azure. Si no tiene una suscripción de Azure, <a href="https://azure.microsoft.com/free/" target="_blank">regístrese para obtener una cuenta gratuita de Azure</a>. 

* Un espacio de nombres de Service Bus y una entidad de mensajería, como una cola. Si no tiene estos elementos, aprenda a [crear el espacio de nombres de Service Bus y una cola](../service-bus-messaging/service-bus-create-namespace-portal.md). 

  Estos elementos deben existir en la misma suscripción de Azure que las aplicaciones lógicas que los van a usar.

* Conocimientos básicos acerca de [cómo crear aplicaciones lógicas](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* La aplicación lógica con la que desea usar Service Bus. La aplicación lógica debe existir en la misma suscripción de Azure que la instancia de Service Bus. Para comenzar con un desencadenador de Service Bus, [cree una aplicación lógica en blanco](../logic-apps/quickstart-create-first-logic-app-workflow.md). Para usar una acción de Service Bus, inicie la aplicación lógica con otro desencadenador, por ejemplo, el de **periodicidad**.

<a name="permissions-connection-string"></a>

## <a name="check-permissions"></a>Comprobación de los permisos

Confirme que la aplicación lógica tiene permiso para acceder al espacio de nombres de Service Bus. 

1. Inicie sesión en el [Azure Portal](https://portal.azure.com). 

2. Vaya al *espacio de nombres* de Service Bus. En la página del espacio de nombres, en **Configuración**, seleccione **Directivas de acceso compartido**. En **Notificaciones**, compruebe que tenga permisos de **Administrador** para ese espacio de nombres.

   ![Administración de permisos para un espacio de nombres de Service Bus](./media/connectors-create-api-azure-service-bus/azure-service-bus-namespace.png)

3. Obtenga la cadena de conexión para el espacio de nombres de Service Bus. La necesita al escribir la información de conexión en la aplicación lógica.

   1. Seleccione **RootManageSharedAccessKey**. 
   
   1. Al lado de la cadena de conexión principal, elija el botón de copia. Guarde la cadena de conexión para usarla más adelante.

      ![Copia de la cadena de conexión del espacio de nombres de Service Bus](./media/connectors-create-api-azure-service-bus/find-service-bus-connection-string.png)

   > [!TIP]
   > Para confirmar si la cadena de conexión está asociada al espacio de nombres de Service Bus o a una entidad de mensajería, como una cola, compruebe la cadena de conexión del parámetro `EntityPath` . Si encuentra este parámetro, la cadena de conexión es para una entidad específica y no es la correcta para la aplicación lógica.

## <a name="add-trigger-or-action"></a>Incorporación de un desencadenador o una acción

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y abra la aplicación lógica en el diseñador de aplicaciones lógicas, si aún no lo ha hecho.

1. Para agregar un *desencadenador* a una aplicación lógica en blanco, en el cuadro de búsqueda, escriba "Azure Service Bus" como filtro. En la lista de desencadenadores, seleccione el que desee. 

   Por ejemplo, para desencadenar la aplicación lógica cuando un nuevo elemento se envía a una cola de Service Bus, seleccione este desencadenador: **Cuando se recibe un mensaje en una cola (autocompletar)**

   ![Selección de un desencadenador de Service Bus](./media/connectors-create-api-azure-service-bus/select-service-bus-trigger.png)

   > [!NOTE]
   > Algunos desencadenadores devuelven uno o varios mensajes, como **Cuando llegan uno o más mensajes a una cola (autocompletar)** . Cuando se activan estos desencadenadores, devuelven un valor comprendido entre 1 y el número de mensajes especificados por la propiedad **Recuento máximo de mensajes** del desencadenador.

   *Todos los desencadenadores de Service Bus son de sondeo largo*, lo que significa que, cuando se activa un desencadenador, este procesa todos los mensajes y espera 30 segundos a que aparezcan más en la suscripción al tema o la cola. 
   Si no es el caso, se omite la ejecución del desencadenador. 
   De lo contrario, el desencadenador sigue leyendo mensajes hasta que la suscripción del tema o de la cola se queda vacía. El siguiente sondeo del desencadenador se basa en el intervalo de periodicidad especificado en las propiedades del desencadenador.

1. Para agregar una *acción* a una aplicación lógica existente, siga estos pasos: 

   1. En el último paso para agregar una acción, elija **Nuevo paso**. 

      Para agregar una acción entre un paso y otro, mueva el puntero sobre la flecha entre ellos. 
      Elija el signo más ( **+** ) que aparece y seleccione **Agregar una acción**.

   1. En el cuadro de búsqueda, escriba "Azure Service Bus" como filtro. 
   En la lista de acciones, seleccione la que desee. 
 
      Por ejemplo, seleccione esta acción: **Enviar mensaje**

      ![Selección de una acción de Service Bus](./media/connectors-create-api-azure-service-bus/select-service-bus-send-message-action.png) 

1. Si se va a conectar la aplicación lógica al espacio de nombres de Service Bus por primera vez, el diseñador de aplicaciones lógicas solicitará la información de conexión en este momento. 

   1. Proporcione un nombre para la conexión y seleccione el espacio de nombres de Service Bus.

      ![Creación de la conexión de Service Bus, parte 1](./media/connectors-create-api-azure-service-bus/create-service-bus-connection-1.png)

      Si prefiere en cambio escribir manualmente la cadena de conexión, elija **Especificar la información de conexión manualmente**. 
      Si no tiene la cadena de conexión, aprenda a [buscar la cadena de conexión](#permissions-connection-string).

   1. Ahora seleccione la directiva de Service Bus y elija **Crear**.

      ![Creación de una conexión de Service Bus, parte 2](./media/connectors-create-api-azure-service-bus/create-service-bus-connection-2.png)

1. En este ejemplo, seleccione la entidad de mensajería que desee, como una cola o un tema. En este ejemplo, seleccione la cola de Service Bus. 
   
   ![Selección de la cola de Service Bus](./media/connectors-create-api-azure-service-bus/service-bus-select-queue.png)

1. Proporcione la información necesaria para el desencadenador o la acción. En este ejemplo, siga los pasos pertinentes para el desencadenador o la acción: 

   * **Para el desencadenador de ejemplo**: establezca el intervalo de sondeo y la frecuencia de comprobación de la cola.

     ![Configuración del intervalo de sondeo](./media/connectors-create-api-azure-service-bus/service-bus-trigger-details.png)

     Cuando haya terminado, continúe la creación del flujo de trabajo de la aplicación lógica mediante la incorporación de las acciones que desee. Por ejemplo, puede agregar una acción que envíe un correo electrónico cuando llegue un mensaje nuevo.
     Cuando el desencadenador comprueba la cola y encuentra un nuevo mensaje, la aplicación lógica ejecuta las acciones seleccionadas para él.

   * **Para la acción de ejemplo**: escriba el contenido del mensaje y otros detalles. 

     ![Indicación del contenido del mensaje y los detalles](./media/connectors-create-api-azure-service-bus/service-bus-send-message-details.png)

     Cuando haya terminado, continúe la creación del flujo de trabajo de la aplicación lógica mediante la incorporación cualquier otra acción que desee. Por ejemplo, puede agregar una acción que envíe un correo electrónico de confirmación de envío del mensaje.

1. Guarde la aplicación lógica. En la barra de herramientas del diseñador, haga clic en **Guardar**.

## <a name="connector-reference"></a>Referencia de conectores

Para obtener detalles técnicos acerca de desencadenadores, acciones y límites, que se describen en la descripción de OpenAPI (antes Swagger) del conector, consulte la [página de referencia](/connectors/servicebus/) del conector.

## <a name="get-support"></a>Obtención de soporte técnico

* Si tiene alguna duda, visite el [foro de Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Para enviar ideas sobre características o votar sobre ellas, visite el [sitio de comentarios de los usuarios de Logic Apps](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre otros [conectores de Logic Apps](../connectors/apis-list.md)