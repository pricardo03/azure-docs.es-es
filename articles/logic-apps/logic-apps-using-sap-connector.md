---
title: 'Conexión a sistemas SAP: Azure Logic Apps | Microsoft Docs'
description: Cómo obtener acceso y administrar los recursos SAP mediante la automatización de flujos de trabajo con Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: divswa, LADocs
ms.topic: article
ms.date: 09/14/2018
tags: connectors
ms.openlocfilehash: dd86fd1aa8b1dab9f329f12924ff37db1256d1eb
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2018
ms.locfileid: "49377914"
---
# <a name="connect-to-sap-systems-from-azure-logic-apps"></a>Conexión a sistemas SAP desde Azure Logic Apps

En este artículo, se muestra cómo obtener acceso a los recursos SAP locales desde una aplicación lógica mediante el conector SAP ERP Central Component (ECC). El conector SAP ECC admite la integración de mensajes o datos hacia y desde sistemas basados en SAP Netweaver a través de un documento intermedio (IDoc), de Business Application Programming Interface (BAPI) o de Remote Function Call (RFC).

El conector SAP ECC utiliza la <a href="https://support.sap.com/en/product/connectors/msnet.html">biblioteca de SAP .Net Connector (NCo)</a> y proporciona estas operaciones o acciones:

- **Enviar a SAP**: enviar IDoc o llamar a funciones de BAPI a través de tRFC en sistemas SAP.
- **Receive from SAP** (Recibir de SAP): recibir IDoc o llamadas de funciones de BAPI a través de tRFC de los sistemas SAP.
- **Generate schemas** (Generar esquemas): generar esquemas de artefactos SAP para IDoc, BAPI o RFC.

