---
title: 'Conexión a sistemas SAP: Azure Logic Apps | Microsoft Docs'
description: Cómo obtener acceso y administrar los recursos SAP mediante la automatización de flujos de trabajo con Azure Logic Apps
author: ecfan
ms.author: estfan
ms.date: 05/31/2018
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, divswa, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: d677c0eae9c92f90783ed4ebd95a528b34c872ec
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "58170843"
---
# <a name="connect-to-sap-systems-from-azure-logic-apps"></a>Conexión a sistemas SAP desde Azure Logic Apps

> [!NOTE]
> Este conector SAP está programada para desuso. Por favor, usar o migrar a la [conector de SAP más reciente y más avanzado](./logic-apps-using-sap-connector.md). 

En este artículo, se muestra cómo obtener acceso a los recursos SAP desde una aplicación lógica mediante el uso de los conectores del servidor de mensajes y del servidor de aplicaciones de SAP. De este modo, puede automatizar las tareas, los procesos y los flujos de trabajo que administran los datos y los recursos SAP mediante la creación de aplicaciones lógicas.

En este ejemplo, se utiliza una aplicación lógica que se puede desencadenar con una solicitud HTTP. La aplicación lógica envía un documento intermedio (IDOC) a un servidor SAP y devuelve una respuesta al solicitante que llamó a la aplicación lógica.
Los conectores SAP actuales tienen acciones, pero no desencadenadores, por lo que en este ejemplo se utiliza el [desencadenador de solicitud HTTP](../connectors/connectors-native-reqres.md) como primer paso del flujo de trabajo de la aplicación lógica. Para obtener información técnica específica del conector SAP, consulte los siguientes artículos de referencia: 

* <a href="https://docs.microsoft.com/connectors/sapapplicationserver/" target="blank">Conector del servidor de aplicaciones de SAP</a>
* <a href="https://docs.microsoft.com/connectors/sapmessageserver/" target="blank">Conector del servidor de mensajes de SAP</a>

Si aún no tiene ninguna suscripción de Azure, <a href="https://azure.microsoft.com/free/" target="_blank">regístrese para obtener una cuenta gratuita de Azure</a>.

## <a name="prerequisites"></a>Requisitos previos

Para seguir con este artículo, necesita los siguientes elementos:

* La aplicación lógica desde donde quiere obtener acceso al sistema SAP y un desencadenador que inicie el flujo de trabajo de la aplicación lógica. Actualmente, los conectores SAP solo proporcionan acciones. Si no está familiarizado con las aplicaciones lógicas, consulte [¿Qué es Azure Logic Apps?](../logic-apps/logic-apps-overview.md) e [Inicio rápido: Creación de la primera aplicación lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md).

* El <a href="https://wiki.scn.sap.com/wiki/display/ABAP/ABAP+Application+Server" target="_blank">servidor de aplicaciones de SAP</a> o el <a href="https://help.sap.com/saphelp_nw70/helpdata/en/40/c235c15ab7468bb31599cc759179ef/frameset.htm" target="_blank">servidor de mensajes de SAP</a>

