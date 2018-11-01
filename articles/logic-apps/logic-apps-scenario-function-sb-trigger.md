---
title: 'Escenario: Desencadenado de aplicaciones lógicas con Azure Functions y Service Bus | Microsoft Docs'
description: Creación de funciones que desencadenan aplicaciones lógicas con Azure Functions y Azure Service Bus
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.reviewer: jehollan, klam, LADocs
ms.topic: article
ms.assetid: 19cbd921-7071-4221-ab86-b44d0fc0ecef
ms.date: 08/25/2018
ms.openlocfilehash: 69a4e4c59038599a7375466c46878bdd017582fa
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2018
ms.locfileid: "50231617"
---
# <a name="scenario-trigger-logic-apps-with-azure-functions-and-azure-service-bus"></a>Escenario: Desencadenamiento de aplicaciones lógicas con Azure Functions y Azure Service Bus

Puede utilizar Azure Functions para crear un desencadenador para una aplicación lógica cuando necesite implementar un agente de escucha o una tarea de ejecución prolongada. Por ejemplo, puede crear una función que escuche en una cola y que active inmediatamente una aplicación lógica como desencadenador de push.

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción de Azure. Si no tiene una suscripción de Azure, <a href="https://azure.microsoft.com/free/" target="_blank">regístrese para obtener una cuenta gratuita de Azure</a>. 

* Conocimientos básicos acerca de [cómo crear aplicaciones lógicas](../logic-apps/quickstart-create-first-logic-app-workflow.md) 

* Antes de poder crear una función de Azure, [cree una aplicación de función](../azure-functions/functions-create-function-app-portal.md).

## <a name="create-logic-app"></a>Creación de la aplicación lógica

En este ejemplo, se ejecuta una función para cada aplicación lógica que necesite desencadenarse. En primer lugar, cree una aplicación lógica que tenga un desencadenador de solicitud HTTP. La función llama a ese punto de conexión siempre que se reciba un mensaje en cola.  

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y cree la aplicación lógica en blanco. 

   Si es la primera vez que usa aplicaciones lógicas, revise la [guía de inicio rápido sobre cómo crear la primera aplicación lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md).

1. En el cuadro de búsqueda, escriba "solicitud http". En la lista de desencadenadores, seleccione el siguiente: **Cuando se recibe una solicitud HTTP**.

   ![Seleccionar un desencadenador](./media/logic-apps-scenario-function-sb-trigger/when-http-request-received-trigger.png)

1. En el desencadenador **Request**, puede escribir opcionalmente un esquema JSON para usarlo con el mensaje de cola. Los esquemas JSON ayudan al Diseñador de aplicaciones lógicas a comprender la estructura de los datos de entrada y facilitan la selección de salidas a lo largo del flujo de trabajo. 

   Para especificar un esquema, especifique el esquema en el cuadro **Esquema JSON de cuerpo de solicitud**, por ejemplo: 

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

1. Agregue todas las demás acciones que desea que ocurran después de recibir el mensaje de cola. 

   Por ejemplo, puede enviar un correo electrónico con el conector Office 365 Outlook.

1. Guarde la aplicación lógica, que genera la dirección URL de devolución de llamada para el desencadenador en esta aplicación lógica. Esta dirección URL aparece en la propiedad **HTTP POST URL**.

   ![Generación de la dirección URL de devolución de llamada para el desencadenador](./media/logic-apps-scenario-function-sb-trigger/callback-URL-for-trigger.png)

## <a name="create-azure-function"></a>Creación de una función de Azure

A continuación, cree la función que actúa como desencadenador y escuche la cola. 

1. En Azure Portal, abra y expanda la aplicación de función, si aún no está abierta. 

1. En el nombre de la aplicación de función, expanda **Functions**. En el panel **Functions**, elija **Nueva función**. Seleccione esta plantilla: **Service Bus Queue trigger - C#**
   
   ![Selección del portal de Azure Functions](./media/logic-apps-scenario-function-sb-trigger/newqueuetriggerfunction.png)

1. Proporcione un nombre para el desencadenador y, a continuación, configure la conexión a la cola de Service Bus, que usa el agente de escucha `OnMessageReceive()` del SDK de Azure Service Bus.

1. Escriba una función básica para llamar al punto de conexión de la aplicación lógica creado anteriormente usando el mensaje de cola como desencadenador. Por ejemplo: 
   
   ```CSharp
   using System;
   using System.Threading.Tasks;
   using System.Net.Http;
   using System.Text;
   
   private static string logicAppUri = @"https://prod-05.westus.logic.azure.com:443/.........";
   
   public static void Run(string myQueueItem, TraceWriter log)
   {
       log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");

       using (var client = new HttpClient())
       {
           var response = client.PostAsync(logicAppUri, new StringContent(myQueueItem, Encoding.UTF8, "application/json")).Result;
       }
   }
   ```

   En el ejemplo se usa el tipo de contenido de mensaje `application/json`, pero puede cambiarlo si es necesario.

1. Para probar la función, agregue un mensaje de cola mediante una herramienta como el [Explorador de Service Bus](https://github.com/paolosalvatori/ServiceBusExplorer). 

   La aplicación lógica se desencadena inmediatamente después de que la función recibe el mensaje.

## <a name="get-support"></a>Obtención de soporte técnico

* Si tiene alguna duda, visite el [foro de Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Para enviar ideas sobre características o votar sobre ellas, visite el [sitio de comentarios de los usuarios de Logic Apps](https://aka.ms/logicapps-wish).

