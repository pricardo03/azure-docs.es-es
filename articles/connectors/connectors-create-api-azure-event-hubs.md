---
title: Conectarse a Azure Event Hubs - Azure Logic Apps
description: Administración y supervisión de eventos con Azure Event Hubs y Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 05/21/2018
tags: connectors
ms.openlocfilehash: a59f21478f85f238d91c01faed44d8e49cb15f0a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60313534"
---
# <a name="monitor-receive-and-send-events-with-azure-event-hubs-and-azure-logic-apps"></a>Supervisión, recepción y envío de eventos con Azure Event Hubs y Azure Logic Apps

En este artículo se muestra cómo puede supervisar y administrar eventos enviados a [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md) desde una aplicación lógica con el conector de Azure Event Hubs. De esta forma, puede crear aplicaciones lógicas que automatizan las tareas y los flujos de trabajo para comprobar, enviar y recibir eventos desde su centro de eventos.

Si no tiene una suscripción de Azure, <a href="https://azure.microsoft.com/free/" target="_blank">regístrese para obtener una cuenta gratuita de Azure</a>. Si no está familiarizado con las aplicaciones lógicas, consulte [¿Qué es Azure Logic Apps?](../logic-apps/logic-apps-overview.md) e [Inicio rápido: Creación de la primera aplicación lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md).
Para obtener información técnica específica del conector, consulte la <a href="https://docs.microsoft.com/connectors/eventhubs/" target="blank">referencia sobre el conector de Azure Event Hubs</a>.

## <a name="prerequisites"></a>Requisitos previos

* Un [espacio de nombres de Azure Event Hubs y un centro de eventos](../event-hubs/event-hubs-create.md)

* La aplicación lógica donde quiere acceder al centro de eventos. Para iniciar la aplicación lógica con un desencadenador de Azure Event Hubs, necesita una [aplicación lógica en blanco](../logic-apps/quickstart-create-first-logic-app-workflow.md).

<a name="permissions-connection-string"></a>

## <a name="check-permissions-and-get-connection-string"></a>Comprobación de los permisos y obtención de la cadena de conexión

Para que la aplicación lógica acceda a su instancia de Event Hubs, debe tener permisos de administrador y la cadena de conexión para el espacio de nombres de Event Hubs.

1. Inicie sesión en el <a href="https://portal.azure.com" target="_blank">Azure Portal</a>.

2. Vaya al *espacio de nombres* de sus instancias de Event Hubs, no a un centro de eventos específico. En la página del espacio de nombres, en **Configuración**, elija **Directivas de acceso compartido**. En **Notificaciones**, compruebe que tenga permisos de **Administrador** para ese espacio de nombres.

   ![Administrar los permisos del espacio de nombres del Event Hub](./media/connectors-create-api-azure-event-hubs/event-hubs-namespace.png)

3. Si más adelante desea escribir manualmente la información de conexión, obtenga la cadena de conexión para el espacio de nombres de sus instancias de Event Hubs.

   1. En **Directiva**, elija **RootManageSharedAccessKey**.

   2. Busque la cadena de conexión de clave principal. Elija el botón Copiar y guarde la cadena de conexión para su uso posterior.

      ![Copie la cadena de conexión del espacio de nombres de los Event Hubs](media/connectors-create-api-azure-event-hubs/find-event-hub-namespace-connection-string.png)

      > [!TIP]
      > Para confirmar si la cadena de conexión está asociada al espacio de nombres de Event Hubs o a un centro de eventos específico, asegúrese de que la cadena de conexión no tiene el parámetro `EntityPath` . Si encuentra este parámetro, la cadena de conexión es para la "entidad" de una instancia de Event Hubs específica y no es la cadena correcta para utilizar con la aplicación lógica.