* Descargue e instale la versión más reciente de la [puerta de enlace de datos local](https://www.microsoft.com/download/details.aspx?id=53127) en cualquier equipo local. Asegúrese de configurar la puerta de enlace en Azure Portal antes de continuar. La puerta de enlace le ayuda a obtener acceso de forma segura a los datos y recursos que se encuentran en el entorno local. Para obtener más información, consulte [Instalación de la puerta de enlace de datos local para Azure Logic Apps](../logic-apps/logic-apps-gateway-install.md).

* Descargue e instale la biblioteca cliente de SAP más reciente, que actualmente es <a href="https://softwaredownloads.sap.com/file/0020000000086282018" target="_blank">Conector SAP (NCo) 3.0.20.0 para Microsoft .NET Framework 4.0 y Windows de 64 bits (x64)</a>, en el mismo equipo que la puerta de enlace de datos local. Debe instalar esta versión o una versión posterior por estos motivos:

  * Las versiones anteriores de NCo de SAP pueden interbloquearse cuando se envía más de un mensaje IDoc al mismo tiempo. 
  Esta condición bloquea todos los mensajes posteriores que se envían al destino de SAP, haciendo que se agote el tiempo de espera de los mensajes.

  * La puerta de enlace de datos local solo se ejecuta en sistemas de 64 bits. 
  De lo contrario, obtendrá un error de "imagen incorrecta" porque el servicio de host de la puerta de enlace de datos no es compatible con los ensamblados de 32 bits.

  * El servicio de host de la puerta de enlace de datos y el adaptador de SAP de Microsoft usan .NET Framework 4.5. NCo de SAP para .NET Framework 4.0 funciona con procesos que usan las versiones 4.0 a 4.7.1 del runtime de .NET. 
  NCo de SAP para .NET Framework 2.0 funciona con procesos que usan las versiones 2.0 a 3.5 del runtime de .NET y ya no funciona con la puerta de enlace de datos local más reciente.

* Contenido del mensaje que puede enviar al servidor SAP, como un archivo de IDoc de ejemplo. Este contenido debe estar en formato XML e incluir el espacio de nombres para la acción de SAP que se va a utilizar.

<a name="add-trigger"></a>

## <a name="add-http-request-trigger"></a>Adición de un desencadenador de solicitud HTTP

En Azure Logic Apps, cada aplicación lógica debe comenzar con un [desencadenador](../logic-apps/logic-apps-overview.md#logic-app-concepts), que se activa cuando sucede un evento específico o cuando se cumple una condición determinada. Cada vez que el desencadenador se activa, el motor de Logic Apps crea una instancia de aplicación lógica y empieza a ejecutar el flujo de trabajo de la aplicación.

En este ejemplo, cree una aplicación lógica con un punto de conexión en Azure para poder enviar *solicitudes HTTP POST* a la aplicación lógica. Cuando la aplicación lógica reciba estas solicitudes HTTP, el desencadenador se activará y ejecutará el paso siguiente en el flujo de trabajo.

1. En Azure Portal, cree una aplicación lógica en blanco que abra el Diseñador de aplicación lógica. 

2. En el cuadro de búsqueda, escriba "solicitud http" para el filtro. En la lista de desencadenadores, seleccione este desencadenador: **Solicitud - Cuando se recibe una solicitud HTTP**.

   ![Adición de un desencadenador de solicitud HTTP](./media/logic-apps-using-sap-connector-old/add-trigger.png)

3. Ahora puede guardar la aplicación lógica para poder generar una dirección URL del punto de conexión para la aplicación lógica.
En la barra de herramientas del diseñador, haga clic en **Guardar**. 

   La dirección URL del punto de conexión aparece ahora en el desencadenador, por ejemplo:

   ![Generación de una dirección URL para el punto de conexión](./media/logic-apps-using-sap-connector-old/generate-http-endpoint-url.png)

<a name="add-action"></a>

## <a name="add-sap-action"></a>Adición de una acción de SAP

En Azure Logic Apps, una [acción](../logic-apps/logic-apps-overview.md#logic-app-concepts) es un paso del flujo de trabajo que sigue a un desencadenador u otra acción. Si aún no ha agregado un desencadenador a la aplicación lógica y quiere seguir este ejemplo, [agregue el desencadenador que se describe en esta sección](#add-trigger).

1. En el Diseñador de aplicación lógica, en el desencadenador, elija **Nuevo paso** > **Agregar una acción**.

   ![Agregar una acción](./media/logic-apps-using-sap-connector-old/add-action.png) 

2. En el cuadro de búsqueda, escriba "servidor sap" como filtro. En la lista de acciones, seleccione la acción del servidor SAP: 

   * **Servidor de aplicaciones de SAP: enviar a SAP**
   * **Servidor de mensajes de SAP: enviar a SAP**

   Este ejemplo usa esta acción: **Servidor de aplicaciones de SAP: enviar a SAP**

   ![Seleccione el "Servidor de aplicaciones de SAP" o el "Servidor de mensajes de SAP".](media/logic-apps-using-sap-connector-old/select-sap-action.png)

3. Si se le pide indicar los detalles de la conexión, cree ahora mismo su conexión de SAP. De lo contrario, si la conexión ya existe, continúe con el paso siguiente para poder configurar la acción de SAP. 

   **Creación de la conexión de SAP local**

   1. En **Puertas de enlace**, seleccione **Conectarse mediante una puerta de enlace de datos local** para que aparezcan las propiedades de conexión local.

   2. Proporcione la información de conexión para el servidor SAP. 
   Para la propiedad **puerta de enlace**, seleccione la puerta de enlace de datos que ha creado en Azure Portal para la instalación de la puerta de enlace, por ejemplo:

      **Servidor de aplicaciones de SAP**

      ![Creación de conexiones del servidor de aplicaciones de SAP](./media/logic-apps-using-sap-connector-old/create-SAP-app-server-connection.png)  

      **Servidor de mensajes de SAP**

      ![Creación de conexiones del servidor de mensajes de SAP](media/logic-apps-using-sap-connector-old/create-SAP-message-server-connection.png) 

   2. Cuando termine, seleccione **Crear**.

      Logic Apps configura y comprueba la conexión y se asegura de que esta funciona correctamente.

4. Ahora, busque y seleccione una acción en el servidor SAP. 

   1. En el cuadro **Acción SAP**, elija el icono de carpeta. 
   Desde la lista de carpetas, busque y seleccione la acción que quiere utilizar. 

      Este ejemplo, se selecciona la categoría **IDOC** para la acción IDoc. 

      ![Búsqueda y selección de la acción IDoc](./media/logic-apps-using-sap-connector-old/SAP-app-server-find-action.png)

      Si no encuentra la acción que quiere, puede escribir manualmente una ruta de acceso, por ejemplo:

      ![Proporcionar manualmente la ruta de acceso a la acción IDoc](./media/logic-apps-using-sap-connector-old/SAP-app-server-manually-enter-action.png)

      Para obtener más información sobre las operaciones de IDoc, consulte [Esquemas de mensaje para las operaciones de IDOC](https://docs.microsoft.com/biztalk/adapters-and-accelerators/adapter-sap/message-schemas-for-idoc-operations)

   2. Haga clic en el cuadro **Mensaje de entrada** para que aparezca la lista de contenido dinámico. 
   En dicha lista, en **When a HTTP request is received** (Cuando se recibe una solicitud HTTP), seleccione el campo **Cuerpo**. 

      En este paso, se incluye el contenido del cuerpo del desencadenador de la solicitud HTTP y se envía la salida al servidor SAP.

      ![Selección del campo "Cuerpo"](./media/logic-apps-using-sap-connector-old/SAP-app-server-action-select-body.png)

      Una vez que haya terminado, la acción SAP debe ser parecida a la de este ejemplo:

      ![Completar una acción SAP](./media/logic-apps-using-sap-connector-old/SAP-app-server-complete-action.png)

6. Guarde la aplicación lógica. En la barra de herramientas del diseñador, haga clic en **Guardar**.

<a name="add-response"></a>

## <a name="add-http-response-action"></a>Adición de una acción de respuesta HTTP

Ahora, agregue una acción de respuesta al flujo de trabajo de la aplicación lógica e incluya el resultado de la acción SAP. De este modo, la aplicación lógica devuelve los resultados del servidor SAP al solicitante original. 

1. En el Diseñador de aplicación lógica, en la acción SAP, elija **Nuevo paso** > **Agregar una acción**.

2. En el cuadro de búsqueda, escriba "respuesta" para el filtro. En la lista de acciones, seleccione esta acción: **Solicitud - Respuesta**

3. Haga clic en el cuadro **Cuerpo** para que aparezca la lista de contenido dinámico. En dicha lista, en **Enviar a SAP**, seleccione el campo **Cuerpo**. 

   ![Completar una acción SAP](./media/logic-apps-using-sap-connector-old/select-sap-body-for-response-action.png)

4. Guarde la aplicación lógica. 

## <a name="test-your-logic-app"></a>Comprobación de la aplicación lógica

1. Si la aplicación lógica aún no está habilitada, en el menú de aplicación lógica, elija **Introducción**. En la barra de herramientas, elija **Habilitar**. 

2. En la barra de herramientas del Diseñador de aplicación lógica, elija **Ejecutar**. Con este paso, se inicia manualmente la aplicación lógica.

3. Desencadene la aplicación lógica mediante el envío de una solicitud HTTP POST a la dirección URL del desencadenador de la solicitud HTTP e incluya el contenido del mensaje con la solicitud. Para enviar la solicitud, puede usar una herramienta como [Postman](https://www.getpostman.com/apps). 

   En el caso de este artículo, la solicitud envía un archivo de IDoc, que debe estar en formato XML e incluir el espacio de nombres de la acción de SAP que está utilizando, por ejemplo: 

   ``` xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <Send xmlns="http://Microsoft.LobServices.Sap/2007/03/Idoc/3/ORDERS05//620/Send">
      <idocData>
         <...>
      </idocData>
   </Send>
   ```

4. Después de enviar la solicitud HTTP, espere la respuesta de la aplicación lógica.

> [!NOTE]
> La aplicación lógica puede agotar el tiempo de espera si todos los pasos necesarios para la respuesta no finalizan dentro del [límite de tiempo de espera de la solicitud](./logic-apps-limits-and-config.md). Si se produce esta situación, es posible que se bloqueen las solicitudes. A fin de poder diagnosticar problemas, obtenga información sobre cómo puede [comprobar y supervisar las aplicaciones lógicas](../logic-apps/logic-apps-monitor-your-logic-apps.md).

Enhorabuena. Ha creado una aplicación lógica que puede comunicarse con el servidor SAP. Ahora que ha configurado una conexión de SAP para la aplicación lógica, puede explorar otras acciones SAP disponibles, como RFC y BAPI.

## <a name="connector-reference"></a>Referencia de conectores

Para obtener información técnica acerca del conector según se describe en los archivos de Swagger de los conectores, consulte los siguientes artículos de referencia: 

* [Servidor de aplicaciones de SAP](/connectors/sapapplicationserver/)
* [Servidor de mensajes de SAP](/connectors/sapmessageserver/)

## <a name="get-support"></a>Obtención de soporte técnico

* Si tiene alguna duda, visite el [foro de Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Para enviar ideas sobre características o votar sobre ellas, visite el [sitio de comentarios de los usuarios de Logic Apps](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Pasos siguientes

* [Conexión a sistemas locales](../logic-apps/logic-apps-gateway-connection.md) desde aplicaciones lógicas
* Obtenga información acerca de cómo validar, transformar y realizar otras operaciones de mensaje con [Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md).
* Obtenga más información sobre otros [conectores de Logic Apps](../connectors/apis-list.md)
