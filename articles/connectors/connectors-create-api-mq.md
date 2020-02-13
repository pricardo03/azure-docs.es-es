---
title: Conexión con el servidor IBM MQ
description: Envío y recuperación de mensajes con un servidor de Azure o servidor IBM MQ local y Azure Logic Apps
services: logic-apps
ms.suite: integration
author: ChristopherHouser
ms.author: chrishou
ms.reviewer: valthom, logicappspm
ms.topic: article
ms.date: 06/19/2019
tags: connectors
ms.openlocfilehash: 489f53a4f4c1c0d5bd782f42a9daf73217234793
ms.sourcegitcommit: 7c18afdaf67442eeb537ae3574670541e471463d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/11/2020
ms.locfileid: "77118051"
---
# <a name="connect-to-an-ibm-mq-server-from-azure-logic-apps"></a>Conectarse a un servidor IBM MQ desde Azure Logic Apps

El conector IBM MQ envía y recupera mensajes almacenados en un servidor IBM MQ local o en Azure. Este conector incluye un cliente de Microsoft MQ que se comunica con un servidor IBM MQ remoto a través de una red TCP/IP. En este artículo se proporciona una guía de inicio para usar el conector MQ. Puede empezar por examinar un único mensaje en una cola y luego intentar otras acciones.

El conector IBM MQ incluye estas acciones, pero no proporciona ningún desencadenador:

- Examinar un único mensaje sin eliminarlo del servidor IBM MQ
- Examinar un lote de mensajes sin eliminarlos del servidor IBM MQ
- Recibir un único mensaje y eliminarlo del servidor IBM MQ
- Recibir un lote de mensajes y eliminarlos del servidor IBM MQ
- Enviar un único mensaje al servidor IBM MQ

## <a name="prerequisites"></a>Prerrequisitos

* Si usa un servidor MQ local, [instale la puerta de enlace de datos local](../logic-apps/logic-apps-gateway-install.md) en un servidor de la red. El servidor donde está instalada la puerta de enlace de datos local también debe tener .NET Framework 4.6 instalado para que el conector de MQ funcione. También debe crear un recurso de Azure para la puerta de enlace de datos local. Para más información, consulte [Configuración de una conexión de puerta de enlace de datos](../logic-apps/logic-apps-gateway-connection.md).

  Sin embargo, si el servidor MQ está públicamente disponible o disponible en Azure, no debe usar la puerta de enlace de datos.

* Versiones oficialmente compatibles con IBM WebSphere MQ:

  * MQ 7.5
  * MQ 8.0
  * MQ 9.0

* La aplicación lógica en la que desea agregar la acción de MQ. Esta aplicación lógica debe usar la misma ubicación que la conexión de puerta de enlace de datos local y debe tener un desencadenador que inicia el flujo de trabajo. 

  El conector de MQ no tiene ningún desencadenador, por lo que debe agregar primero un desencadenador a la aplicación lógica. Por ejemplo, puede usar el desencadenador de periodicidad. Si no está familiarizado con el uso de aplicaciones lógicas, pruebe este [inicio rápido para crear su primera aplicación lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md). 

## <a name="browse-a-single-message"></a>Examen de un único mensaje

1. En la aplicación lógica, en el desencadenador u otra acción, elija **Nuevo paso**. 

1. En el cuadro de búsqueda, escriba "mq" y seleccione esta acción: **Examen de un mensaje**

   ![Buscar mensaje](media/connectors-create-api-mq/Browse_message.png)

1. Si no tiene una conexión de MQ existente, cree la conexión:  

   1. En la acción, seleccione **Conectarse mediante una puerta de enlace de datos local**.
   
   1. Especifique las propiedades del servidor MQ.  

      Para **Servidor**, especifique el nombre del servidor MQ, o escriba la dirección IP seguida de dos puntos y el número del puerto.
    
   1. Abra la lista de **puertas de enlace**, en la que se muestran las conexiones de puerta de enlace configuradas anteriormente. Seleccione la puerta de enlace.
    
   1. Cuando termine, seleccione **Crear**. 
   
      La conexión es similar a este ejemplo:

      ![Propiedades de conexión](media/connectors-create-api-mq/Connection_Properties.png)

