---
title: Uso de los temas y las suscripciones de Service Bus de Azure con Node.js | Microsoft Docs
description: Aprenda a usar los temas y las suscripciones de Service Bus en Azure desde una aplicación Node.js.
services: service-bus-messaging
documentationcenter: nodejs
author: axisc
manager: timlt
editor: spelluru
ms.assetid: b9f5db85-7b6c-4cc7-bd2c-bd3087c99875
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 04/15/2019
ms.author: aschhab
ms.openlocfilehash: 7686014adb989494e6df277de4137b76c3125696
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2019
ms.locfileid: "65992129"
---
# <a name="how-to-use-service-bus-topics-and-subscriptions-with-nodejs-and-the-azureservice-bus-package"></a>Uso de suscripciones y temas de Service Bus con Node.js y el paquete de azure o service bus
> [!div class="op_multi_selector" title1="Programming language" title2="Node.js pacakge"]
> - [(Node.js | azure-sb)](service-bus-nodejs-how-to-use-topics-subscriptions.md)
> - [(Node.js | @azure/service-bus)](service-bus-nodejs-how-to-use-topics-subscriptions-new-package.md)

En este tutorial, obtendrá información sobre cómo escribir un programa de Node.js para enviar mensajes a un tema de Service Bus y recibir mensajes de una suscripción de Service Bus con el nuevo [ @azure/service-bus ](https://www.npmjs.com/package/@azure/service-bus) paquete. Este paquete utiliza el más rápido [protocolo AMQP 1.0](service-bus-amqp-overview.md) , mientras que las antiguas [azure-sb](https://www.npmjs.com/package/azure-sb) paquete utilizado [las API de REST de Service Bus tiempo de ejecución](/rest/api/servicebus/service-bus-runtime-rest). Los ejemplos están escritos en JavaScript.

## <a name="prerequisites"></a>Requisitos previos
- Una suscripción de Azure. Para completar este tutorial, deberá tener una cuenta de Azure. Puede activar su [ventajas de suscriptor MSDN](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A85619ABF) o registrarse para obtener un [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A85619ABF).
- Si no tiene un tema y una suscripción para que funcione con, siga los pasos de la [uso de Azure portal para crear un Bus de servicio de temas y suscripciones](service-bus-quickstart-topics-subscriptions-portal.md) artículo para crearlos. Anote la cadena de conexión para la instancia de Service Bus y los nombres del tema y suscripción que creó. Vamos a usar estos valores en los ejemplos.

> [!NOTE]
> - Este tutorial funciona con ejemplos que puede copiar y ejecutar mediante [Nodejs](https://nodejs.org/). Para obtener instrucciones sobre cómo crear una aplicación de Node.js, consulte [crear e implementar una aplicación Node.js en un sitio Web de Azure](../app-service/app-service-web-get-started-nodejs.md), o [servicio en la nube de Node.js mediante Windows PowerShell](../cloud-services/cloud-services-nodejs-develop-deploy-app.md).
> - El nuevo [ @azure/service-bus ](https://www.npmjs.com/package/@azure/service-bus) paquete no admite aún la creación de temas y suscripciones. Use la [ @azure/arm-servicebus ](https://www.npmjs.com/package/@azure/arm-servicebus) si desea crearlos mediante programación del paquete.

### <a name="use-node-package-manager-npm-to-install-the-package"></a>Uso del Administrador de paquetes para Node (NPM) para instalar el paquete
Para instalar el paquete de npm para Service Bus, abra un símbolo del sistema que tiene `npm` en su ruta de acceso, cambie el directorio a la carpeta donde desea tener sus ejemplos y, a continuación, ejecute este comando.

```bash
npm install @azure/service-bus
```

## <a name="send-messages-to-a-topic"></a>Envío de mensajes a un tema
Interactuar con un Bus de servicio tema empieza por crear una instancia de la [ServiceBusClient](https://docs.microsoft.com/javascript/api/@azure/service-bus/servicebusclient) clase y usarlo para crear una instancia de la [TopicClient](https://docs.microsoft.com/javascript/api/%40azure/service-bus/topicclient) clase. Una vez que el cliente de tema, puede crear un remitente y usar [enviar](https://docs.microsoft.com/javascript/api/%40azure/service-bus/sender#send-sendablemessageinfo-) o [sendBatch](https://docs.microsoft.com/javascript/api/@azure/service-bus/sender#sendbatch-sendablemessageinfo---) método en él para enviar mensajes.

1. Abra el editor que prefiera, como [Visual Studio Code](https://code.visualstudio.com/)
2. Cree un archivo denominado `send.js` y pegue el siguiente código en él. Este código enviará 10 mensajes al tema.

    ```javascript
    const { ServiceBusClient } = require("@azure/service-bus"); 
    
    // Define connection string and related Service Bus entity names here
    const connectionString = "";
    const topicName = ""; 
    
    async function main(){
      const sbClient = ServiceBusClient.createFromConnectionString(connectionString); 
      const topicClient = sbClient.createTopicClient(topicName);
      const sender = topicClient.createSender();
      
        try {
            for (let i = 0; i < 10; i++) {
              const message= {
                body: `Hello world! ${i}`,
                label: `test`,
                userProperties: {
                    myCustomPropertyName: `my custom property value ${i}`
                }
              };
              console.log(`Sending message: ${message.body}`);
              await sender.send(message);
            }

            await topicClient.close();
          } finally {
            await sbClient.close();
          }
    }
    
    main().catch((err) => {
      console.log("Error occurred: ", err);
    });
    ```
3. Escriba la cadena de conexión y el nombre del tema en el código anterior.
4. A continuación, ejecute el comando `node send.js` en un símbolo del sistema para ejecutar este archivo. 

Felicidades. Que acaba de enviar mensajes a una cola de Service Bus.

Los mensajes tienen algunas propiedades estándares como `label` y `messageId` que se pueden establecer cuando se envía. Si desea establecer ninguna propiedad personalizada, utilice el `userProperties`, que es un objeto json que puede contener pares de clave y valor de los datos personalizados.

El tamaño máximo de mensaje que admiten los temas de Service Bus es de 256 KB en el [nivel Estándar](service-bus-premium-messaging.md) y de 1 MB en el [nivel Premium](service-bus-premium-messaging.md). No hay ningún límite en el número de mensajes contenidos en un tema, pero hay un límite en el tamaño total de los mensajes contenidos en un tema. El tamaño de los temas se define en el momento de la creación (el límite máximo es de 5 GB). Para obtener más información sobre las cuotas, vea [Cuotas de Service Bus](service-bus-quotas.md).

## <a name="receive-messages-from-a-subscription"></a>Recepción de mensajes de una suscripción
Interactuar con un Bus de servicio de suscripción empieza por crear una instancia de la [ServiceBusClient](https://docs.microsoft.com/javascript/api/@azure/service-bus/servicebusclient) clase y usarlo para crear una instancia de la [SubscriptionClient](https://docs.microsoft.com/javascript/api/%40azure/service-bus/subscriptionclient) clase. Una vez que el cliente de la suscripción, puede crear un receptor y usar [receiveMessages](https://docs.microsoft.com/javascript/api/%40azure/service-bus/receiver#receivemessages-number--undefined---number-) o [registerMessageHandler](https://docs.microsoft.com/javascript/api/%40azure/service-bus/receiver#registermessagehandler-onmessage--onerror--messagehandleroptions-) método en él para recibir mensajes.

1. Abra el editor que prefiera, como [Visual Studio Code](https://code.visualstudio.com/)
2. Cree un archivo denominado `recieve.js` y pegue el siguiente código en él. Este código intentará recibir 10 mensajes de la suscripción. El recuento real que recibirá depende del número de mensajes en la suscripción y la latencia de red.

    ```javascript
    const { ServiceBusClient, ReceiveMode } = require("@azure/service-bus"); 
    
    // Define connection string and related Service Bus entity names here
    const connectionString = "";
    const topicName = ""; 
    const subscriptionName = ""; 
    
    async function main(){
      const sbClient = ServiceBusClient.createFromConnectionString(connectionString); 
      const subscriptionClient = sbClient.createSubscriptionClient(topicName, subscriptionName);
      const receiver = subscriptionClient.createReceiver(ReceiveMode.ReceiveAndDelete);
      
      try {
        const messages = await receiver.receiveMessages(10);
        console.log("Received messages:");
        console.log(messages.map(message => message.body));
        
        await subscriptionClient.close();
      } finally {
        await sbClient.close();
      }
    }
    
    main().catch((err) => {
      console.log("Error occurred: ", err);
    });
    ```
3. Escriba la cadena de conexión y los nombres de su tema y suscripción en el código anterior.
4. A continuación, ejecute el comando `node receiveMessages.js` en un símbolo del sistema para ejecutar este archivo.

Felicidades. Acaba de recibir mensajes de una suscripción de Service Bus.

El [createReceiver](https://docs.microsoft.com/javascript/api/%40azure/service-bus/subscriptionclient#createreceiver-receivemode-) método toma un `ReceiveMode` que es una enumeración con valores [ReceiveAndDelete](message-transfers-locks-settlement.md#settling-receive-operations) y [PeekLock](message-transfers-locks-settlement.md#settling-receive-operations). No olvide [liquidar los mensajes](message-transfers-locks-settlement.md#settling-receive-operations) si usas el `PeekLock` modo mediante el uso de cualquiera de `complete()`, `abandon()`, `defer()`, o `deadletter()` métodos en el mensaje.

## <a name="subscription-filters-and-actions"></a>Las acciones y los filtros de suscripción
Service Bus admite [filtros y acciones en las suscripciones](topic-filters.md), lo que permite filtrar los mensajes entrantes a una suscripción y editar sus propiedades.

Una vez que tenga una instancia de un `SubscriptionClient` puede usar los siguientes métodos en él para obtener, agregar y quitar las reglas de la suscripción para controlar los filtros y acciones.

- getRules
- addRule
- removeRule

Cada suscripción tiene una regla predeterminada que usa el filtro true para permitir que todos los mensajes entrantes. Cuando se agrega una nueva regla, no olvide quitar el filtro predeterminado en orden del filtro en la nueva regla para que funcione. Si una suscripción no tiene ninguna regla, no recibirá ningún mensaje.

> [!NOTE]
> Puede administrar los recursos de Service Bus con [Explorador de Service Bus](https://github.com/paolosalvatori/ServiceBusExplorer/). El Explorador de Service Bus permite a los usuarios conectarse a un espacio de nombres de Service Bus y administrar las entidades de mensajería de una forma sencilla. La herramienta ofrece características avanzadas, como la funcionalidad de importación/exportación o la capacidad de probar el tema, colas, suscripciones, servicios de retransmisión, notification hubs y los centros de eventos. 

## <a name="next-steps"></a>Pasos siguientes
Para obtener más información, consulte los siguientes recursos.

- [Colas, temas y suscripciones](service-bus-queues-topics-subscriptions.md)
- Consulte otros [Nodejs ejemplos de Service Bus en GitHub](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/servicebus/service-bus/samples/javascript)
- [Centro para desarrolladores de Node.js](https://azure.microsoft.com/develop/nodejs/)