4. Ahora, continúe con [Adición de un desencadenador de Event Hubs](#add-trigger) o [Adición de una acción de Event Hubs](#add-action).

<a name="add-trigger"></a>

## <a name="add-an-event-hubs-trigger"></a>Adición de un desencadenador de Event Hubs

En Azure Logic Apps, cada aplicación lógica debe comenzar con un [desencadenador](../logic-apps/logic-apps-overview.md#logic-app-concepts), que se activa cuando sucede un evento específico o cuando se cumple una condición determinada. Cada vez que el desencadenador se activa, el motor de Logic Apps crea una instancia de aplicación lógica y empieza a ejecutar el flujo de trabajo de la aplicación.

En este ejemplo se muestra cómo puede iniciar un flujo de trabajo de aplicaciones lógicas cuando se envían nuevos eventos al centro de eventos.

1. En Azure Portal o Visual Studio, cree una aplicación lógica en blanco, con lo que se abre el diseñador de Logic Apps. En este ejemplo se usa Azure Portal.

2. En el cuadro de búsqueda, escriba "event hubs" como filtro. En la lista de desencadenadores, seleccione el que desee.

   Este ejemplo utiliza este desencadenador:

   **Event Hubs: cuando los eventos están disponibles en el centro de eventos**

   ![Seleccionar un desencadenador](./media/connectors-create-api-azure-event-hubs/find-event-hubs-trigger.png)

3. Si se le pide indicar los detalles de la conexión, [cree ahora mismo su conexión de Event Hubs](#create-connection). O bien, si la conexión ya existe, especifique la información necesaria para el desencadenador.

   1. En la lista **Nombre del centro de eventos**, seleccione el centro de eventos que desea supervisar.

      ![Especificar Event Hub o grupo de consumidores](./media/connectors-create-api-azure-event-hubs/select-event-hub.png)

   2. Seleccione el intervalo y la frecuencia con la que desea que el desencadenador compruebe el centro de eventos.

   3. Para seleccionar opcionalmente algunas de las opciones avanzadas del desencadenador, elija **Mostrar opciones avanzadas**.

      ![Opciones avanzadas de desencadenador](./media/connectors-create-api-azure-event-hubs/event-hubs-trigger-advanced.png)

      | Propiedad | Detalles | 
      |----------|---------| 
      | Tipo de contenido  | Seleccione el tipo de contenido del evento. El valor predeterminado es "application/octet-stream". |
      | Esquema de contenido | Especifique el esquema de contenido en JSON para los eventos que se leen de Event Hub. |
      | Nombre del grupo de consumidores | Escriba el [nombre del grupo de consumidores](../event-hubs/event-hubs-features.md#consumer-groups) del centro de eventos para leer los eventos. Si no se especifica, se utiliza el grupo de consumidores predeterminado. |
      | Clave de partición mínima | Escriba el identificador de la [partición](../event-hubs/event-hubs-features.md#partitions) mínima que va a leer. De forma predeterminada, se leen todas las particiones. |
      | Clave de partición máxima | Escriba el identificador de la [partición](../event-hubs/event-hubs-features.md#partitions) máxima que va a leer. De forma predeterminada, se leen todas las particiones. |
      | Número máximo de eventos | Escriba el valor del número máximo de eventos. El desencadenador devuelve entre uno y el número de eventos que especifica esta propiedad. |
      |||

4. Cuando esté listo, elija **Guardar** en la barra de herramientas del diseñador.

5. Ahora, agregue a la aplicación lógica una o varias acciones, en función de las tareas que desea realizar con los resultados del desencadenador.

> [!NOTE]
> Todos los desencadenadores de Event Hub son de *sondeo largo*, lo que significa que, cuando se activa cualquiera de ellos, procesa todos los eventos y espera 30 segundos a que aparezcan más eventos en la instancia de Event Hubs.
> Si no se recibe ningún evento en 30 segundos, se omite la ejecución del desencadenador. De lo caso contrario, el desencadenador sigue leyendo eventos hasta que su instancia de Event Hubs está vacía.
> El siguiente sondeo del desencadenador ocurre en función del intervalo de periodicidad especificado en las propiedades del desencadenador.

<a name="add-action"></a>

## <a name="add-an-event-hubs-action"></a>Adición de una acción de Event Hubs

En Azure Logic Apps, una [acción](../logic-apps/logic-apps-overview.md#logic-app-concepts) es un paso del flujo de trabajo que sigue a un desencadenador u otra acción. Para este ejemplo, la aplicación lógica comienza con un desencadenador de Event Hubs que comprueba si hay nuevos eventos en su centro de eventos.

1. En Azure Portal o Visual Studio, abra la aplicación lógica el Diseñador de Logic Apps. En este ejemplo se usa Azure Portal.

2. En el desencadenador o acción, elija **Nuevo paso** > **Agregar una acción**.

   Para agregar una acción entre los pasos existentes, mueva el mouse sobre la flecha de conexión. 
   Elija el signo más (**+**) que aparece y, luego, elija **Agregar una acción**.

3. En el cuadro de búsqueda, escriba "event hubs" como filtro.
En la lista de acciones, seleccione la que desee.

   En este ejemplo, seleccione esta acción: **Event Hubs: Enviar evento**

   ![Seleccione "Event Hubs: Enviar evento"](./media/connectors-create-api-azure-event-hubs/find-event-hubs-action.png)

4. Si se le pide indicar los detalles de la conexión, [cree ahora mismo su conexión de Event Hubs](#create-connection). O bien, si la conexión ya existe, especifique la información necesaria para la acción.

   | Propiedad | Obligatorio | DESCRIPCIÓN |
   |----------|----------|-------------|
   | Nombre del centro de eventos | Sí | Seleccione el centro de eventos donde enviar el evento. |
   | Contenido del evento | Sin  | El contenido del evento que quiere enviar |
   | Properties (Propiedades) | Sin  | Las propiedades de la aplicación y los valores para enviar |
   ||||

   Por ejemplo: 

   ![Seleccione el nombre del centro de eventos y proporcione su contenido](./media/connectors-create-api-azure-event-hubs/event-hubs-send-event-action.png)

5. Cuando esté listo, elija **Guardar** en la barra de herramientas del diseñador.

<a name="create-connection"></a>

## <a name="connect-to-your-event-hub"></a>Conexión al centro de eventos

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)] 

1. Cuando se le solicite la información de conexión, proporcione estos detalles:

   | Propiedad | Obligatorio | Value | DESCRIPCIÓN |
   |----------|----------|-------|-------------|
   | Nombre de la conexión | Sí | <*connection-name*> | El nombre que se va a crear para su conexión |
   | Espacio de nombres de Event Hubs | Sí | <*event-hubs-namespace*> | Seleccione el espacio de nombres de Event Hubs que desea usar. |
   |||||  

   Por ejemplo: 

   ![Creación de una conexión de Event Hubs](./media/connectors-create-api-azure-event-hubs/create-event-hubs-connection-1.png)

   Para escribir de forma manual la cadena de conexión, elija **Especificar la información de conexión manualmente**. 
   Obtenga información acerca de [cómo buscar la cadena de conexión](#permissions-connection-string).

2. Seleccione la directiva de Event Hubs que desea usar, si no está seleccionada. Seleccione **Create**.

   ![Creación de la cadena de conexión de Event Hubs, parte 2](./media/connectors-create-api-azure-event-hubs/create-event-hubs-connection-2.png)

3. Después de crear la conexión, continúe con [Adición de un desencadenador de Event Hubs](#add-trigger) o [Adición de una acción de Event Hubs](#add-action).

## <a name="connector-reference"></a>Referencia de conectores

Para obtener datos técnica, como los desencadenadores, las acciones y los límites, tal como lo describe el archivo Swagger del conector, consulte la [página de referencia del conector](/connectors/eventhubs/).

## <a name="get-support"></a>Obtención de soporte técnico

* Si tiene alguna duda, visite el [foro de Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Para enviar ideas sobre características o votar sobre ellas, visite el [sitio de comentarios de los usuarios de Logic Apps](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre otros [conectores de Logic Apps](../connectors/apis-list.md)