El conector SAP se integra con los sistemas SAP locales a través de la [puerta de enlace de datos local](https://www.microsoft.com/download/details.aspx?id=53127). En los escenarios de envío, por ejemplo, al enviar un mensaje desde Logic Apps a un sistema SAP, la puerta de enlace de datos actúa como un cliente RFC y reenvía las solicitudes recibidas de Logic Apps a SAP.
Del mismo modo, en los escenarios de recepción, la puerta de enlace de datos actúa como un servidor RFC que recibe solicitudes de SAP y las reenvía a Logic Apps. 

En este artículo se muestra cómo crear aplicaciones lógicas que se integren con SAP y se tratan los escenarios de integración descritos anteriormente.

## <a name="prerequisites"></a>Requisitos previos

Para seguir con este artículo, necesita los siguientes elementos:

* Una suscripción de Azure. Si aún no tiene ninguna suscripción de Azure, <a href="https://azure.microsoft.com/free/" target="_blank">regístrese para obtener una cuenta gratuita de Azure</a>.

* La aplicación lógica desde donde quiere obtener acceso al sistema SAP y un desencadenador que inicie el flujo de trabajo de la aplicación lógica. Si nunca trabajó con las aplicaciones lógicas, consulte [¿Qué es Azure Logic Apps](../logic-apps/logic-apps-overview.md) y el artículo sobre [Inicio rápido: creación de su primera aplicación lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md).

* El <a href="https://wiki.scn.sap.com/wiki/display/ABAP/ABAP+Application+Server" target="_blank">servidor de aplicaciones de SAP</a> o el <a href="https://help.sap.com/saphelp_nw70/helpdata/en/40/c235c15ab7468bb31599cc759179ef/frameset.htm" target="_blank">servidor de mensajes de SAP</a>

* Descargue e instale la versión más reciente de la [puerta de enlace de datos local](https://www.microsoft.com/download/details.aspx?id=53127) en cualquier equipo local. Asegúrese de configurar la puerta de enlace en Azure Portal antes de continuar. La puerta de enlace le ayuda a obtener acceso de forma segura a los datos y recursos que se encuentran en el entorno local. Para obtener más información, consulte [Instalación de la puerta de enlace de datos local para Azure Logic Apps](../logic-apps/logic-apps-gateway-install.md).

* Descargue e instale la biblioteca cliente de SAP más reciente, que actualmente es <a href="https://softwaredownloads.sap.com/file/0020000001865512018" target="_blank">Conector SAP (NCo) 3.0.21.0 para Microsoft .NET Framework 4.0 y Windows de 64 bits (x64)</a>, en el mismo equipo que la puerta de enlace de datos local. Debe instalar esta versión o una versión posterior por estos motivos:

  * Las versiones anteriores de NCo de SAP pueden interbloquearse cuando se envía más de un mensaje IDoc al mismo tiempo. 
  Esta condición bloquea todos los mensajes posteriores que se envían al destino de SAP, haciendo que se agote el tiempo de espera de los mensajes.

  * La puerta de enlace de datos local solo se ejecuta en sistemas de 64 bits. 
  De lo contrario, obtendrá un error de "imagen incorrecta" porque el servicio de host de la puerta de enlace de datos no es compatible con los ensamblados de 32 bits.

  * El servicio de host de la puerta de enlace de datos y el adaptador de SAP de Microsoft usan .NET Framework 4.5. NCo de SAP para .NET Framework 4.0 funciona con procesos que usan las versiones 4.0 a 4.7.1 del runtime de .NET. 
  NCo de SAP para .NET Framework 2.0 funciona con procesos que usan las versiones 2.0 a 3.5 del runtime de .NET y ya no funciona con la puerta de enlace de datos local más reciente.

* Contenido del mensaje que puede enviar al servidor SAP, como un archivo de IDoc de ejemplo. Este contenido debe estar en formato XML e incluir el espacio de nombres para la acción de SAP que se va a utilizar.

<a name="add-trigger"></a>

## <a name="send-to-sap"></a>Enviar a SAP

En este ejemplo, se utiliza una aplicación lógica que se puede desencadenar con una solicitud HTTP. La aplicación lógica envía un documento intermedio (IDOC) a un servidor SAP y devuelve una respuesta al solicitante que llamó a la aplicación lógica. 

### <a name="add-http-request-trigger"></a>Adición de un desencadenador de solicitud HTTP

En Azure Logic Apps, cada aplicación lógica debe comenzar con un [desencadenador](../logic-apps/logic-apps-overview.md#logic-app-concepts), que se activa cuando sucede un evento específico o cuando se cumple una condición determinada. Cada vez que el desencadenador se activa, el motor de Logic Apps crea una instancia de aplicación lógica y empieza a ejecutar el flujo de trabajo de la aplicación.

En este ejemplo, cree una aplicación lógica con un punto de conexión en Azure para poder enviar *solicitudes HTTP POST* a la aplicación lógica. Cuando la aplicación lógica reciba estas solicitudes HTTP, el desencadenador se activará y ejecutará el paso siguiente en el flujo de trabajo.

1. En [Azure Portal](https://portal.azure.com), cree una aplicación lógica en blanco que abra el Diseñador de aplicación lógica. 

2. En el cuadro de búsqueda, escriba "solicitud http" para el filtro. En la lista de desencadenadores, seleccione el siguiente: **Request - When a HTTP request is received** (Solicitar: cuando se recibe una solicitud HTTP).

   ![Adición de un desencadenador de solicitud HTTP](./media/logic-apps-using-sap-connector/add-trigger.png)

3. Ahora puede guardar la aplicación lógica para poder generar una dirección URL del punto de conexión para la aplicación lógica.
En la barra de herramientas del diseñador, haga clic en **Guardar**. 

   La dirección URL del punto de conexión aparece ahora en el desencadenador, por ejemplo:

   ![Generación de una dirección URL para el punto de conexión](./media/logic-apps-using-sap-connector/generate-http-endpoint-url.png)

<a name="add-action"></a>

### <a name="add-sap-action"></a>Adición de una acción de SAP

En Azure Logic Apps, una [acción](../logic-apps/logic-apps-overview.md#logic-app-concepts) es un paso del flujo de trabajo que sigue a un desencadenador u otra acción. Si aún no ha agregado un desencadenador a la aplicación lógica y quiere seguir este ejemplo, [agregue el desencadenador que se describe en esta sección](#add-trigger).

1. En el Diseñador de aplicación lógica, en el desencadenador, elija **Nuevo paso** > **Agregar una acción**.

   ![Agregar una acción](./media/logic-apps-using-sap-connector/add-action.png) 

2. En el cuadro de búsqueda, escriba "sap" como filtro. En la lista de acciones, seleccione esta acción: **Send message to SAP** (Enviar mensaje a SAP).
  
   ![Selección de la acción de envío de SAP](media/logic-apps-using-sap-connector/select-sap-send-action.png)

   Como alternativa, en lugar de buscar, elija la pestaña **Enterprise** (Empresa) y seleccione la acción de SAP.

   ![Selección de la acción de envío de SAP desde la pestaña Enterprise](media/logic-apps-using-sap-connector/select-sap-send-action-ent-tab.png)

3. Si se le pide indicar los detalles de la conexión, cree ahora mismo su conexión de SAP. De lo contrario, si la conexión ya existe, continúe con el paso siguiente para poder configurar la acción de SAP. 

   **Creación de la conexión de SAP local**

   1. Proporcione la información de conexión para el servidor SAP. 
   Para la propiedad **Data Gateway** (Puerta de enlace de datos), seleccione la puerta de enlace de datos que creó en Azure Portal para la instalación de la puerta de enlace.

      Si la propiedad **Logon Type** (Tipo de inicio de sesión) está establecida en **Application Server** (Servidor de aplicaciones), estas propiedades, que suelen aparecer como opcionales, son obligatorias:

      ![Creación de conexiones del servidor de aplicaciones de SAP](media/logic-apps-using-sap-connector/create-SAP-application-server-connection.png) 

      Si la propiedad **Logon Type** (Tipo de inicio de sesión) está establecida en **Group** (Grupo), estas propiedades, que suelen aparecer como opcionales, son obligatorias: 

      ![Creación de conexiones del servidor de mensajes de SAP](media/logic-apps-using-sap-connector/create-SAP-message-server-connection.png) 

   2. Cuando termine, seleccione **Crear**. 
   
      Logic Apps configura y comprueba la conexión y se asegura de que esta funciona correctamente.

4. Ahora, busque y seleccione una acción en el servidor SAP. 

   1. En el cuadro **Acción SAP**, elija el icono de carpeta. 
   Desde la lista de archivos, busque y seleccione el mensaje de SAP que quiere utilizar. 
   Para navegar por la lista, utilice las flechas.

      En este ejemplo se selecciona un IDoc con el tipo **Order** (Orden). 

      ![Búsqueda y selección de la acción IDoc](./media/logic-apps-using-sap-connector/SAP-app-server-find-action.png)

      Si no encuentra la acción que quiere, puede escribir manualmente una ruta de acceso, por ejemplo:

      ![Proporcionar manualmente la ruta de acceso a la acción IDoc](./media/logic-apps-using-sap-connector/SAP-app-server-manually-enter-action.png)

      > [!TIP]
      > Proporcionar el valor de acción de SAP mediante el editor de expresiones. De este modo, puede usar la misma acción para diferentes tipos de mensaje.

      Para obtener más información sobre las operaciones de IDoc, consulte [Esquemas de mensaje para operaciones de IDOC](https://docs.microsoft.com/biztalk/adapters-and-accelerators/adapter-sap/message-schemas-for-idoc-operations).

   2. Haga clic en el cuadro **Mensaje de entrada** para que aparezca la lista de contenido dinámico. 
   En dicha lista, en **When a HTTP request is received** (Cuando se recibe una solicitud HTTP), seleccione el campo **Cuerpo**. 

      En este paso, se incluye el contenido del cuerpo del desencadenador de la solicitud HTTP y se envía la salida al servidor SAP.

      ![Selección del campo "Cuerpo"](./media/logic-apps-using-sap-connector/SAP-app-server-action-select-body.png)

      Una vez que haya terminado, la acción SAP debe ser parecida a la de este ejemplo:

      ![Completar una acción SAP](./media/logic-apps-using-sap-connector/SAP-app-server-complete-action.png)

5. Guarde la aplicación lógica. En la barra de herramientas del diseñador, haga clic en **Guardar**.

<a name="add-response"></a>

### <a name="add-http-response-action"></a>Adición de una acción de respuesta HTTP

Ahora, agregue una acción de respuesta al flujo de trabajo de la aplicación lógica e incluya el resultado de la acción SAP. De este modo, la aplicación lógica devuelve los resultados del servidor SAP al solicitante original. 

1. En el Diseñador de aplicación lógica, en la acción SAP, elija **Nuevo paso** > **Agregar una acción**.

2. En el cuadro de búsqueda, escriba "respuesta" para el filtro. En la lista de acciones, seleccione la siguiente acción: **Request - Response (Solicitar: respuesta)**

3. Haga clic en el cuadro **Cuerpo** para que aparezca la lista de contenido dinámico. En dicha lista, en **Enviar a SAP**, seleccione el campo **Cuerpo**. 

   ![Completar una acción SAP](./media/logic-apps-using-sap-connector/select-sap-body-for-response-action.png)

4. Guarde la aplicación lógica. 

### <a name="test-your-logic-app"></a>Comprobación de la aplicación lógica

1. Si la aplicación lógica aún no está habilitada, en el menú de aplicación lógica, elija **Introducción**. En la barra de herramientas, elija **Habilitar**. 

2. En la barra de herramientas del Diseñador de aplicación lógica, elija **Ejecutar**. Con este paso, se inicia manualmente la aplicación lógica.

3. Desencadene la aplicación lógica mediante el envío de una solicitud HTTP POST a la dirección URL del desencadenador de la solicitud HTTP e incluya el contenido del mensaje con la solicitud. Para enviar la solicitud, puede usar una herramienta como [Postman](https://www.getpostman.com/apps). 

   En el caso de este artículo, la solicitud envía un archivo de IDoc, que debe estar en formato XML e incluir el espacio de nombres de la acción de SAP que está utilizando, por ejemplo: 

   ``` xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <Send xmlns="http://Microsoft.LobServices.Sap/2007/03/Idoc/2/ORDERS05//720/Send">
      <idocData>
         <...>
      </idocData>
   </Send>
   ```

4. Después de enviar la solicitud HTTP, espere la respuesta de la aplicación lógica.

   > [!NOTE]
   > La aplicación lógica puede agotar el tiempo de espera si todos los pasos necesarios para la respuesta no finalizan dentro del [límite de tiempo de espera de la solicitud](./logic-apps-limits-and-config.md). Si se produce esta situación, es posible que se bloqueen las solicitudes. A fin de poder diagnosticar problemas, obtenga información sobre cómo puede [comprobar y supervisar las aplicaciones lógicas](../logic-apps/logic-apps-monitor-your-logic-apps.md).

Enhorabuena. Ha creado una aplicación lógica que puede comunicarse con el servidor SAP. Ahora que ha configurado una conexión de SAP para la aplicación lógica, puede explorar otras acciones SAP disponibles, como RFC y BAPI.

## <a name="receive-from-sap"></a>Recibir de SAP

En este ejemplo se utiliza una aplicación lógica que se desencadena cuando se recibe un mensaje de un sistema SAP. 

### <a name="add-sap-trigger"></a>Agregar un desencadenador de SAP

1. En Azure Portal, cree una aplicación lógica en blanco que abra el Diseñador de aplicación lógica. 

2. En el cuadro de búsqueda, escriba "sap" como filtro. En la lista de desencadenadores, seleccione este desencadenador: **When a message is received from SAP** (Cuando se recibe un mensaje de SAP).

   ![Agregar un desencadenador de SAP](./media/logic-apps-using-sap-connector/add-sap-trigger.png)

   Como alternativa, puede ir a la pestaña Enterprise (Empresa) y seleccionar el desencadenador.

   ![Agregar un desencadenador de SAP desde la pestaña Enterprise](./media/logic-apps-using-sap-connector/add-sap-trigger-ent-tab.png)

3. Si se le pide indicar los detalles de la conexión, cree ahora mismo su conexión de SAP. De lo contrario, si la conexión ya existe, continúe con el paso siguiente para poder configurar la acción de SAP. 

   **Creación de la conexión de SAP local**

   1. Proporcione la información de conexión para el servidor SAP. 
   Para la propiedad **Data Gateway** (Puerta de enlace de datos), seleccione la puerta de enlace de datos que creó en Azure Portal para la instalación de la puerta de enlace.

      Si la propiedad **Logon Type** (Tipo de inicio de sesión) está establecida en **Application Server** (Servidor de aplicaciones), estas propiedades, que suelen aparecer como opcionales, son obligatorias:

      ![Creación de conexiones del servidor de aplicaciones de SAP](media/logic-apps-using-sap-connector/create-SAP-application-server-connection.png) 

      Si la propiedad **Logon Type** (Tipo de inicio de sesión) está establecida en **Group** (Grupo), estas propiedades, que suelen aparecer como opcionales, son obligatorias:

      ![Creación de conexiones del servidor de mensajes de SAP](media/logic-apps-using-sap-connector/create-SAP-message-server-connection.png)  

4. Proporcione los parámetros necesarios en función de la configuración del sistema SAP. 

   También puede proporcionar una o varias acciones de SAP. 
   Esta lista de acciones especifica los mensajes que recibe el desencadenador del servidor SAP a través de la puerta de enlace de datos. 
   Una lista vacía especifica que el desencadenador recibe todos los mensajes. 
   Si la lista tiene más de un mensaje, el desencadenador recibe solo los mensajes especificados en la lista. La puerta de enlace rechaza todos los demás mensajes enviados desde el servidor SAP.

   Puede seleccionar una acción de SAP desde el selector de archivos:

   ![Selección de una acción de SAP](media/logic-apps-using-sap-connector/select-SAP-action-trigger.png)  

   También puede especificar una acción manualmente:

   ![Introducción manual de una acción de SAP](media/logic-apps-using-sap-connector/manual-enter-SAP-action-trigger.png) 

   Este es un ejemplo que muestra la apariencia de la acción cuando se configura el desencadenador para recibir varios mensajes.

   ![Ejemplo de desencadenador](media/logic-apps-using-sap-connector/example-trigger.png)  

   Para obtener más información sobre las operaciones de SAP, consulte [Esquemas de mensaje para las operaciones de IDOC](https://docs.microsoft.com/biztalk/adapters-and-accelerators/adapter-sap/message-schemas-for-idoc-operations)

5. Ahora guarde la aplicación lógica para poder empezar a recibir mensajes del sistema SAP.
En la barra de herramientas del diseñador, haga clic en **Guardar**. 

La aplicación lógica está preparada para recibir mensajes del sistema SAP. 

> [!NOTE]
> El desencadenador de SAP no es un desencadenador de sondeo, sino un desencadenador basad en webhook. Solo se llama al desencadenador desde la puerta de enlace cuando existe un mensaje, por lo que no es necesario ningún sondeo. 

### <a name="test-your-logic-app"></a>Comprobación de la aplicación lógica

1. Para desencadenar la aplicación lógica, envíe un mensaje desde el sistema SAP.

2. En el menú de la aplicación lógica, elija **Información general** y revise **Historial de ejecuciones** para conocer las nuevas ejecuciones de su aplicación lógica. 

3. Abra la ejecución más reciente, que muestra el mensaje enviado desde su sistema SAP en la sección de salidas del desencadenador.

## <a name="generate-schemas-for-artifacts-in-sap"></a>Generar esquemas para artefactos en SAP

En este ejemplo, se utiliza una aplicación lógica que se puede desencadenar con una solicitud HTTP. La acción de SAP envía una solicitud a un sistema SAP para generar los esquemas de documento intermedio (IDoc) y BAPI especificados. Los esquemas que se devuelven en la respuesta se cargan en una cuenta de integración mediante el conector de Azure Resource Manager.

### <a name="add-http-request-trigger"></a>Adición de un desencadenador de solicitud HTTP

1. En Azure Portal, cree una aplicación lógica en blanco que abra el Diseñador de aplicación lógica. 

2. En el cuadro de búsqueda, escriba "solicitud http" para el filtro. En la lista de desencadenadores, seleccione el siguiente: **Request - When a HTTP request is received** (Solicitar: cuando se recibe una solicitud HTTP).

   ![Adición de un desencadenador de solicitud HTTP](./media/logic-apps-using-sap-connector/add-trigger.png)

3. Ahora puede guardar la aplicación lógica para poder generar una dirección URL del punto de conexión para la aplicación lógica.
En la barra de herramientas del diseñador, haga clic en **Guardar**. 

   La dirección URL del punto de conexión aparece ahora en el desencadenador, por ejemplo:

   ![Generación de una dirección URL para el punto de conexión](./media/logic-apps-using-sap-connector/generate-http-endpoint-url.png)

### <a name="add-sap-action-to-generate-schemas"></a>Agregar una acción de SAP para generar esquemas

1. En el Diseñador de aplicación lógica, en el desencadenador, elija **Nuevo paso** > **Agregar una acción**.

   ![Agregar una acción](./media/logic-apps-using-sap-connector/add-action.png) 

2. En el cuadro de búsqueda, escriba "sap" como filtro. En la lista de acciones, seleccione esta acción: **Generate schemas** (Generar esquemas).
  
   ![Selección de la acción de envío de SAP](media/logic-apps-using-sap-connector/select-sap-schema-generator-action.png)

   Como alternativa, elija la pestaña **Enterprise** (Empresa) y seleccione la acción de SAP.

   ![Selección de la acción de envío de SAP desde la pestaña Enterprise](media/logic-apps-using-sap-connector/select-sap-schema-generator-ent-tab.png)

3. Si se le pide indicar los detalles de la conexión, cree ahora mismo su conexión de SAP. De lo contrario, si la conexión ya existe, continúe con el paso siguiente para poder configurar la acción de SAP. 

   **Creación de la conexión de SAP local**

   1. Proporcione la información de conexión para el servidor SAP. 
   Para la propiedad **Data Gateway** (Puerta de enlace de datos), seleccione la puerta de enlace de datos que creó en Azure Portal para la instalación de la puerta de enlace.

      Si la propiedad **Logon Type** (Tipo de inicio de sesión) está establecida en **Application Server** (Servidor de aplicaciones), estas propiedades, que suelen aparecer como opcionales, son obligatorias:

      ![Creación de conexiones del servidor de aplicaciones de SAP](media/logic-apps-using-sap-connector/create-SAP-application-server-connection.png) 

      Si la propiedad **Logon Type** (Tipo de inicio de sesión) está establecida en **Group** (Grupo), estas propiedades, que suelen aparecer como opcionales, son obligatorias:
   
      ![Creación de conexiones del servidor de mensajes de SAP](media/logic-apps-using-sap-connector/create-SAP-message-server-connection.png) 

   2. Cuando termine, seleccione **Crear**. Logic Apps configura y comprueba la conexión y se asegura de que esta funciona correctamente.

4. Proporcione la ruta de acceso del artefacto para el que quiere generar el esquema.

   Puede seleccionar la acción de SAP desde el selector de archivos:

   ![Selección de una acción de SAP](media/logic-apps-using-sap-connector/select-SAP-action-schema-generator.png)  

   O bien, puede escribir manualmente la acción:

   ![Introducción manual de una acción de SAP](media/logic-apps-using-sap-connector/manual-enter-SAP-action-schema-generator.png) 

   Para generar esquemas de más de un artefacto, proporcione los detalles de la acción de SAP de cada artefacto, por ejemplo:

   ![Selección de la opción para agregar un elemento](media/logic-apps-using-sap-connector/schema-generator-array-pick.png) 

   ![Visualización de dos elementos](media/logic-apps-using-sap-connector/schema-generator-example.png) 

   Para obtener más información sobre la acción de SAP, consulte [Esquemas de mensaje para las operaciones de IDOC](https://docs.microsoft.com/biztalk/adapters-and-accelerators/adapter-sap/message-schemas-for-idoc-operations).

5. Guarde la aplicación lógica. En la barra de herramientas del diseñador, haga clic en **Guardar**.

### <a name="test-your-logic-app"></a>Comprobación de la aplicación lógica

1. En la barra de herramientas del diseñador, elija **Ejecutar** para desencadenar una ejecución de la aplicación lógica.

2. Abra la ejecución y compruebe las salidas de la acción **Generate schemas** (Generar esquemas). 

   Las salidas muestran los esquemas generados para la lista de mensajes especificada.

### <a name="upload-schemas-to-integration-account"></a>Cargar esquemas en la cuenta de integración

Si lo desea, puede descargar o almacenar los esquemas generados en repositorios, como un blob, el almacenamiento o la cuenta de integración. Las cuentas de integración proporcionan una excelente experiencia con otras acciones de XML, por lo que en este ejemplo se muestra cómo cargar los esquemas en una cuenta de integración de la misma aplicación lógica mediante el conector de Azure Resource Manager.

1. En Diseñador de aplicación lógica, en el desencadenador, elija **Nuevo paso** > **Agregar una acción**. En el cuadro de búsqueda, escriba "resource manager" como filtro. Seleccione esta acción: **Create or update a resource** (Crear o actualizar un recurso).

   ![Selección de la acción de Azure Resource Manager](media/logic-apps-using-sap-connector/select-arm-action.png) 

2. Escriba los detalles, incluida su suscripción a Azure, el grupo de recursos de Azure y la cuenta de integración. Para otros campos, vea el ejemplo siguiente.

   ![Introducción de detalles de la acción de Azure Resource Manager](media/logic-apps-using-sap-connector/arm-action.png)

   La acción **Generate schemas** (Generar esquemas) de SAP genera esquemas como una colección, de modo que el diseñador agrega automáticamente un bucle **For each** a la acción. 
   El siguiente es un ejemplo que muestra cómo aparece esta acción:

   ![Acción de Azure Resource Manager con el bucle "for each"](media/logic-apps-using-sap-connector/arm-action-foreach.png)  

   > [!NOTE]
   > Los esquemas utilizan el formato codificado en Base64. Para cargar los esquemas en una cuenta de integración, deben descodificarse mediante la función `base64ToString()`. El siguiente es un ejemplo en el que se muestra el código del elemento `"properties"`:
   >
   > ```json
   > "properties": {
   >    "Content": "@base64ToString(items('For_each')?['Content'])",
   >    "ContentType": "application/xml",
   >    "SchemaType": "Xml"
   > }
   > ```

3. Guarde la aplicación lógica. En la barra de herramientas del diseñador, haga clic en **Guardar**.

### <a name="test-your-logic-app"></a>Comprobación de la aplicación lógica

1. En la barra de herramientas del diseñador, elija **Ejecutar** para desencadenar manualmente la aplicación lógica.

2. Después de una ejecución correcta, vaya a la cuenta de integración y compruebe que los esquemas generados existen.

## <a name="known-issues-and-limitations"></a>Problemas conocidos y limitaciones

Estos son los problemas y limitaciones actualmente conocidos para el conector SAP:

* El desencadenador SAP no admite la recepción de IDOC por lotes de SAP. Esta acción puede producir un error de conexión de RFC entre el sistema SAP y la puerta de enlace de datos.

* El desencadenador SAP no admite clústeres de puerta de enlace de datos. En algunos casos de conmutación por error, el nodo de puerta de enlace de datos que se comunica con el sistema SAP puede diferir del nodo activo, lo que produce un comportamiento inesperado. Para los escenarios de envío, se admiten clústeres de puerta de enlace de datos.

* En escenarios de recepción, no se admite la devolución de una respuesta no nula. Una aplicación lógica con un desencadenador y una acción de respuesta da como resultado un comportamiento inesperado. 

* Solo un envío único a una llamada o un mensaje de SAP funciona con tRFC. El patrón de confirmación de Business Application Programming Interface (BAPI), como realizar varias llamadas de tRFC en la misma sesión, no se admite.

* No se admiten RFC con datos adjuntos para ambas acciones Send to SAP (Enviar a SAP) y Generate schemas (Generar esquemas).

* El conector SAP no admite actualmente las cadenas de enrutador SAP. La puerta de enlace de datos local debe existir en la misma LAN que el sistema SAP al que quiere conectarse.

* La conversión de los valores ausentes (nulos), vacíos, mínimos y máximos de los campos DATS y TIMS de SAP está sujeta a cambios en actualizaciones posteriores para la puerta de enlace de datos local.

## <a name="get-support"></a>Obtención de soporte técnico

* Si tiene alguna duda, visite el [foro de Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Para enviar ideas sobre características o votar sobre ellas, visite el [sitio de comentarios de los usuarios de Logic Apps](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Pasos siguientes

* [Conexión a sistemas locales](../logic-apps/logic-apps-gateway-connection.md) desde aplicaciones lógicas
* Obtenga información acerca de cómo validar, transformar y realizar otras operaciones de mensaje con [Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md).
* Obtenga más información sobre otros [conectores de Logic Apps](../connectors/apis-list.md)
