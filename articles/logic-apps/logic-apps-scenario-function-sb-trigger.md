---
title: Llamada o desencadenamiento de aplicaciones lógicas con Azure Functions y Azure Service Bus
description: Crear funciones de Azure llamada o desencadenan de aplicaciones lógicas mediante el uso de Azure Service Bus
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: jehollan, klam, LADocs
ms.topic: article
ms.assetid: 19cbd921-7071-4221-ab86-b44d0fc0ecef
ms.date: 06/04/2019
ms.openlocfilehash: 3d4f642ae25a179ea2c3241240996da774cd8c23
ms.sourcegitcommit: 600d5b140dae979f029c43c033757652cddc2029
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66494920"
---
# <a name="call-or-trigger-logic-apps-by-using-azure-functions-and-azure-service-bus"></a>Llamada o desencadenamiento de aplicaciones lógicas mediante las funciones de Azure y Azure Service Bus

Puede usar [Azure Functions](../azure-functions/functions-overview.md) para desencadenar una aplicación lógica cuando necesite implementar un agente de escucha de ejecución prolongada o tareas. Por ejemplo, puede crear una función de Azure que escuche en un [Azure Service Bus](../service-bus-messaging/service-bus-messaging-overview.md) poner en cola y desencadena inmediatamente una aplicación lógica como desencadenador de push.

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción de Azure. Si no tiene una suscripción de Azure, [regístrese para obtener una cuenta gratuita de Azure](https://azure.microsoft.com/free/).

* Un espacio de nombres de Azure Service Bus. Si no tiene un espacio de nombres [crear primero el espacio de nombres](../service-bus-messaging/service-bus-create-namespace-portal.md).

* Una aplicación de función de Azure, que es un contenedor de Azure functions. Si no tiene una aplicación de función, [crear primero la aplicación de función](../azure-functions/functions-create-first-azure-function.md)y asegúrese de que selecciona .NET como la pila en tiempo de ejecución.

* Conocimientos básicos acerca de [cómo crear aplicaciones lógicas](../logic-apps/quickstart-create-first-logic-app-workflow.md)

## <a name="create-logic-app"></a>Creación de la aplicación lógica

En este escenario, tiene una función que se ejecuta cada aplicación lógica que desea desencadenar. En primer lugar, cree una aplicación lógica que se inicia con un desencadenador de solicitud HTTP. La función llama a ese punto de conexión siempre que se reciba un mensaje en cola.  

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y cree la aplicación lógica en blanco.

   Si está familiarizado con las aplicaciones lógicas, consulte [inicio rápido: Creación de la primera aplicación lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md).

1. En el cuadro de búsqueda, escriba "solicitud http". En la lista de desencadenadores, seleccione este desencadenador: **Cuando se recibe una solicitud HTTP**

   ![Seleccionar un desencadenador](./media/logic-apps-scenario-function-sb-trigger/when-http-request-received-trigger.png)

   Con el desencadenador de solicitud, también puede escribir un esquema JSON para usar con el mensaje de cola. Esquemas JSON ayudan a Diseñador de aplicación lógica a comprender la estructura de los datos de entrada y facilitar las salidas para su uso en el flujo de trabajo.

1. Para especificar un esquema, especifique el esquema en el cuadro **Esquema JSON de cuerpo de solicitud**, por ejemplo:

   ![Especificación del esquema JSON](./media/logic-apps-scenario-function-sb-trigger/when-http-request-received-trigger-schema.png)

   Si no tiene un esquema, pero tiene una carga de ejemplo en formato JSON, puede generar un esquema a partir de dicha carga.

   1. En el desencadenador Request, elija **Usar una carga de ejemplo para generar el esquema**.

   1. En **Enter or paste a sample JSON payload** (Especificar o pegar una carga JSON de ejemplo), proporcione una carga de ejemplo y luego elija **Listo**.

      ![Especificación de la carga de ejemplo](./media/logic-apps-scenario-function-sb-trigger/enter-sample-payload.png)

   Esta carga de ejemplo genera este esquema que aparece en el desencadenador:

   ```json
   {
      "type": "object",
      "properties": {
         "address": {
            "type": "object",
            "properties": {
               "number": {
                  "type": "integer"
               },
               "street": {
                  "type": "string"
               },
               "city": {
                  "type": "string"
               },
               "postalCode": {
                  "type": "integer"
               },
               "country": {
                  "type": "string"
               }
            }
         }
      }
   }
   ```

1. Agregue todas las demás acciones que desea ejecutar después de recibir el mensaje de cola.

   Por ejemplo, puede enviar un correo electrónico con el conector Office 365 Outlook.

1. Guarde la aplicación lógica, que genera la dirección URL de devolución de llamada para el desencadenador en esta aplicación lógica. Más adelante, use esta dirección URL de devolución de llamada en el código para el desencadenador de cola de Azure Service Bus.

   La devolución de llamada a la dirección URL aparece en el **URL de HTTP POST** propiedad.

   ![Generación de la dirección URL de devolución de llamada para el desencadenador](./media/logic-apps-scenario-function-sb-trigger/callback-URL-for-trigger.png)

## <a name="create-azure-function"></a>Creación de una función de Azure

A continuación, cree la función que actúa como desencadenador y escuche la cola.

1. En Azure Portal, abra y expanda la aplicación de función, si aún no está abierta. 

1. En el nombre de la aplicación de función, expanda **Functions**. En el panel **Functions**, elija **Nueva función**.

   ![Expanda "Funciones" y elija "Nueva función"](./media/logic-apps-scenario-function-sb-trigger/create-new-function.png)

1. Seleccione esta plantilla en función de si ha creado una nueva aplicación de función donde seleccionó .NET como la pila en tiempo de ejecución, o si está usando una aplicación de función existente.

   * Para las aplicaciones de función nueva, seleccione esta plantilla: **Desencadenador de cola de Service Bus**

     ![Seleccione la plantilla para la nueva aplicación de función](./media/logic-apps-scenario-function-sb-trigger/current-add-queue-trigger-template.png)

   * Para una aplicación de función existente, seleccione esta plantilla: **Desencadenador de cola de Service Bus:C#**

     ![Seleccione la plantilla de aplicación de función existente](./media/logic-apps-scenario-function-sb-trigger/legacy-add-queue-trigger-template.png)

1. En el **desencadenador de cola de Azure Service Bus** panel, proporcione un nombre para el desencadenador y configurar el **conexión de Service Bus** para la cola, que usa el SDK de Azure Service Bus `OnMessageReceive()` agente de escucha y elija **Crear**.

1. Escribir una función básica para llamar al punto de conexión de la aplicación lógica creada anteriormente mediante el mensaje de cola como desencadenador. En el ejemplo se usa el tipo de contenido de mensaje `application/json`, pero puede cambiarlo si es necesario. Si es posible, volver a usar la instancia de los clientes HTTP. Para obtener más información, consulte [administrar conexiones en Azure Functions](../azure-functions/manage-connections.md).

   ```CSharp
   using System;
   using System.Threading.Tasks;
   using System.Net.Http;
   using System.Text;

   // Callback URL for previously created Request trigger
   private static string logicAppUri = @"https://prod-05.westus.logic.azure.com:443/workflows/<remaining-callback-URL>";

   // Reuse the instance of HTTP clients if possible
   private static HttpClient httpClient = new HttpClient();

   public static void Run(string myQueueItem, ILogger log)
   {
       log.LogInformation($"C# ServiceBus queue trigger function processed message: {myQueueItem}");

       var response = httpClient.PostAsync(logicAppUri, new StringContent(myQueueItem, Encoding.UTF8, "application/json")).Result;
   }
   ```

1. Para probar la función, agregue un mensaje de cola mediante una herramienta como el [Explorador de Service Bus](https://github.com/paolosalvatori/ServiceBusExplorer).

   La aplicación lógica se desencadena inmediatamente después de que la función recibe el mensaje.

## <a name="next-steps"></a>Pasos siguientes

[Llamar, desencadenar o anidar flujos de trabajo mediante el uso de puntos de conexión HTTP](../logic-apps/logic-apps-http-endpoint.md)