1. Configure las propiedades de la acción:

   * **Queue**: especifique una cola que sea diferente de la conexión.

   * **MessageId**, **CorrelationId**, **GroupId** y otras propiedades: busque un mensaje basado en las distintas propiedades de mensaje de MQ

   * **IncludeInfo**: especifique **True** para incluir información adicional al mensaje en la salida. O bien, especifique **False** para no incluir información adicional al mensaje en la salida.

   * **Timeout**: especifique un valor para determinar cuánto tiempo esperar a que llegue un mensaje a una cola vacía. Si no se especifica nada, se recupera el primer mensaje de la cola y no hay tiempo de espera para que aparezca un mensaje.

     ![Propiedades de Browse message](media/connectors-create-api-mq/Browse_message_Props.png)

1. Haga clic en **Guardar** para guardar los cambios y en **Ejecutar** para ejecutar la aplicación lógica.

   ![Guardar y ejecutar](media/connectors-create-api-mq/Save_Run.png)

   Una vez finalizada la ejecución, se muestran los pasos de la ejecución y puede revisar la salida.

1. Para revisar los detalles de cada paso, elija la marca de verificación verde. Para revisar más información sobre los datos de salida, elija **Mostrar salidas sin procesar**.

   ![Salida de Buscar mensaje](media/connectors-create-api-mq/Browse_message_output.png)  

   Presentamos algunas salidas sin procesar de ejemplo:

   ![Salida sin procesar de Browse message](media/connectors-create-api-mq/Browse_message_raw_output.png)

1. Si establece **IncludeInfo** en true, se muestra la salida siguiente:

   ![IncludeInfo de Buscar mensaje](media/connectors-create-api-mq/Browse_message_Include_Info.png)

## <a name="browse-multiple-messages"></a>Examen de varios mensajes

La acción **Browse messages** incluye la opción **BatchSize** para indicar cuántos mensajes se deben devolver de la cola.  Si **BatchSize** se deja en blanco, se devuelven todos los mensajes. El resultado devuelto es una matriz de mensajes.

1. Al agregar la acción **Examinar mensajes**, de manera predeterminada se selecciona la primera conexión configurada anteriormente. Para crear una nueva conexión, elija **Cambiar conexión**. O bien, seleccione una conexión diferente.

1. Una vez que termina la ejecución de la aplicación lógica, presentamos algunos resultados de ejemplo de la acción **Examinar mensajes**:

   ![Salida de Browse message](media/connectors-create-api-mq/Browse_messages_output.png)

## <a name="receive-single-message"></a>Recibir un mensaje único

La acción **Receive message** tiene las mismas entradas y salidas que la acción **Browse message**. Cuando se usa **Receive message**, el mensaje se elimina de la cola.

## <a name="receive-multiple-messages"></a>Recepción de varios mensajes

La acción **Receive messages** tiene las mismas entradas y salidas que la acción **Browse messages**. Cuando se usa **Receive messages**, los mensajes se eliminan de la cola.

Si no hay mensajes en la cola al realizar la acción browse o receive, el paso produce un error con la siguiente salida:  

![Error de ausencia de mensajes en MQ](media/connectors-create-api-mq/MQ_No_Msg_Error.png)

## <a name="send-message"></a>Enviar mensaje

Al agregar la acción **Enviar mensajes**, de manera predeterminada se selecciona la primera conexión configurada anteriormente. Para crear una nueva conexión, elija **Cambiar conexión**. O bien, seleccione una conexión diferente.

1. Seleccione un tipo de mensaje válido: **Datagrama**, **Responder** o **Solicitud**  

   ![Propiedades de Enviar mensaje](media/connectors-create-api-mq/Send_Msg_Props.png)

1. Cuando finalice la aplicación lógica, presentamos algunos resultados de ejemplo de la acción **Enviar mensaje**:

   ![Salida de Send message](media/connectors-create-api-mq/Send_Msg_Output.png)

## <a name="connector-reference"></a>Referencia de conectores

Para obtener información técnica sobre las acciones y los límites, que se describen en la descripción de OpenAPI (anteriormente conocido como Swagger) del conector, consulte la [página de referencia del conector](/connectors/mq/).

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre otros [conectores de Logic Apps](../connectors/apis-list.md)
